#!groovy
import groovy.json.JsonSlurper

node {

    def SF_HUB_ORG=env.SF_HUB_ORG //username
    def SF_HOST = env.SF_HOST //url
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

    withCredentials([file(credentialsId: SF_JWT_CRED_ID, variable: 'server_key_file'),usernamePassword(credentialsId: 'SF_HUB_ORG', passwordVariable: 'sfdc_org_consumer_key', usernameVariable: 'sfdc_org_username')]){
        stage('Authorise to Saleforce') {
            rc = command " sfdx force:auth:jwt:grant --clientid ${sfdc_org_consumer_key} --username ${sfdc_org_username} --jwtkeyfile ${server_key_file} --setalias PROD"
            if (rc != 0) { error 'hub org authorization failed' }  
            else { echo 'Authorisation successfull'} 
        }

        stage('Deploy and Run Tests') {

<<<<<<< HEAD
            dir('my-first-package'){​​
=======
            dir('my-first-package'){
>>>>>>> 6279ed3e6cb99216d353390e4e83d050ec7f741b
                rc = command "sfdx force:mdapi:deploy --wait 3 --deploydir . -u ${sfdc_org_username} "
		        if (rc != 0) {
			        error 'Salesforce deploy and test run failed.'
		        }
            }
        }

        stage ('Validate Package'){

            dir('my-first-package'){
                rm = command "sfdx force:mdapi:deploy --checkonly --deploydir . -u ${sfdc_org_username} --wait 3 --json"

                def robj = new JsonSlurper().parseText(rm)
                if (robj.status != 0) { error 'prod deploy failed: ' + robj.message }
                else { env.VALIDATION_ID = robj.result.id }
            }

        }


    } 
}

def command(script) {
    if (isUnix()) {
        return sh(returnStatus: true, script: script);
    } else {
		return bat(returnStatus: true, script: script);
    }
}
