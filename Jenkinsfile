pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven_new"
    }

    stages {
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archive 'target/*.jar'
            }
        }
           stage('Test') {
            steps {
                sh "ls ; mvn test"
            }
            post{
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco (execPattern: 'target/jacoco.exec')
                  }
             }
        }
           stage('Docker Build and Push') {
            steps {
              withDockerRegistry(credentialsId: 'gitlab-arijit', url: 'https://registry.gitlab.com/')  {
                sh 'printenv'
                sh 'docker build -t registry.gitlab.com/ArijitPaul24/numeric-app:""$GIT_COMMIT"" .'
                sh 'docker push registry.gitlab.com/ArijitPaul24/numeric-app:""$GIT_COMMIT""'
            }
         }
      }
   }   
}
