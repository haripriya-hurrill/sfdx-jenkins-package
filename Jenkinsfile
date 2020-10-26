#!groovy
import groovy.json.JsonSlurperClassic

node {

    def SF_HUB_ORG=env.SF_HUB_ORG
    def SF_HOST = env.SF_HOST
    def SF_JWT_CRED_ID = env.JWT_CRED_ID
    def SF_CONSUMER_KEY=env.SF_CONSUMER_KEY   
      

    stage('Init test'){
        echo "Init test"
    }

    echo "Check Out the Source Code"
    stage('checkout source') {
            // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withEnv(["HOME=${env.WORKSPACE}"]){
        stage('Authorise to Saleforce') {
            echo "Wrap All Stages in a withCredentials Command"    
        }
                        
        stage('Deploy and Run Tests') {
            echo "Wrap All Stages in a withCredentials Command"
        }
    } 
}
