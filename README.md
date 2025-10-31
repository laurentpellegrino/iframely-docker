# Iframely Docker Images

Automated builds publish production-ready images of [Iframely](https://github.com/itteco/iframely) at [`laurentpellegrino/iframely`](https://hub.docker.com/r/laurentpellegrino/iframely). Each build is tagged with the short commit SHA from upstream and with `latest`.

## Getting Started

- Pull the most recent image:  
  ```bash
  docker pull laurentpellegrino/iframely:latest
  ```
- To pin a specific upstream revision, substitute `latest` with the desired short SHA tag.

## Configure Iframely

1. Copy `config.local.js.SAMPLE` from the upstream repository and tailor it to your needs.
2. Mount the config into the container at `/iframely/config.local.js` (read-only is recommended).

Example run command:

```bash
docker run -d \
  --name iframely \
  -p 8061:8061 \
  -e NODE_ENV=production \
  -v "$(pwd)/config.local.js:/iframely/config.local.js:ro" \
  laurentpellegrino/iframely:latest
```

- Adjust the host port if needed.
- Add any required provider API keys or cache configuration inside `config.local.js`.

## Docker Compose

```yaml
services:
  iframely:
    image: laurentpellegrino/iframely:latest
    ports:
      - "8061:8061"
    environment:
      NODE_ENV: production
    volumes:
      - ./config.local.js:/iframely/config.local.js:ro
```

Bring it up with `docker compose up -d`. Use the SHA tag in `image:` when you want deterministic upgrades.

## Maintenance Notes

- New upstream commits trigger fresh image builds automatically.
- Existing tags remain published; retag to the desired SHA if you need to roll back.
