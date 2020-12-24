## packer-aws-v4-11tfe

### Notes

* This repo consists of config designed to create a packer base AWS AMI which contains a v4 mounted disk mode Terraform Enterprise instance on it
* The primary thrust is to end up with an AMI which TFC can pull whereby your terraform code checked in will include remote-execs in order to install the instance
* This image cannot bake in a running TFE instance because we do not know the DNS settings ahead of deployment so these need to be dealt with then
* This repo is directed at HashiCorp technical employees who have access to TFE licences/airgaps and need a convenient and quick way to spin up an instance on AWS for testing.
* The image uses a marketplace Ubuntu 18.04LTS as a base
* If this repo has not been updated for a while, find the latest base marketplace image with
```text
aws ec2 describe-images --owners 099720109477 --query "Images[*].[CreationDate,Name,ImageId]" --filters "Name=name,Values=ubuntu-minimal*18.04*amd64*" --region eu-west-2 --output table | sort -r | grep -Ev "^[-+]|DescribeImages" | head -1
```

### Requirements

* Packer >=1.4.5
* Working AWS user credentials in ~/.aws
* An SSH key in your AWS account called id_rsa_packer
* The following non-included three files:
  * replicated.tar.gz (get this with ```https://s3.amazonaws.com/replicated-airgap-work/replicated.tar.gz```)
  * hashicorp-employee-<employeeName>-v4.rli
  * TFE Airgap file
* If you do not have one or more of these files and/or cannot get them, contact the EA team for help

### Instructions

* Clone the repo
* Get the latest replicated.tar.gz, hashicorp-employee-<employeeName>-v4.rli and airgap file into the same directory as this README
* Edit lines 40 onwards in the packerAWSv4TFE-base.json file so the prerequisite files listed above are correctly matched with the config
* Run ```packer build packerAWSv4TFE-base.json```