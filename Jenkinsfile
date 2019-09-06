pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git(
                       url: 'https://github.com/jerrykurian/testrepo.git',
                       credentialsId: 'GitJerry',
                       branch: "master"
                    )
                 sh 'mvn install'
            }
        }
        stage('One') {
            steps {
                echo 'Loading external file'
                script{
                    def echoer = load "./jenkins/echoer.groovy"
                    echoer.echoIt('Hello World')
                }
                echo 'Loaded File and Called'
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
