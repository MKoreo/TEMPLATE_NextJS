# Next.js in Docker Example

Original work by KConner. Check out his repo here: https://github.com/kconner/next-js-in-docker-example
This repository demonstrates a Next.js web application that uses Docker for both deployment and a development environment.
## Features

- [x] Node app in Docker
- [x] Debuggable in VS Code
- [x] Development and Release container variants
- [x] Next.js
- [x] TypeScript
- [x] Allow running CI tools at the test stage
- [x] Allow debugging individual tests
- [x] Use multi-stage Docker builds for compact release artifacts
- [x] Use layer caching for efficient builds
- [x] Deploy to Kubernetes with kubectl
- [x] Demonstrate a complete CI workflow
- [x] Allow running CI tools against a built release artifact to validate it
## How this setup uses Docker

- `Dockerfile` describes a multi-stage build.
    - The test stage includes a copy of the checked out workspace with all packages installed. The builds for the CI and debug services stop here.
    - The build stage continues by removing development packages and preparing a release build.
    - The archive stage starts fresh and copies in just what's needed at runtime for a small release container.
- `docker-compose.yml` prepares a development environment including a debuggable web server and a Storybook server.
- `scripts/run-continuous-integration.sh` and its helper scripts perform a complete CI workflow, including building the application in Docker containers, testing it, pushing it to a container registry, and deploying it to Kubernetes.

## How to use this setup

- Clone the repo.
- Install Docker and VS Code.
- Open `app.code-workspace`. VS Code will recommend a Docker plugin if you don't have it, so install that.
- To develop in Docker, right-click `docker-compose.yml` and select Compose Up.
    - Canonically, `docker-compose up` will start debug mode too. Add `--build` if you have made changes since last time.
    - In VS Code's activity bar, click the Docker icon to view running containers.
    - Visit localhost:3000 to view the site.
    - Save a code file to hot-reload the browser.
    - Select Start Debugging from the Debug menu to attach the debugger. Then click in VS Code's gutter to set breakpoints.
    - To stop, right-click again and select Compose Down, or use `docker-compose down`.
- In addition to the Docker debugger attachment launcher, VS Code launchers also exist for debugging locally, including the web server, all unit tests, and a single unit test file.
- To build, test, and deploy in a continuous integration environment run `./scripts/run-continuous-integration.sh --image <name> --version <version>`.
    - This workflow assumes `docker` is logged into whatever registry you attempt to use for storage and that `kubectl` is configured with a context where you can perform a deployment.
