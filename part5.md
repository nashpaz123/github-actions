
# GitHub Actions 

## Part 5: Real-World Scenarios and Best Practices (30 minutes)

### Common Use Cases

GitHub Actions can automate a wide range of tasks. Here are some common use cases to demonstrate the versatility of GitHub Actions:

#### Deploying Applications

One of the most common use cases for GitHub Actions is automating the deployment of applications. This ensures that your application is always up-to-date and that deployments are consistent and repeatable.

```yaml
name: Deploy Application

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Set up Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '20'

    - name: Install dependencies
      run: npm install

    - name: Build project
      run: npm run build

    - name: Deploy to server
      env:
        SSH_PRIVATE_KEY: ${{ secrets.SSH_PRIVATE_KEY }}
      run: |
        ssh -i $SSH_PRIVATE_KEY user@server "mkdir -p /var/www/myapp"
        scp -i $SSH_PRIVATE_KEY -r dist/* user@server:/var/www/myapp
```

In this example:
- The workflow triggers on pushes to the main branch.
- It checks out the code, sets up Node.js, installs dependencies, builds the project, and then deploys the built files to a remote server using SSH.

#### Automating Releases

Automating the release process can help ensure that your releases are consistent and error-free. You can use GitHub Actions to automatically create releases when a new tag is pushed to the repository.

```yaml
name: Create Release

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  release:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Create Release
      uses: actions/create-release@v1
      with:
        tag_name: ${{ github.ref }}
        release_name: Release ${{ github.ref }}
        body: |
          Release notes for ${{ github.ref }}.
        draft: false
        prerelease: false
        token: ${{ secrets.GITHUB_TOKEN }}
```

In this example:
- The workflow triggers on pushes that create new tags matching the pattern `v*.*.*`.
- It checks out the code and uses the `actions/create-release` action to create a new release with the specified tag and release notes.

#### Managing Complex Workflows

For complex projects, you might need to coordinate multiple jobs and workflows. GitHub Actions provides features like dependencies, conditional execution, and reusable workflows to help manage these scenarios.

```yaml
name: Complex Workflow sequence

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Build project
      run: npm run build

  test:
    needs: build
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Run tests if build completed
      run: npm test

  deploy:
    needs: test
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Deploy application if tested
      run: echo "Deploying application..."
```

In this example:
- The `test` job depends on the `build` job, and the `deploy` job depends on the `test` job.
- This ensures that tests are run only if the build is successful, and deployment happens only if tests pass.

###Para Para workflows
If the jobs are independent, they can run in parallel:

```yaml
name: Complex Workflow sequence

on: [push]

jobs:
  just-checkout:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

  para1:
    runs-on: ubuntu-latest

    steps:
    - name: syncing some server
      uses: sleep 30

  para2:
    runs-on: ubuntu-latest

    steps:
    - name: syncing another server
      run: sleep 30
```


### Best Practices for GitHub Actions

To get the most out of GitHub Actions, it's important to follow best practices. Here are some recommendations:

#### Security Considerations

- **Use Secrets for Sensitive Data**: Never hard-code sensitive information like API keys or passwords in your workflows. Use GitHub Secrets to store and access them securely.
- **Limit Access to Secrets**: Use fine-grained access controls to limit which jobs and actions can access your secrets.
- **Keep Actions Up-to-Date**: Regularly update the actions you use to benefit from the latest features and security fixes.
- **Review Third-Party Actions**: Carefully review the source code of third-party actions to ensure they are safe and trustworthy.

#### Workflow Optimization Tips

- **Use Caching**: Cache dependencies and build artifacts to speed up your workflows. GitHub provides caching mechanisms that can significantly reduce build times.
- **Run Jobs in Parallel**: Where possible, run jobs in parallel to reduce the overall workflow execution time.
- **Use Matrix Builds Wisely**: Use matrix builds to test across different environments, but avoid unnecessary combinations to keep workflows efficient.
- **Reuse Workflows**: Use reusable workflows to avoid duplication and simplify maintenance.

#### Example of Using Caching

```yaml
name: Caching Example

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Cache Node.js modules
      uses: actions/cache@v4
      with:
        path: ~/.npm
        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
        restore-keys: |
          ${{ runner.os }}-node-

    - name: Install dependencies
      run: npm install

    - name: Build project
      run: npm run build
```

In this example:
- The workflow caches Node.js modules to speed up subsequent runs.
- The cache key is based on the operating system and a hash of the `package-lock.json` file, ensuring the cache is updated when dependencies change.

#### Example of Reusable Workflow

```yaml
name: Reusable Workflow

on: workflow_call

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Install dependencies
      run: npm install

    - name: Build project
      run: npm run build

    - name: Run tests
      run: npm test
```

In this example:
- The reusable workflow `Reusable Workflow` can be called by other workflows using the `workflow_call` trigger.

By the end of this part, you should have a comprehensive understanding of how to use GitHub Actions for real-world scenarios and follow best practices to optimize your workflows.
