
// aws iam user which is created for ECR
//ecr: this is aws service name
//us-east-1: this the region name where ecr registry created
//aws-creds: this is the credentials id which create in jenkins
awscreds = 'ecr:us-east-1:aws-creds'

// imagename for docker image tag name
imagename = "617302863543.dkr.ecr.us-east-1.amazonaws.com/rentalcars"

//This ecrRegistry is to push the docker image from jenkins to ECR
ecrRegistry = "https://617302863543.dkr.ecr.us-east-1.amazonaws.com"

node(){

   stage("Git Checkout"){
   checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'repo3git', url: 'https://github.com/muraliphani/realtimehandson.git']]])
   }
   
   stage("Maven Build"){
   sh "mvn clean install"
   }

   //stage("sonarqube"){
   //    scannerHome = tool 'SonarQubeScanner'
   //    withSonarQubeEnv('sonarqube') {
   //         sh "${scannerHome}/bin/sonar-scanner"
   //     }
   //   timeout(time: 10, unit: 'MINUTES') {
   //         waitForQualityGate abortPipeline: true
   //     }
 // }
   
  stage("Build Docker image"){

     dockerImage = docker.build( imagename + ":$BUILD_NUMBER")
  }


  stage('Upload docker Image') {
          steps{
            script {
              docker.withRegistry( ecrRegistry, awscreds) {
                dockerImage.push("$BUILD_NUMBER")
                dockerImage.push('latest')
              }
            }
          }
     }

}