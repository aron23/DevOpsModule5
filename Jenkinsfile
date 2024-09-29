pipeline{
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    
    environment {
        SNAP_REPO = 'vprofile-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin123'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = 'ec2-3-141-101-144.us-east-2.compute.amazonaws.com'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
        NEXUSPASS = credentials('nexuspass')
    }
    stages{
        stage('checkout'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github access', url: 'https://github.com/aron23/DevOpsModule5.git']]])
            }
        }
        stage('build'){
            steps{
               sh mvn -B -DskipTests clean package
            }
			post {
				success {
					archiveArtifacts artifacts: '**/*.jar'
				}
			}
        },		
		stage('UPLOAD ARTIFACT') {
                steps {
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "ec2-3-141-101-144.us-east-2.compute.amazonaws.com:8081",
                        groupId: 'QA',
                        version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                        repository: "vprofile-release",
                        credentialsId: ${NEXUS_LOGIN},
                        artifacts: [
                            [artifactId: 'jb-hello-world-maven' ,
                            classifier: '',
                            file: 'target/jb-hello-world-maven-0.7.0.jar',
                            type: 'jar']
                        ]
                    )
                }
        }		
    }
}