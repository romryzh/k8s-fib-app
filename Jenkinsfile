node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"

    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    registryHost = "172.18.16.10:30400/"

    stage "Build"

        sh "docker build -t ${registryHost}/server server/"
        sh "docker build -t ${registryHost}/client client/"
        sh "docker build -t ${registryHost}/worker worker/"

    stage "Push"

        sh "docker push ${registryHost}/server"
        sh "docker push ${registryHost}/client"
        sh "docker push ${registryHost}/worker"

    stage "Deploy"

        kubernetesDeploy configs: "k8s/*.yaml", kubeconfigId: 'kenzan_kubeconfig'
}