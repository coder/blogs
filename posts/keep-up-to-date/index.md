# Keeping Coder templates up to date

[Templates](https://coder.com/docs/v2/latest/templates) in Coder help keep each developer's workspace consistent. When a new template version is created (e.g. with a newer version of Java or more CPU cores), all developers are notified to update their workspaces.

Updating an existing Coder template is easy. You can make changes to the template directly locally and then push the changes to Coder server in multiple ways.

To keep your Coder templates up to date, you need to push the changes to Coder server. A user with a **`template-admin`** or **`owner`** role can update templates.

## Using the Coder CLI

You can use the Coder CLI to push the changes to the template to the Coder server. This is the easiest way to update the template.

1. Install the Coder CLI. See [Installing the Coder CLI](https://coder.com/docs/v2/latest/install) for more information.
2. Run the following command to push the changes to the template to the Coder server.

```shell
coder templates push <template-name>
```

This will push the changes to the template to the Coder server, assign a random name to the template, and create a new template version. All users will then see the **Update** button on their workspace.

## Using CI/CD

You can also use CI/CD to push the changes to the template to the Coder server. This is useful if you want to automate the process of updating the template. This is the preferred method if you have a version control system for your templates and want your developers to update the template by creating a pull request.

1. Create a new repository for your template(s).
2. Create a new workflow in your repository. See [Creating a workflow file](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions#creating-a-workflow-file) for more information.
3. Create a new personal access token with the **`template-admin`** or **`owner`** role. and add it to the repository action secrets with the name `CODER_SESSION_TOKEN`.

```shell
coder tokens create --lifetime 8760h0m0s
```

4. Add the following steps to the workflow.

```yaml
name: Push template to coder server

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  workflow_dispatch:

jobs:
  deploy_template:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: true
      - name: Get short commit SHA # to use as template version name
        id: vars
        run: echo "::set-output name=sha_short::$(git rev-parse --short HEAD)"
      - name: "Install latest Coder"
        run: |
          curl -L https://coder.com/install.sh | sh
      - name: "Push template"
        run: |
          coder templates push <template-name> /--directory ./<template-directory> /
          --name ${{ steps.vars.outputs.sha_short }} /
          --yes
        env:
          # Consumed by Coder CLI
          CODER_URL: https://coder.example.com
          CODER_SESSION_TOKEN: ${{ secrets.CODER_SESSION_TOKEN }}
```

or you can use the [Update Coder Template](https://github.com/marketplace/actions/update-coder-template) action from the GitHub marketplace.

```yaml
name: Update Coder Template

on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - main
  workflow_dispatch:

jobs:
    update:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout
          uses: actions/checkout@v3
        - name: Get latest commit hash
          id: latest_commit
          run: echo "::set-output name=hash::$(git rev-parse --short HEAD)"

        - name: Update Coder Template
            uses: matifali/update-coder-template@latest
            with:
                CODER_TEMPLATE_NAME: "template-name"
                CODER_TEMPLATE_DIR: "template-directory"
                CODER_URL: "https://coder.example.com"
                CODER_TEMPLATE_VERSION: "${{ steps.latest_commit.outputs.hash }}"
                CODER_SESSION_TOKEN: ${{ secrets.CODER_SESSION_TOKEN }}
```

1. Push the changes to the template to the git repository. This will trigger the workflow and push the changes to the template to the Coder server.

> See Coder's dogfood example workflow [here](https://github.com/coder/coder/main/.github/workflows/dogfood.yaml).
>
> See a community workflow that uses [Update Coder Template](https://github.com/marketplace/actions/update-coder-template) action [here](https://github.com/matifali/coder-templates/blob/main/.github/workflows/deeplearning.yaml)
