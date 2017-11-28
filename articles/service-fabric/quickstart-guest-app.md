---
title: aaaQuickly distribuera ett befintligt app tooan Azure Service Fabric-kluster
description: "Använda en Azure Service Fabric-kluster toohost ett befintligt Node.js-program med Visual Studio."
services: service-fabric
documentationcenter: nodejs
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: adegeo
ms.openlocfilehash: 20a3eb4a9206ba465acf96d0976ba241b07158bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="host-a-nodejs-application-on-azure-service-fabric"></a><span data-ttu-id="34726-103">Skapa ett Node.js-program i Azure med Node.js</span><span class="sxs-lookup"><span data-stu-id="34726-103">Host a Node.js application on Azure Service Fabric</span></span>

<span data-ttu-id="34726-104">Den här snabbstarten hjälper dig att distribuera ett befintligt program (Node.js i det här exemplet) tooa Service Fabric-kluster körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="34726-104">This quickstart helps you deploy an existing application (Node.js in this example) tooa Service Fabric cluster running on Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34726-105">Krav</span><span class="sxs-lookup"><span data-stu-id="34726-105">Prerequisites</span></span>

<span data-ttu-id="34726-106">Du måste [konfigurera utvecklingsmiljön](service-fabric-get-started.md) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="34726-106">Before you get started, make sure that you have [set up your development environment](service-fabric-get-started.md).</span></span> <span data-ttu-id="34726-107">Vilket innefattar installerar hello Service Fabric-SDK och Visual Studio 2017 eller 2015.</span><span class="sxs-lookup"><span data-stu-id="34726-107">Which includes installing hello Service Fabric SDK and Visual Studio 2017 or 2015.</span></span>

<span data-ttu-id="34726-108">Du måste också toohave ett befintligt Node.js-program för distribution.</span><span class="sxs-lookup"><span data-stu-id="34726-108">You also need toohave an existing Node.js application for deployment.</span></span> <span data-ttu-id="34726-109">Denna snabbstart använder en enkel Node.js-webbplats som du kan hämta [här][download-sample].</span><span class="sxs-lookup"><span data-stu-id="34726-109">This quickstart uses a simple Node.js website that can be downloaded [here][download-sample].</span></span> <span data-ttu-id="34726-110">Extrahera den här filen tooyour `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` mapp när du skapar hello-projekt i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="34726-110">Extract this file tooyour `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` folder after you create hello project in hello next step.</span></span>

<span data-ttu-id="34726-111">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto][create-account] innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="34726-111">If you don't have an Azure subscription, create a [free account][create-account].</span></span>

## <a name="create-hello-service"></a><span data-ttu-id="34726-112">Skapa hello-tjänst</span><span class="sxs-lookup"><span data-stu-id="34726-112">Create hello service</span></span>

<span data-ttu-id="34726-113">Starta Visual Studio som **administratör**.</span><span class="sxs-lookup"><span data-stu-id="34726-113">Launch Visual Studio as an **administrator**.</span></span>

<span data-ttu-id="34726-114">Skapa ett projekt med `CTRL`+`SHIFT`+`N`</span><span class="sxs-lookup"><span data-stu-id="34726-114">Create a project with `CTRL`+`SHIFT`+`N`</span></span>

<span data-ttu-id="34726-115">I hello **nytt projekt** dialogrutan Välj **molntjänster > Fabric tjänstprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="34726-115">In hello **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

<span data-ttu-id="34726-116">Namnge programmet hello **MyGuestApp** och tryck på **OK**.</span><span class="sxs-lookup"><span data-stu-id="34726-116">Name hello application **MyGuestApp** and press **OK**.</span></span>

>[!IMPORTANT]
><span data-ttu-id="34726-117">Node.js kan enkelt dela hello 260 tecken för sökvägar som windows har.</span><span class="sxs-lookup"><span data-stu-id="34726-117">Node.js can easily break hello 260 character limit for paths that windows has.</span></span> <span data-ttu-id="34726-118">Använd en kort sökväg för själva hello-projektet som **c:\code\svc1**.</span><span class="sxs-lookup"><span data-stu-id="34726-118">Use a short path for hello project itself such as **c:\code\svc1**.</span></span>
   
![Dialogrutan Nytt projekt i Visual Studio][new-project]

<span data-ttu-id="34726-120">Du kan skapa alla typer av Service Fabric-tjänsten från hello nästa dialogruta.</span><span class="sxs-lookup"><span data-stu-id="34726-120">You can create any type of Service Fabric service from hello next dialog.</span></span> <span data-ttu-id="34726-121">För den här snabbstarten, välj **kan köras av gäst**.</span><span class="sxs-lookup"><span data-stu-id="34726-121">For this quickstart, choose **Guest Executable**.</span></span>

<span data-ttu-id="34726-122">Namnge hello service **MyGuestService** och ange hello alternativ på hello rätt toohello följande värden:</span><span class="sxs-lookup"><span data-stu-id="34726-122">Name hello service **MyGuestService** and set hello options on hello right toohello following values:</span></span>

| <span data-ttu-id="34726-123">Inställning</span><span class="sxs-lookup"><span data-stu-id="34726-123">Setting</span></span>                   | <span data-ttu-id="34726-124">Värde</span><span class="sxs-lookup"><span data-stu-id="34726-124">Value</span></span> |
| ------------------------- | ------ |
| <span data-ttu-id="34726-125">Kodpaketmapp</span><span class="sxs-lookup"><span data-stu-id="34726-125">Code Package Folder</span></span>       | <span data-ttu-id="34726-126">_&lt;hello mappen med din Node.js-app&gt;_</span><span class="sxs-lookup"><span data-stu-id="34726-126">_&lt;hello folder with your Node.js app&gt;_</span></span> |
| <span data-ttu-id="34726-127">Kodpaketbeteende</span><span class="sxs-lookup"><span data-stu-id="34726-127">Code Package Behavior</span></span>     | <span data-ttu-id="34726-128">Kopiera mappen innehållet tooproject</span><span class="sxs-lookup"><span data-stu-id="34726-128">Copy folder contents tooproject</span></span> |
| <span data-ttu-id="34726-129">Program</span><span class="sxs-lookup"><span data-stu-id="34726-129">Program</span></span>                   | <span data-ttu-id="34726-130">node.exe</span><span class="sxs-lookup"><span data-stu-id="34726-130">node.exe</span></span> |
| <span data-ttu-id="34726-131">Argument</span><span class="sxs-lookup"><span data-stu-id="34726-131">Arguments</span></span>                 | <span data-ttu-id="34726-132">server.js</span><span class="sxs-lookup"><span data-stu-id="34726-132">server.js</span></span> |
| <span data-ttu-id="34726-133">Arbetsmapp</span><span class="sxs-lookup"><span data-stu-id="34726-133">Working Folder</span></span>            | <span data-ttu-id="34726-134">Kodpaket</span><span class="sxs-lookup"><span data-stu-id="34726-134">CodePackage</span></span> |

<span data-ttu-id="34726-135">Tryck på **OK**.</span><span class="sxs-lookup"><span data-stu-id="34726-135">Press **OK**.</span></span>

![Dialogrutan Ny tjänst i Visual Studio][new-service]

<span data-ttu-id="34726-137">Visual Studio skapar hello projektet och hello aktören service-projekt och visar dem i Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="34726-137">Visual Studio creates hello application project and hello actor service project and displays them in Solution Explorer.</span></span>

<span data-ttu-id="34726-138">hello projektet (**MyGuestApp**) innehåller inte någon kod direkt.</span><span class="sxs-lookup"><span data-stu-id="34726-138">hello application project (**MyGuestApp**) does not contain any code directly.</span></span> <span data-ttu-id="34726-139">I stället refererar det till en uppsättning tjänstprojekt.</span><span class="sxs-lookup"><span data-stu-id="34726-139">Instead, it references a set of service projects.</span></span> <span data-ttu-id="34726-140">Dessutom innehåller det tre andra typer av innehåll:</span><span class="sxs-lookup"><span data-stu-id="34726-140">In addition, it contains three other types of content:</span></span>

* <span data-ttu-id="34726-141">**Publicera profiler**</span><span class="sxs-lookup"><span data-stu-id="34726-141">**Publish profiles**</span></span>  
<span data-ttu-id="34726-142">Verktygsinställningar för olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="34726-142">Tooling preferences for different environments.</span></span>

* <span data-ttu-id="34726-143">**Skript**</span><span class="sxs-lookup"><span data-stu-id="34726-143">**Scripts**</span></span>  
<span data-ttu-id="34726-144">PowerShell-skript för distribution/uppgradering av program.</span><span class="sxs-lookup"><span data-stu-id="34726-144">PowerShell script for deploying/upgrading your application.</span></span>

* <span data-ttu-id="34726-145">**Programdefinition**</span><span class="sxs-lookup"><span data-stu-id="34726-145">**Application definition**</span></span>  
<span data-ttu-id="34726-146">Innehåller hello programmanifestet under *ApplicationPackageRoot*.</span><span class="sxs-lookup"><span data-stu-id="34726-146">Includes hello application manifest under *ApplicationPackageRoot*.</span></span> <span data-ttu-id="34726-147">Associerade programfiler för parametern är *ApplicationParameters*, som definierar hello program och Tillåt tooconfigure den specifikt för en given miljö.</span><span class="sxs-lookup"><span data-stu-id="34726-147">Associated application parameter files are under *ApplicationParameters*, which define hello application and allow you tooconfigure it specifically for a given environment.</span></span>
    
<span data-ttu-id="34726-148">En översikt över hello innehållet i hello service-projekt, se [komma igång med Reliable Services](service-fabric-reliable-services-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="34726-148">For an overview of hello contents of hello service project, see [Getting started with Reliable Services](service-fabric-reliable-services-quick-start.md).</span></span>

## <a name="set-up-networking"></a><span data-ttu-id="34726-149">Konfigurera nätverk</span><span class="sxs-lookup"><span data-stu-id="34726-149">Set up networking</span></span>

<span data-ttu-id="34726-150">hello exempel Node.js-appen som vi distribuerar använder port **80** och vi behöver tootell Service Fabric som vi behöver för att öppna port.</span><span class="sxs-lookup"><span data-stu-id="34726-150">hello example Node.js app we're deploying uses port **80** and we need tootell Service Fabric that we need that port exposed.</span></span>

<span data-ttu-id="34726-151">Öppna hello **ServiceManifest.xml** fil i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="34726-151">Open hello **ServiceManifest.xml** file in hello project.</span></span> <span data-ttu-id="34726-152">Längst ned hello hello manifestet, det finns en `<Resources> \ <Endpoints>` med en transaktion som redan har definierats.</span><span class="sxs-lookup"><span data-stu-id="34726-152">At hello bottom of hello manifest, there is a `<Resources> \ <Endpoints>` with an entry already defined.</span></span> <span data-ttu-id="34726-153">Ändra den post tooadd `Port`, `Protocol`, och `Type`.</span><span class="sxs-lookup"><span data-stu-id="34726-153">Modify that entry tooadd `Port`, `Protocol`, and `Type`.</span></span> 

```xml
  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyGuestAppServiceTypeEndpoint" Port="80" Protocol="http" Type="Input" />
    </Endpoints>
  </Resources>
```

## <a name="deploy-tooazure"></a><span data-ttu-id="34726-154">Distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="34726-154">Deploy tooAzure</span></span>

<span data-ttu-id="34726-155">Om du trycker på **F5** och kör hello-projekt är det distribuerade toohello lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="34726-155">If you press **F5** and run hello project, it is deployed toohello local cluster.</span></span> <span data-ttu-id="34726-156">Men vi distribuera tooAzure i stället.</span><span class="sxs-lookup"><span data-stu-id="34726-156">However, let's deploy tooAzure instead.</span></span>

<span data-ttu-id="34726-157">Högerklicka på hello projektet och välj **publicera...**  som öppnar en dialogruta toopublish tooAzure.</span><span class="sxs-lookup"><span data-stu-id="34726-157">Right-click on hello project and choose **Publish...** which opens a dialog toopublish tooAzure.</span></span>

![Publicera tooazure dialogrutan för en tjänst för service fabric][publish]

<span data-ttu-id="34726-159">Välj hello **PublishProfiles\Cloud.xml** mål profil.</span><span class="sxs-lookup"><span data-stu-id="34726-159">Select hello **PublishProfiles\Cloud.xml** target profile.</span></span>

<span data-ttu-id="34726-160">Välj en Azure-konto toodeploy till om du inte gjort tidigare.</span><span class="sxs-lookup"><span data-stu-id="34726-160">If you haven't previously, choose an Azure account toodeploy to.</span></span> <span data-ttu-id="34726-161">Om du inte har en ännu, kan du [registrera dig för en][create-account].</span><span class="sxs-lookup"><span data-stu-id="34726-161">If you don't have one yet, [sign-up for one][create-account].</span></span>

<span data-ttu-id="34726-162">Under **Anslutningens slutpunkt**, Välj hello Service Fabric-kluster toodeploy till.</span><span class="sxs-lookup"><span data-stu-id="34726-162">Under **Connection Endpoint**, select hello Service Fabric cluster toodeploy to.</span></span> <span data-ttu-id="34726-163">Om du inte har något väljer  **&lt;Skapa nytt kluster... &gt;**  som öppnas web webbläsare fönstret toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="34726-163">If you do not have one, select **&lt;Create New Cluster...&gt;** which opens up web browser window toohello Azure portal.</span></span> <span data-ttu-id="34726-164">Mer information finns i [skapar ett kluster i hello portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="34726-164">For more information, see [create a cluster in hello portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal).</span></span> 

<span data-ttu-id="34726-165">När du skapar hello Service Fabric-kluster, se till att tooset hello **anpassade slutpunkter** inställning för**80**.</span><span class="sxs-lookup"><span data-stu-id="34726-165">When you create hello Service Fabric cluster, make sure tooset hello **Custom endpoints** setting too**80**.</span></span>

![Service Fabric-nod-typkonfiguration med anpassad slutpunkt][custom-endpoint]

<span data-ttu-id="34726-167">Skapa ett nytt Service Fabric-kluster tar några tid toocomplete.</span><span class="sxs-lookup"><span data-stu-id="34726-167">Creating a new Service Fabric cluster takes some time toocomplete.</span></span> <span data-ttu-id="34726-168">När det har skapats och gå tillbaka toohello dialogrutan Publicera och välj  **&lt;uppdatera&gt;**.</span><span class="sxs-lookup"><span data-stu-id="34726-168">Once it has been created, go back toohello publish dialog and select **&lt;Refresh&gt;**.</span></span> <span data-ttu-id="34726-169">hello nya klustret visas i hello listrutan; Markera den.</span><span class="sxs-lookup"><span data-stu-id="34726-169">hello new cluster is listed in hello drop-down box; select it.</span></span>

<span data-ttu-id="34726-170">Tryck på **publicera** och vänta tills hello distribution toofinish.</span><span class="sxs-lookup"><span data-stu-id="34726-170">Press **Publish** and wait for hello deployment toofinish.</span></span>

<span data-ttu-id="34726-171">Det kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="34726-171">This may take a few minutes.</span></span> <span data-ttu-id="34726-172">När den är klar, kan det ta några minuter för hello programmet toobe helt tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="34726-172">After it completes, it may take a few more minutes for hello application toobe fully available.</span></span>

## <a name="test-hello-website"></a><span data-ttu-id="34726-173">Testa hello webbplats</span><span class="sxs-lookup"><span data-stu-id="34726-173">Test hello website</span></span>

<span data-ttu-id="34726-174">När tjänsten har publicerats kan du testa den i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="34726-174">After your service has been published, test it in a web browser.</span></span> 

<span data-ttu-id="34726-175">Först öppna hello Azure-portalen och Sök efter Service Fabric-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="34726-175">First, open hello Azure portal and find your Service Fabric service.</span></span>

<span data-ttu-id="34726-176">Kontrollera hello översikt bladet för hello-adress.</span><span class="sxs-lookup"><span data-stu-id="34726-176">Check hello overview blade of hello service address.</span></span> <span data-ttu-id="34726-177">Använd hello domännamn från hello _klienten Anslutningens slutpunkt_ egenskapen.</span><span class="sxs-lookup"><span data-stu-id="34726-177">Use hello domain name from hello _Client connection endpoint_ property.</span></span> <span data-ttu-id="34726-178">Till exempel `http://mysvcfab1.westus2.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="34726-178">For example, `http://mysvcfab1.westus2.cloudapp.azure.com`.</span></span>

![Service fabric översikt bladet på hello Azure-portalen][overview]

<span data-ttu-id="34726-180">Navigera toothis adress där du kan se hello `HELLO WORLD` svar.</span><span class="sxs-lookup"><span data-stu-id="34726-180">Navigate toothis address where you will see hello `HELLO WORLD` response.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="34726-181">Ta bort hello kluster</span><span class="sxs-lookup"><span data-stu-id="34726-181">Delete hello cluster</span></span>

<span data-ttu-id="34726-182">Glöm inte toodelete alla hello-resurser som du har skapat för den här snabbstarten, som du debiteras för dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="34726-182">Do not forget toodelete all of hello resources you've created for this quickstart, as you are charged for those resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34726-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34726-183">Next steps</span></span>
<span data-ttu-id="34726-184">Läs mer om [körbara filer för gäst](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="34726-184">Read more about [guest executables](service-fabric-deploy-existing-app.md).</span></span>

<!-- Image References -->

[new-project]: ./media/quickstart-guest-app/new-project.png
[new-service]: ./media/quickstart-guest-app/template.png
[solution-exp]: ./media/quickstart-guest-app/solution-explorer.png
[publish]: ./media/quickstart-guest-app/publish.png
[overview]: ./media/quickstart-guest-app/overview.png
[custom-endpoint]: ./media/quickstart-guest-app/custom-endpoint.png

[download-sample]: https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/service-fabric-node-website.zip
[create-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F