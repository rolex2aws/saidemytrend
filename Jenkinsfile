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
sh 'mvn clean deply'
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
with SonarQubeEnv ('sonarqube-server')
{
sh "${scannerHome}"/bin/sonar-scanner"
}
}
}
}
}


