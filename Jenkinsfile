pipeline
{
  agent any 
  tools
  {
    maven 'maven'
  }
  stages
  {
	stage ('Check-Git-Secrets')
	{
		steps
		{
			sh 'docker run -t dxa4481/trufflehog --json https://github.com/securitytest3r/webapp.git > trufflehog_output.json'
			sh 'cat trufflehog_output.json'
		}
	}
  
    stage ('Initialize')
	{
      steps
	  {
        sh 'echo "PATH = ${PATH}"'
      }
    }
    
    stage ('Build')
	{
      steps
	  {
        sh 'mvn clean package'
      }
    }
	
    stage ('Deploy-To-Tomcat')
    {
        steps
        {
            sshagent(['tomcat'])
            {
                sh 'scp -o StrictHostKeyChecking=no target/*.war tomcat@192.168.206.133:/var/lib/tomcat9/webapps/webapp.war'
            }      
        }       
    }
  }
}
