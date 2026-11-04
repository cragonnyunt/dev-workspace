# dev-workspace

Docker based local development environment for the StudyHub projects.

## Services

- `nodejs` — Node 24 + npm/yarn/bun, mounts this machine's `Development` folder at `/var/www`.
- `mongo` — MongoDB on `localhost:27017` (user `root`, password `root`).
- `seaweedfs` — SeaweedFS 4.52 (master + volume + filer + S3 gateway) for file storage;
  data is kept in the `seaweedfs_data` volume. S3 API on `localhost:8333`, filer UI on
  `localhost:8888`.

## Quick start

```bash
docker compose up -d <services> # services like nodejs, mongo, SeaweedFS
docker compose down
```

| App      | URL                     |
| -------- | ----------------------- |
| Frontend | http://localhost:5174   |
| API      | http://localhost:3001   |
| S3 API   | http://localhost:8333   |
| Filer UI | http://localhost:8888   |
