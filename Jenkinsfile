pipeline{

      agent any
      environment{
          PATH = "/opt/maven/bin:$PATH"
      }


      stages{

            stage("build"){

                  steps{
                         sh "maven clean package"
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

      }


}
