pipeline {
    agent any

	tools {
		jdk 'jdk21'
	}

//	environment {
//		M2_INSTALL = "/usr/bin/mvn"
//	}

    stages {
        stage('Clone-Repo') {
	    	steps {
	        	checkout scm
	    	}
        }

        stage('Build') {
            steps {
                sh 'mvn install -Dmaven.test.skip=true'
            }
        }
		
        stage('Unit Tests') {
            steps {
                sh 'mvn compiler:testCompile'
                sh 'mvn surefire:test'
                junit 'target/**/*.xml'
            }
        }

        stage('Deployment') {
            steps {
                sh 'sshpass -p "wiculty" scp target/test.war wiculty@172.17.0.2:/home/wiculty/Distros/apache-tomcat-9.0.105/webapps'
                sh 'sshpass -p "wiculty" ssh wiculty@172.17.0.2 "/home/wiculty/Distros/apache-tomcat-9.0.105/bin/startup.sh"'
            }
        }
    }
}
