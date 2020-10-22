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
        
        stage('Init test'){
        echo "Init test"
        echo "Test1"
        }
        
        stage('Checkout'){
            checkout scm
        }
    }
    
    catch(err){
        echo "build failed"
    }

}

