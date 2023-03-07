# Keeping Coder templates up to date

Updating an existing Coder template is easy. You can make changes to the template directly locally and then push the changes to Coder server in multiple ways.

- [Using the Coder CLI](#using-the-coder-cli)
- [Using CI/CD](#using-cicd)
- Using the Coder UI (coming soon) see

To keep your Coder templates up to date, you need to push the changes to Coder server. A user with a **`template-admin`** or **`owner`** role can run the following command to update the template:

```bash
coder templates push <template-name>
```

This will push the changes to the template to the Coder server, assign a random name to the template, and create a new template version. All users will then see the **Update** button on their workspace.
