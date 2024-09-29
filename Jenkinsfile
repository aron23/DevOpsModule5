pipeline{
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    
    
    stages{
        stage('Build') { 
            steps {
				sh '''echo ">> build step"'''
                sh 'mvn -B -DskipTests clean package' 
            }
        }
    }
}