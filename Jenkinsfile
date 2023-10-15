// node {
// 	stage('Build') {
// 		echo "Build"
// 	}
// 	stage('Test') {
// 		echo "Test"
// 	}
// }

pipeline {
    agent any
    environment {
        dockerHome = tool 'Mydocker'
        mavenHome = tool 'Mymaven'
        PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
//         stage('Test') {
//             steps {
//                 sh 'mvn test'
//             }
//        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipsTests'
            }
       }
       stage('Build Docker Image') {
           steps {
                script{
                    dockerImage = docker.build("sunilkumarbs05/currency-exchange-devops:$env.BUILD_TAG")
                }
           }
      }

      stage('Push Docker Image') {
         steps {
              script{
                docker.withRegistry('','dockerhub') {
                    dockerImage.push();
                    dockerImage.push('latest')
                }
              }
         }
      }
    }

}
