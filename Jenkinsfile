#!groovy
import groovy.json.JsonSlurperClassic
import groovy.json.* 
import java.time.*


node {

    def SF_HUB_ORG=env.SF_HUB_ORG //username
    def SF_HOST = env.SF_HOST //url
    def SF_CONSUMER_KEY=env.SF_CONSUMER_KEY   
    def SF_JWT_CRED_ID=env.SF_JWT_CRED_ID
    def validationStatus = false
    def  SFDX_PROJECT = ""

    stage('Init test'){
        println "Init test"
    }

    println "Check Out the Source Code"
    stage('checkout source') {
            // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: SF_JWT_CRED_ID, variable: 'server_key_file'),usernamePassword(credentialsId: 'SF_HUB_ORG', passwordVariable: 'sfdc_org_consumer_key', usernameVariable: 'sfdc_org_username')]){
        stage('Authorise to Saleforce') {
            rc = command " sfdx force:auth:jwt:grant --clientid ${sfdc_org_consumer_key} --username ${sfdc_org_username} --jwtkeyfile ${server_key_file} --setalias PROD"
            if (rc != 0) { error 'hub org authorization failed' }  
            else { println 'Authorisation successfull'} 
        }

        

        stage ('Validate Package'){

            dir('my-first-package'){
                
                def rm = command "sfdx force:mdapi:deploy --checkonly --deploydir . -u ${sfdc_org_username} --wait 1 --json --loglevel debug > deployReport.json  2>&1"
                //def rm = sh returnStdout: true, script: "sfdx force:mdapi:deploy --checkonly --deploydir . -u ${sfdc_org_username} --wait 3 --json --loglevel debug"
                sleep time: 1, unit: 'MINUTES'  //to explain
                println rm

                if (rm == 0) {
                    println ('validation is true')
                    validationStatus = true
                } else { println 'Validation is false'}


                //println ("printing rm value" + rm​)
                
                //def res = readFile "deployReport.json"
                //println ("Printing res" + res)
                //def robj = new groovy.json.JsonSlurperClassic().parseText(rm)
                def robj = jsonParse (rm)
                
                //println ("Printing " + robj)
            
                if (robj["result"]["success"])
                    {println 'validation successfull' 
                        validationStatus = true
                    }

                else { error 'validation fail '} 
            }

        }

        stage('Deploy') {

            if (validationStatus){
                dir('my-first-package'){

                    rc = command "sfdx force:mdapi:deploy --wait 1 --deploydir . -u ${sfdc_org_username} "
                    sleep time: 1, unit: 'MINUTES'
                    if (rc != 0) {
                        error 'Salesforce deploy and test run failed.'
                    } else {println 'Deploy success'}
                }
            } else { println 'Validation false, do not deploy'}
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

def commandStdOut(script) {
    if (isUnix()) {
        return sh(returnStdout: true, script: script);
    } else {
		return bat(returnStdout: true, script: script);
    }
}
