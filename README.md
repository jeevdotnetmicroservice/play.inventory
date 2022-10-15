# Play.Inventory
Play Economy Play.Inventory microservice.

## Create and publish package
```powershell
$version="1.0.5"
$packageversion="1.0.4"
$owner="jeevdotnetmicroservice"
$gh_pat="[PAT HERE]"

dotnet pack src\Play.Inventory.Contracts\ --configuration Release -p:PackageVersion=$packageversion -p:RepositoryUrl=https://github.com/$owner/Play.Inventory -o ..\packages

dotnet nuget push ..\packages\Play.Inventory.Contracts.$packageversion.nupkg --api-key $gh_pat --source "github"
```

## Build the docker image
```powershell
$env:GH_OWNER="jeevdotnetmicroservice"
$env:GH_PAT="[PAT HERE]"
docker build --secret id=GH_OWNER --secret id=GH_PAT -t play.inventory:$version .
```

## Run the docker image
```powershell
$cosmosDbConnString="[CONN STRING HERE]"
$serviceBusConnString="[CONN STRING HERE]"

docker run -it --rm -p 5004:5004 --name inventory -e MongoDbSettings__ConnectionString=$cosmosDbConnString -e  ServiceBusSettings__ConnectionString=$serviceBusConnString -e ServiceSettings__MessageBroker="SERVICEBUS" play.inventory:$version
```