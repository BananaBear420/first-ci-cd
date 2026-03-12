---
name: First CI/CD Project
overview: Create a simple Node.js project with tests, a GitHub Actions CI/CD pipeline that automatically runs tests on every push, and a basic deployment step — teaching the core CI/CD loop end to end.
todos:
  - id: init-project
    content: Initialize Node.js project with package.json and install Jest
    status: in_progress
  - id: write-code
    content: Create src/math.js with simple functions and tests/math.test.js with Jest tests
    status: pending
  - id: setup-git
    content: Initialize git repo, create .gitignore, make initial commit
    status: pending
  - id: create-workflow
    content: Create .github/workflows/ci.yml with build, test, and deploy steps
    status: pending
  - id: verify-local
    content: Run tests locally to confirm everything works before pushing
    status: pending
isProject: false
---

# First CI/CD Project with Node.js and GitHub Actions

## What is CI/CD?

- **CI (Continuous Integration):** Automatically run tests and checks every time you push code.
- **CD (Continuous Deployment):** Automatically deploy your code after tests pass.

We'll build a tiny Node.js app, add tests, then wire up GitHub Actions so that every `git push` triggers an automated pipeline.

## Project Structure

```
myFirstCiCd/
├── src/
│   └── math.js            # Simple module with functions to test
├── tests/
│   └── math.test.js       # Tests for the module
├── .github/
│   └── workflows/
│       └── ci.yml          # GitHub Actions workflow definition
├── package.json            # Node.js project config (scripts, dependencies)
├── .gitignore              # Ignore node_modules, etc.
└── README.md               # Quick explanation of the project
```

## Step-by-Step

### 1. Initialize the Node.js project

- Run `npm init` to create `package.json`
- Install **Jest** as a test framework (`npm install --save-dev jest`)
- Add a `"test"` script in `package.json` that runs `jest`

### 2. Write a simple app with testable code

- Create `src/math.js` with a few pure functions (e.g. `add`, `subtract`, `multiply`)
- Create `tests/math.test.js` with Jest tests that verify each function
- Confirm tests pass locally with `npm test`

### 3. Set up Git and `.gitignore`

- Run `git init`
- Create `.gitignore` to exclude `node_modules/`
- Make an initial commit

### 4. Create the GitHub Actions CI workflow

- Create `.github/workflows/ci.yml` with a workflow that:
  1. **Triggers** on every push and pull request to the `main` branch
  2. **Checks out** the code
  3. **Installs** Node.js (using `actions/setup-node`)
  4. **Installs dependencies** (`npm ci`)
  5. **Runs tests** (`npm test`)
  6. **Prints a deploy message** (simulated deployment step)

The workflow file will look roughly like:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm test

  deploy:
    needs: build-and-test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    steps:
      - run: echo "Deploying application..."
```

### 5. Push to GitHub and watch the pipeline

- Create a new GitHub repository
- Push the code
- Open the **Actions** tab on GitHub to see the pipeline run automatically

## What You'll Learn

- How a CI pipeline automatically catches bugs before they reach production
- How a CD step can be triggered only after tests pass
- The basic building blocks of any CI/CD system (trigger, build, test, deploy)
