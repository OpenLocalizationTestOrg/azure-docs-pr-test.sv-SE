---
title: ".NET-flernivåprogram med hjälp av Azure Service Bus | Microsoft Docs"
description: "En .NET-självstudiekurs som hjälper dig att utveckla en flernivåapp i Azure som använder Service Bus-köer för att kommunicera mellan nivåerna."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1b8608ca-aa5a-4700-b400-54d65b02615c
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 04/11/2017
ms.author: sethm
ms.openlocfilehash: 8b502f5ac5d89801d390a872e7a8b06e094ecbba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a><span data-ttu-id="c19ad-103">.NET-flernivåapp med hjälp av Azure Service Bus-köer</span><span class="sxs-lookup"><span data-stu-id="c19ad-103">.NET multi-tier application using Azure Service Bus queues</span></span>
## <a name="introduction"></a><span data-ttu-id="c19ad-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="c19ad-104">Introduction</span></span>
<span data-ttu-id="c19ad-105">Att utveckla för Microsoft Azure är enkelt tack vare Visual Studio och det kostnadsfria utvecklingsverktyget Azure SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="c19ad-105">Developing for Microsoft Azure is easy using Visual Studio and the free Azure SDK for .NET.</span></span> <span data-ttu-id="c19ad-106">Den här självstudiekursen vägleder dig igenom stegen för att skapa en app som använder flera Azure-resurser som körs i din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="c19ad-106">This tutorial walks you through the steps to create an application that uses multiple Azure resources running in your local environment.</span></span>

<span data-ttu-id="c19ad-107">Du kommer att få lära dig följande:</span><span class="sxs-lookup"><span data-stu-id="c19ad-107">You will learn the following:</span></span>

* <span data-ttu-id="c19ad-108">Hur du förbereder datorn för Azure-utveckling med en enda nedladdning och installation.</span><span class="sxs-lookup"><span data-stu-id="c19ad-108">How to enable your computer for Azure development with a single download and install.</span></span>
* <span data-ttu-id="c19ad-109">Hur du använder Visual Studio för att utveckla för Azure.</span><span class="sxs-lookup"><span data-stu-id="c19ad-109">How to use Visual Studio to develop for Azure.</span></span>
* <span data-ttu-id="c19ad-110">Hur du skapar en flernivåapp i Azure genom att använda webb- och arbetsroller.</span><span class="sxs-lookup"><span data-stu-id="c19ad-110">How to create a multi-tier application in Azure using web and worker roles.</span></span>
* <span data-ttu-id="c19ad-111">Hur du kan kommunicera mellan nivåer med hjälp av Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="c19ad-111">How to communicate between tiers using Service Bus queues.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

<span data-ttu-id="c19ad-112">I den här självstudiekursen kommer du att skapa och köra flernivåappen i en molntjänst för Azure.</span><span class="sxs-lookup"><span data-stu-id="c19ad-112">In this tutorial you'll build and run the multi-tier application in an Azure cloud service.</span></span> <span data-ttu-id="c19ad-113">Klientdelen är en ASP.NET MVC-webbroll och serverdelen en arbetsroll som använder en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="c19ad-113">The front end is an ASP.NET MVC web role and the back end is a worker-role that uses a Service Bus queue.</span></span> <span data-ttu-id="c19ad-114">Du kan skapa samma flernivåapp med klientdelen som ett webbprojekt som distribueras till en Azure-webbplats i stället för till en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="c19ad-114">You can create the same multi-tier application with the front end as a web project, that is deployed to an Azure website instead of a cloud service.</span></span> <span data-ttu-id="c19ad-115">Du kan också prova att gå igenom självstudiekursen [.NET-hybridapp lokalt/i molnet](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md).</span><span class="sxs-lookup"><span data-stu-id="c19ad-115">You can also try out the [.NET on-premises/cloud hybrid application](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) tutorial.</span></span>

<span data-ttu-id="c19ad-116">Följande skärmdump visar den färdiga appen.</span><span class="sxs-lookup"><span data-stu-id="c19ad-116">The following screen shot shows the completed application.</span></span>

![][0]

## <a name="scenario-overview-inter-role-communication"></a><span data-ttu-id="c19ad-117">Scenarioöversikt: kommunikation mellan roller</span><span class="sxs-lookup"><span data-stu-id="c19ad-117">Scenario overview: inter-role communication</span></span>
<span data-ttu-id="c19ad-118">Om du vill skicka in en order för bearbetning måste klientdelens UI-komponent, som kör webbrollen, interagera med den mellannivålogik som kör arbetsrollen.</span><span class="sxs-lookup"><span data-stu-id="c19ad-118">To submit an order for processing, the front-end UI component, running in the web role, must interact with the middle tier logic running in the worker role.</span></span> <span data-ttu-id="c19ad-119">I det här exemplet används en meddelandetjänst via Service Bus för kommunikation mellan nivåerna.</span><span class="sxs-lookup"><span data-stu-id="c19ad-119">This example uses Service Bus messaging for the communication between the tiers.</span></span>

<span data-ttu-id="c19ad-120">Genom att använda en meddelandetjänst mellan webben och mellannivåerna frikopplas de båda komponenterna.</span><span class="sxs-lookup"><span data-stu-id="c19ad-120">Using Service Bus messaging between the web and middle tiers decouples the two components.</span></span> <span data-ttu-id="c19ad-121">Till skillnad från direkt dataöverföring (d.v.s. TCP eller HTTP), ansluter webbnivån inte direkt till mellannivån. Istället skickar den arbetsenheter, som meddelanden, till Service Bus, som lagrar dem på ett tillförlitligt sätt tills mellannivån är klar att använda och bearbeta dem.</span><span class="sxs-lookup"><span data-stu-id="c19ad-121">In contrast to direct messaging (that is, TCP or HTTP), the web tier does not connect to the middle tier directly; instead it pushes units of work, as messages, into Service Bus, which reliably retains them until the middle tier is ready to consume and process them.</span></span>

<span data-ttu-id="c19ad-122">Service Bus innehåller två entiteter för att stödja den asynkrona meddelandetjänsten: köer och ämnen.</span><span class="sxs-lookup"><span data-stu-id="c19ad-122">Service Bus provides two entities to support brokered messaging: queues and topics.</span></span> <span data-ttu-id="c19ad-123">När man använder köer förbrukas varje meddelande som skickas till kön av en enda mottagare.</span><span class="sxs-lookup"><span data-stu-id="c19ad-123">With queues, each message sent to the queue is consumed by a single receiver.</span></span> <span data-ttu-id="c19ad-124">Ämnena stödjer det mönster för publicera/prenumerera där varje publicerat meddelande görs tillgängligt för en prenumeration som har registrerats med ämnet.</span><span class="sxs-lookup"><span data-stu-id="c19ad-124">Topics support the publish/subscribe pattern in which each published message is made available to a subscription registered with the topic.</span></span> <span data-ttu-id="c19ad-125">Varje prenumeration underhåller logiskt nog sin egen meddelandekö.</span><span class="sxs-lookup"><span data-stu-id="c19ad-125">Each subscription logically maintains its own queue of messages.</span></span> <span data-ttu-id="c19ad-126">Prenumerationer kan också konfigureras med filterregler som begränsar den uppsättning av meddelanden som skickas vidare till prenumerationskön till dem som matchar filtret.</span><span class="sxs-lookup"><span data-stu-id="c19ad-126">Subscriptions can also be configured with filter rules that restrict the set of messages passed to the subscription queue to those that match the filter.</span></span> <span data-ttu-id="c19ad-127">I följande exempel används Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="c19ad-127">The following example uses Service Bus queues.</span></span>

![][1]

<span data-ttu-id="c19ad-128">Denna kommunikationsmekanism har flera fördelar jämfört med funktioner för direkta meddelanden:</span><span class="sxs-lookup"><span data-stu-id="c19ad-128">This communication mechanism has several advantages over direct messaging:</span></span>

* <span data-ttu-id="c19ad-129">**Tidsbestämd frikoppling.**</span><span class="sxs-lookup"><span data-stu-id="c19ad-129">**Temporal decoupling.**</span></span> <span data-ttu-id="c19ad-130">Med mönstret för asynkrona meddelanden behöver producenter och konsumenter inte vara online samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c19ad-130">With the asynchronous messaging pattern, producers and consumers need not be online at the same time.</span></span> <span data-ttu-id="c19ad-131">Service Bus lagrar meddelanden på ett säkert sätt tills den konsumerande parten är redo att ta emot dem.</span><span class="sxs-lookup"><span data-stu-id="c19ad-131">Service Bus reliably stores messages until the consuming party is ready to receive them.</span></span> <span data-ttu-id="c19ad-132">Detta gör att komponenterna i den distribuerade appen kan frikopplas, antingen frivilligt, till exempel för underhåll, eller på grund av en komponentkrasch, utan att detta påverkar hela systemet.</span><span class="sxs-lookup"><span data-stu-id="c19ad-132">This enables the components of the distributed application to be disconnected, either voluntarily, for example, for maintenance, or due to a component crash, without impacting the system as a whole.</span></span> <span data-ttu-id="c19ad-133">Dessutom behöver den konsumerande appen endast kopplas upp och vara online under vissa tider på dagen.</span><span class="sxs-lookup"><span data-stu-id="c19ad-133">Furthermore, the consuming application might only need to come online during certain times of the day.</span></span>
* <span data-ttu-id="c19ad-134">**Belastningsutjämning.**</span><span class="sxs-lookup"><span data-stu-id="c19ad-134">**Load leveling.**</span></span> <span data-ttu-id="c19ad-135">I många program varierar systembelastningen beroende på tidpunkten, medan den bearbetningstid som krävs för varje arbetsenhet vanligtvis är konstant.</span><span class="sxs-lookup"><span data-stu-id="c19ad-135">In many applications, system load varies over time, while the processing time required for each unit of work is typically constant.</span></span> <span data-ttu-id="c19ad-136">Medlingen mellan meddelandeproducenter och -konsumenter med hjälp av en kö innebär att den konsumerande appen (arbetaren) endast måste konfigureras för att kunna hantera en genomsnittlig belastning i stället för mycket hög belastning vid vissa tider.</span><span class="sxs-lookup"><span data-stu-id="c19ad-136">Intermediating message producers and consumers with a queue means that the consuming application (the worker) only needs to be provisioned to accommodate average load rather than peak load.</span></span> <span data-ttu-id="c19ad-137">Köns djup växer och dras samman allt eftersom den inkommande belastningen varierar.</span><span class="sxs-lookup"><span data-stu-id="c19ad-137">The depth of the queue grows and contracts as the incoming load varies.</span></span> <span data-ttu-id="c19ad-138">Detta sparar direkt pengar eftersom den mängd infrastruktur som krävs för att underhålla programbelastningen blir mindre.</span><span class="sxs-lookup"><span data-stu-id="c19ad-138">This directly saves money in terms of the amount of infrastructure required to service the application load.</span></span>
* <span data-ttu-id="c19ad-139">**Belastningsutjämning.**</span><span class="sxs-lookup"><span data-stu-id="c19ad-139">**Load balancing.**</span></span> <span data-ttu-id="c19ad-140">Allt eftersom belastningen ökar kan fler arbetsprocesser läggas till för att läsa från kön.</span><span class="sxs-lookup"><span data-stu-id="c19ad-140">As load increases, more worker processes can be added to read from the queue.</span></span> <span data-ttu-id="c19ad-141">Varje meddelande bearbetas bara av en av arbetsprocesserna.</span><span class="sxs-lookup"><span data-stu-id="c19ad-141">Each message is processed by only one of the worker processes.</span></span> <span data-ttu-id="c19ad-142">Dessutom gör den här pull-baserade belastningsbalanseringen att du får en optimal användning av arbetsdatorerna. Detta gäller även om arbetsdatorerna har olika processorkraft: de kommer att hämta meddelanden med den hastighet som var och en av dem klarar av.</span><span class="sxs-lookup"><span data-stu-id="c19ad-142">Furthermore, this pull-based load balancing enables optimal use of the worker machines even if the worker machines differ in terms of processing power, as they will pull messages at their own maximum rate.</span></span> <span data-ttu-id="c19ad-143">Det här mönstret kallas ofta för ett *konkurrerande konsument*-mönster.</span><span class="sxs-lookup"><span data-stu-id="c19ad-143">This pattern is often termed the *competing consumer* pattern.</span></span>
  
  ![][2]

<span data-ttu-id="c19ad-144">I följande avsnitt pratar vi om den kod som implementerar denna arkitektur.</span><span class="sxs-lookup"><span data-stu-id="c19ad-144">The following sections discuss the code that implements this architecture.</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="c19ad-145">Konfigurera utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="c19ad-145">Set up the development environment</span></span>
<span data-ttu-id="c19ad-146">Innan du kan börja utveckla Azure-program måste du skaffa de verktyg som krävs och ställa in din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c19ad-146">Before you can begin developing Azure applications, get the tools and set up your development environment.</span></span>

1. <span data-ttu-id="c19ad-147">Installera Azure SDK för .NET från [hämtningssidan](https://azure.microsoft.com/downloads/) för SDK.</span><span class="sxs-lookup"><span data-stu-id="c19ad-147">Install the Azure SDK for .NET from the SDK [downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="c19ad-148">Klicka på den version av [Visual Studio](http://www.visualstudio.com) du använder i kolumnen **.NET**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-148">In the **.NET** column, click the version of [Visual Studio](http://www.visualstudio.com) you are using.</span></span> <span data-ttu-id="c19ad-149">Stegen i den här handledningen använder Visual Studio 2015, men det går lika bra med Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="c19ad-149">The steps in this tutorial use Visual Studio 2015, but they also work with Visual Studio 2017.</span></span>
3. <span data-ttu-id="c19ad-150">När du uppmanas att köra eller spara installationsprogrammet, klickar du på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-150">When prompted to run or save the installer, click **Run**.</span></span>
4. <span data-ttu-id="c19ad-151">I **Installationsprogram för webbplattform** klickar du på **Installera** och fortsätter med installationen.</span><span class="sxs-lookup"><span data-stu-id="c19ad-151">In the **Web Platform Installer**, click **Install** and proceed with the installation.</span></span>
5. <span data-ttu-id="c19ad-152">När installationen är klar har du allt som behövs för att börja utveckla appen.</span><span class="sxs-lookup"><span data-stu-id="c19ad-152">Once the installation is complete, you will have everything necessary to start to develop the app.</span></span> <span data-ttu-id="c19ad-153">SDK inkluderar verktyg som låter dig utveckla Azure-program i Visual Studio på ett enkelt sätt.</span><span class="sxs-lookup"><span data-stu-id="c19ad-153">The SDK includes tools that let you easily develop Azure applications in Visual Studio.</span></span>

## <a name="create-a-namespace"></a><span data-ttu-id="c19ad-154">Skapa ett namnområde</span><span class="sxs-lookup"><span data-stu-id="c19ad-154">Create a namespace</span></span>
<span data-ttu-id="c19ad-155">Nästa steg är att skapa ett namnområde för tjänsten och få en nyckel till signatur för delad åtkomst (SAS).</span><span class="sxs-lookup"><span data-stu-id="c19ad-155">The next step is to create a service namespace, and obtain a Shared Access Signature (SAS) key.</span></span> <span data-ttu-id="c19ad-156">Ett namnområde ger en appgräns för varje app som exponeras via Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c19ad-156">A namespace provides an application boundary for each application exposed through Service Bus.</span></span> <span data-ttu-id="c19ad-157">SAS-nyckeln genereras av systemet när ett namnområde har skapats.</span><span class="sxs-lookup"><span data-stu-id="c19ad-157">A SAS key is generated by the system when a namespace is created.</span></span> <span data-ttu-id="c19ad-158">Kombinationen av namnområdet och SAS-nyckeln ger referensen för Service Bus som används för att tillåta åtkomst till ett program.</span><span class="sxs-lookup"><span data-stu-id="c19ad-158">The combination of namespace and SAS key provides the credentials for Service Bus to authenticate access to an application.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a><span data-ttu-id="c19ad-159">Skapa en webbroll</span><span class="sxs-lookup"><span data-stu-id="c19ad-159">Create a web role</span></span>
<span data-ttu-id="c19ad-160">I det här avsnittet ska du skapa klientdelen för din app.</span><span class="sxs-lookup"><span data-stu-id="c19ad-160">In this section, you build the front end of your application.</span></span> <span data-ttu-id="c19ad-161">Först skapar du de sidor som din app visar.</span><span class="sxs-lookup"><span data-stu-id="c19ad-161">First, you create the pages that your application displays.</span></span>
<span data-ttu-id="c19ad-162">Efter det lägger du till kod som skickar objekt till en Service Bus-kö och visar statusinformation om denna kö.</span><span class="sxs-lookup"><span data-stu-id="c19ad-162">After that, add code that submits items to a Service Bus queue and displays status information about the queue.</span></span>

### <a name="create-the-project"></a><span data-ttu-id="c19ad-163">Skapa projektet</span><span class="sxs-lookup"><span data-stu-id="c19ad-163">Create the project</span></span>
1. <span data-ttu-id="c19ad-164">Du startar Visual Studio med administratörsbehörighet genom att högerklicka på programikonen för **Visual Studio** och sedan klicka på **Kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-164">Using administrator privileges, start Visual Studio: right-click the **Visual Studio** program icon, and then click **Run as administrator**.</span></span> <span data-ttu-id="c19ad-165">Azure Compute Emulator, som diskuteras senare i den här artikel, kräver att Visual Studio startas med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="c19ad-165">The Azure compute emulator, discussed later in this article, requires that Visual Studio be started with administrator privileges.</span></span>
   
   <span data-ttu-id="c19ad-166">I Visual Studio klickar du på **Nytt** i menyn **Arkiv** och sedan på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-166">In Visual Studio, on the **File** menu, click **New**, and then click **Project**.</span></span>
2. <span data-ttu-id="c19ad-167">Från **Installerade mallar**, under **Visual C#**, klickar du på **Moln** och sedan på **Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-167">From **Installed Templates**, under **Visual C#**, click **Cloud** and then click **Azure Cloud Service**.</span></span> <span data-ttu-id="c19ad-168">Ge projektet följande namn: **MultiTierApp**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-168">Name the project **MultiTierApp**.</span></span> <span data-ttu-id="c19ad-169">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-169">Then click **OK**.</span></span>
   
   ![][9]
3. <span data-ttu-id="c19ad-170">Dubbelklicka på **ASP.NET Web Role** från rollerna **.NET Framework 4.5**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-170">From **.NET Framework 4.5** roles, double-click **ASP.NET Web Role**.</span></span>
   
   ![][10]
4. <span data-ttu-id="c19ad-171">Hovra över **WebRole1** under **Azure Cloud Service Solution** och klicka på pennikonen. Byt sedan namn på webbrollen till **FrontendWebRole**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-171">Hover over **WebRole1** under **Azure Cloud Service solution**, click the pencil icon, and rename the web role to **FrontendWebRole**.</span></span> <span data-ttu-id="c19ad-172">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-172">Then click **OK**.</span></span> <span data-ttu-id="c19ad-173">(Kontrollera att du anger ”Frontend” med ett litet ”e”, det vill säga, inte ”FrontEnd”).</span><span class="sxs-lookup"><span data-stu-id="c19ad-173">(Make sure you enter "Frontend" with a lower-case 'e,' not "FrontEnd".)</span></span>
   
   ![][11]
5. <span data-ttu-id="c19ad-174">Från dialogrutan **Nytt ASP.NET-projekt** klickar du på **MVC** i listan **Välj en mall**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-174">From the **New ASP.NET Project** dialog box, in the **Select a template** list, click **MVC**.</span></span>
   
   ![][12]
6. <span data-ttu-id="c19ad-175">Medan du fortfarande är kvar i dialogrutan **Nytt ASP.NET-projekt**, klickar du på knappen **Ändra autentisering**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-175">Still in the **New ASP.NET Project** dialog box, click the **Change Authentication** button.</span></span> <span data-ttu-id="c19ad-176">I dialogrutan **Ändra autentisering** klickar du på **Ingen autentisering** och sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-176">In the **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.</span></span> <span data-ttu-id="c19ad-177">För den här självstudiekursen distribuerar du en app som inte kräver någon användarinloggning.</span><span class="sxs-lookup"><span data-stu-id="c19ad-177">For this tutorial, you're deploying an app that doesn't need a user login.</span></span>
   
    ![][16]
7. <span data-ttu-id="c19ad-178">Väl tillbaka i dialogrutan **Nytt ASP.NET-projekt** klickar du på **OK** för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="c19ad-178">Back in the **New ASP.NET Project** dialog box, click **OK** to create the project.</span></span>
8. <span data-ttu-id="c19ad-179">I projektet **FrontendWebRole** i **Solution Explorer** högerklickar du på **Referenser** och sedan klickar du på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-179">In **Solution Explorer**, in the **FrontendWebRole** project, right-click **References**, then click **Manage NuGet Packages**.</span></span>
9. <span data-ttu-id="c19ad-180">Klicka på **Bläddra**-fliken och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="c19ad-180">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="c19ad-181">Välj paketet **WindowsAzure.ServiceBus** genom att klicka på **Installera** och godkänn användarvillkoren.</span><span class="sxs-lookup"><span data-stu-id="c19ad-181">Select the **WindowsAzure.ServiceBus** package, click **Install**, and accept the terms of use.</span></span>
   
   ![][13]
   
   <span data-ttu-id="c19ad-182">Observera att de obligatoriska klientsammansättningarna nu refereras och vissa nya kodfiler har lagts till.</span><span class="sxs-lookup"><span data-stu-id="c19ad-182">Note that the required client assemblies are now referenced and some new code files have been added.</span></span>
10. <span data-ttu-id="c19ad-183">I **Solution Explorer** högerklickar du på **Modeller**. Klicka sedan på **Lägg till** och på **Klass**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-183">In **Solution Explorer**, right-click **Models** and click **Add**, then click **Class**.</span></span> <span data-ttu-id="c19ad-184">I rutan **Namn** anger du namnet **OnlineOrder.cs**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-184">In the **Name** box, type the name **OnlineOrder.cs**.</span></span> <span data-ttu-id="c19ad-185">Klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-185">Then click **Add**.</span></span>

### <a name="write-the-code-for-your-web-role"></a><span data-ttu-id="c19ad-186">Skriva koden för webbrollen</span><span class="sxs-lookup"><span data-stu-id="c19ad-186">Write the code for your web role</span></span>
<span data-ttu-id="c19ad-187">I detta avsnitt ska du skapa de olika sidor som din app visar.</span><span class="sxs-lookup"><span data-stu-id="c19ad-187">In this section, you create the various pages that your application displays.</span></span>

1. <span data-ttu-id="c19ad-188">Ersätt den befintliga definitionen för namnområdet med följande kod i filen OnlineOrder.cs i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c19ad-188">In the OnlineOrder.cs file in Visual Studio, replace the existing namespace definition with the following code:</span></span>
   
   ```csharp
   namespace FrontendWebRole.Models
   {
       public class OnlineOrder
       {
           public string Customer { get; set; }
           public string Product { get; set; }
       }
   }
   ```
2. <span data-ttu-id="c19ad-189">I **Solution Explorer** dubbelklickar du på **Controllers\HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-189">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span> <span data-ttu-id="c19ad-190">Lägg till följande **using**-uttryck högst upp i filen för att inkludera namnområdena för den modell som du precis har skapat, samt Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c19ad-190">Add the following **using** statements at the top of the file to include the namespaces for the model you just created, as well as Service Bus.</span></span>
   
   ```csharp
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. <span data-ttu-id="c19ad-191">Ersätt även den befintliga definitionen för namnområdet med följande kod i filen HomeController.cs i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c19ad-191">Also in the HomeController.cs file in Visual Studio, replace the existing namespace definition with the following code.</span></span> <span data-ttu-id="c19ad-192">Den här koden innehåller metoder för att hantera överföring av objekt till kön.</span><span class="sxs-lookup"><span data-stu-id="c19ad-192">This code contains methods for handling the submission of items to the queue.</span></span>
   
   ```csharp
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect to Submit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for the submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from the submission
           // form.
           [HttpPost]
           // Attribute to help prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting to queue here.
   
                   return RedirectToAction("Submit");
               }
               else
               {
                   return View(order);
               }
           }
       }
   }
   ```
4. <span data-ttu-id="c19ad-193">I menyn **Skapa** klickar du på **Skapa lösning** för att kontrollera att det arbete du utfört hittills är korrekt.</span><span class="sxs-lookup"><span data-stu-id="c19ad-193">From the **Build** menu, click **Build Solution** to test the accuracy of your work so far.</span></span>
5. <span data-ttu-id="c19ad-194">Nu ska du skapa vyn för den `Submit()`-metod som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="c19ad-194">Now, create the view for the `Submit()` method you created earlier.</span></span> <span data-ttu-id="c19ad-195">Högerklicka inne i `Submit()`-metoden (överlagringen av `Submit()` som inte tar några parametrar) och välj sedan **Lägg till vy**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-195">Right-click within the `Submit()` method (the overload of `Submit()` that takes no parameters), and then choose **Add View**.</span></span>
   
   ![][14]
6. <span data-ttu-id="c19ad-196">En dialogruta för att skapa vyn visas.</span><span class="sxs-lookup"><span data-stu-id="c19ad-196">A dialog box appears for creating the view.</span></span> <span data-ttu-id="c19ad-197">Välj **Skapa** i listan **Mall**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-197">In the **Template** list, choose **Create**.</span></span> <span data-ttu-id="c19ad-198">Klicka på klassen **OnlineOrder** i listan **Modellklass**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-198">In the **Model class** list, click the **OnlineOrder** class.</span></span>
   
   ![][15]
7. <span data-ttu-id="c19ad-199">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-199">Click **Add**.</span></span>
8. <span data-ttu-id="c19ad-200">Nu ska du ändra visningsnamnet för din app.</span><span class="sxs-lookup"><span data-stu-id="c19ad-200">Now, change the displayed name of your application.</span></span> <span data-ttu-id="c19ad-201">Dubbelklicka på filen **Views\Shared\\_Layout.cshtml** i **Solution Explorer** för att öppna den i Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="c19ad-201">In **Solution Explorer**, double-click the **Views\Shared\\_Layout.cshtml** file to open it in the Visual Studio editor.</span></span>
9. <span data-ttu-id="c19ad-202">Ersätt alla förekomster av **My ASP.NET Application** med **LITWARE'S Products**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-202">Replace all occurrences of **My ASP.NET Application** with **LITWARE'S Products**.</span></span>
10. <span data-ttu-id="c19ad-203">Ta bort länkarna för **Start**, **Om** och **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-203">Remove the **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="c19ad-204">Ta bort den markerade koden:</span><span class="sxs-lookup"><span data-stu-id="c19ad-204">Delete the highlighted code:</span></span>
    
    ![][28]
11. <span data-ttu-id="c19ad-205">Slutligen ändrar du överföringssidan för att inkludera information om kön.</span><span class="sxs-lookup"><span data-stu-id="c19ad-205">Finally, modify the submission page to include some information about the queue.</span></span> <span data-ttu-id="c19ad-206">Dubbelklicka på filen **Views\Home\Submit.cshtml** i **Solution Explorer** för att öppna den i Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="c19ad-206">In **Solution Explorer**, double-click the **Views\Home\Submit.cshtml** file to open it in the Visual Studio editor.</span></span> <span data-ttu-id="c19ad-207">Lägg till följande rad efter `<h2>Submit</h2>`.</span><span class="sxs-lookup"><span data-stu-id="c19ad-207">Add the following line after `<h2>Submit</h2>`.</span></span> <span data-ttu-id="c19ad-208">Just nu är `ViewBag.MessageCount` tom.</span><span class="sxs-lookup"><span data-stu-id="c19ad-208">For now, the `ViewBag.MessageCount` is empty.</span></span> <span data-ttu-id="c19ad-209">Du kommer att fylla i denna senare.</span><span class="sxs-lookup"><span data-stu-id="c19ad-209">You will populate it later.</span></span>
    
    ```html
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```
12. <span data-ttu-id="c19ad-210">Du har nu implementerat ditt användargränssnitt (UI).</span><span class="sxs-lookup"><span data-stu-id="c19ad-210">You now have implemented your UI.</span></span> <span data-ttu-id="c19ad-211">Du kan trycka på **F5** för att köra programmet och bekräfta att det ser ut som du förväntar dig att det ska göra.</span><span class="sxs-lookup"><span data-stu-id="c19ad-211">You can press **F5** to run your application and confirm that it looks as expected.</span></span>
    
    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a><span data-ttu-id="c19ad-212">Skriva koden för att skicka objekt till en Service Bus-kö</span><span class="sxs-lookup"><span data-stu-id="c19ad-212">Write the code for submitting items to a Service Bus queue</span></span>
<span data-ttu-id="c19ad-213">Nu ska du lägga till kod för att skicka objekt till en kö.</span><span class="sxs-lookup"><span data-stu-id="c19ad-213">Now, add code for submitting items to a queue.</span></span> <span data-ttu-id="c19ad-214">Först måste du skapa en klass som innehåller anslutningsinformation för Service Bus-kön.</span><span class="sxs-lookup"><span data-stu-id="c19ad-214">First, you create a class that contains your Service Bus queue connection information.</span></span> <span data-ttu-id="c19ad-215">Sedan initierar du anslutningen från Global.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="c19ad-215">Then, initialize your connection from Global.aspx.cs.</span></span> <span data-ttu-id="c19ad-216">Slutligen uppdaterar du den överföringskod som du skapade tidigare i HomeController.cs för att den faktiskt ska skicka objekt till en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="c19ad-216">Finally, update the submission code you created earlier in HomeController.cs to actually submit items to a Service Bus queue.</span></span>

1. <span data-ttu-id="c19ad-217">Högerklicka på **FrontendWebRole** (högerklicka på projektet, inte på rollen) i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-217">In **Solution Explorer**, right-click **FrontendWebRole** (right-click the project, not the role).</span></span> <span data-ttu-id="c19ad-218">Klicka på **Lägg till** och sedan på **Klass**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-218">Click **Add**, and then click **Class**.</span></span>
2. <span data-ttu-id="c19ad-219">Ge klassen namnet **QueueConnector.cs**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-219">Name the class **QueueConnector.cs**.</span></span> <span data-ttu-id="c19ad-220">Klicka på **Lägg till** för att skapa klassen.</span><span class="sxs-lookup"><span data-stu-id="c19ad-220">Click **Add** to create the class.</span></span>
3. <span data-ttu-id="c19ad-221">Lägg nu till kod som innehåller anslutningsinformationen och initierar anslutningen till en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="c19ad-221">Now, add code that encapsulates the connection information and initializes the connection to a Service Bus queue.</span></span> <span data-ttu-id="c19ad-222">Ersätt hela innehållet i QueueConnector.cs med följande kod och ange värden för `your Service Bus namespace` (namnet på ditt namnområde) och `yourKey`, som är den **primärnyckel** som du tidigare hämtade från Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c19ad-222">Replace the entire contents of QueueConnector.cs with the following code, and enter values for `your Service Bus namespace` (your namespace name) and `yourKey`, which is the **primary key** you previously obtained from the Azure portal.</span></span>
   
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Web;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   
   namespace FrontendWebRole
   {
       public static class QueueConnector
       {
           // Thread-safe. Recommended that you cache rather than recreating it
           // on every request.
           public static QueueClient OrdersQueueClient;
   
           // Obtain these values from the portal.
           public const string Namespace = "your Service Bus namespace";
   
           // The name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create the namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http to be friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create the namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create the queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client to the queue.
               var messagingFactory = MessagingFactory.Create(
                   namespaceManager.Address,
                   namespaceManager.Settings.TokenProvider);
               OrdersQueueClient = messagingFactory.CreateQueueClient(
                   "OrdersQueue");
           }
       }
   }
   ```
4. <span data-ttu-id="c19ad-223">Se nu till att din **initiera**-metod anropas.</span><span class="sxs-lookup"><span data-stu-id="c19ad-223">Now, ensure that your **Initialize** method gets called.</span></span> <span data-ttu-id="c19ad-224">Dubbelklicka på **Global.asax\Global.asax.cs** i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-224">In **Solution Explorer**, double-click **Global.asax\Global.asax.cs**.</span></span>
5. <span data-ttu-id="c19ad-225">Lägg till följande kodrad i slutet av metoden **Application_Start**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-225">Add the following line of code at the end of the **Application_Start** method.</span></span>
   
   ```csharp
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. <span data-ttu-id="c19ad-226">Slutligen uppdaterar du den webbkod som du skapade tidigare, för att på så sätt skicka objekt till kön.</span><span class="sxs-lookup"><span data-stu-id="c19ad-226">Finally, update the web code you created earlier, to submit items to the queue.</span></span> <span data-ttu-id="c19ad-227">I **Solution Explorer** dubbelklickar du på **Controllers\HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-227">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span>
7. <span data-ttu-id="c19ad-228">Uppdatera metoden `Submit()` (överlagring som inte tar några parametrar) enligt följande anvisningar för att hämta meddelanderäknaren för kön.</span><span class="sxs-lookup"><span data-stu-id="c19ad-228">Update the `Submit()` method (the overload that takes no parameters) as follows to get the message count for the queue.</span></span>
   
   ```csharp
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you to perform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get the queue, and obtain the message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. <span data-ttu-id="c19ad-229">Uppdatera metoden `Submit(OnlineOrder order)` (överlagring som inte tar några parametrar) enligt följande anvisningar för att skicka orderinformation till kön.</span><span class="sxs-lookup"><span data-stu-id="c19ad-229">Update the `Submit(OnlineOrder order)` method (the overload that takes one parameter) as follows to submit order information to the queue.</span></span>
   
   ```csharp
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from the order.
           var message = new BrokeredMessage(order);
   
           // Submit the order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. <span data-ttu-id="c19ad-230">Du kan nu köra appen igen.</span><span class="sxs-lookup"><span data-stu-id="c19ad-230">You can now run the application again.</span></span> <span data-ttu-id="c19ad-231">Varje gång du skickar en order, ökar antalet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c19ad-231">Each time you submit an order, the message count increases.</span></span>
   
   ![][18]

## <a name="create-the-worker-role"></a><span data-ttu-id="c19ad-232">Skapa arbetsrollen</span><span class="sxs-lookup"><span data-stu-id="c19ad-232">Create the worker role</span></span>
<span data-ttu-id="c19ad-233">Du ska nu skapa den arbetsroll som behandlar orderöverföringen.</span><span class="sxs-lookup"><span data-stu-id="c19ad-233">You will now create the worker role that processes the order submissions.</span></span> <span data-ttu-id="c19ad-234">I det här exemplet använder vi den projektmall för Visual Studio som heter **Arbetsroll med Service Bus-kö**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-234">This example uses the **Worker Role with Service Bus Queue** Visual Studio project template.</span></span> <span data-ttu-id="c19ad-235">Du har redan fått de autentiseringsuppgifter som krävs från portalen.</span><span class="sxs-lookup"><span data-stu-id="c19ad-235">You already obtained the required credentials from the portal.</span></span>

1. <span data-ttu-id="c19ad-236">Kontrollera att du har anslutit Visual Studio till ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c19ad-236">Make sure you have connected Visual Studio to your Azure account.</span></span>
2. <span data-ttu-id="c19ad-237">Högerklicka på **Roller** i Visual Studio, i **Solution Explorer**, under projektet **MultiTierApp**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-237">In Visual Studio, in **Solution Explorer** right-click the **Roles** folder under the **MultiTierApp** project.</span></span>
3. <span data-ttu-id="c19ad-238">Klicka på **Lägg till** och sedan på **Nytt arbetsrollprojekt**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-238">Click **Add**, and then click **New Worker Role Project**.</span></span> <span data-ttu-id="c19ad-239">Dialogrutan **Lägg till nytt rollprojekt** visas.</span><span class="sxs-lookup"><span data-stu-id="c19ad-239">The **Add New Role Project** dialog box appears.</span></span>
   
   ![][26]
4. <span data-ttu-id="c19ad-240">Klicka på **Arbetsroll med Service Bus-kö** i dialogrutan **Lägg till nytt rollprojekt**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-240">In the **Add New Role Project** dialog box, click **Worker Role with Service Bus Queue**.</span></span>
   
   ![][23]
5. <span data-ttu-id="c19ad-241">Ge projektet namnet **OrderProcessingRole** i rutan **Namn**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-241">In the **Name** box, name the project **OrderProcessingRole**.</span></span> <span data-ttu-id="c19ad-242">Klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-242">Then click **Add**.</span></span>
6. <span data-ttu-id="c19ad-243">Kopiera den anslutningssträng som du fick i steg 9 av avsnittet ”Skapa ett namnområde för Service Bus” till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="c19ad-243">Copy the connection string that you obtained in step 9 of the "Create a Service Bus namespace" section to the clipboard.</span></span>
7. <span data-ttu-id="c19ad-244">I **Solution Explorer** högerklickar du på **OrderProcessingRole** som du skapade i steg 5 (se till att du högerklickar på **OrderProcessingRole** under **Roller** och inte på klassen).</span><span class="sxs-lookup"><span data-stu-id="c19ad-244">In **Solution Explorer**, right-click the **OrderProcessingRole** you created in step 5 (make sure that you right-click **OrderProcessingRole** under **Roles**, and not the class).</span></span> <span data-ttu-id="c19ad-245">Klicka sedan på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-245">Then click **Properties**.</span></span>
8. <span data-ttu-id="c19ad-246">På fliken **Inställningar** i dialogrutan **Egenskaper** klickar du inuti rutan **Värde** för **Microsoft.ServiceBus.ConnectionString** och sedan klistrar du in slutpunktsvärdet som du kopierade i steg 6.</span><span class="sxs-lookup"><span data-stu-id="c19ad-246">On the **Settings** tab of the **Properties** dialog box, click inside the **Value** box for **Microsoft.ServiceBus.ConnectionString**, and then paste the endpoint value you copied in step 6.</span></span>
   
   ![][25]
9. <span data-ttu-id="c19ad-247">Skapa en **OnlineOrder**-klass för att representera ordrarna allt eftersom du behandlar dem från kön.</span><span class="sxs-lookup"><span data-stu-id="c19ad-247">Create an **OnlineOrder** class to represent the orders as you process them from the queue.</span></span> <span data-ttu-id="c19ad-248">Du kan återanvända en klass som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="c19ad-248">You can reuse a class you have already created.</span></span> <span data-ttu-id="c19ad-249">Högerklicka på klassen **OrderProcessingRole** (högerklicka på klassikonen, inte på rollen) i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-249">In **Solution Explorer**, right-click the **OrderProcessingRole** class (right-click the class icon, not the role).</span></span> <span data-ttu-id="c19ad-250">Klicka på **Lägg till** och sedan på **Befintligt objekt**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-250">Click **Add**, then click **Existing Item**.</span></span>
10. <span data-ttu-id="c19ad-251">Bläddra till undermappen för **FrontendWebRole\Models** och dubbelklicka sedan på **OnlineOrder.cs** för att lägga till den i projektet.</span><span class="sxs-lookup"><span data-stu-id="c19ad-251">Browse to the subfolder for **FrontendWebRole\Models**, and then double-click **OnlineOrder.cs** to add it to this project.</span></span>
11. <span data-ttu-id="c19ad-252">I **WorkerRole.cs** ändrar du värdet för variabeln **QueueName** från `"ProcessingQueue"` till `"OrdersQueue"`, som visas i följande kod.</span><span class="sxs-lookup"><span data-stu-id="c19ad-252">In **WorkerRole.cs**, change the value of the **QueueName** variable from `"ProcessingQueue"` to `"OrdersQueue"` as shown in the following code.</span></span>
    
    ```csharp
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. <span data-ttu-id="c19ad-253">Lägg till följande using-uttryck högst upp i filen WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="c19ad-253">Add the following using statement at the top of the WorkerRole.cs file.</span></span>
    
    ```csharp
    using FrontendWebRole.Models;
    ```
13. <span data-ttu-id="c19ad-254">I funktionen `Run()`, inuti anropet `OnMessage()`, ersätter du innehållet i satsen `try` med följande kod.</span><span class="sxs-lookup"><span data-stu-id="c19ad-254">In the `Run()` function, inside the `OnMessage()` call, replace the contents of the `try` clause with the following code.</span></span>
    
    ```csharp
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. <span data-ttu-id="c19ad-255">Du har nu gjort klart appen!</span><span class="sxs-lookup"><span data-stu-id="c19ad-255">You have completed the application.</span></span> <span data-ttu-id="c19ad-256">Du kan testa den kompletta appen genom att högerklicka på MultiTierApp-projektet i Solution Explorer och välja **Ställ in som startprojekt**. Slutligen trycker du på F5.</span><span class="sxs-lookup"><span data-stu-id="c19ad-256">You can test the full application by right-clicking the MultiTierApp project in Solution Explorer, selecting **Set as Startup Project**, and then pressing F5.</span></span> <span data-ttu-id="c19ad-257">Observera att antalet meddelanden inte ökar. Detta eftersom arbetsrollen behandlar objekt från kön och sedan markeras dessa som slutförda.</span><span class="sxs-lookup"><span data-stu-id="c19ad-257">Note that the message count does not increment, because the worker role processes items from the queue and marks them as complete.</span></span> <span data-ttu-id="c19ad-258">Du kan se spårad utdata från arbetsrollen genom att titta i användargränssnittet för Azure Compute Emulator.</span><span class="sxs-lookup"><span data-stu-id="c19ad-258">You can see the trace output of your worker role by viewing the Azure Compute Emulator UI.</span></span> <span data-ttu-id="c19ad-259">Du kan göra detta genom att högerklicka på emulatorikonen i aktivitetsfältets meddelandefält och välja **Visa Compute Emulator UI**.</span><span class="sxs-lookup"><span data-stu-id="c19ad-259">You can do this by right-clicking the emulator icon in the notification area of your taskbar and selecting **Show Compute Emulator UI**.</span></span>
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a><span data-ttu-id="c19ad-260">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c19ad-260">Next steps</span></span>
<span data-ttu-id="c19ad-261">Om du vill lära dig mer om Service Bus kan du använda följande resurser:</span><span class="sxs-lookup"><span data-stu-id="c19ad-261">To learn more about Service Bus, see the following resources:</span></span>  

* <span data-ttu-id="c19ad-262">[Dokumentation för Azure Service Bus][sbdocs]</span><span class="sxs-lookup"><span data-stu-id="c19ad-262">[Azure Service Bus documentation][sbdocs]</span></span>  
* <span data-ttu-id="c19ad-263">[Tjänstesida för Service Bus][sbacom]</span><span class="sxs-lookup"><span data-stu-id="c19ad-263">[Service Bus service page][sbacom]</span></span>  
* <span data-ttu-id="c19ad-264">[Använda Service Bus-köer][sbacomqhowto]</span><span class="sxs-lookup"><span data-stu-id="c19ad-264">[How to Use Service Bus Queues][sbacomqhowto]</span></span>  

<span data-ttu-id="c19ad-265">Mer information om flernivåscenarier finns i:</span><span class="sxs-lookup"><span data-stu-id="c19ad-265">To learn more about multi-tier scenarios, see:</span></span>  

* <span data-ttu-id="c19ad-266">[.NET-flernivåapp med hjälp av lagringstabeller, köer och blob-objekt][mutitierstorage]</span><span class="sxs-lookup"><span data-stu-id="c19ad-266">[.NET Multi-Tier Application Using Storage Tables, Queues, and Blobs][mutitierstorage]</span></span>  

[0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
[2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
[9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
[10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
[11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
[12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
[13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
[14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
[15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
[16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
[17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app2.png

[19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
[20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
[23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
[25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
[26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
[28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

[sbdocs]: /azure/service-bus-messaging/  
[sbacom]: https://azure.microsoft.com/services/service-bus/  
[sbacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
[mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
