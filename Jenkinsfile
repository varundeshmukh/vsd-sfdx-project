#!groovy
import groovy.json.JsonSlurperClassic
node{
    def HUB_ORG=env.HUB_ORG_OH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID=env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY

    println 'Test Build 02-14-2020'
    println HUB_ORG
    println SFDC_HOST
    println JWT_KEY_CRED_ID
    println CONNECTED_APP_CONSUMER_KEY

    stage('checkout source'){
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]){
        stage('Deploy Code'){
            println('VSD withCredentials START')
            if(isUnix()){
                rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }
            else{
                rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }
            if(rc != 0){ error 'Salesforce org authorization failed.'}
            println rc
            if(isUnix()){
                rmsg = sh returnStdout: true, script: "${toolbelt} force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
            }
            else{
                rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
            }
            printf rmsg
            println('VSD withCredentials END')
            println(rmsg)
        }
    }
}