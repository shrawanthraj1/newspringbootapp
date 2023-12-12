pipeline {
   tools {
        maven 'Maven3'
    }
    agent any
    environment {
        registry = "034074176915.dkr.ecr.us-east-1.amazonaws.com/myrepo"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/shrawanthraj1/newspringbootapp.git']]])     
            }
        }
      stage('Build') {
          steps {
            sh 'mvn clean install'           
            }
      }
      stage('Unit Test') {
            steps {
                echo '<--------------- Unit Testing started  --------------->'
                sh 'mvn surefire-report:report'
                echo '<------------- Unit Testing stopped  --------------->'
      }
    }
    stage('Sonar Analysis') {
      environment {
        scannerHome = tool 'sonar-scanner'
      }
      steps {
        echo '<--------------- Sonar Analysis started  --------------->'
        //         withSonarQubeEnv('sonar-cloud') {
        //         sh "${scannerHome}/bin/sonar-scanner"

        // }
        withSonarQubeEnv('sonar-cloud') {
          sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=shrawanthraj1 -Dsonar.organization=shrawanthraj1 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=7d679ca3a87fb8b64d589518b4ddfd3bfa940451'
          echo '<--------------- Sonar Analysis stopped  --------------->'
        }
      }
    }
           stage('Quality Gate') {
      steps {
        script {
          echo '<--------------- Quality Gate started  --------------->'
          timeout(time: 1, unit: 'MINUTES') {
            def qg = waitForQualityGate()
            if (qg.status != 'OK') {
              error 'Pipeline failed due to the Quality gate issue'
            }
          }
          echo '<--------------- Quality Gate stopped  --------------->'
        }
      }
    }
         stage('Build Docker Image') {
             steps {
                 script {
                     // Build the Docker image
                     sh 'docker build -t myrepo .'
                 }
             }
     }
          stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 700469642939.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker tag myrepo:latest 700469642939.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest'
                sh 'docker push 700469642939.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest'
         }
        }
      }

    }
    }
    
