#!groovy
pipeline {
   agent any
   
   parameters {
        choice(name: 'NAME_SPACE', choices: ['dev', 'ppe', 'prod'], description: 'Environment to deploy the service')
        booleanParam(defaultValue: true, description: 'Run Sonarqube ?', name: 'sonarqube')

    }

   stages {
      echo "Running ${env.BUILD_ID} ${env.BUILD_DISPLAY_NAME} on ${env.NODE_NAME} and JOB ${env.JOB_NAME}"
      stage('Build Trigger') {
         // When the condition matches, only run the "Steps"
         when {
            // When the branch name matches any of the following conditions
                anyOf {
                    branch 'master'; branch "XYZ-*";
                }
            }
         stages {
                stage('Checkout') {
                    steps {
                        echo 'Checkout...'
                        git(url: 'https://github.com/sanjeevpr/hello-world-spring-boot/',
                                branch: env.BRANCH_NAME,
                                changelog: true
                        )
                    }
                
                stage('Build') {
                    steps {
                        echo 'Building...'
                        echo "Running ${env.BUILD_ID} ${env.BUILD_DISPLAY_NAME} on ${env.NODE_NAME} and JOB ${env.JOB_NAME}"
                        script {
                            notifyBuild('STARTED')
                            sh "mvn clean install"
                        }
                    }
                }
         }
      }
  }
}

// Function to notify build status
def notifyBuild(String buildStatus = 'STARTED') {
    // build status of null means successful
    buildStatus = buildStatus ?: 'SUCCESSFUL'

    // Default values
    def colorName = 'RED'
    def colorCode = '#FF0000'
    def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
    def summary = "${subject} (${env.BUILD_URL})"
    def details = """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
    <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME}
 [${env.BUILD_NUMBER}]</a>"</p>"""

    // Override default values based on build status
    if (buildStatus == 'STARTED') {
        color = 'YELLOW'
        colorCode = '#FFFF00'
    } else if (buildStatus == 'SUCCESSFUL') {
        color = 'GREEN'
        colorCode = '#00FF00'
    } else {
        color = 'RED'
        colorCode = '#FF0000'
    }
}
