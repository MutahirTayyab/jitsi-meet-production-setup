# Jitsi Meet + Jibri Deployment on AWS EC2 using Docker

This project documents a Docker-based deployment of Jitsi Meet on AWS EC2 with a separate Jibri recording server. The setup includes HTTPS, DNS, security group rules, Jibri recording configuration, and troubleshooting notes.

## Architecture

```text
Users / Browsers
      |
      v
Jitsi EC2: web + prosody + jicofo + jvb
      |
      | TCP 5222 XMPP
      v
Jibri EC2: jibri recording containers
```

## Main Features

- Jitsi Meet deployed with Docker Compose
- Separate Jibri EC2 instance for recording
- HTTPS configured with mounted SSL certificate files
- Jibri connected to Jitsi through XMPP port 5222
- Recording service enabled
- Notes for scaling Jibri containers

## Repository Structure

```text
.
├── jitsi/
│   ├── docker-compose.yml
│   └── .env.example
├── jibri/
│   ├── jibri.yml
│   └── .env.example
├── docs/
│   ├── deployment-guide.md
│   └── security-notes.md
└── screenshots-redacted/
```

## Important Security Notice

Real `.env` files, AWS PEM keys, SSL private keys, public IPs, instance IDs, and real passwords are not included. Use the `.env.example` files and generate your own secrets.

## Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/jitsi-jibri-aws-deployment.git
cd jitsi-jibri-aws-deployment
```

### 2. Prepare Jitsi EC2

```bash
cd jitsi
cp .env.example .env
nano .env
```

Replace placeholders such as:

```env
PUBLIC_URL=https://your-domain.example.com
JVB_ADVERTISE_IPS=YOUR_JITSI_PUBLIC_IP
DOCKER_HOST_ADDRESS=YOUR_JITSI_PUBLIC_IP
```

Start Jitsi:

```bash
docker compose -f docker-compose.yml up -d
```

### 3. Prepare Jibri EC2

```bash
cd jibri
cp .env.example .env
nano .env
```

Make sure the Jibri passwords match the Jitsi `.env` values.

Start Jibri:

```bash
docker compose -f jibri.yml up -d
```

### 4. Verify

```bash
docker ps
nc -zv your-domain.example.com 443
nc -zv your-domain.example.com 5222
```

## Notes

One Jibri can handle one active recording. For four simultaneous recordings, four Jibri workers are required. On AWS EC2, running multiple Jibri containers on one host may have audio limitations because ALSA loopback may not be available on the AWS kernel.
