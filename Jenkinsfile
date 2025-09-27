pipeline{

      agent any
      environment{
          PATH = "/opt/maven/bin:$PATH"
      }


      stages{

            stage("build"){

                  steps{
                         sh "mvn clean package"
                  }
            }


            stage("sonarqube check"){
                  
                  environment{
                        scannerHome = tool 'my-sonar-scanner'
                  }


                  steps{
                      
                      withSonarQubeEnv('my-sonar-server') {
                                 sh "${scannerHome}/bin/sonar-scanner"
                      }
                  }

            }



            stage("Jar Publish") {
              steps {
                script {
                    echo '<--------------- Jar Publish Started --------------->'
                    def server = Artifactory.newServer url: registry + "/artifactory", credentialsId: "jfrog-cred"
                    def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"
                    def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "vaibhav-libs-release-local/{1}",
                              "flat": "false",
                              "props": "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                    def buildInfo = server.upload(uploadSpec)
                    buildInfo.env.collect()
                    server.publishBuildInfo(buildInfo)
                    echo '<--------------- Jar Publish Ended --------------->'
                  }
              }
           }

      }


}
