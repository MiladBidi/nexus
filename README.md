# Nexus
This is a practical guide to configuring Nexus behind a reverse proxy. Here our reverse proxy is Traefik.

Important: There is no need to map any port inside Nexus compose .

Setup Docker Hosted Repo:
1. Create repository
2. HTTP Port 5000

---

Change Docker Config File:

```bash
{
  "insecure-registries": ["https://docker.rahbia.iamsre.ir"]
}
```

Change Labels for traefik in nexus compose: 

```bash
labels:
      - "traefik.enable=true"
      - "traefik.http.routers.http-repo.rule=Host(`repo.rahbia.iamsre.ir`)"
      - "traefik.http.routers.http-repo.entrypoints=web"
      - "traefik.http.routers.repo.rule=Host(`repo.rahbia.iamsre.ir`)"
      - "traefik.http.routers.repo.entrypoints=web-secure"
      - "traefik.http.routers.repo.tls=true"
      - "traefik.http.routers.repo.tls.certresolver=myresolver"
      - "traefik.http.routers.repo.service=repo"
      - "traefik.http.services.repo.loadBalancer.server.port=8081"
      - "traefik.http.routers.http-docker.rule=Host(`docker.rahbia.iamsre.ir`)"
      - "traefik.http.routers.http-docker.entrypoints=web"
      - "traefik.http.routers.docker.rule=Host(`docker.rahbia.iamsre.ir`)"
      - "traefik.http.routers.docker.entrypoints=web-secure"
      - "traefik.http.routers.docker.tls=true"
      - "traefik.http.routers.docker.tls.certresolver=myresolver"
      - "traefik.http.routers.docker.service=docker-srv"
      - "traefik.http.services.docker-srv.loadbalancer.server.port=5000"

```
