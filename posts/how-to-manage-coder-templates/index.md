# How to manage Coder templates

Coder templates are the DNA used to create workspaces. They abstract the complexity of cloud environments, allowing developers to focus on their projects.

[![Coder templates](./static/templates.png)](./static/templates.png)

## Organizing your templates

There are many ways to organize your templates. You can use them to:

### Team based templates

A template for each of your teams e.g. a template for your _frontend_ team and a template for your _backend_ team.

### Project based templates

A template for each of your projects e.g. a template for your _cool-project_ and a template for your _awesome-project_.

### Image based templates

A single template that is used for all your projects but with a different image for each project.

## What is the difference between a template and an image?

A template is a collection of settings that are used to create a workspace. An image is a collection of software that is used to create a workspace. A template can use one or more images. For example, you can have a template that uses the _ubuntu_ image and the _node_ image and the user will have the choice of which image to use when creating a workspace. Choices are managed by a terraform variable e.g.

```hcl
    variable "image" {
      type        = string
      description = "The image to use for the workspace"
      default     = "ubuntu"
      validation {
        condition     = contains(["ubuntu", "node"], var.image)
        error_message = "The image must be either ubuntu or node"
      }
    }
```

## Creating your first template

1. Create a new repository in your GitHub account. This will be the repository that contains your Coder templates.

2. Create a new directory in your repository called `deeplearning`.

3. Create a new file in the _templates_ directory called `main.tf` This is the file that contains the Coder template.

   ```hcl
     terraform {
       required_providers {
          coder = {
            source  = "coder/coder"
            version = "0.6.12"
          }
         docker = {
           source = "kreuzwerker/docker"
           version = "3.0.1"
         }
       }
     }

     provider "docker" {
       # Configuration options
     }
   ```
