pipeline {
  agent any

  stages {
      stage('Build Artifact Stage') {
            steps {
              sh "mvn clean package -DskipTests=true"
              // archive 'target/*.jar' 
              archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
        }   
    
    stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }
    
    stage('Docker Build and Push') {
      steps {
         withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t gvenkat/numeric-app:""$GIT_COMMIT"" .'
          sh 'docker push gvenkat/numeric-app:""$GIT_COMMIT""'
        }
      }
    }
    
    }
}
