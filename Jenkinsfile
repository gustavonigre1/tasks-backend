pipeline {
    agent any
    stages {
        stage ('Build Backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            } 
        }
        stage ('Deploy Backend') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }

        stage ('Deploy Frontend') {
            steps {
                dir('frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/gustavonigre1/tasks-frontend'
                    bat 'mvn clean package -DskipTests=true'
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                } 
            }
        }  
    }
    post {
            unsuccessful{
                emailext attachLog: true, body: 'teste', subject: 'Failed', to: 'gustavonigre@hotmail.com'
            }
            fixed{
                emailext attachLog: true, body: 'teste', subject: 'fixed', to: 'gustavonigre@hotmail.com'
            }
        }
}



