node('ubuntu-AppServer-3120')
{

def app
stage('Cloning Github')
{
    /*cloning the GitHub repository*/
    checkout scm
}

stage('Build and Tag Image')
{
    /*build the docker image and append a tag at the end*/
    
    app = docker.build('penjack/car_docker_repo')
}

stage('Post Image to Dockerhub')
{
    /*push Image to DockerHub Repository*/
    
    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials')
    {
        app.push('latest')
    }
}

stage('Prepare Environment') {
            steps {
                script {
                    // Check if any container is using port 80 and stop it
                    def containerId = sh(
                        script: "docker ps -q --filter 'publish=80'",
                        returnStdout: true
                    ).trim()
                    
                    if (containerId) {
                        sh "docker stop ${containerId}"
                        sh "docker rm ${containerId}"
                    }
                }
            }
        }


stage('Deploy')
{
    sh 'docker-compose down --remove-orphans'
    sh 'docker-compose up -d'
    
}
}
