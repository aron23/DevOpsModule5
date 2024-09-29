pipeline{
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    
    
    stages{
        stage('checkout'){
            steps{
				echo ">>> checking out"
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github access', url: 'https://github.com/aron23/DevOpsModule5.git']]])
            }
        }
        stage('build'){
            steps{
			   echo ">>> building"
               bat 'mvn package'
            }
			post {
				success {
					echo ">>> archiving."
					archiveArtifacts artifacts: '**/*.jar'
				}
			}
        }
		stage('UPLOAD ARTIFACT') {
                steps {
					echo ">>> uploading"
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                        groupId: 'QA',
                        version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                        repository: "${RELEASE_REPO}",
                        credentialsId: ${NEXUS_LOGIN},
                        artifacts: [
                            [artifactId: 'hello-artifact' ,
                            classifier: '',
                            file: 'target/jb-hello-world-maven-0.2.0.jar',
                            type: 'jar']
                        ]
                    )
                }
        }		
    }
}