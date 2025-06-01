# GENSYN NODE VPS SETUP
Easy, step-by-step guide and scripts to set up and run a Gensyn AI node on any VPS. Supports node installation, dependency setup, and frontend access.

# GENSYN Node Setup Guide

This guide is for new and existing users to install and run the Gensyn node on your VPS.

---

## Step 1: Install Dependencies

Run the following commands to update your system and install all required packages:

```bash
sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt update && sudo apt install -y nodejs
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update && sudo apt install -y yarn
```

---

## Step 2: Verify Installed Versions

Check that everything installed correctly:

```bash
node -v
npm -v
yarn -v
python3 --version
```

---

## Step 3: Clone the Gensyn Repository

```bash
git clone https://github.com/gensyn-ai/rl-swarm.git
cd rl-swarm
```

---

## Step 4: Start a Screen Session

Start a detached screen session to keep your node running:

```bash
screen -S gensyn
```

For old users: Copy your .pem file to the VPS now before continuing.

---

## Step 5: Set Up Python Virtual Environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

---

## Step 6: Install Frontend Dependencies

```bash
cd modal-login
yarn install
yarn upgrade
yarn add next@latest
yarn add viem@latest
cd ..
```

---

## Step 7: Checkout Specific Version

```bash
git reset --hard
git pull origin main
git checkout tags/v0.4.3
```

---

## Step 8: Run the Gensyn Node

```bash
./run_rl_swarm.sh
```

When prompted, press Y  
Then press A  
Then choose 7 or 32 (based on your VPS RAM)

For 16GB RAM, choose 7  
For 32GB RAM, choose 32

---

## Step 9: Expose the Frontend (Login Access)

Open a new SSH terminal on the same VPS and run one of the following methods to expose the frontend:

### Method 1 (Try first):

```bash
ssh -R 80:localhost:3000 serveo.net
```

### Method 2 (If Method 1 fails):

```bash
curl -fsSL https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64 -o cloudflared
chmod +x cloudflared
sudo mv cloudflared /usr/local/bin
cloudflared tunnel --url http://localhost:3000
```

### Method 3 (If both above fail, this will definitely work):

```bash
npx localtunnel --port 3000
```

Type yes if prompted

Login with your email as requested

---

## Additional Notes

After steps 1–2, your node will ask for Weights & Biases (w&b) login.

Choose option 3 if you don’t want results logged.

If you choose option 1 (to log results), create a w&b account using the same email, generate an API key, and enter it when prompted.

Enjoy your running Gensyn node! 🚀

