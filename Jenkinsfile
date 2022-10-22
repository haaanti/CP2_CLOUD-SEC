pipeline {
    agent any

    environment {

        EC2_SSH_KEY = "/home/matheus/cp2_cloud/cp2key"
        EC2_KEY_NAME = "cp2key"
        EC2_IMAGE_ID = "ami-0ee23bfc74a881de5" 
        EC2_INSTANCE_TYPE = "t2.micro" 
        EC2_SEC_GROUP = "sg-0cd56566f3daa32a4"
        EC2_USER = "ubuntu"
        EC2_COUNT = 1

        PLAYBOOK = "/home/matheus/cp2_cloud/wordpress-lamp_ubuntu1804/playbook.yml"
        PLAYBOOK_INVENTORY = '/home/matheus/cp2_cloud/wordpress-lamp_ubuntu1804/hosts'

        TMP_INFO='/tmp/$(date +%s)'
    }

    stages {

        stage('[EC2] - CREATE SSH-KEY') {
            steps {
                sh "aws ec2 create-key-pair --key-name $EC2_KEY_NAME --query KeyMaterial --output text > $EC2_SSH_KEY"
            }
        }

        stage('[EC2] - DEPLOY EC2') {
            steps {
                sh "aws ec2 run-instances --image-id $EC2_IMAGE_ID --instance-type $EC2_INSTANCE_TYPE --count $EC2_COUNT --key-name $EC2_KEY_NAME --security-group-ids $EC2_SEC_GROUP > $TMP_INFO"
            }
        }
        
        stage('[EC2] - GET EC2 IP FOR PLAYBOOK') {
            steps {
                sh 'sleep 5'
                sh 'INSTANCE_ID=$(cat $TMP_INFO | grep -i instanceid | cut -d "|" -f 4 | xargs); EC2_IP=$(aws ec2 describe-instances | grep "$INSTANCE_ID" -A10 | grep -i publicipaddress | cut -d "|" -f 5 | xargs); echo "$EC2_IP" > $PLAYBOOK_INVENTORY'
            }
        }

        stage('[ANSIBLE] DEPLOY LAMP+WP PLAYBOOK') {
            steps {
                ansiblePlaybook become: true, becomeUser: $EC2_USER, colorized: true, credentialsId: 'ssh-key', disableHostKeyChecking: true, installation: 'AnsibleCP2', inventory: $PLAYBOOK_INVENTORY, playbook: $PLAYBOOK, sudoUser: $EC2_USER
            }
        }
    }
}
