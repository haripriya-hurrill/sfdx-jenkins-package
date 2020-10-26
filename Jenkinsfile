#!groovy
import groovy.json.JsonSlurperClassic

node {
    try{
        
        
        def toolbelt = tool 'toolbelt'

        stage('Init test'){
        echo "Init test"
        }

        echo "Check Out the Source Code"
        stage('checkout source') {
            // when running in multi-branch job, one must issue this command
            checkout scm
        }
       
        stage('Authorise to Saleforce') {
            echo "Wrap All Stages in a withCredentials Command"    
        }
                    
        stage('Deploy and Run Tests') {
            echo "Wrap All Stages in a withCredentials Command"
        }
            

    } 

    catch(err){
        echo "build failed"
    }    
}
