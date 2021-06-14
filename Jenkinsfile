 node{
         stage('SCM Checkout'){
                 git 'https://github.com/shalu9999/my-app.git'
                 }
         stage('Compile-Package'){
	         def mvnHome =  tool name: 'm3', type: 'maven'   
	         sh "${mvnHome}/bin/mvn clean package"
	         sh 'mv target/myweb*.war target/newapp.war'
                 }
         stage('SonarQube Analysis') {
	         def mvnHome =  tool name: 'm3', type: 'maven'
	         withSonarQubeEnv('sonar') { 
	         sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	        }
         stage('Build Docker Imager'){
                 sh 'docker build -t shalu9999/myweb:0.0.2 .'
                 }
         
         stage('Docker Image Push'){
                withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]){                       
                sh "docker login -u shalu9999 -p ${dockerPassword}"
                }
                sh 'docker push shalu9999/myweb:0.0.2'
                }
         stage('Nexus Image Push'){
                sh "docker login -u admin -p 1234 15.207.20.208:8083"
                sh "docker tag shalu9999/myweb:0.0.2 15.207.20.208:8083/shalu9999:1.0.0"
                sh 'docker push 15.207.20.208:8083/shalu9999:1.0.0'
                }
         stage('Remove Previous Container'){
	        try{
		sh 'docker rm -f tomcattest'
	        }catch(error){
		//  do nothing if there is an exception
        	}
         stage('Docker deployment'){
                sh 'docker run -d -p 8090:8080 --name tomcattest shalu9999/myweb:0.0.2' 
                }
                }
}
