pipeline{
    agent any
    tools{
        maven 'maven-3.8'
    }
    stages{

        // stage('Prepare Workspace') {
        //     steps {
        //         cleanWs()
        //         checkout scm
        //     }
        // }
        // sonar cloud analysis
        stage('Compile and Run Sonar Analysis') {
    steps {
        script {
            // Use the SonarCloud token stored in Jenkins credentials
            withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                // Run Maven with correct parameters for SonarCloud
                sh '''
                    mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=devm_devm \
                    -Dsonar.organization=devm \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }
    }
}

        stage('Build docker image'){
            steps{
                withDockerRegistry([credentialsId:'dockerhubcred', url: '']) {
                    script{
                        app = docker.build("devm")
                    }
                }
            }
        }

        stage('Push Docker Image to ECR'){
            steps{
                script{
                    docker.withRegistry('https://002298879977.dkr.ecr.us-east-1.amazonaws.com/devm','ecr:us-east-1:aws-credentials') {
                    app.push("latest")
                }
                }
            }
        }

        stage('Deploy to kubernetes'){
            steps{
                script{
                   withKubeConfig([credentialsId: 'kubelogin']) {
                    sh 'kubectl delete all --all -n devsecops || true'
                    sh 'kubectl apply -f deployment.yaml -n devsecops'
                   }
                    
                }
            }
        }

        // post stage to clean
        stage('Clean up workspace'){
            steps{
                cleanWs()
            }
        }

    }


}
