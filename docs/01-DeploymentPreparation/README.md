- create s3 bucket e.g vfx-deadline-rezabekf
- download linux installer from [ThinkBox web site](https://downloads.thinkboxsoftware.com)
you need to login amazon.com account
grab .tar installer

mkdir MyFolder && cd MyFolder

git clone https://github.com/rezabekf/cfn-vfx-deadline.git
git submodule init
git submodule update



- upload files to S3
aws s3 cp . s3://MyBucketName --recursive

- next deploy [CloudFormation stack](../../docs/02-DeploymentParameters/README.md)