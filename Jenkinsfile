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
                    sh 'build'
                }
            }
        }
        stage('deploy') { 
            steps { 
                container('mkdocs') {
                    sh 'gh-deploy'
                }
            }
        }
    }
}