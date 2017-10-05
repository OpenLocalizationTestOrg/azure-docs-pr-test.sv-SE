---
title: "Skapa en Azure Service Fabric-behållarapp på Linux | Microsoft Docs"
description: "Skapa din första Linux-behållarapp på Azure Service Fabric.  Skapa en Docker-avbildning med din app, överför avbildningen till ett behållarregister och skapa och distribuera en Service Fabric-behållarapp."
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
ms.openlocfilehash: 8355478cb2fff3a63bc4a9b359ec8e2b132c80f6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-service-fabric-container-application-on-linux"></a>Skapa din första Service Fabric-behållarapp i Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Du behöver inga göra några ändringar i din app för att köra en befintlig app i en Linux-behållare i ett Service Fabric-kluster. Den här artikeln vägleder dig genom att skapa en Docker-avbildning som innehåller ett Python [Flask](http://flask.pocoo.org/)-program och distribuera den till ett Service Fabric-kluster.  Du kan också dela programmet via [Azure Container-registret](/azure/container-registry/).  Den här artikeln förutsätter att du har grundläggande kunskaper om Docker. Mer information om Docker finns i [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Översikt över Docker).

## <a name="prerequisites"></a>Krav
* En utvecklingsdator som kör:
  * [Service Fabric SDK och verktyg](service-fabric-get-started-linux.md).
  * [Docker CE för Linux](https://docs.docker.com/engine/installation/#prior-releases). 
  * [Service Fabric CLI](service-fabric-cli.md)

* Ett register i Azure Container Registry – [Skapa ett behållarregister](../container-registry/container-registry-get-started-portal.md) i din Azure-prenumeration. 

## <a name="define-the-docker-container"></a>Definiera dockerbehållare
Skapa en avbildning baserat på [Python-avbildningen](https://hub.docker.com/_/python/) på Docker Hub. 

Definiera Docker-behållaren i en Dockerfile. Dockerfile innehåller instruktioner för att ställa in miljön i di behållare, läsa in programmet som du vill köra och mappa portar. Dockerfile är indata för kommandot `docker build` som skapar avbildningen. 

Skapa en tom katalog och skapa filen *Dockerfile* (utan filtillägget). Lägg till följande i *Dockerfile* och spara dina ändringar:

```
# Use an official Python runtime as a base image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

Läs [Dockerfile-referensen](https://docs.docker.com/engine/reference/builder/) för mer information.

## <a name="create-a-simple-web-application"></a>Skapa en enkel webbapp
Skapa en flask-webbapplikation som lyssnar på port 80 och returnerar ”Hello World”!.  Skapa filen *requirements.txt* i samma katalog.  Lägg till följande och spara dina ändringar:
```
Flask
```

Skapa även filen *app.py* och lägg till följande:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    
    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

## <a name="build-the-image"></a>Skapa avbildningen
Kör kommandot `docker build` för att skapa avbildningen som kör ditt webbprogram. Öppna ett PowerShell-fönster och gå till *c:\temp\helloworldapp*. Kör följande kommando:

```bash
docker build -t helloworldapp .
```

Med det här kommandot skapas den nya avbildningen med hjälp av instruktionerna i din Dockerfile och avbildningen får namnet "helloworldapp" (-t-taggning). Genom att skapa en avbildning hämtas basavbildningen från Docker Hub och en ny avbildning skapas som lägger till ditt program ovanpå basavbildningen.  

När build-kommandot har slutförts kör du `docker images`-kommandot för att se information om den nya avbildningen:

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-the-application-locally"></a>Kör programmet lokalt
Kontrollera att av programmet körs lokalt innan du skickar det till behållarregistret.  

Kör programmet, vilket mappar port 4000 på datorn till behållarens exponerade port 80:

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

*name* namnger den behållare som körs (i stället för behållar-ID:t).

Anslut till den behållare som körs.  Öppna en webbläsare med IP-adressen som returnerades på port 4000, till exempel "http://localhost:4000". Nu visas normalt rubriken "Hello World!" i webbläsaren.

![Hello World!][hello-world]

Om du vill stoppa behållaren kör du:

```bash
docker stop my-web-site
```

Ta bort behållaren från utvecklingsdatorn:

```bash
docker rm my-web-site
```

## <a name="push-the-image-to-the-container-registry"></a>Överför avbildningen till behållarregistret
När du har kontrollerat att behållaren körs på Docker överför du avbildningen till registret i Azure Container Registry.

Kör `docker login` för att logga in till behållarregistret med dina [autentiseringsuppgifter för registret](../container-registry/container-registry-authentication.md).

I följande exempel skickas ID:t och lösenordet för ett Azure Active Directory [-tjänstobjekt](../active-directory/active-directory-application-objects.md). Du kanske till exempel har tilldelat ett tjänstobjekt till registret för ett automatiseringsscenario.  Du kan också logga in med ditt användarnamn och lösenord för registret.

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Följande kommando skapar en tagg, eller ett alias, för avbildningen, med en fullständigt kvalificerad sökväg till registret. I det här exemplet placeras avbildningen i `samples`-namnområdet för att undvika oreda i registrets rot.

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Överför avbildningen till behållarregistret:

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-the-docker-image-with-yeoman"></a>Paketetera Docker-avbildningen med Yeoman
I Service Fabric SDK för Linux finns en [Yeoman](http://yeoman.io/)-generator som gör det enkelt att skapa ditt program och lägga till en behållaravbildning. Nu ska vi använda Yeoman för att skapa ett program med en enda dockerbehållare som kallas *SimpleContainerApp*.

Om du vill skapa ett program för Service Fabric-behållare kan du öppna ett terminalfönster och köra `yo azuresfcontainer`.  

Namnge ditt program (till exempel ”minbehållare”). 

Ange en URL för behållaravbildningen i ett behållarregister (till exempel ””). 

Den här avbildningen har en definierad startpunkt arbetsbelastningen, så måste du uttryckligen ange inkommande kommandon (kommandon körs i den behållare som kommer att hålla den behållare som körs efter start). 

Ange ett instansantal på ”1”.

![Service Fabric Yeoman-generator för behållare][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a>Konfigurera portmappning och autentisering av behållardatabas
Behållartjänsten behöver en slutpunkt för kommunikation.  Lägg nu till protokollet, porten och typen till en `Endpoint` i filen ServiceManifest.xml. Behållartjänsten för den här artikeln lyssnar på port 4000: 

```xml
<Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
```
Genom att tillhandahålla `UriScheme` registreras automatiskt behållarslutpunkten med namngivningstjänsten för Service Fabric för identifiering. En fullständig ServiceManifest.xml-exempelfil finns i slutet av den här artikeln. 

Konfigurera behållarens portmappning (port till värd) genom att ange en `PortBinding`-princip i `ContainerHostPolicies` för filen ApplicationManifest.xml.  I den här artikeln är `ContainerPort` 80 (behållaren exponerar port 80, som anges i Dockerfile) och `EndpointRef` är ”myserviceTypeEndpoint” (slutpunkt som definierats i tjänstmanifestet).  Inkommande begäranden till tjänsten på port 4000 mappas till port 80 för behållaren.  Om behållaren behöver autentiseras med en privat lagringsplats lägger du till `RepositoryCredentials`.  I den här artikeln lägger du till kontonamnet och lösenordet för behållarregistret myregistry.azurecr.io. 

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-the-service-fabric-application"></a>Utveckla och distribuera ett Service Fabric-program
I Service Fabric Yeoman-mallarna ingår ett byggskript för [Gradle](https://gradle.org/) som du kan använda för att skapa programmet från terminalen. När du ska bygga och paketera programmet kör du följande:

```bash
cd mycontainer
gradle
```

## <a name="deploy-the-application"></a>Distribuera programmet
När du har skapat programmet kan du distribuera det till det lokala klustret med Service Fabric CLI.

Anslut till det lokala Service Fabric-klustret.

```bash
sfctl cluster select --endpoint http://localhost:19080
```

Använd installationsskriptet som medföljer mallen för att kopiera programpaketet till klustrets avbildningsarkiv, registrera programtypen och skapa en instans av programmet.

```bash
./install.sh
```

Öppna en webbläsare och gå till Service Fabric Explorer på http://localhost:19080/Explorer (ersätt localhost med den virtuella datorns privata IP om du använder Vagrant på Mac OS X). Expandera programnoden och observera att det nu finns en post för din programtyp och en post för den första instansen av den typen.

Anslut till den behållare som körs.  Öppna en webbläsare med IP-adressen som returnerades på port 4000, till exempel "http://localhost:4000". Nu visas normalt rubriken "Hello World!" i webbläsaren.

![Hello World!][hello-world]

## <a name="clean-up"></a>Rensa
Använd installationsskriptet som medföljer mallen för att ta bort programinstansen från det lokala utvecklingsklustret och avregistrera programtypen.

```bash
./uninstall.sh
```

När du har överfört avbildningen till behållarregistret kan du ta bort den lokala avbildningen från utvecklingsdatorn:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Komplett exempel på Service Fabric-app och tjänstmanifest
Här är de fullständiga tjänst- och appmanifesten som används i den här artikeln.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="myservicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="myserviceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers 
      to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
        <Commands></Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    
    <EnvironmentVariables>
      <!--
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
      -->
    </EnvironmentVariables>
    
  </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
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
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
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
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="myservice">
      <!-- On a local development cluster, set InstanceCount to 1.  On a multi-node production 
      cluster, set InstanceCount to -1 for the container service to run on every node in 
      the cluster.
      -->
      <StatelessService ServiceTypeName="myserviceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
## <a name="adding-more-services-to-an-existing-application"></a>Lägga till fler tjänster till ett befintligt program

Om du vill lägga till en till behållartjänst till ett program som redan har skapats med hjälp av yeoman utför du följande steg:

1. Ändra katalogen till roten för det befintliga programmet.  Till exempel `cd ~/YeomanSamples/MyApplication` om `MyApplication` är programmet som skapats av Yeoman.
2. Kör `yo azuresfcontainer:AddService`

<a id="manually"></a>


## <a name="configure-time-interval-before-container-is-force-terminated"></a>Ställ in tidsintervall innan behållaren tvångsavslutas

Du kan ställa in ett tidsintervall för hur lång exekveringstid som ska gå innan behållaren tas bort när borttagning av tjänsten (eller flytt till en annan nod) har påbörjats. När du ställer in ett tidsintervall skickas kommandot `docker stop <time in seconds>` till behållaren.   Mer information finns i [docker stop](https://docs.docker.com/engine/reference/commandline/stop/). Tidsintervallet anges i avsnittet `Hosting`. I följande klustermanifestutdrag visas hur du ställer in väntetidsintervallet:

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
Standardtidsintervallet är inställt på 10 sekunder. Eftersom inställningen är dynamisk uppdateras tidsgränsen med en konfigurationsuppdatering på klustret. 


## <a name="configure-the-runtime-to-remove-unused-container-images"></a>Ställ in exekveringstid för att ta bort behållaravbildningar som inte används

Du kan ställa in Service Fabric-klustret på att ta bort oanvända behållaravbildningar från noden. Med den här inställningen kan du få tillbaka diskutrymme om det finns för många behållaravbildningar på noden.  Aktivera funktionen genom att uppdatera avsnittet `Hosting` i klustermanifestet enligt följande utdrag: 


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

Avbildningar som inte ska raderas kan du ange under parametern `ContainerImagesToSkip`. 


## <a name="next-steps"></a>Nästa steg
* Mer information om hur du kör [behållare i Service Fabric](service-fabric-containers-overview.md).
* Läs kursen [Distribuera ett .NET-program i en behållare](service-fabric-host-app-in-a-container.md).
* Läs om Service Fabric-[applivscykeln](service-fabric-application-lifecycle.md).
* Se [kodexempel för Service Fabric-behållare](https://github.com/Azure-Samples/service-fabric-dotnet-containers) på GitHub.

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png
