# docker-react

A React application containerized with a multi-stage Docker build and deployed to
AWS Elastic Beanstalk through a Travis CI pipeline.

> Built in 2020 while learning Docker and CI/CD. Kept as-is (aside from docs cleanup)
> as a snapshot of that work.

## What this demonstrates

- **Multi-stage Docker build** (`Dockerfile`): a `node:alpine` builder stage compiles the
  production bundle, then only the static output is copied into a slim `nginx` image —
  the final image ships no Node, no `node_modules`, no source.
- **Separate dev workflow** (`Dockerfile.dev` + `docker-compose.yml`): hot-reloading dev
  server with the source mounted as a volume, plus a dedicated test service.
- **CI/CD** (`.travis.yml`): every push builds the image and runs the test suite in a
  container; the `master` branch auto-deploys to AWS Elastic Beanstalk (ap-south-1).

## Running it

```bash
# Development (hot reload on :3000)
docker compose up

# Production image
docker build -t docker-react .
docker run -p 8080:80 docker-react
```

## Tests

```bash
docker build -t docker-react-dev -f Dockerfile.dev .
docker run -e CI=true docker-react-dev npm run test
```
