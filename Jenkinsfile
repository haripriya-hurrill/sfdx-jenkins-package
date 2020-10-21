#!groovy

import groovy.json.JsonSlurperClassic

node {
    try{
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

