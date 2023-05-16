pipeline{
    agent any;
    stages{
        stage ('check kubectl installed'){
            steps{
                sh '''kubectl version
                 if [ $? == 0 ]
                 then
                   echo "kubectl installed"
                 else
                   exit 1
                 fi
                '''
            }
        }
        stage("connectivity check to EKS"){
            steps{
               sh ''' kubectl get no
                 if [ $? == 0 ]
                 then
                   echo "connected to EKS"
                 else
                   exit 1
                 fi
                    '''
            }
        }
        stage("deploy mariadb"){
            steps{
                sh ''' kubectl apply -f maridb-secret.yaml
                       sleep 5s
                       kubectl apply -f mariadb.yaml
                '''
            }
        }
        stage("deploy java spring app"){
            steps{
                sh ''' kubectl apply -f java-config.yaml
                       kubectl apply -f java-deploy.yaml
                       sleep 10s
                       kubectl apply -f java-svc.yaml
                '''
            }
        }
        stage("deploy reactjs app"){
            steps{
                sh ''' kubectl apply -f react-deploy.yaml
                       sleep 10s
                       kubectl apply -f react-svc.yaml
                '''
            }
        }
    }
}
