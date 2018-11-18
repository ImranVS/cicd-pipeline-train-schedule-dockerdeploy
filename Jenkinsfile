pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage("Build Docker Image") {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("imranvs/train-schedule")
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistery('https://registry.hub.docker.com', 'docker_hub_login') {
                        add.push("${env.BUILD_NUMBER}")
                        add.push("latest")
                    }
                }
            }
        }
    }
}
