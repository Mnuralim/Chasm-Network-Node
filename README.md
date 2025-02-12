
# Chasm Network Node Incentive

For detailed information, visit: [Chasm Inference Scout Setup Guide](https://network-docs.chasm.net/chasm-scout-season-0/chasm-inference-scout-setup-guide) 

Airdrop Source: [Airdrop Finder](https://t.me/airdropfind/91035)  
Tutorial Video: [YouTube](www.youtube.com)

## Minimum Requirements (VPS)
- 1 vCPU
- 1GB RAM
- 20GB SSD

## Suggested Requirements (VPS)
- 2 vCPU
- 4GB RAM
- 50GB SSD

## Step 1: Prerequisites
1. Visit [Groq Console](https://console.groq.com/keys) and save your API Key.

## Step 2: Obtain Your SCOUT_UID and WEBHOOK_API_KEY
1. Go to [Chasm Scout Private Mint](https://scout.chasm.net/private-mint).
2. Click on "_mint(scout)" (requires 0.025 $MNT gas fee).
3. Log in and retrieve your webhook API key and UID.

## Step 3: Get Auth Token from NGROK
1. Register at [NGROK](https://dashboard.ngrok.com/signup).
2. Go to "Your Authtoken".
3. Click "Show Authtoken" and copy & save the token.

## Step 4: Install Docker

### Set up Docker's apt repository
```bash
sudo apt update && sudo apt upgrade -y
```
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
```bash
sudo apt-get update
```
```bash
sudo apt-get install ca-certificates curl
```
```bash
sudo install -m 0755 -d /etc/apt/keyrings
```
```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```
```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```bash
sudo apt-get update
```
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Step 5: Install Screen
```bash
sudo apt install screen -y
```

## Step 6: Install NGROK
```bash
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null && echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list && sudo apt update && sudo apt install ngrok
```

## Step 7: Paste Your AuthToken (from Step 3)
```bash
ngrok config add-authtoken YOUR_AUTH_TOKEN
```
Replace YOUR_AUTH_TOKEN with your auth token from Step 3.

## Step 8: Run NGROK in Screen
```bash
screen ngrok http 3001
```
Go to the forwarding section and copy the first URL. Save it (It will look like https://1bXX.XX.XXX.XXX.ngrok-free.app).

Detach from the session:
Press `CTRL + A + D`

## Step 9: Set up the Environment File
```bash
nano .env
```
Paste the following format into the env file:
```bash
ORCHESTRATOR_URL=https://orchestrator.chasm.net
SCOUT_NAME=any_name
SCOUT_UID=uid_from_step2
WEBHOOK_API_KEY=webhook_apiKey_from_step2
WEBHOOK_URL=https://1bXX.XX.XXX.XXX.ngrok-free.app
PROVIDERS=groq
MODEL=gemma2-9b-it
GROQ_API_KEY=groq_apiKey_from_step1
```
Save the file using `Ctrl + X`, then `Y`, and then press `Enter`.

## Step 10: Final Steps
Allow access through the firewall:
```bash
ufw allow 3001
```
Pull the Docker image:
```bash
docker pull chasmtech/chasm-scout:latest
```
Run the Docker container:
```bash
docker run -d --restart=always --env-file ./.env -p 3001:3001 --name scout chasmtech/chasm-scout
```
Check the Docker logs:
```bash
docker logs scout
```
Check the service locally:
```bash
curl localhost:3001
```
Send a test request:
```bash
source ./.env
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer $WEBHOOK_API_KEY" -d '{"body":"{"model":"gemma2-9b-it","messages":[{"role":"system","content":"You are a helpful assistant."}]}"}' $WEBHOOK_URL
```

Check Node Status: https://scout.chasm.net/dashboard (Check after 2 hours)

**DONE**



By:
GitHub: https://github.com/Mnuralim

YouTube: https://www.youtube.com/@IzzyTSN

Twitter: `@Izzycracker04`

Telegram: `@fitriay19`

