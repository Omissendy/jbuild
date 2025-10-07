pipeline {
    agent any

    environment {
        registry = "registry.gitlab.com/xavki/presentations-jenkins"
        IMAGE = "${registry}:version-${env.BUILD_ID}"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Omissendy/jbuild.git'
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t ${IMAGE} ."
            }
        }

        stage('Run') {
            steps {
                echo "🚀 Démarrage du conteneur sur le port 8081..."
                sh "docker run -d --name run-${env.BUILD_ID} -p 8081:80 ${IMAGE}"
                sh "sleep 5 && curl -f localhost:8081"
                sh "docker stop run-${env.BUILD_ID} && docker rm run-${env.BUILD_ID}"
            }
        }

        stage('Push') {
            steps {
                echo "📦 Push de l'image vers GitLab Registry"
                withDockerRegistry([url: "https://registry.gitlab.com", credentialsId: "reg1"]) {
                    sh "docker push ${IMAGE}"
                }
            }
        }
    }
}


