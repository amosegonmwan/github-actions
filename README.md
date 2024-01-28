# React NodeJS Project

This is a sample React NodeJS project showcasing the usage of GitHub Actions for various workflows.

## Workflow 1: Deployment project 1

This GitHub Actions workflow, named "Deployment project 1," is triggered on every push to the main branch. It consists of a series of steps to deploy a React NodeJS project. The workflow checks out the code, installs dependencies, lints the code, runs tests, builds the project, and finally, deploys the code. The deployment step only executes if all the previous steps are successful.

```yaml
name: Deployment project 1

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3

      - name: Install dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Test code
        run: npm run test

      - name: Build code
        run: npm run build

      - name: Deploy code
        run: echo "Deploying..."
        # Add conditional check to run only if previous steps are successful
        if: success()
```

## Workflow 2: Deployment project 2

The "Deployment project 2" workflow is divided into three jobs: lint, test, and deploy. This workflow is triggered on pushes to the main branch as well. The lint job checks the code for style and formatting, the test job runs the project's tests, and the deploy job builds and deploys the code. The deploy job is dependent on the success of both lint and test jobs.

```yaml
name: Deployment project 2

on:
  push:
    branches:
      - main

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Install dependencies
        run: npm ci
      - name: Lint
        run: npm run lint

  test:
    runs-on: ubuntu-latest
    needs: lint  # Make test job dependent on lint job
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Install dependencies
        run: npm ci
      - name: Test code
        run: npm run test

  deploy:
    runs-on: ubuntu-latest
    needs: [lint, test]  # Make deploy job dependent on lint and test jobs
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Install dependencies
        run: npm ci
      - name: Build code
        run: npm run build
      - name: Deploy code
        run: echo "Deploying..."
```


## Workflow 3: Handling Issues

The "Handle issues" workflow is triggered on issues created in the main branch. It contains a single job named "output-info" that runs on an Ubuntu environment. The job outputs information about the GitHub issue event, providing details about the issue created.

name: Handle issues

on: 
  issues:
    branches:
      - main

jobs:
  output-info:
    runs-on: ubuntu-latest
    steps:
      - name: Output event details
        run: echo "${{ toJson(github.event) }}"



