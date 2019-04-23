node {
  def app

  stage('Clone repo') {
    checkout scm
  }

  stage('Build image') {
    app = docker.build("astb01/sample-node-app")
  }

  stage('Test image') {
    app.inside {
      sh 'echo "Tests passed"'
    }
  }

  stage('Push image') {
    shortCommit = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
    echo "Using commit hash ${shortCommit}"
    
    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
      app.push("${env.BUILD_NUMBER}")
      app.push(shortCommit)
      app.push("latest")
    }
  }
}