
# docker-node-oracle-pm2

![GitHub workflow](https://github.com/thesuhu/docker-node-oracle-pm2/actions/workflows/docker-image.yml/badge.svg) ![Docker pull](https://img.shields.io/docker/pulls/thesuhu/docker-node-oracle-pm2) [![license](https://img.shields.io/github/license/thesuhu/docker-node-oracle-pm2)](https://github.com/thesuhu/docker-node-oracle-pm2/blob/master/LICENSE)

Docker images containing Node.js, Oracle Instant Client (12.2) and PM2 — intended as a base image for Node apps that need Oracle connectivity.

## Summary

- Base image: Node.js + Oracle Instant Client + PM2
- Oracle Instant Client ZIP files are not distributed in the public image because of Oracle licensing; this repository includes the ZIPs in `oracle/linux/` for users who already have access.

## Available tags

| Tag | Node.js | PM2 | Oracle Client |
| --- | --- | --- | --- |
| latest | 22.0.0 | 5.4.2 | 12.2.0 |
| 22 | 22.0.0 | 5.4.2 | 12.2.0 |
| 16 | 16.20.2 | 5.3.0 | 12.2.0 |
| 14 | 14.18.1 | 5.1.2 | 12.2.0 |

If you need a different version, fork this repo and modify the `Dockerfile` to build your desired versions.

## Usage

1. In your application's Dockerfile, use this image as the base:

```Dockerfile
FROM thesuhu/docker-node-oracle-pm2:{TAG}
# replace {TAG} with one of the tags above, e.g. latest
```

2. Since Oracle Instant Client ZIPs are not publicly distributed, there are two ways to include them when building your image:

a) Use the ZIP files included locally in the `oracle/linux/` folder (if you keep them in the repository):

```sh
# From repo root
docker build -t my-node-oracle-app .
```

b) Or bind/mount the ZIP files during build (recommended for CI):

```sh
# Example using BuildKit and build-args to point to host paths for the instantclient zips
DOCKER_BUILDKIT=1 docker build --build-arg INSTANTCLIENT_BASIC=/host/path/instantclient-basic-linux.x64-12.2.0.1.0.zip \
	--build-arg INSTANTCLIENT_SDK=/host/path/instantclient-sdk-linux.x64-12.2.0.1.0.zip -t my-node-oracle-app .
```

3. Run the container as usual (simple example):

```sh
docker run -d --name app -p 3000:3000 my-node-oracle-app
```

## Example Dockerfile (optional)

If you want to build your own image on top of this base, a minimal example:

```Dockerfile
FROM thesuhu/docker-node-oracle-pm2:latest
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
CMD ["pm2-runtime", "start", "ecosystem.config.js"]
```

## The `oracle/` folder in this repository

This repo contains the following ZIP files under `oracle/linux/` (you must have rights to download/use these from Oracle):

- instantclient-basic-linux.x64-12.2.0.1.0.zip
- instantclient-sdk-linux.x64-12.2.0.1.0.zip

Place these files in `oracle/linux/` before doing a local build if you want them baked into the image.

## Quick troubleshooting

- Oracle connection fails: verify environment variables (e.g. ORACLE_HOST, ORACLE_PORT, ORACLE_SID/ORACLE_SERVICE_NAME, USER, PASSWORD).
- Native module (oracledb) errors during npm install: ensure Node version matches the image tag and Oracle Instant Client is present in the image.
- File permissions: when mounting ZIPs from the host, ensure Docker build has read access to those files.

## License

MIT — see the `LICENSE` file.
