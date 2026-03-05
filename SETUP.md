
# Initial EC2 Setup
# System packages
sudo apt update && sudo apt upgrade -y
sudo apt install -y git cmake ninja-build clang-format \
    python3-pip python3-venv docker.io curl build-essential \
    libssl-dev pkg-config

# Add yourself to docker group (so you don't need sudo)
sudo usermod -aG docker $USER
newgrp docker

# Python tools
python3 -m venv ~/venv
source ~/venv/bin/activate
pip install ruff pre-commit twine cibuildwheel
echo 'source ~/venv/bin/activate' >> ~/.bashrc #make virtual env persistent

# Verify
git --version
cmake --version
docker --version
python3 --version

# Environment Setup

## EC2 Instance
- Region: ap-southeast-1
- Instance type: c7i.flex.large
- Instance ID: i-039c315e773bdb280
- Public IP: 18.136.194.180
- AMI: Ubuntu 22.04 LTS

## SSH Config (Mac)
Host moodist-dev
    HostName 18.136.194.180
    User ubuntu
    IdentityFile ~/.ssh/moodist-dev-singapore.pem
    ServerAliveInterval 60

## Installed on EC2 (date: 2026-03-04)
- [ ] apt: git, cmake, ninja-build, clang-format, docker.io...
- [ ] pip: ruff, pre-commit, twine
- [ ] docker group configured
- [ ] git identity set

## Key files
- .pem location: ~/.ssh/moodist-dev-singapore.pem
- Fork: github.com/ntl2105/moodist

Updating ```source ~/.zshrc``` with prompts in zshrc.txt

Grabbing my new IP address 
MY_IP=$(curl -s https://checkip.amazonaws.com)
echo "Your IP: $MY_IP"
Updating security rules with new IP address on EC2 instance

# Generate SSH key on EC2
ssh-keygen -t ed25519 -C "your@github-email.com"
# press Enter 3 times — accept defaults, no passphrase

# Print the public key
cat ~/.ssh/id_ed25519.pub
# copy the entire output

ssh -T git@github.com
# Should say: Hi YOUR_USERNAME! You've successfully authenticated...