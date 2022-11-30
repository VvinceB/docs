podTemplate(yaml:'''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: dockergw
    image: docker:19.03.1
    command:
    - sleep
    args:
    - 99d
'''){
    node(POD_LABEL) {
        container('dockergw') {
            def dockerEndpoint="tcp://192.168.23.17:2376"

            echo "docker endpoint is set to :$dockerEndpoint"
            
            def image="squidfunk/mkdocs-material:latest"
            def volumes="$WORKSPACE:/docs"
            def options="-it --rm"

            echo "using container from image $image (volumes=$volumes ,options=$options)" 

            def workspace = WORKSPACE
            echo "Current workspace is $workspace"
            
            checkout scm
            echo('Build') 
            
            docker.withServer(dockerEndpoint) {
                docker.image(image).run("-v $volumes $options","build")
            }
            
            echo 'Deploy'  
            docker.withServer(dockerEndpoint) {
                docker.image(image).run("-v $volumes $options","gh-deploy")
            }
        }
    }
}