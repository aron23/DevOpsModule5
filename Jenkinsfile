pipeline{
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    stages{
		stage('Clone sources') {
			git url: 'https://github.com/aron23/DevOpsModule5'
		}
		stage('Maven build') {
			buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install'
		}		
    }
}