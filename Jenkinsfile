def remote = [:]
remote.name = "ubuntu"
remote.host = "18.184.14.65"
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
              sh'kubectl describe services my-appnew'
            }
            stage("Test2"){
                sh 'kubectl get svc my-appnew'
          }
          // stage("Node")
           //{ sh'kubectl get nodes'
           //}
            stage ("Verifications"){
                sh 'kubectl cluster-info'
                sh 'kubectl get nodes'
                sh 'kubectl version --short'
            }
            stage ("Deployment of files"){
                sh 'rm -rf yamlFile'
                sh 'git clone https://github.com/ermelindaaa/yamlFile.git'
                sh "sed -i \"s/<<NFS Server IP>>/\"${remote.host}\"/g\" yamlFile/deployment.yaml"
            }
            stage ("Deployment of the 3 files"){
              //  sh 'kubectl delete deployment.apps/nfs-client-provisioner'
                sh 'kubectl create -f yamlFile/deployment.yaml' 
                sh 'kubectl create -f yamlFile/class.yaml --validate=false'
                sh 'kubectl create -f yamlFile/rbac.yaml'
            }
            stage ("Helm Installation"){
                sh 'curl -LO https://git.io/get_helm.sh'
                sh 'chmod 700 get_helm.sh'
                sh './get_helm.sh'
            }
            stage ("Prometheus pre-Installation"){
                sh 'kubectl -n kube-system create service account tiller'
                sh 'kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller'
                sh 'helm init --service-account tiller'
            }
            stage ("Waiting function"){
                /*
                    TODO - Not done yet
                */
            }
            stage ("Prometheus Configuration"){
                sh 'helm inspect values stable/prometheus > /tmp/prometheus.values'
                sh 'sed -i "s/type:ClusterIP/type:NodePort\nnodePort:32322/g" prometheus.values'

            }
            stage ("Prometheus Installation"){
                sh 'helm install stable/prometheus --name prometheus --values /tmp/prometheus.values --namespace prometheus'
                sh 'kubectl get all -n prometheus'
            }

            stage ("Grafana Configuration"){
                sh 'helm inspect values stable/grafana > /tmp/grafana.values'
                /*
                    TODO editimi i vlerave dhe passwordi dhe persistence
                */
            }
            stage ("Grafana Installation"){
                sh 'helm install stable/grafana --name grafana --values /tmp/grafana.values --namespace grafana'
            }
            stage("Slack message")
            {
                slackSend message:"build finished "
            }
        }  
    }
}
