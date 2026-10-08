pipeline{
agent any
environment{
DOCKER_IMAGE="chethasss/myapp-image"
}
stages{
stage('clone Repository'){
steps{
git 'https://github.com/chethasgowda6-art/demo.git'
}
}
stage('Build Docker Image'){
steps{
script{
docker.build("${DOCKER_IMAGE}:latest")
}
}
}
stage('login to Docker Hub'){
steps{
withCredentials([usernamePassword(
credentialsId: 'dockerhub-creds',
usernameVariable:'DOCKER_USER',
passwordVariable:'DOCKER_PASS')]){
bat 'eco $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
}
}
}
stage('Push Docker Image'){
steps{
script{
docker.withRegistry('','dockerhub-creds'){
docker.image("${DOCKER_IMAGE}:lab").push()
}
}
}
}
}
post{
success{
echo 'Image successfully build and pushed to Docker Hub'
}
failure{
echo 'pipeline failed'
}
}
}