#!groovy
import groovy.json.JsonSlurperClassic

node {

    def SF_HUB_ORG=env.SF_HUB_ORG
    def SF_HOST = env.SF_HOST
    def SF_JWT_CRED_ID = env.JWT_CRED_ID
    def SF_CONSUMER_KEY=env.SF_CONSUMER_KEY   
    def SF_JWT_CRED_ID=env.SF_JWT_CRED_ID
      

    stage('Init test'){
        echo "Init test"
    }

    echo "Check Out the Source Code"
    stage('checkout source') {
            // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: SF_JWT_CRED_ID, variable: 'server_key_file')]){
        stage('Authorise to Saleforce') {
            rc = sh returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${SF_CONSUMER_KEY} --username ${SF_HUB_ORG} --jwtkeyfile server.key --setalias PROD"
            if (rc != 0) { error 'hub org authorization failed' }   
        }
                        
        stage('Deploy and Run Tests') {
            echo "Wrap All Stages in a withCredentials Command"
        }
    } 
}
