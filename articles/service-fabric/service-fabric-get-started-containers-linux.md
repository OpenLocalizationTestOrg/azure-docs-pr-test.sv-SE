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
# <a name="create-your-first-service-fabric-container-application-on-linux"></a><span data-ttu-id="a339b-104">Skapa din första Service Fabric-behållarapp i Linux</span><span class="sxs-lookup"><span data-stu-id="a339b-104">Create your first Service Fabric container application on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a339b-105">Windows</span><span class="sxs-lookup"><span data-stu-id="a339b-105">Windows</span></span>](service-fabric-get-started-containers.md)
> * [<span data-ttu-id="a339b-106">Linux</span><span class="sxs-lookup"><span data-stu-id="a339b-106">Linux</span></span>](service-fabric-get-started-containers-linux.md)

<span data-ttu-id="a339b-107">Kör ett befintligt program i en Linux-behållare på ett Service Fabric-kluster kräver inte några ändringar tooyour program.</span><span class="sxs-lookup"><span data-stu-id="a339b-107">Running an existing application in a Linux container on a Service Fabric cluster doesn't require any changes tooyour application.</span></span> <span data-ttu-id="a339b-108">Den här artikeln vägleder dig genom att skapa en Docker-avbildning som innehåller en Python [Flask](http://flask.pocoo.org/) program och distribuera den tooa Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="a339b-108">This article walks you through creating a Docker image containing a Python [Flask](http://flask.pocoo.org/) web application and deploying it tooa Service Fabric cluster.</span></span>  <span data-ttu-id="a339b-109">Du kan också dela programmet via [Azure Container-registret](/azure/container-registry/).</span><span class="sxs-lookup"><span data-stu-id="a339b-109">You will also share your containerized application through [Azure Container Registry](/azure/container-registry/).</span></span>  <span data-ttu-id="a339b-110">Den här artikeln förutsätter att du har grundläggande kunskaper om Docker.</span><span class="sxs-lookup"><span data-stu-id="a339b-110">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="a339b-111">Du kan lära dig om Docker genom att läsa hello [Docker översikt](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="a339b-111">You can learn about Docker by reading hello [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a339b-112">Krav</span><span class="sxs-lookup"><span data-stu-id="a339b-112">Prerequisites</span></span>
* <span data-ttu-id="a339b-113">En utvecklingsdator som kör:</span><span class="sxs-lookup"><span data-stu-id="a339b-113">A development computer running:</span></span>
  * <span data-ttu-id="a339b-114">[Service Fabric SDK och verktyg](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="a339b-114">[Service Fabric SDK and tools](service-fabric-get-started-linux.md).</span></span>
  * <span data-ttu-id="a339b-115">[Docker CE för Linux](https://docs.docker.com/engine/installation/#prior-releases).</span><span class="sxs-lookup"><span data-stu-id="a339b-115">[Docker CE for Linux](https://docs.docker.com/engine/installation/#prior-releases).</span></span> 
  * [<span data-ttu-id="a339b-116">Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="a339b-116">Service Fabric CLI</span></span>](service-fabric-cli.md)

* <span data-ttu-id="a339b-117">Ett register i Azure Container Registry – [Skapa ett behållarregister](../container-registry/container-registry-get-started-portal.md) i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a339b-117">A registry in Azure Container Registry - [Create a container registry](../container-registry/container-registry-get-started-portal.md) in your Azure subscription.</span></span> 

## <a name="define-hello-docker-container"></a><span data-ttu-id="a339b-118">Definiera hello dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="a339b-118">Define hello Docker container</span></span>
<span data-ttu-id="a339b-119">Skapa en avbildning baserat på hello [Python avbildningen](https://hub.docker.com/_/python/) finns på Docker-hubb.</span><span class="sxs-lookup"><span data-stu-id="a339b-119">Build an image based on hello [Python image](https://hub.docker.com/_/python/) located on Docker Hub.</span></span> 

<span data-ttu-id="a339b-120">Definiera Docker-behållaren i en Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="a339b-120">Define your Docker container in a Dockerfile.</span></span> <span data-ttu-id="a339b-121">Hej Dockerfile innehåller instruktioner för konfigurerar hello miljön i ditt behållare, inläsning hello-program som du vill toorun och mappa portar.</span><span class="sxs-lookup"><span data-stu-id="a339b-121">hello Dockerfile contains instructions for setting up hello environment inside your container, loading hello application you want toorun, and mapping ports.</span></span> <span data-ttu-id="a339b-122">Hej Dockerfile är hello inkommande toohello `docker build` -kommandot, vilket skapar hello bild.</span><span class="sxs-lookup"><span data-stu-id="a339b-122">hello Dockerfile is hello input toohello `docker build` command, which creates hello image.</span></span> 

<span data-ttu-id="a339b-123">Skapa en tom katalog och skapa hello filen *Dockerfile* (med utan filtillägget).</span><span class="sxs-lookup"><span data-stu-id="a339b-123">Create an empty directory and create hello file *Dockerfile* (with no file extension).</span></span> <span data-ttu-id="a339b-124">Lägg till följande hello för*Dockerfile* och spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="a339b-124">Add hello following too*Dockerfile* and save your changes:</span></span>

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

<span data-ttu-id="a339b-125">Läs hello [Dockerfile referens](https://docs.docker.com/engine/reference/builder/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a339b-125">Read hello [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more information.</span></span>

## <a name="create-a-simple-web-application"></a><span data-ttu-id="a339b-126">Skapa en enkel webbapp</span><span class="sxs-lookup"><span data-stu-id="a339b-126">Create a simple web application</span></span>
<span data-ttu-id="a339b-127">Skapa en flask-webbapplikation som lyssnar på port 80 och returnerar ”Hello World”!.</span><span class="sxs-lookup"><span data-stu-id="a339b-127">Create a Flask web application listening on port 80 that returns "Hello World!".</span></span>  <span data-ttu-id="a339b-128">Hej samma katalog, skapa hello-fil i *requirements.txt*.</span><span class="sxs-lookup"><span data-stu-id="a339b-128">In hello same directory, create hello file *requirements.txt*.</span></span>  <span data-ttu-id="a339b-129">Lägg till följande hello och spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="a339b-129">Add hello following and save your changes:</span></span>
```
Flask
```

<span data-ttu-id="a339b-130">Skapa även hello *app.py* och Lägg till hello följande:</span><span class="sxs-lookup"><span data-stu-id="a339b-130">Also create hello *app.py* file and add hello following:</span></span>

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    
    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

## <a name="build-hello-image"></a><span data-ttu-id="a339b-131">Skapa hello-bild</span><span class="sxs-lookup"><span data-stu-id="a339b-131">Build hello image</span></span>
<span data-ttu-id="a339b-132">Kör hello `docker build` kommandot toocreate hello avbildningen som kör ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a339b-132">Run hello `docker build` command toocreate hello image that runs your web application.</span></span> <span data-ttu-id="a339b-133">Öppna ett PowerShell-fönster och navigera för*c:\temp\helloworldapp*.</span><span class="sxs-lookup"><span data-stu-id="a339b-133">Open a PowerShell window and navigate too*c:\temp\helloworldapp*.</span></span> <span data-ttu-id="a339b-134">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="a339b-134">Run hello following command:</span></span>

```bash
docker build -t helloworldapp .
```

<span data-ttu-id="a339b-135">Det här kommandot versioner hello ny avbildning med hjälp av hello instruktioner i din Dockerfile naming (-t taggning) hello avbildning ”helloworldapp”.</span><span class="sxs-lookup"><span data-stu-id="a339b-135">This command builds hello new image using hello instructions in your Dockerfile, naming (-t tagging) hello image "helloworldapp".</span></span> <span data-ttu-id="a339b-136">Skapa en avbildning hämtar hello basavbildning från Docker-hubb och skapar en ny avbildning som lägger till ditt program ovanpå hello basavbildning.</span><span class="sxs-lookup"><span data-stu-id="a339b-136">Building an image pulls hello base image down from Docker Hub and creates a new image that adds your application on top of hello base image.</span></span>  

<span data-ttu-id="a339b-137">När hello build-kommandot har slutförts kör hello `docker images` kommandot toosee information om nya hello-avbildningen:</span><span class="sxs-lookup"><span data-stu-id="a339b-137">Once hello build command completes, run hello `docker images` command toosee information on hello new image:</span></span>

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-hello-application-locally"></a><span data-ttu-id="a339b-138">Kör hello programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="a339b-138">Run hello application locally</span></span>
<span data-ttu-id="a339b-139">Kontrollera att av programmet körs lokalt innan du skickar den hello behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="a339b-139">Verify that your containerized application runs locally before pushing it hello container registry.</span></span>  

<span data-ttu-id="a339b-140">Kör programmet hello mappning datorns port 4000 toohello behållarens port 80 visas:</span><span class="sxs-lookup"><span data-stu-id="a339b-140">Run hello application, mapping your computer's port 4000 toohello container's exposed port 80:</span></span>

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

<span data-ttu-id="a339b-141">*namnet* ger en namnet toohello kör behållare (i stället för hello behållar-ID).</span><span class="sxs-lookup"><span data-stu-id="a339b-141">*name* gives a name toohello running container (instead of hello container ID).</span></span>

<span data-ttu-id="a339b-142">Ansluta toohello kör behållare.</span><span class="sxs-lookup"><span data-stu-id="a339b-142">Connect toohello running container.</span></span>  <span data-ttu-id="a339b-143">Öppna en webbläsare som pekar på port 4000, till exempel ”http://localhost:4000” toohello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a339b-143">Open a web browser pointing toohello IP address returned on port 4000, for example "http://localhost:4000".</span></span> <span data-ttu-id="a339b-144">Du bör se hello rubriken ”Hello World”!</span><span class="sxs-lookup"><span data-stu-id="a339b-144">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="a339b-145">Visa i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a339b-145">display in hello browser.</span></span>

![Hello World!][hello-world]

<span data-ttu-id="a339b-147">toostop din behållare, kör:</span><span class="sxs-lookup"><span data-stu-id="a339b-147">toostop your container, run:</span></span>

```bash
docker stop my-web-site
```

<span data-ttu-id="a339b-148">Ta bort hello behållare från utvecklingsdatorn:</span><span class="sxs-lookup"><span data-stu-id="a339b-148">Delete hello container from your development machine:</span></span>

```bash
docker rm my-web-site
```

## <a name="push-hello-image-toohello-container-registry"></a><span data-ttu-id="a339b-149">Push hello avbildningen toohello behållare registret</span><span class="sxs-lookup"><span data-stu-id="a339b-149">Push hello image toohello container registry</span></span>
<span data-ttu-id="a339b-150">När du har kontrollerat att hello-program körs i Docker push hello avbildningen tooyour registret i registret för Azure-behållare.</span><span class="sxs-lookup"><span data-stu-id="a339b-150">After you verify that hello application runs in Docker, push hello image tooyour registry in Azure Container Registry.</span></span>

<span data-ttu-id="a339b-151">Kör `docker login` toolog i tooyour behållare registret med din [registret autentiseringsuppgifter](../container-registry/container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="a339b-151">Run `docker login` toolog in tooyour container registry with your [registry credentials](../container-registry/container-registry-authentication.md).</span></span>

<span data-ttu-id="a339b-152">hello följande exempel skickar hello-ID och lösenord för ett Azure Active Directory [tjänstens huvudnamn](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="a339b-152">hello following example passes hello ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="a339b-153">Du kan till exempel har tilldelat en service principal tooyour registret för ett automation-scenario.</span><span class="sxs-lookup"><span data-stu-id="a339b-153">For example, you might have assigned a service principal tooyour registry for an automation scenario.</span></span>  <span data-ttu-id="a339b-154">Du kan också logga in med ditt användarnamn och lösenord för registret.</span><span class="sxs-lookup"><span data-stu-id="a339b-154">Or, you could login using your registry username and password.</span></span>

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="a339b-155">hello följande kommando skapar en tagg eller alias för hello bild med ett fullständigt kvalificerad sökväg tooyour register.</span><span class="sxs-lookup"><span data-stu-id="a339b-155">hello following command creates a tag, or alias, of hello image, with a fully qualified path tooyour registry.</span></span> <span data-ttu-id="a339b-156">Det här exemplet platser hello bilden i hello `samples` namnområde tooavoid oreda i hello rot hello registret.</span><span class="sxs-lookup"><span data-stu-id="a339b-156">This example places hello image in hello `samples` namespace tooavoid clutter in hello root of hello registry.</span></span>

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

<span data-ttu-id="a339b-157">Push hello avbildningen tooyour behållare registret:</span><span class="sxs-lookup"><span data-stu-id="a339b-157">Push hello image tooyour container registry:</span></span>

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-hello-docker-image-with-yeoman"></a><span data-ttu-id="a339b-158">Paketet hello Docker avbildningen med Yeoman</span><span class="sxs-lookup"><span data-stu-id="a339b-158">Package hello Docker image with Yeoman</span></span>
<span data-ttu-id="a339b-159">hello Service Fabric-SDK för Linux innehåller en [Yeoman](http://yeoman.io/) generator som gör det enkelt toocreate ditt program och lägga till en behållare avbildning.</span><span class="sxs-lookup"><span data-stu-id="a339b-159">hello Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy toocreate your application and add a container image.</span></span> <span data-ttu-id="a339b-160">Nu ska vi använda Yeoman toocreate kallas för ett program med en enda dockerbehållare *SimpleContainerApp*.</span><span class="sxs-lookup"><span data-stu-id="a339b-160">Let's use Yeoman toocreate an application with a single Docker container called *SimpleContainerApp*.</span></span>

<span data-ttu-id="a339b-161">toocreate ett program för Service Fabric behållare, öppna ett terminalfönster och kör `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="a339b-161">toocreate a Service Fabric container application, open a terminal window and run `yo azuresfcontainer`.</span></span>  

<span data-ttu-id="a339b-162">Namnge ditt program (till exempel ”minbehållare”).</span><span class="sxs-lookup"><span data-stu-id="a339b-162">Name your application (for example, "mycontainer").</span></span> 

<span data-ttu-id="a339b-163">Ange hello URL för hello behållaren bilden i en behållare registret (till exempel ””).</span><span class="sxs-lookup"><span data-stu-id="a339b-163">Provide hello URL for hello container image in a container registry (for example, "").</span></span> 

<span data-ttu-id="a339b-164">Den här avbildningen har en arbetsbelastning startpunkten definierats, så behöver tooexplicitly ange inkommande kommandon (kommandon körs i hello behållare som kommer att hålla hello-behållare som körs efter start).</span><span class="sxs-lookup"><span data-stu-id="a339b-164">This image has a workload entry-point defined, so need tooexplicitly specify input commands (commands run inside hello container, which will keep hello container running after startup).</span></span> 

<span data-ttu-id="a339b-165">Ange ett instansantal på ”1”.</span><span class="sxs-lookup"><span data-stu-id="a339b-165">Specify an instance count of "1".</span></span>

![Service Fabric Yeoman-generator för behållare][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a><span data-ttu-id="a339b-167">Konfigurera portmappning och autentisering av behållardatabas</span><span class="sxs-lookup"><span data-stu-id="a339b-167">Configure port mapping and container repository authentication</span></span>
<span data-ttu-id="a339b-168">Behållartjänsten behöver en slutpunkt för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="a339b-168">Your containerized service needs an endpoint for communication.</span></span>  <span data-ttu-id="a339b-169">Lägg nu till hello-protokollet, porten och typ tooan `Endpoint` i hello ServiceManifest.xml-filen.</span><span class="sxs-lookup"><span data-stu-id="a339b-169">Now add hello protocol, port, and type tooan `Endpoint` in hello ServiceManifest.xml file.</span></span> <span data-ttu-id="a339b-170">Den här artikeln lyssnar hello av tjänsten på port 4000:</span><span class="sxs-lookup"><span data-stu-id="a339b-170">For this article, hello containerized service listens on port 4000:</span></span> 

```xml
<Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
```
<span data-ttu-id="a339b-171">Att tillhandahålla hello `UriScheme` automatiskt registrerar hello behållaren slutpunkt med hello Service Fabric-namngivningstjänst för synlighet.</span><span class="sxs-lookup"><span data-stu-id="a339b-171">Providing hello `UriScheme` automatically registers hello container endpoint with hello Service Fabric Naming service for discoverability.</span></span> <span data-ttu-id="a339b-172">En fullständig ServiceManifest.xml exempelfil tillhandahålls hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a339b-172">A full ServiceManifest.xml example file is provided at hello end of this article.</span></span> 

<span data-ttu-id="a339b-173">Konfigurera hello behållaren port-till-värd portmappning genom att ange en `PortBinding` princip i `ContainerHostPolicies` hello ApplicationManifest.xml filen.</span><span class="sxs-lookup"><span data-stu-id="a339b-173">Configure hello container port-to-host port mapping by specifying a `PortBinding` policy in `ContainerHostPolicies` of hello ApplicationManifest.xml file.</span></span>  <span data-ttu-id="a339b-174">Den här artikeln `ContainerPort` är 80 (hello behållaren visar port 80, som anges i hello Dockerfile) och `EndpointRef` är ”myserviceTypeEndpoint” (hello slutpunkt som definierats i hello tjänstmanifestet).</span><span class="sxs-lookup"><span data-stu-id="a339b-174">For this article, `ContainerPort` is 80 (hello container exposes port 80, as specified in hello Dockerfile) and `EndpointRef` is "myserviceTypeEndpoint" (hello endpoint defined in hello service manifest).</span></span>  <span data-ttu-id="a339b-175">Inkommande begäranden toohello tjänst på port 4000 mappas tooport 80 hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="a339b-175">Incoming requests toohello service on port 4000 are mapped tooport 80 on hello container.</span></span>  <span data-ttu-id="a339b-176">Om din behållaren måste tooauthenticate med en privat databas, Lägg sedan till `RepositoryCredentials`.</span><span class="sxs-lookup"><span data-stu-id="a339b-176">If your container needs tooauthenticate with a private repository, then add `RepositoryCredentials`.</span></span>  <span data-ttu-id="a339b-177">Lägg till hello kontonamn och lösenord för hello myregistry.azurecr.io behållare registernyckeln för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a339b-177">For this article, add hello account name and password for hello myregistry.azurecr.io container registry.</span></span> 

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-hello-service-fabric-application"></a><span data-ttu-id="a339b-178">Bygg- och paketet hello Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="a339b-178">Build and package hello Service Fabric application</span></span>
<span data-ttu-id="a339b-179">hello Service Fabric Yeoman mallar innehåller ett build-skript för [Gradle](https://gradle.org/), som du kan använda toobuild hello programmet från hello terminal.</span><span class="sxs-lookup"><span data-stu-id="a339b-179">hello Service Fabric Yeoman templates include a build script for [Gradle](https://gradle.org/), which you can use toobuild hello application from hello terminal.</span></span> <span data-ttu-id="a339b-180">toobuild och paketet hello program, köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="a339b-180">toobuild and package hello application, run hello following:</span></span>

```bash
cd mycontainer
gradle
```

## <a name="deploy-hello-application"></a><span data-ttu-id="a339b-181">Distribuera programmet hello</span><span class="sxs-lookup"><span data-stu-id="a339b-181">Deploy hello application</span></span>
<span data-ttu-id="a339b-182">När hello programmet är skapat kan distribuera du den toohello lokala kluster med hjälp av hello Service Fabric CLI.</span><span class="sxs-lookup"><span data-stu-id="a339b-182">Once hello application is built, you can deploy it toohello local cluster using hello Service Fabric CLI.</span></span>

<span data-ttu-id="a339b-183">Ansluta toohello lokala Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="a339b-183">Connect toohello local Service Fabric cluster.</span></span>

```bash
sfctl cluster select --endpoint http://localhost:19080
```

<span data-ttu-id="a339b-184">Använd hello installera skript hello mallen toocopy programmet hello paketet toohello klustret avbildningsarkivet, registrera hello programtyp och skapa en instans av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="a339b-184">Use hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

```bash
./install.sh
```

<span data-ttu-id="a339b-185">Öppna en webbläsare och gå tooService Fabric-Utforskaren på http://localhost:19080/Explorer (Ersätt localhost med hello privat IP hello VM om använder Vagrant på Mac OS X).</span><span class="sxs-lookup"><span data-stu-id="a339b-185">Open a browser and navigate tooService Fabric Explorer at http://localhost:19080/Explorer (replace localhost with hello private IP of hello VM if using Vagrant on Mac OS X).</span></span> <span data-ttu-id="a339b-186">Expandera noden för hello-program och Lägg märke till att det finns en post för din typ av program och en annan för hello första instansen av den typen.</span><span class="sxs-lookup"><span data-stu-id="a339b-186">Expand hello Applications node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>

<span data-ttu-id="a339b-187">Ansluta toohello kör behållare.</span><span class="sxs-lookup"><span data-stu-id="a339b-187">Connect toohello running container.</span></span>  <span data-ttu-id="a339b-188">Öppna en webbläsare som pekar på port 4000, till exempel ”http://localhost:4000” toohello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a339b-188">Open a web browser pointing toohello IP address returned on port 4000, for example "http://localhost:4000".</span></span> <span data-ttu-id="a339b-189">Du bör se hello rubriken ”Hello World”!</span><span class="sxs-lookup"><span data-stu-id="a339b-189">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="a339b-190">Visa i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a339b-190">display in hello browser.</span></span>

![Hello World!][hello-world]

## <a name="clean-up"></a><span data-ttu-id="a339b-192">Rensa</span><span class="sxs-lookup"><span data-stu-id="a339b-192">Clean up</span></span>
<span data-ttu-id="a339b-193">Använd hello avinstallera skript i hello toodelete hello programmet mallinstansen från hello lokal utveckling kluster och avregistrering av programtyp hello.</span><span class="sxs-lookup"><span data-stu-id="a339b-193">Use hello uninstall script provided in hello template toodelete hello application instance from hello local development cluster and unregister hello application type.</span></span>

```bash
./uninstall.sh
```

<span data-ttu-id="a339b-194">När du trycker på hello avbildningen toohello behållare registret kan du ta bort hello lokal image på utvecklingsdatorn:</span><span class="sxs-lookup"><span data-stu-id="a339b-194">After you push hello image toohello container registry you can delete hello local image from your development computer:</span></span>

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a><span data-ttu-id="a339b-195">Komplett exempel på Service Fabric-app och tjänstmanifest</span><span class="sxs-lookup"><span data-stu-id="a339b-195">Complete example Service Fabric application and service manifests</span></span>
<span data-ttu-id="a339b-196">Här är klar hello-tjänsten och applikationsmanifest som används i denna artikel.</span><span class="sxs-lookup"><span data-stu-id="a339b-196">Here are hello complete service and application manifests used in this article.</span></span>

### <a name="servicemanifestxml"></a><span data-ttu-id="a339b-197">ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="a339b-197">ServiceManifest.xml</span></span>
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
### <a name="applicationmanifestxml"></a><span data-ttu-id="a339b-198">ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="a339b-198">ApplicationManifest.xml</span></span>
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
## <a name="adding-more-services-tooan-existing-application"></a><span data-ttu-id="a339b-199">Lägga till fler tjänster tooan befintliga program</span><span class="sxs-lookup"><span data-stu-id="a339b-199">Adding more services tooan existing application</span></span>

<span data-ttu-id="a339b-200">tooadd en annan behållare tooan tjänstprogrammet redan har skapats med hjälp av yeoman, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a339b-200">tooadd another container service tooan application already created using yeoman, perform hello following steps:</span></span>

1. <span data-ttu-id="a339b-201">Ändra toohello rotkatalog till hello befintliga program.</span><span class="sxs-lookup"><span data-stu-id="a339b-201">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="a339b-202">Till exempel `cd ~/YeomanSamples/MyApplication`om `MyApplication` är hello-program som skapats av Yeoman.</span><span class="sxs-lookup"><span data-stu-id="a339b-202">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="a339b-203">Kör `yo azuresfcontainer:AddService`</span><span class="sxs-lookup"><span data-stu-id="a339b-203">Run `yo azuresfcontainer:AddService`</span></span>

<a id="manually"></a>


## <a name="configure-time-interval-before-container-is-force-terminated"></a><span data-ttu-id="a339b-204">Ställ in tidsintervall innan behållaren tvångsavslutas</span><span class="sxs-lookup"><span data-stu-id="a339b-204">Configure time interval before container is force terminated</span></span>

<span data-ttu-id="a339b-205">Du kan konfigurera ett tidsintervall för hello runtime toowait innan hello behållaren tas bort när hello tjänsten tas bort (eller en flytta tooanother nod) har startats.</span><span class="sxs-lookup"><span data-stu-id="a339b-205">You can configure a time interval for hello runtime toowait before hello container is removed after hello service deletion (or a move tooanother node) has started.</span></span> <span data-ttu-id="a339b-206">Konfigurera hello tidsintervall skickar hello `docker stop <time in seconds>` kommandot toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="a339b-206">Configuring hello time interval sends hello `docker stop <time in seconds>` command toohello container.</span></span>   <span data-ttu-id="a339b-207">Mer information finns i [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span><span class="sxs-lookup"><span data-stu-id="a339b-207">For more detail, see [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span></span> <span data-ttu-id="a339b-208">hello tidsintervall toowait anges under hello `Hosting` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a339b-208">hello time interval toowait is specified under hello `Hosting` section.</span></span> <span data-ttu-id="a339b-209">hello följande klustret manifestet fragment visar hur tooset hello vänta intervall:</span><span class="sxs-lookup"><span data-stu-id="a339b-209">hello following cluster manifest snippet shows how tooset hello wait interval:</span></span>

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
<span data-ttu-id="a339b-210">hello standard tidsintervall har angetts too10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="a339b-210">hello default time interval is set too10 seconds.</span></span> <span data-ttu-id="a339b-211">Eftersom den här konfigurationen är dynamiska, en config endast uppgradera hello klustret uppdateringar hello tidsgränsen uppnåddes.</span><span class="sxs-lookup"><span data-stu-id="a339b-211">Since this configuration is dynamic, a config only upgrade on hello cluster updates hello timeout.</span></span> 


## <a name="configure-hello-runtime-tooremove-unused-container-images"></a><span data-ttu-id="a339b-212">Konfigurera hello runtime tooremove oanvända behållaren bilder</span><span class="sxs-lookup"><span data-stu-id="a339b-212">Configure hello runtime tooremove unused container images</span></span>

<span data-ttu-id="a339b-213">Du kan konfigurera hello Service Fabric-kluster tooremove oanvända behållaren bilder från hello-nod.</span><span class="sxs-lookup"><span data-stu-id="a339b-213">You can configure hello Service Fabric cluster tooremove unused container images from hello node.</span></span> <span data-ttu-id="a339b-214">Den här konfigurationen kan disk space toobe återfångats om för många behållare avbildningar finns på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="a339b-214">This configuration allows disk space toobe recaptured if too many container images are present on hello node.</span></span>  <span data-ttu-id="a339b-215">tooenable den här funktionen, uppdatering hello `Hosting` avsnittet i hello klustermanifestet som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="a339b-215">tooenable this feature, update hello `Hosting` section in hello cluster manifest as shown in hello following snippet:</span></span> 


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

<span data-ttu-id="a339b-216">För bilder som inte ska tas bort, kan du ange dem under hello `ContainerImagesToSkip` parameter.</span><span class="sxs-lookup"><span data-stu-id="a339b-216">For images that should not be deleted, you can specify them under hello `ContainerImagesToSkip` parameter.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="a339b-217">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a339b-217">Next steps</span></span>
* <span data-ttu-id="a339b-218">Mer information om hur du kör [behållare i Service Fabric](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a339b-218">Learn more about running [containers on Service Fabric](service-fabric-containers-overview.md).</span></span>
* <span data-ttu-id="a339b-219">Läs hello [distribuera ett .NET-program i en behållare](service-fabric-host-app-in-a-container.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="a339b-219">Read hello [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) tutorial.</span></span>
* <span data-ttu-id="a339b-220">Lär dig mer om hello Service Fabric [programmet livscykel](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="a339b-220">Learn about hello Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
* <span data-ttu-id="a339b-221">Utcheckning hello [kodexempel för Service Fabric-behållaren](https://github.com/Azure-Samples/service-fabric-dotnet-containers) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="a339b-221">Checkout hello [Service Fabric container code samples](https://github.com/Azure-Samples/service-fabric-dotnet-containers) on GitHub.</span></span>

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png
