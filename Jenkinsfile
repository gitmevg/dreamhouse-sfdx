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

        stage('GIT Clone') {
            // when running in multi-branch job, one must issue this command
            checkout scm
        }

        stage('SonarQube Analysis') {
            // requires SonarQube Scanner 2.8+
            def scannerHome = tool 'LocalSonarScanner';
            withSonarQubeEnv('SQ_CI_Test') {
            bat "${scannerHome}/bin/sonar-scanner.bat"
            }
        }
    
        stage("SonarQube Quality Gate Results"){
            timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
            def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                if (qg.status != 'OK') {
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
                else {
                    print "Result Status: ${qg.status}"   
                }
                print "Checking if looped is skipped, if printed then not skipped"
            }
        }
}
