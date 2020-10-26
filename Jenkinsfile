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

    withCredentials([file(credentialsId: SF_JWT_CRED_ID, variable: 'server_key_file')]){
        stage('Authorise to Saleforce') {
            rc = command "${toolbelt}/ sfdx force:auth:jwt:grant --clientid ${SF_CONSUMER_KEY} --username ${SF_HUB_ORG} --jwtkeyfile ${server_key_file} --username ${SF_HUB_ORG} --setalias PROD"
            if (rc != 0) { error 'hub org authorization failed' }   
        }
                        
        stage('Check only deploy') {
            rc = sh command "${toolbelt}/ sfdx force:source:convert --rootdir force-app/ --outputdir src/"
            if (rc != 0) { error 'metadata convert failed' }
            rmsg = command "${toolbelt}/ sfdx force:mdapi:deploy --checkonly --deploydir src/ --targetusername PROD --testlevel RunLocalTests --wait 10 --json"
            
            def robj = new JsonSlurper().parseText(rmsg)
            if (robj.status != 0) { error 'prod deploy failed: ' + robj.message }
            else { env.VALIDATION_ID = robj.result.id }
        }

        stage('Deploy Result'){
            rc = command "${toolbelt}/sfdx force:mdapi:deploy --targetusername PROD --validateddeployrequestid ${env.VALIDATION_ID}"
            if (rc != 0) { error 'Deploy result failed' }
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