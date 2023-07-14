pipeline {
    agent {
        node {
            label 'L5JENKSLVWD1 || L5JENKSLVWD2'
        }
    }
    
    stages {
		
		 stage('Checkout') {
            steps {
                // Checkout the repository from GitHub
		    checkout scm
                // git branch: "master",
                 //changelog: false,
                 //credentialsId: 'svc_jenkins',
                 //poll: false,
                 //url: 'https://github.ent.stateauto.com/StateAuto/QE-SA-API-PostmanScripts.git'
            }
        }

	
		stage('Install Newman if not present') {
            steps {
                script {
                    def newmanInstalled = bat(returnStatus: true, script: 'newman --version').exitStatus == 0
                    if (!newmanInstalled) {
                        bat 'npm install -g newman'
						bat 'npm install -g newman-reporter-htmlextra'
                    }
                }
            }
        }
		
		stage('Execute Postman Collections') {
            steps {
			    script {
                // Define the collections to execute
                def collections = [
                    "PostmanScripts/Address Scrub v2.postman_collection.json",
                    "PostmanScripts/Credit copy.postman_collection.json",
                    "PostmanScripts/ZipLookup_SpringBoot.postman_collection.json"
                ]
                
                // Loop through the collections and execute Newman
                for (def collection in collections) {
                    bat "newman run ${collection} -r html --reporter-html-template ./results/finalreport.html"
                }
			  }	
            }
        }
		
		stage('Modify Newman Report') {
            steps {
                             
               // Modify the HTML template file
                bat "sed -i 's/newman run report/My Custom Newman Run Report/g' ./results/finalreport.html"
            }
        }
		
       
    }
}
