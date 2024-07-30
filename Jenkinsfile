pipeline {
    agent any

    stages {
        
        
        stage('Checkout the Code') {
            steps {
                // Clone the repository
                git branch: 'dotnetwebapp', url: 'https://github.com/monk8081/CICDJenkins.git'
                sh 'ls -la'
            }
        }
        
        stage('Build The .NET Applicaion ') {
            steps {
                // Build the .NET application
                sh 'dotnet build'
            }
        }

        stage('Test') {
            steps {
                // Run tests for the application
                sh 'dotnet test'
            }
        }
        
        stage('Building Docker  image '){
              steps{
                 script{
                    echo "=================Build The Docker  Image =================="
                    sh 'docker build -t mmaurya694/dotnetapp:1  . '
                    
                }
            }
        }
         
        stage(' Docker image Push on Dockerhub'){
              steps{
                 script{
                    echo "=================Build and Push Image on Docker Hub=================="
                    withCredentials([usernamePassword(credentialsId:'docker_credential', passwordVariable: 'PASS', usernameVariable:'USER')]){
                       sh "echo $PASS | docker login -u  $USER --password-stdin"
                       sh 'docker push mmaurya694/dotnetapp:1'
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps{
				echo "===============Running the docker images=================== "
				sh returnStdout: true, script: "docker run --rm -d  -p 8081:5000 mmaurya694/dotnetapp:1"
			}
            
        }

        
    }

    post {
        always {
            script {
                // Cleanup Docker containers
                sh 'docker stop dotnet_project_with_CICD || true'
                sh 'docker rm dotnet_project_with_CICD || true'
            }
        }
    }
}
