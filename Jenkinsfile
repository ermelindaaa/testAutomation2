def remote = [:]
remote.name = "ubuntu"
remote.allowAnyHosts = true
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
            stage ("Deployment & Replicas"){
                sh 'kubectl run my-app2 --image=grisildarr/repository:firsttry --replicas=2 --port=8080' 
            }
            stage ("Exposing the Deployment"){
                sh 'kubectl expose deployment my-app2 --type=LoadBalancer --port=8080 --targetPort=8080'
            }
            stage ("Autoscaling"){
                sh 'kubectl autoscale deployment my-app2 --cpu-percent=50 --min=1 --max=10'
            }
            stage ("Update"){
                sh 'kubectl set image deployment/my-app2 my-app=teaaa2000/repository:firsttry'
            }
           // stage("test")
            //{
              //  sh'aws iam create-service-linked-role --aws-service-name "elasticloadbalancing.amazonaws.com"'
            //}
            //stage("test1")
            //{
              //  sh'kubectl describe services my-app'
            //}
            stage("test2"){
                sh 'kubectl get svc my-app'
            }
        }  
    }
}
