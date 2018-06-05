#!/usr/bin/env groovy
node() {

		//Dowloading files
		stage("Checkout") {		
			 // Checkout hummingbird-main
			 dir('hummingbird-main') {
				 checkout scm

			 }
		}

		// build and package the artifact
		stage("Build Service") {
			 // ensure the workspace is clean
			 sh "rm -rf target"

			 dir('hummingbird-main') {
				 //for jar in lib folder
				 sh "./build.bat"
			 }
        }

       // run the unit tests
        stage("Unit Tests") {
           dir('hummingbird-main') {
               sh "mvn test -Dmaven.test.failure.ignore=true"
               step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
            }
        }

        stage("Package") {
			dir('hummingbird-main') {
               stash name: 'artifacts', includes: '.git/**,Dockerfile,**/target/**', useDefaultExcludes: false
            }
		}

}
