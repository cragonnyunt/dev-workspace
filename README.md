# dev-workspace

Docker based local development environment for the StudyHub projects.

## Services

- `nodejs` - Node 24 + npm/yarn/bun, mounts this machine's `Development` folder at `/var/www`.
- `mongo` - MongoDB on `localhost:27017` (user `root`, password `root`).
- `seaweedfs` - SeaweedFS 4.52 (master + volume + filer + S3 gateway) for file storage;
  data is kept in the `seaweedfs_data` volume. S3 API on `localhost:8333`, filer UI on
  `localhost:8888`.
- `chrome` - Minimal headless Chrome (`chromedp/headless-shell`) with Chrome DevTools
  Protocol on `localhost:9222`, for AI agents and UI inspection.

## Quick start

```bash
docker compose up -d <services> # services like nodejs, mongo, seaweedfs, chrome
docker compose down
```

| App        | URL                     |
| ---------- | ----------------------- |
| Frontend   | http://localhost:5174   |
| API        | http://localhost:3001   |
| S3 API     | http://localhost:8333   |
| Filer UI   | http://localhost:8888   |
| Chrome CDP | http://localhost:9222   |

## Local S3

SeaweedFS provides an S3-compatible endpoint at `http://localhost:8333`.

```env
AWS_ACCESS_KEY_ID=studyhub
AWS_SECRET_ACCESS_KEY=studyhub-secret
AWS_REGION=us-east-1
```

Use `http://seaweedfs:8333` from containers on the `workspace-network` and
`http://localhost:8333` from the host. The bucket is created automatically on the first
admin upload, or you can create it with any S3 client.

## Headless Chrome

The `chrome` service exposes Chrome DevTools Protocol at:

```text
http://localhost:9222
```

It shares the `nodejs` container's network namespace, so a browser session connected
through CDP can open `http://localhost:5174` and call `http://localhost:3001` exactly
like a normal local browser.

Check it:

```bash
curl http://localhost:9222/json/version
```

Playwright can connect to it:

```js
const { chromium } = require('playwright');
const browser = await chromium.connectOverCDP('http://localhost:9222');
```
