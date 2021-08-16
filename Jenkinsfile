properties([pipelineTriggers([githubPush()])])

pipeline {
    agent {label "slave555"}
    
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

        stage ("test") {
            steps {
                echo "this is related to testing"
            }
        }

        stage ("publish") {
            steps {
                script {
                    try {
                        rtUpload ( 
                            serverId: 'myjfrog',
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

        stage ("deploy") {
            steps {
              script{
                sh (
                  '''
                   jar_file=$(ls target/*.war)
                   ansible-playbook -i /home/devops/ansible1/tom_host /home/devops/ansible1/cd.yml --extra-vars artifact_version=$jar_file
                   '''
                    )
              }
            }
        }

    }

    post {
        always {
             /*emailext body: '''this is status of

job: "${JOB_NAME}"
url: "${BUILD_URL}"''', subject: 'Status of "{$JOB_NAME}"', to: 'tejesh2311@gmail.com'
        } */
        echo "post action"
        }
    }
}
