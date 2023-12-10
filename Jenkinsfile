// pipeline {
//      agent any
//      triggers { cron('* 3 * * *') }
//      options { timeout(time: 5) }
//      parameters {
//         booleanParam(name: 'DEBUG_BUILD', defaultValue: true,
//         description: 'Is it the debug build?')
//      }
//      stages {
//           stage("Compile") {
//                steps {
//                     sh "./gradlew compileJava"
//                }
//           }
//           stage("Unit test") {
//                steps {
//                     sh "./gradlew test"
//                }
//           }
//           stage("Code coverage") {
//                steps {
//                     sh "./gradlew jacocoTestReport"
//                     publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'build/reports/jacoco/test/html', reportFiles: 'index.html', reportName: 'Code Coverage Report'])
//                     sh "./gradlew jacocoTestCoverageVerification"
//                }
//           }
//           stage("Static code analysis") {
//                steps {
//                     sh "./gradlew checkstyleMain"
//                }
//           }
//      }
// }

podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: java
        image: openjdk:17
        command:
        - sleep
        args:
        - 99d
      - name: gradle
        image: gradle:7.2.0-jdk17
        command:
        - sleep
        args:
        - 99d
''') {
    node {
        stage('Checkout') {
             steps {
                container('gradle') {
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'refs/heads/main']],
                        userRemoteConfigs: [[url: 'https://github.com/richinex/calculator-api.git']]
                    ])
                }
            }
        }
        stage('Compile') {
            steps {
                container('gradle') {
                    sh './gradlew compileJava'
                }
            }
        }
        stage('Unit test') {
            steps {
                container('gradle') {
                    sh './gradlew test'
                }
            }
        }
        stage('Code coverage') {
            steps {
                container('gradle') {
                    sh './gradlew jacocoTestReport'
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'build/reports/jacoco/test/html', reportFiles: 'index.html', reportName: 'Code Coverage Report'])
                    sh './gradlew jacocoTestCoverageVerification'
                }
            }
        }
        stage('Static code analysis') {
            steps {
                container('gradle') {
                    sh './gradlew checkstyleMain'
                }
            }
        }
    }
}
