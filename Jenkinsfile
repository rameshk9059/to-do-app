pipeline {
    agent any
    environment {
        SCANNER_HOME = tool 'Sonar-Scanner'
    }
    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/rameshk9059/to-do-app.git'
            }
        }

        stage('Sonarqube-Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-cred-new', variable: 'SONAR_LOGIN')]) {
                    sh '''
           
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.url=http://192.168.203.132:9000/ \
                        -Dsonar.login=$SONAR_LOGIN \
                        -Dsonar.projectName=todo \
                        -Dsonar.sources=. \
                        -Dsonar.projectKey=todo
                    '''
                }
            }
        }
        
        stage('OWASP Dependency Check') {
            steps {
               dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'owasp'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

        stage('Docker build via ansible') {
            steps {
                withCredentials([string(credentialsId: 'sudo_pass', variable: 'SUDO'), usernamePassword(credentialsId: 'docker_hub_cred', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {

                 sh '''
                 export ANSIBLE_HOST_KEY_CHECKING=False
                  ansible-playbook -i /etc/ansible/hosts /etc/ansible/deployment.yml \
                        -e dockerhub_username=$DOCKERHUB_USERNAME \
                        -e dockerhub_password=$DOCKERHUB_PASSWORD \
                        -e ansible_become_pass=${SUDO}

                 '''
                 }
            }

            }
        

    }
}
