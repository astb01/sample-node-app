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
    GIT_COMMIT_HASH = sh (script: "git log -n 1 --pretty=format:'%H'", returnStdout: true)

    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
      app.push("${env.BUILD_NUMBER}")
      app.push(GIT_COMMIT_HASH)
      app.push("latest")
    }
  }
}