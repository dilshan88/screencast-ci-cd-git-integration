pipeline    {

	agent   {
		node{
			label 'built-in'
		}
	}
	environment {
		Path = "/root/apictl:$PATH"
	}
	options {
	 buildDiscarder logRotator(
		daysToKeepStr: '16',
		numToKeepStr: '10'
     )
	}
	stages  
    {
	 stage('Setup Environment for APICTL'){
		steps{
			sh '''#!/bin/bash
                #rm C:/ProgramData/Jenkins/.jenkins/workspace/gitconfig
                #touch C:/ProgramData/Jenkins/.jenkins/workspace/gitconfig
                apictl set --vcs-config-path C:/ProgramData/Jenkins/.jenkins/workspace/gitconfig
				apictl set --vcs-source-repo-path C:/ProgramData/Jenkins/.jenkins/workspace/CICD-PIPELINE-PROD

                envs=$(apictl get envs --format "{{.Name}}")
                if [ -z "$envs" ]; 
                then 
                    echo "No environment configured. Setting Prod environment.."
                    apictl add env prod --apim https://localhost:9444 
                else
                    echo "Environments :"$envs
                    if [[ $envs != *"prod"* ]]; then
                    echo "Prod environment is not configured. Setting dev environment.."
                    apictl add env prod --apim https://localhost:9444 
                    fi
                fi
                '''
		}
	 }
	 stage('Deploy APIs to Dev Environment'){
		steps{
			sh """
			apictl login prod -u admin -p admin -k
			apictl vcs deploy -e prod
			"""
		}
	 }
	
	}
}
