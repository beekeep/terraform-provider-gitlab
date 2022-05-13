---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "gitlab_project_protected_environment Resource - terraform-provider-gitlab"
subcategory: ""
description: |-
  The gitlab_project_protected_environment resource allows to manage the lifecycle of a protected environment in a project.
  Upstream API: GitLab REST API docs https://docs.gitlab.com/ee/api/protected_environments.html
---

# gitlab_project_protected_environment (Resource)

The `gitlab_project_protected_environment` resource allows to manage the lifecycle of a protected environment in a project.

**Upstream API**: [GitLab REST API docs](https://docs.gitlab.com/ee/api/protected_environments.html)

## Example Usage

```terraform
resource "gitlab_project_environment" "this" {
  project      = 123
  name         = "example"
  external_url = "www.example.com"
}

# Example with access level
resource "gitlab_project_protected_environment" "example_with_access_level" {
  project     = gitlab_project_environment.this.project
  environment = gitlab_project_environment.this.name

  deploy_access_levels {
    access_level = "developer"
  }
}

# Example with group
resource "gitlab_project_protected_environment" "example_with_group" {
  project     = gitlab_project_environment.this.project
  environment = gitlab_project_environment.this.name

  deploy_access_levels {
    group_id = 456
  }
}

# Example with user
resource "gitlab_project_protected_environment" "example_with_user" {
  project     = gitlab_project_environment.this.project
  environment = gitlab_project_environment.this.name

  deploy_access_levels {
    user_id = 789
  }
}

# Example with multiple access levels
resource "gitlab_project_protected_environment" "example_with_multiple" {
  project     = gitlab_project_environment.this.project
  environment = gitlab_project_environment.this.name

  deploy_access_levels {
    access_level = "developer"
  }

  deploy_access_levels {
    group_id = 456
  }

  deploy_access_levels {
    user_id = 789
  }
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `deploy_access_levels` (Block List, Min: 1) Array of access levels allowed to deploy, with each described by a hash. (see [below for nested schema](#nestedblock--deploy_access_levels))
- `environment` (String) The name of the environment.
- `project` (String) The ID or full path of the project which the protected environment is created against.

### Read-Only

- `id` (String) The ID of this resource.

<a id="nestedblock--deploy_access_levels"></a>
### Nested Schema for `deploy_access_levels`

Optional:

- `access_level` (String) Levels of access required to deploy to this protected environment. Valid values are `developer`, `maintainer`.
- `group_id` (Number) The ID of the group allowed to deploy to this protected environment. The project must be shared with the group.
- `user_id` (Number) The ID of the user allowed to deploy to this protected environment. The user must be a member of the project.

Read-Only:

- `access_level_description` (String) Readable description of level of access.

## Import

Import is supported using the following syntax:

```shell
# GitLab protected environments can be imported using an id made up of `projectId:environmentName`, e.g.
terraform import gitlab_project_protected_environment.bar 123:production
```