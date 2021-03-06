## Amazon ECS "Set Container Name in Task Definition" Action for GitHub Actions

Updates the container name in an Amazon ECS task definition JSON file, creating a new task definition file.

**Table of Contents**

<!-- toc -->

- [Usage](#usage)
- [License Summary](#license-summary)
- [Security Disclosures](#security-disclosures)

<!-- tocstop -->

## Usage

To update the container name to `database` for the `web` container in the task definition file, and then deploy the edited task definition file to ECS:

```yaml
- name: Render Amazon ECS task definition
  id: render-web-container
  uses: clarifydata/amazon-ecs-task-definition-set-container-name@master
  with:
    task-definition: task-definition.json
    container-name: postgres
    new-container-name: database

- name: Deploy to Amazon ECS service
  uses: aws-actions/amazon-ecs-deploy-task-definition@v1
  with:
    task-definition: ${{ steps.render-web-container.outputs.task-definition }}
    service: my-service
    cluster: my-cluster
```

If your task definition file holds multiple containers in the `containerDefinitions`
section which require updated container names, chain multiple executions of this action
together using the output value from the first action for the `task-definition`
input of the second:

```yaml
- name: Render Amazon ECS task definition for first container
  id: render-web-container
  uses: clarifydata/amazon-ecs-task-definition-set-container-name@master
  with:
    task-definition: task-definition.json
    container-name: postgres
    new-container-name: database

- name: Modify Amazon ECS task definition with second container
  id: render-app-container
  uses: clarifydata/amazon-ecs-task-definition-set-container-name@master
  with:
    task-definition: ${{ steps.render-web-container.outputs.task-definition }}
    container-name: web
    new-container-name: app

- name: Deploy to Amazon ECS service
  uses: aws-actions/amazon-ecs-deploy-task-definition@v1
  with:
    task-definition: ${{ steps.render-app-container.outputs.task-definition }}
    service: my-service
    cluster: my-cluster
```

See [action.yml](action.yml) for the full documentation for this action's inputs and outputs.

## License Summary

This code is made available under the MIT license.

## Security Disclosures

If you would like to report a potential security issue in this project, please do not create a GitHub issue. Instead, please follow the instructions [here](https://aws.amazon.com/security/vulnerability-reporting/) or [email AWS security directly](mailto:aws-security@amazon.com).
