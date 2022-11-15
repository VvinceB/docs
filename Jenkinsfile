pipeline {
  agent {
    kubernetes {
      yamlFile 'agents/K8SPod.yaml'
    }
  }
    stages {
        stage('Build') { 
            steps {
                //container('mkdocs') {
                    docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material build
                //}
            }
        }
        stage('deploy') { 
            steps {
                //container('mkdocs') {
                    docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material gh-deploy
                //}
            }
        }
    }
}