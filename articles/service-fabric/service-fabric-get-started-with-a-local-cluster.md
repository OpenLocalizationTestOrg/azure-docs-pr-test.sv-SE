---
title: "aaaDeploy och uppgradera lokalt Azure mikrotjänster | Microsoft Docs"
description: "Lär dig hur tooset upp en lokal Service Fabric-kluster, distribuera ett befintligt program tooit och sedan uppgradera programmet."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 60a1f6a5-5478-46c0-80a8-18fe62da17a8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi;mikhegn
ms.openlocfilehash: e5f5adc9edb71433b2a7635e9d661ff92a4b18ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a><span data-ttu-id="60472-103">Komma igång med att distribuera och uppgradera program i det lokala klustret</span><span class="sxs-lookup"><span data-stu-id="60472-103">Get started with deploying and upgrading applications on your local cluster</span></span>
<span data-ttu-id="60472-104">hello Azure Service Fabric SDK innehåller en fullständig lokal utvecklingsmiljö som du kan använda tooquickly komma igång med att distribuera och hantera program på lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="60472-104">hello Azure Service Fabric SDK includes a full local development environment that you can use tooquickly get started with deploying and managing applications on a local cluster.</span></span> <span data-ttu-id="60472-105">I den här artikeln, skapa ett lokalt kluster, distribuera ett befintligt program tooit och uppgradera programmet tooa nya versionen, allt från Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="60472-105">In this article, you create a local cluster, deploy an existing application tooit, and then upgrade that application tooa new version, all from Windows PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="60472-106">I den här artikeln förutsätter vi att du redan har [konfigurerat utvecklingsmiljön](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="60472-106">This article assumes that you already [set up your development environment](service-fabric-get-started.md).</span></span>
> 
> 

## <a name="create-a-local-cluster"></a><span data-ttu-id="60472-107">Skapa ett lokalt kluster</span><span class="sxs-lookup"><span data-stu-id="60472-107">Create a local cluster</span></span>
<span data-ttu-id="60472-108">Ett Service Fabric-kluster representerar en uppsättning maskinvaruresurser som du kan distribuera program till.</span><span class="sxs-lookup"><span data-stu-id="60472-108">A Service Fabric cluster represents a set of hardware resources that you can deploy applications to.</span></span> <span data-ttu-id="60472-109">Vanligtvis består ett kluster av var som helst från fem toomany tusentals datorer.</span><span class="sxs-lookup"><span data-stu-id="60472-109">Typically, a cluster is made up of anywhere from five toomany thousands of machines.</span></span> <span data-ttu-id="60472-110">Hello Service Fabric SDK innehåller emellertid en klusterkonfiguration som kan köras på en enskild dator.</span><span class="sxs-lookup"><span data-stu-id="60472-110">However, hello Service Fabric SDK includes a cluster configuration that can run on a single machine.</span></span>

<span data-ttu-id="60472-111">Det är viktigt toounderstand som hello lokala Service Fabric-kluster inte är en emulator eller simulatorn.</span><span class="sxs-lookup"><span data-stu-id="60472-111">It is important toounderstand that hello Service Fabric local cluster is not an emulator or simulator.</span></span> <span data-ttu-id="60472-112">Det körs hello samma plattformskod som finns på flera datorer kluster.</span><span class="sxs-lookup"><span data-stu-id="60472-112">It runs hello same platform code that is found on multi-machine clusters.</span></span> <span data-ttu-id="60472-113">hello enda skillnaden är att den körs hello plattform processer som vanligtvis fördelade på fem datorer på en dator.</span><span class="sxs-lookup"><span data-stu-id="60472-113">hello only difference is that it runs hello platform processes that are normally spread across five machines on one machine.</span></span>

<span data-ttu-id="60472-114">hello SDK tillhandahåller två sätt tooset in ett lokala kluster: en Windows PowerShell-skript och hello Klusterhanterare för lokala system fack app.</span><span class="sxs-lookup"><span data-stu-id="60472-114">hello SDK provides two ways tooset up a local cluster: a Windows PowerShell script and hello Local Cluster Manager system tray app.</span></span> <span data-ttu-id="60472-115">I den här kursen använder vi hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="60472-115">In this tutorial, we use hello PowerShell script.</span></span>

> [!NOTE]
> <span data-ttu-id="60472-116">Om du redan har skapat ett lokalt kluster genom att distribuera ett program från Visual Studio kan du hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="60472-116">If you have already created a local cluster by deploying an application from Visual Studio, you can skip this section.</span></span>
> 
> 

1. <span data-ttu-id="60472-117">Starta ett nytt PowerShell-fönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="60472-117">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="60472-118">Kör installationsskriptet för hello kluster från hello SDK-mappen:</span><span class="sxs-lookup"><span data-stu-id="60472-118">Run hello cluster setup script from hello SDK folder:</span></span>
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```
   
    <span data-ttu-id="60472-119">Klusterinstallationen tar en stund.</span><span class="sxs-lookup"><span data-stu-id="60472-119">Cluster setup takes a few moments.</span></span> <span data-ttu-id="60472-120">När installationen är klar bör skärmen visa något som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="60472-120">After setup is finished, you should see output similar to:</span></span>
   
    ![Utdata efter klusterinstallationen][cluster-setup-success]
   
    <span data-ttu-id="60472-122">Du är nu redo tootry distribuera ett program tooyour kluster.</span><span class="sxs-lookup"><span data-stu-id="60472-122">You are now ready tootry deploying an application tooyour cluster.</span></span>

## <a name="deploy-an-application"></a><span data-ttu-id="60472-123">Distribuera ett program</span><span class="sxs-lookup"><span data-stu-id="60472-123">Deploy an application</span></span>
<span data-ttu-id="60472-124">hello Service Fabric SDK innehåller en omfattande uppsättning ramverk och utvecklare tooling för att skapa program.</span><span class="sxs-lookup"><span data-stu-id="60472-124">hello Service Fabric SDK includes a rich set of frameworks and developer tooling for creating applications.</span></span> <span data-ttu-id="60472-125">Om du vill veta hur toocreate program i Visual Studio, se [skapa ditt första Service Fabric-program i Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="60472-125">If you are interested in learning how toocreate applications in Visual Studio, see [Create your first Service Fabric application in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span></span>

<span data-ttu-id="60472-126">I kursen får du använder en befintlig exempelprogrammet (kallas WordCount) så att du kan fokusera på hello management aspekter av hello plattform: distribution, övervakning och uppgradering.</span><span class="sxs-lookup"><span data-stu-id="60472-126">In this tutorial, you use an existing sample application (called WordCount) so that you can focus on hello management aspects of hello platform: deployment, monitoring, and upgrade.</span></span>

1. <span data-ttu-id="60472-127">Starta ett nytt PowerShell-fönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="60472-127">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="60472-128">Importera hello Service Fabric SDK PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="60472-128">Import hello Service Fabric SDK PowerShell module.</span></span>
   
    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```
3. <span data-ttu-id="60472-129">Skapa en katalog toostore hello-program som du vill hämta och distribuera, till exempel C:\ServiceFabric.</span><span class="sxs-lookup"><span data-stu-id="60472-129">Create a directory toostore hello application that you download and deploy, such as C:\ServiceFabric.</span></span>
   
    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```
4. <span data-ttu-id="60472-130">[Hämta hello WordCount program](http://aka.ms/servicefabric-wordcountapp) toohello plats som du skapade.</span><span class="sxs-lookup"><span data-stu-id="60472-130">[Download hello WordCount application](http://aka.ms/servicefabric-wordcountapp) toohello location you created.</span></span>  <span data-ttu-id="60472-131">Obs: hello Microsoft Edge-webbläsaren sparar hello-fil med en *.zip* tillägg.</span><span class="sxs-lookup"><span data-stu-id="60472-131">Note: hello Microsoft Edge browser saves hello file with a *.zip* extension.</span></span>  <span data-ttu-id="60472-132">Ändra hello filnamnstillägget för*.sfpkg*.</span><span class="sxs-lookup"><span data-stu-id="60472-132">Change hello file extension too*.sfpkg*.</span></span>
5. <span data-ttu-id="60472-133">Anslut toohello lokala klustret:</span><span class="sxs-lookup"><span data-stu-id="60472-133">Connect toohello local cluster:</span></span>
   
    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```
6. <span data-ttu-id="60472-134">Skapa ett nytt program kommandot på hello SDK-distribution med ett namn och en sökväg toohello programpaket.</span><span class="sxs-lookup"><span data-stu-id="60472-134">Create a new application using hello SDK's deployment command with a name and a path toohello application package.</span></span>
   
    ```powershell  
   Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```
   
    <span data-ttu-id="60472-135">Om allt går bra bör du se hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="60472-135">If all goes well, you should see hello following output:</span></span>
   
    ![Distribuera ett program toohello lokala kluster][deploy-app-to-local-cluster]
7. <span data-ttu-id="60472-137">toosee hello program i åtgärden, starta hello webbläsare och gå för[http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html).</span><span class="sxs-lookup"><span data-stu-id="60472-137">toosee hello application in action, launch hello browser and navigate too[http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html).</span></span> <span data-ttu-id="60472-138">Du bör se:</span><span class="sxs-lookup"><span data-stu-id="60472-138">You should see:</span></span>
   
    ![Distribuerat programgränssnitt][deployed-app-ui]
   
    <span data-ttu-id="60472-140">Hej WordCount program är enkelt.</span><span class="sxs-lookup"><span data-stu-id="60472-140">hello WordCount application is simple.</span></span> <span data-ttu-id="60472-141">Den omfattar klientens JavaScript-kod toogenerate slumpmässiga fem tecken ”ord”, som sedan vidarebefordras toohello program via ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="60472-141">It includes client-side JavaScript code toogenerate random five-character "words", which are then relayed toohello application via ASP.NET Web API.</span></span> <span data-ttu-id="60472-142">En tillståndskänslig service spårar hello antalet ord räknas.</span><span class="sxs-lookup"><span data-stu-id="60472-142">A stateful service tracks hello number of words counted.</span></span> <span data-ttu-id="60472-143">De partitioneras baserat på hello första tecknet i hello word.</span><span class="sxs-lookup"><span data-stu-id="60472-143">They are partitioned based on hello first character of hello word.</span></span> <span data-ttu-id="60472-144">Du kan hitta hello källkoden för hello WordCount-app i hello [klassiska komma igång exempel](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount).</span><span class="sxs-lookup"><span data-stu-id="60472-144">You can find hello source code for hello WordCount app in hello [classic getting started samples](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount).</span></span>
   
    <span data-ttu-id="60472-145">hello-programmet som vi har distribuerats innehåller fyra partitioner.</span><span class="sxs-lookup"><span data-stu-id="60472-145">hello application that we deployed contains four partitions.</span></span> <span data-ttu-id="60472-146">Så ord som inleds med ett via G lagras i hello första partitionen som ord som inleds med H till N lagras i andra hello-partition och så vidare.</span><span class="sxs-lookup"><span data-stu-id="60472-146">So words beginning with A through G are stored in hello first partition, words beginning with H through N are stored in hello second partition, and so on.</span></span>

## <a name="view-application-details-and-status"></a><span data-ttu-id="60472-147">Visa programinformation och programstatus</span><span class="sxs-lookup"><span data-stu-id="60472-147">View application details and status</span></span>
<span data-ttu-id="60472-148">Nu när vi har distribuerat programmet hello ska vi titta på några av hello appinformation i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="60472-148">Now that we have deployed hello application, let's look at some of hello app details in PowerShell.</span></span>

1. <span data-ttu-id="60472-149">Fråga alla distribuerade program på hello klustret:</span><span class="sxs-lookup"><span data-stu-id="60472-149">Query all deployed applications on hello cluster:</span></span>
   
    ```powershell
    Get-ServiceFabricApplication
    ```
   
    <span data-ttu-id="60472-150">Förutsatt att du har bara distribuerat hello WordCount app kan se du något som liknar:</span><span class="sxs-lookup"><span data-stu-id="60472-150">Assuming that you have only deployed hello WordCount app, you see something similar to:</span></span>
   
    ![Fråga alla distribuerade program i PowerShell][ps-getsfapp]
2. <span data-ttu-id="60472-152">Gå toohello nästa nivå genom att fråga hello uppsättning tjänster som ingår i hello WordCount program.</span><span class="sxs-lookup"><span data-stu-id="60472-152">Go toohello next level by querying hello set of services that are included in hello WordCount application.</span></span>
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Visa lista över tjänster för programmet hello i PowerShell][ps-getsfsvc]
   
    <span data-ttu-id="60472-154">hello program består av två tjänster och hello frontwebb hello tillståndskänslig tjänsten som hanterar hello ord.</span><span class="sxs-lookup"><span data-stu-id="60472-154">hello application is made up of two services, hello web front end, and hello stateful service that manages hello words.</span></span>
3. <span data-ttu-id="60472-155">Slutligen vill titta på hello lista över partitioner för WordCountService:</span><span class="sxs-lookup"><span data-stu-id="60472-155">Finally, look at hello list of partitions for WordCountService:</span></span>
   
    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```
   
    ![Visa hello service partitioner i PowerShell][ps-getsfpartitions]
   
    <span data-ttu-id="60472-157">Hej uppsättning kommandon som du använde som alla Service Fabric PowerShell-kommandon, är tillgängliga för ett kluster som du kan ansluta till lokal eller fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="60472-157">hello set of commands that you used, like all Service Fabric PowerShell commands, are available for any cluster that you might connect to, local or remote.</span></span>
   
    <span data-ttu-id="60472-158">För en mer visuell sätt toointeract med hello kluster, du kan använda hello Service Fabric Explorer Webbverktyg genom att navigera för[http://localhost:19080/Explorer](http://localhost:19080/Explorer) i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="60472-158">For a more visual way toointeract with hello cluster, you can use hello web-based Service Fabric Explorer tool by navigating too[http://localhost:19080/Explorer](http://localhost:19080/Explorer) in hello browser.</span></span>
   
    ![Visa programinformation i Service Fabric Explorer][sfx-service-overview]
   
   > [!NOTE]
   > <span data-ttu-id="60472-160">toolearn mer om Service Fabric Explorer finns [visualisera ditt kluster med Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="60472-160">toolearn more about Service Fabric Explorer, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
   > 
   > 

## <a name="upgrade-an-application"></a><span data-ttu-id="60472-161">Uppgradera ett program</span><span class="sxs-lookup"><span data-stu-id="60472-161">Upgrade an application</span></span>
<span data-ttu-id="60472-162">Service Fabric ger ingen avbrottstid uppgraderingar genom övervakning hello hälsotillståndet för programmet hello när den samlar över hello kluster.</span><span class="sxs-lookup"><span data-stu-id="60472-162">Service Fabric provides no-downtime upgrades by monitoring hello health of hello application as it rolls out across hello cluster.</span></span> <span data-ttu-id="60472-163">Utföra en uppgradering av hello WordCount-programmet.</span><span class="sxs-lookup"><span data-stu-id="60472-163">Perform an upgrade of hello WordCount application.</span></span>

<span data-ttu-id="60472-164">hello ny version av hello programmet nu räknar ord som börjar med en vokal.</span><span class="sxs-lookup"><span data-stu-id="60472-164">hello new version of hello application now counts only words that begin with a vowel.</span></span> <span data-ttu-id="60472-165">Eftersom hello uppgraderingen implementerar, visas två ändringar i hello programmets beteende.</span><span class="sxs-lookup"><span data-stu-id="60472-165">As hello upgrade rolls out, we see two changes in hello application's behavior.</span></span> <span data-ttu-id="60472-166">Först bör hello hastighet som hello antalet växer långsammare, eftersom färre ord räknas.</span><span class="sxs-lookup"><span data-stu-id="60472-166">First, hello rate at which hello count grows should slow, since fewer words are being counted.</span></span> <span data-ttu-id="60472-167">Andra, eftersom hello första partitionen har två vokal (A- och E) och alla andra partitioner endast innehålla en varje, antalet slutligen att starta toooutpace hello andra.</span><span class="sxs-lookup"><span data-stu-id="60472-167">Second, since hello first partition has two vowels (A and E) and all other partitions contain only one each, its count should eventually start toooutpace hello others.</span></span>

1. <span data-ttu-id="60472-168">[Hämta hello WordCount version 2](http://aka.ms/servicefabric-wordcountappv2) toohello samma plats där du sparade hello version 1 paket.</span><span class="sxs-lookup"><span data-stu-id="60472-168">[Download hello WordCount version 2 package](http://aka.ms/servicefabric-wordcountappv2) toohello same location where you downloaded hello version 1 package.</span></span>
2. <span data-ttu-id="60472-169">Returnera tooyour PowerShell-fönster och Använd hello SDK kommandot uppgradera tooregister hello den nya versionen i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="60472-169">Return tooyour PowerShell window and use hello SDK's upgrade command tooregister hello new version in hello cluster.</span></span> <span data-ttu-id="60472-170">Sedan börjar uppgradera hello fabric: / WordCount-programmet.</span><span class="sxs-lookup"><span data-stu-id="60472-170">Then begin upgrading hello fabric:/WordCount application.</span></span>
   
    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```
   
    <span data-ttu-id="60472-171">Du bör se hello följande utdata i PowerShell som hello uppgraderingen börjar.</span><span class="sxs-lookup"><span data-stu-id="60472-171">You should see hello following output in PowerShell as hello upgrade begins.</span></span>
   
    ![Uppgraderingsförlopp i PowerShell][ps-appupgradeprogress]
3. <span data-ttu-id="60472-173">Medan du fortsätter hello uppgraderingen kan du enklare toomonitor dess status från Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="60472-173">While hello upgrade is proceeding, you may find it easier toomonitor its status from Service Fabric Explorer.</span></span> <span data-ttu-id="60472-174">Starta ett webbläsarfönster och navigera för[http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="60472-174">Launch a browser window and navigate too[http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="60472-175">Expandera **program** i trädet för hello hello vänster Välj **WordCount**, och slutligen **fabric: / WordCount**.</span><span class="sxs-lookup"><span data-stu-id="60472-175">Expand **Applications** in hello tree on hello left, then choose **WordCount**, and finally **fabric:/WordCount**.</span></span> <span data-ttu-id="60472-176">Hello essentials på fliken finns hello status hello uppgradering när det fortsätter via hello klustret uppgraderingsdomäner.</span><span class="sxs-lookup"><span data-stu-id="60472-176">In hello essentials tab, you see hello status of hello upgrade as it proceeds through hello cluster's upgrade domains.</span></span>
   
    ![Uppgraderingsförlopp i Service Fabric Explorer][sfx-upgradeprogress]
   
    <span data-ttu-id="60472-178">Medan hello uppgraderingen utförs via varje domän, är hälsokontroller utförs tooensure som hello programmet fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="60472-178">As hello upgrade proceeds through each domain, health checks are performed tooensure that hello application is behaving properly.</span></span>
4. <span data-ttu-id="60472-179">Om du kör hello tidigare fråga efter hello uppsättning tjänster i hello fabric: / WordCount-programmet, Lägg märke till att hello WordCountService version har ändrats men hello WordCountWebService version inte:</span><span class="sxs-lookup"><span data-stu-id="60472-179">If you rerun hello earlier query for hello set of services in hello fabric:/WordCount application, notice that hello WordCountService version changed but hello WordCountWebService version did not:</span></span>
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Fråga programtjänster efter uppgraderingen][ps-getsfsvc-postupgrade]
   
    <span data-ttu-id="60472-181">I det här exemplet ser du hur Service Fabric hanterar programuppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="60472-181">This example highlights how Service Fabric manages application upgrades.</span></span> <span data-ttu-id="60472-182">Den når endast hello uppsättning tjänster (eller konfigurations och kod paket i dessa tjänster) som har ändrats, vilket gör hello processen med att uppgradera snabbare och mer tillförlitlig.</span><span class="sxs-lookup"><span data-stu-id="60472-182">It touches only hello set of services (or code/configuration packages within those services) that have changed, which makes hello process of upgrading faster and more reliable.</span></span>
5. <span data-ttu-id="60472-183">Slutligen returneras toohello webbläsare tooobserve hello beteendet för hello ny programversion.</span><span class="sxs-lookup"><span data-stu-id="60472-183">Finally, return toohello browser tooobserve hello behavior of hello new application version.</span></span> <span data-ttu-id="60472-184">Som förväntat, hello antal fortlöper långsammare och hello första partitionen slutar med lite mer hello volym.</span><span class="sxs-lookup"><span data-stu-id="60472-184">As expected, hello count progresses more slowly, and hello first partition ends up with slightly more of hello volume.</span></span>
   
    ![Visa hello ny version av programmet hello i hello webbläsare][deployed-app-ui-v2]

## <a name="cleaning-up"></a><span data-ttu-id="60472-186">Rensa</span><span class="sxs-lookup"><span data-stu-id="60472-186">Cleaning up</span></span>
<span data-ttu-id="60472-187">Det är viktigt tooremember som hello lokala klustret är verkliga innan du omsluter.</span><span class="sxs-lookup"><span data-stu-id="60472-187">Before wrapping up, it's important tooremember that hello local cluster is real.</span></span> <span data-ttu-id="60472-188">Programmen toorun i bakgrunden hello tills du tar bort dem.</span><span class="sxs-lookup"><span data-stu-id="60472-188">Applications continue toorun in hello background until you remove them.</span></span>  <span data-ttu-id="60472-189">Beroende på hello uppbyggnad dina appar, kan en app som körs ta upp viktiga resurser på din dator.</span><span class="sxs-lookup"><span data-stu-id="60472-189">Depending on hello nature of your apps, a running app can take up significant resources on your machine.</span></span> <span data-ttu-id="60472-190">Du har flera alternativ toomanage program och hello kluster:</span><span class="sxs-lookup"><span data-stu-id="60472-190">You have several options toomanage applications and hello cluster:</span></span>

1. <span data-ttu-id="60472-191">tooremove ett enskilt program och alla den datorns informationen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="60472-191">tooremove an individual application and all it's data, run hello following command:</span></span>
   
    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```
   
    <span data-ttu-id="60472-192">Ta bort programmet hello från hello Service Fabric Explorer **åtgärder** -menyn eller hello snabbmenyn i listvyn för hello vänstra program.</span><span class="sxs-lookup"><span data-stu-id="60472-192">Or, delete hello application from hello Service Fabric Explorer **ACTIONS** menu or hello context menu in hello left-hand application list view.</span></span>
   
    ![Ta bort ett program i Service Fabric Explorer][sfe-delete-application]
2. <span data-ttu-id="60472-194">Avregistrera version 1.0.0 och 2.0.0 av hello WordCount programtyp när du tar bort programmet hello från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="60472-194">After deleting hello application from hello cluster, unregister versions 1.0.0 and 2.0.0 of hello WordCount application type.</span></span> <span data-ttu-id="60472-195">Ta bort tar bort hello programpaket, inklusive hello koden och konfigurationen från avbildningsarkivet hello klustret.</span><span class="sxs-lookup"><span data-stu-id="60472-195">Deletion removes hello application packages, including hello code and configuration, from hello cluster's image store.</span></span>
   
    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```
   
    <span data-ttu-id="60472-196">Eller välj i Service Fabric Explorer **avetablera typen** för hello program.</span><span class="sxs-lookup"><span data-stu-id="60472-196">Or, in Service Fabric Explorer, choose **Unprovision Type** for hello application.</span></span>
3. <span data-ttu-id="60472-197">tooshut ned hello klustret men behåll hello programdata och spårning, klickar du på **stoppa lokala klustret** i hello system fack app.</span><span class="sxs-lookup"><span data-stu-id="60472-197">tooshut down hello cluster but keep hello application data and traces, click **Stop Local Cluster** in hello system tray app.</span></span>
4. <span data-ttu-id="60472-198">toodelete hello klustret helt, klickar du på **ta bort lokala klustret** i hello system fack app.</span><span class="sxs-lookup"><span data-stu-id="60472-198">toodelete hello cluster entirely, click **Remove Local Cluster** in hello system tray app.</span></span> <span data-ttu-id="60472-199">Det här alternativet resulterar i en annan långsam distribution hello nästa gång du trycka på F5 i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="60472-199">This option will result in another slow deployment hello next time you press F5 in Visual Studio.</span></span> <span data-ttu-id="60472-200">Ta bort hello lokala klustret bara om du inte tänker toouse den under en viss tid eller om du behöver tooreclaim resurser.</span><span class="sxs-lookup"><span data-stu-id="60472-200">Remove hello local cluster only if you don't intend toouse it for some time or if you need tooreclaim resources.</span></span>

## <a name="one-node-and-five-node-cluster-mode"></a><span data-ttu-id="60472-201">Läge för kluster med en nod och fem noder</span><span class="sxs-lookup"><span data-stu-id="60472-201">One-node and five-node cluster mode</span></span>
<span data-ttu-id="60472-202">När du utvecklar program gör du ofta snabba iterationer när du skriver, felsöker och ändrar kod.</span><span class="sxs-lookup"><span data-stu-id="60472-202">When developing applications, you often find yourself doing quick iterations of writing code, debugging, changing code, and debugging.</span></span> <span data-ttu-id="60472-203">toohelp optimera den här processen, hello lokala klustret kan köras i två lägen: en nod eller fem noder.</span><span class="sxs-lookup"><span data-stu-id="60472-203">toohelp optimize this process, hello local cluster can run in two modes: one-node or five-node.</span></span> <span data-ttu-id="60472-204">Båda klusterlägena har sina fördelar.</span><span class="sxs-lookup"><span data-stu-id="60472-204">Both cluster modes have their benefits.</span></span> <span data-ttu-id="60472-205">Läget för kluster med fem noder kan du toowork med en verklig kluster.</span><span class="sxs-lookup"><span data-stu-id="60472-205">Five-node cluster mode enables you toowork with a real cluster.</span></span> <span data-ttu-id="60472-206">Du kan testa redundansscenarier, samt arbeta med flera instanser och repliker av dina tjänster.</span><span class="sxs-lookup"><span data-stu-id="60472-206">You can test failover scenarios, work with more instances and replicas of your services.</span></span> <span data-ttu-id="60472-207">Kluster med en nod läge är optimerad toodo snabb distribution och registrering av tjänster, toohelp du snabbt Validera kod med hello Service Fabric runtime.</span><span class="sxs-lookup"><span data-stu-id="60472-207">One-node cluster mode is optimized toodo quick deployment and registration of services, toohelp you quickly validate code using hello Service Fabric runtime.</span></span>

<span data-ttu-id="60472-208">Varken läget för kluster med en nod eller fem noder är en emulator eller simulator.</span><span class="sxs-lookup"><span data-stu-id="60472-208">Neither one-node cluster or five-node cluster modes are an emulator or simulator.</span></span> <span data-ttu-id="60472-209">hello lokal utveckling klustret körs hello samma plattformskod som finns på flera datorer kluster.</span><span class="sxs-lookup"><span data-stu-id="60472-209">hello local development cluster runs hello same platform code that is found on multi-machine clusters.</span></span>

> [!WARNING]
> <span data-ttu-id="60472-210">När du ändrar hello klustret läge hello aktuella klustret tas bort från systemet och ett nytt kluster skapas.</span><span class="sxs-lookup"><span data-stu-id="60472-210">When you change hello cluster mode, hello current cluster is removed from your system and a new cluster is created.</span></span> <span data-ttu-id="60472-211">hello-data som lagras i hello klustret tas bort när du ändrar läget för klustret.</span><span class="sxs-lookup"><span data-stu-id="60472-211">hello data stored in hello cluster is deleted when you change cluster mode.</span></span>
> 
> 

<span data-ttu-id="60472-212">toochange hello läge tooone kluster, Välj **växla klustret läge** i hello Klusterhanterare för Service Fabric lokalt.</span><span class="sxs-lookup"><span data-stu-id="60472-212">toochange hello mode tooone-node cluster, select **Switch Cluster Mode** in hello Service Fabric Local Cluster Manager.</span></span>

![Växla klusterläge][switch-cluster-mode]

<span data-ttu-id="60472-214">Eller ändra hello klustret läge med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="60472-214">Or, change hello cluster mode using PowerShell:</span></span>

1. <span data-ttu-id="60472-215">Starta ett nytt PowerShell-fönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="60472-215">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="60472-216">Kör installationsskriptet för hello kluster från hello SDK-mappen:</span><span class="sxs-lookup"><span data-stu-id="60472-216">Run hello cluster setup script from hello SDK folder:</span></span>
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```
   
    <span data-ttu-id="60472-217">Klusterinstallationen tar en stund.</span><span class="sxs-lookup"><span data-stu-id="60472-217">Cluster setup takes a few moments.</span></span> <span data-ttu-id="60472-218">När installationen är klar bör skärmen visa något som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="60472-218">After setup is finished, you should see output similar to:</span></span>
   
    ![Utdata efter klusterinstallationen][cluster-setup-success-1-node]

## <a name="next-steps"></a><span data-ttu-id="60472-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="60472-220">Next steps</span></span>
* <span data-ttu-id="60472-221">Nu när du har distribuerat och uppgraderat vissa fördefinierade program kan du [skapa ett eget program i Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="60472-221">Now that you have deployed and upgraded some pre-built applications, you can [try building your own in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span></span>
* <span data-ttu-id="60472-222">Alla hello-åtgärder som utförs på hello lokala klustret i den här artikeln kan utföras på en [Azure klustret](service-fabric-cluster-creation-via-portal.md) samt.</span><span class="sxs-lookup"><span data-stu-id="60472-222">All hello actions performed on hello local cluster in this article can be performed on an [Azure cluster](service-fabric-cluster-creation-via-portal.md) as well.</span></span>
* <span data-ttu-id="60472-223">Det gick grundläggande hello uppgradering som vi utförs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="60472-223">hello upgrade that we performed in this article was basic.</span></span> <span data-ttu-id="60472-224">Se hello [uppgraderingsinformationen](service-fabric-application-upgrade.md) toolearn mer om hello kraften och flexibiliteten hos Service Fabric-uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="60472-224">See hello [upgrade documentation](service-fabric-application-upgrade.md) toolearn more about hello power and flexibility of Service Fabric upgrades.</span></span>

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
