```markdown
# GitHub Actions 

## Part 3: Advanced Workflow Features (45 minutes)

### Using Secrets and Variables

#### Adding Secrets to Your Repository
Secrets are sensitive pieces of data, such as API keys or passwords, that you do not want to expose in your codebase. GitHub Actions allows you to securely store secrets and use them in your workflows.

1. **Navigate to the Settings tab of your repository**:
    - Go to your GitHub repository and click on the "Settings" tab.

2. **Access the Secrets section**:
    - In the left sidebar, click on "Secrets and variables" and then select "Actions".

3. **Add a new secret**:
    - Click on the "New repository secret" button.
    - Enter a name for the secret (e.g., `MY_SECRET`) and the value (e.g., `supersecretvalue`).
    - Click "Add secret".

You can now use this secret in your workflows.

#### Using Secrets in Workflows
To use the secrets in your workflows, you need to reference them using the `secrets` context.

```yaml
name: Use Secrets

on: [push]

jobs:
  use_secrets_job:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Print secret
      run: echo ${{ secrets.MY_SECRET }}
```

In this example:
- **${{ secrets.MY_SECRET }}**: This is how you reference a secret in your workflow. It uses the `secrets` context to access the value of `MY_SECRET`.

#### Exercise 3: Using Secrets and Environment Variables
Now, let’s walk through a hands-on exercise to create a workflow that uses secrets and environment variables.

```markdown
# Exercise 3: Using Secrets and Environment Variables

In this exercise, you will create a workflow that uses a secret and an environment variable.

## Steps:

1. **Add a secret to your repository**:
    - Navigate to the Settings tab of your repository.
    - Access the Secrets section under "Actions".
    - Add a new secret named `MY_SECRET` with a value of your choice.

2. **Create the workflow file**:
    - In the `.github/workflows` directory, create a new file named `use-secrets.yml`.

3. **Add workflow content**:
    - Add the following content to `use-secrets.yml`:

    ```yaml
    name: Use Secrets and Env Variables

    on: [push]

    jobs:
      use_secrets_job:
        runs-on: ubuntu-latest

        steps:
        - name: Checkout code
          uses: actions/checkout@v2

        - name: Print secret
          run: echo ${{ secrets.MY_SECRET }}

        - name: Print environment variable
          env:
            MY_ENV_VAR: 'Hello, Environment!'
          run: echo $MY_ENV_VAR
    ```

4. **Commit and push your changes to the repository**:
    - Commit your changes with a message like `Add use secrets and env variables workflow`.
    - Push the commit to the repository.

5. **Navigate to the Actions tab on GitHub**:
    - Go to your GitHub repository and click on the "Actions" tab.
    - You should see the "Use Secrets and Env Variables" workflow running. Click on it to see the details.
    - Verify that the workflow ran successfully and printed the secret and the environment variable.
```

### Matrix Builds

#### What are Matrix Builds?
Matrix builds allow you to run multiple job configurations simultaneously. This is useful for testing your code against multiple environments, such as different operating systems or software versions.

#### Configuring Matrix Builds
To set up a matrix build, you define a matrix strategy in your workflow file. This strategy specifies the different combinations of parameters to use for each job run.

```yaml
name: Matrix Build

on: [push]

jobs:
  test_job:
    runs-on: ${{ matrix.os }}

    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node: [12, 14, 16]

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: ${{ matrix.node }}

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test
```

In this example:
- **strategy**: Defines the matrix build strategy.
  - **matrix**: Specifies the parameters for the matrix build. This example tests on three operating systems (`ubuntu-latest`, `windows-latest`, and `macos-latest`) and three Node.js versions (`12`, `14`, `16`).
- **${{ matrix.os }}**: Refers to the current operating system in the matrix.
- **${{ matrix.node }}**: Refers to the current Node.js version in the matrix.

#### Exercise 4: Matrix Builds
Let's walk through a hands-on exercise to create a workflow with matrix builds.

```markdown
# Exercise 4: Matrix Builds

In this exercise, you will create a workflow that uses matrix builds to run tests on multiple environments.

## Steps:

1. **Create the workflow file**:
    - In the `.github/workflows` directory, create a new file named `matrix-build.yml`.

2. **Add workflow content**:
    - Add the following content to `matrix-build.yml`:

    ```yaml
    name: Matrix Build

    on: [push]

    jobs:
      test_job:
        runs-on: ${{ matrix.os }}

        strategy:
          matrix:
            os: [ubuntu-latest, windows-latest, macos-latest]
            node: [12, 14, 16]

        steps:
        - name: Checkout code
          uses: actions/checkout@v2

        - name: Set up Node.js
          uses: actions/setup-node@v2
          with:
            node-version: ${{ matrix.node }}

        - name: Install dependencies
          run: npm install

        - name: Run tests
          run: npm test
    ```

3. **Ensure you have a `package.json` file with a test script**:
    - Your `package.json` file should include a test script, for example:
    ```json
    {
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      }
    }
    ```

4. **Commit and push your changes to the repository**:
    - Commit your changes with a message like `Add matrix build workflow`.
    - Push the commit to the repository.

5. **Navigate to the Actions tab on GitHub**:
    - Go to your GitHub repository and click on the "Actions" tab.
    - You should see the "Matrix Build" workflow running. Click on it to see the details.
    - Verify that the workflow ran successfully and executed the tests on all specified environments.
```

By the end of this part, you should have a solid understanding of how to use advanced features of GitHub Actions, such as secrets, environment variables, and matrix builds. These features allow you to create more secure and flexible workflows that can handle various environments and configurations.
