---
title: "aaaDeploy en .NET-app i en behållare tooAzure Service Fabric | Microsoft Docs"
description: "Lär dig hur toopackage en .NET-app i Visual Studio i en Docker-behållare. Den här nya ”behållare” app distribueras sedan tooa Service Fabric-klustret."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: mikhegn
ms.openlocfilehash: 094b0e71d76b2e56ffb9b23638dd8154b3aff5fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-net-application-in-a-windows-container-tooazure-service-fabric"></a><span data-ttu-id="2b166-104">Distribuera ett .NET-program i en Windows-behållaren tooAzure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="2b166-104">Deploy a .NET application in a Windows container tooAzure Service Fabric</span></span>

<span data-ttu-id="2b166-105">De här självstudierna visar hur toodeploy ett befintligt ASP.NET-program i en Windows-behållare på Azure.</span><span class="sxs-lookup"><span data-stu-id="2b166-105">This tutorial shows you how toodeploy an existing ASP.NET application in a Windows container on Azure.</span></span>

<span data-ttu-id="2b166-106">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="2b166-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2b166-107">Skapa en Docker-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b166-107">Create a Docker project in Visual Studio</span></span>
> * <span data-ttu-id="2b166-108">Containerize ett befintligt program</span><span class="sxs-lookup"><span data-stu-id="2b166-108">Containerize an existing application</span></span>
> * <span data-ttu-id="2b166-109">Installationsprogrammet kontinuerlig integrering med Visual Studio och VSTS</span><span class="sxs-lookup"><span data-stu-id="2b166-109">Setup continuous integration with Visual Studio and VSTS</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b166-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2b166-110">Prerequisites</span></span>

1. <span data-ttu-id="2b166-111">Installera [Docker CE för Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) så att du kan köra behållare på Windows 10.</span><span class="sxs-lookup"><span data-stu-id="2b166-111">Install [Docker CE for Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) so that you can run containers on Windows 10.</span></span>
2. <span data-ttu-id="2b166-112">Bekanta dig med hello [Windows 10 behållare quickstart][link-container-quickstart].</span><span class="sxs-lookup"><span data-stu-id="2b166-112">Familiarize yourself with hello [Windows 10 Containers quickstart][link-container-quickstart].</span></span>
3. <span data-ttu-id="2b166-113">Hämta hello [Fabrikam Fiber CallCenter] [ link-fabrikam-github] exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="2b166-113">Download hello [Fabrikam Fiber CallCenter][link-fabrikam-github] sample application.</span></span>
4. <span data-ttu-id="2b166-114">Installera [Azure PowerShell][link-azure-powershell-install]</span><span class="sxs-lookup"><span data-stu-id="2b166-114">Install [Azure PowerShell][link-azure-powershell-install]</span></span>
5. <span data-ttu-id="2b166-115">Installera hello [kontinuerlig leveransverktyg tillägget för Visual Studio 2017][link-visualstudio-cd-extension]</span><span class="sxs-lookup"><span data-stu-id="2b166-115">Install hello [Continuous Delivery Tools extension for Visual Studio 2017][link-visualstudio-cd-extension]</span></span>
6. <span data-ttu-id="2b166-116">Skapa en [Azure-prenumeration] [ link-azure-subscription] och en [Visual Studio Team Services-konto][link-vsts-account].</span><span class="sxs-lookup"><span data-stu-id="2b166-116">Create an [Azure subscription][link-azure-subscription] and a [Visual Studio Team Services account][link-vsts-account].</span></span> 
7. [<span data-ttu-id="2b166-117">Skapa ett kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="2b166-117">Create a cluster on Azure</span></span>](service-fabric-tutorial-create-cluster-azure-ps.md)

## <a name="containerize-hello-application"></a><span data-ttu-id="2b166-118">Containerize hello program</span><span class="sxs-lookup"><span data-stu-id="2b166-118">Containerize hello application</span></span>

<span data-ttu-id="2b166-119">Nu när du har en [Service Fabric-klustret körs i Azure](service-fabric-tutorial-create-cluster-azure-ps.md) du är redo toocreate och distribuera en av programmet.</span><span class="sxs-lookup"><span data-stu-id="2b166-119">Now that you have a [Service Fabric cluster is running in Azure](service-fabric-tutorial-create-cluster-azure-ps.md) you are ready toocreate and deploy a containerized application.</span></span> <span data-ttu-id="2b166-120">toostart kör appen i en behållare, behöver vi tooadd **Docker stöd** toohello projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2b166-120">toostart running our application in a container, we need tooadd **Docker Support** toohello project in Visual Studio.</span></span> <span data-ttu-id="2b166-121">När du lägger till **Docker stöd** toohello programmet två saker.</span><span class="sxs-lookup"><span data-stu-id="2b166-121">When you add **Docker support** toohello application, two things happen.</span></span> <span data-ttu-id="2b166-122">Först en _Dockerfile_ läggs toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="2b166-122">First, a _Dockerfile_ is added toohello project.</span></span> <span data-ttu-id="2b166-123">Den nya filen beskriver hur hello behållaren bilden är toobe som har skapats.</span><span class="sxs-lookup"><span data-stu-id="2b166-123">This new file describes how hello container image is toobe built.</span></span> <span data-ttu-id="2b166-124">Sedan andra, en ny _docker compose_ projektet läggs toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="2b166-124">Then second, a new _docker-compose_ project is added toohello solution.</span></span> <span data-ttu-id="2b166-125">hello nytt projekt innehåller några docker compose filer.</span><span class="sxs-lookup"><span data-stu-id="2b166-125">hello new project contains a few docker-compose files.</span></span> <span data-ttu-id="2b166-126">Docker compose filer kan vara används toodescribe hur hello behållare körs.</span><span class="sxs-lookup"><span data-stu-id="2b166-126">Docker-compose files can be used toodescribe how hello container is run.</span></span>

<span data-ttu-id="2b166-127">Mer information om hur du arbetar med [verktyg för Visual Studio-behållaren][link-visualstudio-container-tools].</span><span class="sxs-lookup"><span data-stu-id="2b166-127">More info on working with [Visual Studio Container Tools][link-visualstudio-container-tools].</span></span>

>[!NOTE]
><span data-ttu-id="2b166-128">Om den är hello första gången du kör Windows behållaren bilder på datorn, måste Docker CE hämtar hello grundläggande avbildningar för din behållare.</span><span class="sxs-lookup"><span data-stu-id="2b166-128">If it is hello first time you are running Windows container images on your computer, Docker CE must pull down hello base images for your containers.</span></span> <span data-ttu-id="2b166-129">hello bilder används i den här kursen är 14 GB.</span><span class="sxs-lookup"><span data-stu-id="2b166-129">hello images used in this tutorial are 14 GB.</span></span> <span data-ttu-id="2b166-130">Gå vidare och kör följande terminal kommandot toopull hello grundläggande bilder hello:</span><span class="sxs-lookup"><span data-stu-id="2b166-130">Go ahead and run hello following terminal command toopull hello base images:</span></span>
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a><span data-ttu-id="2b166-131">Lägga till Docker-stöd</span><span class="sxs-lookup"><span data-stu-id="2b166-131">Add Docker support</span></span>

<span data-ttu-id="2b166-132">Öppna hello [FabrikamFiber.CallCenter.sln] [ link-fabrikam-github] filen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2b166-132">Open hello [FabrikamFiber.CallCenter.sln][link-fabrikam-github] file in Visual Studio.</span></span>

<span data-ttu-id="2b166-133">Högerklicka på hello **FabrikamFiber.Web** project > **Lägg till** > **Docker Support**.</span><span class="sxs-lookup"><span data-stu-id="2b166-133">Right-click hello **FabrikamFiber.Web** project > **Add** > **Docker Support**.</span></span>

### <a name="add-support-for-sql"></a><span data-ttu-id="2b166-134">Lägga till stöd för SQL</span><span class="sxs-lookup"><span data-stu-id="2b166-134">Add support for SQL</span></span>

<span data-ttu-id="2b166-135">Det här programmet används SQL som hello DataProvider och en SQLServer är obligatoriska toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="2b166-135">This application uses SQL as hello data provider, so a SQL server is required toorun hello application.</span></span> <span data-ttu-id="2b166-136">Referera till en SQL Server-behållaren avbildning i vår docker-compose.override.yml-filen.</span><span class="sxs-lookup"><span data-stu-id="2b166-136">Reference a SQL Server container image in our docker-compose.override.yml file.</span></span>

<span data-ttu-id="2b166-137">Öppna i Visual Studio **Solution Explorer**, hitta **docker compose**, och öppna hello **docker compose.override.yml**.</span><span class="sxs-lookup"><span data-stu-id="2b166-137">In Visual Studio, open **Solution Explorer**, find **docker-compose**, and open hello file **docker-compose.override.yml**.</span></span>

<span data-ttu-id="2b166-138">Navigera toohello `services:` nod, lägga till en nod med namnet `db:` som definierar hello SQL Server-post för hello behållare.</span><span class="sxs-lookup"><span data-stu-id="2b166-138">Navigate toohello `services:` node, add a node named `db:` that defines hello SQL Server entry for hello container.</span></span>

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
><span data-ttu-id="2b166-139">Du kan använda alla SQL-Server som du föredrar för lokala felsökning så länge den kan nås från värden.</span><span class="sxs-lookup"><span data-stu-id="2b166-139">You can use any SQL Server you prefer for local debugging, as long as it is reachable from your host.</span></span> <span data-ttu-id="2b166-140">Dock **localdb** stöder inte `container -> host` kommunikation.</span><span class="sxs-lookup"><span data-stu-id="2b166-140">However, **localdb** does not support `container -> host` communication.</span></span>

>[!WARNING]
><span data-ttu-id="2b166-141">Kör SQL Server i en behållare stöder inte bestående data.</span><span class="sxs-lookup"><span data-stu-id="2b166-141">Running SQL Server in a container does not support persisting data.</span></span> <span data-ttu-id="2b166-142">Dina data raderas när hello behållaren stoppar.</span><span class="sxs-lookup"><span data-stu-id="2b166-142">When hello container stops, your data is erased.</span></span> <span data-ttu-id="2b166-143">Använd inte den här konfigurationen för produktion.</span><span class="sxs-lookup"><span data-stu-id="2b166-143">Do not use this configuration for production.</span></span>

<span data-ttu-id="2b166-144">Navigera toohello `fabrikamfiber.web:` nod och Lägg till en underordnad nod med namnet `depends_on:`.</span><span class="sxs-lookup"><span data-stu-id="2b166-144">Navigate toohello `fabrikamfiber.web:` node and add a child node named `depends_on:`.</span></span> <span data-ttu-id="2b166-145">Detta säkerställer att hello `db` (hello SQL Server-behållare)-tjänsten startar före våra webbprogram (fabrikamfiber.web).</span><span class="sxs-lookup"><span data-stu-id="2b166-145">This ensures that hello `db` service (hello SQL Server container) starts before our web application (fabrikamfiber.web).</span></span>

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-hello-web-config"></a><span data-ttu-id="2b166-146">Uppdatera hello Webbkonfiguration</span><span class="sxs-lookup"><span data-stu-id="2b166-146">Update hello web config</span></span>

<span data-ttu-id="2b166-147">Tillbaka i hello **FabrikamFiber.Web** projektet update hello anslutningssträngen i hello **web.config** filen toopoint toohello SQL Server i hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="2b166-147">Back in hello **FabrikamFiber.Web** project, update hello connection string in hello **web.config** file, toopoint toohello SQL Server in hello container.</span></span>

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
><span data-ttu-id="2b166-148">Om du vill toouse en annan SQL-Server när du skapar en Versionspost skapa för ditt webbprogram, kan du lägga till en annan sträng tooyour web.release.config anslutningsfilen.</span><span class="sxs-lookup"><span data-stu-id="2b166-148">If you want toouse a different SQL Server when building a release build of your web application, add another connection string tooyour web.release.config file.</span></span>

### <a name="test-your-container"></a><span data-ttu-id="2b166-149">Testa din behållare</span><span class="sxs-lookup"><span data-stu-id="2b166-149">Test your container</span></span>

<span data-ttu-id="2b166-150">Tryck på **F5** toorun och felsökningsloggar hello-program i din behållaren.</span><span class="sxs-lookup"><span data-stu-id="2b166-150">Press **F5** toorun and debug hello application in your container.</span></span>

<span data-ttu-id="2b166-151">Edge öppnar programmets definierade startsida med hello IP-adress för hello behållare i hello interna NAT nätverket (vanligtvis 172.x.x.x).</span><span class="sxs-lookup"><span data-stu-id="2b166-151">Edge opens your application's defined launch page using hello IP address of hello container on hello internal NAT network (typically 172.x.x.x).</span></span> <span data-ttu-id="2b166-152">toolearn mer information om hur du felsöker program i behållare med hjälp av Visual Studio 2017 finns [i den här artikeln][link-debug-container].</span><span class="sxs-lookup"><span data-stu-id="2b166-152">toolearn more about debugging applications in containers using Visual Studio 2017, see [this article][link-debug-container].</span></span>

![exempel på fabrikam i en behållare][image-web-preview]

<span data-ttu-id="2b166-154">hello-behållaren är nu redo toobe skapats och paketeras i en Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="2b166-154">hello container is now ready toobe built and packaged in a Service Fabric application.</span></span> <span data-ttu-id="2b166-155">När du har hello behållaren avbildningen skapades på datorn kan du dra nedåt tooany värden toorun push-installera den tooany behållare registret.</span><span class="sxs-lookup"><span data-stu-id="2b166-155">Once you have hello container image built on your machine, you can push it tooany container registry and pull it down tooany host toorun.</span></span>

## <a name="get-hello-application-ready-for-hello-cloud"></a><span data-ttu-id="2b166-156">Förbereda hello program för hello moln</span><span class="sxs-lookup"><span data-stu-id="2b166-156">Get hello application ready for hello cloud</span></span>

<span data-ttu-id="2b166-157">tooget hello program som är redo för att köra i Service Fabric i Azure, behöver vi toocomplete två steg:</span><span class="sxs-lookup"><span data-stu-id="2b166-157">tooget hello application ready for running in Service Fabric in Azure, we need toocomplete two steps:</span></span>

1. <span data-ttu-id="2b166-158">Exponera hello port där vi vill toobe kan tooreach våra webbprogram i hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="2b166-158">Expose hello port where we want toobe able tooreach our web application in hello Service Fabric cluster.</span></span>
2. <span data-ttu-id="2b166-159">Ange en klar SQL-databas för produktion för vårt program.</span><span class="sxs-lookup"><span data-stu-id="2b166-159">Provide a production ready SQL database for our application.</span></span>

### <a name="expose-hello-port-for-hello-app"></a><span data-ttu-id="2b166-160">Exponera hello port för hello app</span><span class="sxs-lookup"><span data-stu-id="2b166-160">Expose hello port for hello app</span></span>
<span data-ttu-id="2b166-161">Vi har konfigurerat hello Service Fabric-klustret har port *80* öppen som standard i hello Azure belastningsutjämnare, som balanserar inkommande trafik toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="2b166-161">hello Service Fabric cluster we have configured, has port *80* open by default in hello Azure Load Balancer, that balances incoming traffic toohello cluster.</span></span> <span data-ttu-id="2b166-162">Via vårt filen docker-compose.yml kan du använda vår behållaren på den här porten.</span><span class="sxs-lookup"><span data-stu-id="2b166-162">We can expose our container on this port via our docker-compose.yml file.</span></span>

<span data-ttu-id="2b166-163">Öppna i Visual Studio **Solution Explorer**, hitta **docker compose**, och öppna hello **docker compose.override.yml**.</span><span class="sxs-lookup"><span data-stu-id="2b166-163">In Visual Studio, open **Solution Explorer**, find **docker-compose**, and open hello file **docker-compose.override.yml**.</span></span>

<span data-ttu-id="2b166-164">Ändra hello `fabrikamfiber.web:` nod, lägga till en underordnad nod med namnet `ports:`.</span><span class="sxs-lookup"><span data-stu-id="2b166-164">Modify hello `fabrikamfiber.web:` node, add a child node named `ports:`.</span></span>

<span data-ttu-id="2b166-165">Lägg till en sträng som värde `- "80:80"`.</span><span class="sxs-lookup"><span data-stu-id="2b166-165">Add a string entry `- "80:80"`.</span></span>

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a><span data-ttu-id="2b166-166">Använd en SQL-databas för produktion</span><span class="sxs-lookup"><span data-stu-id="2b166-166">Use a production SQL database</span></span>
<span data-ttu-id="2b166-167">När du kör i produktion kan vi behöver våra data kvar i vår databas.</span><span class="sxs-lookup"><span data-stu-id="2b166-167">When running in production, we need our data persisted in our database.</span></span> <span data-ttu-id="2b166-168">Det finns för närvarande inga sätt tooguarantee beständiga data i en behållare, därför du lagra inte produktionsdata i SQL Server i en behållare.</span><span class="sxs-lookup"><span data-stu-id="2b166-168">There is currently no way tooguarantee persistent data in a container, therefore you cannot store production data in SQL Server in a container.</span></span>

<span data-ttu-id="2b166-169">Vi rekommenderar att du använder en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="2b166-169">We recommend you utilize an Azure SQL Database.</span></span> <span data-ttu-id="2b166-170">tooset in och köra en hanterad SQL Server på Azure, gå hello [Azure SQL Database Snabbstart] [ link-azure-sql] artikel.</span><span class="sxs-lookup"><span data-stu-id="2b166-170">tooset up and run a managed SQL Server in Azure, visit hello [Azure SQL Database Quickstarts][link-azure-sql] article.</span></span>

>[!NOTE]
><span data-ttu-id="2b166-171">Kom ihåg toochange hello anslutning strängar toohello SQLServer i hello **web.release.config** filen i hello **FabrikamFiber.Web** projekt.</span><span class="sxs-lookup"><span data-stu-id="2b166-171">Remember toochange hello connection strings toohello SQL server in hello **web.release.config** file in hello **FabrikamFiber.Web** project.</span></span>
>
><span data-ttu-id="2b166-172">Det här programmet misslyckas programinstallation utan problem om inga SQL-databasen inte kan nås.</span><span class="sxs-lookup"><span data-stu-id="2b166-172">This application fails gracefully if no SQL database is reachable.</span></span> <span data-ttu-id="2b166-173">Du kan välja toogo vidare och distribuera hello program med ingen SQLServer.</span><span class="sxs-lookup"><span data-stu-id="2b166-173">You can choose toogo ahead and deploy hello application with no SQL server.</span></span>

## <a name="deploy-with-visual-studio-team-services"></a><span data-ttu-id="2b166-174">Distribuera med Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="2b166-174">Deploy with Visual Studio Team Services</span></span>

<span data-ttu-id="2b166-175">tooset upp distribution med hjälp av Visual Studio Team Services behöver du tooinstall hello [kontinuerlig leveransverktyg tillägget för Visual Studio-2017][link-visualstudio-cd-extension].</span><span class="sxs-lookup"><span data-stu-id="2b166-175">tooset up deployment using Visual Studio Team Services, you need tooinstall hello [Continuous Delivery Tools extension for Visual Studio 2017][link-visualstudio-cd-extension].</span></span> <span data-ttu-id="2b166-176">Det här tillägget gör det enkelt toodeploy tooAzure genom att konfigurera Visual Studio Team Services och hämta app distribuerat tooyour Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="2b166-176">This extension makes it easy toodeploy tooAzure by configuring Visual Studio Team Services and get your app deployed tooyour Service Fabric cluster.</span></span>

<span data-ttu-id="2b166-177">tooget igång din kod måste finnas i källkontroll.</span><span class="sxs-lookup"><span data-stu-id="2b166-177">tooget started, your code must be hosted in source control.</span></span> <span data-ttu-id="2b166-178">hello resten av det här avsnittet förutsätter **git** används.</span><span class="sxs-lookup"><span data-stu-id="2b166-178">hello rest of this section assumes **git** is being used.</span></span>

### <a name="set-up-a-vsts-repo"></a><span data-ttu-id="2b166-179">Konfigurera en VSTS repo</span><span class="sxs-lookup"><span data-stu-id="2b166-179">Set up a VSTS repo</span></span>
<span data-ttu-id="2b166-180">Hello nedre högra hörnet av Visual Studio klickar du på **lägga till tooSource kontrollen** > **Git** (eller alternativet som du föredrar).</span><span class="sxs-lookup"><span data-stu-id="2b166-180">At hello bottom-right corner of Visual Studio, click **Add tooSource Control** > **Git** (or whichever option you prefer).</span></span>

![Klicka på hello källa kontroll för][image-source-control]

<span data-ttu-id="2b166-182">I hello _Team Explorer_ rutan, tryck på **publicera Git Repo**.</span><span class="sxs-lookup"><span data-stu-id="2b166-182">In hello _Team Explorer_ pane, press **Publish Git Repo**.</span></span>

<span data-ttu-id="2b166-183">Välj VSTS databasnamn och tryck på **databasen**.</span><span class="sxs-lookup"><span data-stu-id="2b166-183">Select your VSTS repository name and press **Repository**.</span></span>

![Publicera lagringsplatsen tooVSTS][image-publish-repo]

<span data-ttu-id="2b166-185">Nu när koden synkroniseras med en VSTS källdatabasen, kan du konfigurera kontinuerlig integrering och kontinuerlig leverans.</span><span class="sxs-lookup"><span data-stu-id="2b166-185">Now that your code is synchronized with a VSTS source repository, you can configure continuous integration and continuous delivery.</span></span>

### <a name="setup-continuous-delivery"></a><span data-ttu-id="2b166-186">Installationsprogrammet kontinuerlig leverans</span><span class="sxs-lookup"><span data-stu-id="2b166-186">Setup continuous delivery</span></span>

<span data-ttu-id="2b166-187">I _Solution Explorer_, högerklicka på hello **lösning** > **konfigurera kontinuerlig leverans**.</span><span class="sxs-lookup"><span data-stu-id="2b166-187">In _Solution Explorer_, right-click hello **solution** > **Configure Continuous Delivery**.</span></span>

<span data-ttu-id="2b166-188">Välj hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2b166-188">Select hello Azure Subscription.</span></span>

<span data-ttu-id="2b166-189">Ange **värdtyp** för**Service Fabric-kluster**.</span><span class="sxs-lookup"><span data-stu-id="2b166-189">Set **Host Type** too**Service Fabric Cluster**.</span></span>

<span data-ttu-id="2b166-190">Ange **målvärden** toohello service fabric-kluster som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="2b166-190">Set **Target Host** toohello service fabric cluster you created in hello previous section.</span></span>

<span data-ttu-id="2b166-191">Välj en **behållare registret** toopublish behållare för att.</span><span class="sxs-lookup"><span data-stu-id="2b166-191">Choose a **Container Registry** toopublish your container to.</span></span>

>[!TIP]
><span data-ttu-id="2b166-192">Använd hello **redigera** knappen toocreate ett register för behållaren.</span><span class="sxs-lookup"><span data-stu-id="2b166-192">Use hello **Edit** button toocreate a container registry.</span></span>

<span data-ttu-id="2b166-193">Tryck på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2b166-193">Press **OK**.</span></span>

![installationsprogrammet för service fabric kontinuerlig integration][image-setup-ci]
   
   <span data-ttu-id="2b166-195">När hello konfigurationen har slutförts är din behållaren distribuerade tooService Fabric.</span><span class="sxs-lookup"><span data-stu-id="2b166-195">Once hello configuration is completed, your container is deployed tooService Fabric.</span></span> <span data-ttu-id="2b166-196">När du trycker på uppdateringar toohello databasen körs en ny version och utgåva.</span><span class="sxs-lookup"><span data-stu-id="2b166-196">Whenever you push updates toohello repository a new build and release is executed.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="2b166-197">Skapa hello behållaren bilder tar ungefär 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="2b166-197">Building hello container images take approximately 15 minutes.</span></span>
   ><span data-ttu-id="2b166-198">hello första distributionen toohello Service Fabric-klustret gör hello grundläggande Windows Server Core behållaren bilder toobe hämtas.</span><span class="sxs-lookup"><span data-stu-id="2b166-198">hello first deployment toohello Service Fabric cluster causes hello base Windows Server Core container images toobe downloaded.</span></span> <span data-ttu-id="2b166-199">hello download tar toocomplete ytterligare 5-10 minuter.</span><span class="sxs-lookup"><span data-stu-id="2b166-199">hello download takes additional 5-10 minutes toocomplete.</span></span>

<span data-ttu-id="2b166-200">Bläddra toohello Fabrikam Callcenter program med hjälp av hello url för klustret: till exempel *http://mycluster.westeurope.cloudapp.azure.com*</span><span class="sxs-lookup"><span data-stu-id="2b166-200">Browse toohello Fabrikam Call Center application using hello url of your cluster: for example, *http://mycluster.westeurope.cloudapp.azure.com*</span></span>

<span data-ttu-id="2b166-201">Nu när du har av och distribuerats hello Fabrikam Callcenter lösningen, kan du öppna hello [Azure-portalen] [ link-azure-portal] och se hello-program som körs i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="2b166-201">Now that you have containerized and deployed hello Fabrikam Call Center solution, you can open hello [Azure portal][link-azure-portal] and see hello application running in Service Fabric.</span></span> <span data-ttu-id="2b166-202">tootry hello program, öppna en webbläsare och gå toohello URL för Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="2b166-202">tootry hello application, open a web browser and go toohello URL of your Service Fabric cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b166-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b166-203">Next steps</span></span>

<span data-ttu-id="2b166-204">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="2b166-204">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2b166-205">Skapa en Docker-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b166-205">Create a Docker project in Visual Studio</span></span>
> * <span data-ttu-id="2b166-206">Containerize ett befintligt program</span><span class="sxs-lookup"><span data-stu-id="2b166-206">Containerize an existing application</span></span>
> * <span data-ttu-id="2b166-207">Installationsprogrammet kontinuerlig integrering med Visual Studio och VSTS</span><span class="sxs-lookup"><span data-stu-id="2b166-207">Setup continuous integration with Visual Studio and VSTS</span></span>

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance toohello next tutorial toolearn how toobind a custom SSL certificate tooit.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate tooAzure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
