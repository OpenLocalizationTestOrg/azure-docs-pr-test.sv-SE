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
# <a name="create-your-first-service-fabric-container-application-on-linux"></a><span data-ttu-id="66521-104">Skapa din första Service Fabric-behållarapp i Linux</span><span class="sxs-lookup"><span data-stu-id="66521-104">Create your first Service Fabric container application on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="66521-105">Windows</span><span class="sxs-lookup"><span data-stu-id="66521-105">Windows</span></span>](service-fabric-get-started-containers.md)
> * [<span data-ttu-id="66521-106">Linux</span><span class="sxs-lookup"><span data-stu-id="66521-106">Linux</span></span>](service-fabric-get-started-containers-linux.md)

<span data-ttu-id="66521-107">Du behöver inga göra några ändringar i din app för att köra en befintlig app i en Linux-behållare i ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="66521-107">Running an existing application in a Linux container on a Service Fabric cluster doesn't require any changes to your application.</span></span> <span data-ttu-id="66521-108">Den här artikeln vägleder dig genom att skapa en Docker-avbildning som innehåller ett Python [Flask](http://flask.pocoo.org/)-program och distribuera den till ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="66521-108">This article walks you through creating a Docker image containing a Python [Flask](http://flask.pocoo.org/) web application and deploying it to a Service Fabric cluster.</span></span>  <span data-ttu-id="66521-109">Du kan också dela programmet via [Azure Container-registret](/azure/container-registry/).</span><span class="sxs-lookup"><span data-stu-id="66521-109">You will also share your containerized application through [Azure Container Registry](/azure/container-registry/).</span></span>  <span data-ttu-id="66521-110">Den här artikeln förutsätter att du har grundläggande kunskaper om Docker.</span><span class="sxs-lookup"><span data-stu-id="66521-110">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="66521-111">Mer information om Docker finns i [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Översikt över Docker).</span><span class="sxs-lookup"><span data-stu-id="66521-111">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66521-112">Krav</span><span class="sxs-lookup"><span data-stu-id="66521-112">Prerequisites</span></span>
* <span data-ttu-id="66521-113">En utvecklingsdator som kör:</span><span class="sxs-lookup"><span data-stu-id="66521-113">A development computer running:</span></span>
  * <span data-ttu-id="66521-114">[Service Fabric SDK och verktyg](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="66521-114">[Service Fabric SDK and tools](service-fabric-get-started-linux.md).</span></span>
  * <span data-ttu-id="66521-115">[Docker CE för Linux](https://docs.docker.com/engine/installation/#prior-releases).</span><span class="sxs-lookup"><span data-stu-id="66521-115">[Docker CE for Linux](https://docs.docker.com/engine/installation/#prior-releases).</span></span> 
  * [<span data-ttu-id="66521-116">Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="66521-116">Service Fabric CLI</span></span>](service-fabric-cli.md)

* <span data-ttu-id="66521-117">Ett register i Azure Container Registry – [Skapa ett behållarregister](../container-registry/container-registry-get-started-portal.md) i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="66521-117">A registry in Azure Container Registry - [Create a container registry](../container-registry/container-registry-get-started-portal.md) in your Azure subscription.</span></span> 

## <a name="define-the-docker-container"></a><span data-ttu-id="66521-118">Definiera dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="66521-118">Define the Docker container</span></span>
<span data-ttu-id="66521-119">Skapa en avbildning baserat på [Python-avbildningen](https://hub.docker.com/_/python/) på Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="66521-119">Build an image based on the [Python image](https://hub.docker.com/_/python/) located on Docker Hub.</span></span> 

<span data-ttu-id="66521-120">Definiera Docker-behållaren i en Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="66521-120">Define your Docker container in a Dockerfile.</span></span> <span data-ttu-id="66521-121">Dockerfile innehåller instruktioner för att ställa in miljön i di behållare, läsa in programmet som du vill köra och mappa portar.</span><span class="sxs-lookup"><span data-stu-id="66521-121">The Dockerfile contains instructions for setting up the environment inside your container, loading the application you want to run, and mapping ports.</span></span> <span data-ttu-id="66521-122">Dockerfile är indata för kommandot `docker build` som skapar avbildningen.</span><span class="sxs-lookup"><span data-stu-id="66521-122">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span> 

<span data-ttu-id="66521-123">Skapa en tom katalog och skapa filen *Dockerfile* (utan filtillägget).</span><span class="sxs-lookup"><span data-stu-id="66521-123">Create an empty directory and create the file *Dockerfile* (with no file extension).</span></span> <span data-ttu-id="66521-124">Lägg till följande i *Dockerfile* och spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="66521-124">Add the following to *Dockerfile* and save your changes:</span></span>

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

<span data-ttu-id="66521-125">Läs [Dockerfile-referensen](https://docs.docker.com/engine/reference/builder/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="66521-125">Read the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more information.</span></span>

## <a name="create-a-simple-web-application"></a><span data-ttu-id="66521-126">Skapa en enkel webbapp</span><span class="sxs-lookup"><span data-stu-id="66521-126">Create a simple web application</span></span>
<span data-ttu-id="66521-127">Skapa en flask-webbapplikation som lyssnar på port 80 och returnerar ”Hello World”!.</span><span class="sxs-lookup"><span data-stu-id="66521-127">Create a Flask web application listening on port 80 that returns "Hello World!".</span></span>  <span data-ttu-id="66521-128">Skapa filen *requirements.txt* i samma katalog.</span><span class="sxs-lookup"><span data-stu-id="66521-128">In the same directory, create the file *requirements.txt*.</span></span>  <span data-ttu-id="66521-129">Lägg till följande och spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="66521-129">Add the following and save your changes:</span></span>
```
Flask
```

<span data-ttu-id="66521-130">Skapa även filen *app.py* och lägg till följande:</span><span class="sxs-lookup"><span data-stu-id="66521-130">Also create the *app.py* file and add the following:</span></span>

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    
    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

## <a name="build-the-image"></a><span data-ttu-id="66521-131">Skapa avbildningen</span><span class="sxs-lookup"><span data-stu-id="66521-131">Build the image</span></span>
<span data-ttu-id="66521-132">Kör kommandot `docker build` för att skapa avbildningen som kör ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="66521-132">Run the `docker build` command to create the image that runs your web application.</span></span> <span data-ttu-id="66521-133">Öppna ett PowerShell-fönster och gå till *c:\temp\helloworldapp*.</span><span class="sxs-lookup"><span data-stu-id="66521-133">Open a PowerShell window and navigate to *c:\temp\helloworldapp*.</span></span> <span data-ttu-id="66521-134">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="66521-134">Run the following command:</span></span>

```bash
docker build -t helloworldapp .
```

<span data-ttu-id="66521-135">Med det här kommandot skapas den nya avbildningen med hjälp av instruktionerna i din Dockerfile och avbildningen får namnet "helloworldapp" (-t-taggning).</span><span class="sxs-lookup"><span data-stu-id="66521-135">This command builds the new image using the instructions in your Dockerfile, naming (-t tagging) the image "helloworldapp".</span></span> <span data-ttu-id="66521-136">Genom att skapa en avbildning hämtas basavbildningen från Docker Hub och en ny avbildning skapas som lägger till ditt program ovanpå basavbildningen.</span><span class="sxs-lookup"><span data-stu-id="66521-136">Building an image pulls the base image down from Docker Hub and creates a new image that adds your application on top of the base image.</span></span>  

<span data-ttu-id="66521-137">När build-kommandot har slutförts kör du `docker images`-kommandot för att se information om den nya avbildningen:</span><span class="sxs-lookup"><span data-stu-id="66521-137">Once the build command completes, run the `docker images` command to see information on the new image:</span></span>

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-the-application-locally"></a><span data-ttu-id="66521-138">Kör programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="66521-138">Run the application locally</span></span>
<span data-ttu-id="66521-139">Kontrollera att av programmet körs lokalt innan du skickar det till behållarregistret.</span><span class="sxs-lookup"><span data-stu-id="66521-139">Verify that your containerized application runs locally before pushing it the container registry.</span></span>  

<span data-ttu-id="66521-140">Kör programmet, vilket mappar port 4000 på datorn till behållarens exponerade port 80:</span><span class="sxs-lookup"><span data-stu-id="66521-140">Run the application, mapping your computer's port 4000 to the container's exposed port 80:</span></span>

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

<span data-ttu-id="66521-141">*name* namnger den behållare som körs (i stället för behållar-ID:t).</span><span class="sxs-lookup"><span data-stu-id="66521-141">*name* gives a name to the running container (instead of the container ID).</span></span>

<span data-ttu-id="66521-142">Anslut till den behållare som körs.</span><span class="sxs-lookup"><span data-stu-id="66521-142">Connect to the running container.</span></span>  <span data-ttu-id="66521-143">Öppna en webbläsare med IP-adressen som returnerades på port 4000, till exempel "http://localhost:4000".</span><span class="sxs-lookup"><span data-stu-id="66521-143">Open a web browser pointing to the IP address returned on port 4000, for example "http://localhost:4000".</span></span> <span data-ttu-id="66521-144">Nu visas normalt rubriken "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="66521-144">You should see the heading "Hello World!"</span></span> <span data-ttu-id="66521-145">i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="66521-145">display in the browser.</span></span>

![Hello World!][hello-world]

<span data-ttu-id="66521-147">Om du vill stoppa behållaren kör du:</span><span class="sxs-lookup"><span data-stu-id="66521-147">To stop your container, run:</span></span>

```bash
docker stop my-web-site
```

<span data-ttu-id="66521-148">Ta bort behållaren från utvecklingsdatorn:</span><span class="sxs-lookup"><span data-stu-id="66521-148">Delete the container from your development machine:</span></span>

```bash
docker rm my-web-site
```

## <a name="push-the-image-to-the-container-registry"></a><span data-ttu-id="66521-149">Överför avbildningen till behållarregistret</span><span class="sxs-lookup"><span data-stu-id="66521-149">Push the image to the container registry</span></span>
<span data-ttu-id="66521-150">När du har kontrollerat att behållaren körs på Docker överför du avbildningen till registret i Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="66521-150">After you verify that the application runs in Docker, push the image to your registry in Azure Container Registry.</span></span>

<span data-ttu-id="66521-151">Kör `docker login` för att logga in till behållarregistret med dina [autentiseringsuppgifter för registret](../container-registry/container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="66521-151">Run `docker login` to log in to your container registry with your [registry credentials](../container-registry/container-registry-authentication.md).</span></span>

<span data-ttu-id="66521-152">I följande exempel skickas ID:t och lösenordet för ett Azure Active Directory [-tjänstobjekt](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="66521-152">The following example passes the ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="66521-153">Du kanske till exempel har tilldelat ett tjänstobjekt till registret för ett automatiseringsscenario.</span><span class="sxs-lookup"><span data-stu-id="66521-153">For example, you might have assigned a service principal to your registry for an automation scenario.</span></span>  <span data-ttu-id="66521-154">Du kan också logga in med ditt användarnamn och lösenord för registret.</span><span class="sxs-lookup"><span data-stu-id="66521-154">Or, you could login using your registry username and password.</span></span>

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="66521-155">Följande kommando skapar en tagg, eller ett alias, för avbildningen, med en fullständigt kvalificerad sökväg till registret.</span><span class="sxs-lookup"><span data-stu-id="66521-155">The following command creates a tag, or alias, of the image, with a fully qualified path to your registry.</span></span> <span data-ttu-id="66521-156">I det här exemplet placeras avbildningen i `samples`-namnområdet för att undvika oreda i registrets rot.</span><span class="sxs-lookup"><span data-stu-id="66521-156">This example places the image in the `samples` namespace to avoid clutter in the root of the registry.</span></span>

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

<span data-ttu-id="66521-157">Överför avbildningen till behållarregistret:</span><span class="sxs-lookup"><span data-stu-id="66521-157">Push the image to your container registry:</span></span>

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-the-docker-image-with-yeoman"></a><span data-ttu-id="66521-158">Paketetera Docker-avbildningen med Yeoman</span><span class="sxs-lookup"><span data-stu-id="66521-158">Package the Docker image with Yeoman</span></span>
<span data-ttu-id="66521-159">I Service Fabric SDK för Linux finns en [Yeoman](http://yeoman.io/)-generator som gör det enkelt att skapa ditt program och lägga till en behållaravbildning.</span><span class="sxs-lookup"><span data-stu-id="66521-159">The Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy to create your application and add a container image.</span></span> <span data-ttu-id="66521-160">Nu ska vi använda Yeoman för att skapa ett program med en enda dockerbehållare som kallas *SimpleContainerApp*.</span><span class="sxs-lookup"><span data-stu-id="66521-160">Let's use Yeoman to create an application with a single Docker container called *SimpleContainerApp*.</span></span>

<span data-ttu-id="66521-161">Om du vill skapa ett program för Service Fabric-behållare kan du öppna ett terminalfönster och köra `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="66521-161">To create a Service Fabric container application, open a terminal window and run `yo azuresfcontainer`.</span></span>  

<span data-ttu-id="66521-162">Namnge ditt program (till exempel ”minbehållare”).</span><span class="sxs-lookup"><span data-stu-id="66521-162">Name your application (for example, "mycontainer").</span></span> 

<span data-ttu-id="66521-163">Ange en URL för behållaravbildningen i ett behållarregister (till exempel ””).</span><span class="sxs-lookup"><span data-stu-id="66521-163">Provide the URL for the container image in a container registry (for example, "").</span></span> 

<span data-ttu-id="66521-164">Den här avbildningen har en definierad startpunkt arbetsbelastningen, så måste du uttryckligen ange inkommande kommandon (kommandon körs i den behållare som kommer att hålla den behållare som körs efter start).</span><span class="sxs-lookup"><span data-stu-id="66521-164">This image has a workload entry-point defined, so need to explicitly specify input commands (commands run inside the container, which will keep the container running after startup).</span></span> 

<span data-ttu-id="66521-165">Ange ett instansantal på ”1”.</span><span class="sxs-lookup"><span data-stu-id="66521-165">Specify an instance count of "1".</span></span>

![Service Fabric Yeoman-generator för behållare][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a><span data-ttu-id="66521-167">Konfigurera portmappning och autentisering av behållardatabas</span><span class="sxs-lookup"><span data-stu-id="66521-167">Configure port mapping and container repository authentication</span></span>
<span data-ttu-id="66521-168">Behållartjänsten behöver en slutpunkt för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="66521-168">Your containerized service needs an endpoint for communication.</span></span>  <span data-ttu-id="66521-169">Lägg nu till protokollet, porten och typen till en `Endpoint` i filen ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="66521-169">Now add the protocol, port, and type to an `Endpoint` in the ServiceManifest.xml file.</span></span> <span data-ttu-id="66521-170">Behållartjänsten för den här artikeln lyssnar på port 4000:</span><span class="sxs-lookup"><span data-stu-id="66521-170">For this article, the containerized service listens on port 4000:</span></span> 

```xml
<Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
```
<span data-ttu-id="66521-171">Genom att tillhandahålla `UriScheme` registreras automatiskt behållarslutpunkten med namngivningstjänsten för Service Fabric för identifiering.</span><span class="sxs-lookup"><span data-stu-id="66521-171">Providing the `UriScheme` automatically registers the container endpoint with the Service Fabric Naming service for discoverability.</span></span> <span data-ttu-id="66521-172">En fullständig ServiceManifest.xml-exempelfil finns i slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="66521-172">A full ServiceManifest.xml example file is provided at the end of this article.</span></span> 

<span data-ttu-id="66521-173">Konfigurera behållarens portmappning (port till värd) genom att ange en `PortBinding`-princip i `ContainerHostPolicies` för filen ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="66521-173">Configure the container port-to-host port mapping by specifying a `PortBinding` policy in `ContainerHostPolicies` of the ApplicationManifest.xml file.</span></span>  <span data-ttu-id="66521-174">I den här artikeln är `ContainerPort` 80 (behållaren exponerar port 80, som anges i Dockerfile) och `EndpointRef` är ”myserviceTypeEndpoint” (slutpunkt som definierats i tjänstmanifestet).</span><span class="sxs-lookup"><span data-stu-id="66521-174">For this article, `ContainerPort` is 80 (the container exposes port 80, as specified in the Dockerfile) and `EndpointRef` is "myserviceTypeEndpoint" (the endpoint defined in the service manifest).</span></span>  <span data-ttu-id="66521-175">Inkommande begäranden till tjänsten på port 4000 mappas till port 80 för behållaren.</span><span class="sxs-lookup"><span data-stu-id="66521-175">Incoming requests to the service on port 4000 are mapped to port 80 on the container.</span></span>  <span data-ttu-id="66521-176">Om behållaren behöver autentiseras med en privat lagringsplats lägger du till `RepositoryCredentials`.</span><span class="sxs-lookup"><span data-stu-id="66521-176">If your container needs to authenticate with a private repository, then add `RepositoryCredentials`.</span></span>  <span data-ttu-id="66521-177">I den här artikeln lägger du till kontonamnet och lösenordet för behållarregistret myregistry.azurecr.io.</span><span class="sxs-lookup"><span data-stu-id="66521-177">For this article, add the account name and password for the myregistry.azurecr.io container registry.</span></span> 

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-the-service-fabric-application"></a><span data-ttu-id="66521-178">Utveckla och distribuera ett Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="66521-178">Build and package the Service Fabric application</span></span>
<span data-ttu-id="66521-179">I Service Fabric Yeoman-mallarna ingår ett byggskript för [Gradle](https://gradle.org/) som du kan använda för att skapa programmet från terminalen.</span><span class="sxs-lookup"><span data-stu-id="66521-179">The Service Fabric Yeoman templates include a build script for [Gradle](https://gradle.org/), which you can use to build the application from the terminal.</span></span> <span data-ttu-id="66521-180">När du ska bygga och paketera programmet kör du följande:</span><span class="sxs-lookup"><span data-stu-id="66521-180">To build and package the application, run the following:</span></span>

```bash
cd mycontainer
gradle
```

## <a name="deploy-the-application"></a><span data-ttu-id="66521-181">Distribuera programmet</span><span class="sxs-lookup"><span data-stu-id="66521-181">Deploy the application</span></span>
<span data-ttu-id="66521-182">När du har skapat programmet kan du distribuera det till det lokala klustret med Service Fabric CLI.</span><span class="sxs-lookup"><span data-stu-id="66521-182">Once the application is built, you can deploy it to the local cluster using the Service Fabric CLI.</span></span>

<span data-ttu-id="66521-183">Anslut till det lokala Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="66521-183">Connect to the local Service Fabric cluster.</span></span>

```bash
sfctl cluster select --endpoint http://localhost:19080
```

<span data-ttu-id="66521-184">Använd installationsskriptet som medföljer mallen för att kopiera programpaketet till klustrets avbildningsarkiv, registrera programtypen och skapa en instans av programmet.</span><span class="sxs-lookup"><span data-stu-id="66521-184">Use the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.</span></span>

```bash
./install.sh
```

<span data-ttu-id="66521-185">Öppna en webbläsare och gå till Service Fabric Explorer på http://localhost:19080/Explorer (ersätt localhost med den virtuella datorns privata IP om du använder Vagrant på Mac OS X).</span><span class="sxs-lookup"><span data-stu-id="66521-185">Open a browser and navigate to Service Fabric Explorer at http://localhost:19080/Explorer (replace localhost with the private IP of the VM if using Vagrant on Mac OS X).</span></span> <span data-ttu-id="66521-186">Expandera programnoden och observera att det nu finns en post för din programtyp och en post för den första instansen av den typen.</span><span class="sxs-lookup"><span data-stu-id="66521-186">Expand the Applications node and note that there is now an entry for your application type and another for the first instance of that type.</span></span>

<span data-ttu-id="66521-187">Anslut till den behållare som körs.</span><span class="sxs-lookup"><span data-stu-id="66521-187">Connect to the running container.</span></span>  <span data-ttu-id="66521-188">Öppna en webbläsare med IP-adressen som returnerades på port 4000, till exempel "http://localhost:4000".</span><span class="sxs-lookup"><span data-stu-id="66521-188">Open a web browser pointing to the IP address returned on port 4000, for example "http://localhost:4000".</span></span> <span data-ttu-id="66521-189">Nu visas normalt rubriken "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="66521-189">You should see the heading "Hello World!"</span></span> <span data-ttu-id="66521-190">i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="66521-190">display in the browser.</span></span>

![Hello World!][hello-world]

## <a name="clean-up"></a><span data-ttu-id="66521-192">Rensa</span><span class="sxs-lookup"><span data-stu-id="66521-192">Clean up</span></span>
<span data-ttu-id="66521-193">Använd installationsskriptet som medföljer mallen för att ta bort programinstansen från det lokala utvecklingsklustret och avregistrera programtypen.</span><span class="sxs-lookup"><span data-stu-id="66521-193">Use the uninstall script provided in the template to delete the application instance from the local development cluster and unregister the application type.</span></span>

```bash
./uninstall.sh
```

<span data-ttu-id="66521-194">När du har överfört avbildningen till behållarregistret kan du ta bort den lokala avbildningen från utvecklingsdatorn:</span><span class="sxs-lookup"><span data-stu-id="66521-194">After you push the image to the container registry you can delete the local image from your development computer:</span></span>

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a><span data-ttu-id="66521-195">Komplett exempel på Service Fabric-app och tjänstmanifest</span><span class="sxs-lookup"><span data-stu-id="66521-195">Complete example Service Fabric application and service manifests</span></span>
<span data-ttu-id="66521-196">Här är de fullständiga tjänst- och appmanifesten som används i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="66521-196">Here are the complete service and application manifests used in this article.</span></span>

### <a name="servicemanifestxml"></a><span data-ttu-id="66521-197">ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="66521-197">ServiceManifest.xml</span></span>
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
### <a name="applicationmanifestxml"></a><span data-ttu-id="66521-198">ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="66521-198">ApplicationManifest.xml</span></span>
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
## <a name="adding-more-services-to-an-existing-application"></a><span data-ttu-id="66521-199">Lägga till fler tjänster till ett befintligt program</span><span class="sxs-lookup"><span data-stu-id="66521-199">Adding more services to an existing application</span></span>

<span data-ttu-id="66521-200">Om du vill lägga till en till behållartjänst till ett program som redan har skapats med hjälp av yeoman utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="66521-200">To add another container service to an application already created using yeoman, perform the following steps:</span></span>

1. <span data-ttu-id="66521-201">Ändra katalogen till roten för det befintliga programmet.</span><span class="sxs-lookup"><span data-stu-id="66521-201">Change directory to the root of the existing application.</span></span>  <span data-ttu-id="66521-202">Till exempel `cd ~/YeomanSamples/MyApplication` om `MyApplication` är programmet som skapats av Yeoman.</span><span class="sxs-lookup"><span data-stu-id="66521-202">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is the application created by Yeoman.</span></span>
2. <span data-ttu-id="66521-203">Kör `yo azuresfcontainer:AddService`</span><span class="sxs-lookup"><span data-stu-id="66521-203">Run `yo azuresfcontainer:AddService`</span></span>

<a id="manually"></a>


## <a name="configure-time-interval-before-container-is-force-terminated"></a><span data-ttu-id="66521-204">Ställ in tidsintervall innan behållaren tvångsavslutas</span><span class="sxs-lookup"><span data-stu-id="66521-204">Configure time interval before container is force terminated</span></span>

<span data-ttu-id="66521-205">Du kan ställa in ett tidsintervall för hur lång exekveringstid som ska gå innan behållaren tas bort när borttagning av tjänsten (eller flytt till en annan nod) har påbörjats.</span><span class="sxs-lookup"><span data-stu-id="66521-205">You can configure a time interval for the runtime to wait before the container is removed after the service deletion (or a move to another node) has started.</span></span> <span data-ttu-id="66521-206">När du ställer in ett tidsintervall skickas kommandot `docker stop <time in seconds>` till behållaren.</span><span class="sxs-lookup"><span data-stu-id="66521-206">Configuring the time interval sends the `docker stop <time in seconds>` command to the container.</span></span>   <span data-ttu-id="66521-207">Mer information finns i [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span><span class="sxs-lookup"><span data-stu-id="66521-207">For more detail, see [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span></span> <span data-ttu-id="66521-208">Tidsintervallet anges i avsnittet `Hosting`.</span><span class="sxs-lookup"><span data-stu-id="66521-208">The time interval to wait is specified under the `Hosting` section.</span></span> <span data-ttu-id="66521-209">I följande klustermanifestutdrag visas hur du ställer in väntetidsintervallet:</span><span class="sxs-lookup"><span data-stu-id="66521-209">The following cluster manifest snippet shows how to set the wait interval:</span></span>

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
<span data-ttu-id="66521-210">Standardtidsintervallet är inställt på 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="66521-210">The default time interval is set to 10 seconds.</span></span> <span data-ttu-id="66521-211">Eftersom inställningen är dynamisk uppdateras tidsgränsen med en konfigurationsuppdatering på klustret.</span><span class="sxs-lookup"><span data-stu-id="66521-211">Since this configuration is dynamic, a config only upgrade on the cluster updates the timeout.</span></span> 


## <a name="configure-the-runtime-to-remove-unused-container-images"></a><span data-ttu-id="66521-212">Ställ in exekveringstid för att ta bort behållaravbildningar som inte används</span><span class="sxs-lookup"><span data-stu-id="66521-212">Configure the runtime to remove unused container images</span></span>

<span data-ttu-id="66521-213">Du kan ställa in Service Fabric-klustret på att ta bort oanvända behållaravbildningar från noden.</span><span class="sxs-lookup"><span data-stu-id="66521-213">You can configure the Service Fabric cluster to remove unused container images from the node.</span></span> <span data-ttu-id="66521-214">Med den här inställningen kan du få tillbaka diskutrymme om det finns för många behållaravbildningar på noden.</span><span class="sxs-lookup"><span data-stu-id="66521-214">This configuration allows disk space to be recaptured if too many container images are present on the node.</span></span>  <span data-ttu-id="66521-215">Aktivera funktionen genom att uppdatera avsnittet `Hosting` i klustermanifestet enligt följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="66521-215">To enable this feature, update the `Hosting` section in the cluster manifest as shown in the following snippet:</span></span> 


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

<span data-ttu-id="66521-216">Avbildningar som inte ska raderas kan du ange under parametern `ContainerImagesToSkip`.</span><span class="sxs-lookup"><span data-stu-id="66521-216">For images that should not be deleted, you can specify them under the `ContainerImagesToSkip` parameter.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="66521-217">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66521-217">Next steps</span></span>
* <span data-ttu-id="66521-218">Mer information om hur du kör [behållare i Service Fabric](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="66521-218">Learn more about running [containers on Service Fabric](service-fabric-containers-overview.md).</span></span>
* <span data-ttu-id="66521-219">Läs kursen [Distribuera ett .NET-program i en behållare](service-fabric-host-app-in-a-container.md).</span><span class="sxs-lookup"><span data-stu-id="66521-219">Read the [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) tutorial.</span></span>
* <span data-ttu-id="66521-220">Läs om Service Fabric-[applivscykeln](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="66521-220">Learn about the Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
* <span data-ttu-id="66521-221">Se [kodexempel för Service Fabric-behållare](https://github.com/Azure-Samples/service-fabric-dotnet-containers) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="66521-221">Checkout the [Service Fabric container code samples](https://github.com/Azure-Samples/service-fabric-dotnet-containers) on GitHub.</span></span>

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png
