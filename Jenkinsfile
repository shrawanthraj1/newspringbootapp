pipeline {
   tools {
        maven 'maven3'
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
          sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=org-kube -Dsonar.organization=org -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=aa94284971e239c18d2c591629d16cbc88a23c78'
          echo '<--------------- Sonar Analysis stopped  --------------->'
        }
    }
    }
    }
