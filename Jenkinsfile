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
        
            
            echo "Create package version"
            stage('Deploy code') {
                if (isUnix()) {
                    rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${SF_CONSUMER_KEY} --username ${SF_USERNAME} --jwtkeyfile ${SERVER_KEY_CREDENTIALS_ID} --setdefaultdevhubusername --instanceurl ${SF_INSTANCE_URL}"
                } else {
                    rc = bat(returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${SF_CONSUMER_KEY} --username ${SF_USERNAME} --jwtkeyfile \"${SERVER_KEY_CREDENTIALS_ID}\" --setdefaultdevhubusername --instanceurl ${SF_INSTANCE_URL}"
                }

                if (rc != 0) { error 'hub org authorization failed' }

			    println rc
			
			    // need to pull out assigned username
			    if (isUnix()) {
				    rmsg = sh returnStdout: true, script: "${toolbelt} force:mdapi:deploy -d manifest/. -u ${SF_USERNAME}"
			    }else{
			    rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d manifest/. -u ${SF_USERNAME}"
			    }
			  
                printf rmsg
                println('Hello from a Job DSL script!')
                println(rmsg)
            }
            
        }
    
    
    }
    
    
    catch(err){
        echo "build failed"
    }

}

