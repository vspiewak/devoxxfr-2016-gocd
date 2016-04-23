Devoxx FR 2016 - Continuous Delivery avec GoCD
==============================================

Introduction here: [slides.pdf](./slides.pdf)



Install a GoCD stack on EC2 :

  * GoCD Server
  * GoCD Agent(s)


Install Ansible
---------------
(On OSX for instance...)

    sudo easy_install pip
    sudo -H pip install --upgrade pip
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


Configure EC2 instances tags
----------------------------

    export CLIENT=devoxx
    export ENV=tia


Configure your VPC
------------------

Use the default VPC or create one with a public subnet

    export VPC_ID=<vpc_id>
    export SUBNET_ID=<public_subnet_id>


Configure hubot
---------------

    export HUBOT_SLACK_TOKEN=<xoxb-*******-*********************>

    export HUBOT_GOCI_CCTRAY_URL=http://gocd.devoxx.dailybrain.fr/go/cctray.xml

    export HUBOT_ANSIBLE_INVENTORY_INI=/opt/hubot/inventory/ec2.ini
    export HUBOT_ANSIBLE_INVENTORY_FILE=/opt/hubot/inventory/ec2.py
    export HUBOT_ANSIBLE_PRIVATE_KEY=/opt/hubot/<pem_file_name>.pem
    export HUBOT_ANSIBLE_PREFIX_HOSTS="devoxx_tia_"
    export HUBOT_ANSIBLE_REMOTE_USER="admin"

    export HUBOT_PEM_PATH=~/.ssh/<pem_file_name>.pem
    export HUBOT_ANSIBLE_HOST_KEY_CHECKING=False
    export HUBOT_EC2_REGION=us-east-1

    export HUBOT_AWS_ACCESS_KEY=<hubot_aws_access_key>
    export HUBOT_AWS_SECRET_KEY=<hubot_aws_secret_key>
    export HUBOT_AWS_ACCESS_KEY_ID=$HUBOT_AWS_ACCESS_KEY
    export HUBOT_AWS_SECRET_ACCESS_KEY=$HUBOT_AWS_SECRET_KEY


Create EC2 instances
--------------------

    ansible-playbook -i inventory/local playbooks/create-ec2.yml


Configure EC2 instances
-----------------------

    ansible-playbook -i inventory/ec2.py playbooks/configure-eip.yml
    rm -rf ~/.ansible/tmp
    ansible-playbook -i inventory/ec2.py playbooks/site.yml

    <or>

    ansible-playbook -i inventory/ec2.py playbooks/configure-eip.yml
    rm -rf ~/.ansible/tmp
    ansible-playbook -i inventory/ec2.py playbooks/haproxy.yml
    ansible-playbook -i inventory/ec2.py playbooks/gocd-server.yml
    ansible-playbook -i inventory/ec2.py playbooks/gocd-agent.yml
    ansible-playbook -i inventory/ec2.py playbooks/hubot.yml
    ansible-playbook -i inventory/ec2.py playbooks/nodes.yml


You can browse
--------------

  * `http://<gocd_server_url>:8153`


Terminate EC2 instances
-----------------------

    ansible-playbook -i inventory/ec2.py playbooks/terminate-ec2.yml


Source
------

* http://continuousdelivery.com/talks/
* http://fr.slideshare.net/jezhumble/continuous-delivery-5359386
* http://www.slideshare.net/fwendt/gocd-the-tool-that-jenkins-aint
* https://build.go.cd (user: view, pass: password)
