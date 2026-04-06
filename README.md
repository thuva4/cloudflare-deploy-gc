# Cloudflare Deployment GC

A reusable GitHub Action to delete old Cloudflare **Pages** or **Workers** deployments, keeping a configurable number of recent ones.

## Usage

### Pages

```yaml
- uses: thuva4/cloudflare-deploy-gc@v1
  with:
    api-token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
    account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
    service-type: pages
    project-name: my-pages-project
    keep: "5"
```

### Workers

```yaml
- uses: thuva4/cloudflare-deploy-gc@v1
  with:
    api-token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
    account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
    service-type: workers
    script-name: my-worker
    keep: "5"
```

### Scheduled cleanup example

```yaml
name: Purge old Cloudflare deployments
on:
  schedule:
    - cron: "0 3 * * 1" # every Monday at 3am
  workflow_dispatch:

jobs:
  purge:
    runs-on: ubuntu-latest
    steps:
      - uses: thuva4/cloudflare-deploy-gc@v1
        with:
          api-token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          service-type: pages
          project-name: my-pages-project
```

## Inputs

| Input          | Required | Default | Description                                                  |
|----------------|----------|---------|--------------------------------------------------------------|
| `api-token`    | Yes      |         | Cloudflare API token                                         |
| `account-id`   | Yes      |         | Cloudflare account ID                                        |
| `service-type` | Yes      |         | Type of service: `pages` or `workers`                        |
| `project-name` | No       |         | Cloudflare Pages project name (required for `pages`)         |
| `script-name`  | No       |         | Cloudflare Workers script name (required for `workers`)      |
| `keep`         | No       | `5`     | Number of most recent deployments to keep                    |
| `dry-run`      | No       | `false` | Preview deletions without actually deleting                  |

## Outputs

| Output          | Description                     |
|-----------------|---------------------------------|
| `deleted-count` | Number of deployments deleted   |
| `kept-count`    | Number of deployments kept      |
