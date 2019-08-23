def remote = [:]
remote.name = "ubuntu"
remote.allowAnyHosts = true
node{
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
           // stage("update")
            //{
              //  sh'kops rolling-update cluster --state s3://k8s.taleas.in'
            //}
            //stage("test0")
            //{  
              //sh' kops validate cluster --state s3://k8s.taleas.in'
            //}
            stage("Test1")
            {
              sh'kubectl describe services my-app'
            }
            stage("Test2"){
                sh 'kubectl get svc my-app'
            }
            stage("Node")
            { sh'kubectl get nodes'
            }
            stage("Dashboard")
            {
              sh'kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml'
            }
        }  
    }
}
