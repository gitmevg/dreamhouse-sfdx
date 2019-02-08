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

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Deploye Code') {
            if (isUnix()) {
                rc = sh returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }else{
                 rc = bat returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }
            if (rc != 0) { error 'hub org authorization failed' }

			println rc
            
            //Create Folder Named: manifest
            //def folder = new File( 'manifest' )
            
            //if( !folder.exists() ) {
            //Create all folders up-to and including B
            //folder.mkdirs()
            //}
            if (isUnix()) {
				rcr = sh returnStdout: true, script: "mkdir -p ${PACK_DIR}"
			}else{
			   rcr = bat returnStdout: true, script: "mkdir ${PACK_DIR}"
            //   sleep(time:30,unit:"SECONDS") 
            //   rmsg = rmsg = bat returnStdout: true, script: "sfdx force:source:convert -d ${PACK_DIR}"  
               rcon = bat returnStatus: true, script: "sfdx force:source:convert -d ${PACK_DIR}" 
			}
            print rcon
            
            //convert source
           // if (isUnix()) {
				//rmsg = sh returnStdout: true, script: "${toolbelt} force:source:convert -d ${PACK_DIR} --rootdir ${WORKSPACE}"
			//}else{
               
			    //rmsg = rmsg = bat returnStdout: true, script: "sfdx force:source:convert -d ${PACK_DIR}" 
			   //rmsg = bat returnStdout: true, script: "sfdx force:mdapi:deploy -d ${PACK_DIR} -u ${HUB_ORG}"
			//}
            
            //sleep time for Jenkins
             //sleep(time:120,unit:"SECONDS")
			// need to pull out assigned username
            if (rcon!=0) {
                print "Check it"
                print "Job Failed"
                error("Build failed because of $rcon")
            }
            else{
			if (isUnix()) {
				rmsg = sh returnStdout: true, script: "${toolbelt} force:mdapi:deploy -d ${PACK_DIR} -u ${HUB_ORG}"
			}else{
                print "Job Successful1"
			    //rmsg = bat returnStdout: true, script: "sfdx force:mdapi:deploy -d ${PACK_DIR} -u ${HUB_ORG}"
			    rmsg = bat returnStdout: true, script: "sfdx force:mdapi:deploy -d ${PACK_DIR} -u ${HUB_ORG}"
                printf rmsg
                print "Job Successful2"
                
			}
            }
			  
           
            //println('Hello from a Job DSL script!')
            //println(rmsg)
        }
    }
}
