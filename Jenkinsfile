pipeline {
  agent {
    kubernetes {
      yamlFile 'agents/K8SPod.yaml'
    }
  }
    stages {
        stage('Build') { 
            container('mkdocs') {
                steps {
                    build
                }
            }
        }
        stage('deploy') { 
            container('mkdocs') {
                steps {
                    gh-deploy
                }
            }
        }
    }
}