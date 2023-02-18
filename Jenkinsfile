pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    
     environment {
        SNAP_REPO = 'snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'qwerty-release'
        CENTRAL_REPO = 'qwerty-maven-central'
        // NEXUS_VERSION = "nexus3"
        // NEXUS_PROTOCOL = "http"
        NEXUSIP = '172.31.22.190'
        NEXUSPORT = '8081'
        // NEXUS_URL = "172.31.22.190:8081"
        NEXUS_GRP_REPO = 'qwerty-maven-group'
        // NEXUS_REPOSITORY = "vprofile-release"
	    // NEXUS_REPOGRP_ID    = "vprofile-grp-repo"
        // NEXUS_CREDENTIAL_ID = "nexuslogin"
        NEXUS_LOGIN = "nexuslogin"
        // ARTVERSION = "${env.BUILD_ID}"
        SONARSERVER = 'sonarsever'
        SONARSCANNER = 'sonarscanner'
    }

    stages {
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Test'){
            steps {
                sh 'mvn -s settings.xml test'
            }
        }

        stage('Checkstyle Analysis'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }

        stage('CODE ANALYSIS with SONARQUBE') {
          
		  environment {
             scannerHome = tool "${SONARSCANNER}"
          }

          steps {
            withSonarQubeEnv("${SONARSERVER}") {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }

            timeout(time: 10, unit: 'MINUTES') {
               waitForQualityGate abortPipeline: true
            }
          }
        }
    }
}