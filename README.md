# CP2_CLOUD-SEC

Description
------------

Repository containing a pipeline **Jenkinsfile** that is used to deploy a **EC2 instance**, and further deploy a **"LAMP" stack with WordPress** using a **Ansible playbook**.

How to use
------------

Edit `Jenkinsfile` file according to your preference, based on the variables available at the start of the file;

`EC2_SSH_KEY` : Path/Name to save the SSH Key to further connect to EC2 instance

`EC2_KEY_NAME` = Key name to be generated

`EC2_IMAGE_ID` = SO image id

`EC2_INSTANCE_TYPE` = Instance type 

`EC2_SEC_GROUP` = Security group

`EC2_USER` = EC2 instance user (SO)

`EC2_COUNT` = Instance qty. to be created

`PLAYBOOK` = Path to playbook file

`PLAYBOOK_INVENTORY` = Path to hosts file
