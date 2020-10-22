#!groovy

import groovy.json.JsonSlurperClassic

node {
    try{
        
        echo "define variable"
        def SF_CONSUMER_KEY=env.SF_CONSUMER_KEY
        def SERVER_KEY_CREDENTIALS_ID=env.SERVER_KEY_CREDENTIALS_ID
        def SF_INSTANCE_URL = env.SF_INSTANCE_URL ?: "https://login.salesforce.com"
        def SF_USERNAME=env.SF_USERNAME
        
        echo "Key is"
        echo SF_CONSUMER_KEY
        echo SERVER_KEY_CREDENTIALS_ID
        echo SF_INSTANCE_URL
        echo SF_USERNAME
        
        def toolbelt = tool 'toolbelt'
        
        stage('Init test'){
        echo "Init test"
        echo "Test1"
        }
        
        echo "Check Out the Source Code"
        stage('Checkout'){
            checkout scm
        }
        
        echo "Wrap All Stages in a withCredentials Command"
        withCredentials([file(credentialsId: SERVER_KEY_CREDENTIALS_ID, variable: 'server_key_file')]) {
        
            echo "Authorize Your Dev Hub Org with JWT key and give it an alias"
            stage('Authorize DevHub') {   
                rc = command "${toolbelt}/sfdx force:auth:jwt:grant --instanceurl ${SF_INSTANCE_URL} --clientid ${SF_CONSUMER_KEY} --username ${SF_USERNAME} --jwtkeyfile ${server_key_file} --setdefaultdevhubusername --setalias HubOrg"
                if (rc != 0) {
                    error 'Salesforce dev hub org authorization failed.'
                }
            }
            
            echo "Create package version"
            stage('Create Package Version') {
                if (isUnix()) {
                    output = sh returnStdout: true, script: "${toolbelt}/sfdx force:package:version:create --package ${PACKAGE_NAME} --installationkeybypass --wait 10 --json --targetdevhubusername HubOrg"
                } else {
                    output = bat(returnStdout: true, script: "${toolbelt}/sfdx force:package:version:create --package ${PACKAGE_NAME} --installationkeybypass --wait 10 --json --targetdevhubusername HubOrg").trim()
                    output = output.readLines().drop(1).join(" ")
                }

                // Wait 5 minutes for package replication.
                sleep 300

                def jsonSlurper = new JsonSlurperClassic()
                def response = jsonSlurper.parseText(output)

                PACKAGE_VERSION = response.result.SubscriberPackageVersionId

                response = null

                echo ${PACKAGE_VERSION}
            }
            
        }
    
    
    }
    
    
    catch(err){
        echo "build failed"
    }

}

