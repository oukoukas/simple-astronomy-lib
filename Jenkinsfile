pipeline {
    agent any
    
    environment {
        JAVA_HOME='/usr/lib/jvm/java-1.11.0-openjdk-amd64'
    }

    stages {
        stage('Package') {
            steps {
                git 'https://github.com/oukoukas/simple-astronomy-lib'
            	sh 'mvn clean'
                sh 'mvn package' 
            }
        }
        stage('Analyse') {
            steps {
            	sh 'mvn checkstyle:checkstyle'
                sh 'mvn spotbugs:spotbugs'
                sh 'mvn pmd:pmd' 
            }
        }
        stage('Publish') {
            steps {
                archiveArtifacts '**/target/*.jar'
            }
        }
        
    }
    
    post {
        always {
            junit '**/surefire-reports/*.xml'
            
			recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
            recordIssues enabledForFailure: true, tool: checkStyle()
            recordIssues enabledForFailure: true, tool: spotBugs()
            recordIssues enabledForFailure: true, tool: cpd(pattern: '**/target/cpd.xml')
            recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
            
        }

    }

}
