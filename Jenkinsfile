#!groovy
import groovy.json.JsonSlurperClassic

node {
    try{
        def SF_HUB_ORG=env.SF_HUB_ORG
        def SF_HOST = env.SF_HOST
        def SF_JWT_CRED_ID = env.JWT_CRED_ID
        def SF_CONSUMER_KEY=env.SF_CONSUMER_KEY
        def DEPLOYDIR='src'
        def TEST_LEVEL='RunLocalTests'

        println 'KEY IS' 
        println SF_JWT_CRED_ID
        println SF_HUB_ORG
        println SF_HOST
        println SF_CONSUMER_KEY
        
        def toolbelt = tool 'toolbelt'

        stage('Init test'){
        echo "Init test"
        }

        echo "Check Out the Source Code"
        stage('checkout source') {
            // when running in multi-branch job, one must issue this command
            checkout scm
        }

            echo "Wrap All Stages in a withCredentials Command"
            withCredentials([file(credentialsId: SF_JWT_CRED_ID, variable: 'jwt_key_file')]) {
                
                stage('Authorise to Saleforce') {
                    rc = command "${toolbelt}/sfdx force:auth:jwt:grant --instanceurl ${SF_HOST} --clientid ${SF_CONSUMER_KEY} --jwtkeyfile ${server_key_file} --username ${SF_HUB_ORG} --setalias UAT "

                    if (rc != 0) { 
                        error 'hub org authorization failed' 
                    }    
                }
                    
                stage('Deploy and Run Tests') {
                    rc = command "${toolbelt}/sfdx force:mdapi:deploy --wait 10 --deploydir ${DEPLOYDIR} --targetusername UAT --testlevel ${TEST_LEVEL}"
		            
                    if (rc != 0) {
			        error 'Salesforce deploy and test run failed.'
		            }
                }
            }

    } 
    
    catch(err){
        echo "build failed"
    }    
}

def command(script) {
        if (isUnix()) {
            return sh(returnStatus: true, script: script);
        } else {
		    return bat(returnStatus: true, script: script);
        }
    }
