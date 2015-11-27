Infrastructure
==============

Install the infrastructure on EC2


Install Ansible
---------------
(On OSX for instance...)

    sudo easy_install pip
    sudo pip install --upgrade pip
    sudo -H pip install --upgrade boto
    sudo -H pip install --upgrade ansible


Configure your AWS Credentials
------------------------------

    export ANSIBLE_HOST_KEY_CHECKING=False
    export EC2_REGION=eu-west-1

    export AWS_ACCESS_KEY=...
    export AWS_SECRET_KEY=...
    export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY
    export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_KEY


Configure your EC2 pem file
---------------------------

    export PEM_NAME=<aws_pem_name>
    export PEM_PATH=~/.ssh/<aws_pem_name>.pem


Configure your Default VPC
--------------------------
Ansible can create ASG only inside the Default VPC :(

    export VPC_ID=<vpc_id>
    export SUBNET_ID=<public_subnet_id>


Configure EC2 instances tags
----------------------------

    export CLIENT=company
    export STAGE=dev


Create IAM Policy for Elasticsearch AWS EC2 plugin
--------------------------------------------------

    {
        "Statement": [
            {
                "Action": [
                    "ec2:DescribeInstances"
                ],
                "Effect": "Allow",
                "Resource": [
                    "*"
                ]
            }
        ],
        "Version": "2012-10-17"
    }


Configure Elasticsearch AWS EC2 plugin
--------------------------------------

    export ES_DISCOVERY_AWS_REGION=eu-west
    export ES_DISCOVERY_AWS_ACCESS_KEY=...
    export ES_DISCOVERY_AWS_SECRET_KEY=...


Create EC2 Infra
----------------

    ansible-playbook -i inventory/ec2.py playbooks/create-ec2.yml


Configure EC2 instance
----------------------

    ansible-playbook -i inventory/ec2.py playbooks/elasticsearch.yml


Create EC2 AMI
--------------

    ansible-playbook -i inventory/ec2.py playbooks/create-ami.yml
    export AMI_ID=<created_ami_id>


Configure EC2 ELB + LC + ASG
----------------------------

    ansible-playbook -i inventory/ec2.py playbooks/create-asg.yml


Demo
----

For demo, configure the following:

ELB with:

* Timeout: 2
* Interval: 5
* (Un)healthy Threshold: 2

Autoscaling Group with:

* Default Cooldown: 5
* Health Check Grace Period: 0



    export ELB=http://<elb_cname>:9200
    curl $ELB/comics/hero -d '{ "name" : "Batman" }'
    curl -X PUT $ELB/comics/_settings -d '{ "number_of_replicas": 2 }'

