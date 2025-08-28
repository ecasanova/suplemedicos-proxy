
# Suplemedicos Proxy

Este repositorio contiene un pequeño [Caddy](https://caddyserver.com) reverse proxy
que reenvía solicitudes a `suplemedicos.com.co`. La configuración se gestiona con
Docker Compose y está pensada para ser fácil de desplegar en cualquier servidor.

> ⚠️ **IMPORTANTE**: Tu servidor debe tener configurado tanto **IPv4** como **IPv6** para que el proxy funcione correctamente. Verifica que tu proveedor de hosting soporte ambos protocolos antes de continuar.


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

> Nota: El archivo `Caddyfile` está configurado por defecto para apuntar a la IP `45.152.46.144` como destino del proxy.

   ```caddy
   # api.example.com -> reverse proxy a suplemedicos.com.co por **IPv4**
   api.example.com {
       ...
   }
   ```

   Sustituye `api.example.com` por tu propio dominio.

2. Configura tu email para los certificados Let's Encrypt:

   ```bash
   # Edita el archivo .env con tu email
   nano .env
   ```

   Cambia `admin@ejemplo.com` por tu email real en el archivo `.env`.

### Iniciar el proxy

```bash
docker compose up -d
```

## Configuración de DNS

En el proveedor de DNS de tu dominio:

- Crea un registro **A** que apunte `api.example.com` a la dirección IPv4 de tu
  servidor.
- Crea un registro **AAAA** para IPv6.

Asegúrate de que los puertos 80 y 443 estén abiertos en tu firewall.


## Probar que el proxy funciona

Puedes verificar que el proxy está funcionando correctamente utilizando `curl` desde cualquier máquina con acceso a internet. Por ejemplo:

### Probar con IPv4


```bash
curl -4 -I https://api.example.com
```

### Probar con IPv6


```bash
curl -6 -I https://api.example.com
```

Si el proxy está funcionando, deberías recibir una respuesta con encabezados HTTP, incluyendo un código de estado 200, 301, 302, etc.

## Detener el servicio

```bash
docker compose down
```

