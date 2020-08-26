#!groovy

node {

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH
	//def CONNECTED_APP_CONSUMER_KEY='3MVG9KsVczVNcM8zoxX.NRcO8pAPjpasK_GKirThC3oYqGE3iTX4cGkX_TZVJtoVlNXvUTfx3Ir_hD3mb8kv1'
	def TEST_LEVEL='RunLocalTests'
	//def TEST_LEVEL= 'RunSpecifiedTests'
	//def TEST_CLASSES = "AccountControllerTest,ContactControllerTest"
	//def TEST_CLASSES = "AccountControllerTest"
	//def HUB_ORG = 'rsailaja@mazdausa.com.mazdadev'

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY

    //def SF_INSTANCE_URL = env.SF_INSTANCE_URL ?: "https://test.salesforce.com"


    def toolbelt = tool 'toolbelt'


    // -------------------------------------------------------------------------
    // Check out code from source control.
    // -------------------------------------------------------------------------

    stage('checkout source') {
        checkout scm
    }


    // -------------------------------------------------------------------------
    // Run all the enclosed stages with access to the Salesforce
    // JWT key credentials.
    // -------------------------------------------------------------------------

 	withEnv(["HOME=${env.WORKSPACE}"]) {	
	
	    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
		// -------------------------------------------------------------------------
		// Authenticate to Salesforce using the server key.
		// -------------------------------------------------------------------------

		stage('Authorize to Salesforce') {
			//rc = command "\"${toolbelt}\" force:auth:logout --targetusername ${HUB_ORG} -p & \"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --instanceurl ${SFDC_HOST} --setdefaultusername"
			//rc = command "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --instanceurl ${SFDC_HOST} --setdefaultusername"
			rc = command "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --instanceurl ${SFDC_HOST} --setdefaultdevhubusername"
			if (rc != 0) { error 'Salesforce org authorization failed.'}
                println rc
		}
		
		    
		//-------------------------------------------------------------------------
		// Deploy metadata and execute unit tests for metadata package zip file
		// -------------------------------------------------------------------------
		
		//stage('Deploy and Run Tests'){
			//rc = command "\"${toolbelt}\" force:auth:logout --targetusername ${HUB_ORG} -p & \"${toolbelt}\" force:mdapi:deploy -f release_1.zip -u ${HUB_ORG} -l ${TEST_LEVEL}"
			rc = command "\"${toolbelt}\" force:source:deploy  -p force-app/. -u ${HUB_ORG} -l ${TEST_LEVEL}"
		    //RunSpecifiedTests --runtests TestLanguageCourseTrigger"
		//}


		//-------------------------------------------------------------------------
		// Deploy metadata and execute unit tests.
		// -------------------------------------------------------------------------

		//stage('Deploy and Run Tests') {
			//rc = command "\"${toolbelt}\" force:source:deploy  -x manifest/package.xml -u ${HUB_ORG} -l ${TEST_LEVEL}"
			//rc = command "\"${toolbelt}\" force:source:deploy  -p force-app/. -u ${HUB_ORG} -l ${TEST_LEVEL}"
			//rc = command "\"${toolbelt}\" force:source:deploy  -p force-app/. -u ${HUB_ORG} -l ${TEST_LEVEL} -r ${TEST_CLASSES}"
		    //if (rc != 0) {
			//error 'Salesforce deploy and test run failed.'
		    //}
		//}


	
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
