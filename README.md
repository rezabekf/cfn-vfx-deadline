# cfn-vfx-deadline

This is a Cloud Formation template to deploy Deadline 10 management toolkit for render farms. The template will set up Deadline Database and Repository,
two render node clients and workstation on AWS.

The template will reuse [VFX Burst Rendering environment](https://github.com/aws-quickstart/quickstart-vfx-ise) to help VFX studios
meet Independent Security Evaluators (ISE) standards and guidelines.

Below is a brief explanation of what we have generated for you:
```
.
├── LICENSE
├── README.md
├── deadline_client
│   ├── render_node.template
│   └── workstation.template
├── deadline_repository
│   └── repository.template
├── docs
│   ├── 01-DeploymentPreparation
│   │   └── README.md
│   ├── 02-DeploymentParameters
│   │   └── README.md
│   ├── 03-SetupAWSClientVPN
│   │   └── README.md
│   └── 04-SetupClientVPN
│       └── README.md
├── submodules
│   └── quickstart-vfx-ise
└── templates
    ├── deadline-stack.template
```

## Requirements
* AWS CLI already configured with Administrator permission
* Git installed