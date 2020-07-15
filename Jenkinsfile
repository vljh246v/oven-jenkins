node('master') {
  checkout scm
  stage('Build') {
    echo "Some code compilation here..."
    withMaven(maven: 'M3') {
      sh 'mvn clean package'
    }
  }
  stage('Results') {
    junit '**/target/surefire-reports/TEST-*.xml'
    archive 'target/*.jar'
  }
}
