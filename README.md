# .NET on Kubernetes

prerequisite

oc project openshift
oc create -f https://raw.githubusercontent.com/redhat-developer/s2i-dotnetcore/master/dotnet_imagestreams.json

or 

oc replace -f https://raw.githubusercontent.com/redhat-developer/s2i-dotnetcore/master/dotnet_imagestreams.json

check .net core 5 ready to use
oc describe is dotnet

deploy .net from developer console

developer console
new project: dotnetdemo1
deploy from git
git url: https://github.com/redhat-developer/s2i-dotnetcore-ex
branche: dotnet-5.0
context-dir: /app
switch and show builder image 5 to 3 and back to 5
create route

view build, pod, resource, route


deploy from Dockerfile
create project dotnetdocker

git url: https://github.com/chatapazar/qotd-csharp
Dockerfile: 
FROM registry.access.redhat.com/ubi8/dotnet-31:3.1
USER 1001
RUN mkdir qotd-csharp
WORKDIR qotd-csharp
ADD . .

RUN dotnet publish -c Release

EXPOSE 8080

CMD ["dotnet", "./bin/Release/netcoreapp3.0/publish/qotd-csharp.dll"]


deploy .net from oc command with s2i

# Create a new OpenShift project
$ oc new-project dotnetdemo2

# Add the .NET Core application
$ oc new-app dotnet:5.0-ubi8~https://github.com/redhat-developer/s2i-dotnetcore-ex#dotnet-5.0 --context-dir app

# Make the .NET Core application accessible externally and show the url
$ oc expose service s2i-dotnetcore-ex
$ oc get deployment,pods,svc,route

Optional: deploy with odo

# Use git to check out the .NET Core application
$ git clone https://github.com/redhat-developer/s2i-dotnetcore-ex
$ cd s2i-dotnetcore-ex/app
$ git checkout dotnet-5.0

# Create a new OpenShift project
$ odo project create mydemo

# Add a component for the .NET Core application
$ odo create dotnet:5.0-ubi8

# Make the .NET Core application accessible externally
$ odo url create

# Deploy the application
$ odo push

tekton

show demo git url: https://github.com/redhat-developer/s2i-dotnetcore-ex/tree/dotnetcore-3.1-openshift-manual-pipeline
install OpenShift Pipelines Operator

go to s2i-dotnetcore-ex
git checkout dotnetcore-3.1-openshift-manual-pipeline

$ oc new-project dotnet-pipeline-app
create task,pipeline,resource
$ oc apply -f ci/simple/simple-pipeline.yaml

show pipelines in dotnet-pipeline-app

create pipelinerun

$ oc create -f ci/simple/run-simple-pipeline.yaml

show pipelinerun, view log, show build and test


.NET Framework
