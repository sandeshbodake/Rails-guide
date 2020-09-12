### Install elastic beanstalk 

```
brew install aws-elasticbeanstalk
```

#### Go to your project directory

#### Init elastic beanstalk
```
eb init
```

During init eb shows some CLI questions & it will create `config.yml` like below

```
branch-defaults:
  master:
    environment: staging
    group_suffix: null
global:
  application_name: testing-rails
  branch: null
  default_ec2_keyname: sandesh
  default_platform: Puma with Ruby 2.6 running on 64bit Amazon Linux
  default_region: ap-south-1
  include_git_submodules: true
  instance_profile: null
  platform_name: null
  platform_version: null
  profile: eb-cli
  repository: null
  sc: git
  workspace_type: Application
```


#### create project with postgres

```
eb create staging -db.engine postgres
```

#### Sample logs

```
2020-09-12 14:59:43    INFO    createEnvironment is starting.
2020-09-12 14:59:44    INFO    Using elasticbeanstalk-ap-south-1-575689566956 as Amazon S3 storage bucket for environment data.
2020-09-12 15:00:10    INFO    Created target group named: arn:aws:elasticloadbalancing:ap-south-1:575689566956:targetgroup/awseb-AWSEB-18R2XQ1R8YMFV/e7c91bd35ed8fb5a
2020-09-12 15:00:10    INFO    Created security group named: sg-0e97209914d26387b
2020-09-12 15:00:26    INFO    Created security group named: awseb-e-urp3iwwxh3-stack-AWSEBSecurityGroup-1AO6G8UOUXC80
2020-09-12 15:00:26    INFO    Created Auto Scaling launch configuration named: awseb-e-urp3iwwxh3-stack-AWSEBAutoScalingLaunchConfiguration-509D9VK4UOUY
2020-09-12 15:00:26    INFO    Created security group named: awseb-e-urp3iwwxh3-stack-AWSEBRDSDBSecurityGroup-16MS5ESZRS3EO
2020-09-12 15:00:41    INFO    Creating RDS database named: aa19ydkibpws6jb. This may take a few minutes.
2020-09-12 15:02:15    INFO    Created load balancer named: arn:aws:elasticloadbalancing:ap-south-1:575689566956:loadbalancer/app/awseb-AWSEB-2KHZYMLZHUJA/47bcde8d23c308c5
2020-09-12 15:02:15    INFO    Created Load Balancer listener named: arn:aws:elasticloadbalancing:ap-south-1:575689566956:listener/app/awseb-AWSEB-2KHZYMLZHUJA/47bcde8d23c308c5/c6766141be366dbc
````


