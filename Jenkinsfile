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
        stage('Compile and Run Sonar Analysis'){
            steps{
                script {
                    withCredentials([string(credentialsId: 'Babz_token', variable: 'SONAR_TOKEN')]) {
                        sh 'mvn clean verify sonar:sonar -Dsonar.token=$Babz_token -Dsonar.organization=Babz -Dsonar.projectKey=babz_babz-Dsonar.host.url=https://sonarcloud.io'
                    }
                }
            }
        }

        stage('Run scan analysis with Snyk '){
            steps{
                script {
                    withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                        sh "mvn snyk:test -fn"
                    }
                }
            }
        }

        stage('Build docker image'){
            steps{
                withDockerRegistry([credentialsId:'dockerhubcred', url: '']) {
                    script{
                        app = docker.build("babz")
                    }
                }
            }
        }

        stage('Push Docker Image to ECR'){
            steps{
                script{
                    docker.withRegistry('https://002298879977.dkr.ecr.us-east-1.amazonaws.com/babz','ecr:us-east-1:aws-credentials') {
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
