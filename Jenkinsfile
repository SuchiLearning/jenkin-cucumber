pipeline{

    agent any

    stages {

		stage('Build') {
            steps {
                sh "mvn --batch-mode -U deploy"
                script {
                    TAG_SELECTOR = readMavenPom().getVersion()
                }
                echo("TAG_SELECTOR=${TAG_SELECTOR}")
            }
        }
        stage ('Compile Stage') {

            steps {
				withMaven(maven: 'maven_3_5_0') {
                    sh 'mvn clean install'

                }

            }
        }
    stage ('Test Stage') {

            steps {

                withMaven(maven: 'maven_3_5_0') {
                    sh 'mvn test'

                }

            }
        }


        stage ('Cucumber Reports') {

            steps {
                cucumber buildStatus: "UNSTABLE",
                    fileIncludePattern: "**/cucumber.json",
                    jsonReportDirectory: 'target'

            }

        }

    }

}