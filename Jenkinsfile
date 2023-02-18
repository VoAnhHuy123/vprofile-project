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
    }

    stages {
        stage('Build'){
            steps {
                sh 'mvn -s settings.mxl -DskipTests install'
            }
        }
    }
}