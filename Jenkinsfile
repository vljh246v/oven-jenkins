node('master') {
  stage('Poll') {
    checkout scm
     echo "Poll"
  }
  stage('Build'){
    withMaven(maven: 'M3') {
      sh 'mvn clean package -Dmaven.test.skip=true'
    }
  }
  stage('Results'){
    archiveArtifacts: 'target/hello.jar'
  }
  stage('stash') {
    stash includes: 'target/hello.jar,src/pt/Hello_World_Test_Plan.jmx',
    name: 'binary'
  }
}
node('docker_pt'){
  // stage('Start Tomcat') {
  //   sh '''cd /home/jenkins/tomcat/bin
  //    ./startup.sh''';
  // }
  stage('Deploy'){
    unstash 'binary',
    sh 'cp target/hello.jar /tmp/';
  }
  stage('Performance Testing'){
    sh 'java -jar /tmp/target/hello.jar';
    step([$class: 'ArtifactArchiver', artifacts: '**/*.jtl'])
  }
}