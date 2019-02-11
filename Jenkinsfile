podTemplate(label: 'kubectl-heptio-builder',
            containers: [                  
                    containerTemplate(name: 'docker', image: 'docker/compose:1.21.0', command: 'cat', ttyEnabled: true)
            ],
            volumes: [
                    hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')
            ]) {

        node('kubectl-heptio-builder') {
            
            stage('Checkout') {
              checkout scm
            }     

            stage('Build docker images') {
                def GIT_COMMIT = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                container('docker') {
                    def SERVICE_NAME = "kubectl-helm-heptio"
                    def DOCKER_IMAGE_REPO = "zeppelinops/kubectl-helm-heptio"

                    withDockerRegistry(credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/') {                  
                        sh """
                           docker-compose up --build
                           docker tag kubectl-helm-heptio:2.12.3 ${DOCKER_IMAGE_REPO}:2.12.3                                     
                           docker tag kubectl-helm-heptio:2.12.3 ${DOCKER_IMAGE_REPO}:latest
                           docker push ${DOCKER_IMAGE_REPO}:2.12.3                             
                           docker push ${DOCKER_IMAGE_REPO}:latest                           
                           """ 
                    }
                }
            }
        }
    }
