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
                 script {
                     def mvnHome = tool 'Maven 3.5.2'
                     def targetVersion = getDevVersion()
                     print 'target build version...'
                     print targetVersion
                     sh "'${mvnHome}/bin/mvn' -Dintegration-tests.skip=true -Dbuild.number=${targetVersion} clean package"
                     def pom = readMavenPom file: 'pom.xml'
                     // get the current development version
                     developmentArtifactVersion = "${pom.version}-${targetVersion}"
                     print pom.version
                     // execute the unit testing and collect the reports
                     junit '**//*target/surefire-reports/TEST-*.xml'
                     archive 'target*//*.jar'
                 }
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
