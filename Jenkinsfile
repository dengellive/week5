podTemplate(containers: [
    containerTemplate(
        name: 'gradle',
        image: 'gradle:6.3-jdk14',
        command: 'sleep',
        args: '30d'
        ),
    ]) {
    node(POD_LABEL) {
        stage('Run pipeline against a gradle project') {
            git 'https://git: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@github.com/dengellive/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git'
            container('gradle') {
                stage('Build a gradle project') {
                    sh '''
                    cd Chapter08/sample1
                    chmod +x gradlew
                    ./gradlew test
                    '''
                    }
                stage("Code coverage") {
                    sh '''
                    pwd
                    cd Chapter08/sample1
                    ./gradlew jacocoTestCoverageVerification
                    ./gradlew jacocoTestReport
                    '''
                    publishHTML (target: [
                        reportDir: 'Chapter08/sample1/build/reports/jacoco/test/html',
                        reportFiles: 'index.html',
                        reportName: "JaCoCo Report"
                    ])
                }
		stage("jacoco checkstyle") {
		    try {
			sh '''
        	        pwd
                	cd Chapter08/sample1
		    	./gradlew checkstyleMain
                    	./gradlew jacocoTestReport
                    	'''
		    } 
		    catch (err) {
			echo "checkstyle failed"
		    }
                    publishHTML (target: [
                        reportDir: 'Chapter08/sample1/build/reports/jacoco/test/html',
                        reportFiles: 'checkstyle.html',
                        reportName: "JaCoCo CheckStyle"
                    ])
                }
            }
        }
    }
}
