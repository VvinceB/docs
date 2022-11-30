node(POD_LABEL) {
    podTemplate(yaml: readFile("./agents/K8SPod")){
        stages {
            stage ('init'){
                def dockerEndpoint="tcp://192.168.23.17:2376"

                echo "docker endpoint is set to :$dockerEndpoint"
                
                def image="squidfunk/mkdocs-material:latest"
                def volumes="$WORKSPACE:/docs"
                def options="-it --rm"

                echo "using container from image $image (volumes=$volumes ,options=$options)" 

                def workspace = WORKSPACE
                echo "Current workspace is $workspace"
            }
            stage('Pull') { 
                steps {
                    checkout scm
                }
            }
            stage('Build') { 
                steps {
                    docker.withServer(dockerEndpoint) {
                        docker.image(image).run("-v $volumes $options","build")
                    }
                }
            }
            stage('deploy') { 
                steps { 
                    docker.withServer(dockerEndpoint) {
                        docker.image(image).run("-v $volumes $options","gh-deploy")
                    }
                }
            }
        }
    }
}