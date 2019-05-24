#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def PACK_DIR="MyPackage${BUILD_NUMBER}"
    def SFDC_USERNAME
    def workspace = env.WORKSPACE

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    //def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }
   // if (isUnix()) {
     //            rc = sh returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
       //     }else{
         //        rc = bat returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
           // }
    
    //stage('Sonarqube') {
    //environment {
       // scannerHome = tool 'LocalSonarScanner'
    //}
    //steps {
      //  withSonarQubeEnv('sonarqube') {
            // sh "${scannerHome}/bin/sonar-scanner"
        //    ss = bat returnStdout: true, script: "%scannerHome%/bin/sonar-scanner.bat"
        //}
        //timeout(time: 10, unit: 'MINUTES') {
          //  waitForQualityGate abortPipeline: true
       // }
       // print ss
    //}
//}
        stage('SonarQube analysis') {
    // requires SonarQube Scanner 2.8+
    def scannerHome = tool 'LocalSonarScanner';
    withSonarQubeEnv('SQ_CI_Test') {
      bat "${scannerHome}/bin/sonar-scanner.bat"
    }
  }
}
