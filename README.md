# 🚀 Traefik + Docker Compose Setup

This repository contains a ready-to-use **Traefik v3.5** reverse proxy setup with automatic **Let's Encrypt HTTPS certificates** and support for multiple web services running on the same VPS.  
The configuration is designed to make it easy to deploy Django, Nginx, or any other Dockerized web service behind a secure HTTPS reverse proxy.

---

## ✨ Features
- 🔒 Automatic HTTPS certificates from **Let's Encrypt**
- 🌐 Support for multiple domains & subdomains (e.g. `example.com`, `nginx.example.com`, `traefik.example.com`)
- 📊 Built-in **Traefik Dashboard** (protected with basic auth)
- 🔄 Automatic redirection from HTTP → HTTPS
- 🐳 Works with any Docker container (Django, Nginx, Node.js, etc.)
- 🔧 Easy to configure via `.env`

---

## 📂 Project Structure

.
├── docker-compose.yml   # Main Traefik reverse proxy configuration
├── .env.example         # Example environment variables
└── letsencrypt/         # Directory for Let’s Encrypt certificates

---

## ⚙️ Requirements
- A VPS or server with:
    - Docker
    - Docker Compose
- A domain name with DNS records pointing to your VPS (e.g. `A record` for `@`, `nginx.example.com`, `traefik.example.com`, etc.)

---

## 🚀 Deployment

1. **Clone this repository**
   ```
   git clone https://github.com/yourusername/traefik-docker-compose.git
   cd traefik-docker-compose
   ```
2. **Create .env file**
    ```
    nano .env
    ```
    and add this
    ```
   TRAEFIK_ACME_EMAIL=admin@raketaweb.eu
   TRAEFIK_BASIC_AUTH=sokov:$$apr1$$82THfKmp$$jvATilnWRxMys3DCrmp0r/
    ```
   You can generate a new password with:
   ```
   htpasswd -nbB youruser "yourpassword"
   ```
3. Start Traefik
   ```
   docker compose up -d
   ```
4. Create your docker network
   ```
   docker network create proxynet
   ```

🛠️ Adding New Services

To add a new container (for example, an Nginx site or Django app), just connect it to the same proxynet network and add Traefik labels.

    Example:
    ```
    services:
    nginx:
    image: nginx:latest
    container_name: nginx_test
    networks:
    - proxynet
    
        labels:
          - "traefik.enable=true"
          - "traefik.http.routers.nginx.rule=Host(`nginx.example.com`)"
    
    networks:
      proxynet:
        external: true
    ```

📊 Traefik Dashboard

Once Traefik is up, you can access the dashboard at https://traefik.example.com

✅ Summary

This setup allows you to:

•	Run multiple web apps on one VPS 

•	Automatically secure them with Let’s Encrypt

•	Manage everything through Traefik Dashboard

Perfect for hosting Django, FastAPI, Node.js, or even static Nginx sites with HTTPS out-of-the-box 🚀
