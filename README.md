
# Proxmox Media Stack with Docker Compose

A comprehensive guide to set up a media stack on Proxmox using Docker Compose.

## Table of Contents

- [Introduction](#introduction)
- [Requirements](#requirements)
- [Proxmox Initial Configuration](#proxmox-initial-configuration)
- [Docker and Docker Compose](#docker-and-docker-compose)
- [Media Stack Configuration](#media-stack-configuration)
- [Nginx Reverse Proxy](#nginx-reverse-proxy)
- [Security Considerations](#security-considerations)
- [Troubleshooting](#troubleshooting)

## Introduction

This guide assists in setting up a media stack on Proxmox using Docker Compose, including services like Jellyfin, Radarr, Sonarr, qBittorrent, and more.

## Requirements

- A Proxmox server.
- Docker version 20.x and above.
- Docker Compose version 1.25.x and above.
- Basic knowledge of Linux, Docker, and Docker Compose.

## Proxmox Initial Configuration

1. **Setting up the Host Machine**:
   - Update your Proxmox.
   - Allocate sufficient resources (CPU, RAM, and Storage).

2. **LXC Container**:
   - Create an LXC container using a standard template like Ubuntu.
   - Make sure to assign a static IP for easy access.
   - Set up mount points if using additional storage drives.

## Docker and Docker Compose

1. **Docker Installation**:
   - Install Docker.
   - Ensure the Docker service is active.

2. **Docker Compose Installation**:
   - Check if installed or Install Docker Compose via `pip` or the official binary.

3. **Network Configuration**:
   - Create a Docker network: `docker network create media-network`.

## Media Stack Configuration

1. **VPN Setup (Optional)**:
   - Set up the VPN using the `qmcgaw/gluetun` Docker image.
   - Update the environment variables with your VPN credentials.

2. **Service Configuration**:
   - Configure services like Jellyfin, Radarr, etc.
   - If using NZBGet for Usenet, ensure it's integrated properly.
   - Ensure all services are part of the `media-network`.

3. **Deploying the Stack**:
   - If using a VPN, modify the `docker-compose.yml` as per the instructions in the file, then deploy with:
   
     ```
     VPN_SERVICE_PROVIDER=your_vpn OPENVPN_USER=username OPENVPN_PASSWORD=password SERVER_COUNTRIES=country docker compose --profile vpn up -d
     ```

   - Without VPN:

     ```
     docker compose up -d
     ```

## Nginx Reverse Proxy

1. **Entering the Nginx Container**:
   - Execute: `docker exec -it nginx bash`.

2. **Configuration**:
   - docker cp /path/on/host/nginx.conf nginx:/etc/nginx/nginx.conf
  OR
   - Navigate to `/etc/nginx/conf.d`.
   - Update the default configuration or add a new configuration file.
   - Ensure you set up proxies for all tools: Radarr, Sonarr, qBittorrent, Jellyfin, etc.
   - Reload the Nginx configuration with: `docker exec -it nginx nginx -s reload`.

## Security Considerations

1. **Limit Access**:
   - Only allow internal network IPs to access the services.
   - Use the `allow` and `deny` directives in Nginx to limit access.

2. **HTTPS (Optional)**:
   - Consider using SSL for encrypted communication, especially if accessing services outside your home network.

## Troubleshooting

1. **Docker Logs**:
   - Check logs using: `docker logs <container_name>`.

2. **Nginx Logs**:
   - Located in `/var/log/nginx/`.

3. **Network Issues**:
   - Ensure services are reachable and there are no port conflicts.

---

This README provides a structured and detailed guide based on our conversation. Remember, while this guide is comprehensive, you might need to adjust it based on specific use cases or changes in software. Always refer to official documentation for any third-party software or services.
