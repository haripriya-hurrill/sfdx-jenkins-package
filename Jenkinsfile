#!groovy

import groovy.json.JsonSlurperClassic

node {
    try{
        
        
        stage('Init test'){
        echo "Init test"
        echo "Test1"
        }
        
        echo "Check Out the Source Code"
        stage('Checkout'){
            checkout scm
        }
        
        echo "Wrap All Stages in a withCredentials Command"
   
        
	stage ('PackageValidation') {
	echo "entering package validation"
	}
            
     
    
    
    }
    
    
    catch(err){
        echo "build failed"
    }

}

