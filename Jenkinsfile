pipeline {
    agent any

    stages {
        stage('Zip and Push to JFrog Artifactory') {
            steps {
                // Create a zip file containing all files except Jenkinsfile
                script {
                    sh 'zip -r ansible-code.zip week18-ansible-x Jenkinsfile'
                    // Upload the zip file to JFrog Artifactory
                    sh 'curl -uadmin:AP2UiQDXx5CkVw7BxWRVtEPHVXo -T ansible-code.zip "http://107.22.132.203:8081/artifactory/ansible-pipeline/"'
                }
            }
        }
        
        stage('Download Artifact from Jfrog') {
            agent {
                label 'ansible'
            }
            steps {
                // Download the zip file from JFrog Artifactory
                script {
                    sh 'curl -uadmin:AP2UiQDXx5CkVw7BxWRVtEPHVXo -O "http://107.22.132.203:8081/artifactory/ansible-pipeline/ansible-code.zip"'
                    // Now you can use the downloaded files on the agent
                }
            }
        }
        
        stage('Run playbook') {
            agent {
                label 'ansible'
            }
            steps {
                // Unzip ansible-code.zip
                script {
                    sh 'unzip -o ansible-code.zip'
                    // Run ansible-playbook from the correct directory
                    dir('week18-ansible-x/ansible-codes') {
                        sh 'ansible-playbook -i /home/ec2-user/ansible-dev/inventory.yml /home/ec2-user/ansible-dev/workspace/week16-project/ansible-codes/play1.yml'
                    }
                }
            }
        }
    }
}
