def remote = [:]
remote.name = "ubuntu"
remote.host = "35.159.24.124"
remote.allowAnyHosts = true
node
{
    withCredentials([sshUserPrivateKey(credentialsId: '01eb9d49-682c-4e68-94a6-ec77889de9aa', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) 
    {
        remote.user = userName
        remote.identityFile = identity
        withCredentials(
        [[
            $class: 'AmazonWebServicesCredentialsBinding',
            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
            credentialsId:'39c07877-ebc4-4f70-a4ca-084feda446e1',  // ID of credentials in kubernetes
            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
        ]]) 
        {
          
           // stage("test")
            //{
              //  sh'aws iam create-service-linked-role --aws-service-name "elasticloadbalancing.amazonaws.com"'
            //}
           
         stage("Export")
            {
                sh'export KOPS_STATE_STORE=s3://k8s.taleas.in'
            }
           
            stage("Describe Services")
            {
              sh'kubectl describe services my-appnew'
            }
           stage("My-appNew"){
                sh 'kubectl get svc my-appnew'
            }
           stage("Node")
          { sh'kubectl get nodes'
            }
             stage("Slack message")
            {
                slackSend message:"Build finished "
            }  
        }  
    }
}
