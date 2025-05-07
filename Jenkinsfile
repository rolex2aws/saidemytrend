def registry = 'https://triald77uza.jfrog.io'
pipeline
{
agent any
environment
{
PATH = "/opt/maven/bin:$PATH"
}
stages
{
stage ('build')
{
steps
{
echo "---------------start build--------------------"
sh 'mvn clean deploy -Dmaven.test.skip=true'
echo "---------------end build----------------------"
}
}
stage ('sonarqube analysis')
{
environment
{
scannerHome = tool 'sonar-scanner'
}
steps
{
withSonarQubeEnv ('sonarqube-server')
{
sh "${scannerHome}/bin/sonar-scanner"
}
}
}

stage("Jar Publish") {                // 14  // Creates a stage named 'Jar Publish'
            steps {                           // 15  // Defines the steps that will be executed in this stage
                script {                      // 16  // Allows running custom Groovy script inside the pipeline
                    echo '<--------------- Jar Publish Started --------------->'  
                                              // Logs a message indicating the start of JAR publishing
                    def server = Artifactory.newServer url: registry + "/artifactory", credentialsId: "jfrog-credentials"  
                                              // Defines the Artifactory server with the specified URL and credentials
                    def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"  
                                              // Sets properties like build ID and Git commit ID for the build
                    def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "rolex2aws-libs-release-local/{1}",
                              "flat": "false",
                              "props": "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""  
                                              // Defines the upload specification for uploading JAR files to Artifactory
                    def buildInfo = server.upload(uploadSpec)  
                                              // Uploads the 
        }
               
}
}
}
}

