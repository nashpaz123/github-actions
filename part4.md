
```markdown
# GitHub Actions 

## Part 4: Custom Actions and Marketplace (30 minutes)

### Creating Custom Actions

GitHub Actions allows you to create custom actions to automate workflows in your repository. There are two main types of custom actions: JavaScript actions and Docker-based actions. Custom actions enable you to encapsulate reusable logic that can be shared across multiple workflows and repositories.

#### Writing JavaScript Actions

JavaScript actions are defined by a JavaScript file and an action metadata file. These actions can be run directly on the GitHub-hosted runner without the need for a container.

1. **Create the action metadata file**:
   - In your repository, create a new directory named `my-js-action`.
   - Inside this directory, create a file named `action.yml` with the following content:

     ```yaml
     name: 'My JS Action'
     description: 'A custom JavaScript action'
     inputs:
       name:
         description: 'The name to greet'
         required: true
     runs:
       using: 'node20'
       main: 'index.js'
     ```

     This file defines the action's name, description, inputs, and runtime environment.

2. **Create the JavaScript file**:
   - Inside the `my-js-action` directory, create a file named `index.js` with the following content:

     ```javascript
     const core = require('@actions/core');

     try {
       const name = core.getInput('name');
       console.log(`Hello, ${name}!`);
     } catch (error) {
       core.setFailed(error.message);
     }
     ```

     This script uses the `@actions/core` package to get the input value and print a greeting.

3. **Update `package.json`**:
   - Ensure you have a `package.json` file in your `my-js-action` directory with the required dependencies:

     ```json
     {
       "name": "my-js-action",
       "version": "1.0.0",
       "main": "index.js",
       "dependencies": {
         "@actions/core": "^1.2.6"
       }
     }
     ```

4. **Publish your action**:
   - Commit and push your changes to your repository. You can now use your custom action in workflows.

5. In your repository, create a new workflow file `custom-action.yml` in the .github/workflows directory.

    ```yaml
name: Custom js action

on: [push]

jobs:
  my_job:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Run my custom action
        uses: ./my-js-action
        with:
          name: 'myname'
```


#### Writing Docker-based Actions

Docker-based actions use a Docker container to run the action's logic, which can include any tools or dependencies installed in the container.

1. **Create the action metadata file**:
   - In your repository, create a new directory named `my-docker-action`.
   - Inside this directory, create a file named `action.yml` with the following content:

     ```yaml
     name: 'My Docker Action'
     description: 'A custom Docker action'
     inputs:
       name:
         description: 'The name to greet'
         required: true
     runs:
       using: 'docker'
       image: 'Dockerfile'
       args:
         - ${{ inputs.name }}
     ```

     This file defines the action's name, description, inputs, and runtime environment.

2. **Create the Dockerfile**:
   - Inside the `my-docker-action` directory, create a file named `Dockerfile` with the following content:

     ```dockerfile
     FROM node:12-slim

     COPY entrypoint.sh /entrypoint.sh
     RUN chmod +x /entrypoint.sh

     ENTRYPOINT ["/entrypoint.sh"]
     ```

3. **Create the entrypoint script**:
   - Inside the `my-docker-action` directory, create a file named `entrypoint.sh` with the following content:

     ```bash
     #!/bin/sh -l

     echo "Hello, $1!"
     ```

4. **Publish your action**:
   - Commit and push your changes to your repository. You can now use your custom action in workflows.

#### Exercise 5: Creating a Custom Action

```markdown
# Exercise 5: Creating a Custom Action

In this exercise, you will create a custom Docker-based action and use it in a workflow.

## Steps:

### Part A: Creating the Custom Docker-based Action

1. **Create the action directory**:
   - In your repository, create a new directory named `my-docker-action`.

2. **Create the action metadata file**:
   - Inside the `my-docker-action` directory, create a file named `action.yml` with the following content:

     ```yaml
     name: 'My Docker Action'
     description: 'A custom Docker action'
     inputs:
       name:
         description: 'The name to greet'
         required: true
     runs:
       using: 'docker'
       image: 'Dockerfile'
       args:
         - ${{ inputs.name }}
     ```

3. **Create the Dockerfile**:
   - Inside the `my-docker-action` directory, create a file named `Dockerfile` with the following content:

     ```dockerfile
     FROM node:12-slim

     COPY entrypoint.sh /entrypoint.sh
     RUN chmod +x /entrypoint.sh

     ENTRYPOINT ["/entrypoint.sh"]
     ```

4. **Create the entrypoint script**:
   - Inside the `my-docker-action` directory, create a file named `entrypoint.sh` with the following content:

     ```bash
     #!/bin/sh -l

     echo "Hello, $1!"
     ```

5. **Commit and push your changes**:
   - Commit your changes with a message like `Add custom Docker action`.
   - Push the commit to your repository.

### Part B: Using the Custom Action in a Workflow

1. **Create a workflow file**:
   - In the `.github/workflows` directory, create a new file named `custom-action.yml`.

2. **Add workflow content**:
   - Add the following content to `custom-action.yml`:

     ```yaml
     name: Custom Action Docker

     on: [push]

     jobs:
       custom_action_job:
         runs-on: ubuntu-latest

         steps:
         - name: Checkout code
           uses: actions/checkout@v4

         - name: Run custom action
           uses: ./.github/actions/my-docker-action
           with:
             name: 'World'
     ```

3. **Commit and push your changes to the repository**:
   - Commit your changes with a message like `Add workflow for custom action`.
   - Push the commit to the repository.

4. **Navigate to the Actions tab on GitHub**:
   - Go to your GitHub repository and click on the "Actions" tab.
   - You should see the "Custom Action" workflow running. Click on it to see the details.
   - Verify that the workflow ran successfully and printed "Hello, World!".
```

### Publishing Actions to the GitHub Marketplace

To share your custom actions with the GitHub community, you can publish them to the GitHub Marketplace.

1. **Prepare your action for release**:
   - Ensure your repository has a `README.md` file with usage instructions and examples.

2. **Tag a release**:
   - Create a new tag in your repository for the release. This can be done via the GitHub web interface or using Git commands.

     ```bash
     git tag -a v1.0.0 -m "Initial release"
     git push origin v1.0.0
     ```

3. **Publish the action**:
   - Navigate to the "Actions" tab in your repository.
   - Click on the "New Release" button.
   - Follow the prompts to publish your action to the GitHub Marketplace.

By the end of this part, you should have a solid understanding of how to create and publish custom actions for GitHub Actions. Custom actions allow you to encapsulate and share reusable logic, making your workflows more modular and maintainable.
