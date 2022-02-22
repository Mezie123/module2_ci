pipeline{
  agent any 
  tools { maven 'maven'}
  stages{
    stage('git-clone'){
      steps{
         checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'd7d508c4-91de-464e-a422-816cd6e01563', url: 'https://github.com/Mezie123/module2_ci']]])
      }
    }
    stage('etech-hello'){
      steps{
        sh 'git version'
      }
    }
   stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
   }
   stage('Unit Tests - JUnit and JaCoCo') {
      steps {
        sh 'mvn test'
        sh 'mvn -v'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }
   stage('Mutation Tests - PIT') {
      steps {
        sh "mvn org.pitest:pitest-maven:mutationCoverage"
      }
      post {
        always {
          pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
        }
      }
    }
    stage('CodeQuality-SAST'){
      steps{
        sh 'mvn clean verify sonar:sonar \
  -Dsonar.projectKey=devsecops-spring-app \
  -Dsonar.host.url=http://etechconsultingdevops.eastus.cloudapp.azure.com:9000 \
  -Dsonar.login=a4edfbfb8e683df97236dc0184c956b574fa925e'
      }
    }
  }    
}
Artifactory build:
     stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }
