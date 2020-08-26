node('master') {
  stage('Poll') {
    checkout scm
     echo "Poll"
  }
  stage('Build'){
    withMaven(maven: 'M3') {
      sh 'mvn clean install compile package -DskipTests'
    }
  }
  stage('stash') {
    stash includes: 'target/oven-0.0.1-SNAPSHOT.jar,src/pt/Hello_World_Test_Plan.jmx',
    name: 'binary'
  }
}
node('docker_pt'){
  stage('Deploy'){
    unstash 'binary'
  }
  stage('Performance Testing'){
    sh 'java -jar target/oven-0.0.1-SNAPSHOT.jar';
    step([$class: 'ArtifactArchiver', artifacts: '**/*.jtl'])
  }
}

