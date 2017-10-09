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
# <a name="create-your-first-service-fabric-container-application-on-windows"></a><span data-ttu-id="af507-104">Skapa din första Service Fabric-behållarapp i Windows</span><span class="sxs-lookup"><span data-stu-id="af507-104">Create your first Service Fabric container application on Windows</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="af507-105">Windows</span><span class="sxs-lookup"><span data-stu-id="af507-105">Windows</span></span>](service-fabric-get-started-containers.md)
> * [<span data-ttu-id="af507-106">Linux</span><span class="sxs-lookup"><span data-stu-id="af507-106">Linux</span></span>](service-fabric-get-started-containers-linux.md)

<span data-ttu-id="af507-107">Kör ett befintligt program i en Windows-behållare på ett Service Fabric-kluster kräver inte några ändringar tooyour program.</span><span class="sxs-lookup"><span data-stu-id="af507-107">Running an existing application in a Windows container on a Service Fabric cluster doesn't require any changes tooyour application.</span></span> <span data-ttu-id="af507-108">Den här artikeln vägleder dig genom att skapa en Docker-avbildning som innehåller en Python [Flask](http://flask.pocoo.org/) program och distribuera den tooa Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="af507-108">This article walks you through creating a Docker image containing a Python [Flask](http://flask.pocoo.org/) web application and deploying it tooa Service Fabric cluster.</span></span>  <span data-ttu-id="af507-109">Du kan också dela programmet via [Azure Container-registret](/azure/container-registry/).</span><span class="sxs-lookup"><span data-stu-id="af507-109">You will also share your containerized application through [Azure Container Registry](/azure/container-registry/).</span></span>  <span data-ttu-id="af507-110">Den här artikeln förutsätter att du har grundläggande kunskaper om Docker.</span><span class="sxs-lookup"><span data-stu-id="af507-110">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="af507-111">Du kan lära dig om Docker genom att läsa hello [Docker översikt](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="af507-111">You can learn about Docker by reading hello [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af507-112">Krav</span><span class="sxs-lookup"><span data-stu-id="af507-112">Prerequisites</span></span>
<span data-ttu-id="af507-113">En utvecklingsdator som kör:</span><span class="sxs-lookup"><span data-stu-id="af507-113">A development computer running:</span></span>
* <span data-ttu-id="af507-114">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="af507-114">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="af507-115">[Service Fabric SDK och verktyg](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="af507-115">[Service Fabric SDK and tools](service-fabric-get-started.md).</span></span>
*  <span data-ttu-id="af507-116">Docker för Windows.</span><span class="sxs-lookup"><span data-stu-id="af507-116">Docker for Windows.</span></span>  <span data-ttu-id="af507-117">[Hämta Docker CE för Windows (stabil)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description).</span><span class="sxs-lookup"><span data-stu-id="af507-117">[Get Docker CE for Windows (stable)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description).</span></span> <span data-ttu-id="af507-118">Efter installation och Start Docker, högerklicka på ikonen i systemfältet hello och välj **växla tooWindows behållare**.</span><span class="sxs-lookup"><span data-stu-id="af507-118">After installing and starting Docker, right-click on hello tray icon and select **Switch tooWindows containers**.</span></span> <span data-ttu-id="af507-119">Detta är obligatorisk toorun Docker bilder baserat på Windows.</span><span class="sxs-lookup"><span data-stu-id="af507-119">This is required toorun Docker images based on Windows.</span></span>

<span data-ttu-id="af507-120">Ett Windows-kluster med tre eller fler noder som kör Windows Server 2016 med behållare – [Skapa ett kluster](service-fabric-cluster-creation-via-portal.md) eller [prova Service Fabric utan kostnad](https://aka.ms/tryservicefabric).</span><span class="sxs-lookup"><span data-stu-id="af507-120">A Windows cluster with three or more nodes running on Windows Server 2016 with Containers- [Create a cluster](service-fabric-cluster-creation-via-portal.md) or [try Service Fabric for free](https://aka.ms/tryservicefabric).</span></span>

<span data-ttu-id="af507-121">Ett register i Azure Container Registry – [Skapa ett behållarregister](../container-registry/container-registry-get-started-portal.md) i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="af507-121">A registry in Azure Container Registry - [Create a container registry](../container-registry/container-registry-get-started-portal.md) in your Azure subscription.</span></span>

## <a name="define-hello-docker-container"></a><span data-ttu-id="af507-122">Definiera hello dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="af507-122">Define hello Docker container</span></span>
<span data-ttu-id="af507-123">Skapa en avbildning baserat på hello [Python avbildningen](https://hub.docker.com/_/python/) finns på Docker-hubb.</span><span class="sxs-lookup"><span data-stu-id="af507-123">Build an image based on hello [Python image](https://hub.docker.com/_/python/) located on Docker Hub.</span></span>

<span data-ttu-id="af507-124">Definiera Docker-behållaren i en Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="af507-124">Define your Docker container in a Dockerfile.</span></span> <span data-ttu-id="af507-125">Hej Dockerfile innehåller instruktioner för konfigurerar hello miljön i ditt behållare, inläsning hello-program som du vill toorun och mappa portar.</span><span class="sxs-lookup"><span data-stu-id="af507-125">hello Dockerfile contains instructions for setting up hello environment inside your container, loading hello application you want toorun, and mapping ports.</span></span> <span data-ttu-id="af507-126">Hej Dockerfile är hello inkommande toohello `docker build` -kommandot, vilket skapar hello bild.</span><span class="sxs-lookup"><span data-stu-id="af507-126">hello Dockerfile is hello input toohello `docker build` command, which creates hello image.</span></span>

<span data-ttu-id="af507-127">Skapa en tom katalog och skapa hello filen *Dockerfile* (med utan filtillägget).</span><span class="sxs-lookup"><span data-stu-id="af507-127">Create an empty directory and create hello file *Dockerfile* (with no file extension).</span></span> <span data-ttu-id="af507-128">Lägg till följande hello för*Dockerfile* och spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="af507-128">Add hello following too*Dockerfile* and save your changes:</span></span>

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

<span data-ttu-id="af507-129">Läs hello [Dockerfile referens](https://docs.docker.com/engine/reference/builder/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="af507-129">Read hello [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more information.</span></span>

## <a name="create-a-simple-web-application"></a><span data-ttu-id="af507-130">Skapa en enkel webbapp</span><span class="sxs-lookup"><span data-stu-id="af507-130">Create a simple web application</span></span>
<span data-ttu-id="af507-131">Skapa en flask-webbapplikation som lyssnar på port 80 och returnerar ”Hello World”!.</span><span class="sxs-lookup"><span data-stu-id="af507-131">Create a Flask web application listening on port 80 that returns "Hello World!".</span></span>  <span data-ttu-id="af507-132">Hej samma katalog, skapa hello-fil i *requirements.txt*.</span><span class="sxs-lookup"><span data-stu-id="af507-132">In hello same directory, create hello file *requirements.txt*.</span></span>  <span data-ttu-id="af507-133">Lägg till följande hello och spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="af507-133">Add hello following and save your changes:</span></span>
```
Flask
```

<span data-ttu-id="af507-134">Skapa även hello *app.py* och Lägg till hello följande:</span><span class="sxs-lookup"><span data-stu-id="af507-134">Also create hello *app.py* file and add hello following:</span></span>

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
## <a name="build-hello-image"></a><span data-ttu-id="af507-135">Skapa hello-bild</span><span class="sxs-lookup"><span data-stu-id="af507-135">Build hello image</span></span>
<span data-ttu-id="af507-136">Kör hello `docker build` kommandot toocreate hello avbildningen som kör ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="af507-136">Run hello `docker build` command toocreate hello image that runs your web application.</span></span> <span data-ttu-id="af507-137">Öppna ett PowerShell-fönster och navigera toohello katalog som innehåller hello Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="af507-137">Open a PowerShell window and navigate toohello directory containing hello Dockerfile.</span></span> <span data-ttu-id="af507-138">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="af507-138">Run hello following command:</span></span>

```
docker build -t helloworldapp .
```

<span data-ttu-id="af507-139">Det här kommandot versioner hello ny avbildning med hjälp av hello instruktioner i din Dockerfile naming (-t taggning) hello avbildning ”helloworldapp”.</span><span class="sxs-lookup"><span data-stu-id="af507-139">This command builds hello new image using hello instructions in your Dockerfile, naming (-t tagging) hello image "helloworldapp".</span></span> <span data-ttu-id="af507-140">Skapa en avbildning hämtar hello basavbildning från Docker-hubb och skapar en ny avbildning som lägger till ditt program ovanpå hello basavbildning.</span><span class="sxs-lookup"><span data-stu-id="af507-140">Building an image pulls hello base image down from Docker Hub and creates a new image that adds your application on top of hello base image.</span></span>  

<span data-ttu-id="af507-141">När hello build-kommandot har slutförts kör hello `docker images` kommandot toosee information om nya hello-avbildningen:</span><span class="sxs-lookup"><span data-stu-id="af507-141">Once hello build command completes, run hello `docker images` command toosee information on hello new image:</span></span>

```
$ docker images

REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-hello-application-locally"></a><span data-ttu-id="af507-142">Kör hello programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="af507-142">Run hello application locally</span></span>
<span data-ttu-id="af507-143">Kontrollera avbildningen lokalt innan du skickar den hello behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="af507-143">Verify your image locally before pushing it hello container registry.</span></span>  

<span data-ttu-id="af507-144">Kör programmet hello:</span><span class="sxs-lookup"><span data-stu-id="af507-144">Run hello application:</span></span>

```
docker run -d --name my-web-site helloworldapp
```

<span data-ttu-id="af507-145">*namnet* ger en namnet toohello kör behållare (i stället för hello behållar-ID).</span><span class="sxs-lookup"><span data-stu-id="af507-145">*name* gives a name toohello running container (instead of hello container ID).</span></span>

<span data-ttu-id="af507-146">När hello behållare startar hitta dess IP-adress så att du kan ansluta tooyour kör behållare från en webbläsare:</span><span class="sxs-lookup"><span data-stu-id="af507-146">Once hello container starts, find its IP address so that you can connect tooyour running container from a browser:</span></span>
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

<span data-ttu-id="af507-147">Ansluta toohello kör behållare.</span><span class="sxs-lookup"><span data-stu-id="af507-147">Connect toohello running container.</span></span>  <span data-ttu-id="af507-148">Öppna en webbläsare som pekar toohello IP-adress returneras, till exempel ”http://172.31.194.61”.</span><span class="sxs-lookup"><span data-stu-id="af507-148">Open a web browser pointing toohello IP address returned, for example "http://172.31.194.61".</span></span> <span data-ttu-id="af507-149">Du bör se hello rubriken ”Hello World”!</span><span class="sxs-lookup"><span data-stu-id="af507-149">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="af507-150">Visa i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="af507-150">display in hello browser.</span></span>

<span data-ttu-id="af507-151">toostop din behållare, kör:</span><span class="sxs-lookup"><span data-stu-id="af507-151">toostop your container, run:</span></span>

```
docker stop my-web-site
```

<span data-ttu-id="af507-152">Ta bort hello behållare från utvecklingsdatorn:</span><span class="sxs-lookup"><span data-stu-id="af507-152">Delete hello container from your development machine:</span></span>

```
docker rm my-web-site
```

<a id="Push-Containers"></a>
## <a name="push-hello-image-toohello-container-registry"></a><span data-ttu-id="af507-153">Push hello avbildningen toohello behållare registret</span><span class="sxs-lookup"><span data-stu-id="af507-153">Push hello image toohello container registry</span></span>
<span data-ttu-id="af507-154">När du har kontrollerat hello behållaren som körs på utvecklingsdatorn push hello avbildningen tooyour registret i registret för Azure-behållare.</span><span class="sxs-lookup"><span data-stu-id="af507-154">After you verify that hello container runs on your development machine, push hello image tooyour registry in Azure Container Registry.</span></span>

<span data-ttu-id="af507-155">Kör ``docker login`` toolog i tooyour behållare registret med din [registret autentiseringsuppgifter](../container-registry/container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="af507-155">Run ``docker login`` toolog in tooyour container registry with your [registry credentials](../container-registry/container-registry-authentication.md).</span></span>

<span data-ttu-id="af507-156">hello följande exempel skickar hello-ID och lösenord för ett Azure Active Directory [tjänstens huvudnamn](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="af507-156">hello following example passes hello ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="af507-157">Du kan till exempel har tilldelat en service principal tooyour registret för ett automation-scenario.</span><span class="sxs-lookup"><span data-stu-id="af507-157">For example, you might have assigned a service principal tooyour registry for an automation scenario.</span></span> <span data-ttu-id="af507-158">Du kan också logga in med ditt användarnamn och lösenord för registret.</span><span class="sxs-lookup"><span data-stu-id="af507-158">Or, you could login using your registry username and password.</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="af507-159">hello följande kommando skapar en tagg eller alias för hello bild med ett fullständigt kvalificerad sökväg tooyour register.</span><span class="sxs-lookup"><span data-stu-id="af507-159">hello following command creates a tag, or alias, of hello image, with a fully qualified path tooyour registry.</span></span> <span data-ttu-id="af507-160">Det här exemplet platser hello bilden i hello ```samples``` namnområde tooavoid oreda i hello rot hello registret.</span><span class="sxs-lookup"><span data-stu-id="af507-160">This example places hello image in hello ```samples``` namespace tooavoid clutter in hello root of hello registry.</span></span>

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

<span data-ttu-id="af507-161">Push hello avbildningen tooyour behållare registret:</span><span class="sxs-lookup"><span data-stu-id="af507-161">Push hello image tooyour container registry:</span></span>

```
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="create-hello-containerized-service-in-visual-studio"></a><span data-ttu-id="af507-162">Skapa hello container service i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af507-162">Create hello containerized service in Visual Studio</span></span>
<span data-ttu-id="af507-163">hello Service Fabric-SDK och verktyg tillhandahåller en tjänst mallen toohelp du skapar ett container program.</span><span class="sxs-lookup"><span data-stu-id="af507-163">hello Service Fabric SDK and tools provide a service template toohelp you create a containerized application.</span></span>

1. <span data-ttu-id="af507-164">Starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af507-164">Start Visual Studio.</span></span>  <span data-ttu-id="af507-165">Välj **Arkiv** > **Nytt** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="af507-165">Select **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="af507-166">Välj **Service Fabric-programmet**, ge det namnet "MyFirstContainer" och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="af507-166">Select **Service Fabric application**, name it "MyFirstContainer", and click **OK**.</span></span>
3. <span data-ttu-id="af507-167">Välj **gäst behållaren** hello listan över **tjänstmallar**.</span><span class="sxs-lookup"><span data-stu-id="af507-167">Select **Guest Container** from hello list of **service templates**.</span></span>
4. <span data-ttu-id="af507-168">I **avbildningsnamn** ange ”myregistry.azurecr.io/samples/helloworldapp”, hello bild du pushas tooyour behållare databasen.</span><span class="sxs-lookup"><span data-stu-id="af507-168">In **Image Name** enter "myregistry.azurecr.io/samples/helloworldapp", hello image you pushed tooyour container repository.</span></span>
5. <span data-ttu-id="af507-169">Namnge tjänsten och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="af507-169">Give your service a name, and click **OK**.</span></span>

## <a name="configure-communication"></a><span data-ttu-id="af507-170">Konfigurera kommunikation</span><span class="sxs-lookup"><span data-stu-id="af507-170">Configure communication</span></span>
<span data-ttu-id="af507-171">hello container service måste en slutpunkt för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="af507-171">hello containerized service needs an endpoint for communication.</span></span> <span data-ttu-id="af507-172">Lägg till en `Endpoint` element med hello-protokollet, porten och typen toohello ServiceManifest.xml fil.</span><span class="sxs-lookup"><span data-stu-id="af507-172">Add an `Endpoint` element with hello protocol, port, and type toohello ServiceManifest.xml file.</span></span> <span data-ttu-id="af507-173">Hello av tjänsten lyssnar på port 8081 för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="af507-173">For this article, hello containerized service listens on port 8081.</span></span>  <span data-ttu-id="af507-174">I det här exemplet används en fast port, 8081.</span><span class="sxs-lookup"><span data-stu-id="af507-174">In this example, a fixed port 8081 is used.</span></span>  <span data-ttu-id="af507-175">Om ingen port anges väljs en slumpmässigt vald port från hello portintervall för programmet.</span><span class="sxs-lookup"><span data-stu-id="af507-175">If no port is specified, a random port from hello application port range is chosen.</span></span>  

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
  </Endpoints>
</Resources>
```

<span data-ttu-id="af507-176">Genom att definiera en slutpunkt publicerar Service Fabric hello endpoint toohello Naming service.</span><span class="sxs-lookup"><span data-stu-id="af507-176">By defining an endpoint, Service Fabric publishes hello endpoint toohello Naming service.</span></span>  <span data-ttu-id="af507-177">Andra tjänster som körs i hello kluster kan lösa den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="af507-177">Other services running in hello cluster can resolve this container.</span></span>  <span data-ttu-id="af507-178">Du kan också utföra behållare till en annan kommunikation via hello [omvänd proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="af507-178">You can also perform container-to-container communication using hello [reverse proxy](service-fabric-reverseproxy.md).</span></span>  <span data-ttu-id="af507-179">Kommunikation utförs genom att ange hello omvänd proxy HTTP lyssningsport och hello namnet på hello-tjänster som du vill använda toocommunicate med som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="af507-179">Communication is performed by providing hello reverse proxy HTTP listening port and hello name of hello services that you want toocommunicate with as environment variables.</span></span>

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="af507-180">Konfigurera och ange miljövariabler</span><span class="sxs-lookup"><span data-stu-id="af507-180">Configure and set environment variables</span></span>
<span data-ttu-id="af507-181">Miljövariabler kan anges för varje kodpaketet i hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="af507-181">Environment variables can be specified for each code package in hello service manifest.</span></span> <span data-ttu-id="af507-182">Den här funktionen är tillgänglig för alla tjänster oavsett om de har distribueras som behållare eller processer, eller körbara gäster.</span><span class="sxs-lookup"><span data-stu-id="af507-182">This feature is available for all services irrespective of whether they are deployed as containers or processes or guest executables.</span></span> <span data-ttu-id="af507-183">Du kan åsidosätta miljövariabeln värden i hello application manifest eller ange dem under distributionen som parametrar för programmet.</span><span class="sxs-lookup"><span data-stu-id="af507-183">You can override environment variable values in hello application manifest or specify them during deployment as application parameters.</span></span>

<span data-ttu-id="af507-184">hello följande service manifest XML-fragment visar ett exempel på hur toospecify miljövariabler för ett paket med koden:</span><span class="sxs-lookup"><span data-stu-id="af507-184">hello following service manifest XML snippet shows an example of how toospecify environment variables for a code package:</span></span>
```xml
<CodePackage Name="Code" Version="1.0.0">
  ...
  <EnvironmentVariables>
    <EnvironmentVariable Name="HttpGatewayPort" Value=""/>    
  </EnvironmentVariables>
</CodePackage>
```

<span data-ttu-id="af507-185">De här miljövariablerna kan åsidosättas i programmanifestet hello:</span><span class="sxs-lookup"><span data-stu-id="af507-185">These environment variables can be overridden in hello application manifest:</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
  </EnvironmentOverrides>
  ...
</ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping-and-container-to-container-discovery"></a><span data-ttu-id="af507-186">Konfigurera mappning mellan behållarport och värdport och identifiering mellan behållare</span><span class="sxs-lookup"><span data-stu-id="af507-186">Configure container port-to-host port mapping and container-to-container discovery</span></span>
<span data-ttu-id="af507-187">Konfigurera en toocommunicate för porten som används av värden med hello behållare.</span><span class="sxs-lookup"><span data-stu-id="af507-187">Configure a host port used toocommunicate  with hello container.</span></span> <span data-ttu-id="af507-188">hello-portbindningen mappar hello port på vilken hello tjänsten lyssnar inuti hello behållaren tooa port på hello värden.</span><span class="sxs-lookup"><span data-stu-id="af507-188">hello port binding maps hello port on which hello service is listening inside hello container tooa port on hello host.</span></span> <span data-ttu-id="af507-189">Lägg till en `PortBinding` element i `ContainerHostPolicies` element av hello ApplicationManifest.xml fil.</span><span class="sxs-lookup"><span data-stu-id="af507-189">Add a `PortBinding` element in `ContainerHostPolicies` element of hello ApplicationManifest.xml file.</span></span>  <span data-ttu-id="af507-190">Den här artikeln `ContainerPort` är 80 (hello behållaren visar port 80, som anges i hello Dockerfile) och `EndpointRef` är ”Guest1TypeEndpoint” (hello slutpunkt som tidigare definierats i hello tjänstmanifestet).</span><span class="sxs-lookup"><span data-stu-id="af507-190">For this article, `ContainerPort` is 80 (hello container exposes port 80, as specified in hello Dockerfile) and `EndpointRef` is "Guest1TypeEndpoint" (hello endpoint previously defined in hello service manifest).</span></span>  <span data-ttu-id="af507-191">Inkommande begäranden toohello tjänst på port 8081 mappas tooport 80 hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="af507-191">Incoming requests toohello service on port 8081 are mapped tooport 80 on hello container.</span></span>

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-container-registry-authentication"></a><span data-ttu-id="af507-192">Konfigurera autentisering av behållarregister</span><span class="sxs-lookup"><span data-stu-id="af507-192">Configure container registry authentication</span></span>
<span data-ttu-id="af507-193">Konfigurera behållaren registret autentisering genom att lägga till `RepositoryCredentials` för`ContainerHostPolicies` hello ApplicationManifest.xml filen.</span><span class="sxs-lookup"><span data-stu-id="af507-193">Configure container registry authentication by adding `RepositoryCredentials` too`ContainerHostPolicies` of hello ApplicationManifest.xml file.</span></span> <span data-ttu-id="af507-194">Lägg till hello konto och lösenord för hello myregistry.azurecr.io behållare registret, vilket gör att hello service toodownload hello behållaren avbildningen från hello databasen.</span><span class="sxs-lookup"><span data-stu-id="af507-194">Add hello account and password for hello myregistry.azurecr.io container registry, which allows hello service toodownload hello container image from hello repository.</span></span>

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

<span data-ttu-id="af507-195">Vi rekommenderar att du krypterar hello databasen lösenord med hjälp av en chiffrering av certifikat som har distribuerats tooall klusternoder hello.</span><span class="sxs-lookup"><span data-stu-id="af507-195">We recommend that you encrypt hello repository password by using an encipherment certificate that's deployed tooall nodes of hello cluster.</span></span> <span data-ttu-id="af507-196">När Service Fabric distribuerar hello service paketet toohello kluster, är hello chiffrering certifikat används toodecrypt hello chiffertext.</span><span class="sxs-lookup"><span data-stu-id="af507-196">When Service Fabric deploys hello service package toohello cluster, hello encipherment certificate is used toodecrypt hello cipher text.</span></span>  <span data-ttu-id="af507-197">hello Invoke-ServiceFabricEncryptText cmdlet har använt toocreate hello chiffertext för hello lösenord, som har lagts till toohello ApplicationManifest.xml fil.</span><span class="sxs-lookup"><span data-stu-id="af507-197">hello Invoke-ServiceFabricEncryptText cmdlet is used toocreate hello cipher text for hello password, which is added toohello ApplicationManifest.xml file.</span></span>

<span data-ttu-id="af507-198">hello följande skript skapar ett nytt självsignerat certifikat och exporterar den tooa PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="af507-198">hello following script creates a new self-signed certificate and exports it tooa PFX file.</span></span>  <span data-ttu-id="af507-199">hello certifikatet importeras till en befintlig nyckelvalvet och sedan distribueras toohello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="af507-199">hello certificate is imported into an existing key vault and then deployed toohello Service Fabric cluster.</span></span>

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
<span data-ttu-id="af507-200">Kryptera hello lösenord med hjälp av hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="af507-200">Encrypt hello password using hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet.</span></span>

```powershell
$text = "=P==/==/=8=/=+u4lyOB=+=nWzEeRfF="
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint $cer.Thumbprint -Text $text -StoreLocation Local -StoreName My
```

<span data-ttu-id="af507-201">Ersätt hello lösenord med hello chiffertext som returneras av hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet och ange `PasswordEncrypted` för ”true”.</span><span class="sxs-lookup"><span data-stu-id="af507-201">Replace hello password with hello cipher text returned by hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet and set `PasswordEncrypted` too"true".</span></span>

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

## <a name="configure-isolation-mode"></a><span data-ttu-id="af507-202">Konfigurera isoleringsläge</span><span class="sxs-lookup"><span data-stu-id="af507-202">Configure isolation mode</span></span>
<span data-ttu-id="af507-203">Windows stöder två isoleringslägen för behållare: process och Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="af507-203">Windows supports two isolation modes for containers: process and Hyper-V.</span></span> <span data-ttu-id="af507-204">Med hello arbetsprocesser hello alla hello-behållare som körs på samma värd datorn resursen hello kernel med hello-värden.</span><span class="sxs-lookup"><span data-stu-id="af507-204">With hello process isolation mode, all hello containers running on hello same host machine share hello kernel with hello host.</span></span> <span data-ttu-id="af507-205">Med hello Hyper-V-isoleringsläge isoleras hello kärnor mellan varje Hyper-V-behållaren och hello behållaren värden.</span><span class="sxs-lookup"><span data-stu-id="af507-205">With hello Hyper-V isolation mode, hello kernels are isolated between each Hyper-V container and hello container host.</span></span> <span data-ttu-id="af507-206">hello isoleringsläge har angetts i hello `ContainerHostPolicies` element i hello programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="af507-206">hello isolation mode is specified in hello `ContainerHostPolicies` element in hello application manifest file.</span></span> <span data-ttu-id="af507-207">hello arbetslägen som kan anges är `process`, `hyperv`, och `default`.</span><span class="sxs-lookup"><span data-stu-id="af507-207">hello isolation modes that can be specified are `process`, `hyperv`, and `default`.</span></span> <span data-ttu-id="af507-208">hello standard isoleringsläge standardvärden för`process` på Windows Server är värd för, och är som standard för`hyperv` på Windows 10-värdar.</span><span class="sxs-lookup"><span data-stu-id="af507-208">hello default isolation mode defaults too`process` on Windows Server hosts, and defaults too`hyperv` on Windows 10 hosts.</span></span> <span data-ttu-id="af507-209">hello följande utdrag visar hur hello isoleringsläge har angetts i hello programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="af507-209">hello following snippet shows how hello isolation mode is specified in hello application manifest file.</span></span>

```xml
<ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
```

## <a name="configure-resource-governance"></a><span data-ttu-id="af507-210">Konfigurera resursstyrning</span><span class="sxs-lookup"><span data-stu-id="af507-210">Configure resource governance</span></span>
<span data-ttu-id="af507-211">[Resurs-styrning](service-fabric-resource-governance.md) begränsar hello resurser som hello behållare kan använda på hello värden.</span><span class="sxs-lookup"><span data-stu-id="af507-211">[Resource governance](service-fabric-resource-governance.md) restricts hello resources that hello container can use on hello host.</span></span> <span data-ttu-id="af507-212">Hej `ResourceGovernancePolicy` element som anges i hello programmanifestet är används toodeclare gränserna för ett paket för service-kod.</span><span class="sxs-lookup"><span data-stu-id="af507-212">hello `ResourceGovernancePolicy` element, which is specified in hello application manifest, is used toodeclare resource limits for a service code package.</span></span> <span data-ttu-id="af507-213">Resursen gränser kan anges för hello följande resurser: minne, MemorySwap, CpuShares (CPU relativa viktade), MemoryReservationInMB BlkioWeight (BlockIO relativa vikt).</span><span class="sxs-lookup"><span data-stu-id="af507-213">Resource limits can be set for hello following resources: Memory, MemorySwap, CpuShares (CPU relative weight), MemoryReservationInMB, BlkioWeight (BlockIO relative weight).</span></span>  <span data-ttu-id="af507-214">I det här exemplet hämtar servicepaket Guest1Pkg en kärna på hello klusternoder där den är placerad.</span><span class="sxs-lookup"><span data-stu-id="af507-214">In this example, service package Guest1Pkg gets one core on hello cluster nodes where it is placed.</span></span>  <span data-ttu-id="af507-215">Minnesgränserna är absolut, så hello kodpaketet är begränsad too1024 MB minne (och en mjuk garanti reservation av hello samma).</span><span class="sxs-lookup"><span data-stu-id="af507-215">Memory limits are absolute, so hello code package is limited too1024 MB of memory (and a soft-guarantee reservation of hello same).</span></span> <span data-ttu-id="af507-216">Koden paket (behållare eller processer) är inte kan tooallocate mer minne än den här gränsen och försök toodo så resulterar i ett undantag i minnet är slut.</span><span class="sxs-lookup"><span data-stu-id="af507-216">Code packages (containers or processes) are not able tooallocate more memory than this limit, and attempting toodo so results in an out-of-memory exception.</span></span> <span data-ttu-id="af507-217">Resursen gränsen tvingande toowork har alla kod paket i ett tjänstepaket minnesgränserna som angetts.</span><span class="sxs-lookup"><span data-stu-id="af507-217">For resource limit enforcement toowork, all code packages within a service package should have memory limits specified.</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
  <Policies>
    <ServicePackageResourceGovernancePolicy CpuCores="1"/>
    <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
  </Policies>
</ServiceManifestImport>
```

## <a name="deploy-hello-container-application"></a><span data-ttu-id="af507-218">Distribuera programmet hello</span><span class="sxs-lookup"><span data-stu-id="af507-218">Deploy hello container application</span></span>
<span data-ttu-id="af507-219">Spara dina ändringar och skapa hello program.</span><span class="sxs-lookup"><span data-stu-id="af507-219">Save all your changes and build hello application.</span></span> <span data-ttu-id="af507-220">toopublish ditt program, högerklicka på **MyFirstContainer** i Solution Explorer och markera **publicera**.</span><span class="sxs-lookup"><span data-stu-id="af507-220">toopublish your application, right-click on **MyFirstContainer** in Solution Explorer and select **Publish**.</span></span>

<span data-ttu-id="af507-221">I **Anslutningens slutpunkt**, ange hello hanteringsslutpunkten för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="af507-221">In **Connection Endpoint**, enter hello management endpoint for hello cluster.</span></span>  <span data-ttu-id="af507-222">Till exempel "containercluster.westus2.cloudapp.azure.com:19000".</span><span class="sxs-lookup"><span data-stu-id="af507-222">For example, "containercluster.westus2.cloudapp.azure.com:19000".</span></span> <span data-ttu-id="af507-223">Du kan hitta hello klientanslutning slutpunkt i hello översikt bladet för klustret i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="af507-223">You can find hello client connection endpoint in hello Overview blade for your cluster in hello [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="af507-224">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="af507-224">Click **Publish**.</span></span>

<span data-ttu-id="af507-225">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) är ett webbaserat verktyg för att granska och hantera appar och noder i ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="af507-225">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a web-based tool for inspecting and managing applications and nodes in a Service Fabric cluster.</span></span> <span data-ttu-id="af507-226">Öppna en webbläsare och gå toohttp://containercluster.westus2.cloudapp.azure.com:19080 Explorer- och följ hello programdistribution.</span><span class="sxs-lookup"><span data-stu-id="af507-226">Open a browser and navigate toohttp://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ and follow hello application deployment.</span></span>  <span data-ttu-id="af507-227">hello programmet distribuerar men är i ett feltillstånd tills hello avbildningen hämtas på hello klusternoder (vilket kan ta en stund, beroende på hello bildstorleken): ![fel][1]</span><span class="sxs-lookup"><span data-stu-id="af507-227">hello application deploys but is in an error state until hello image is downloaded on hello cluster nodes (which can take some time, depending on hello image size): ![Error][1]</span></span>

<span data-ttu-id="af507-228">hello programmet är klart när den är i ```Ready``` tillstånd: ![redo][2]</span><span class="sxs-lookup"><span data-stu-id="af507-228">hello application is ready when it's in ```Ready``` state: ![Ready][2]</span></span>

<span data-ttu-id="af507-229">Öppna en webbläsare och gå toohttp://containercluster.westus2.cloudapp.azure.com:8081.</span><span class="sxs-lookup"><span data-stu-id="af507-229">Open a browser and navigate toohttp://containercluster.westus2.cloudapp.azure.com:8081.</span></span> <span data-ttu-id="af507-230">Du bör se hello rubriken ”Hello World”!</span><span class="sxs-lookup"><span data-stu-id="af507-230">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="af507-231">Visa i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="af507-231">display in hello browser.</span></span>

## <a name="clean-up"></a><span data-ttu-id="af507-232">Rensa</span><span class="sxs-lookup"><span data-stu-id="af507-232">Clean up</span></span>
<span data-ttu-id="af507-233">Du kan fortsätta tooincur avgifter medan hello klustret körs bör du överväga att [tar bort klustret](service-fabric-get-started-azure-cluster.md#remove-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="af507-233">You continue tooincur charges while hello cluster is running, consider [deleting your cluster](service-fabric-get-started-azure-cluster.md#remove-the-cluster).</span></span>  <span data-ttu-id="af507-234">[Party-kluster](http://tryazureservicefabric.westus.cloudapp.azure.com/) tas bort automatiskt efter ett par timmar.</span><span class="sxs-lookup"><span data-stu-id="af507-234">[Party clusters](http://tryazureservicefabric.westus.cloudapp.azure.com/) are automatically deleted after a few hours.</span></span>

<span data-ttu-id="af507-235">När du trycker på hello avbildningen toohello behållare registret kan du ta bort hello lokal image på utvecklingsdatorn:</span><span class="sxs-lookup"><span data-stu-id="af507-235">After you push hello image toohello container registry you can delete hello local image from your development computer:</span></span>

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a><span data-ttu-id="af507-236">Komplett exempel på Service Fabric-app och tjänstmanifest</span><span class="sxs-lookup"><span data-stu-id="af507-236">Complete example Service Fabric application and service manifests</span></span>
<span data-ttu-id="af507-237">Här är klar hello-tjänsten och applikationsmanifest som används i denna artikel.</span><span class="sxs-lookup"><span data-stu-id="af507-237">Here are hello complete service and application manifests used in this article.</span></span>

### <a name="servicemanifestxml"></a><span data-ttu-id="af507-238">ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="af507-238">ServiceManifest.xml</span></span>
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
### <a name="applicationmanifestxml"></a><span data-ttu-id="af507-239">ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="af507-239">ApplicationManifest.xml</span></span>
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

## <a name="configure-time-interval-before-container-is-force-terminated"></a><span data-ttu-id="af507-240">Ställ in tidsintervall innan behållaren tvångsavslutas</span><span class="sxs-lookup"><span data-stu-id="af507-240">Configure time interval before container is force terminated</span></span>

<span data-ttu-id="af507-241">Du kan konfigurera ett tidsintervall för hello runtime toowait innan hello behållaren tas bort när hello tjänsten tas bort (eller en flytta tooanother nod) har startats.</span><span class="sxs-lookup"><span data-stu-id="af507-241">You can configure a time interval for hello runtime toowait before hello container is removed after hello service deletion (or a move tooanother node) has started.</span></span> <span data-ttu-id="af507-242">Konfigurera hello tidsintervall skickar hello `docker stop <time in seconds>` kommandot toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="af507-242">Configuring hello time interval sends hello `docker stop <time in seconds>` command toohello container.</span></span>   <span data-ttu-id="af507-243">Mer information finns i [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span><span class="sxs-lookup"><span data-stu-id="af507-243">For more detail, see [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span></span> <span data-ttu-id="af507-244">hello tidsintervall toowait anges under hello `Hosting` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="af507-244">hello time interval toowait is specified under hello `Hosting` section.</span></span> <span data-ttu-id="af507-245">hello följande klustret manifestet fragment visar hur tooset hello vänta intervall:</span><span class="sxs-lookup"><span data-stu-id="af507-245">hello following cluster manifest snippet shows how tooset hello wait interval:</span></span>

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
<span data-ttu-id="af507-246">hello standard tidsintervall har angetts too10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="af507-246">hello default time interval is set too10 seconds.</span></span> <span data-ttu-id="af507-247">Eftersom den här konfigurationen är dynamiska, en config endast uppgradera hello klustret uppdateringar hello tidsgränsen uppnåddes.</span><span class="sxs-lookup"><span data-stu-id="af507-247">Since this configuration is dynamic, a config only upgrade on hello cluster updates hello timeout.</span></span> 


## <a name="configure-hello-runtime-tooremove-unused-container-images"></a><span data-ttu-id="af507-248">Konfigurera hello runtime tooremove oanvända behållaren bilder</span><span class="sxs-lookup"><span data-stu-id="af507-248">Configure hello runtime tooremove unused container images</span></span>

<span data-ttu-id="af507-249">Du kan konfigurera hello Service Fabric-kluster tooremove oanvända behållaren bilder från hello-nod.</span><span class="sxs-lookup"><span data-stu-id="af507-249">You can configure hello Service Fabric cluster tooremove unused container images from hello node.</span></span> <span data-ttu-id="af507-250">Den här konfigurationen kan disk space toobe återfångats om för många behållare avbildningar finns på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="af507-250">This configuration allows disk space toobe recaptured if too many container images are present on hello node.</span></span>  <span data-ttu-id="af507-251">tooenable den här funktionen, uppdatering hello `Hosting` avsnittet i hello klustermanifestet som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="af507-251">tooenable this feature, update hello `Hosting` section in hello cluster manifest as shown in hello following snippet:</span></span> 


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

<span data-ttu-id="af507-252">För bilder som inte ska tas bort, kan du ange dem under hello `ContainerImagesToSkip` parameter.</span><span class="sxs-lookup"><span data-stu-id="af507-252">For images that should not be deleted, you can specify them under hello `ContainerImagesToSkip` parameter.</span></span> 



## <a name="next-steps"></a><span data-ttu-id="af507-253">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af507-253">Next steps</span></span>
* <span data-ttu-id="af507-254">Mer information om hur du kör [behållare i Service Fabric](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="af507-254">Learn more about running [containers on Service Fabric](service-fabric-containers-overview.md).</span></span>
* <span data-ttu-id="af507-255">Läs hello [distribuera ett .NET-program i en behållare](service-fabric-host-app-in-a-container.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="af507-255">Read hello [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) tutorial.</span></span>
* <span data-ttu-id="af507-256">Lär dig mer om hello Service Fabric [programmet livscykel](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="af507-256">Learn about hello Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
* <span data-ttu-id="af507-257">Utcheckning hello [kodexempel för Service Fabric-behållaren](https://github.com/Azure-Samples/service-fabric-dotnet-containers) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="af507-257">Checkout hello [Service Fabric container code samples](https://github.com/Azure-Samples/service-fabric-dotnet-containers) on GitHub.</span></span>

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png
