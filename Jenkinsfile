#!groovy

import groovy.json.JsonSlurperClassic

node {
    try{
            echo "Test1"
            stage('Init test'){
            echo "Init test"
        }
        stage('Checkout'){

         checkout scm
        }
    }
    catch(err){
        echo "build failed"
    }

}

