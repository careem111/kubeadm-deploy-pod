pipeline{
    agent {
     kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        metadata:
          name: mypod
          namespace: default
        spec:
          serviceAccount: jenkins-agent-sa
          containers:
          - name: build-agent
            image: careem785/jenkins-build-agent:2.0
            command: 
             - cat
            tty: true
      '''
        }
    }
    stages {     
      stage('Helm Chart'){
        steps{
          container('build-agent'){
              dir('charts') {
                withCredentials([usernamePassword(credentialsId: 'jfrog', usernameVariable: 'username', passwordVariable: 'password')]) {
                      sh '/usr/local/bin/helm repo add dptweb-helm-local  https://kubekrm.jfrog.io/artifactory/dpthelm-helm-local --username $username --password $password'
                      sh "/usr/local/bin/helm repo update"
                      sh "/usr/local/bin/helm install dptweb-dev dptweb-helm-local/webapp --namespace dev -f values.yaml"
                      sh "/usr/local/bin/helm list -a --namespace dev"
                  }
                }
             }
          }
      }
    }
}