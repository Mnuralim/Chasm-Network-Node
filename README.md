
# Chasm Network Node Incentive

Details: https://network-docs.chasm.net/chasm-scout-season-0/chasm-inference-scout-setup-guide
Source: https://t.me/airdropfind/91035
Tutorial Video: www.youtube.com

Minimum Requirements (use VPS):
- 1 vCPU
- 1GB RAM
- 20GB SSD

Suggested Requirements (use VPS):
- 2 vCPU
- 4GB RAM
- 50GB SSD

Step 1: Prerequisites
1. Go to: https://console.groq.com/keys
2. Save the API Key

Step 2: Obtaining Your SCOUT_UID and WEBHOOK_API_KEY
1. Go to https://scout.chasm.net/private-mint
2. Click _mint(scout), need 0.025 $MNT Gas fee
3. Log in to the website and retrieve your webhook API key and UID.

Step 3: Get Auth Token from NGROK
1. Register https://dashboard.ngrok.com/signup
2. Go to "Your Authtoken"
3. Click on the "Show Authtoken" button
4. Copy & save the token

Step 4: Install Docker
Set up Docker's apt repository
```
sudo apt update && sudo apt upgrade -y
```
```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
```
sudo apt-get update
```
```
sudo apt-get install ca-certificates curl
```
```
sudo install -m 0755 -d /etc/apt/keyrings
```
```
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```
```
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt-get update
```

Install the Docker packages
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Step 5: Install Screen
```
sudo apt install screen -y
```

Step 6: Install NGROK
```
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null && echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list && sudo apt update && sudo apt install ngrok
```

Step 7: Paste Your AuthToken (from Step 3)
```
ngrok config add-authtoken YOUR_AUTH_TOKEN
```
Replace YOUR_AUTH_TOKEN with your auth token from Step 3.

Step 8: Run NGROK in Screen
```
screen ngrok http 3001
```
Go to the forwarding section and copy the first URL. Save it (It will look like https://1bXX.XX.XXX.XXX.ngrok-free.app).

Detach from the session:
Press `CTRL + A + D`

Step 9: Set up the Environment File
```
nano .env
```
Paste the following format into the env file:
```
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

Step 10: Final Steps
Allow access through the firewall:
```
ufw allow 3001
```
Pull the Docker image:
```
docker pull chasmtech/chasm-scout:latest
```
Run the Docker container:
```
docker run -d --restart=always --env-file ./.env -p 3001:3001 --name scout chasmtech/chasm-scout
```
Check the Docker logs:
```
docker logs scout
```
Check the service locally:
```
curl localhost:3001
```
Send a test request:
```
source ./.env
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer $WEBHOOK_API_KEY" -d '{"body":"{"model":"gemma2-9b-it","messages":[{"role":"system","content":"You are a helpful assistant."}]}"}' $WEBHOOK_URL
```
Check Node Status: https://scout.chasm.net/dashboard (Check after 2 hours)

**DONE**

By:
GitHub: https://github.com/Mnuralim
Twitter: @Izzycracker04
Telegram: @fitriay19
YouTube: https://www.youtube.com/@IzzyTSN
