List<String> CHOICES = [];
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh 'gradle build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build and Scan Docker Image') {
            steps {
                script {
                    app = docker.build("nicholasgull/train-schedule")
                    app.inside {
                        sh """
                        echo \$(curl localhost:8080)
                        """
                    }
                }
        }
        }
        stage('User Input') {

            steps {
                script {

            
                        CHOICES = ["Deploy", "DoNotDeploy"];   

                        env.YourTag = input  message: 'Do you want to Deploy the Image?',ok : 'Deploy',id :'tag_id',

                                        parameters:[choice(choices: CHOICES, description: 'Select a tag for this build', name: 'POLICY PASSED')]

                        }          

                echo "Deploying ${env.YourTag}. Deploy Image."

            }

        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dhubtoken') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
