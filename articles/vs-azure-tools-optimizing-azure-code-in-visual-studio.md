---
title: aaaOptimizing din Azure kod i Visual Studio | Microsoft Docs
description: "Lär dig mer om hur Azure kod optimering verktyg i Visual Studio gör din kod mer stabilt och bättre prestanda."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ed48ee06-e2d2-4322-af22-07200fb16987
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 7df932def9dc16c93de29fc6a77c8fc121fda338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimizing-your-azure-code"></a><span data-ttu-id="8902c-103">Optimera din Azure kod</span><span class="sxs-lookup"><span data-stu-id="8902c-103">Optimizing Your Azure Code</span></span>
<span data-ttu-id="8902c-104">När du programmering appar som använder Microsoft Azure, det finns vissa kodningsexempel som du bör följa toohelp undvika problem med appen skalbarhet, funktioner och prestanda i en molnmiljö.</span><span class="sxs-lookup"><span data-stu-id="8902c-104">When you’re programming apps that use Microsoft Azure, there are some coding practices you should follow toohelp avoid problems with app scalability, behavior and performance in a cloud environment.</span></span> <span data-ttu-id="8902c-105">Microsoft tillhandahåller ett analysverktyg i Azure kod som identifierar och identifierar de dessa ofta påträffade problem och hjälper dig att lösa dem.</span><span class="sxs-lookup"><span data-stu-id="8902c-105">Microsoft provides an Azure Code Analysis tool that recognizes and identifies several of these commonly-encountered issues and helps you resolve them.</span></span> <span data-ttu-id="8902c-106">Du kan hämta hello verktyg i Visual Studio via NuGet.</span><span class="sxs-lookup"><span data-stu-id="8902c-106">You can download hello tool in Visual Studio via NuGet.</span></span>

## <a name="azure-code-analysis-rules"></a><span data-ttu-id="8902c-107">Azure kod Analysis-regler</span><span class="sxs-lookup"><span data-stu-id="8902c-107">Azure Code Analysis rules</span></span>
<span data-ttu-id="8902c-108">hello Azure kod analysverktyget använder hello enligt reglerna för tooautomatically flaggan Azure koden när den söker efter kända problem som påverkar prestanda.</span><span class="sxs-lookup"><span data-stu-id="8902c-108">hello Azure Code Analysis tool uses hello following rules tooautomatically flag your Azure code when it finds known performance-impacting issues.</span></span> <span data-ttu-id="8902c-109">Identifierat problem visas som en varningar eller fel.</span><span class="sxs-lookup"><span data-stu-id="8902c-109">Detected issues appear as a warnings or compiler errors.</span></span> <span data-ttu-id="8902c-110">Koden korrigeringar eller förslag tooresolve hello varnings- eller ofta sker via en lampikonen.</span><span class="sxs-lookup"><span data-stu-id="8902c-110">Code fixes or suggestions tooresolve hello warning or error are often provided through a light bulb icon.</span></span>

## <a name="avoid-using-default-in-process-session-state-mode"></a><span data-ttu-id="8902c-111">Undvik att använda sessionstillståndsläge för standard (pågående)</span><span class="sxs-lookup"><span data-stu-id="8902c-111">Avoid using default (in-process) session state mode</span></span>
### <a name="id"></a><span data-ttu-id="8902c-112">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-112">ID</span></span>
<span data-ttu-id="8902c-113">AP0000</span><span class="sxs-lookup"><span data-stu-id="8902c-113">AP0000</span></span>

### <a name="description"></a><span data-ttu-id="8902c-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-114">Description</span></span>
<span data-ttu-id="8902c-115">Om du använder hello standard (pågående) sessionstillståndsläge för molnprogram, förlorar du sessionstillstånd.</span><span class="sxs-lookup"><span data-stu-id="8902c-115">If you use hello default (in-process) session state mode for cloud applications, you can lose session state.</span></span>

<span data-ttu-id="8902c-116">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-116">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-117">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-117">Reason</span></span>
<span data-ttu-id="8902c-118">Som standard är hello sessionstillståndsläge som angetts i web.config-filen för hello i processen.</span><span class="sxs-lookup"><span data-stu-id="8902c-118">By default, hello session state mode specified in hello web.config file is in-process.</span></span> <span data-ttu-id="8902c-119">Även om ingen post som angetts i konfigurationsfilen för hello hello sessionstillstånd läge som standard tooin-processen.</span><span class="sxs-lookup"><span data-stu-id="8902c-119">Also, if no entry specified in hello configuration file, hello Session State mode defaults tooin-process.</span></span> <span data-ttu-id="8902c-120">hello-läge i processen lagrar sessionstillstånd i minnet på hello webbserver.</span><span class="sxs-lookup"><span data-stu-id="8902c-120">hello in-process mode stores session state in memory on hello web server.</span></span> <span data-ttu-id="8902c-121">När en instans har startats om eller en ny instans används för belastningsutjämning eller stöd för redundans, sparas inte hello sessionstillstånd lagras i minnet på hello webbserver.</span><span class="sxs-lookup"><span data-stu-id="8902c-121">When an instance is restarted or a new instance is used for load balancing or failover support, hello session state stored in memory on hello web server isn’t saved.</span></span> <span data-ttu-id="8902c-122">Detta förhindrar att hello program som skalbara på hello moln.</span><span class="sxs-lookup"><span data-stu-id="8902c-122">This situation prevents hello application from being scalable on hello cloud.</span></span>

<span data-ttu-id="8902c-123">Sessionstillståndet ASP.NET stöder flera olika lagringsalternativ för sessionstillståndsdata: InProc, StateServer, SQLServer, anpassad, och ut.</span><span class="sxs-lookup"><span data-stu-id="8902c-123">ASP.NET session state supports several different storage options for session state data: InProc, StateServer, SQLServer, Custom, and Off.</span></span> <span data-ttu-id="8902c-124">Det rekommenderas att du använder anpassade läge toohost data på en extern butik sessionstillstånd, t.ex [Azure sessionstillståndsprovider för Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span><span class="sxs-lookup"><span data-stu-id="8902c-124">It’s recommended that you use Custom mode toohost data on an external Session State store, such as [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span></span>

### <a name="solution"></a><span data-ttu-id="8902c-125">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-125">Solution</span></span>
<span data-ttu-id="8902c-126">En rekommenderad lösning är toostore sessionens tillstånd på en hanterad cache-tjänst.</span><span class="sxs-lookup"><span data-stu-id="8902c-126">One recommended solution is toostore session state on a managed cache service.</span></span> <span data-ttu-id="8902c-127">Lär dig hur toouse [Azure sessionstillståndsprovider för Redis](http://go.microsoft.com/fwlink/?LinkId=401521) toostore sessionens tillstånd.</span><span class="sxs-lookup"><span data-stu-id="8902c-127">Learn how toouse [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521) toostore your session state.</span></span> <span data-ttu-id="8902c-128">Du kan också store-sessionen tillstånd i andra platser tooensure ditt program är skalbara på hello moln.</span><span class="sxs-lookup"><span data-stu-id="8902c-128">You can also store session state in other places tooensure your application is scalable on hello cloud.</span></span> <span data-ttu-id="8902c-129">Mer information om alternativa lösningar Läs toolearn [Session tillstånd lägen](https://msdn.microsoft.com/library/ms178586).</span><span class="sxs-lookup"><span data-stu-id="8902c-129">toolearn more about alternative solutions please read [Session State Modes](https://msdn.microsoft.com/library/ms178586).</span></span>

## <a name="run-method-should-not-be-async"></a><span data-ttu-id="8902c-130">Kör metoden får inte vara asynkrona</span><span class="sxs-lookup"><span data-stu-id="8902c-130">Run method should not be async</span></span>
### <a name="id"></a><span data-ttu-id="8902c-131">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-131">ID</span></span>
<span data-ttu-id="8902c-132">AP1000</span><span class="sxs-lookup"><span data-stu-id="8902c-132">AP1000</span></span>

### <a name="description"></a><span data-ttu-id="8902c-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-133">Description</span></span>
<span data-ttu-id="8902c-134">Skapa asynkrona metoder (som [await](https://msdn.microsoft.com/library/hh156528.aspx)) utanför hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden och sedan anropa hello asynkrona metoder från [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-134">Create asynchronous methods (such as [await](https://msdn.microsoft.com/library/hh156528.aspx)) outside of hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and then call hello async methods from [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span></span> <span data-ttu-id="8902c-135">Deklarerar hello [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden som asynkron gör hello worker rollen tooenter en omstart loop.</span><span class="sxs-lookup"><span data-stu-id="8902c-135">Declaring hello [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method as async causes hello worker role tooenter a restart loop.</span></span>

<span data-ttu-id="8902c-136">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-136">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-137">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-137">Reason</span></span>
<span data-ttu-id="8902c-138">Anropar asynkrona metoder i hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden gör hello cloud service runtime toorecycle hello worker-rollen.</span><span class="sxs-lookup"><span data-stu-id="8902c-138">Calling async methods inside hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes hello cloud service runtime toorecycle hello worker role.</span></span> <span data-ttu-id="8902c-139">När en arbetsroll startar alla programkörningen äger rum inom hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="8902c-139">When a worker role starts, all program execution takes place inside hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="8902c-140">Befintliga hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden gör hello worker rollen toorestart.</span><span class="sxs-lookup"><span data-stu-id="8902c-140">Exiting hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes hello worker role toorestart.</span></span> <span data-ttu-id="8902c-141">När hello worker-rollen runtime träffar hello async-metod, skickar alla åtgärder efter hello async-metod och returnerar sedan.</span><span class="sxs-lookup"><span data-stu-id="8902c-141">When hello worker role runtime hits hello async method, it dispatches all operations after hello async method and then returns.</span></span> <span data-ttu-id="8902c-142">Detta medför hello worker rollen tooexit från hello [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod och starta om.</span><span class="sxs-lookup"><span data-stu-id="8902c-142">This causes hello worker role tooexit from hello [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and restart.</span></span> <span data-ttu-id="8902c-143">I hello nästa iteration av körningen träffar hello arbetsrollen hello asynkrona metoden igen och omstarter, orsakar hello worker rollen toorecycle samt igen.</span><span class="sxs-lookup"><span data-stu-id="8902c-143">In hello next iteration of execution, hello worker role hits hello async method again and restarts, causing hello worker role toorecycle again as well.</span></span>

### <a name="solution"></a><span data-ttu-id="8902c-144">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-144">Solution</span></span>
<span data-ttu-id="8902c-145">Placera alla asynkrona åtgärder utanför hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="8902c-145">Place all async operations outside of hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="8902c-146">Anropa sedan hello omstrukturerade async-metod från inuti hello [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod, till exempel RunAsync () .wait.</span><span class="sxs-lookup"><span data-stu-id="8902c-146">Then, call hello refactored async method from inside hello [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method, such as RunAsync().wait.</span></span> <span data-ttu-id="8902c-147">hello Azure kod analysverktyget hjälper dig att åtgärda problemet.</span><span class="sxs-lookup"><span data-stu-id="8902c-147">hello Azure Code Analysis tool can help you fix this issue.</span></span>

<span data-ttu-id="8902c-148">hello följande kodfragment som visar hello kod korrigering för problemet:</span><span class="sxs-lookup"><span data-stu-id="8902c-148">hello following code snippet demonstrates hello code fix for this issue:</span></span>

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a><span data-ttu-id="8902c-149">Använda Service Bus signatur för delad åtkomst-autentisering</span><span class="sxs-lookup"><span data-stu-id="8902c-149">Use Service Bus Shared Access Signature authentication</span></span>
### <a name="id"></a><span data-ttu-id="8902c-150">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-150">ID</span></span>
<span data-ttu-id="8902c-151">AP2000</span><span class="sxs-lookup"><span data-stu-id="8902c-151">AP2000</span></span>

### <a name="description"></a><span data-ttu-id="8902c-152">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-152">Description</span></span>
<span data-ttu-id="8902c-153">Använda delade signatur åtkomst (SAS) för autentisering.</span><span class="sxs-lookup"><span data-stu-id="8902c-153">Use Shared Access Signature (SAS) for authentication.</span></span> <span data-ttu-id="8902c-154">Access Control Service (ACS) är inaktuell för service bus-autentisering.</span><span class="sxs-lookup"><span data-stu-id="8902c-154">Access Control Service (ACS) is being deprecated for service bus authentication.</span></span>

<span data-ttu-id="8902c-155">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-155">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-156">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-156">Reason</span></span>
<span data-ttu-id="8902c-157">För förbättrad säkerhet ersätter Azure Active Directory ACS-autentisering med SAS-autentisering.</span><span class="sxs-lookup"><span data-stu-id="8902c-157">For enhanced security, Azure Active Directory is replacing ACS authentication with SAS authentication.</span></span> <span data-ttu-id="8902c-158">Se [Azure Active Directory är hello framtida av ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) information om hello övergången plan.</span><span class="sxs-lookup"><span data-stu-id="8902c-158">See [Azure Active Directory is hello future of ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) for information on hello transition plan.</span></span>

### <a name="solution"></a><span data-ttu-id="8902c-159">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-159">Solution</span></span>
<span data-ttu-id="8902c-160">Använd SAS-autentisering i dina appar.</span><span class="sxs-lookup"><span data-stu-id="8902c-160">Use SAS authentication in your apps.</span></span> <span data-ttu-id="8902c-161">hello som följande exempel visar hur toouse en befintlig SAS-token tooaccess en service bus namnområde eller enhet.</span><span class="sxs-lookup"><span data-stu-id="8902c-161">hello following example shows how toouse an existing SAS token tooaccess a service bus namespace or entity.</span></span>

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

<span data-ttu-id="8902c-162">Se hello följande avsnitt för mer information.</span><span class="sxs-lookup"><span data-stu-id="8902c-162">See hello following topics for more information.</span></span>

* <span data-ttu-id="8902c-163">En översikt finns [signatur autentisering för delad åtkomst med Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span><span class="sxs-lookup"><span data-stu-id="8902c-163">For an overview, see [Shared Access Signature Authentication with Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span></span>
* [<span data-ttu-id="8902c-164">Hur toouse autentisering med signatur för delad åtkomst med Service Bus</span><span class="sxs-lookup"><span data-stu-id="8902c-164">How toouse Shared Access Signature Authentication with Service Bus</span></span>](https://msdn.microsoft.com/library/dn205161.aspx)
* <span data-ttu-id="8902c-165">En exempelprojektet finns [autentisering med delad signatur åtkomst (SAS) med Service Bus-prenumerationer](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span><span class="sxs-lookup"><span data-stu-id="8902c-165">For a sample project, see [Using Shared Access Signature (SAS) authentication with Service Bus Subscriptions](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span></span>

## <a name="consider-using-onmessage-method-tooavoid-receive-loop"></a><span data-ttu-id="8902c-166">Överväg att använda OnMessage metoden tooavoid ”meddelandemottagning”</span><span class="sxs-lookup"><span data-stu-id="8902c-166">Consider using OnMessage method tooavoid "receive loop"</span></span>
### <a name="id"></a><span data-ttu-id="8902c-167">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-167">ID</span></span>
<span data-ttu-id="8902c-168">AP2002</span><span class="sxs-lookup"><span data-stu-id="8902c-168">AP2002</span></span>

### <a name="description"></a><span data-ttu-id="8902c-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-169">Description</span></span>
<span data-ttu-id="8902c-170">tooavoid gå in på en ”meddelandemottagning”, anropa hello **OnMessage** metoden är en bättre lösning för att ta emot meddelanden än anropa hello **Receive** metod.</span><span class="sxs-lookup"><span data-stu-id="8902c-170">tooavoid going into a "receive loop," calling hello **OnMessage** method is a better solution for receiving messages than calling hello **Receive** method.</span></span> <span data-ttu-id="8902c-171">Men om du måste använda hello **Receive** metod och ange väntetiden för en icke-standard-servern, kontrollera hello server väntetiden är mer än en minut.</span><span class="sxs-lookup"><span data-stu-id="8902c-171">However, if you must use hello **Receive** method, and you specify a non-default server wait time, make sure hello server wait time is more than one minute.</span></span>

<span data-ttu-id="8902c-172">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-172">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-173">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-173">Reason</span></span>
<span data-ttu-id="8902c-174">När du anropar **OnMessage**, hello klienten startar ett internt meddelande pump som ständigt avsöker hello kö eller prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8902c-174">When calling **OnMessage**, hello client starts an internal message pump that constantly polls hello queue or subscription.</span></span> <span data-ttu-id="8902c-175">Det här meddelandet pump innehåller en oändlig loop som utfärdar ett anrop tooreceive meddelanden.</span><span class="sxs-lookup"><span data-stu-id="8902c-175">This message pump contains an infinite loop that issues a call tooreceive messages.</span></span> <span data-ttu-id="8902c-176">Om hello anropet uppnår tidsgränsen, utfärdas ett nytt anrop.</span><span class="sxs-lookup"><span data-stu-id="8902c-176">If hello call times out, it issues a new call.</span></span> <span data-ttu-id="8902c-177">hello timeoutintervall bestäms av hello värdet för hello [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) -egenskapen för hello [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)som används.</span><span class="sxs-lookup"><span data-stu-id="8902c-177">hello timeout interval is determined by hello value of hello [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) property of hello [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)that’s being used.</span></span>

<span data-ttu-id="8902c-178">Hej fördelen med att använda **OnMessage** jämfört med för**Receive** är att användare inte har toomanually söka efter meddelanden, hantera undantag, bearbeta flera meddelanden parallellt och slutföra hello meddelanden.</span><span class="sxs-lookup"><span data-stu-id="8902c-178">hello advantage of using **OnMessage** compared too**Receive** is that users don’t have toomanually poll for messages, handle exceptions, process multiple messages in parallel, and complete hello messages.</span></span>

<span data-ttu-id="8902c-179">Om du anropar **Receive** utan att använda standardvärdet vara säker på att hello *ServerWaitTime* värdet är mer än en minut.</span><span class="sxs-lookup"><span data-stu-id="8902c-179">If you call **Receive** without using its default value, be sure hello *ServerWaitTime* value is more than one minute.</span></span> <span data-ttu-id="8902c-180">Ange *ServerWaitTime* toomore än en minut timeout innan hello helt meddelande förhindrar hello-servern.</span><span class="sxs-lookup"><span data-stu-id="8902c-180">Setting *ServerWaitTime* toomore than one minute prevents hello server from timing out before hello message is fully received.</span></span>

### <a name="solution"></a><span data-ttu-id="8902c-181">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-181">Solution</span></span>
<span data-ttu-id="8902c-182">Se följande kodexempel för rekommenderade användningsområden hello.</span><span class="sxs-lookup"><span data-stu-id="8902c-182">Please see hello following code examples for recommended usages.</span></span> <span data-ttu-id="8902c-183">Mer information finns i [QueueClient.OnMessage-metoden (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)och [QueueClient.Receive-metoden (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-183">For more details, see [QueueClient.OnMessage Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)and [QueueClient.Receive Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span></span>

<span data-ttu-id="8902c-184">tooimprove hello prestanda för hello Azure meddelandeinfrastruktur finns hello designmönstret [asynkrona meddelanden introduktion](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-184">tooimprove hello performance of hello Azure messaging infrastructure, see hello design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

<span data-ttu-id="8902c-185">hello följande är ett exempel på hur du använder **OnMessage** tooreceive meddelanden.</span><span class="sxs-lookup"><span data-stu-id="8902c-185">hello following is an example of using **OnMessage** tooreceive messages.</span></span>

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if hello message-pump should call complete on messages after hello callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates hello maximum number of concurrent calls toohello callback hello pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you tooget notified of any errors encountered by hello message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates hello message pump and callback is invoked for each message that is recieved, calling close on hello client will stop hello pump.
    {
        // Process hello message.
    }, options);
    Console.WriteLine("Press any key tooexit.");
    Console.ReadKey();
```

<span data-ttu-id="8902c-186">hello följande är ett exempel på hur du använder **ta emot** med hello standardserver väntetid.</span><span class="sxs-lookup"><span data-stu-id="8902c-186">hello following is an example of using **Receive** with hello default server wait time.</span></span>

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

<span data-ttu-id="8902c-187">hello följande är ett exempel på hur du använder **Receive** väntetid med en server som inte är standard.</span><span class="sxs-lookup"><span data-stu-id="8902c-187">hello following is an example of using **Receive** with a non-default server wait time.</span></span>

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a><span data-ttu-id="8902c-188">Överväg att använda asynkrona Service Bus-metoder</span><span class="sxs-lookup"><span data-stu-id="8902c-188">Consider using asynchronous Service Bus methods</span></span>
### <a name="id"></a><span data-ttu-id="8902c-189">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-189">ID</span></span>
<span data-ttu-id="8902c-190">AP2003</span><span class="sxs-lookup"><span data-stu-id="8902c-190">AP2003</span></span>

### <a name="description"></a><span data-ttu-id="8902c-191">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-191">Description</span></span>
<span data-ttu-id="8902c-192">Använda asynkrona Service Bus metoder tooimprove prestanda med asynkrona meddelanden.</span><span class="sxs-lookup"><span data-stu-id="8902c-192">Use asynchronous Service Bus methods tooimprove performance with brokered messaging.</span></span>

<span data-ttu-id="8902c-193">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-193">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-194">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-194">Reason</span></span>
<span data-ttu-id="8902c-195">När du använder asynkrona metoder kan programmet programmet samtidighet eftersom körs varje anrop inte blockerar hello huvudtråden.</span><span class="sxs-lookup"><span data-stu-id="8902c-195">Using asynchronous methods enables application program concurrency because executing each call doesn’t block hello main thread.</span></span> <span data-ttu-id="8902c-196">När du använder Service Bus messaging metoder, utför en åtgärd (skicka, ta emot, ta bort, etc.) tar tid.</span><span class="sxs-lookup"><span data-stu-id="8902c-196">When using Service Bus messaging methods, performing an operation (send, receive, delete, etc.) takes time.</span></span> <span data-ttu-id="8902c-197">Nu innehåller hello bearbetningen av hello åtgärden av hello Service Bus-tjänst i tillägg toohello svarstiden för hello och hello svarsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="8902c-197">This time includes hello processing of hello operation by hello Service Bus service in addition toohello latency of hello request and hello reply.</span></span> <span data-ttu-id="8902c-198">tooincrease hello antalet åtgärder per tid, åtgärder måste köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="8902c-198">tooincrease hello number of operations per time, operations must execute concurrently.</span></span> <span data-ttu-id="8902c-199">Mer information finns för[bästa praxis för prestanda förbättringar med Service Bus asynkrona meddelanden](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-199">For more information please refer too[Best Practices for Performance Improvements Using Service Bus Brokered Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="8902c-200">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-200">Solution</span></span>
<span data-ttu-id="8902c-201">Se [QueueClient klass (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) information om hur toouse hello rekommenderad asynkron metod.</span><span class="sxs-lookup"><span data-stu-id="8902c-201">See [QueueClient Class (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) for information about how toouse hello recommended asynchronous method.</span></span>

<span data-ttu-id="8902c-202">tooimprove hello prestanda för hello Azure meddelandeinfrastruktur finns hello designmönstret [asynkrona meddelanden introduktion](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-202">tooimprove hello performance of hello Azure messaging infrastructure, see hello design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

## <a name="consider-partitioning-service-bus-queues-and-topics"></a><span data-ttu-id="8902c-203">Överväg att partitionering Service Bus-köer och ämnen</span><span class="sxs-lookup"><span data-stu-id="8902c-203">Consider partitioning Service Bus queues and topics</span></span>
### <a name="id"></a><span data-ttu-id="8902c-204">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-204">ID</span></span>
<span data-ttu-id="8902c-205">AP2004</span><span class="sxs-lookup"><span data-stu-id="8902c-205">AP2004</span></span>

### <a name="description"></a><span data-ttu-id="8902c-206">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-206">Description</span></span>
<span data-ttu-id="8902c-207">Partitionen Service Bus-köer och ämnen för bättre prestanda med Service Bus-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="8902c-207">Partition Service Bus queues and topics for better performance with Service Bus messaging.</span></span>

<span data-ttu-id="8902c-208">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-208">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-209">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-209">Reason</span></span>
<span data-ttu-id="8902c-210">Partitionering Service Bus-köer och ämnen ökar prestanda genomflöde och tjänsten tillgänglighet eftersom hello totala genomflödet av en partitionerad kö eller ett ämne begränsas inte längre av hello resultatet av ett enda meddelande broker eller meddelandearkiv.</span><span class="sxs-lookup"><span data-stu-id="8902c-210">Partitioning Service Bus queues and topics increases performance throughput and service availability because hello overall throughput of a partitioned queue or topic is no longer limited by hello performance of a single message broker or messaging store.</span></span> <span data-ttu-id="8902c-211">Dessutom kan spärra ett tillfälligt avbrott i ett meddelandearkiv inte en partitionerad kö eller ett ämne.</span><span class="sxs-lookup"><span data-stu-id="8902c-211">In addition, a temporary outage of a messaging store doesn’t make a partitioned queue or topic unavailable.</span></span> <span data-ttu-id="8902c-212">Mer information finns i [partitionering Meddelandeentiteter](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-212">For more information, see [Partitioning Messaging Entities](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="8902c-213">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-213">Solution</span></span>
<span data-ttu-id="8902c-214">Hej följande fragment visas hur toopartition meddelandeentiteter.</span><span class="sxs-lookup"><span data-stu-id="8902c-214">hello following code snippet shows how toopartition messaging entities.</span></span>

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

<span data-ttu-id="8902c-215">Mer information finns i [partitionerad Service Bus-köer och ämnen | Microsoft Azure-blogg](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) och checka ut hello [Microsoft Azure Service Bus partitionerad-kö](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) exempel.</span><span class="sxs-lookup"><span data-stu-id="8902c-215">For more information, see [Partitioned Service Bus Queues and Topics | Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) and check out hello [Microsoft Azure Service Bus Partitioned Queue](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) sample.</span></span>

## <a name="do-not-set-sharedaccessstarttime"></a><span data-ttu-id="8902c-216">Ange inte SharedAccessStartTime</span><span class="sxs-lookup"><span data-stu-id="8902c-216">Do not set SharedAccessStartTime</span></span>
### <a name="id"></a><span data-ttu-id="8902c-217">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-217">ID</span></span>
<span data-ttu-id="8902c-218">AP3001</span><span class="sxs-lookup"><span data-stu-id="8902c-218">AP3001</span></span>

### <a name="description"></a><span data-ttu-id="8902c-219">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-219">Description</span></span>
<span data-ttu-id="8902c-220">Du bör undvika att använda SharedAccessStartTimeset toohello klockslaget tooimmediately starta hello princip för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8902c-220">You should avoid using SharedAccessStartTimeset toohello current time tooimmediately start hello Shared Access policy.</span></span> <span data-ttu-id="8902c-221">Du behöver bara tooset den här egenskapen om du vill toostart hello delad åtkomstprincip vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="8902c-221">You only need tooset this property if you want toostart hello Shared Access policy at a later time.</span></span>

<span data-ttu-id="8902c-222">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-222">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-223">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-223">Reason</span></span>
<span data-ttu-id="8902c-224">Synkronisering av datorklocka medför en lätt tidsskillnaden mellan datacenter.</span><span class="sxs-lookup"><span data-stu-id="8902c-224">Clock synchronization causes a slight time difference among datacenters.</span></span> <span data-ttu-id="8902c-225">Du skulle till exempel logiskt se inställningen hello starttiden för lagring SAS-princip som hello aktuell tid med hjälp av DateTime.Now eller en liknande metod kommer hello SAS princip tootake gälla omedelbart.</span><span class="sxs-lookup"><span data-stu-id="8902c-225">For example, you would logically think setting hello start time of a storage SAS policy as hello current time by using DateTime.Now or a similar method will cause hello SAS policy tootake effect immediately.</span></span> <span data-ttu-id="8902c-226">Hello mindre tidsskillnaderna mellan Datacenter kan emellertid orsaka problem med den här eftersom datacenter ibland kan vara lite senare än starttiden för hello, medan andra i den.</span><span class="sxs-lookup"><span data-stu-id="8902c-226">However, hello slight time differences between datacenters can cause problems with this since some datacenter times might be slightly later than hello start time, while others ahead of it.</span></span> <span data-ttu-id="8902c-227">Därför hello SAS-princip kan gälla snabbt (eller även omedelbart) om hello princip livstid är för kort.</span><span class="sxs-lookup"><span data-stu-id="8902c-227">As a result, hello SAS policy can expire quickly (or even immediately) if hello policy lifetime is set too short.</span></span>

<span data-ttu-id="8902c-228">Mer information om med signatur för delad åtkomst på Azure storage finns [introduktion till tabellen SAS (signatur för delad åtkomst), Queue SAS och uppdatera tooBlob SAS - Microsoft Azure Storage-teamets blogg - platsen Start - MSDN-bloggarna](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-228">For more guidance on using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update tooBlob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="8902c-229">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-229">Solution</span></span>
<span data-ttu-id="8902c-230">Ta bort hello-uttryck som hello starttiden för hello delad åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="8902c-230">Remove hello statement that sets hello start time of hello shared access policy.</span></span> <span data-ttu-id="8902c-231">hello Azure kod analysverktyget är en lösning på problemet.</span><span class="sxs-lookup"><span data-stu-id="8902c-231">hello Azure Code Analysis tool provides a fix for this issue.</span></span> <span data-ttu-id="8902c-232">Mer information om säkerhetshantering finns hello designmönstret [Valet nyckeln mönster](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-232">For more information on security management, please see hello design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="8902c-233">hello följande kodfragment som visar hello kod korrigering för problemet.</span><span class="sxs-lookup"><span data-stu-id="8902c-233">hello following code snippet demonstrates hello code fix for this issue.</span></span>

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a><span data-ttu-id="8902c-234">Delad åtkomstprincip förfallotiden måste vara fler än fem minuter</span><span class="sxs-lookup"><span data-stu-id="8902c-234">Shared Access Policy expiry time must be more than five minutes</span></span>
### <a name="id"></a><span data-ttu-id="8902c-235">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-235">ID</span></span>
<span data-ttu-id="8902c-236">AP3002</span><span class="sxs-lookup"><span data-stu-id="8902c-236">AP3002</span></span>

### <a name="description"></a><span data-ttu-id="8902c-237">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-237">Description</span></span>
<span data-ttu-id="8902c-238">Det kan finnas lika mycket som en fem minuters skillnad i klockor mellan Datacenter på olika platser på grund av tooa tillstånd som kallas ”klockavvikelser”.</span><span class="sxs-lookup"><span data-stu-id="8902c-238">There can be as much as a five minute difference in clocks among datacenters at different locations due tooa condition known as "clock skew."</span></span> <span data-ttu-id="8902c-239">tooprevent hello SAS princip-token upphör att gälla ange tidigare än planerat, hello utgången tid toobe mer än fem minuter.</span><span class="sxs-lookup"><span data-stu-id="8902c-239">tooprevent hello SAS policy token from expiring earlier than planned, set hello expiry time toobe more than five minutes.</span></span>

<span data-ttu-id="8902c-240">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-240">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-241">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-241">Reason</span></span>
<span data-ttu-id="8902c-242">Datacenter på olika platser runt hello world synkronisera genom en signal klockan.</span><span class="sxs-lookup"><span data-stu-id="8902c-242">Datacenters at different locations around hello world synchronize by a clock signal.</span></span> <span data-ttu-id="8902c-243">Eftersom det tar tid för klockan signal tootravel toodifferent platser, kan det finnas en gång avvikelse mellan Datacenter på olika geografiska platser trots allt är synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="8902c-243">Because it takes time for clock signal tootravel toodifferent locations, there can be a time variance between datacenters at different geographical locations although everything is supposedly synchronized.</span></span> <span data-ttu-id="8902c-244">Den här tidsskillnaden kan påverka hello Shared Access start tid och förfallodatum avsökningsintervallet för klientprinciper.</span><span class="sxs-lookup"><span data-stu-id="8902c-244">This time difference can affect hello Shared Access policy start time and expiration interval.</span></span> <span data-ttu-id="8902c-245">Därför tooensure princip för delad åtkomst träder i kraft omedelbart, anger inte hello starttid.</span><span class="sxs-lookup"><span data-stu-id="8902c-245">Therefore, tooensure Shared Access policy takes effect immediately, don’t specify hello start time.</span></span> <span data-ttu-id="8902c-246">Kontrollera dessutom att hello förfallotid är mer än 5 minuter tooprevent tidig timeout.</span><span class="sxs-lookup"><span data-stu-id="8902c-246">In addition, make sure hello expiration time is more than 5 minutes tooprevent early timeout.</span></span>

<span data-ttu-id="8902c-247">Mer information om hur du använder signatur för delad åtkomst på Azure storage finns [introduktion till tabellen SAS (signatur för delad åtkomst), Queue SAS och uppdatera tooBlob SAS - Microsoft Azure Storage-teamets blogg - platsen Start - MSDN-bloggarna](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-247">For more information about using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update tooBlob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="8902c-248">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-248">Solution</span></span>
<span data-ttu-id="8902c-249">Mer information om säkerhetshantering finns hello designmönstret [Valet nyckeln mönster](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-249">For more information on security management, see hello design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="8902c-250">hello följande är ett exempel på inte att ange en starttid för delad åtkomst princip.</span><span class="sxs-lookup"><span data-stu-id="8902c-250">hello following is an example of not specifying a Shared Access policy start time.</span></span>

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="8902c-251">hello följande är ett exempel på att ange en starttid för delad åtkomst principen med princip upphör att gälla mer än fem minuter.</span><span class="sxs-lookup"><span data-stu-id="8902c-251">hello following is an example of specifying a Shared Access policy start time with a policy expiration duration greater than five minutes.</span></span>

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="8902c-252">Mer information finns i [skapar och använder en signatur för delad åtkomst](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-252">For more information, see [Create and Use a Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span></span>

## <a name="use-cloudconfigurationmanager"></a><span data-ttu-id="8902c-253">Använd CloudConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="8902c-253">Use CloudConfigurationManager</span></span>
### <a name="id"></a><span data-ttu-id="8902c-254">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-254">ID</span></span>
<span data-ttu-id="8902c-255">AP4000</span><span class="sxs-lookup"><span data-stu-id="8902c-255">AP4000</span></span>

### <a name="description"></a><span data-ttu-id="8902c-256">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-256">Description</span></span>
<span data-ttu-id="8902c-257">Med hjälp av hello [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) klassen för projekt som Azure-webbplats och Azure mobile services inte introducerar runtime problem.</span><span class="sxs-lookup"><span data-stu-id="8902c-257">Using hello [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) class for projects such as Azure Website and Azure mobile services won't introduce runtime issues.</span></span> <span data-ttu-id="8902c-258">Som bästa praxis, men det är en bra idé toouse moln[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) som ett enhetligt sätt att hantera konfigurationer för alla program i Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="8902c-258">As a best practice, however, it's a good idea toouse Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) as a unified way of managing configurations for all Azure Cloud applications.</span></span>

<span data-ttu-id="8902c-259">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-259">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-260">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-260">Reason</span></span>
<span data-ttu-id="8902c-261">CloudConfigurationManager läser hello configuration file lämpliga toohello programmiljö.</span><span class="sxs-lookup"><span data-stu-id="8902c-261">CloudConfigurationManager reads hello configuration file appropriate toohello application environment.</span></span>

[<span data-ttu-id="8902c-262">CloudConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="8902c-262">CloudConfigurationManager</span></span>](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a><span data-ttu-id="8902c-263">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-263">Solution</span></span>
<span data-ttu-id="8902c-264">Refactor din kod toouse hello [CloudConfigurationManager-klassen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-264">Refactor your code toouse hello [CloudConfigurationManager Class](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span></span> <span data-ttu-id="8902c-265">Hello Azure kod analysverktyg som en kod korrigering för problemet.</span><span class="sxs-lookup"><span data-stu-id="8902c-265">A code fix for this issue is provided by hello Azure Code Analysis tool.</span></span>

<span data-ttu-id="8902c-266">hello följande kodfragment som visar hello kod korrigering för problemet.</span><span class="sxs-lookup"><span data-stu-id="8902c-266">hello following code snippet demonstrates hello code fix for this issue.</span></span> <span data-ttu-id="8902c-267">Ersätt</span><span class="sxs-lookup"><span data-stu-id="8902c-267">Replace</span></span>

`var settings = ConfigurationManager.AppSettings["mySettings"];`

<span data-ttu-id="8902c-268">med</span><span class="sxs-lookup"><span data-stu-id="8902c-268">with</span></span>

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

<span data-ttu-id="8902c-269">Här är ett exempel på hur toostore hello Konfigurationsinställningen i App.config eller Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="8902c-269">Here's an example of how toostore hello configuration setting in a App.config or Web.config file.</span></span> <span data-ttu-id="8902c-270">Lägg till hello inställningar toohello AppSettings i konfigurationsfilen för hello.</span><span class="sxs-lookup"><span data-stu-id="8902c-270">Add hello settings toohello appSettings section of hello configuration file.</span></span> <span data-ttu-id="8902c-271">hello följer hello Web.config-filen för hello förra kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="8902c-271">hello following is hello Web.config file for hello previous code example.</span></span>

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a><span data-ttu-id="8902c-272">Undvik att använda hårdkodade anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="8902c-272">Avoid using hard-coded connection strings</span></span>
### <a name="id"></a><span data-ttu-id="8902c-273">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-273">ID</span></span>
<span data-ttu-id="8902c-274">AP4001</span><span class="sxs-lookup"><span data-stu-id="8902c-274">AP4001</span></span>

### <a name="description"></a><span data-ttu-id="8902c-275">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-275">Description</span></span>
<span data-ttu-id="8902c-276">Om du använder hårdkodade anslutningssträngar och du behöver tooupdate dem senare, du har toomake ändringar tooyour källkoden och omkompilera hello program.</span><span class="sxs-lookup"><span data-stu-id="8902c-276">If you use hard-coded connection strings and you need tooupdate them later, you’ll have toomake changes tooyour source code and recompile hello application.</span></span> <span data-ttu-id="8902c-277">Men om du sparar din anslutningssträngar i en konfigurationsfil, kan du ändra dem senare genom att helt enkelt uppdatera hello konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="8902c-277">However, if you store your connection strings in a configuration file, you can change them later by simply updating hello configuration file.</span></span>

<span data-ttu-id="8902c-278">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-278">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-279">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-279">Reason</span></span>
<span data-ttu-id="8902c-280">Hårdkoda anslutningssträngar är felaktigt eftersom problem när anslutningssträngar behöver toobe som ändras snabbt.</span><span class="sxs-lookup"><span data-stu-id="8902c-280">Hard-coding connection strings is a bad practice because it introduces problems when connection strings need toobe changed quickly.</span></span> <span data-ttu-id="8902c-281">Dessutom hello projektet måste toobe incheckad toosource kontroll, införa hårdkodade anslutningssträngar säkerhetsrisker eftersom hello strängar kan visas i hello källkod.</span><span class="sxs-lookup"><span data-stu-id="8902c-281">In addition, if hello project needs toobe checked in toosource control, hard-coded connection strings introduce security vulnerabilities since hello strings can be viewed in hello source code.</span></span>

### <a name="solution"></a><span data-ttu-id="8902c-282">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-282">Solution</span></span>
<span data-ttu-id="8902c-283">Lagrar anslutningssträngar i hello configuration-filer eller miljöer i Azure.</span><span class="sxs-lookup"><span data-stu-id="8902c-283">Store connection strings in hello configuration files or Azure environments.</span></span>

* <span data-ttu-id="8902c-284">För fristående program kan du använda inställningarna för app.config toostore-anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="8902c-284">For standalone applications, use app.config toostore connection string settings.</span></span>
* <span data-ttu-id="8902c-285">Använd web.config toostore anslutningssträngar för IIS-värdbaserade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="8902c-285">For IIS-hosted web applications, use web.config toostore connection strings.</span></span>
* <span data-ttu-id="8902c-286">Använd configuration.json toostore anslutningssträngar för ASP.NET-program vNext.</span><span class="sxs-lookup"><span data-stu-id="8902c-286">For ASP.NET vNext applications, use configuration.json toostore connection strings.</span></span>

<span data-ttu-id="8902c-287">Information om hur du använder konfigurationer filer, till exempel web.config eller app.config finns [riktlinjer för konfiguration av ASP.NET Web](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span><span class="sxs-lookup"><span data-stu-id="8902c-287">For information on using configurations files such as web.config or app.config, see [ASP.NET Web Configuration Guidelines](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span></span> <span data-ttu-id="8902c-288">Mer information om hur Azure miljö variabler arbete finns [Azure webbplatser: hur programmet strängar och anslutningen strängar fungerar](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span><span class="sxs-lookup"><span data-stu-id="8902c-288">For information on how Azure environment variables work, see [Azure Web Sites: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span></span> <span data-ttu-id="8902c-289">Mer information om lagra anslutningssträngen i källkontrollen finns [undvika att sätta känslig information, till exempel anslutningssträngar i filer som lagras i koden källdatabasen](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span><span class="sxs-lookup"><span data-stu-id="8902c-289">For information on storing connection string in source control, see [avoid putting sensitive information such as connection strings in files that are stored in source code repository](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span></span>

## <a name="use-diagnostics-configuration-file"></a><span data-ttu-id="8902c-290">Använd diagnostik-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="8902c-290">Use diagnostics configuration file</span></span>
### <a name="id"></a><span data-ttu-id="8902c-291">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-291">ID</span></span>
<span data-ttu-id="8902c-292">AP5000</span><span class="sxs-lookup"><span data-stu-id="8902c-292">AP5000</span></span>

### <a name="description"></a><span data-ttu-id="8902c-293">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-293">Description</span></span>
<span data-ttu-id="8902c-294">I stället för att konfigurera diagnostikinställningar för i din kod som genom att använda hello Microsoft.WindowsAzure.Diagnostics programmering API bör du konfigurera inställningarna för webbprogramdiagnostik i hello diagnostics.wadcfg-filen.</span><span class="sxs-lookup"><span data-stu-id="8902c-294">Instead of configuring diagnostics settings in your code such as by using hello Microsoft.WindowsAzure.Diagnostics programming API, you should configure diagnostics settings in hello diagnostics.wadcfg file.</span></span> <span data-ttu-id="8902c-295">(Eller diagnostics.wadcfgx om du använder Azure SDK 2.5).</span><span class="sxs-lookup"><span data-stu-id="8902c-295">(Or, diagnostics.wadcfgx if you use Azure SDK 2.5).</span></span> <span data-ttu-id="8902c-296">Du kan ändra inställningarna för webbprogramdiagnostik utan toorecompile din kod genom att göra.</span><span class="sxs-lookup"><span data-stu-id="8902c-296">By doing this, you can change diagnostics settings without having toorecompile your code.</span></span>

<span data-ttu-id="8902c-297">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-297">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-298">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-298">Reason</span></span>
<span data-ttu-id="8902c-299">Innan Azure SDK 2,5 (som använder Azure diagnostics 1.3), Azure Diagnostics (BOMULLSTUSS) kan konfigureras med hjälp av flera olika metoder: lägga till den toohello configuration blob i lagring, med hjälp av tvingande koden, deklarativa konfiguration eller hello standard konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8902c-299">Before Azure SDK 2.5 (which uses Azure diagnostics 1.3), Azure Diagnostics (WAD) could be configured by using several different methods: adding it toohello configuration blob in storage, by using imperative code, declarative configuration, or hello default configuration.</span></span> <span data-ttu-id="8902c-300">Dock rekommenderas hello sätt tooconfigure diagnostics är toouse en XML-konfigurationsfil (diagnostics.wadcfg eller diagnositcs.wadcfgx för SDK-2.5 och senare) i hello projektet.</span><span class="sxs-lookup"><span data-stu-id="8902c-300">However, hello preferred way tooconfigure diagnostics is toouse an XML configuration file (diagnostics.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later) in hello application project.</span></span> <span data-ttu-id="8902c-301">I den här metoden kan hello diagnostics.wadcfg filen helt definierar hello konfigurationen och uppdateras och omdistribueras när du vill.</span><span class="sxs-lookup"><span data-stu-id="8902c-301">In this approach, hello diagnostics.wadcfg file completely defines hello configuration and can be updated and redeployed at will.</span></span> <span data-ttu-id="8902c-302">Blanda hello användning av hello diagnostics.wadcfg konfigurationsfilen med hello programmering för konfigurationer med hjälp av hello [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)eller [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx) klasser kan leda tooconfusion.</span><span class="sxs-lookup"><span data-stu-id="8902c-302">Mixing hello use of hello diagnostics.wadcfg configuration file with hello programmatic methods of setting configurations by using hello [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)or [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)classes can lead tooconfusion.</span></span> <span data-ttu-id="8902c-303">Se [initieras eller ändra Azure Diagnostics konfiguration](https://msdn.microsoft.com/library/azure/hh411537.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="8902c-303">See [Initialize or Change Azure Diagnostics Configuration](https://msdn.microsoft.com/library/azure/hh411537.aspx) for more information.</span></span>

<span data-ttu-id="8902c-304">Från och med BOMULLSTUSS 1.3 (ingår i Azure SDK 2.5) kan den inte längre möjligt toouse kod tooconfigure diagnostik.</span><span class="sxs-lookup"><span data-stu-id="8902c-304">Beginning with WAD 1.3 (included with Azure SDK 2.5), it’s no longer possible toouse code tooconfigure diagnostics.</span></span> <span data-ttu-id="8902c-305">Därför kan du bara ange hello konfiguration när du tillämpar eller uppdaterar hello diagnostik tillägg.</span><span class="sxs-lookup"><span data-stu-id="8902c-305">As a result, you can only provide hello configuration when applying or updating hello diagnostics extension.</span></span>

### <a name="solution"></a><span data-ttu-id="8902c-306">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-306">Solution</span></span>
<span data-ttu-id="8902c-307">Använda hello diagnostik configuration designer toomove diagnostikinställningar toohello diagnostik konfigurationsfil (diagnositcs.wadcfg eller diagnositcs.wadcfgx för SDK-2.5 och senare).</span><span class="sxs-lookup"><span data-stu-id="8902c-307">Use hello diagnostics configuration designer toomove diagnostic settings toohello diagnostics configuration file (diagnositcs.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later).</span></span> <span data-ttu-id="8902c-308">Det rekommenderas också att du installerar [Azure SDK 2,5](http://go.microsoft.com/fwlink/?LinkId=513188) och använda hello senaste diagnostik.</span><span class="sxs-lookup"><span data-stu-id="8902c-308">It’s also recommended that you install [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) and use hello latest diagnostics feature.</span></span>

1. <span data-ttu-id="8902c-309">Hello snabbmenyn för hello roll som du vill tooconfigure väljer Egenskaper och klicka på fliken för hello-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8902c-309">On hello shortcut menu for hello role that you want tooconfigure, choose Properties, and then choose hello Configuration tab.</span></span>
2. <span data-ttu-id="8902c-310">I hello **diagnostik** Kontrollera att hello **aktivera diagnostik** är markerad.</span><span class="sxs-lookup"><span data-stu-id="8902c-310">In hello **Diagnostics** section, make sure that hello **Enable Diagnostics** check box is selected.</span></span>
3. <span data-ttu-id="8902c-311">Välj hello **konfigurera** knappen.</span><span class="sxs-lookup"><span data-stu-id="8902c-311">Choose hello **Configure** button.</span></span>

   ![Åtkomst till hello aktivera diagnostik alternativet](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   <span data-ttu-id="8902c-313">Se [konfigurera diagnostik för virtuella datorer och Azure Cloud Services](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="8902c-313">See [Configuring Diagnostics for Azure Cloud Services and Virtual Machines](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) for more information.</span></span>

## <a name="avoid-declaring-dbcontext-objects-as-static"></a><span data-ttu-id="8902c-314">Undvik att deklarera DbContext-objekt som statisk</span><span class="sxs-lookup"><span data-stu-id="8902c-314">Avoid declaring DbContext objects as static</span></span>
### <a name="id"></a><span data-ttu-id="8902c-315">ID</span><span class="sxs-lookup"><span data-stu-id="8902c-315">ID</span></span>
<span data-ttu-id="8902c-316">AP6000</span><span class="sxs-lookup"><span data-stu-id="8902c-316">AP6000</span></span>

### <a name="description"></a><span data-ttu-id="8902c-317">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8902c-317">Description</span></span>
<span data-ttu-id="8902c-318">toosave minne undvika deklarerar DBContext-objekt som statisk.</span><span class="sxs-lookup"><span data-stu-id="8902c-318">toosave memory, avoid declaring DBContext objects as static.</span></span>

<span data-ttu-id="8902c-319">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="8902c-319">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="8902c-320">Orsak</span><span class="sxs-lookup"><span data-stu-id="8902c-320">Reason</span></span>
<span data-ttu-id="8902c-321">DBContext-objekt innehåller hello frågeresultat från varje anrop.</span><span class="sxs-lookup"><span data-stu-id="8902c-321">DBContext objects hold hello query results from each call.</span></span> <span data-ttu-id="8902c-322">Statiska DBContext-objekt är inte bort förrän hello programdomänen tas bort.</span><span class="sxs-lookup"><span data-stu-id="8902c-322">Static DBContext objects are not disposed until hello application domain is unloaded.</span></span> <span data-ttu-id="8902c-323">Därför kan ett statiskt DBContext-objekt förbruka stora mängder minne.</span><span class="sxs-lookup"><span data-stu-id="8902c-323">Therefore, a static DBContext object can consume large amounts of memory.</span></span>

### <a name="solution"></a><span data-ttu-id="8902c-324">Lösning</span><span class="sxs-lookup"><span data-stu-id="8902c-324">Solution</span></span>
<span data-ttu-id="8902c-325">Deklarera DBContext som en lokal variabel eller icke-statisk instansfält, använda den för en aktivitet och låt den avyttras efter användning.</span><span class="sxs-lookup"><span data-stu-id="8902c-325">Declare DBContext as a local variable or non-static instance field, use it for a task, and then let it be disposed of after use.</span></span>

<span data-ttu-id="8902c-326">hello följande exempel MVC kontrollantklassen visar hur toouse hello DBContext-objektet.</span><span class="sxs-lookup"><span data-stu-id="8902c-326">hello following example MVC controller class shows how toouse hello DBContext object.</span></span>

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass tooDbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a><span data-ttu-id="8902c-327">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8902c-327">Next steps</span></span>
<span data-ttu-id="8902c-328">toolearn mer om hur du optimerar och felsöka Azure apps finns [felsöka en webbapp i Azure App Service med Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="8902c-328">toolearn more about optimizing and troubleshooting Azure apps, see [Troubleshoot a web app in Azure App Service using Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>
