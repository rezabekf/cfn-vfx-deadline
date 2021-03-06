## Deployment preparation
Create a S3 bucket:
```
aws s3 mb s3://${BUCKET_NAME}
```

Download the Linux installer from [ThinkBox web site](https://downloads.thinkboxsoftware.com).
Log in with your Amazon account and download `Deadline-10.X.X.X-linux-installers.tar`.

Upload the installer to S3 bucket:
```
aws s3 cp ~/Downloads/Deadline-10.X.X.X-linux-installers.tar s3://${BUCKET_NAME}
```

Clone the GitHub repository and initialize submodules:
```bash
mkdir -p ~/Documents/vfx-deadline && cd ~/Documents/vfx-deadline
git clone https://github.com/rezabekf/cfn-vfx-deadline.git && cd cfn-vfx-deadline
git submodule init
git submodule update

```

Upload the templates to an S3 bucket:
```
aws s3 sync . s3://${BUCKET_NAME}/cfn-vfx-deadline
```

Navigate to the link and click on "Continue to Subscribe" for the official [CentOS 7 image in AWS marketplace](https://aws.amazon.com/marketplace/pp/B00O7WM7QW).
Accept the terms and conditions and wait for the subscription to complete.

This concludes the deployment preparation. 
In the next step we will deploy [CloudFormation stack](../../docs/02-DeploymentParameters/README.md).
