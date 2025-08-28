# Suplemedicos Proxy

This repository contains a small [Caddy](https://caddyserver.com) reverse proxy
that forwards requests to `suplemedicos.com.co`. The setup is managed with
Docker Compose and is intended to be easy to deploy on any server.

## Requisitos

- Docker
- Docker Compose (plugin incluido en versiones recientes de Docker)

### Instalar Docker

```bash
sudo apt-get update
sudo apt-get install -y docker.io docker-compose-plugin
sudo systemctl enable --now docker
```

### Configuración

1. Ajusta el dominio en el `Caddyfile`. Por ejemplo:

   ```caddy
   # api.example.com -> reverse proxy a suplemedicos.com.co por **IPv4**
   api.example.com {
       ...
   }
   ```

   Sustituye `api.example.com` por tu propio dominio.

2. Opcionalmente define la variable de entorno `ACME_EMAIL` con el correo que
   usarás para el certificado TLS emitido por Let's Encrypt.

### Iniciar el proxy

```bash
docker compose up -d
```

## Configuración de DNS

En el proveedor de DNS de tu dominio:

- Crea un registro **A** que apunte `api.example.com` a la dirección IPv4 de tu
  servidor.
- (Opcional) Crea un registro **AAAA** para IPv6.

Asegúrate de que los puertos 80 y 443 estén abiertos en tu firewall.

## Detener el servicio

```bash
docker compose down
```

