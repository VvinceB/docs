podTemplate(yaml:'''
apiVersion: "v1"
kind: "Pod"
metadata:
  labels:
    jenkins/jenkins-jenkins-agent: "true"
    jenkins/label: "jenkins-jenkins-agent"
  name: $(JENKINS_NAME)
  namespace: "jenkins"
spec:
  containers:
  - args:
    - $(JENKINS_SECRET)
    - $(JENKINS_NAME)
    env:
    - name: "JENKINS_TUNNEL"
      value: "jenkins-agent.jenkins.svc.cluster.local:50000"
    - name: "JENKINS_AGENT_WORKDIR"
      value: "/home/jenkins/agent"
    image: "jenkins/inbound-agent:4.11.2-4"
    imagePullPolicy: "IfNotPresent"
    name: "jnlp"
    resources:
      limits:
        memory: "512Mi"
        cpu: "512m"
      requests:
        memory: "512Mi"
        cpu: "512m"
    tty: false
    volumeMounts:
    - mountPath: "/home/jenkins/agent"
      name: "volume-0"
      readOnly: false
    workingDir: "/home/jenkins/agent"
  - name: dockergw
    image: docker:19.03.1
    volumeMounts:
      - name: "volume-0"
        mountPath: "/home/jenkins/agent"
    command:
      - sleep
    args:
      - 99d
  hostNetwork: false
  nodeSelector:
    kubernetes.io/os: "linux"
  restartPolicy: "Never"
  serviceAccountName: "default"
  volumes:
  - hostPath:
      path: "/home/vincent/k8s/jenkins/workspace/"
    name: "volume-0"
  - hostPath:
      path: "/home/vincent/k8s/jenkins/workspace/"
    name: "workspace-volume"
''') {
    node(POD_LABEL) {
        def dockerEndpoint="tcp://10.0.1.1:2376"
        def image="squidfunk/mkdocs-material:latest"
        def volumes="-v /home/vincent/k8s/jenkins/workspace/workspace/docs/test:/docs"
        def name="coco"
        def options="$volumes -it --name $name"
                    
        stage ('Prepare'){
            checkout scm
        }

        stage ('Build'){
            container('dockergw') {
                docker.withServer(dockerEndpoint) {
                    docker.image(image).run("$options","build -v")
                    
                    sh "docker wait $name"
                    sh "docker logs $name"
                    def status = sh(returnStdout: true, script:"docker inspect $name --format='{{.State.ExitCode}}'").trim()
                    sh "docker rm $name"
                    echo ("status<$status>")
                    if (status != "0") {
                        error "build failed"
                    }
                }
            }
        }

        stage ('Deploy'){
            container('dockergw') {
                docker.withServer(dockerEndpoint) {
                    docker.image(image).run("$options","gh-deploy --force -v")
                    
                    sh "docker wait $name"
                    sh "docker logs $name"
                    def status = sh(returnStdout: true, script:"docker inspect $name --format='{{.State.ExitCode}}'").trim()
                    sh "docker rm $name"

                    if (status != "0") {
                        error "deploy failed"
                    }
                }
            }
        }

        stage ('Clean'){

        }
    }
}