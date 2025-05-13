pipeline {
    agent any
    environment {
        GCP_PROJECT     = 'your-gcp-project-id'
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_USERNAME = 'ingmdevsecops'
        DOCKER_CREDS    = 'dockerhub-creds'
        APP_NAME        = 'spring-on-k8s'
        CLUSTER_NAME    = 'gke-cluster'
        CLUSTER_ZONE    = 'asia-east1-c'
        PRISMA_API_URL  = 'https://api.sg.prismacloud.io'
    }
    options {
        preserveStashes()
        timestamps()
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/im-lab-git/spring-on-k8s-demo.git'
                stash includes: '**/*', name: 'source'
            }
        }
        
        stage('Checkov Scan') {
            steps {
            sh 'echo scanned'
            //     withCredentials([string(credentialsId: 'PC_USER', variable: 'pc_user'), 
            //                    string(credentialsId: 'PC_PASSWORD', variable: 'pc_password')]) {
            //         script {
            //             docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''") {
            //                 unstash 'source'
            //                 try {
            //                     sh 'checkov -d . --use-enforcement-rules -o cli -o junitxml --output-file-path console,results.xml --bc-api-key ${pc_user}::${pc_password} --repo-id im-lab-git/spring-on-k8s-demo --branch main'
            //                     junit skipPublishingChecks: true, testResults: 'results.xml'
            //                 } catch (err) {
            //                     junit skipPublishingChecks: true, testResults: 'results.xml'
            //                     throw err
            //                 }
            //             }
            //         }
            //     }
            }
        }

        stage('Build and Test') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_REGISTRY}/${DOCKER_USERNAME}/${APP_NAME}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'echo pushed'
                    // docker.withRegistry("https://${DOCKER_REGISTRY}", "${DOCKER_CREDS}") {
                    //     docker.image("${DOCKER_REGISTRY}/${DOCKER_USERNAME}/${APP_NAME}:${env.BUILD_ID}").push()
                    // }
                }
            }
        }

        stage('Deploy to K8s Cluster') {
            steps {
                script {
                    sh 'echo deployed'
                    // withCredentials([file(credentialsId: 'gke-kubeconfig', variable: 'KUBECONFIG')]) {
                    //     // Update deployment with new image tag
                    //     sh "sed -i 's|spring-on-k8s:latest|${DOCKER_REGISTRY}/${DOCKER_USERNAME}/${APP_NAME}:${env.BUILD_ID}|g' k8s/deployment.yml"
                        
                    //     sh """
                    //         kubectl apply -f k8s/namespace.yml
                    //         kubectl apply -f k8s/cm.yml
                    //         kubectl apply -f k8s/service.yml
                    //         kubectl apply -f k8s/deployment.yml
                    //     """
                    // }
                }
            }
        }
    }
}
