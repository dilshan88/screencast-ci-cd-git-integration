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
				apictl set --vcs-source-repo-path C:/ProgramData/Jenkins/.jenkins/workspace/CICD-PIPELINE-DEV

                envs=$(apictl get envs --format "{{.Name}}")
                if [ -z "$envs" ]; 
                then 
                    echo "No environment configured. Setting dev environment.."
                    apictl add env dev --apim https://localhost:9443 
                else
                    echo "Environments :"$envs
                    if [[ $envs != *"dev"* ]]; then
                    echo "Dev environment is not configured. Setting dev environment.."
                    apictl add env dev --apim https://localhost:9443 
                    fi
                fi
                '''
		}
	 }
	 stage('Deploy APIs to Dev Environment'){
		steps{
			sh """
			#apictl login dev -u admin -p admin -k
			#apictl gen deployment-dir -s C:/ProgramData/Jenkins/.jenkins/workspace/CICD-PIPELINE-DEV/HRIS-v1
			#apictl vcs deploy -e dev
			# derive param content name 
					fileName=$(echo HRIS-v1 | sed 's/\\(.*\\).zip/\\1 /')
					deploymentName=$(echo $fileName | sed 's/\\(.*\\)_/\\1-/')
					paramPath="DeploymentArtifacts_"$deploymentName
					echo "Param path :"$paramPath
					# login to the dev environment
					apictl login dev -u admin -p admin -k
					# import the artifact
					message=$(apictl import api -f $name --params $paramPath -e dev --update -k)
					if [ "$message" = "Successfully imported API." ]; then
						echo "Successfully imported API."
					else
						echo $message
					fi
			"""
		}
	 }
	
	}
}