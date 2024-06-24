
# GitHub Actions 

## Part 2: Setting Up and Running Your First Workflow (45 minutes)

### Creating Your First Workflow

#### YAML Basics
YAML (YAML Ain't Markup Language) is a human-readable data serialization standard commonly used for configuration files. YAML uses indentation to represent the structure of data, making it easy to read and write. Here are some basic concepts to understand:

- **Key-Value Pairs**: Data in YAML is represented as key-value pairs, separated by a colon and space.

  ```yaml
  name: John Doe
  age: 30
  ```

- **Lists**: Lists in YAML are represented using a dash `-` followed by a space.
  ```yaml
  fruits:
    - Apple
    - Banana
    - Orange
  ```

- **Nested Structures**: YAML uses indentation to represent nested structures. Each level of indentation represents a level of hierarchy.
  ```yaml
  person:
    name: John Doe
    age: 30
    address:
      street: 123 Main St
      city: Anytown
      state: CA
  ```

#### Creating a `.github/workflows` Directory
To create a workflow in your GitHub repository, you need to place a YAML file inside the `.github/workflows` directory. This directory is where GitHub Actions looks for workflow definitions.

1. **Navigate to the root of your repository**: In your GitHub repository, navigate to the root directory.
2. **Create a `.github` directory**: If it doesn't already exist, create a new directory named `.github`.
3. **Create a `workflows` directory**: Inside the `.github` directory, create another directory named `workflows`.

Your directory structure should look like this:
```
/your-repo
  ├── .github
  │   └── workflows
```

#### Writing a Basic Workflow File
A workflow file defines the automated process that will be run on your repository. It specifies the events that trigger the workflow, the jobs to be run, and the steps to execute within each job. Let’s create a basic workflow that prints "Hello, World!".

1. **Create a workflow file**: Inside the `workflows` directory, create a new file named `hello-world.yml`.
2. **Add workflow content**: Add the following content to `hello-world.yml`:

```yaml
name: Hello World

on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Print Hello World
      run: echo "Hello, World!"
```

In this example:
- **name**: The workflow is named `Hello World`.
- **on**: Specifies the events that trigger the workflow. This workflow is triggered by `push` events.
- **jobs**: Defines the jobs that make up the workflow. This example has one job named `hello_world_job`.
- **runs-on**: Specifies the type of runner to use. GitHub provides runners for various operating systems. This job runs on an `ubuntu-latest` runner.
- **steps**: Lists the steps to run as part of the job. Each step can use an action or run a command directly.
  - **name: Checkout code**: This step uses the `actions/checkout` action to check out the repository’s code.
  - **name: Print Hello World**: This step runs the command `echo "Hello, World!"` to print a message.

#### Exercise 1: Hello World Workflow
Now, let’s walk through a hands-on exercise to create and run this basic workflow.

```markdown
# Exercise 1: Hello World Workflow

In this exercise, you will create a basic GitHub Actions workflow that prints "Hello, World!".

## Steps:

1. **Create the workflows directory**:
    - In your GitHub repository, create a new directory named `.github/workflows`.

2. **Create the workflow file**:
    - Inside the `workflows` directory, create a new file named `hello-world.yml`.

3. **Add workflow content**:
    - Add the following content to `hello-world.yml`:

    ```yaml
    name: Hello World

    on: [push]

    jobs:
      hello_world_job:
        runs-on: ubuntu-latest

        steps:
        - name: Checkout code
          uses: actions/checkout@v2

        - name: Print Hello World
          run: echo "Hello, World!"
    ```

4. **Commit and push your changes to the repository**:
    - Commit your changes with a message like `Add hello world workflow`.
    - Push the commit to the repository.

5. **Navigate to the Actions tab on GitHub**:
    - Go to your GitHub repository and click on the "Actions" tab.
    - You should see the "Hello World" workflow running. Click on it to see the details.
    - Verify that the workflow ran successfully and printed "Hello, World!".


### Running Tests on Push
Now that you’ve created a simple "Hello, World!" workflow, let's create a more practical workflow that runs tests every time code is pushed to the repository. This ensures that your code changes do not introduce any new bugs.

#### Writing a Workflow to Run Tests
1. **Create a workflow file**: Inside the `workflows` directory, create a new file named `run-tests.yml`.
2. **Add workflow content**: Add the following content to `run-tests.yml`:

```yaml
name: Run Tests

on: [push]

jobs:
  test_job:
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

    - name: Run tests
      run: npm test
```

In this example:
- **name**: The workflow is named `Run Tests`.
- **on**: Specifies the events that trigger the workflow. This workflow is triggered by `push` events.
- **jobs**: Defines the jobs that make up the workflow. This example has one job named `test_job`.
- **runs-on**: Specifies the type of runner to use. This job runs on an `ubuntu-latest` runner.
- **steps**: Lists the steps to run as part of the job. Each step can use an action or run a command directly.
  - **name: Checkout code**: This step uses the `actions/checkout` action to check out the repository’s code.
  - **name: Set up Node.js**: This step uses the `actions/setup-node` action to set up a Node.js environment.
  - **name: Install dependencies**: This step runs the command `npm install` to install the project dependencies.
  - **name: Run tests**: This step runs the command `npm test` to execute the tests.

#### Exercise 2: Running Tests on Push
Let's walk through a hands-on exercise to create and run a workflow that runs tests on every push.

```markdown
# Exercise 2: Running Tests on Push

In this exercise, you will create a workflow that runs tests every time code is pushed to the repository.

## Steps:

1. **Create the workflow file**:
    - In the `.github/workflows` directory, create a new file named `run-tests.yml`.

2. **Add workflow content**:
    - Add the following content to `run-tests.yml`:

    ```yaml
    name: Run Tests

    on: [push]

    jobs:
      test_job:
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
    - Commit your changes with a message like `Add run tests workflow`.
    - Push the commit to the repository.

5. **Navigate to the Actions tab on GitHub**:
    - Go to your GitHub repository and click on the "Actions" tab.
    - You should see the "Run Tests" workflow running. Click on it to see the details.
    - Verify that the workflow ran successfully and executed the tests.
```

By the end of this part, you should have a solid understanding of how to create and run basic GitHub Actions workflows. These workflows automate tasks such as printing messages and running tests on your code, which are essential steps in any CI/CD pipeline.
```
