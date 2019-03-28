# cfn-vfx-deadline

Cloud formation template to deploy [DeadLine Database and Repository](https://docs.thinkboxsoftware.com/products/deadline/10.0/1_User%20Manual/manual/install-db-repo.html) on Linux OS
as per advanced instructions.

**! Important !**
This is extension only and will not work without [quickstart-vfx-ise](https://github.com/aws-quickstart/quickstart-vfx-ise) framework deployed first.

#### Prerequisites:
- Successfully deployed [quickstart-vfx-ise](https://github.com/aws-quickstart/quickstart-vfx-ise) framework.

- Make sure to deploy both Cloud Formation templates in a same AWS Region.

- If deploying via AWS CLI: modify repository-parameters.json with values from quickstart-vfx-ise CloudFormation Outputs tab.

- If deploying from AWS console: provide values from quickstart-vfx-ise CloudFormation Outputs tab.

#### Deployment:

- **Option 1: via AWS console**
> Go to https://console.aws.amazon.com/cloudformation/

> Double check that [quickstart-vfx-ise](https://github.com/aws-quickstart/quickstart-vfx-ise) is in Status CREATE COMPLETE.

> Upload the template file and fill in required Parameters.

> Create Stack 

- **Option 2: via AWS Command Line**
> from within the cfn-vpc-vfx-extension directory run command bellow 

`aws cloudformation create-stack --stack-name <MyStack> --template-body file://vpc-extension.template --parameters file://parameters.json --capabilities CAPABILITY_IAM --region <AWS-Region>`

> for example, if your quickstart-vfx-ise stack is deployed in Dublin region

`aws cloudformation create-stack --stack-name vfx-deadline-repo --template-body file://deadline_repository/repository.template --parameters file://deadline_repository/repository-parameters.json --capabilities CAPABILITY_IAM --region eu-west-1`

#### Testing connection from RenderFarm instance to DeadLine repository instance:

Start session to RenderFarm instance (use SSM Session Manager to connect)

> create mount point:

`$ mkdir /mnt/repo`

> mount the repo: 

`mount -t cifs -o username=deadlineuser,password=<YourPassword> //<samba_server_address>/DeadlineRepository /mnt/repo`

`<YourPassword>` is password for Samba set up earlier in the CloudFormation
`<samba_server_address>` is Deadline Repository Private IP address.