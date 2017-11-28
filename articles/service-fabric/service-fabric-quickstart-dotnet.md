---
title: aaaCreate ett .NET Service Fabric-program i Azure | Microsoft Docs
description: "Skapa ett .NET-program för Azure med hjälp av hello Service Fabric Snabbstart exempel."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.openlocfilehash: 20ef88dbf9fb0def31234ef05679a19009b9b529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-net-service-fabric-application-in-azure"></a><span data-ttu-id="c00ae-103">Skapa ett .NET Service Fabric-program i Azure</span><span class="sxs-lookup"><span data-stu-id="c00ae-103">Create a .NET Service Fabric application in Azure</span></span>
<span data-ttu-id="c00ae-104">Azure Service Fabric är en plattform för distribuerade system för distribution och hantering av skalbara och tillförlitliga mikrotjänster och behållare.</span><span class="sxs-lookup"><span data-stu-id="c00ae-104">Azure Service Fabric is a distributed systems platform for deploying and managing scalable and reliable microservices and containers.</span></span> 

<span data-ttu-id="c00ae-105">Den här snabbstarten visar hur toodeploy din första .NET application tooService Fabric.</span><span class="sxs-lookup"><span data-stu-id="c00ae-105">This quickstart shows how toodeploy your first .NET application tooService Fabric.</span></span> <span data-ttu-id="c00ae-106">När du är klar har röstningsapp med en ASP.NET Core webbklientdelen som sparar röstning resultat i en tillståndskänslig backend-tjänst i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="c00ae-106">When you're finished, you have a voting application with an ASP.NET Core web front-end that saves voting results in a stateful back-end service in hello cluster.</span></span>

![Skärmbild av programmet](./media/service-fabric-quickstart-dotnet/application-screenshot.png)

<span data-ttu-id="c00ae-108">Med det här programmet som du lär dig hur du:</span><span class="sxs-lookup"><span data-stu-id="c00ae-108">Using this application you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="c00ae-109">Skapa ett program med .NET och Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c00ae-109">Create an application using .NET and Service Fabric</span></span>
> * <span data-ttu-id="c00ae-110">Använda ASP.NET core som en webbklientdel</span><span class="sxs-lookup"><span data-stu-id="c00ae-110">Use ASP.NET core as a web front-end</span></span>
> * <span data-ttu-id="c00ae-111">Lagra programdata i en tillståndskänslig service</span><span class="sxs-lookup"><span data-stu-id="c00ae-111">Store application data in a stateful service</span></span>
> * <span data-ttu-id="c00ae-112">Felsöka programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="c00ae-112">Debug your application locally</span></span>
> * <span data-ttu-id="c00ae-113">Distribuera hello programmet tooa kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="c00ae-113">Deploy hello application tooa cluster in Azure</span></span>
> * <span data-ttu-id="c00ae-114">Skalbar hello program över flera noder</span><span class="sxs-lookup"><span data-stu-id="c00ae-114">Scale-out hello application across multiple nodes</span></span>
> * <span data-ttu-id="c00ae-115">Utföra en uppgradering av programmet</span><span class="sxs-lookup"><span data-stu-id="c00ae-115">Perform a rolling application upgrade</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c00ae-116">Krav</span><span class="sxs-lookup"><span data-stu-id="c00ae-116">Prerequisites</span></span>
<span data-ttu-id="c00ae-117">toocomplete denna Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="c00ae-117">toocomplete this quickstart:</span></span>
1. <span data-ttu-id="c00ae-118">[Installera Visual Studio 2017](https://www.visualstudio.com/) med hello **Azure-utveckling** och **ASP.NET och web development** arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="c00ae-118">[Install Visual Studio 2017](https://www.visualstudio.com/) with hello **Azure development** and **ASP.NET and web development** workloads.</span></span>
2. [<span data-ttu-id="c00ae-119">Installera Git</span><span class="sxs-lookup"><span data-stu-id="c00ae-119">Install Git</span></span>](https://git-scm.com/)
3. [<span data-ttu-id="c00ae-120">Installera hello Microsoft Azure Service Fabric-SDK</span><span class="sxs-lookup"><span data-stu-id="c00ae-120">Install hello Microsoft Azure Service Fabric SDK</span></span>](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK)
4. <span data-ttu-id="c00ae-121">Kör hello efter kommandot tooenable Visual Studio toodeploy toohello lokala Service Fabric-kluster:</span><span class="sxs-lookup"><span data-stu-id="c00ae-121">Run hello following command tooenable Visual Studio toodeploy toohello local Service Fabric cluster:</span></span>
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
    ```

## <a name="download-hello-sample"></a><span data-ttu-id="c00ae-122">Hämta hello-exempel</span><span class="sxs-lookup"><span data-stu-id="c00ae-122">Download hello sample</span></span>
<span data-ttu-id="c00ae-123">Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett kommandofönster.</span><span class="sxs-lookup"><span data-stu-id="c00ae-123">In a command window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>
```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="run-hello-application-locally"></a><span data-ttu-id="c00ae-124">Kör hello programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="c00ae-124">Run hello application locally</span></span>
<span data-ttu-id="c00ae-125">Högerklicka på hello Visual Studio-ikonen i hello Start-menyn och välj **kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-125">Right-click hello Visual Studio icon in hello Start Menu and choose **Run as administrator**.</span></span> <span data-ttu-id="c00ae-126">I ordning tooattach hello felsökare tooyour tjänster behöver du toorun Visual Studio som administratör.</span><span class="sxs-lookup"><span data-stu-id="c00ae-126">In order tooattach hello debugger tooyour services, you need toorun Visual Studio as administrator.</span></span>

<span data-ttu-id="c00ae-127">Öppna hello **Voting.sln** Visual Studio-lösning från hello lagringsplats du har klonat.</span><span class="sxs-lookup"><span data-stu-id="c00ae-127">Open hello **Voting.sln** Visual Studio solution from hello repository you cloned.</span></span>

<span data-ttu-id="c00ae-128">toodeploy hello program, tryck på **F5**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-128">toodeploy hello application, press **F5**.</span></span>

> [!NOTE]
> <span data-ttu-id="c00ae-129">hello skapar första gången du kör och distribuera programmet hello, Visual Studio ett lokala kluster för felsökning.</span><span class="sxs-lookup"><span data-stu-id="c00ae-129">hello first time you run and deploy hello application, Visual Studio creates a local cluster for debugging.</span></span> <span data-ttu-id="c00ae-130">Den här åtgärden kan ta lite tid.</span><span class="sxs-lookup"><span data-stu-id="c00ae-130">This operation may take some time.</span></span> <span data-ttu-id="c00ae-131">hello klustret Skapandestatus visas i utdatafönstret för hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c00ae-131">hello cluster creation status is displayed in hello Visual Studio output window.</span></span>

<span data-ttu-id="c00ae-132">När hello distributionen är klar, öppnar en webbläsare och öppna den här sidan: `http://localhost:8080` -hello webbklientdelen av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="c00ae-132">When hello deployment is complete, launch a browser and open this page: `http://localhost:8080` - hello web front-end of hello application.</span></span>

![Programmet frontend](./media/service-fabric-quickstart-dotnet/application-screenshot-new.png)

<span data-ttu-id="c00ae-134">Du kan nu lägga till en uppsättning röstning alternativ och starta tar röster.</span><span class="sxs-lookup"><span data-stu-id="c00ae-134">You can now add a set of voting options, and start taking votes.</span></span> <span data-ttu-id="c00ae-135">hello-program körs och lagrar alla data i ditt Service Fabric-kluster utan hello krävs en separat databas.</span><span class="sxs-lookup"><span data-stu-id="c00ae-135">hello application runs and stores all data in your Service Fabric cluster, without hello need for a separate database.</span></span>

## <a name="walk-through-hello-voting-sample-application"></a><span data-ttu-id="c00ae-136">Gå igenom hello röstning exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="c00ae-136">Walk through hello voting sample application</span></span>
<span data-ttu-id="c00ae-137">hello röstning program består av två tjänster:</span><span class="sxs-lookup"><span data-stu-id="c00ae-137">hello voting application consists of two services:</span></span>
- <span data-ttu-id="c00ae-138">Web frontend-tjänst (VotingWeb) – en ASP.NET Core webbtjänsten frontend, vilket fungerar webbsidan hello och visar webb-API: er toocommunicate med hello backend-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c00ae-138">Web front-end service (VotingWeb)- An ASP.NET Core web front-end service, which serves hello web page and exposes web APIs toocommunicate with hello backend service.</span></span>
- <span data-ttu-id="c00ae-139">Backend-tjänst (VotingData)-ett ASP.NET Core webbtjänsten, som visar ett API toostore hello rösten resulterar i en tillförlitlig ordlista kvar på disken.</span><span class="sxs-lookup"><span data-stu-id="c00ae-139">Back-end service (VotingData)- An ASP.NET Core web service, which exposes an API toostore hello vote results in a reliable dictionary persisted on disk.</span></span>

![Diagram över](./media/service-fabric-quickstart-dotnet/application-diagram.png)

<span data-ttu-id="c00ae-141">När du rösta i hello programmet hello följande inträffar händelser:</span><span class="sxs-lookup"><span data-stu-id="c00ae-141">When you vote in hello application hello following events occur:</span></span>
1. <span data-ttu-id="c00ae-142">En JavaScript skickar hello rösten begäran toohello webb-API i hello Frontend-webbtjänsten som ett HTTP PUT-begäran.</span><span class="sxs-lookup"><span data-stu-id="c00ae-142">A JavaScript sends hello vote request toohello web API in hello web front-end service as an HTTP PUT request.</span></span>

2. <span data-ttu-id="c00ae-143">hello Frontend-webbtjänsten använder en proxy-toolocate och vidarebefordra en HTTP PUT-begäran toohello backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="c00ae-143">hello web front-end service uses a proxy toolocate and forward an HTTP PUT request toohello back-end service.</span></span>

3. <span data-ttu-id="c00ae-144">hello backend-tjänst tar hello inkommande begäran och lagrar hello uppdateras resultera i en tillförlitlig ordlista som hämtar replikerade toomultiple noder i klustret hello och kvar på disken.</span><span class="sxs-lookup"><span data-stu-id="c00ae-144">hello back-end service takes hello incoming request, and stores hello updated result in a reliable dictionary, which gets replicated toomultiple nodes within hello cluster and persisted on disk.</span></span> <span data-ttu-id="c00ae-145">Alla hello programdata lagras i hello klustret, så det behövs ingen databas.</span><span class="sxs-lookup"><span data-stu-id="c00ae-145">All hello application's data is stored in hello cluster, so no database is needed.</span></span>

## <a name="debug-in-visual-studio"></a><span data-ttu-id="c00ae-146">Felsökning i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c00ae-146">Debug in Visual Studio</span></span>
<span data-ttu-id="c00ae-147">När du felsöker programmet i Visual Studio använder du en lokal utveckling Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="c00ae-147">When debugging application in Visual Studio, you are using a local Service Fabric development cluster.</span></span> <span data-ttu-id="c00ae-148">Du har hello alternativet tooadjust felsökning upplevelse tooyour scenariot.</span><span class="sxs-lookup"><span data-stu-id="c00ae-148">You have hello option tooadjust your debugging experience tooyour scenario.</span></span> <span data-ttu-id="c00ae-149">I det här programmet lagrar vi data i vår backend-tjänst med hjälp av en tillförlitlig ordlista.</span><span class="sxs-lookup"><span data-stu-id="c00ae-149">In this application, we store data in our back-end service, using a reliable dictionary.</span></span> <span data-ttu-id="c00ae-150">Visual Studio tar bort hello program per standard när du stoppar hello felsökare.</span><span class="sxs-lookup"><span data-stu-id="c00ae-150">Visual Studio removes hello application per default when you stop hello debugger.</span></span> <span data-ttu-id="c00ae-151">Ta bort programmet hello medför hello data i hello backend-tjänst tooalso tas bort.</span><span class="sxs-lookup"><span data-stu-id="c00ae-151">Removing hello application causes hello data in hello back-end service tooalso be removed.</span></span> <span data-ttu-id="c00ae-152">toopersist hello data mellan felsökning sessioner, kan du ändra hello **programmet felsökningsläge** som en egenskap på hello **Röstningsdatabasen** projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c00ae-152">toopersist hello data between debugging sessions, you can change hello **Application Debug Mode** as a property on hello **Voting** project in Visual Studio.</span></span>

<span data-ttu-id="c00ae-153">toolook på vad som händer i hello kod, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c00ae-153">toolook at what happens in hello code, complete hello following steps:</span></span>
1. <span data-ttu-id="c00ae-154">Öppna hello **VotesController.cs** fil och anger en brytpunkt i hello webb-API: er **placera** metod (rad 47) – du kan söka efter hello-filen i hello Solution Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c00ae-154">Open hello **VotesController.cs** file and set a breakpoint in hello web API's **Put** method (line 47) - You can search for hello file in hello Solution Explorer in Visual Studio.</span></span>

2. <span data-ttu-id="c00ae-155">Öppna hello **VoteDataController.cs** fil och anger en brytpunkt i den här web API **placera** metod (rad 50).</span><span class="sxs-lookup"><span data-stu-id="c00ae-155">Open hello **VoteDataController.cs** file and set a breakpoint in this web API's **Put** method (line 50).</span></span>

3. <span data-ttu-id="c00ae-156">Gå tillbaka toohello webbläsare och klicka på ett alternativ för röstning eller lägga till ett nytt alternativ för röstning.</span><span class="sxs-lookup"><span data-stu-id="c00ae-156">Go back toohello browser and click a voting option or add a new voting option.</span></span> <span data-ttu-id="c00ae-157">Du kan träffa hello första brytpunkt i hello web front slutpunkts api-kontrollanten.</span><span class="sxs-lookup"><span data-stu-id="c00ae-157">You hit hello first breakpoint in hello web front-end's api controller.</span></span>
    - <span data-ttu-id="c00ae-158">Detta är där hello JavaScript i hello webbläsaren skickar en begäran toohello web API-kontrollanten i hello frontend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="c00ae-158">This is where hello JavaScript in hello browser sends a request toohello web API controller in hello front-end service.</span></span>
    
    ![Lägg till röst frontend-tjänst](./media/service-fabric-quickstart-dotnet/addvote-frontend.png)

    - <span data-ttu-id="c00ae-160">Först skapar vi hello URL toohello ReverseProxy för vår backend-tjänst **(1)**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-160">First we construct hello URL toohello ReverseProxy for our back-end service **(1)**.</span></span>
    - <span data-ttu-id="c00ae-161">Vi skickar hello HTTP PUT-begäran om toohello ReverseProxy **(2)**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-161">Then we send hello HTTP PUT Request toohello ReverseProxy **(2)**.</span></span>
    - <span data-ttu-id="c00ae-162">Slutligen hello returnerar vi hello svar från hello backend-tjänst toohello klient **(3)**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-162">Finally hello we return hello response from hello back-end service toohello client **(3)**.</span></span>

4. <span data-ttu-id="c00ae-163">Tryck på **F5** toocontinue</span><span class="sxs-lookup"><span data-stu-id="c00ae-163">Press **F5** toocontinue</span></span>
    - <span data-ttu-id="c00ae-164">Du är nu vid hello break i hello backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="c00ae-164">You are now at hello break point in hello back-end service.</span></span>
    
    ![Lägga till röst backend-tjänst](./media/service-fabric-quickstart-dotnet/addvote-backend.png)

    - <span data-ttu-id="c00ae-166">I hello första raden i hello metoden **(1)** vi använder hello `StateManager` tooget eller Lägg till en tillförlitlig ordlista som heter `counts`.</span><span class="sxs-lookup"><span data-stu-id="c00ae-166">In hello first line in hello method **(1)** we are using hello `StateManager` tooget or add a reliable dictionary called `counts`.</span></span>
    - <span data-ttu-id="c00ae-167">All interaktion med värden i en tillförlitlig ordlista kräver en transaktion detta med hjälp av instruktionen **(2)** skapar den aktuella transaktionen.</span><span class="sxs-lookup"><span data-stu-id="c00ae-167">All interactions with values in a reliable dictionary require a transaction, this using statement **(2)** creates that transaction.</span></span>
    - <span data-ttu-id="c00ae-168">I hello transaktion vi sedan uppdatera hello värdet för hello relevanta nyckeln för hello röstning alternativet och incheckningar hello åtgärden **(3)**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-168">In hello transaction, we then update hello value of hello relevant key for hello voting option and commits hello operation **(3)**.</span></span> <span data-ttu-id="c00ae-169">När hello checkat returnerar metoden hello data uppdateras i hello ordlista och replikeras tooother noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="c00ae-169">Once hello commit method returns, hello data is updated in hello dictionary and replicated tooother nodes in hello cluster.</span></span> <span data-ttu-id="c00ae-170">hello data lagras nu på ett säkert sätt i hello kluster och hello backend-tjänst kan växlas tooother noder och fortfarande har hello data är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="c00ae-170">hello data is now safely stored in hello cluster, and hello back-end service can fail over tooother nodes, still having hello data available.</span></span>
5. <span data-ttu-id="c00ae-171">Tryck på **F5** toocontinue</span><span class="sxs-lookup"><span data-stu-id="c00ae-171">Press **F5** toocontinue</span></span>

<span data-ttu-id="c00ae-172">toostop hello felsökningssessionen, tryck på **SKIFT + F5**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-172">toostop hello debugging session, press **Shift+F5**.</span></span>

## <a name="deploy-hello-application-tooazure"></a><span data-ttu-id="c00ae-173">Distribuera hello programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="c00ae-173">Deploy hello application tooAzure</span></span>
<span data-ttu-id="c00ae-174">toodeploy hello programmet tooa klustret i Azure, du kan antingen välja toocreate ditt eget kluster eller Använd en part klustret.</span><span class="sxs-lookup"><span data-stu-id="c00ae-174">toodeploy hello application tooa cluster in Azure, you can either choose toocreate your own cluster, or use a Party Cluster.</span></span>

<span data-ttu-id="c00ae-175">Part kluster är ledigt, tidsbegränsade Service Fabric-kluster i Azure och kör hello Service Fabric-grupp där alla kan distribuera program och lär dig mer om hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="c00ae-175">Party clusters are free, limited-time Service Fabric clusters hosted on Azure and run by hello Service Fabric team where anyone can deploy applications and learn about hello platform.</span></span> <span data-ttu-id="c00ae-176">tooget åtkomst tooa part klustret [Följ instruktionerna för hello](http://aka.ms/tryservicefabric).</span><span class="sxs-lookup"><span data-stu-id="c00ae-176">tooget access tooa Party Cluster, [follow hello instructions](http://aka.ms/tryservicefabric).</span></span> 

<span data-ttu-id="c00ae-177">Information om hur du skapar ett eget kluster finns i [Skapa ditt första Service Fabric-kluster i Azure](service-fabric-get-started-azure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="c00ae-177">For information about creating your own cluster, see [Create your first Service Fabric cluster on Azure](service-fabric-get-started-azure-cluster.md).</span></span>

> [!Note]
> <span data-ttu-id="c00ae-178">hello Frontend-webbtjänsten är konfigurerade toolisten på port 8080 för inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="c00ae-178">hello web front-end service is configured toolisten on port 8080 for incoming traffic.</span></span> <span data-ttu-id="c00ae-179">Se till att den porten är öppen i ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="c00ae-179">Make sure that port is open in your cluster.</span></span> <span data-ttu-id="c00ae-180">Om du använder hello part klustret är den här porten öppen.</span><span class="sxs-lookup"><span data-stu-id="c00ae-180">If you are using hello Party Cluster, this port is open.</span></span>
>

### <a name="deploy-hello-application-using-visual-studio"></a><span data-ttu-id="c00ae-181">Distribuera hello program med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c00ae-181">Deploy hello application using Visual Studio</span></span>
<span data-ttu-id="c00ae-182">Nu när programmet hello är klar, kan du distribuera den tooa kluster direkt från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c00ae-182">Now that hello application is ready, you can deploy it tooa cluster directly from Visual Studio.</span></span>

1. <span data-ttu-id="c00ae-183">Högerklicka på **Röstningsdatabasen** hello i Solution Explorer och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-183">Right-click **Voting** in hello Solution Explorer and choose **Publish**.</span></span> <span data-ttu-id="c00ae-184">hello publicera dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="c00ae-184">hello Publish dialog appears.</span></span>

    ![Dialogrutan Publicera](./media/service-fabric-quickstart-dotnet/publish-app.png)

2. <span data-ttu-id="c00ae-186">Ange hello Anslutningens slutpunkt för hello-kluster i hello **Anslutningens slutpunkt** fältet och klickar på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-186">Type in hello Connection Endpoint of hello cluster in hello **Connection Endpoint** field and click **Publish**.</span></span> <span data-ttu-id="c00ae-187">När du registrerar dig för hello part klustret tillhandahålls hello Anslutningens slutpunkt i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="c00ae-187">When signing up for hello Party Cluster, hello Connection Endpoint is provided in hello browser.</span></span> <span data-ttu-id="c00ae-188">– till exempel `winh1x87d1d.westus.cloudapp.azure.com:19000`.</span><span class="sxs-lookup"><span data-stu-id="c00ae-188">- for example, `winh1x87d1d.westus.cloudapp.azure.com:19000`.</span></span>

3. <span data-ttu-id="c00ae-189">Öppna en webbläsare och Skriv i hello klusteradress - exempelvis `http://winh1x87d1d.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c00ae-189">Open a browser and type in hello cluster address - for example, `http://winh1x87d1d.westus.cloudapp.azure.com`.</span></span> <span data-ttu-id="c00ae-190">Du bör nu se hello-program som körs i hello kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="c00ae-190">You should now see hello application running in hello cluster in Azure.</span></span>

![Programmet frontend](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)

## <a name="scale-applications-and-services-in-a-cluster"></a><span data-ttu-id="c00ae-192">Skala program och tjänster i ett kluster</span><span class="sxs-lookup"><span data-stu-id="c00ae-192">Scale applications and services in a cluster</span></span>
<span data-ttu-id="c00ae-193">Service Fabric-tjänster kan enkelt skalas över ett kluster tooaccommodate för en ändring i hello belastningen på hello-tjänster.</span><span class="sxs-lookup"><span data-stu-id="c00ae-193">Service Fabric services can easily be scaled across a cluster tooaccommodate for a change in hello load on hello services.</span></span> <span data-ttu-id="c00ae-194">Du kan skala en tjänst genom att ändra hello antalet instanser som körs i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="c00ae-194">You scale a service by changing hello number of instances running in hello cluster.</span></span> <span data-ttu-id="c00ae-195">Du har flera olika sätt att skala dina tjänster kan du använda skript eller kommandon från PowerShell eller CLI för Service Fabric (sfctl).</span><span class="sxs-lookup"><span data-stu-id="c00ae-195">You have multiple ways of scaling your services, you can use scripts or commands from PowerShell or Service Fabric CLI (sfctl).</span></span> <span data-ttu-id="c00ae-196">I det här exemplet använder vi Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="c00ae-196">In this example, we are using Service Fabric Explorer.</span></span>

<span data-ttu-id="c00ae-197">Service Fabric Explorer körs i alla Service Fabric-kluster och kan nås från en webbläsare genom att bläddra toohello kluster HTTP port på hanteringsservern (19080), till exempel `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span><span class="sxs-lookup"><span data-stu-id="c00ae-197">Service Fabric Explorer runs in all Service Fabric clusters and can be accessed from a browser, by browsing toohello clusters HTTP management port (19080), for example, `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span></span>

<span data-ttu-id="c00ae-198">tooscale hello Frontend webbtjänsten, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c00ae-198">tooscale hello web front-end service, do hello following steps:</span></span>

1. <span data-ttu-id="c00ae-199">Öppna Service Fabric Explorer i ditt kluster, till exempel `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span><span class="sxs-lookup"><span data-stu-id="c00ae-199">Open Service Fabric Explorer in your cluster - for example,`http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span></span>
2. <span data-ttu-id="c00ae-200">Klicka på nästa toohello för hello ellips (tre punkter) **fabric: / Röstningsdatabasen/VotingWeb** noden i TreeView-kontrollen hello och välj **skala Service**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-200">Click on hello ellipsis (three dots) next toohello **fabric:/Voting/VotingWeb** node in hello treeview and choose **Scale Service**.</span></span>

    ![Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scale.png)

    <span data-ttu-id="c00ae-202">Nu kan du välja tooscale hello antal instanser av hello Frontend-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="c00ae-202">You can now choose tooscale hello number of instances of hello web front-end service.</span></span>

3. <span data-ttu-id="c00ae-203">Ändra hello nummer för**2** och på **skala Service**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-203">Change hello number too**2** and click **Scale Service**.</span></span>
4. <span data-ttu-id="c00ae-204">Klicka på hello **fabric: / Röstningsdatabasen/VotingWeb** nod i hello trädvy och expandera hello partition nod (representerade av GUID).</span><span class="sxs-lookup"><span data-stu-id="c00ae-204">Click on hello **fabric:/Voting/VotingWeb** node in hello tree-view and expand hello partition node (represented by a GUID).</span></span>

    ![Service Fabric Explorer skala Service](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scaled-service.png)

    <span data-ttu-id="c00ae-206">Du kan nu se att hello-tjänsten har två instanser och hello trädvyn visas i vilka noder hello-instanser körs på.</span><span class="sxs-lookup"><span data-stu-id="c00ae-206">You can now see that hello service has two instances, and in hello tree view you see which nodes hello instances run on.</span></span>

<span data-ttu-id="c00ae-207">Enkel hantering uppgiften dubblerad vi hello-resurser som är tillgängliga för våra klienttjänst tooprocess användare.</span><span class="sxs-lookup"><span data-stu-id="c00ae-207">By this simple management task, we doubled hello resources available for our front-end service tooprocess user load.</span></span> <span data-ttu-id="c00ae-208">Det är viktigt toounderstand som du inte behöver flera instanser av en tjänst toohave den köras på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="c00ae-208">It's important toounderstand that you do not need multiple instances of a service toohave it run reliably.</span></span> <span data-ttu-id="c00ae-209">Om en tjänst misslyckas, säkerställer Service Fabric en ny instans av tjänsten som körs i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="c00ae-209">If a service fails, Service Fabric makes sure a new service instance runs in hello cluster.</span></span>

## <a name="perform-a-rolling-application-upgrade"></a><span data-ttu-id="c00ae-210">Utföra en uppgradering av programmet</span><span class="sxs-lookup"><span data-stu-id="c00ae-210">Perform a rolling application upgrade</span></span>
<span data-ttu-id="c00ae-211">När du distribuerar nya uppdateringar tooyour programmet samlar Service Fabric ut hello uppdatering på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="c00ae-211">When deploying new updates tooyour application, Service Fabric rolls out hello update in a safe way.</span></span> <span data-ttu-id="c00ae-212">Rullande uppgraderingar får du inget driftstopp medan uppgraderas samt automatisk återställning fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="c00ae-212">Rolling upgrades gives you no downtime while upgrading as well as automated rollback should errors occur.</span></span>

<span data-ttu-id="c00ae-213">tooupgrade Hej program, hello följande:</span><span class="sxs-lookup"><span data-stu-id="c00ae-213">tooupgrade hello application, do hello following:</span></span>

1. <span data-ttu-id="c00ae-214">Öppna hello **Index.cshtml** filen i Visual Studio – du kan söka efter hello-filen i hello Solution Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c00ae-214">Open hello **Index.cshtml** file in Visual Studio - You can search for hello file in hello Solution Explorer in Visual Studio.</span></span>
2. <span data-ttu-id="c00ae-215">Ändra hello rubrik på hello sidan genom att lägga till text - till exempel.</span><span class="sxs-lookup"><span data-stu-id="c00ae-215">Change hello heading on hello page by adding some text - for example.</span></span>
    ```html
        <div class="col-xs-8 col-xs-offset-2 text-center">
            <h2>Service Fabric Voting Sample v2</h2>
        </div>
    ```
3. <span data-ttu-id="c00ae-216">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="c00ae-216">Save hello file.</span></span>
4. <span data-ttu-id="c00ae-217">Högerklicka på **Röstningsdatabasen** hello i Solution Explorer och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-217">Right-click **Voting** in hello Solution Explorer and choose **Publish**.</span></span> <span data-ttu-id="c00ae-218">hello publicera dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="c00ae-218">hello Publish dialog appears.</span></span>
5. <span data-ttu-id="c00ae-219">Klicka på hello **Manifest Version** knappen toochange hello version av hello-tjänst och program.</span><span class="sxs-lookup"><span data-stu-id="c00ae-219">Click hello **Manifest Version** button toochange hello version of hello service and application.</span></span>
6. <span data-ttu-id="c00ae-220">Ändra hello version av hello **kod** elementet under **VotingWebPkg** för ”2.0.0”, till exempel och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-220">Change hello version of hello **Code** element under **VotingWebPkg** too"2.0.0", for example, and click **Save**.</span></span>

    ![Ändra Version dialogrutan](./media/service-fabric-quickstart-dotnet/change-version.png)
7. <span data-ttu-id="c00ae-222">I hello **publicera Fabric tjänstprogrammet** dialogrutan, kontrollera hello uppgradera hello programmet kryssrutan och klickar på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="c00ae-222">In hello **Publish Service Fabric Application** dialog, check hello Upgrade hello Application checkbox, and click **Publish**.</span></span>

    ![Dialogrutan Publicera uppgradera inställningen](./media/service-fabric-quickstart-dotnet/upgrade-app.png)
8. <span data-ttu-id="c00ae-224">Öppna webbläsaren och bläddra till exempel toohello klusteradress på port 19080 - `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span><span class="sxs-lookup"><span data-stu-id="c00ae-224">Open your browser and browse toohello cluster address on port 19080 - for example, `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span></span>
9. <span data-ttu-id="c00ae-225">Klicka på hello **program** nod i trädvyn hello och sedan **uppgraderingar pågår** i hello högra rutan.</span><span class="sxs-lookup"><span data-stu-id="c00ae-225">Click on hello **Applications** node in hello tree view, and then **Upgrades in Progress** in hello right-hand pane.</span></span> <span data-ttu-id="c00ae-226">Du ser hur hello uppgraderingen samlas via hello uppgraderingsdomäner i klustret, kontrollera att varje domän är felfri innan du fortsätter toohello nästa.</span><span class="sxs-lookup"><span data-stu-id="c00ae-226">You see how hello upgrade rolls through hello upgrade domains in your cluster, making sure each domain is healthy before proceeding toohello next.</span></span>
    <span data-ttu-id="c00ae-227">![Uppgradera vyn i Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/upgrading.png)</span><span class="sxs-lookup"><span data-stu-id="c00ae-227">![Upgrade View in Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/upgrading.png)</span></span>

    <span data-ttu-id="c00ae-228">Service Fabric är uppgraderingar säker väntar på två minuter efter en uppgradering hello-tjänsten på varje nod i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="c00ae-228">Service Fabric makes upgrades safe by waiting two minutes after upgrading hello service on each node in hello cluster.</span></span> <span data-ttu-id="c00ae-229">Förvänta dig hello hela uppdateringen tootake cirka åtta minuter.</span><span class="sxs-lookup"><span data-stu-id="c00ae-229">Expect hello entire update tootake approximately eight minutes.</span></span>

10. <span data-ttu-id="c00ae-230">Du kan fortfarande använda programmet hello medan hello uppgraderingen är igång.</span><span class="sxs-lookup"><span data-stu-id="c00ae-230">While hello upgrade is running, you can still use hello application.</span></span> <span data-ttu-id="c00ae-231">Eftersom du har två instanser av hello-tjänsten som körs i hello kluster, kan vissa av dina begäranden få en uppgraderad version av programmet hello, medan andra kan fortfarande få hello gammal version.</span><span class="sxs-lookup"><span data-stu-id="c00ae-231">Because you have two instances of hello service running in hello cluster, some of your requests may get an upgraded version of hello application, while others may still get hello old version.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c00ae-232">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c00ae-232">Next steps</span></span>
<span data-ttu-id="c00ae-233">I den här snabbstarten har du lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="c00ae-233">In this quickstart, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c00ae-234">Skapa ett program med .NET och Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c00ae-234">Create an application using .NET and Service Fabric</span></span>
> * <span data-ttu-id="c00ae-235">Använda ASP.NET core som en webbklientdel</span><span class="sxs-lookup"><span data-stu-id="c00ae-235">Use ASP.NET core as a web front-end</span></span>
> * <span data-ttu-id="c00ae-236">Lagra programdata i en tillståndskänslig service</span><span class="sxs-lookup"><span data-stu-id="c00ae-236">Store application data in a stateful service</span></span>
> * <span data-ttu-id="c00ae-237">Felsöka programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="c00ae-237">Debug your application locally</span></span>
> * <span data-ttu-id="c00ae-238">Distribuera hello programmet tooa kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="c00ae-238">Deploy hello application tooa cluster in Azure</span></span>
> * <span data-ttu-id="c00ae-239">Skalbar hello program över flera noder</span><span class="sxs-lookup"><span data-stu-id="c00ae-239">Scale-out hello application across multiple nodes</span></span>
> * <span data-ttu-id="c00ae-240">Utföra en uppgradering av programmet</span><span class="sxs-lookup"><span data-stu-id="c00ae-240">Perform a rolling application upgrade</span></span>

<span data-ttu-id="c00ae-241">toolearn mer om Service Fabric och .NET, ta en titt på den här kursen:</span><span class="sxs-lookup"><span data-stu-id="c00ae-241">toolearn more about Service Fabric and .NET, take a look at this tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c00ae-242">.NET-program på Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c00ae-242">.NET application on Service Fabric</span></span>](service-fabric-tutorial-create-dotnet-app.md)