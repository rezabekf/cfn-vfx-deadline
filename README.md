# cfn-vfx-deadline

This is a CloudFormation template to deploy Deadline 10 management toolkit for VFX render farms. The template will set-up the Deadline Database and Repository, two render node clients and a workstation on AWS.

The template will reuse [VFX Burst Rendering environment](https://github.com/aws-quickstart/quickstart-vfx-ise) to help VFX studios meet the Independent Security Evaluators (ISE) standards and guidelines.

Below is a brief overview of the project structure:
```
.
├── LICENSE
├── README.md
├── deadline_client
│   ├── render_node.template
│   └── workstation.template
├── deadline_license_server
│   └── license_server.template
├── deadline_repository
│   └── repository.template
├── docs
│   ├── 01-DeploymentPreparation
│   │   └── README.md
│   ├── 02-DeploymentParameters
│   │   └── README.md
│   ├── 03-SetupAWSClientVPN
│   │   └── README.md
│   └── 04-CleanUp
│       └── README.md
├── submodules
│   └── quickstart-vfx-ise
└── templates
    └── deadline-stack.template

```

## Requirements
* AWS CLI already configured with Administrator permission
* Git installed

## Deployment instructions
[Deployment preparation](docs/01-DeploymentPreparation/README.md)
