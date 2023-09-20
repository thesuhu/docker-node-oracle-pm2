# docker-node-oracle-pm2

![GitHub workflow](https://github.com/thesuhu/docker-node-oracle-pm2/actions/workflows/docker-image.yml/badge.svg) ![Docker pull](https://img.shields.io/docker/pulls/thesuhu/docker-node-oracle-pm2) [![license](https://img.shields.io/github/license/thesuhu/docker-node-oracle-pm2)](https://github.com/thesuhu/docker-node-oracle-pm2/blob/master/LICENSE)

Docker images used to create containers ready with Node.js, Oracle Client and PM2.

## Usage

Within your Dockerfile:

```sh
FROM thesuhu/docker-node-oracle-pm2:{VERSION}
```

Specify the Node.js version you will use in the above *{VERSION}*.

## Release

The following version are available:

| Tag | Node.js | PM2 | Oracle Client |
| --- | --- | --- | --- |
| Latest | 16.20.2 | 5.3.0 | 12.2.0 |
| 16 | 16.20.2 | 5.3.0 | 12.2.0 |
| 14 | 14.18.1 | 5.1.2 | 12.2.0 |

If you need another version, you can fork and edit the `Dockerfile` and then build for your own.

## License

[MIT](https://github.com/thesuhu/docker-node-oracle-pm2/blob/master/LICENSE)
