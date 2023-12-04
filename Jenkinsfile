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
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/shalinimeka/newspringbootapp.git']]])     
            }
        }
      stage ('Build') {
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
        withSonarQubeEnv('SonarCloud') {
          sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=newspringboot -Dsonar.organization=jen-raj -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=7d7f5e33f70d0b04536e60a90745e3081e44db8c'
          echo '<--------------- Sonar Analysis stopped  --------------->'
        }
      }
    }
    }
}
