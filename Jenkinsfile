pipeline {
    /* On dit à Jenkins d'exécuter le pipeline sur n'importe quel agent.
       ⚠ Cet agent doit avoir Docker installé et le droit d'exécuter "docker build / docker push". */
    agent any

    /* Variables globales */
    environment {
        // Nom local de l'image 
        backend-image = ""
        frontend-image = ""
        
        // Repo folders
        
        backendF = ""
        frontendF = ""

        // URL du repo GitHub
        GIT_REPO = ""
    }

    stages {

        stage('Checkout') {
            steps {
                echo "==> Récupération du code source depuis GitHub"

                /* Clone le repo avec tes identifiants Jenkins ''
                   - branch: mets 'main' ou 'master' selon ta branche */
                git branch: 'master',
                    credentialsId: '',
                    url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image emp -- backend') {
            steps {
                echo "==> Build de l'image Docker locale"

                /* On construit l'image Docker en tag 'latest'
                   Le Dockerfile doit être à la racine du repo */
                sh """
                    docker build -t ${backend-image}:latest ${backendF}
                """
            }
        }

        stage('Push Docker Image emp -- backend') {
            steps {
                echo "==> Push de l'image sur Docker Hub (tag: latest)"

                /* On utilise les identifiants Jenkins ''
                   ATTENTION :
                   - usernameVariable = ton username Docker Hub
                   - passwordVariable = le mot de passe / token Docker Hub
                */
                script {
                    withCredentials([usernamePassword(
                        credentialsId: '',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKERHUB_PASS'
                    )]) {

                        // login Docker Hub
                        sh """
                            echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USER}" --password-stdin
                        """

                        // retag l'image locale avec ton namespace Docker Hub
                        sh """
                            docker tag ${backend-image}:latest ${DOCKERHUB_USER}/${backend-image}:latest
                            docker push ${DOCKERHUB_USER}/${backend-image}:latest
                        """

                        // logout 
                        sh "docker logout"
                    }
                }
            }
        }

        stage('Build Docker Image emp -- frontend') {
            steps {
                echo "==> Build de l'image Docker locale"

                /* On construit l'image Docker en tag 'latest'
                   Le Dockerfile doit être à la racine du repo */
                sh """
                    docker build -t ${frontend-image}:latest ${frontendF}
                """
            }
        }

        stage('Push Docker Image emp -- frontend') {
            steps {
                echo "==> Push de l'image sur Docker Hub (tag: latest)"

                /* On utilise les identifiants Jenkins ''
                   ATTENTION :
                   - usernameVariable = ton username Docker Hub
                   - passwordVariable = le mot de passe / token Docker Hub
                */
                script {
                    withCredentials([usernamePassword(
                        credentialsId: '',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKERHUB_PASS'
                    )]) {

                        // login Docker Hub
                        sh """
                            echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USER}" --password-stdin
                        """

                        // retag l'image locale avec ton namespace Docker Hub
                        sh """
                            docker tag ${frontend-image}:latest ${DOCKERHUB_USER}/${frontend-image}:latest
                            docker push ${DOCKERHUB_USER}/${frontend-image}:latest
                        """

                        // logout 
                        sh "docker logout"
                    }
                }
            }
        }

    }

    post {
        always {
            echo "Pipeline terminé."
        }
    }
}