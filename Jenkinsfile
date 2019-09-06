pipeline {
    agent any
    stages {
        stage('One') {
            input {
                message "Should we send a mail?"
                ok "Yes"
                parameters {
                    string(defaultValue: '', description: 'The name to send an email to ', name: 'Name', trim: false)
                }
            }
            steps {
                echo 'Loading external file'
                script{
                    def echoer = load "./jenkins/echoer.groovy"
                    echoer.echoIt('Hello World')
                }
                echo 'Loaded File and Called'
                 echo "Sending mail to ${Name}"
                sh "echo Hello from the shell"
                sh "hostname"
                sh "uptime"
            }
        }
        stage('Two') {
             steps {
                script {
                    def disk_size = sh(script: "df / --output=avail | tail -1", returnStdout: true).trim() as Integer
                    println("disk_size = ${disk_size}")
                }
            }
        }
    }
}
def sendEmail(status) {
    mail(
            to: "$EMAIL_RECIPIENTS",
            subject: "Build $BUILD_NUMBER - " + status + " (${currentBuild.fullDisplayName})",
            body: "Changes:\n " + getChangeString() + "\n\n Check console output at: $BUILD_URL/console" + "\n")
}