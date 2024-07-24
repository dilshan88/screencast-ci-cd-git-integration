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
				#apictl set --vcs-source-repo-path C:/ProgramData/Jenkins/.jenkins/workspace/CICD-PIPELINE-DEV

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
			sh '''#!/bin/bash
			apictl login dev -u admin -p admin -k
			apis=$(apictl vcs status -e dev --format="{{ jsonPretty . }}" | C:/jq -r '.API | .[] | .NickName')

			words=()
			for i in $apis; do words+=($i) ; done
			for word in ${words[@]}; do echo $word ; done
			
			if [ -z "$apis" ]; 
                then 
                    echo "======== No API Changes detected =========="; 
                else 
                    apiArray=($apis)
					array_length=${#apiArray[@]}
					# Print the array length
					echo "Array length: $array_length"
                    for i in "${apiArray[@]}"; do
					    echo "apictl bundle -s "$i
                        apictl bundle -s $i
                     done
                fi

			#apictl bundle -s HRIS-v1 -d upload
			#apictl import api -f C:/ProgramData/Jenkins/.jenkins/workspace/CICD-PIPELINE-DEV/upload/HRIS_v1.zip --environment dev --params DeploymentArtifacts_HRIS-v1 --update -k
			#apictl vcs deploy -e dev
			'''
		}
	 }
	
	}
}