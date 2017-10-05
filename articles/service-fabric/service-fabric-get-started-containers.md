---
title: "Skapa en Azure Service Fabric-behållarapp | Microsoft Docs"
description: "Skapa din första Windows-behållarapp på Azure Service Fabric.  Skapa en Docker-avbildning med en Python-app, överför avbildningen till ett behållarregister och skapa och distribuera en Service Fabric-behållarapp."
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
ms.openlocfilehash: 025bde02b3f342ec3399d51819d1fa8a91f11374
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-your-first-service-fabric-container-application-on-windows"></a><span data-ttu-id="4918f-104">Skapa din första Service Fabric-behållarapp i Windows</span><span class="sxs-lookup"><span data-stu-id="4918f-104">Create your first Service Fabric container application on Windows</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4918f-105">Windows</span><span class="sxs-lookup"><span data-stu-id="4918f-105">Windows</span></span>](service-fabric-get-started-containers.md)
> * [<span data-ttu-id="4918f-106">Linux</span><span class="sxs-lookup"><span data-stu-id="4918f-106">Linux</span></span>](service-fabric-get-started-containers-linux.md)

<span data-ttu-id="4918f-107">Du behöver inga göra några ändringar i din app för att köra en befintlig app i en Windows-behållare i ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="4918f-107">Running an existing application in a Windows container on a Service Fabric cluster doesn't require any changes to your application.</span></span> <span data-ttu-id="4918f-108">Den här artikeln vägleder dig genom att skapa en Docker-avbildning som innehåller ett Python [Flask](http://flask.pocoo.org/)-program och distribuera den till ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="4918f-108">This article walks you through creating a Docker image containing a Python [Flask](http://flask.pocoo.org/) web application and deploying it to a Service Fabric cluster.</span></span>  <span data-ttu-id="4918f-109">Du kan också dela programmet via [Azure Container-registret](/azure/container-registry/).</span><span class="sxs-lookup"><span data-stu-id="4918f-109">You will also share your containerized application through [Azure Container Registry](/azure/container-registry/).</span></span>  <span data-ttu-id="4918f-110">Den här artikeln förutsätter att du har grundläggande kunskaper om Docker.</span><span class="sxs-lookup"><span data-stu-id="4918f-110">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="4918f-111">Mer information om Docker finns i [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Översikt över Docker).</span><span class="sxs-lookup"><span data-stu-id="4918f-111">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4918f-112">Krav</span><span class="sxs-lookup"><span data-stu-id="4918f-112">Prerequisites</span></span>
<span data-ttu-id="4918f-113">En utvecklingsdator som kör:</span><span class="sxs-lookup"><span data-stu-id="4918f-113">A development computer running:</span></span>
* <span data-ttu-id="4918f-114">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="4918f-114">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="4918f-115">[Service Fabric SDK och verktyg](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4918f-115">[Service Fabric SDK and tools](service-fabric-get-started.md).</span></span>
*  <span data-ttu-id="4918f-116">Docker för Windows.</span><span class="sxs-lookup"><span data-stu-id="4918f-116">Docker for Windows.</span></span>  <span data-ttu-id="4918f-117">[Hämta Docker CE för Windows (stabil)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description).</span><span class="sxs-lookup"><span data-stu-id="4918f-117">[Get Docker CE for Windows (stable)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description).</span></span> <span data-ttu-id="4918f-118">Efter installationen startar du Docker, högerklickar på ikonen för fack och väljer **Switch to Windows containers** (Växla till Windows-behållare).</span><span class="sxs-lookup"><span data-stu-id="4918f-118">After installing and starting Docker, right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="4918f-119">Detta krävs för att köra Docker-avbildningar baserade på Windows.</span><span class="sxs-lookup"><span data-stu-id="4918f-119">This is required to run Docker images based on Windows.</span></span>

<span data-ttu-id="4918f-120">Ett Windows-kluster med tre eller fler noder som kör Windows Server 2016 med behållare – [Skapa ett kluster](service-fabric-cluster-creation-via-portal.md) eller [prova Service Fabric utan kostnad](https://aka.ms/tryservicefabric).</span><span class="sxs-lookup"><span data-stu-id="4918f-120">A Windows cluster with three or more nodes running on Windows Server 2016 with Containers- [Create a cluster](service-fabric-cluster-creation-via-portal.md) or [try Service Fabric for free](https://aka.ms/tryservicefabric).</span></span>

<span data-ttu-id="4918f-121">Ett register i Azure Container Registry – [Skapa ett behållarregister](../container-registry/container-registry-get-started-portal.md) i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4918f-121">A registry in Azure Container Registry - [Create a container registry](../container-registry/container-registry-get-started-portal.md) in your Azure subscription.</span></span>

## <a name="define-the-docker-container"></a><span data-ttu-id="4918f-122">Definiera dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="4918f-122">Define the Docker container</span></span>
<span data-ttu-id="4918f-123">Skapa en avbildning baserat på [Python-avbildningen](https://hub.docker.com/_/python/) på Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="4918f-123">Build an image based on the [Python image](https://hub.docker.com/_/python/) located on Docker Hub.</span></span>

<span data-ttu-id="4918f-124">Definiera Docker-behållaren i en Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="4918f-124">Define your Docker container in a Dockerfile.</span></span> <span data-ttu-id="4918f-125">Dockerfile innehåller instruktioner för att ställa in miljön i di behållare, läsa in programmet som du vill köra och mappa portar.</span><span class="sxs-lookup"><span data-stu-id="4918f-125">The Dockerfile contains instructions for setting up the environment inside your container, loading the application you want to run, and mapping ports.</span></span> <span data-ttu-id="4918f-126">Dockerfile är indata för kommandot `docker build` som skapar avbildningen.</span><span class="sxs-lookup"><span data-stu-id="4918f-126">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span>

<span data-ttu-id="4918f-127">Skapa en tom katalog och skapa filen *Dockerfile* (utan filtillägget).</span><span class="sxs-lookup"><span data-stu-id="4918f-127">Create an empty directory and create the file *Dockerfile* (with no file extension).</span></span> <span data-ttu-id="4918f-128">Lägg till följande i *Dockerfile* och spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="4918f-128">Add the following to *Dockerfile* and save your changes:</span></span>

```
# Use an official Python runtime as a base image
FROM python:2.7-windowsservercore

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

<span data-ttu-id="4918f-129">Läs [Dockerfile-referensen](https://docs.docker.com/engine/reference/builder/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="4918f-129">Read the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more information.</span></span>

## <a name="create-a-simple-web-application"></a><span data-ttu-id="4918f-130">Skapa en enkel webbapp</span><span class="sxs-lookup"><span data-stu-id="4918f-130">Create a simple web application</span></span>
<span data-ttu-id="4918f-131">Skapa en flask-webbapplikation som lyssnar på port 80 och returnerar ”Hello World”!.</span><span class="sxs-lookup"><span data-stu-id="4918f-131">Create a Flask web application listening on port 80 that returns "Hello World!".</span></span>  <span data-ttu-id="4918f-132">Skapa filen *requirements.txt* i samma katalog.</span><span class="sxs-lookup"><span data-stu-id="4918f-132">In the same directory, create the file *requirements.txt*.</span></span>  <span data-ttu-id="4918f-133">Lägg till följande och spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="4918f-133">Add the following and save your changes:</span></span>
```
Flask
```

<span data-ttu-id="4918f-134">Skapa även filen *app.py* och lägg till följande:</span><span class="sxs-lookup"><span data-stu-id="4918f-134">Also create the *app.py* file and add the following:</span></span>

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
## <a name="build-the-image"></a><span data-ttu-id="4918f-135">Skapa avbildningen</span><span class="sxs-lookup"><span data-stu-id="4918f-135">Build the image</span></span>
<span data-ttu-id="4918f-136">Kör kommandot `docker build` för att skapa avbildningen som kör ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4918f-136">Run the `docker build` command to create the image that runs your web application.</span></span> <span data-ttu-id="4918f-137">Öppna ett PowerShell-fönster och navigera till den katalog som innehåller din Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="4918f-137">Open a PowerShell window and navigate to the directory containing the Dockerfile.</span></span> <span data-ttu-id="4918f-138">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4918f-138">Run the following command:</span></span>

```
docker build -t helloworldapp .
```

<span data-ttu-id="4918f-139">Med det här kommandot skapas den nya avbildningen med hjälp av instruktionerna i din Dockerfile och avbildningen får namnet "helloworldapp" (-t-taggning).</span><span class="sxs-lookup"><span data-stu-id="4918f-139">This command builds the new image using the instructions in your Dockerfile, naming (-t tagging) the image "helloworldapp".</span></span> <span data-ttu-id="4918f-140">Genom att skapa en avbildning hämtas basavbildningen från Docker Hub och en ny avbildning skapas som lägger till ditt program ovanpå basavbildningen.</span><span class="sxs-lookup"><span data-stu-id="4918f-140">Building an image pulls the base image down from Docker Hub and creates a new image that adds your application on top of the base image.</span></span>  

<span data-ttu-id="4918f-141">När build-kommandot har slutförts kör du `docker images`-kommandot för att se information om den nya avbildningen:</span><span class="sxs-lookup"><span data-stu-id="4918f-141">Once the build command completes, run the `docker images` command to see information on the new image:</span></span>

```
$ docker images

REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-the-application-locally"></a><span data-ttu-id="4918f-142">Kör programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="4918f-142">Run the application locally</span></span>
<span data-ttu-id="4918f-143">Kontrollera avbildningen lokalt innan du överför den till behållarregistret.</span><span class="sxs-lookup"><span data-stu-id="4918f-143">Verify your image locally before pushing it the container registry.</span></span>  

<span data-ttu-id="4918f-144">Kör programmet:</span><span class="sxs-lookup"><span data-stu-id="4918f-144">Run the application:</span></span>

```
docker run -d --name my-web-site helloworldapp
```

<span data-ttu-id="4918f-145">*name* namnger den behållare som körs (i stället för behållar-ID:t).</span><span class="sxs-lookup"><span data-stu-id="4918f-145">*name* gives a name to the running container (instead of the container ID).</span></span>

<span data-ttu-id="4918f-146">När behållaren har startat letar du reda på dess IP-adress så att du kan ansluta till den behållare som körs via en webbläsare:</span><span class="sxs-lookup"><span data-stu-id="4918f-146">Once the container starts, find its IP address so that you can connect to your running container from a browser:</span></span>
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

<span data-ttu-id="4918f-147">Anslut till den behållare som körs.</span><span class="sxs-lookup"><span data-stu-id="4918f-147">Connect to the running container.</span></span>  <span data-ttu-id="4918f-148">Öppna en webbläsare som pekar på den returnerade IP-adressen, till exempel ”http://172.31.194.61”.</span><span class="sxs-lookup"><span data-stu-id="4918f-148">Open a web browser pointing to the IP address returned, for example "http://172.31.194.61".</span></span> <span data-ttu-id="4918f-149">Nu visas normalt rubriken "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="4918f-149">You should see the heading "Hello World!"</span></span> <span data-ttu-id="4918f-150">i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="4918f-150">display in the browser.</span></span>

<span data-ttu-id="4918f-151">Om du vill stoppa behållaren kör du:</span><span class="sxs-lookup"><span data-stu-id="4918f-151">To stop your container, run:</span></span>

```
docker stop my-web-site
```

<span data-ttu-id="4918f-152">Ta bort behållaren från utvecklingsdatorn:</span><span class="sxs-lookup"><span data-stu-id="4918f-152">Delete the container from your development machine:</span></span>

```
docker rm my-web-site
```

<a id="Push-Containers"></a>
## <a name="push-the-image-to-the-container-registry"></a><span data-ttu-id="4918f-153">Överför avbildningen till behållarregistret</span><span class="sxs-lookup"><span data-stu-id="4918f-153">Push the image to the container registry</span></span>
<span data-ttu-id="4918f-154">När du har kontrollerat att behållaren körs på utvecklingsdatorn överför du avbildningen till registret i Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="4918f-154">After you verify that the container runs on your development machine, push the image to your registry in Azure Container Registry.</span></span>

<span data-ttu-id="4918f-155">Kör ``docker login`` för att logga in till behållarregistret med dina [autentiseringsuppgifter för registret](../container-registry/container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="4918f-155">Run ``docker login`` to log in to your container registry with your [registry credentials](../container-registry/container-registry-authentication.md).</span></span>

<span data-ttu-id="4918f-156">I följande exempel skickas ID:t och lösenordet för ett Azure Active Directory [-tjänstobjekt](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="4918f-156">The following example passes the ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="4918f-157">Du kanske till exempel har tilldelat ett tjänstobjekt till registret för ett automatiseringsscenario.</span><span class="sxs-lookup"><span data-stu-id="4918f-157">For example, you might have assigned a service principal to your registry for an automation scenario.</span></span> <span data-ttu-id="4918f-158">Du kan också logga in med ditt användarnamn och lösenord för registret.</span><span class="sxs-lookup"><span data-stu-id="4918f-158">Or, you could login using your registry username and password.</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="4918f-159">Följande kommando skapar en tagg, eller ett alias, för avbildningen, med en fullständigt kvalificerad sökväg till registret.</span><span class="sxs-lookup"><span data-stu-id="4918f-159">The following command creates a tag, or alias, of the image, with a fully qualified path to your registry.</span></span> <span data-ttu-id="4918f-160">I det här exemplet placeras avbildningen i ```samples```-namnområdet för att undvika oreda i registrets rot.</span><span class="sxs-lookup"><span data-stu-id="4918f-160">This example places the image in the ```samples``` namespace to avoid clutter in the root of the registry.</span></span>

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

<span data-ttu-id="4918f-161">Överför avbildningen till behållarregistret:</span><span class="sxs-lookup"><span data-stu-id="4918f-161">Push the image to your container registry:</span></span>

```
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="create-the-containerized-service-in-visual-studio"></a><span data-ttu-id="4918f-162">Skapa behållartjänsten i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4918f-162">Create the containerized service in Visual Studio</span></span>
<span data-ttu-id="4918f-163">SDK:en och verktygen för Service Fabric innehåller en tjänstmall som hjälper dig att skapa ett behållarprogram.</span><span class="sxs-lookup"><span data-stu-id="4918f-163">The Service Fabric SDK and tools provide a service template to help you create a containerized application.</span></span>

1. <span data-ttu-id="4918f-164">Starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4918f-164">Start Visual Studio.</span></span>  <span data-ttu-id="4918f-165">Välj **Arkiv** > **Nytt** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="4918f-165">Select **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="4918f-166">Välj **Service Fabric-programmet**, ge det namnet "MyFirstContainer" och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4918f-166">Select **Service Fabric application**, name it "MyFirstContainer", and click **OK**.</span></span>
3. <span data-ttu-id="4918f-167">Välj **Guest Container** (Gästbehållare) i listan med **tjänstmallar**.</span><span class="sxs-lookup"><span data-stu-id="4918f-167">Select **Guest Container** from the list of **service templates**.</span></span>
4. <span data-ttu-id="4918f-168">I **Avbildningsnamn** skriver du "myregistry.azurecr.io/samples/helloworldapp" (den avbildning som du skickade till lagringsplatsen för behållaren).</span><span class="sxs-lookup"><span data-stu-id="4918f-168">In **Image Name** enter "myregistry.azurecr.io/samples/helloworldapp", the image you pushed to your container repository.</span></span>
5. <span data-ttu-id="4918f-169">Namnge tjänsten och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4918f-169">Give your service a name, and click **OK**.</span></span>

## <a name="configure-communication"></a><span data-ttu-id="4918f-170">Konfigurera kommunikation</span><span class="sxs-lookup"><span data-stu-id="4918f-170">Configure communication</span></span>
<span data-ttu-id="4918f-171">Behållartjänsten behöver en slutpunkt för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="4918f-171">The containerized service needs an endpoint for communication.</span></span> <span data-ttu-id="4918f-172">Lägg till ett `Endpoint`-element med protokollet, porten och typen i filen ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="4918f-172">Add an `Endpoint` element with the protocol, port, and type to the ServiceManifest.xml file.</span></span> <span data-ttu-id="4918f-173">Behållartjänsten för den här artikeln lyssnar på port 8081.</span><span class="sxs-lookup"><span data-stu-id="4918f-173">For this article, the containerized service listens on port 8081.</span></span>  <span data-ttu-id="4918f-174">I det här exemplet används en fast port, 8081.</span><span class="sxs-lookup"><span data-stu-id="4918f-174">In this example, a fixed port 8081 is used.</span></span>  <span data-ttu-id="4918f-175">Om ingen port har angetts väljs en port slumpmässigt från portintervallet för programmet.</span><span class="sxs-lookup"><span data-stu-id="4918f-175">If no port is specified, a random port from the application port range is chosen.</span></span>  

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
  </Endpoints>
</Resources>
```

<span data-ttu-id="4918f-176">Genom att definiera en slutpunkt publicerar Service Fabric slutpunkten i namngivningstjänsten.</span><span class="sxs-lookup"><span data-stu-id="4918f-176">By defining an endpoint, Service Fabric publishes the endpoint to the Naming service.</span></span>  <span data-ttu-id="4918f-177">Andra tjänster som körs i klustret kan lösa den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="4918f-177">Other services running in the cluster can resolve this container.</span></span>  <span data-ttu-id="4918f-178">Du kan också utföra kommunikation mellan behållare med hjälp av den [omvända proxyn](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="4918f-178">You can also perform container-to-container communication using the [reverse proxy](service-fabric-reverseproxy.md).</span></span>  <span data-ttu-id="4918f-179">Du utför kommunikation genom att tillhandahålla HTTP-lyssningsporten för den omvända proxyn och namnet på de tjänster som du vill kommunicera med som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="4918f-179">Communication is performed by providing the reverse proxy HTTP listening port and the name of the services that you want to communicate with as environment variables.</span></span>

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="4918f-180">Konfigurera och ange miljövariabler</span><span class="sxs-lookup"><span data-stu-id="4918f-180">Configure and set environment variables</span></span>
<span data-ttu-id="4918f-181">Miljövariabler kan anges för varje kodpaketet i tjänstmanifestet.</span><span class="sxs-lookup"><span data-stu-id="4918f-181">Environment variables can be specified for each code package in the service manifest.</span></span> <span data-ttu-id="4918f-182">Den här funktionen är tillgänglig för alla tjänster oavsett om de har distribueras som behållare eller processer, eller körbara gäster.</span><span class="sxs-lookup"><span data-stu-id="4918f-182">This feature is available for all services irrespective of whether they are deployed as containers or processes or guest executables.</span></span> <span data-ttu-id="4918f-183">Du kan åsidosätta värden för miljövariabler i applikationsmanifestet eller ange dem under distributionen som programparametrar.</span><span class="sxs-lookup"><span data-stu-id="4918f-183">You can override environment variable values in the application manifest or specify them during deployment as application parameters.</span></span>

<span data-ttu-id="4918f-184">Följande XML-kodfragment i tjänstmanifestet visar ett exempel på hur du anger miljövariabler för ett kodpaket:</span><span class="sxs-lookup"><span data-stu-id="4918f-184">The following service manifest XML snippet shows an example of how to specify environment variables for a code package:</span></span>
```xml
<CodePackage Name="Code" Version="1.0.0">
  ...
  <EnvironmentVariables>
    <EnvironmentVariable Name="HttpGatewayPort" Value=""/>    
  </EnvironmentVariables>
</CodePackage>
```

<span data-ttu-id="4918f-185">Dessa miljövariabler kan åsidosättas i applikationsmanifestet:</span><span class="sxs-lookup"><span data-stu-id="4918f-185">These environment variables can be overridden in the application manifest:</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
  </EnvironmentOverrides>
  ...
</ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping-and-container-to-container-discovery"></a><span data-ttu-id="4918f-186">Konfigurera mappning mellan behållarport och värdport och identifiering mellan behållare</span><span class="sxs-lookup"><span data-stu-id="4918f-186">Configure container port-to-host port mapping and container-to-container discovery</span></span>
<span data-ttu-id="4918f-187">Konfigurera en värdport som används för att kommunicera med behållaren.</span><span class="sxs-lookup"><span data-stu-id="4918f-187">Configure a host port used to communicate  with the container.</span></span> <span data-ttu-id="4918f-188">Portbindningen mappar porten där tjänsten lyssnar inuti behållaren till en port på värden.</span><span class="sxs-lookup"><span data-stu-id="4918f-188">The port binding maps the port on which the service is listening inside the container to a port on the host.</span></span> <span data-ttu-id="4918f-189">Lägg till ett `PortBinding`-element i `ContainerHostPolicies`-elementet i filen ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="4918f-189">Add a `PortBinding` element in `ContainerHostPolicies` element of the ApplicationManifest.xml file.</span></span>  <span data-ttu-id="4918f-190">I den här artikeln är `ContainerPort` 80 (behållaren exponerar port 80, som anges i Dockerfile) och `EndpointRef` är ”Guest1TypeEndpoint” (slutpunkten som tidigare definierats i tjänstmanifestet).</span><span class="sxs-lookup"><span data-stu-id="4918f-190">For this article, `ContainerPort` is 80 (the container exposes port 80, as specified in the Dockerfile) and `EndpointRef` is "Guest1TypeEndpoint" (the endpoint previously defined in the service manifest).</span></span>  <span data-ttu-id="4918f-191">Inkommande begäranden till tjänsten på port 8081 mappas till port 80 på behållaren.</span><span class="sxs-lookup"><span data-stu-id="4918f-191">Incoming requests to the service on port 8081 are mapped to port 80 on the container.</span></span>

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-container-registry-authentication"></a><span data-ttu-id="4918f-192">Konfigurera autentisering av behållarregister</span><span class="sxs-lookup"><span data-stu-id="4918f-192">Configure container registry authentication</span></span>
<span data-ttu-id="4918f-193">Konfigurera autentisering av behållarregister genom att lägga till `RepositoryCredentials` i `ContainerHostPolicies` filen ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="4918f-193">Configure container registry authentication by adding `RepositoryCredentials` to `ContainerHostPolicies` of the ApplicationManifest.xml file.</span></span> <span data-ttu-id="4918f-194">Lägg till kontot och lösenordet för behållarregistret myregistry.azurecr.io, så att tjänsten kan ladda ned behållaravbildningen från centrallagret.</span><span class="sxs-lookup"><span data-stu-id="4918f-194">Add the account and password for the myregistry.azurecr.io container registry, which allows the service to download the container image from the repository.</span></span>

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

<span data-ttu-id="4918f-195">Vi rekommenderar att du krypterar centrallagrets lösenord genom att använda ett chiffreringscertifikat som distribueras till alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="4918f-195">We recommend that you encrypt the repository password by using an encipherment certificate that's deployed to all nodes of the cluster.</span></span> <span data-ttu-id="4918f-196">När Service Fabric distribuerar tjänstpaketet till klustret används chiffreringscertifikatet för att avkryptera chiffertexten.</span><span class="sxs-lookup"><span data-stu-id="4918f-196">When Service Fabric deploys the service package to the cluster, the encipherment certificate is used to decrypt the cipher text.</span></span>  <span data-ttu-id="4918f-197">Cmdleten Invoke-ServiceFabricEncryptText används för att skapa chiffertexten för lösenordet, som läggs till i filen ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="4918f-197">The Invoke-ServiceFabricEncryptText cmdlet is used to create the cipher text for the password, which is added to the ApplicationManifest.xml file.</span></span>

<span data-ttu-id="4918f-198">Följande skript skapar ett nytt självsignerat certifikat och exporterar det till en PFX-fil.</span><span class="sxs-lookup"><span data-stu-id="4918f-198">The following script creates a new self-signed certificate and exports it to a PFX file.</span></span>  <span data-ttu-id="4918f-199">Certifikatet importeras till ett befintligt nyckelvalv och distribueras sedan till Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="4918f-199">The certificate is imported into an existing key vault and then deployed to the Service Fabric cluster.</span></span>

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

# Create a self signed cert, export to PFX file.
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject $subjectname -Provider 'Microsoft Enhanced Cryptographic Provider v1.0' `
| Export-PfxCertificate -FilePath $filepath -Password $certpwd

# Import the certificate to an existing key vault.  The key vault must be enabled for deployment.
$cer = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certificateName -FilePath $filepath -Password $certpwd

Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $groupname -EnabledForDeployment

# Add the certificate to all the VMs in the cluster.
Add-AzureRmServiceFabricApplicationCertificate -ResourceGroupName $groupname -Name $clustername -SecretIdentifier $cer.SecretId
```
<span data-ttu-id="4918f-200">Kryptera lösenordet med cmdleten [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="4918f-200">Encrypt the password using the [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet.</span></span>

```powershell
$text = "=P==/==/=8=/=+u4lyOB=+=nWzEeRfF="
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint $cer.Thumbprint -Text $text -StoreLocation Local -StoreName My
```

<span data-ttu-id="4918f-201">Ersätt lösenordet med chiffertexten som returneras av cmdleten [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) och ange `PasswordEncrypted` ”true”.</span><span class="sxs-lookup"><span data-stu-id="4918f-201">Replace the password with the cipher text returned by the [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet and set `PasswordEncrypted` to "true".</span></span>

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

## <a name="configure-isolation-mode"></a><span data-ttu-id="4918f-202">Konfigurera isoleringsläge</span><span class="sxs-lookup"><span data-stu-id="4918f-202">Configure isolation mode</span></span>
<span data-ttu-id="4918f-203">Windows stöder två isoleringslägen för behållare: process och Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="4918f-203">Windows supports two isolation modes for containers: process and Hyper-V.</span></span> <span data-ttu-id="4918f-204">Om processisoleringsläget används delar alla behållare som körs på samma värddator kärna med värden.</span><span class="sxs-lookup"><span data-stu-id="4918f-204">With the process isolation mode, all the containers running on the same host machine share the kernel with the host.</span></span> <span data-ttu-id="4918f-205">Om Hyper-V-isoleringsläget används isoleras kärnorna mellan varje Hyper-V-behållare och behållarvärden.</span><span class="sxs-lookup"><span data-stu-id="4918f-205">With the Hyper-V isolation mode, the kernels are isolated between each Hyper-V container and the container host.</span></span> <span data-ttu-id="4918f-206">Isoleringsläget anges i `ContainerHostPolicies`-elementet i applikationsmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="4918f-206">The isolation mode is specified in the `ContainerHostPolicies` element in the application manifest file.</span></span> <span data-ttu-id="4918f-207">Isoleringslägena som kan anges är `process`, `hyperv` och `default`.</span><span class="sxs-lookup"><span data-stu-id="4918f-207">The isolation modes that can be specified are `process`, `hyperv`, and `default`.</span></span> <span data-ttu-id="4918f-208">Standardinställningen för standardisoleringsläget är `process` på Windows Server-värddatorer och `hyperv` på Windows 10-värddatorer.</span><span class="sxs-lookup"><span data-stu-id="4918f-208">The default isolation mode defaults to `process` on Windows Server hosts, and defaults to `hyperv` on Windows 10 hosts.</span></span> <span data-ttu-id="4918f-209">Följande kodfragment visar hur isoleringsläget har angetts i applikationsmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="4918f-209">The following snippet shows how the isolation mode is specified in the application manifest file.</span></span>

```xml
<ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
```

## <a name="configure-resource-governance"></a><span data-ttu-id="4918f-210">Konfigurera resursstyrning</span><span class="sxs-lookup"><span data-stu-id="4918f-210">Configure resource governance</span></span>
<span data-ttu-id="4918f-211">Med [resursstyrning](service-fabric-resource-governance.md) begränsas resurserna som behållaren kan använda på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="4918f-211">[Resource governance](service-fabric-resource-governance.md) restricts the resources that the container can use on the host.</span></span> <span data-ttu-id="4918f-212">`ResourceGovernancePolicy`-elementet som anges i applikationsmanifestet, används för att deklarera resursgränser för ett tjänstkodpaket.</span><span class="sxs-lookup"><span data-stu-id="4918f-212">The `ResourceGovernancePolicy` element, which is specified in the application manifest, is used to declare resource limits for a service code package.</span></span> <span data-ttu-id="4918f-213">Resursgränser kan anges för följande resurser: Memory, MemorySwap, CpuShares (relativ processorvikt), MemoryReservationInMB, BlkioWeight (relativ BlockIO-vikt).</span><span class="sxs-lookup"><span data-stu-id="4918f-213">Resource limits can be set for the following resources: Memory, MemorySwap, CpuShares (CPU relative weight), MemoryReservationInMB, BlkioWeight (BlockIO relative weight).</span></span>  <span data-ttu-id="4918f-214">I det här exemplet hämtar tjänstpaketet Guest1Pkg en kärna på klusternoderna där det är placerat.</span><span class="sxs-lookup"><span data-stu-id="4918f-214">In this example, service package Guest1Pkg gets one core on the cluster nodes where it is placed.</span></span>  <span data-ttu-id="4918f-215">Minnesgränser är absoluta, så kodpaketet är begränsat till 1024 MB minne (med samma reservation).</span><span class="sxs-lookup"><span data-stu-id="4918f-215">Memory limits are absolute, so the code package is limited to 1024 MB of memory (and a soft-guarantee reservation of the same).</span></span> <span data-ttu-id="4918f-216">Kodpaket (behållare eller processer) kan inte tilldela mer minne än den här gränsen, och försök att göra detta leder till undantag utanför minnet.</span><span class="sxs-lookup"><span data-stu-id="4918f-216">Code packages (containers or processes) are not able to allocate more memory than this limit, and attempting to do so results in an out-of-memory exception.</span></span> <span data-ttu-id="4918f-217">För att tvingande resursbegränsning ska fungera bör minnesbegränsningar ha angetts för alla kodpaket inom ett tjänstpaket.</span><span class="sxs-lookup"><span data-stu-id="4918f-217">For resource limit enforcement to work, all code packages within a service package should have memory limits specified.</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
  <Policies>
    <ServicePackageResourceGovernancePolicy CpuCores="1"/>
    <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
  </Policies>
</ServiceManifestImport>
```

## <a name="deploy-the-container-application"></a><span data-ttu-id="4918f-218">Distribuera behållarappen</span><span class="sxs-lookup"><span data-stu-id="4918f-218">Deploy the container application</span></span>
<span data-ttu-id="4918f-219">Spara alla dina ändringar och skapa programmet.</span><span class="sxs-lookup"><span data-stu-id="4918f-219">Save all your changes and build the application.</span></span> <span data-ttu-id="4918f-220">Om du vill publicera appen högerklickar du på **MyFirstContainer** i Solution Explorer och väljer **Publish** (Publicera).</span><span class="sxs-lookup"><span data-stu-id="4918f-220">To publish your application, right-click on **MyFirstContainer** in Solution Explorer and select **Publish**.</span></span>

<span data-ttu-id="4918f-221">I **anslutningsslutpunkten** anger du hanteringsslutpunkten för klustret.</span><span class="sxs-lookup"><span data-stu-id="4918f-221">In **Connection Endpoint**, enter the management endpoint for the cluster.</span></span>  <span data-ttu-id="4918f-222">Till exempel "containercluster.westus2.cloudapp.azure.com:19000".</span><span class="sxs-lookup"><span data-stu-id="4918f-222">For example, "containercluster.westus2.cloudapp.azure.com:19000".</span></span> <span data-ttu-id="4918f-223">Slutpunkten för klientanslutningen finns på översiktsbladet för ditt kluster i [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4918f-223">You can find the client connection endpoint in the Overview blade for your cluster in the [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="4918f-224">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="4918f-224">Click **Publish**.</span></span>

<span data-ttu-id="4918f-225">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) är ett webbaserat verktyg för att granska och hantera appar och noder i ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="4918f-225">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a web-based tool for inspecting and managing applications and nodes in a Service Fabric cluster.</span></span> <span data-ttu-id="4918f-226">Öppna en webbläsare och gå till http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ och följ programdistributionen.</span><span class="sxs-lookup"><span data-stu-id="4918f-226">Open a browser and navigate to http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ and follow the application deployment.</span></span>  <span data-ttu-id="4918f-227">Appen distribueras men är i ett feltillstånd tills avbildningen har laddats ned på klusternoderna (vilket kan ta en stund beroende på avbildningens storlek): ![Fel][1]</span><span class="sxs-lookup"><span data-stu-id="4918f-227">The application deploys but is in an error state until the image is downloaded on the cluster nodes (which can take some time, depending on the image size): ![Error][1]</span></span>

<span data-ttu-id="4918f-228">Appen är klar när den har ```Ready```status: ![Ready][2] (Klar)</span><span class="sxs-lookup"><span data-stu-id="4918f-228">The application is ready when it's in ```Ready``` state: ![Ready][2]</span></span>

<span data-ttu-id="4918f-229">Öppna en webbläsare och navigera till http://containercluster.westus2.cloudapp.azure.com:8081.</span><span class="sxs-lookup"><span data-stu-id="4918f-229">Open a browser and navigate to http://containercluster.westus2.cloudapp.azure.com:8081.</span></span> <span data-ttu-id="4918f-230">Nu visas normalt rubriken "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="4918f-230">You should see the heading "Hello World!"</span></span> <span data-ttu-id="4918f-231">i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="4918f-231">display in the browser.</span></span>

## <a name="clean-up"></a><span data-ttu-id="4918f-232">Rensa</span><span class="sxs-lookup"><span data-stu-id="4918f-232">Clean up</span></span>
<span data-ttu-id="4918f-233">Det kostar pengar så länge klustret körs. Fundera på om du vill [ta bort klustret](service-fabric-get-started-azure-cluster.md#remove-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="4918f-233">You continue to incur charges while the cluster is running, consider [deleting your cluster](service-fabric-get-started-azure-cluster.md#remove-the-cluster).</span></span>  <span data-ttu-id="4918f-234">[Party-kluster](http://tryazureservicefabric.westus.cloudapp.azure.com/) tas bort automatiskt efter ett par timmar.</span><span class="sxs-lookup"><span data-stu-id="4918f-234">[Party clusters](http://tryazureservicefabric.westus.cloudapp.azure.com/) are automatically deleted after a few hours.</span></span>

<span data-ttu-id="4918f-235">När du har överfört avbildningen till behållarregistret kan du ta bort den lokala avbildningen från utvecklingsdatorn:</span><span class="sxs-lookup"><span data-stu-id="4918f-235">After you push the image to the container registry you can delete the local image from your development computer:</span></span>

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a><span data-ttu-id="4918f-236">Komplett exempel på Service Fabric-app och tjänstmanifest</span><span class="sxs-lookup"><span data-stu-id="4918f-236">Complete example Service Fabric application and service manifests</span></span>
<span data-ttu-id="4918f-237">Här är de fullständiga tjänst- och appmanifesten som används i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4918f-237">Here are the complete service and application manifests used in this article.</span></span>

### <a name="servicemanifestxml"></a><span data-ttu-id="4918f-238">ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="4918f-238">ServiceManifest.xml</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Guest1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="Guest1Type" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->    
    <EnvironmentVariables>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
    </EnvironmentVariables>

  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to
           listen. Please note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a><span data-ttu-id="4918f-239">ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="4918f-239">ApplicationManifest.xml</span></span>
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
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
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
    <!-- The section below creates instances of service types, when an instance of this
         application type is created. You can also create one or more instances of service type using the
         ServiceFabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Guest1">
      <StatelessService ServiceTypeName="Guest1Type" InstanceCount="[Guest1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="configure-time-interval-before-container-is-force-terminated"></a><span data-ttu-id="4918f-240">Ställ in tidsintervall innan behållaren tvångsavslutas</span><span class="sxs-lookup"><span data-stu-id="4918f-240">Configure time interval before container is force terminated</span></span>

<span data-ttu-id="4918f-241">Du kan ställa in ett tidsintervall för hur lång exekveringstid som ska gå innan behållaren tas bort när borttagning av tjänsten (eller flytt till en annan nod) har påbörjats.</span><span class="sxs-lookup"><span data-stu-id="4918f-241">You can configure a time interval for the runtime to wait before the container is removed after the service deletion (or a move to another node) has started.</span></span> <span data-ttu-id="4918f-242">När du ställer in ett tidsintervall skickas kommandot `docker stop <time in seconds>` till behållaren.</span><span class="sxs-lookup"><span data-stu-id="4918f-242">Configuring the time interval sends the `docker stop <time in seconds>` command to the container.</span></span>   <span data-ttu-id="4918f-243">Mer information finns i [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span><span class="sxs-lookup"><span data-stu-id="4918f-243">For more detail, see [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span></span> <span data-ttu-id="4918f-244">Tidsintervallet anges i avsnittet `Hosting`.</span><span class="sxs-lookup"><span data-stu-id="4918f-244">The time interval to wait is specified under the `Hosting` section.</span></span> <span data-ttu-id="4918f-245">I följande klustermanifestutdrag visas hur du ställer in väntetidsintervallet:</span><span class="sxs-lookup"><span data-stu-id="4918f-245">The following cluster manifest snippet shows how to set the wait interval:</span></span>

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
<span data-ttu-id="4918f-246">Standardtidsintervallet är inställt på 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="4918f-246">The default time interval is set to 10 seconds.</span></span> <span data-ttu-id="4918f-247">Eftersom inställningen är dynamisk uppdateras tidsgränsen med en konfigurationsuppdatering på klustret.</span><span class="sxs-lookup"><span data-stu-id="4918f-247">Since this configuration is dynamic, a config only upgrade on the cluster updates the timeout.</span></span> 


## <a name="configure-the-runtime-to-remove-unused-container-images"></a><span data-ttu-id="4918f-248">Ställ in exekveringstid för att ta bort behållaravbildningar som inte används</span><span class="sxs-lookup"><span data-stu-id="4918f-248">Configure the runtime to remove unused container images</span></span>

<span data-ttu-id="4918f-249">Du kan ställa in Service Fabric-klustret på att ta bort oanvända behållaravbildningar från noden.</span><span class="sxs-lookup"><span data-stu-id="4918f-249">You can configure the Service Fabric cluster to remove unused container images from the node.</span></span> <span data-ttu-id="4918f-250">Med den här inställningen kan du få tillbaka diskutrymme om det finns för många behållaravbildningar på noden.</span><span class="sxs-lookup"><span data-stu-id="4918f-250">This configuration allows disk space to be recaptured if too many container images are present on the node.</span></span>  <span data-ttu-id="4918f-251">Aktivera funktionen genom att uppdatera avsnittet `Hosting` i klustermanifestet enligt följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="4918f-251">To enable this feature, update the `Hosting` section in the cluster manifest as shown in the following snippet:</span></span> 


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

<span data-ttu-id="4918f-252">Avbildningar som inte ska raderas kan du ange under parametern `ContainerImagesToSkip`.</span><span class="sxs-lookup"><span data-stu-id="4918f-252">For images that should not be deleted, you can specify them under the `ContainerImagesToSkip` parameter.</span></span> 



## <a name="next-steps"></a><span data-ttu-id="4918f-253">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4918f-253">Next steps</span></span>
* <span data-ttu-id="4918f-254">Mer information om hur du kör [behållare i Service Fabric](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4918f-254">Learn more about running [containers on Service Fabric](service-fabric-containers-overview.md).</span></span>
* <span data-ttu-id="4918f-255">Läs kursen [Distribuera ett .NET-program i en behållare](service-fabric-host-app-in-a-container.md).</span><span class="sxs-lookup"><span data-stu-id="4918f-255">Read the [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) tutorial.</span></span>
* <span data-ttu-id="4918f-256">Läs om Service Fabric-[applivscykeln](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="4918f-256">Learn about the Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
* <span data-ttu-id="4918f-257">Se [kodexempel för Service Fabric-behållare](https://github.com/Azure-Samples/service-fabric-dotnet-containers) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="4918f-257">Checkout the [Service Fabric container code samples](https://github.com/Azure-Samples/service-fabric-dotnet-containers) on GitHub.</span></span>

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png
