#!groovy

import groovy.json.JsonSlurperClassic

node {
    try{
    stage('Init test'){
    echo "Init test"
    }
    stage('Checkout'){
    sh 'git config --global credential.helper store'
    sh 'git config --global push.default simple'

     checkout scm
    }
    }
    catch(err){
    echo "build failed"
    }

}

