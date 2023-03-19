pipeline{
    agent any
    
    environment {
        LANG = 'en_US.UTF-8'
    }
    
    stages{
        stage("git clone"){
            steps{
                git 'https://github.com/Yamangoyal/Calculator_devops_miniproject.git'
            }
        }
        stage("Build"){
            steps{
                sh "mvn clean install"
            }
        }
        stage("Image Build"){
            steps{
                script{
                    docker.build("yamangoyal209/calculator_image:latest")
                }
            }
        }
        
        stage("Push our Image to DockerHub"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'password', variable: 'dockerhub_pwd')]) {
                        // some block
                        sh "docker login -u yamangoyal209 -p ${dockerhub_pwd}"
                        sh "docker push yamangoyal209/calculator_image:latest"
                    }
                }
            }
        }
        
        stage("deleting image from local"){
            steps{
                sh "docker rmi yamangoyal209/calculator_image"
            }
        }
         stage("Ansible Deploy"){
            steps{
                sh "ansible-playbook -i inventory plybk.yml"
            }
        }
    }
}
