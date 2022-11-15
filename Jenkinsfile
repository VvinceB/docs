pipeline {
  agent {
    kubernetes {
      yamlFile 'agents/K8SPod.yaml'
    }
  }
    stages {
        stage('Build') { 
            steps {
                container('mkdocs') {
                    build
                }
            }
        }
        stage('deploy') { 
            steps {
                container('mkdocs') {
                    gh-deploy
                }
            }
        }
    }
}