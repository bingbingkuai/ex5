// THis is a copy of Jenkins file
// This will be used in wk6 ex2, multi pipeline, pull request

// This example uses Jenkin's "scripted" syntax, as opposed to its "declarative" syntax
// see: https://www.jenkins.io/doc/book/pipeline/syntax/#scripted-pipeline
// Defines a Kubernetes pod template that can be used to create nodes.

podTemplate(
	containers: [
    		containerTemplate(
        	    name: 'gradle', 
                    image: 'gradle', 
                    command: 'sleep', 
                    args: '30d'
        	),
    	],
	podRetention: onFailure()
) {
    node(POD_LABEL) {
        // "container" Selects a container of the agent pod so that all shell steps are executed in that container.
        container('gradle') {
            stage {

                stage('Build a gradle project') {
                    steps {
                        // from the git plugin
                        git 'https://github.com/bingbingkuai/chp5.git'
                        sh '''
                        cd Chapter08/sample1
                        chmod +x gradlew
                        '''
                    }
                }


                stage('Code Coverage Test (main)') {
                    when {
                        // Run only on the "main" branch
                        branch 'main'
                    }
                    steps {
                        try {
                            echo 'This is on main branch, we need to do Code Coverage Test on the main branch'
                            sh ''' pwd
                            cd Chapter08/sample1
                            ./gradlew test
                            ./gradlew jacocoTestCoverageVerification
                            '''
                            } catch (Exception E) { 
                                echo 'Failure detected'
                            }
                    }
                }

                stage('Checkstyle (branch)') {
                    when {
                        // Run only if not on main branch
                         branch 'br2' 
                    }
                    steps {
                        try {
                            echo 'Not on the main branch, do a Checkstyle Test when not on the main branch'
                            sh '''
                                pwd
                                cd Chapter08/sample1
                                ./gradlew checkstyleMain
                                '''
                        } catch (Exception E) {
                                echo 'Failure detected'
                        }
                    }
                }
            }
        }        

    }
}
