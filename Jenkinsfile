def remote = [:]
remote.name = "ubuntu"
remote.allowAnyHosts = true

def ID
def IP
node{
    withCredentials([sshUserPrivateKey(credentialsId: '01eb9d49-682c-4e68-94a6-ec77889de9aa', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
        remote.user = userName
        remote.identityFile = identity
        withCredentials(
        [[
            $class: 'AmazonWebServicesCredentialsBinding',
            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
            credentialsId:'39c07877-ebc4-4f70-a4ca-084feda446e1',  // ID of credentials in kubernetes
            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
        ]]) {
            stage("test"){
                sh 'kubectl get svc my-app'
            }
        }  
    }
}
