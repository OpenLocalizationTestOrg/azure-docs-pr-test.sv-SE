---
title: "aaaCreate ett program för Azure Service Fabric-behållaren | Microsoft Docs"
description: "Skapa din första Windows-behållarapp på Azure Service Fabric.  Skapa en Docker-avbildning med en Python-program, push hello avbildningen tooa behållare registret, skapa och distribuera ett program för Service Fabric-behållare."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/18/2017
ms.author: ryanwi
ms.openlocfilehash: b79d3a41eb2da5f7791266588fe9ea7becb0e58f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-container-application-on-windows"></a>Skapa din första Service Fabric-behållarapp i Windows
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Kör ett befintligt program i en Windows-behållare på ett Service Fabric-kluster kräver inte några ändringar tooyour program. Den här artikeln vägleder dig genom att skapa en Docker-avbildning som innehåller en Python [Flask](http://flask.pocoo.org/) program och distribuera den tooa Service Fabric-klustret.  Du kan också dela programmet via [Azure Container-registret](/azure/container-registry/).  Den här artikeln förutsätter att du har grundläggande kunskaper om Docker. Du kan lära dig om Docker genom att läsa hello [Docker översikt](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Krav
En utvecklingsdator som kör:
* Visual Studio 2015 eller Visual Studio 2017.
* [Service Fabric SDK och verktyg](service-fabric-get-started.md).
*  Docker för Windows.  [Hämta Docker CE för Windows (stabil)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Efter installation och Start Docker, högerklicka på ikonen i systemfältet hello och välj **växla tooWindows behållare**. Detta är obligatorisk toorun Docker bilder baserat på Windows.

Ett Windows-kluster med tre eller fler noder som kör Windows Server 2016 med behållare – [Skapa ett kluster](service-fabric-cluster-creation-via-portal.md) eller [prova Service Fabric utan kostnad](https://aka.ms/tryservicefabric).

Ett register i Azure Container Registry – [Skapa ett behållarregister](../container-registry/container-registry-get-started-portal.md) i din Azure-prenumeration.

## <a name="define-hello-docker-container"></a>Definiera hello dockerbehållare
Skapa en avbildning baserat på hello [Python avbildningen](https://hub.docker.com/_/python/) finns på Docker-hubb.

Definiera Docker-behållaren i en Dockerfile. Hej Dockerfile innehåller instruktioner för konfigurerar hello miljön i ditt behållare, inläsning hello-program som du vill toorun och mappa portar. Hej Dockerfile är hello inkommande toohello `docker build` -kommandot, vilket skapar hello bild.

Skapa en tom katalog och skapa hello filen *Dockerfile* (med utan filtillägget). Lägg till följande hello för*Dockerfile* och spara dina ändringar:

```
# Use an official Python runtime as a base image
FROM python:2.7-windowsservercore

# Set hello working directory too/app
WORKDIR /app

# Copy hello current directory contents into hello container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available toohello world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when hello container launches
CMD ["python", "app.py"]
```

Läs hello [Dockerfile referens](https://docs.docker.com/engine/reference/builder/) för mer information.

## <a name="create-a-simple-web-application"></a>Skapa en enkel webbapp
Skapa en flask-webbapplikation som lyssnar på port 80 och returnerar ”Hello World”!.  Hej samma katalog, skapa hello-fil i *requirements.txt*.  Lägg till följande hello och spara dina ändringar:
```
Flask
```

Skapa även hello *app.py* och Lägg till hello följande:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():

    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

<a id="Build-Containers"></a>
## <a name="build-hello-image"></a>Skapa hello-bild
Kör hello `docker build` kommandot toocreate hello avbildningen som kör ditt webbprogram. Öppna ett PowerShell-fönster och navigera toohello katalog som innehåller hello Dockerfile. Kör följande kommando hello:

```
docker build -t helloworldapp .
```

Det här kommandot versioner hello ny avbildning med hjälp av hello instruktioner i din Dockerfile naming (-t taggning) hello avbildning ”helloworldapp”. Skapa en avbildning hämtar hello basavbildning från Docker-hubb och skapar en ny avbildning som lägger till ditt program ovanpå hello basavbildning.  

När hello build-kommandot har slutförts kör hello `docker images` kommandot toosee information om nya hello-avbildningen:

```
$ docker images

REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-hello-application-locally"></a>Kör hello programmet lokalt
Kontrollera avbildningen lokalt innan du skickar den hello behållaren registret.  

Kör programmet hello:

```
docker run -d --name my-web-site helloworldapp
```

*namnet* ger en namnet toohello kör behållare (i stället för hello behållar-ID).

När hello behållare startar hitta dess IP-adress så att du kan ansluta tooyour kör behållare från en webbläsare:
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

Ansluta toohello kör behållare.  Öppna en webbläsare som pekar toohello IP-adress returneras, till exempel ”http://172.31.194.61”. Du bör se hello rubriken ”Hello World”! Visa i hello webbläsare.

toostop din behållare, kör:

```
docker stop my-web-site
```

Ta bort hello behållare från utvecklingsdatorn:

```
docker rm my-web-site
```

<a id="Push-Containers"></a>
## <a name="push-hello-image-toohello-container-registry"></a>Push hello avbildningen toohello behållare registret
När du har kontrollerat hello behållaren som körs på utvecklingsdatorn push hello avbildningen tooyour registret i registret för Azure-behållare.

Kör ``docker login`` toolog i tooyour behållare registret med din [registret autentiseringsuppgifter](../container-registry/container-registry-authentication.md).

hello följande exempel skickar hello-ID och lösenord för ett Azure Active Directory [tjänstens huvudnamn](../active-directory/active-directory-application-objects.md). Du kan till exempel har tilldelat en service principal tooyour registret för ett automation-scenario. Du kan också logga in med ditt användarnamn och lösenord för registret.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

hello följande kommando skapar en tagg eller alias för hello bild med ett fullständigt kvalificerad sökväg tooyour register. Det här exemplet platser hello bilden i hello ```samples``` namnområde tooavoid oreda i hello rot hello registret.

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Push hello avbildningen tooyour behållare registret:

```
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="create-hello-containerized-service-in-visual-studio"></a>Skapa hello container service i Visual Studio
hello Service Fabric-SDK och verktyg tillhandahåller en tjänst mallen toohelp du skapar ett container program.

1. Starta Visual Studio.  Välj **Arkiv** > **Nytt** > **Projekt**.
2. Välj **Service Fabric-programmet**, ge det namnet "MyFirstContainer" och klicka på **OK**.
3. Välj **gäst behållaren** hello listan över **tjänstmallar**.
4. I **avbildningsnamn** ange ”myregistry.azurecr.io/samples/helloworldapp”, hello bild du pushas tooyour behållare databasen.
5. Namnge tjänsten och klicka på **OK**.

## <a name="configure-communication"></a>Konfigurera kommunikation
hello container service måste en slutpunkt för kommunikation. Lägg till en `Endpoint` element med hello-protokollet, porten och typen toohello ServiceManifest.xml fil. Hello av tjänsten lyssnar på port 8081 för den här artikeln.  I det här exemplet används en fast port, 8081.  Om ingen port anges väljs en slumpmässigt vald port från hello portintervall för programmet.  

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
  </Endpoints>
</Resources>
```

Genom att definiera en slutpunkt publicerar Service Fabric hello endpoint toohello Naming service.  Andra tjänster som körs i hello kluster kan lösa den här behållaren.  Du kan också utföra behållare till en annan kommunikation via hello [omvänd proxy](service-fabric-reverseproxy.md).  Kommunikation utförs genom att ange hello omvänd proxy HTTP lyssningsport och hello namnet på hello-tjänster som du vill använda toocommunicate med som miljövariabler.

## <a name="configure-and-set-environment-variables"></a>Konfigurera och ange miljövariabler
Miljövariabler kan anges för varje kodpaketet i hello service manifest. Den här funktionen är tillgänglig för alla tjänster oavsett om de har distribueras som behållare eller processer, eller körbara gäster. Du kan åsidosätta miljövariabeln värden i hello application manifest eller ange dem under distributionen som parametrar för programmet.

hello följande service manifest XML-fragment visar ett exempel på hur toospecify miljövariabler för ett paket med koden:
```xml
<CodePackage Name="Code" Version="1.0.0">
  ...
  <EnvironmentVariables>
    <EnvironmentVariable Name="HttpGatewayPort" Value=""/>    
  </EnvironmentVariables>
</CodePackage>
```

De här miljövariablerna kan åsidosättas i programmanifestet hello:

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
  </EnvironmentOverrides>
  ...
</ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping-and-container-to-container-discovery"></a>Konfigurera mappning mellan behållarport och värdport och identifiering mellan behållare
Konfigurera en toocommunicate för porten som används av värden med hello behållare. hello-portbindningen mappar hello port på vilken hello tjänsten lyssnar inuti hello behållaren tooa port på hello värden. Lägg till en `PortBinding` element i `ContainerHostPolicies` element av hello ApplicationManifest.xml fil.  Den här artikeln `ContainerPort` är 80 (hello behållaren visar port 80, som anges i hello Dockerfile) och `EndpointRef` är ”Guest1TypeEndpoint” (hello slutpunkt som tidigare definierats i hello tjänstmanifestet).  Inkommande begäranden toohello tjänst på port 8081 mappas tooport 80 hello behållaren.

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-container-registry-authentication"></a>Konfigurera autentisering av behållarregister
Konfigurera behållaren registret autentisering genom att lägga till `RepositoryCredentials` för`ContainerHostPolicies` hello ApplicationManifest.xml filen. Lägg till hello konto och lösenord för hello myregistry.azurecr.io behållare registret, vilket gör att hello service toodownload hello behållaren avbildningen från hello databasen.

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

Vi rekommenderar att du krypterar hello databasen lösenord med hjälp av en chiffrering av certifikat som har distribuerats tooall klusternoder hello. När Service Fabric distribuerar hello service paketet toohello kluster, är hello chiffrering certifikat används toodecrypt hello chiffertext.  hello Invoke-ServiceFabricEncryptText cmdlet har använt toocreate hello chiffertext för hello lösenord, som har lagts till toohello ApplicationManifest.xml fil.

hello följande skript skapar ett nytt självsignerat certifikat och exporterar den tooa PFX-filen.  hello certifikatet importeras till en befintlig nyckelvalvet och sedan distribueras toohello Service Fabric-klustret.

```powershell
# Variables.
$certpwd = ConvertTo-SecureString -String "Pa$$word321!" -Force -AsPlainText
$filepath = "C:\MyCertificates\dataenciphermentcert.pfx"
$subjectname = "dataencipherment"
$vaultname = "mykeyvault"
$certificateName = "dataenciphermentcert"
$groupname="myclustergroup"
$clustername = "mycluster"

$subscriptionId = "subscription ID"

Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Create a self signed cert, export tooPFX file.
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject $subjectname -Provider 'Microsoft Enhanced Cryptographic Provider v1.0' `
| Export-PfxCertificate -FilePath $filepath -Password $certpwd

# Import hello certificate tooan existing key vault.  hello key vault must be enabled for deployment.
$cer = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certificateName -FilePath $filepath -Password $certpwd

Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $groupname -EnabledForDeployment

# Add hello certificate tooall hello VMs in hello cluster.
Add-AzureRmServiceFabricApplicationCertificate -ResourceGroupName $groupname -Name $clustername -SecretIdentifier $cer.SecretId
```
Kryptera hello lösenord med hjälp av hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet.

```powershell
$text = "=P==/==/=8=/=+u4lyOB=+=nWzEeRfF="
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint $cer.Thumbprint -Text $text -StoreLocation Local -StoreName My
```

Ersätt hello lösenord med hello chiffertext som returneras av hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet och ange `PasswordEncrypted` för ”true”.

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-isolation-mode"></a>Konfigurera isoleringsläge
Windows stöder två isoleringslägen för behållare: process och Hyper-V. Med hello arbetsprocesser hello alla hello-behållare som körs på samma värd datorn resursen hello kernel med hello-värden. Med hello Hyper-V-isoleringsläge isoleras hello kärnor mellan varje Hyper-V-behållaren och hello behållaren värden. hello isoleringsläge har angetts i hello `ContainerHostPolicies` element i hello programmanifestfilen. hello arbetslägen som kan anges är `process`, `hyperv`, och `default`. hello standard isoleringsläge standardvärden för`process` på Windows Server är värd för, och är som standard för`hyperv` på Windows 10-värdar. hello följande utdrag visar hur hello isoleringsläge har angetts i hello programmanifestfilen.

```xml
<ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
```

## <a name="configure-resource-governance"></a>Konfigurera resursstyrning
[Resurs-styrning](service-fabric-resource-governance.md) begränsar hello resurser som hello behållare kan använda på hello värden. Hej `ResourceGovernancePolicy` element som anges i hello programmanifestet är används toodeclare gränserna för ett paket för service-kod. Resursen gränser kan anges för hello följande resurser: minne, MemorySwap, CpuShares (CPU relativa viktade), MemoryReservationInMB BlkioWeight (BlockIO relativa vikt).  I det här exemplet hämtar servicepaket Guest1Pkg en kärna på hello klusternoder där den är placerad.  Minnesgränserna är absolut, så hello kodpaketet är begränsad too1024 MB minne (och en mjuk garanti reservation av hello samma). Koden paket (behållare eller processer) är inte kan tooallocate mer minne än den här gränsen och försök toodo så resulterar i ett undantag i minnet är slut. Resursen gränsen tvingande toowork har alla kod paket i ett tjänstepaket minnesgränserna som angetts.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
  <Policies>
    <ServicePackageResourceGovernancePolicy CpuCores="1"/>
    <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
  </Policies>
</ServiceManifestImport>
```

## <a name="deploy-hello-container-application"></a>Distribuera programmet hello
Spara dina ändringar och skapa hello program. toopublish ditt program, högerklicka på **MyFirstContainer** i Solution Explorer och markera **publicera**.

I **Anslutningens slutpunkt**, ange hello hanteringsslutpunkten för hello klustret.  Till exempel "containercluster.westus2.cloudapp.azure.com:19000". Du kan hitta hello klientanslutning slutpunkt i hello översikt bladet för klustret i hello [Azure-portalen](https://portal.azure.com).

Klicka på **Publicera**.

[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) är ett webbaserat verktyg för att granska och hantera appar och noder i ett Service Fabric-kluster. Öppna en webbläsare och gå toohttp://containercluster.westus2.cloudapp.azure.com:19080 Explorer- och följ hello programdistribution.  hello programmet distribuerar men är i ett feltillstånd tills hello avbildningen hämtas på hello klusternoder (vilket kan ta en stund, beroende på hello bildstorleken): ![fel][1]

hello programmet är klart när den är i ```Ready``` tillstånd: ![redo][2]

Öppna en webbläsare och gå toohttp://containercluster.westus2.cloudapp.azure.com:8081. Du bör se hello rubriken ”Hello World”! Visa i hello webbläsare.

## <a name="clean-up"></a>Rensa
Du kan fortsätta tooincur avgifter medan hello klustret körs bör du överväga att [tar bort klustret](service-fabric-get-started-azure-cluster.md#remove-the-cluster).  [Party-kluster](http://tryazureservicefabric.westus.cloudapp.azure.com/) tas bort automatiskt efter ett par timmar.

När du trycker på hello avbildningen toohello behållare registret kan du ta bort hello lokal image på utvecklingsdatorn:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Komplett exempel på Service Fabric-app och tjänstmanifest
Här är klar hello-tjänsten och applikationsmanifest som används i denna artikel.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Guest1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is hello name of your ServiceType.
         hello UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="Guest1Type" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers tooService Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables tooyour container: -->    
    <EnvironmentVariables>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
    </EnvironmentVariables>

  </CodePackage>

  <!-- Config package is hello contents of hello Config directoy under PackageRoot that contains an
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which to
           listen. Please note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Guest1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import hello ServiceManifest from hello ServicePackage. hello ServiceManifestName and ServiceManifestVersion
       should match hello Name and Version attributes of hello ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="FrontendService.Code">
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentOverrides>
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
      </ContainerHostPolicies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- hello section below creates instances of service types, when an instance of this
         application type is created. You can also create one or more instances of service type using the
         ServiceFabric PowerShell module.

         hello attribute ServiceTypeName below must match hello name defined in hello imported ServiceManifest.xml file. -->
    <Service Name="Guest1">
      <StatelessService ServiceTypeName="Guest1Type" InstanceCount="[Guest1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="configure-time-interval-before-container-is-force-terminated"></a>Ställ in tidsintervall innan behållaren tvångsavslutas

Du kan konfigurera ett tidsintervall för hello runtime toowait innan hello behållaren tas bort när hello tjänsten tas bort (eller en flytta tooanother nod) har startats. Konfigurera hello tidsintervall skickar hello `docker stop <time in seconds>` kommandot toohello behållare.   Mer information finns i [docker stop](https://docs.docker.com/engine/reference/commandline/stop/). hello tidsintervall toowait anges under hello `Hosting` avsnitt. hello följande klustret manifestet fragment visar hur tooset hello vänta intervall:

```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "ContainerDeactivationTimeout": "10",
          ...
          }
        ]
}
```
hello standard tidsintervall har angetts too10 sekunder. Eftersom den här konfigurationen är dynamiska, en config endast uppgradera hello klustret uppdateringar hello tidsgränsen uppnåddes. 


## <a name="configure-hello-runtime-tooremove-unused-container-images"></a>Konfigurera hello runtime tooremove oanvända behållaren bilder

Du kan konfigurera hello Service Fabric-kluster tooremove oanvända behållaren bilder från hello-nod. Den här konfigurationen kan disk space toobe återfångats om för många behållare avbildningar finns på hello-nod.  tooenable den här funktionen, uppdatering hello `Hosting` avsnittet i hello klustermanifestet som visas i följande fragment hello: 


```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "PruneContainerImages": “True”,
            "ContainerImagesToSkip": "microsoft/windowsservercore|microsoft/nanoserver|…",
          ...
          }
        ]
} 
```

För bilder som inte ska tas bort, kan du ange dem under hello `ContainerImagesToSkip` parameter. 



## <a name="next-steps"></a>Nästa steg
* Mer information om hur du kör [behållare i Service Fabric](service-fabric-containers-overview.md).
* Läs hello [distribuera ett .NET-program i en behållare](service-fabric-host-app-in-a-container.md) kursen.
* Lär dig mer om hello Service Fabric [programmet livscykel](service-fabric-application-lifecycle.md).
* Utcheckning hello [kodexempel för Service Fabric-behållaren](https://github.com/Azure-Samples/service-fabric-dotnet-containers) på GitHub.

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png
