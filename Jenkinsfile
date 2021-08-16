properties([pipelineTriggers([githubPush()])])

pipeline {
    agent any
    
    stages {
        
        stage ("git clone project url") {
            steps {
                git url: 'https://github.com/surya150894/applogin.git'
                sh "ls -all"
            }
        }

        stage ("build") {
            steps {
                sh "mvn clean install"
            }
        }

        stage ("publish") {
            steps {
                script {
                    try {
                        rtUpload ( 
                            serverId: 'surya158',
                            spec: '''
                                  {
                                      "files": [
                                          {
                                              "pattern": "target/*.war",
                                              "target": "myapp/"
                                          }
                                      ]
                                  }
                            ''',
                            buildName: "${JOB_NAME}",
                            buildNumber: "${BUILD_NUMBER}"
                        )
                    }
                    catch(err) {
                        println "unable to push the artifact kindly check the configuration"
                        println err.getMessage()
                    }
                }
            }
        }

    }

}
