pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies...'
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                bat 'python -m unittest discover'
            }
        }
    }

    post {
        always {
            script {
                def buildStatus = currentBuild.result
                def jobName = env.JOB_NAME
                def buildNumber = env.BUILD_NUMBER
                def buildUrl = env.BUILD_URL

                def emailSubject = "[Jenkins] ${jobName} - Build #${buildNumber} - ${buildStatus}"
                def emailBody = """
                    <p>Build ${buildStatus} for job '${jobName}' - Build #${buildNumber}.</p>
                    <p>Repository: ${env.GIT_URL}</p>
                    <p>Branch: ${env.GIT_BRANCH}</p>
                    <p>Check console output at: <a href='${buildUrl}'>${buildUrl}</a></p>
                    ${buildStatus == 'SUCCESS' ? '<p>Build completed successfully!</p>' : ''}
                    ${buildStatus == 'FAILURE' ? '<p>Please investigate the build failure.</p>' : ''}
                """

                echo "Sending email notification..."

                emailext (
                    to: "sare@osidigital.com",
                    subject: "${emailSubject}",
                    body: "${emailBody}",
                    attachLog: true
                )
            }
        }
    }
}
