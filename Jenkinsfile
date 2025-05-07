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

           stage('Upload to Artifactory') {
            steps {
                // Requires Artifactory plugin to be installed in Jenkins
                withArtifactory(serverId: 'jfrog-rolex2aws') { // Replace with your server ID
                    rtUpload(
                        spec: '''
                        {
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",  // Or your artifact pattern
                              "target": "rolex2aws-libs-release-local/{1}"  // Your Artifactory path
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                          ]
                        }
                        '''
                    )
                }
            }
        }

        stage('Publish Build Info') {
            steps {
                withArtifactory(serverId: 'jfrog-rolex2aws') {
                  rtPublishBuildInfo()
                }
            }
        }                  
}
}

