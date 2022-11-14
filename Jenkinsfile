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
                    sh 'mkdocs build' 
                }
            }
        }
        stage('deploy') { 
            steps {
                container('mkdocs') {
                    sh 'mkdocs gh-deploy --force' 
                }
            }
        }
    }
}