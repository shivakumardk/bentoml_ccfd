node {
         stage("Git Clone"){

         git credentialsId: 'Git-Hub-Credentials', url: "https://github.com/shivakumardk/bentoml_ccfd.git"
         
         stage("Docker build"){
             sh 'docker version'
             sh 'pip install -r requirements.txt'
             sh 'python3 train.py'
             sh 'bentoml build .'
             sh 'bentoml containerize xgb_classifier1:latest -t dkshivakumar/xgb_classifier1:latest'
         
         }
         stage("Docker Login"){
                   
             withCredentials([string(credentialsId: 'dkshivakumar', variable: 'PASSWORD')]) {
        	    sh "docker login -u dkshivakumar -p ${PASSWORD}"
         }
         }
         stage("Push image to docker hub"){
             sh 'docker push dkshivakumar/xgb_classifier1:latest'
         }

         stage("Kubernetes deployment"){
             kubernetesDeploy(configs: 'deploymentservice.yaml', kubeconfigId: 'kuberneteskey')
           }

        }
    }
