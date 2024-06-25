1. To enable runner diagnostic logging, set the following secret or variable in the repository that contains the workflow: ACTIONS_RUNNER_DEBUG to true. If both the secret and variable are set, the value of the secret takes precedence over the variable.

2. To enable step debug logging, set the following secret or variable in the repository that contains the workflow: ACTIONS_STEP_DEBUG to true. If both the secret and variable are set, the value of the secret takes precedence over the variable.

3. Use tmate to ssh into the runner instance:

    ```yaml
    name: Use Secrets and Env Variables

    on: [push]

    jobs:
      use_secrets_job:
        runs-on: ubuntu-latest
        environment: env1

        steps:
        - name: Checkout code
          uses: actions/checkout@v4

        - name: Print secret
          run: echo ${{ secrets.MY_SECRET }} > 1.txt

        - name: Run tmate
          uses: mxschmitt/action-tmate@v2
      ```

4. User Input Example:

```yaml
name: User Input Example

on:
  workflow_dispatch:
    inputs:
      name:
        description: 'Person to greet'
        required: true
        default: 'World'
      time_of_day:
        description: 'Time of day to greet'
        required: false
        default: 'day'

jobs:
  greet:
    runs-on: ubuntu-latest
    steps:
      - name: Print greeting
        run: echo "Good ${{ github.event.inputs.time_of_day }}, ${{ github.event.inputs.name }}!"
```

5. GH pages

```
a. Create a index.html in the root folder of your repo, 
b. go to settings -> pages , 
c. change source to github actions. 
d. choose static
Go to the gh pages url ( will be displayed in the job view, https://<your user name>.github.io/github-actions/
e. make changes to the index.html
```
