---
title: "aaaCreate ett program med Azure Service Fabric behållare på Linux | Microsoft Docs"
description: "Skapa din första Linux-behållarapp på Azure Service Fabric.  Skapar en Docker-avbildning med ditt program, push hello avbildningen tooa behållare registret, skapa och distribuera ett program för Service Fabric-behållare."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: ryanwi
ms.openlocfilehash: 348dbcbaa1a534fb2808450e4c2d5f9acc7c7b35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-container-application-on-linux"></a>Skapa din första Service Fabric-behållarapp i Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Kör ett befintligt program i en Linux-behållare på ett Service Fabric-kluster kräver inte några ändringar tooyour program. Den här artikeln vägleder dig genom att skapa en Docker-avbildning som innehåller en Python [Flask](http://flask.pocoo.org/) program och distribuera den tooa Service Fabric-klustret.  Du kan också dela programmet via [Azure Container-registret](/azure/container-registry/).  Den här artikeln förutsätter att du har grundläggande kunskaper om Docker. Du kan lära dig om Docker genom att läsa hello [Docker översikt](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Krav
* En utvecklingsdator som kör:
  * [Service Fabric SDK och verktyg](service-fabric-get-started-linux.md).
  * [Docker CE för Linux](https://docs.docker.com/engine/installation/#prior-releases). 
  * [Service Fabric CLI](service-fabric-cli.md)

* Ett register i Azure Container Registry – [Skapa ett behållarregister](../container-registry/container-registry-get-started-portal.md) i din Azure-prenumeration. 

## <a name="define-hello-docker-container"></a>Definiera hello dockerbehållare
Skapa en avbildning baserat på hello [Python avbildningen](https://hub.docker.com/_/python/) finns på Docker-hubb. 

Definiera Docker-behållaren i en Dockerfile. Hej Dockerfile innehåller instruktioner för konfigurerar hello miljön i ditt behållare, inläsning hello-program som du vill toorun och mappa portar. Hej Dockerfile är hello inkommande toohello `docker build` -kommandot, vilket skapar hello bild. 

Skapa en tom katalog och skapa hello filen *Dockerfile* (med utan filtillägget). Lägg till följande hello för*Dockerfile* och spara dina ändringar:

```
# Use an official Python runtime as a base image
FROM python:2.7-slim

# Set hello working directory too/app
WORKDIR /app

# Copy hello current directory contents into hello container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available outside this container
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

## <a name="build-hello-image"></a>Skapa hello-bild
Kör hello `docker build` kommandot toocreate hello avbildningen som kör ditt webbprogram. Öppna ett PowerShell-fönster och navigera för*c:\temp\helloworldapp*. Kör följande kommando hello:

```bash
docker build -t helloworldapp .
```

Det här kommandot versioner hello ny avbildning med hjälp av hello instruktioner i din Dockerfile naming (-t taggning) hello avbildning ”helloworldapp”. Skapa en avbildning hämtar hello basavbildning från Docker-hubb och skapar en ny avbildning som lägger till ditt program ovanpå hello basavbildning.  

När hello build-kommandot har slutförts kör hello `docker images` kommandot toosee information om nya hello-avbildningen:

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-hello-application-locally"></a>Kör hello programmet lokalt
Kontrollera att av programmet körs lokalt innan du skickar den hello behållaren registret.  

Kör programmet hello mappning datorns port 4000 toohello behållarens port 80 visas:

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

*namnet* ger en namnet toohello kör behållare (i stället för hello behållar-ID).

Ansluta toohello kör behållare.  Öppna en webbläsare som pekar på port 4000, till exempel ”http://localhost:4000” toohello IP-adress. Du bör se hello rubriken ”Hello World”! Visa i hello webbläsare.

![Hello World!][hello-world]

toostop din behållare, kör:

```bash
docker stop my-web-site
```

Ta bort hello behållare från utvecklingsdatorn:

```bash
docker rm my-web-site
```

## <a name="push-hello-image-toohello-container-registry"></a>Push hello avbildningen toohello behållare registret
När du har kontrollerat att hello-program körs i Docker push hello avbildningen tooyour registret i registret för Azure-behållare.

Kör `docker login` toolog i tooyour behållare registret med din [registret autentiseringsuppgifter](../container-registry/container-registry-authentication.md).

hello följande exempel skickar hello-ID och lösenord för ett Azure Active Directory [tjänstens huvudnamn](../active-directory/active-directory-application-objects.md). Du kan till exempel har tilldelat en service principal tooyour registret för ett automation-scenario.  Du kan också logga in med ditt användarnamn och lösenord för registret.

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

hello följande kommando skapar en tagg eller alias för hello bild med ett fullständigt kvalificerad sökväg tooyour register. Det här exemplet platser hello bilden i hello `samples` namnområde tooavoid oreda i hello rot hello registret.

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Push hello avbildningen tooyour behållare registret:

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-hello-docker-image-with-yeoman"></a>Paketet hello Docker avbildningen med Yeoman
hello Service Fabric-SDK för Linux innehåller en [Yeoman](http://yeoman.io/) generator som gör det enkelt toocreate ditt program och lägga till en behållare avbildning. Nu ska vi använda Yeoman toocreate kallas för ett program med en enda dockerbehållare *SimpleContainerApp*.

toocreate ett program för Service Fabric behållare, öppna ett terminalfönster och kör `yo azuresfcontainer`.  

Namnge ditt program (till exempel ”minbehållare”). 

Ange hello URL för hello behållaren bilden i en behållare registret (till exempel ””). 

Den här avbildningen har en arbetsbelastning startpunkten definierats, så behöver tooexplicitly ange inkommande kommandon (kommandon körs i hello behållare som kommer att hålla hello-behållare som körs efter start). 

Ange ett instansantal på ”1”.

![Service Fabric Yeoman-generator för behållare][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a>Konfigurera portmappning och autentisering av behållardatabas
Behållartjänsten behöver en slutpunkt för kommunikation.  Lägg nu till hello-protokollet, porten och typ tooan `Endpoint` i hello ServiceManifest.xml-filen. Den här artikeln lyssnar hello av tjänsten på port 4000: 

```xml
<Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
```
Att tillhandahålla hello `UriScheme` automatiskt registrerar hello behållaren slutpunkt med hello Service Fabric-namngivningstjänst för synlighet. En fullständig ServiceManifest.xml exempelfil tillhandahålls hello slutet av den här artikeln. 

Konfigurera hello behållaren port-till-värd portmappning genom att ange en `PortBinding` princip i `ContainerHostPolicies` hello ApplicationManifest.xml filen.  Den här artikeln `ContainerPort` är 80 (hello behållaren visar port 80, som anges i hello Dockerfile) och `EndpointRef` är ”myserviceTypeEndpoint” (hello slutpunkt som definierats i hello tjänstmanifestet).  Inkommande begäranden toohello tjänst på port 4000 mappas tooport 80 hello behållaren.  Om din behållaren måste tooauthenticate med en privat databas, Lägg sedan till `RepositoryCredentials`.  Lägg till hello kontonamn och lösenord för hello myregistry.azurecr.io behållare registernyckeln för den här artikeln. 

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-hello-service-fabric-application"></a>Bygg- och paketet hello Service Fabric-program
hello Service Fabric Yeoman mallar innehåller ett build-skript för [Gradle](https://gradle.org/), som du kan använda toobuild hello programmet från hello terminal. toobuild och paketet hello program, köra hello följande:

```bash
cd mycontainer
gradle
```

## <a name="deploy-hello-application"></a>Distribuera programmet hello
När hello programmet är skapat kan distribuera du den toohello lokala kluster med hjälp av hello Service Fabric CLI.

Ansluta toohello lokala Service Fabric-klustret.

```bash
sfctl cluster select --endpoint http://localhost:19080
```

Använd hello installera skript hello mallen toocopy programmet hello paketet toohello klustret avbildningsarkivet, registrera hello programtyp och skapa en instans av programmet hello.

```bash
./install.sh
```

Öppna en webbläsare och gå tooService Fabric-Utforskaren på http://localhost:19080/Explorer (Ersätt localhost med hello privat IP hello VM om använder Vagrant på Mac OS X). Expandera noden för hello-program och Lägg märke till att det finns en post för din typ av program och en annan för hello första instansen av den typen.

Ansluta toohello kör behållare.  Öppna en webbläsare som pekar på port 4000, till exempel ”http://localhost:4000” toohello IP-adress. Du bör se hello rubriken ”Hello World”! Visa i hello webbläsare.

![Hello World!][hello-world]

## <a name="clean-up"></a>Rensa
Använd hello avinstallera skript i hello toodelete hello programmet mallinstansen från hello lokal utveckling kluster och avregistrering av programtyp hello.

```bash
./uninstall.sh
```

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
<ServiceManifest Name="myservicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is hello name of your ServiceType.
         hello UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="myserviceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers 
      tooService Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
        <Commands></Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables tooyour container: -->
    
    <EnvironmentVariables>
      <!--
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
      -->
    </EnvironmentVariables>
    
  </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="mycontainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- Import hello ServiceManifest from hello ServicePackage. hello ServiceManifestName and ServiceManifestVersion 
       should match hello Name and Version attributes of hello ServiceManifest element defined in hello 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="myservicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- hello section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using hello 
         ServiceFabric PowerShell module.
         
         hello attribute ServiceTypeName below must match hello name defined in hello imported ServiceManifest.xml file. -->
    <Service Name="myservice">
      <!-- On a local development cluster, set InstanceCount too1.  On a multi-node production 
      cluster, set InstanceCount too-1 for hello container service toorun on every node in 
      hello cluster.
      -->
      <StatelessService ServiceTypeName="myserviceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
## <a name="adding-more-services-tooan-existing-application"></a>Lägga till fler tjänster tooan befintliga program

tooadd en annan behållare tooan tjänstprogrammet redan har skapats med hjälp av yeoman, utföra hello följande steg:

1. Ändra toohello rotkatalog till hello befintliga program.  Till exempel `cd ~/YeomanSamples/MyApplication`om `MyApplication` är hello-program som skapats av Yeoman.
2. Kör `yo azuresfcontainer:AddService`

<a id="manually"></a>


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

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png
