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
}
}


