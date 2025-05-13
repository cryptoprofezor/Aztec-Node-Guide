# 👨🏻‍💻 Aztec Sequencer Node Setup (By cryptoprofezor)

This guide will take you from 0 to running a fully working **Aztec Sequencer Node**, step-by-step, with no confusion and with fully working tested commands. Screenshots, custom tips, and no copy-paste bloat — just exactly what you need to earn that operator role 🔥

---

## 🖥️ System Requirements (Recommended for Sequencer)

| Type         | Required Specs         |
|--------------|------------------------|
| CPU          | 8-core                 |
| RAM          | 16 GB                  |
| SSD          | 1 TB                   |
| Bandwidth    | 25 Mbps up/down        |
| OS           | Ubuntu 22.04 LTS       |
| Access       | Root/Sudo access       |

---

## 🔧 Pre-Requirements

- ✅ Docker + Docker Compose *(run in background)*
- ✅ Aztec CLI Tool
- ✅ Sepolia RPC URL (from Metamask/Alchemy)
- ✅ EVM Wallet (Public + Private Key)

---

## 🛠 Dependency Installation

### 🔁 Update packages
```bash
sudo apt-get update && sudo apt-get upgrade -y
```

### 🟢 Install Node.js
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt update && sudo apt install -y nodejs
```

### 🔨 Other Packages
```bash
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano \
automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev \
tar clang bsdmainutils ncdu unzip screen ufw -y
```

### 🐳 Install Docker + Docker Compose
Use our fixed manual installer (even works on Ubuntu 24.04):
```bash
curl -O https://raw.githubusercontent.com/cryptoprofezor/docker/main/install-docker.sh
chmod +x install-docker.sh
./install-docker.sh
```

---

## ⚙️ Install Aztec CLI
```bash
bash -i <(curl -s https://install.aztec.network)
```

### 🧠 Add CLI to path
```bash
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### 🔍 Confirm installation
```bash
aztec -h
```

---

## 🧪 Set Up the Testnet
```bash
aztec-up alpha-testnet
```

---

## 💸 Load Wallet with Sepolia ETH
Use any of the following faucets:
- https://sepolia-faucet.pk910.de/
- https://www.alchemy.com/faucets/ethereum-sepolia

---

## 🔓 Open Required Ports
```bash
sudo ufw allow 22
sudo ufw allow ssh
sudo ufw enable
sudo ufw allow 40400
sudo ufw allow 8080
```

---

## 🧬 Start Your Sequencer Node
```bash
screen -S aztec
```

```bash
aztec start --node --archiver --sequencer \
  --network alpha-testnet \
  --l1-rpc-urls https://YOUR_SEPOLIA_RPC \
  --l1-consensus-host-urls https://YOUR_BEACON_RPC \
  --sequencer.validatorPrivateKey 0xYOUR_PRIVATE_KEY \
  --sequencer.coinbase YOUR_PUBLIC_ADDRESS \
  --p2p.p2pIp $(curl -s ifconfig.me)
```

Replace:
- ✅ `YOUR_SEPOLIA_RPC` with Metamask/Alchemy RPC
- ✅ `YOUR_BEACON_RPC` with Chainstack or DRPC beacon
- ✅ Wallet keys and IP

![Screenshot 2025-05-13 172832](https://github.com/user-attachments/assets/8dc4d0a7-2b36-4089-a1b3-9c388dbd6c38)

---

## 💾 Save These Details Somewhere
```
• Ethereum sepolia rpc :
• Beacon sepolia RPC   :
• Private Key (0x...)  :
• Wallet Address       :
• VPS IP               :
• Proven Block Number  :
• Base64 Proof String  :
```

---

## 🎯 Discord Operator Role

### Step 1: Get Proven Block
```bash
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getL2Tips","params":[],"id":67}' \
http://localhost:8080 | jq -r ".result.proven.number"
```

### Step 2: Generate Proof
```bash
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getArchiveSiblingPath","params":["BLOCK","BLOCK"],"id":67}' \
http://localhost:8080 | jq -r ".result"
```

📸 *Add screenshot of proof output*

### Step 3: Register on Discord
- Join: https://discord.gg/aztec
- Channel: `#operators│start-here`
- Type: `/operator start`
- Fill in: Wallet, Block, Proof

📸 *Add screenshot of successful role assignment*

---

## 🔁 Screen Tips
- Detach session: `Ctrl + A`, then `D`
- Reattach session: `screen -r aztec`

---

## 🛡 Register as Validator (Optional)
```bash
aztec add-l1-validator \
  --l1-rpc-urls https://YOUR_SEPOLIA_RPC \
  --private-key 0xYOUR_PRIVATE_KEY \
  --attester YOUR_PUBLIC_ADDRESS \
  --proposer-eoa YOUR_PUBLIC_ADDRESS \
  --staking-asset-handler 0xF739D03e98e23A7B65940848aBA8921fF3bAc4b2 \
  --l1-chain-id 11155111
```

---

## 📬 Community & Help

- 📣 Telegram: [t.me/cryptogg](https://t.me/Mrcrypto_tamilan)
- 🐛 Raise a GitHub issue if you need help

---

Made with ❤️ by [@cryptoprofezor](https://github.com/cryptoprofezor)  
Happy Coding. Happy Syncing 💻
