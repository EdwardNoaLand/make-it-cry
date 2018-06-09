devops_host = '65.52.170.46'
web_host = '65.52.170.46'
user_id = 'user1'


stage('build'){
  node {
    git url:'https://github.com/EdwardNoaLand/make-it-cry.git', branch: 'master'
    withMaven(maven: 'mvn', mavenLocalRepo: '.repository') { sh "mvn clean compile" }
  }
}

stage('test'){
  node {
    withMaven(maven: 'mvn', mavenLocalRepo: '.repository') { sh "mvn clean test" }
  }
}


stage('publish'){
  node{
    withMaven(maven: 'mvn', mavenLocalRepo: '.repository') { sh "mvn deploy" }
  }
}


stage('deployment'){
  versionId = input message: 'Please provide versionId', ok: 'Deploy', parameters: [string(defaultValue: '', description: '', name: 'versionId')]
  node {
    sshagent(['userx']) {
      java_installer_url = "http://${devops_host}:8081/server-jre-8u171-linux-x64.tar.gz"

      sh "ssh -o StrictHostKeyChecking=no ${user_id}@${web_host} "+
      "\"bash -c 'source /cd/install-java.sh ${java_installer_url} ; " + 
      "/cd/run-app.sh ${versionId}'\""
    }
  }
}

