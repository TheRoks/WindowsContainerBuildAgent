# WindowsContainerBuildImage
Docker files and scripts for creating a Windows Server Core image with builds tools and Azure Pipelines build agent

Spin up a Azure Container Instance
az container create -g DevOpsPipelines -n windows2019 --os-type Windows --image theroks/azure-devops-windows-buildagent:vs2019-latest --cpu 4 --memory 7 --environment-variables AZP_URL=https://dev.azure.com/<organisation> AZP_TOKEN=<token> AZP_AGENT_NAME=vs2019-1 AZP_POOL=WindowsContainers