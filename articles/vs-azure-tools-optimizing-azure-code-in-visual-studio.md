---
title: Optimera din Azure kod i Visual Studio | Microsoft Docs
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
ms.openlocfilehash: 8f145502a856798d6e69ac11f324c72fa23f938e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="optimizing-your-azure-code"></a><span data-ttu-id="a9a35-103">Optimera din Azure kod</span><span class="sxs-lookup"><span data-stu-id="a9a35-103">Optimizing Your Azure Code</span></span>
<span data-ttu-id="a9a35-104">När du programmering appar som använder Microsoft Azure, finns det vissa kodningsexempel som du bör följa för att undvika problem med appen skalbarhet, beteende och prestanda i en molnmiljö.</span><span class="sxs-lookup"><span data-stu-id="a9a35-104">When you’re programming apps that use Microsoft Azure, there are some coding practices you should follow to help avoid problems with app scalability, behavior and performance in a cloud environment.</span></span> <span data-ttu-id="a9a35-105">Microsoft tillhandahåller ett analysverktyg i Azure kod som identifierar och identifierar de dessa ofta påträffade problem och hjälper dig att lösa dem.</span><span class="sxs-lookup"><span data-stu-id="a9a35-105">Microsoft provides an Azure Code Analysis tool that recognizes and identifies several of these commonly-encountered issues and helps you resolve them.</span></span> <span data-ttu-id="a9a35-106">Du kan hämta verktyget i Visual Studio via NuGet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-106">You can download the tool in Visual Studio via NuGet.</span></span>

## <a name="azure-code-analysis-rules"></a><span data-ttu-id="a9a35-107">Azure kod Analysis-regler</span><span class="sxs-lookup"><span data-stu-id="a9a35-107">Azure Code Analysis rules</span></span>
<span data-ttu-id="a9a35-108">Verktyget Azure kod används följande regler för att flaggan Azure koden automatiskt när den söker efter kända problem som påverkar prestanda.</span><span class="sxs-lookup"><span data-stu-id="a9a35-108">The Azure Code Analysis tool uses the following rules to automatically flag your Azure code when it finds known performance-impacting issues.</span></span> <span data-ttu-id="a9a35-109">Identifierat problem visas som en varningar eller fel.</span><span class="sxs-lookup"><span data-stu-id="a9a35-109">Detected issues appear as a warnings or compiler errors.</span></span> <span data-ttu-id="a9a35-110">Koden korrigeringar eller förslag för att lösa varningen eller felet sker ofta via en lampikonen.</span><span class="sxs-lookup"><span data-stu-id="a9a35-110">Code fixes or suggestions to resolve the warning or error are often provided through a light bulb icon.</span></span>

## <a name="avoid-using-default-in-process-session-state-mode"></a><span data-ttu-id="a9a35-111">Undvik att använda sessionstillståndsläge för standard (pågående)</span><span class="sxs-lookup"><span data-stu-id="a9a35-111">Avoid using default (in-process) session state mode</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-112">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-112">ID</span></span>
<span data-ttu-id="a9a35-113">AP0000</span><span class="sxs-lookup"><span data-stu-id="a9a35-113">AP0000</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-114">Description</span></span>
<span data-ttu-id="a9a35-115">Om du använder den standard (pågående) sessionstillståndsläge för molnprogram, förlorar du sessionstillstånd.</span><span class="sxs-lookup"><span data-stu-id="a9a35-115">If you use the default (in-process) session state mode for cloud applications, you can lose session state.</span></span>

<span data-ttu-id="a9a35-116">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-116">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-117">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-117">Reason</span></span>
<span data-ttu-id="a9a35-118">Som standard är sessionstillståndsläge som angetts i web.config-filen i processen.</span><span class="sxs-lookup"><span data-stu-id="a9a35-118">By default, the session state mode specified in the web.config file is in-process.</span></span> <span data-ttu-id="a9a35-119">Även om ingen post som angetts i konfigurationsfilen standardvärden läge för sessionstillstånd om du vill i processen.</span><span class="sxs-lookup"><span data-stu-id="a9a35-119">Also, if no entry specified in the configuration file, the Session State mode defaults to in-process.</span></span> <span data-ttu-id="a9a35-120">Läget i processen lagrar sessionstillstånd i minnet på webbservern.</span><span class="sxs-lookup"><span data-stu-id="a9a35-120">The in-process mode stores session state in memory on the web server.</span></span> <span data-ttu-id="a9a35-121">Sessionstillstånd lagras i minnet på webbservern sparas inte när en instans har startats om eller en ny instans används för belastningsutjämning eller stöd för redundans.</span><span class="sxs-lookup"><span data-stu-id="a9a35-121">When an instance is restarted or a new instance is used for load balancing or failover support, the session state stored in memory on the web server isn’t saved.</span></span> <span data-ttu-id="a9a35-122">Detta hindrar programmet från att vara skalbar i molnet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-122">This situation prevents the application from being scalable on the cloud.</span></span>

<span data-ttu-id="a9a35-123">Sessionstillståndet ASP.NET stöder flera olika lagringsalternativ för sessionstillståndsdata: InProc, StateServer, SQLServer, anpassad, och ut.</span><span class="sxs-lookup"><span data-stu-id="a9a35-123">ASP.NET session state supports several different storage options for session state data: InProc, StateServer, SQLServer, Custom, and Off.</span></span> <span data-ttu-id="a9a35-124">Det rekommenderas att du använder läget för anpassade inställningar som värd för data på en extern sessionstillstånd butik som [Azure sessionstillståndsprovider för Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span><span class="sxs-lookup"><span data-stu-id="a9a35-124">It’s recommended that you use Custom mode to host data on an external Session State store, such as [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span></span>

### <a name="solution"></a><span data-ttu-id="a9a35-125">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-125">Solution</span></span>
<span data-ttu-id="a9a35-126">En rekommenderad lösning är att lagra sessionstillstånd i en hanterad cache-tjänst.</span><span class="sxs-lookup"><span data-stu-id="a9a35-126">One recommended solution is to store session state on a managed cache service.</span></span> <span data-ttu-id="a9a35-127">Lär dig hur du använder [Azure sessionstillståndsprovider för Redis](http://go.microsoft.com/fwlink/?LinkId=401521) att lagra dina sessionstillstånd.</span><span class="sxs-lookup"><span data-stu-id="a9a35-127">Learn how to use [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521) to store your session state.</span></span> <span data-ttu-id="a9a35-128">Du kan också lagra sessionstillstånd i andra platser att se till att programmet är skalbart i molnet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-128">You can also store session state in other places to ensure your application is scalable on the cloud.</span></span> <span data-ttu-id="a9a35-129">Mer information om alternativa lösningar Läs [Session tillstånd lägen](https://msdn.microsoft.com/library/ms178586).</span><span class="sxs-lookup"><span data-stu-id="a9a35-129">To learn more about alternative solutions please read [Session State Modes](https://msdn.microsoft.com/library/ms178586).</span></span>

## <a name="run-method-should-not-be-async"></a><span data-ttu-id="a9a35-130">Kör metoden får inte vara asynkrona</span><span class="sxs-lookup"><span data-stu-id="a9a35-130">Run method should not be async</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-131">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-131">ID</span></span>
<span data-ttu-id="a9a35-132">AP1000</span><span class="sxs-lookup"><span data-stu-id="a9a35-132">AP1000</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-133">Description</span></span>
<span data-ttu-id="a9a35-134">Skapa asynkrona metoder (som [await](https://msdn.microsoft.com/library/hh156528.aspx)) utanför det [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod och sedan anropa de asynkron metoderna från [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-134">Create asynchronous methods (such as [await](https://msdn.microsoft.com/library/hh156528.aspx)) outside of the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and then call the async methods from [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span></span> <span data-ttu-id="a9a35-135">Deklarerar den [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden som asynkron gör worker-rollen att ange en omstart loop.</span><span class="sxs-lookup"><span data-stu-id="a9a35-135">Declaring the [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method as async causes the worker role to enter a restart loop.</span></span>

<span data-ttu-id="a9a35-136">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-136">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-137">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-137">Reason</span></span>
<span data-ttu-id="a9a35-138">Anropar asynkrona metoder i den [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden gör cloud service runtime att återvinna arbetsrollen.</span><span class="sxs-lookup"><span data-stu-id="a9a35-138">Calling async methods inside the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes the cloud service runtime to recycle the worker role.</span></span> <span data-ttu-id="a9a35-139">När en arbetsroll startar alla programkörningen äger rum inom den [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="a9a35-139">When a worker role starts, all program execution takes place inside the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="a9a35-140">Avslutar den [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden gör arbetsrollen att starta om.</span><span class="sxs-lookup"><span data-stu-id="a9a35-140">Exiting the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes the worker role to restart.</span></span> <span data-ttu-id="a9a35-141">När worker-rollen runtime träffar async-metod, skickar alla åtgärder efter async-metod och returnerar sedan.</span><span class="sxs-lookup"><span data-stu-id="a9a35-141">When the worker role runtime hits the async method, it dispatches all operations after the async method and then returns.</span></span> <span data-ttu-id="a9a35-142">Detta gör att arbetsrollen du avsluta den [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod och starta om.</span><span class="sxs-lookup"><span data-stu-id="a9a35-142">This causes the worker role to exit from the [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and restart.</span></span> <span data-ttu-id="a9a35-143">I nästa version av körningen arbetsrollen träffar asynkrona metoden igen och startar om orsakar arbetsrollen att återvinna samt igen.</span><span class="sxs-lookup"><span data-stu-id="a9a35-143">In the next iteration of execution, the worker role hits the async method again and restarts, causing the worker role to recycle again as well.</span></span>

### <a name="solution"></a><span data-ttu-id="a9a35-144">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-144">Solution</span></span>
<span data-ttu-id="a9a35-145">Placera alla asynkrona åtgärder utanför den [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="a9a35-145">Place all async operations outside of the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="a9a35-146">Anropa sedan metoden omstrukturerade asynkrona från inuti den [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod, till exempel RunAsync () .wait.</span><span class="sxs-lookup"><span data-stu-id="a9a35-146">Then, call the refactored async method from inside the [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method, such as RunAsync().wait.</span></span> <span data-ttu-id="a9a35-147">Verktyget Azure kod kan hjälpa dig att lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-147">The Azure Code Analysis tool can help you fix this issue.</span></span>

<span data-ttu-id="a9a35-148">Följande kodavsnitt visar koden korrigering för problemet:</span><span class="sxs-lookup"><span data-stu-id="a9a35-148">The following code snippet demonstrates the code fix for this issue:</span></span>

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

## <a name="use-service-bus-shared-access-signature-authentication"></a><span data-ttu-id="a9a35-149">Använda Service Bus signatur för delad åtkomst-autentisering</span><span class="sxs-lookup"><span data-stu-id="a9a35-149">Use Service Bus Shared Access Signature authentication</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-150">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-150">ID</span></span>
<span data-ttu-id="a9a35-151">AP2000</span><span class="sxs-lookup"><span data-stu-id="a9a35-151">AP2000</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-152">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-152">Description</span></span>
<span data-ttu-id="a9a35-153">Använda delade signatur åtkomst (SAS) för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a9a35-153">Use Shared Access Signature (SAS) for authentication.</span></span> <span data-ttu-id="a9a35-154">Access Control Service (ACS) är inaktuell för service bus-autentisering.</span><span class="sxs-lookup"><span data-stu-id="a9a35-154">Access Control Service (ACS) is being deprecated for service bus authentication.</span></span>

<span data-ttu-id="a9a35-155">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-155">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-156">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-156">Reason</span></span>
<span data-ttu-id="a9a35-157">För förbättrad säkerhet ersätter Azure Active Directory ACS-autentisering med SAS-autentisering.</span><span class="sxs-lookup"><span data-stu-id="a9a35-157">For enhanced security, Azure Active Directory is replacing ACS authentication with SAS authentication.</span></span> <span data-ttu-id="a9a35-158">Se [Azure Active Directory är framtiden för ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) information om övergång planen.</span><span class="sxs-lookup"><span data-stu-id="a9a35-158">See [Azure Active Directory is the future of ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) for information on the transition plan.</span></span>

### <a name="solution"></a><span data-ttu-id="a9a35-159">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-159">Solution</span></span>
<span data-ttu-id="a9a35-160">Använd SAS-autentisering i dina appar.</span><span class="sxs-lookup"><span data-stu-id="a9a35-160">Use SAS authentication in your apps.</span></span> <span data-ttu-id="a9a35-161">I följande exempel visar hur du använder en befintlig SAS-token för åtkomst till en service bus-namnrymd eller enhet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-161">The following example shows how to use an existing SAS token to access a service bus namespace or entity.</span></span>

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

<span data-ttu-id="a9a35-162">Finns i följande avsnitt för mer information.</span><span class="sxs-lookup"><span data-stu-id="a9a35-162">See the following topics for more information.</span></span>

* <span data-ttu-id="a9a35-163">En översikt finns [signatur autentisering för delad åtkomst med Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span><span class="sxs-lookup"><span data-stu-id="a9a35-163">For an overview, see [Shared Access Signature Authentication with Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span></span>
* [<span data-ttu-id="a9a35-164">Hur du använder autentisering med signatur för delad åtkomst med Service Bus</span><span class="sxs-lookup"><span data-stu-id="a9a35-164">How to use Shared Access Signature Authentication with Service Bus</span></span>](https://msdn.microsoft.com/library/dn205161.aspx)
* <span data-ttu-id="a9a35-165">En exempelprojektet finns [autentisering med delad signatur åtkomst (SAS) med Service Bus-prenumerationer](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span><span class="sxs-lookup"><span data-stu-id="a9a35-165">For a sample project, see [Using Shared Access Signature (SAS) authentication with Service Bus Subscriptions](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span></span>

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a><span data-ttu-id="a9a35-166">Överväg att använda OnMessage metod för att undvika ”meddelandemottagning”</span><span class="sxs-lookup"><span data-stu-id="a9a35-166">Consider using OnMessage method to avoid "receive loop"</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-167">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-167">ID</span></span>
<span data-ttu-id="a9a35-168">AP2002</span><span class="sxs-lookup"><span data-stu-id="a9a35-168">AP2002</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-169">Description</span></span>
<span data-ttu-id="a9a35-170">Att undvika att gå till en ”meddelandemottagning”, anropar den **OnMessage** metoden är en bättre lösning för att ta emot meddelanden än anropar den **Receive** metod.</span><span class="sxs-lookup"><span data-stu-id="a9a35-170">To avoid going into a "receive loop," calling the **OnMessage** method is a better solution for receiving messages than calling the **Receive** method.</span></span> <span data-ttu-id="a9a35-171">Men om du måste använda den **Receive** metod och ange väntetiden för en icke-standard-servern, kontrollera server väntetiden är mer än en minut.</span><span class="sxs-lookup"><span data-stu-id="a9a35-171">However, if you must use the **Receive** method, and you specify a non-default server wait time, make sure the server wait time is more than one minute.</span></span>

<span data-ttu-id="a9a35-172">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-172">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-173">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-173">Reason</span></span>
<span data-ttu-id="a9a35-174">När du anropar **OnMessage**, klienten startar ett internt meddelande pump som ständigt avsöker kö eller prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a9a35-174">When calling **OnMessage**, the client starts an internal message pump that constantly polls the queue or subscription.</span></span> <span data-ttu-id="a9a35-175">Det här meddelandet pump innehåller en oändlig loop som utfärdar ett anrop för att ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a9a35-175">This message pump contains an infinite loop that issues a call to receive messages.</span></span> <span data-ttu-id="a9a35-176">Om anropet uppnår tidsgränsen, utfärdas ett nytt anrop.</span><span class="sxs-lookup"><span data-stu-id="a9a35-176">If the call times out, it issues a new call.</span></span> <span data-ttu-id="a9a35-177">Timeout-intervall bestäms av värdet för den [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) -egenskapen för den [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)som används.</span><span class="sxs-lookup"><span data-stu-id="a9a35-177">The timeout interval is determined by the value of the [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) property of the [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)that’s being used.</span></span>

<span data-ttu-id="a9a35-178">Fördelen med att använda **OnMessage** jämfört med **Receive** är att användarna inte behöver manuellt söka efter meddelanden, hantera undantag, bearbeta flera meddelanden parallellt och slutföra meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a9a35-178">The advantage of using **OnMessage** compared to **Receive** is that users don’t have to manually poll for messages, handle exceptions, process multiple messages in parallel, and complete the messages.</span></span>

<span data-ttu-id="a9a35-179">Om du anropar **Receive** utan att använda standardvärdet, måste den *ServerWaitTime* värdet är mer än en minut.</span><span class="sxs-lookup"><span data-stu-id="a9a35-179">If you call **Receive** without using its default value, be sure the *ServerWaitTime* value is more than one minute.</span></span> <span data-ttu-id="a9a35-180">Ange *ServerWaitTime* till mer än en minut förhindrar att servern timeout innan det är fullständigt ta emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-180">Setting *ServerWaitTime* to more than one minute prevents the server from timing out before the message is fully received.</span></span>

### <a name="solution"></a><span data-ttu-id="a9a35-181">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-181">Solution</span></span>
<span data-ttu-id="a9a35-182">Se följande kodexempel för rekommenderade användningsområden.</span><span class="sxs-lookup"><span data-stu-id="a9a35-182">Please see the following code examples for recommended usages.</span></span> <span data-ttu-id="a9a35-183">Mer information finns i [QueueClient.OnMessage-metoden (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)och [QueueClient.Receive-metoden (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-183">For more details, see [QueueClient.OnMessage Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)and [QueueClient.Receive Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span></span>

<span data-ttu-id="a9a35-184">För att förbättra prestanda för Azure meddelandeinfrastrukturen finns designmönstret [asynkrona meddelanden introduktion](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-184">To improve the performance of the Azure messaging infrastructure, see the design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

<span data-ttu-id="a9a35-185">Följande är ett exempel på hur **OnMessage** ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a9a35-185">The following is an example of using **OnMessage** to receive messages.</span></span>

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

<span data-ttu-id="a9a35-186">Följande är ett exempel på hur **ta emot** med standardservern väntetid.</span><span class="sxs-lookup"><span data-stu-id="a9a35-186">The following is an example of using **Receive** with the default server wait time.</span></span>

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

<span data-ttu-id="a9a35-187">Följande är ett exempel på hur **Receive** väntetid med en server som inte är standard.</span><span class="sxs-lookup"><span data-stu-id="a9a35-187">The following is an example of using **Receive** with a non-default server wait time.</span></span>

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
## <a name="consider-using-asynchronous-service-bus-methods"></a><span data-ttu-id="a9a35-188">Överväg att använda asynkrona Service Bus-metoder</span><span class="sxs-lookup"><span data-stu-id="a9a35-188">Consider using asynchronous Service Bus methods</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-189">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-189">ID</span></span>
<span data-ttu-id="a9a35-190">AP2003</span><span class="sxs-lookup"><span data-stu-id="a9a35-190">AP2003</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-191">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-191">Description</span></span>
<span data-ttu-id="a9a35-192">Använda asynkrona Service Bus-metoder för att förbättra prestanda med asynkrona meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a9a35-192">Use asynchronous Service Bus methods to improve performance with brokered messaging.</span></span>

<span data-ttu-id="a9a35-193">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-193">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-194">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-194">Reason</span></span>
<span data-ttu-id="a9a35-195">När du använder asynkrona metoder kan programmet programmet samtidighet eftersom körs varje anrop inte blockerar huvudtråden.</span><span class="sxs-lookup"><span data-stu-id="a9a35-195">Using asynchronous methods enables application program concurrency because executing each call doesn’t block the main thread.</span></span> <span data-ttu-id="a9a35-196">När du använder Service Bus messaging metoder, utför en åtgärd (skicka, ta emot, ta bort, etc.) tar tid.</span><span class="sxs-lookup"><span data-stu-id="a9a35-196">When using Service Bus messaging methods, performing an operation (send, receive, delete, etc.) takes time.</span></span> <span data-ttu-id="a9a35-197">Den här gången innefattar bearbetning av åtgärden av tjänsten Service Bus förutom svarstid för begäran och svar.</span><span class="sxs-lookup"><span data-stu-id="a9a35-197">This time includes the processing of the operation by the Service Bus service in addition to the latency of the request and the reply.</span></span> <span data-ttu-id="a9a35-198">Om du vill öka antalet åtgärder per tid måste operations köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="a9a35-198">To increase the number of operations per time, operations must execute concurrently.</span></span> <span data-ttu-id="a9a35-199">Mer information finns i [bästa praxis för prestanda förbättringar med Service Bus asynkrona meddelanden](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-199">For more information please refer to [Best Practices for Performance Improvements Using Service Bus Brokered Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="a9a35-200">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-200">Solution</span></span>
<span data-ttu-id="a9a35-201">Se [QueueClient klass (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) information om hur du använder den rekommenderade asynkrona metoden.</span><span class="sxs-lookup"><span data-stu-id="a9a35-201">See [QueueClient Class (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) for information about how to use the recommended asynchronous method.</span></span>

<span data-ttu-id="a9a35-202">För att förbättra prestanda för Azure meddelandeinfrastrukturen finns designmönstret [asynkrona meddelanden introduktion](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-202">To improve the performance of the Azure messaging infrastructure, see the design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

## <a name="consider-partitioning-service-bus-queues-and-topics"></a><span data-ttu-id="a9a35-203">Överväg att partitionering Service Bus-köer och ämnen</span><span class="sxs-lookup"><span data-stu-id="a9a35-203">Consider partitioning Service Bus queues and topics</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-204">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-204">ID</span></span>
<span data-ttu-id="a9a35-205">AP2004</span><span class="sxs-lookup"><span data-stu-id="a9a35-205">AP2004</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-206">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-206">Description</span></span>
<span data-ttu-id="a9a35-207">Partitionen Service Bus-köer och ämnen för bättre prestanda med Service Bus-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a9a35-207">Partition Service Bus queues and topics for better performance with Service Bus messaging.</span></span>

<span data-ttu-id="a9a35-208">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-208">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-209">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-209">Reason</span></span>
<span data-ttu-id="a9a35-210">Partitionering Service Bus-köer och ämnen ökar prestanda genomflöde och tjänsten tillgänglighet eftersom det totala genomflödet i en partitionerad kö eller ett ämne begränsas inte längre av prestanda i ett enda meddelande broker eller meddelandearkiv.</span><span class="sxs-lookup"><span data-stu-id="a9a35-210">Partitioning Service Bus queues and topics increases performance throughput and service availability because the overall throughput of a partitioned queue or topic is no longer limited by the performance of a single message broker or messaging store.</span></span> <span data-ttu-id="a9a35-211">Dessutom kan spärra ett tillfälligt avbrott i ett meddelandearkiv inte en partitionerad kö eller ett ämne.</span><span class="sxs-lookup"><span data-stu-id="a9a35-211">In addition, a temporary outage of a messaging store doesn’t make a partitioned queue or topic unavailable.</span></span> <span data-ttu-id="a9a35-212">Mer information finns i [partitionering Meddelandeentiteter](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-212">For more information, see [Partitioning Messaging Entities](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="a9a35-213">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-213">Solution</span></span>
<span data-ttu-id="a9a35-214">Följande kodavsnitt visar hur du partitionera meddelandeentiteter.</span><span class="sxs-lookup"><span data-stu-id="a9a35-214">The following code snippet shows how to partition messaging entities.</span></span>

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

<span data-ttu-id="a9a35-215">Mer information finns i [partitionerad Service Bus-köer och ämnen | Microsoft Azure-blogg](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) och checka ut den [Microsoft Azure Service Bus partitionerad-kö](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) exempel.</span><span class="sxs-lookup"><span data-stu-id="a9a35-215">For more information, see [Partitioned Service Bus Queues and Topics | Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) and check out the [Microsoft Azure Service Bus Partitioned Queue](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) sample.</span></span>

## <a name="do-not-set-sharedaccessstarttime"></a><span data-ttu-id="a9a35-216">Ange inte SharedAccessStartTime</span><span class="sxs-lookup"><span data-stu-id="a9a35-216">Do not set SharedAccessStartTime</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-217">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-217">ID</span></span>
<span data-ttu-id="a9a35-218">AP3001</span><span class="sxs-lookup"><span data-stu-id="a9a35-218">AP3001</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-219">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-219">Description</span></span>
<span data-ttu-id="a9a35-220">Du bör undvika att använda SharedAccessStartTimeset till aktuell tid för att starta direkt princip för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a9a35-220">You should avoid using SharedAccessStartTimeset to the current time to immediately start the Shared Access policy.</span></span> <span data-ttu-id="a9a35-221">Du behöver bara använda den här egenskapen om du vill starta princip för delad åtkomst vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="a9a35-221">You only need to set this property if you want to start the Shared Access policy at a later time.</span></span>

<span data-ttu-id="a9a35-222">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-222">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-223">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-223">Reason</span></span>
<span data-ttu-id="a9a35-224">Synkronisering av datorklocka medför en lätt tidsskillnaden mellan datacenter.</span><span class="sxs-lookup"><span data-stu-id="a9a35-224">Clock synchronization causes a slight time difference among datacenters.</span></span> <span data-ttu-id="a9a35-225">Till exempel skulle du logiskt tror att starttiden för lagring SAS-principen som den aktuella tiden med DateTime.Now eller orsakar en liknande metoden SAS-principen ska gälla omedelbart.</span><span class="sxs-lookup"><span data-stu-id="a9a35-225">For example, you would logically think setting the start time of a storage SAS policy as the current time by using DateTime.Now or a similar method will cause the SAS policy to take effect immediately.</span></span> <span data-ttu-id="a9a35-226">Mindre tidsskillnaderna mellan Datacenter kan emellertid orsaka problem med den här eftersom datacenter ibland kan vara lite senare än starttiden, medan andra i den.</span><span class="sxs-lookup"><span data-stu-id="a9a35-226">However, the slight time differences between datacenters can cause problems with this since some datacenter times might be slightly later than the start time, while others ahead of it.</span></span> <span data-ttu-id="a9a35-227">Därför SAS-principen kan gälla snabbt (eller även omedelbart) om principen livstid är för kort.</span><span class="sxs-lookup"><span data-stu-id="a9a35-227">As a result, the SAS policy can expire quickly (or even immediately) if the policy lifetime is set too short.</span></span>

<span data-ttu-id="a9a35-228">Mer information om med signatur för delad åtkomst på Azure storage finns [introduktion till tabellen SAS (signatur för delad åtkomst), Queue SAS och uppdaterar till Blob SAS - Microsoft Azure Storage-teamets blogg - platsen Start - MSDN-bloggarna](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-228">For more guidance on using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update to Blob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="a9a35-229">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-229">Solution</span></span>
<span data-ttu-id="a9a35-230">Ta bort den instruktion som anger starttiden för princip för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a9a35-230">Remove the statement that sets the start time of the shared access policy.</span></span> <span data-ttu-id="a9a35-231">Verktyget Azure koden innehåller en korrigering för problemet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-231">The Azure Code Analysis tool provides a fix for this issue.</span></span> <span data-ttu-id="a9a35-232">Mer information om säkerhetshantering finns designmönstret [Valet nyckeln mönster](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-232">For more information on security management, please see the design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="a9a35-233">Följande kodavsnitt visar koden korrigering för problemet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-233">The following code snippet demonstrates the code fix for this issue.</span></span>

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a><span data-ttu-id="a9a35-234">Delad åtkomstprincip förfallotiden måste vara fler än fem minuter</span><span class="sxs-lookup"><span data-stu-id="a9a35-234">Shared Access Policy expiry time must be more than five minutes</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-235">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-235">ID</span></span>
<span data-ttu-id="a9a35-236">AP3002</span><span class="sxs-lookup"><span data-stu-id="a9a35-236">AP3002</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-237">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-237">Description</span></span>
<span data-ttu-id="a9a35-238">Det kan finnas lika mycket som en fem minuters skillnad i klockor mellan Datacenter på olika platser på grund av ett tillstånd som kallas ”klockavvikelser”.</span><span class="sxs-lookup"><span data-stu-id="a9a35-238">There can be as much as a five minute difference in clocks among datacenters at different locations due to a condition known as "clock skew."</span></span> <span data-ttu-id="a9a35-239">För att förhindra SAS ange principen token upphör att gälla tidigare än planerat, förfallotiden vara mer än fem minuter.</span><span class="sxs-lookup"><span data-stu-id="a9a35-239">To prevent the SAS policy token from expiring earlier than planned, set the expiry time to be more than five minutes.</span></span>

<span data-ttu-id="a9a35-240">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-240">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-241">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-241">Reason</span></span>
<span data-ttu-id="a9a35-242">Datacenter på olika platser över hela världen synkronisera genom en signal klockan.</span><span class="sxs-lookup"><span data-stu-id="a9a35-242">Datacenters at different locations around the world synchronize by a clock signal.</span></span> <span data-ttu-id="a9a35-243">Eftersom det tar tid för klockan signal överföras till olika platser, kan det finnas en gång avvikelse mellan Datacenter på olika geografiska platser trots allt är synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="a9a35-243">Because it takes time for clock signal to travel to different locations, there can be a time variance between datacenters at different geographical locations although everything is supposedly synchronized.</span></span> <span data-ttu-id="a9a35-244">Den här tidsskillnaden kan påverka Shared Access princip start tid och förfallodatum för intervallet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-244">This time difference can affect the Shared Access policy start time and expiration interval.</span></span> <span data-ttu-id="a9a35-245">Därför för att säkerställa princip för delad åtkomst träder i kraft omedelbart, inte ange starttiden.</span><span class="sxs-lookup"><span data-stu-id="a9a35-245">Therefore, to ensure Shared Access policy takes effect immediately, don’t specify the start time.</span></span> <span data-ttu-id="a9a35-246">Kontrollera dessutom att förfallotiden är mer än 5 minuter för att förhindra tidig timeout.</span><span class="sxs-lookup"><span data-stu-id="a9a35-246">In addition, make sure the expiration time is more than 5 minutes to prevent early timeout.</span></span>

<span data-ttu-id="a9a35-247">Mer information om hur du använder signatur för delad åtkomst på Azure storage finns [introduktion till tabellen SAS (signatur för delad åtkomst), Queue SAS och uppdaterar till Blob SAS - Microsoft Azure Storage-teamets blogg - platsen Start - MSDN-bloggarna](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-247">For more information about using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update to Blob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="a9a35-248">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-248">Solution</span></span>
<span data-ttu-id="a9a35-249">Mer information om säkerhetshantering finns designmönstret [Valet nyckeln mönster](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-249">For more information on security management, see the design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="a9a35-250">Följande är ett exempel på inte att ange en starttid för delad åtkomst princip.</span><span class="sxs-lookup"><span data-stu-id="a9a35-250">The following is an example of not specifying a Shared Access policy start time.</span></span>

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="a9a35-251">Följande är ett exempel på att ange en starttid för delad åtkomst principen med princip upphör att gälla mer än fem minuter.</span><span class="sxs-lookup"><span data-stu-id="a9a35-251">The following is an example of specifying a Shared Access policy start time with a policy expiration duration greater than five minutes.</span></span>

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="a9a35-252">Mer information finns i [skapar och använder en signatur för delad åtkomst](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-252">For more information, see [Create and Use a Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span></span>

## <a name="use-cloudconfigurationmanager"></a><span data-ttu-id="a9a35-253">Använd CloudConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="a9a35-253">Use CloudConfigurationManager</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-254">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-254">ID</span></span>
<span data-ttu-id="a9a35-255">AP4000</span><span class="sxs-lookup"><span data-stu-id="a9a35-255">AP4000</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-256">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-256">Description</span></span>
<span data-ttu-id="a9a35-257">Med hjälp av den [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) klassen för projekt som Azure-webbplats och Azure mobile services inte introducerar runtime problem.</span><span class="sxs-lookup"><span data-stu-id="a9a35-257">Using the [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) class for projects such as Azure Website and Azure mobile services won't introduce runtime issues.</span></span> <span data-ttu-id="a9a35-258">Som bästa praxis, men det är en bra idé att använda molnet[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) som ett enhetligt sätt att hantera konfigurationer för alla program i Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-258">As a best practice, however, it's a good idea to use Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) as a unified way of managing configurations for all Azure Cloud applications.</span></span>

<span data-ttu-id="a9a35-259">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-259">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-260">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-260">Reason</span></span>
<span data-ttu-id="a9a35-261">CloudConfigurationManager läser konfigurationsfilen som är lämpliga för program-miljö.</span><span class="sxs-lookup"><span data-stu-id="a9a35-261">CloudConfigurationManager reads the configuration file appropriate to the application environment.</span></span>

[<span data-ttu-id="a9a35-262">CloudConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="a9a35-262">CloudConfigurationManager</span></span>](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a><span data-ttu-id="a9a35-263">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-263">Solution</span></span>
<span data-ttu-id="a9a35-264">Refactor din kod för att använda den [CloudConfigurationManager-klassen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-264">Refactor your code to use the [CloudConfigurationManager Class](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span></span> <span data-ttu-id="a9a35-265">En kod korrigering för problemet tillhandahålls av verktyget Azure kod.</span><span class="sxs-lookup"><span data-stu-id="a9a35-265">A code fix for this issue is provided by the Azure Code Analysis tool.</span></span>

<span data-ttu-id="a9a35-266">Följande kodavsnitt visar koden korrigering för problemet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-266">The following code snippet demonstrates the code fix for this issue.</span></span> <span data-ttu-id="a9a35-267">Ersätt</span><span class="sxs-lookup"><span data-stu-id="a9a35-267">Replace</span></span>

`var settings = ConfigurationManager.AppSettings["mySettings"];`

<span data-ttu-id="a9a35-268">med</span><span class="sxs-lookup"><span data-stu-id="a9a35-268">with</span></span>

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

<span data-ttu-id="a9a35-269">Här är ett exempel på hur du lagrar Konfigurationsinställningen i App.config eller Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="a9a35-269">Here's an example of how to store the configuration setting in a App.config or Web.config file.</span></span> <span data-ttu-id="a9a35-270">Lägga till inställningarna i avsnittet AppSettings i konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="a9a35-270">Add the settings to the appSettings section of the configuration file.</span></span> <span data-ttu-id="a9a35-271">Följande är Web.config-filen för det förra kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-271">The following is the Web.config file for the previous code example.</span></span>

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a><span data-ttu-id="a9a35-272">Undvik att använda hårdkodade anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="a9a35-272">Avoid using hard-coded connection strings</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-273">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-273">ID</span></span>
<span data-ttu-id="a9a35-274">AP4001</span><span class="sxs-lookup"><span data-stu-id="a9a35-274">AP4001</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-275">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-275">Description</span></span>
<span data-ttu-id="a9a35-276">Om du använder hårdkodade anslutningssträngar och du måste uppdatera dem senare, måste du göra ändringar i källkoden och kompilerar om programmet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-276">If you use hard-coded connection strings and you need to update them later, you’ll have to make changes to your source code and recompile the application.</span></span> <span data-ttu-id="a9a35-277">Men om du sparar din anslutningssträngar i en konfigurationsfil, kan du ändra dem senare genom att helt enkelt uppdatera konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="a9a35-277">However, if you store your connection strings in a configuration file, you can change them later by simply updating the configuration file.</span></span>

<span data-ttu-id="a9a35-278">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-278">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-279">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-279">Reason</span></span>
<span data-ttu-id="a9a35-280">Hårdkoda anslutningssträngar är felaktigt eftersom problem när anslutningssträngar behöver snabbt ska kunna ändras.</span><span class="sxs-lookup"><span data-stu-id="a9a35-280">Hard-coding connection strings is a bad practice because it introduces problems when connection strings need to be changed quickly.</span></span> <span data-ttu-id="a9a35-281">Dessutom om projektet måste checkas in till källkontroll, införa hårdkodade anslutningssträngar säkerhetsrisker eftersom strängarna som kan visas i källkoden.</span><span class="sxs-lookup"><span data-stu-id="a9a35-281">In addition, if the project needs to be checked in to source control, hard-coded connection strings introduce security vulnerabilities since the strings can be viewed in the source code.</span></span>

### <a name="solution"></a><span data-ttu-id="a9a35-282">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-282">Solution</span></span>
<span data-ttu-id="a9a35-283">Lagrar anslutningssträngar i configuration-filer eller miljöer i Azure.</span><span class="sxs-lookup"><span data-stu-id="a9a35-283">Store connection strings in the configuration files or Azure environments.</span></span>

* <span data-ttu-id="a9a35-284">Använda app.config för att lagra strängen anslutningsinställningar för fristående program.</span><span class="sxs-lookup"><span data-stu-id="a9a35-284">For standalone applications, use app.config to store connection string settings.</span></span>
* <span data-ttu-id="a9a35-285">Använda web.config för att lagra anslutningssträngar för IIS-värdbaserade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a9a35-285">For IIS-hosted web applications, use web.config to store connection strings.</span></span>
* <span data-ttu-id="a9a35-286">Använda configuration.json för att lagra anslutningssträngar för ASP.NET-program vNext.</span><span class="sxs-lookup"><span data-stu-id="a9a35-286">For ASP.NET vNext applications, use configuration.json to store connection strings.</span></span>

<span data-ttu-id="a9a35-287">Information om hur du använder konfigurationer filer, till exempel web.config eller app.config finns [riktlinjer för konfiguration av ASP.NET Web](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span><span class="sxs-lookup"><span data-stu-id="a9a35-287">For information on using configurations files such as web.config or app.config, see [ASP.NET Web Configuration Guidelines](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span></span> <span data-ttu-id="a9a35-288">Mer information om hur Azure miljö variabler arbete finns [Azure webbplatser: hur programmet strängar och anslutningen strängar fungerar](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span><span class="sxs-lookup"><span data-stu-id="a9a35-288">For information on how Azure environment variables work, see [Azure Web Sites: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span></span> <span data-ttu-id="a9a35-289">Mer information om lagra anslutningssträngen i källkontrollen finns [undvika att sätta känslig information, till exempel anslutningssträngar i filer som lagras i koden källdatabasen](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span><span class="sxs-lookup"><span data-stu-id="a9a35-289">For information on storing connection string in source control, see [avoid putting sensitive information such as connection strings in files that are stored in source code repository](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span></span>

## <a name="use-diagnostics-configuration-file"></a><span data-ttu-id="a9a35-290">Använd diagnostik-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="a9a35-290">Use diagnostics configuration file</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-291">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-291">ID</span></span>
<span data-ttu-id="a9a35-292">AP5000</span><span class="sxs-lookup"><span data-stu-id="a9a35-292">AP5000</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-293">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-293">Description</span></span>
<span data-ttu-id="a9a35-294">I stället för att konfigurera diagnostikinställningar i din kod som genom att använda Microsoft.WindowsAzure.Diagnostics programmering API, bör du konfigurera inställningarna för webbprogramdiagnostik i filen diagnostics.wadcfg.</span><span class="sxs-lookup"><span data-stu-id="a9a35-294">Instead of configuring diagnostics settings in your code such as by using the Microsoft.WindowsAzure.Diagnostics programming API, you should configure diagnostics settings in the diagnostics.wadcfg file.</span></span> <span data-ttu-id="a9a35-295">(Eller diagnostics.wadcfgx om du använder Azure SDK 2.5).</span><span class="sxs-lookup"><span data-stu-id="a9a35-295">(Or, diagnostics.wadcfgx if you use Azure SDK 2.5).</span></span> <span data-ttu-id="a9a35-296">Du kan ändra inställningarna för webbprogramdiagnostik utan att kompilera om din kod genom att göra.</span><span class="sxs-lookup"><span data-stu-id="a9a35-296">By doing this, you can change diagnostics settings without having to recompile your code.</span></span>

<span data-ttu-id="a9a35-297">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-297">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-298">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-298">Reason</span></span>
<span data-ttu-id="a9a35-299">Innan Azure SDK 2,5 (som använder Azure diagnostics 1.3), Azure Diagnostics (BOMULLSTUSS) kan konfigureras med hjälp av flera olika metoder: lägger till den konfiguration blob i lagring, med hjälp av tvingande koden, deklarativa konfigurationen eller standardkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a9a35-299">Before Azure SDK 2.5 (which uses Azure diagnostics 1.3), Azure Diagnostics (WAD) could be configured by using several different methods: adding it to the configuration blob in storage, by using imperative code, declarative configuration, or the default configuration.</span></span> <span data-ttu-id="a9a35-300">Det bästa sättet att konfigurera diagnostik är dock att använda en XML-konfigurationsfil (diagnostics.wadcfg eller diagnositcs.wadcfgx för SDK-2.5 och senare) i projektet för konsolprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-300">However, the preferred way to configure diagnostics is to use an XML configuration file (diagnostics.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later) in the application project.</span></span> <span data-ttu-id="a9a35-301">I den här metoden kan diagnostics.wadcfg filen helt definierar konfigurationen och uppdateras och omdistribueras när du vill.</span><span class="sxs-lookup"><span data-stu-id="a9a35-301">In this approach, the diagnostics.wadcfg file completely defines the configuration and can be updated and redeployed at will.</span></span> <span data-ttu-id="a9a35-302">Blandning av användningen av konfigurationsfilen diagnostics.wadcfg med programmering för konfigurationer med hjälp av den [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)eller [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)klasser kan leda till förvirring.</span><span class="sxs-lookup"><span data-stu-id="a9a35-302">Mixing the use of the diagnostics.wadcfg configuration file with the programmatic methods of setting configurations by using the [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)or [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)classes can lead to confusion.</span></span> <span data-ttu-id="a9a35-303">Se [initieras eller ändra Azure Diagnostics konfiguration](https://msdn.microsoft.com/library/azure/hh411537.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a9a35-303">See [Initialize or Change Azure Diagnostics Configuration](https://msdn.microsoft.com/library/azure/hh411537.aspx) for more information.</span></span>

<span data-ttu-id="a9a35-304">Från och med BOMULLSTUSS 1.3 (ingår i Azure SDK 2.5) kan den inte längre att använda koden för att konfigurera diagnostik.</span><span class="sxs-lookup"><span data-stu-id="a9a35-304">Beginning with WAD 1.3 (included with Azure SDK 2.5), it’s no longer possible to use code to configure diagnostics.</span></span> <span data-ttu-id="a9a35-305">Därför kan du bara ange konfigurationen när du tillämpar eller uppdaterar tillägget diagnostik.</span><span class="sxs-lookup"><span data-stu-id="a9a35-305">As a result, you can only provide the configuration when applying or updating the diagnostics extension.</span></span>

### <a name="solution"></a><span data-ttu-id="a9a35-306">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-306">Solution</span></span>
<span data-ttu-id="a9a35-307">Använd diagnostik configuration designer för att flytta diagnostikinställningar till konfigurationsfilen diagnostik (diagnositcs.wadcfg eller diagnositcs.wadcfgx för SDK-2.5 och senare).</span><span class="sxs-lookup"><span data-stu-id="a9a35-307">Use the diagnostics configuration designer to move diagnostic settings to the diagnostics configuration file (diagnositcs.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later).</span></span> <span data-ttu-id="a9a35-308">Det rekommenderas också att du installerar [Azure SDK 2,5](http://go.microsoft.com/fwlink/?LinkId=513188) och använder funktionen senaste diagnostik.</span><span class="sxs-lookup"><span data-stu-id="a9a35-308">It’s also recommended that you install [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) and use the latest diagnostics feature.</span></span>

1. <span data-ttu-id="a9a35-309">Välj Egenskaper på snabbmenyn för den roll som du vill konfigurera och välj sedan fliken konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a9a35-309">On the shortcut menu for the role that you want to configure, choose Properties, and then choose the Configuration tab.</span></span>
2. <span data-ttu-id="a9a35-310">I den **diagnostik** avsnittet, se till att den **aktivera diagnostik** är markerad.</span><span class="sxs-lookup"><span data-stu-id="a9a35-310">In the **Diagnostics** section, make sure that the **Enable Diagnostics** check box is selected.</span></span>
3. <span data-ttu-id="a9a35-311">Välj den **konfigurera** knappen.</span><span class="sxs-lookup"><span data-stu-id="a9a35-311">Choose the **Configure** button.</span></span>

   ![Åtkomst till alternativet Aktivera diagnostik](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   <span data-ttu-id="a9a35-313">Se [konfigurera diagnostik för virtuella datorer och Azure Cloud Services](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a9a35-313">See [Configuring Diagnostics for Azure Cloud Services and Virtual Machines](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) for more information.</span></span>

## <a name="avoid-declaring-dbcontext-objects-as-static"></a><span data-ttu-id="a9a35-314">Undvik att deklarera DbContext-objekt som statisk</span><span class="sxs-lookup"><span data-stu-id="a9a35-314">Avoid declaring DbContext objects as static</span></span>
### <a name="id"></a><span data-ttu-id="a9a35-315">ID</span><span class="sxs-lookup"><span data-stu-id="a9a35-315">ID</span></span>
<span data-ttu-id="a9a35-316">AP6000</span><span class="sxs-lookup"><span data-stu-id="a9a35-316">AP6000</span></span>

### <a name="description"></a><span data-ttu-id="a9a35-317">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a9a35-317">Description</span></span>
<span data-ttu-id="a9a35-318">Om du vill spara minne Undvik att deklarera DBContext-objekt som statisk.</span><span class="sxs-lookup"><span data-stu-id="a9a35-318">To save memory, avoid declaring DBContext objects as static.</span></span>

<span data-ttu-id="a9a35-319">Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="a9a35-319">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="a9a35-320">Orsak</span><span class="sxs-lookup"><span data-stu-id="a9a35-320">Reason</span></span>
<span data-ttu-id="a9a35-321">DBContext-objekt innehåller resultatet av frågan från varje anrop.</span><span class="sxs-lookup"><span data-stu-id="a9a35-321">DBContext objects hold the query results from each call.</span></span> <span data-ttu-id="a9a35-322">Statiska DBContext-objekt är inte bort förrän programdomänen tas bort.</span><span class="sxs-lookup"><span data-stu-id="a9a35-322">Static DBContext objects are not disposed until the application domain is unloaded.</span></span> <span data-ttu-id="a9a35-323">Därför kan ett statiskt DBContext-objekt förbruka stora mängder minne.</span><span class="sxs-lookup"><span data-stu-id="a9a35-323">Therefore, a static DBContext object can consume large amounts of memory.</span></span>

### <a name="solution"></a><span data-ttu-id="a9a35-324">Lösning</span><span class="sxs-lookup"><span data-stu-id="a9a35-324">Solution</span></span>
<span data-ttu-id="a9a35-325">Deklarera DBContext som en lokal variabel eller icke-statisk instansfält, använda den för en aktivitet och låt den avyttras efter användning.</span><span class="sxs-lookup"><span data-stu-id="a9a35-325">Declare DBContext as a local variable or non-static instance field, use it for a task, and then let it be disposed of after use.</span></span>

<span data-ttu-id="a9a35-326">I följande exempel MVC kontrollantklassen visas hur du använder DBContext-objektet.</span><span class="sxs-lookup"><span data-stu-id="a9a35-326">The following example MVC controller class shows how to use the DBContext object.</span></span>

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
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

## <a name="next-steps"></a><span data-ttu-id="a9a35-327">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9a35-327">Next steps</span></span>
<span data-ttu-id="a9a35-328">Läs mer om hur du optimerar och felsöka Azure apps i [felsöka en webbapp i Azure App Service med Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="a9a35-328">To learn more about optimizing and troubleshooting Azure apps, see [Troubleshoot a web app in Azure App Service using Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>
