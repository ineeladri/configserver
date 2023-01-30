node {
      stage("Git Clone"){

        git branch: 'main', url: 'https://github.com/ineeladri/new-configserver.git'
      }
    stage('Build Project'){
        def mvnHome = tool name: 'maven', type: 'maven'
          sh "mvn clean package"
          echo "Executed Successfully Project1"
    }
      stage("Docker build"){
        sh 'docker build -t configserver1 .'
        sh 'docker image ls'
      }
      withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'test', usernameVariable: 'ineeladri', passwordVariable: 'password']]) {
        sh 'docker login -u ineeladri -p $password'
      }
      stage("Pushing Image to Docker Hub"){
	sh 'docker tag configserver ineeladri/configserver1:latest'
	sh 'docker push ineeladri/configserver1:latest'
      }
	stage("SSH Into Server") {
       def remote = [:]
       remote.name = 'Claims-1'
       remote.host = '20.232.127.94'
       remote.user = 'azureuser'
       remote.password = 'Miracle@1234'
       remote.allowAnyHosts = true
     }
     stage("Deploy"){
	     sh 'docker stop configserver1 || true && docker rm -f configserver1 || true'
	     sh 'docker run -d -p 8888:8888 --name configserver1 configserver1:latest'
     }
}






