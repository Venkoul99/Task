pipeline

{

agent

{

label 'docker-node'

}

environment

{

DOCKERHUB_CREDENTIALS=credentials('docker-hub')

}

stages

{

stage('Clone')

{

steps

{

git branch: 'main', url: 'http://192.168.99.102:3000/Venko/DemoApp'

}

}

stage('Run compose file')

{

steps

{
    
sh 'docker compose up -d'

}

}

stage('Test')

{

steps

{

script

{

echo 'Test #1 - reachability'
sh 'echo $(curl --write-out "%{http_code}" --silent --output /dev/null http://192.168.99.102:8080) | grep 200'

echo 'Test #2 - check for a city'
sh "curl --silent --data 'София' http://192.168.99.102:8080 | grep 'София'" 
}

}

}

stage('Clean')

{

steps

{

sh 'docker compose down'

}

}

stage('Deploy on port 80')

{

steps

{

sh 'docker compose -f docker-compose-deploy.yaml up -d'

}

}

stage('Docker Hub Login')

{

steps

{

sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'

}

}

stage('Push to Docker Hub')

{

steps

{

sh 'docker image tag demoapp_web venkodev/demoapp-web'
sh 'docker push venkodev/demoapp-web'

sh 'docker image tag demoapp_db venkodev/demoapp-db'
sh 'docker push venkodev/demoapp-db'

}

} 

}

}
