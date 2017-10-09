---
title: "aaa.NET flernivåapp med hjälp av Azure Service Bus | Microsoft Docs"
description: "En .NET-självstudiekurs som hjälper dig att utveckla en flernivåapp i Azure som använder Service Bus-köer toocommunicate mellan nivåer."
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
ms.openlocfilehash: 485910ff1d3b8b0a709ee14ede32e57cf873829a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a><span data-ttu-id="49daf-103">.NET-flernivåapp med hjälp av Azure Service Bus-köer</span><span class="sxs-lookup"><span data-stu-id="49daf-103">.NET multi-tier application using Azure Service Bus queues</span></span>
## <a name="introduction"></a><span data-ttu-id="49daf-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="49daf-104">Introduction</span></span>
<span data-ttu-id="49daf-105">Utveckla för Microsoft Azure är enkelt med Visual Studio och hello kostnadsfria Azure SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="49daf-105">Developing for Microsoft Azure is easy using Visual Studio and hello free Azure SDK for .NET.</span></span> <span data-ttu-id="49daf-106">Den här självstudiekursen vägleder dig genom hello steg toocreate ett program som använder flera Azure-resurser som körs i din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="49daf-106">This tutorial walks you through hello steps toocreate an application that uses multiple Azure resources running in your local environment.</span></span>

<span data-ttu-id="49daf-107">Får du lära dig hello följande:</span><span class="sxs-lookup"><span data-stu-id="49daf-107">You will learn hello following:</span></span>

* <span data-ttu-id="49daf-108">Hur tooenable datorn för Azure-utveckling med en enda hämta och installera.</span><span class="sxs-lookup"><span data-stu-id="49daf-108">How tooenable your computer for Azure development with a single download and install.</span></span>
* <span data-ttu-id="49daf-109">Hur toouse Visual Studio toodevelop för Azure.</span><span class="sxs-lookup"><span data-stu-id="49daf-109">How toouse Visual Studio toodevelop for Azure.</span></span>
* <span data-ttu-id="49daf-110">Hur toocreate en flernivåapp i Azure med hjälp av webb-och arbetsroller.</span><span class="sxs-lookup"><span data-stu-id="49daf-110">How toocreate a multi-tier application in Azure using web and worker roles.</span></span>
* <span data-ttu-id="49daf-111">Hur toocommunicate mellan nivåer med hjälp av Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="49daf-111">How toocommunicate between tiers using Service Bus queues.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

<span data-ttu-id="49daf-112">I den här självstudiekursen kommer du bygger och kör hello flernivåapp i en Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="49daf-112">In this tutorial you'll build and run hello multi-tier application in an Azure cloud service.</span></span> <span data-ttu-id="49daf-113">hello klientdelen är en ASP.NET MVC-webbroll och serverdelen hello är en arbetsroll som använder Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="49daf-113">hello front end is an ASP.NET MVC web role and hello back end is a worker-role that uses a Service Bus queue.</span></span> <span data-ttu-id="49daf-114">Du kan skapa hello samma flernivåapp med klientdelen hello som ett webbprojekt som är distribuerade tooan Azure-webbplats i stället för en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="49daf-114">You can create hello same multi-tier application with hello front end as a web project, that is deployed tooan Azure website instead of a cloud service.</span></span> <span data-ttu-id="49daf-115">Du kan också testa hello [.NET på lokalt/i molnet hybridprogram](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="49daf-115">You can also try out hello [.NET on-premises/cloud hybrid application](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) tutorial.</span></span>

<span data-ttu-id="49daf-116">hello visar följande skärmbild som hello slutföra programmet.</span><span class="sxs-lookup"><span data-stu-id="49daf-116">hello following screen shot shows hello completed application.</span></span>

![][0]

## <a name="scenario-overview-inter-role-communication"></a><span data-ttu-id="49daf-117">Scenarioöversikt: kommunikation mellan roller</span><span class="sxs-lookup"><span data-stu-id="49daf-117">Scenario overview: inter-role communication</span></span>
<span data-ttu-id="49daf-118">toosubmit en order för bearbetning, hello frontend UI-komponent, körs i hello webbrollen, måste interagera med hello mellannivålogik som körs i hello worker-rollen.</span><span class="sxs-lookup"><span data-stu-id="49daf-118">toosubmit an order for processing, hello front-end UI component, running in hello web role, must interact with hello middle tier logic running in hello worker role.</span></span> <span data-ttu-id="49daf-119">Det här exemplet använder Service Bus-meddelanden för hello kommunikation mellan hello nivåer.</span><span class="sxs-lookup"><span data-stu-id="49daf-119">This example uses Service Bus messaging for hello communication between hello tiers.</span></span>

<span data-ttu-id="49daf-120">Med hjälp av Service Bus frikopplas-meddelanden mellan hello webben och mellannivåerna de båda komponenterna.</span><span class="sxs-lookup"><span data-stu-id="49daf-120">Using Service Bus messaging between hello web and middle tiers decouples the two components.</span></span> <span data-ttu-id="49daf-121">Däremot toodirect messaging (d.v.s. TCP eller HTTP) hello webbnivå inte ansluta toohello mellannivå direkt; i stället skickar den arbetsenheter, som meddelanden, till Service Bus, som lagrar dem tills hello mellannivån är klar tooconsume och bearbeta dem på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="49daf-121">In contrast toodirect messaging (that is, TCP or HTTP), hello web tier does not connect toohello middle tier directly; instead it pushes units of work, as messages, into Service Bus, which reliably retains them until hello middle tier is ready tooconsume and process them.</span></span>

<span data-ttu-id="49daf-122">Service Bus innehåller två entiteter toosupport asynkrona meddelanden: köer och ämnen.</span><span class="sxs-lookup"><span data-stu-id="49daf-122">Service Bus provides two entities toosupport brokered messaging: queues and topics.</span></span> <span data-ttu-id="49daf-123">Med köer skickas varje meddelande toohello kö används av en enda mottagare.</span><span class="sxs-lookup"><span data-stu-id="49daf-123">With queues, each message sent toohello queue is consumed by a single receiver.</span></span> <span data-ttu-id="49daf-124">Ämnena stödjer hello Publicera/prenumerera-mönster där varje publicerat meddelande görs tillgängligt tooa prenumeration som har registrerats med hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="49daf-124">Topics support hello publish/subscribe pattern in which each published message is made available tooa subscription registered with hello topic.</span></span> <span data-ttu-id="49daf-125">Varje prenumeration underhåller logiskt nog sin egen meddelandekö.</span><span class="sxs-lookup"><span data-stu-id="49daf-125">Each subscription logically maintains its own queue of messages.</span></span> <span data-ttu-id="49daf-126">Prenumerationer kan också konfigureras med filterregler som begränsar hello uppsättning av meddelanden som skickas till hello prenumeration kön toothose som matchar filtret hello.</span><span class="sxs-lookup"><span data-stu-id="49daf-126">Subscriptions can also be configured with filter rules that restrict hello set of messages passed to hello subscription queue toothose that match hello filter.</span></span> <span data-ttu-id="49daf-127">hello används följande exempel Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="49daf-127">hello following example uses Service Bus queues.</span></span>

![][1]

<span data-ttu-id="49daf-128">Denna kommunikationsmekanism har flera fördelar jämfört med funktioner för direkta meddelanden:</span><span class="sxs-lookup"><span data-stu-id="49daf-128">This communication mechanism has several advantages over direct messaging:</span></span>

* <span data-ttu-id="49daf-129">**Tidsbestämd frikoppling.**</span><span class="sxs-lookup"><span data-stu-id="49daf-129">**Temporal decoupling.**</span></span> <span data-ttu-id="49daf-130">Med hello mönstret för asynkrona meddelanden, producenter och konsumenter behöver inte vara online på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="49daf-130">With hello asynchronous messaging pattern, producers and consumers need not be online at hello same time.</span></span> <span data-ttu-id="49daf-131">Service Bus lagrar meddelanden på ett tillförlitligt sätt tills hello konsumerande parten är redo att ta emot dem..</span><span class="sxs-lookup"><span data-stu-id="49daf-131">Service Bus reliably stores messages until hello consuming party is ready to receive them.</span></span> <span data-ttu-id="49daf-132">Detta gör att hello komponenter i hello distribuerade program toobe frånkopplad, antingen frivilligt, till exempel för underhåll, eller på grund av tooa komponentkrasch, utan att påverka systemet som helhet.</span><span class="sxs-lookup"><span data-stu-id="49daf-132">This enables hello components of hello distributed application toobe disconnected, either voluntarily, for example, for maintenance, or due tooa component crash, without impacting the system as a whole.</span></span> <span data-ttu-id="49daf-133">Dessutom kanske hello förbrukar program behöver bara toocome online vid vissa tidpunkter på dagen hello.</span><span class="sxs-lookup"><span data-stu-id="49daf-133">Furthermore, hello consuming application might only need toocome online during certain times of hello day.</span></span>
* <span data-ttu-id="49daf-134">**Belastningsutjämning.**</span><span class="sxs-lookup"><span data-stu-id="49daf-134">**Load leveling.**</span></span> <span data-ttu-id="49daf-135">I många program varierar systembelastningen över tiden, medan hello bearbetningstid som krävs för varje arbetsenhet vanligtvis är konstant.</span><span class="sxs-lookup"><span data-stu-id="49daf-135">In many applications, system load varies over time, while hello processing time required for each unit of work is typically constant.</span></span> <span data-ttu-id="49daf-136">Medlingen mellan meddelandeproducenter och konsumenter med hjälp av en kö innebär att hello förbrukar program (hello arbetaren) endast måste toobe etableras tooaccommodate genomsnittlig belastning i stället för belastning.</span><span class="sxs-lookup"><span data-stu-id="49daf-136">Intermediating message producers and consumers with a queue means that hello consuming application (hello worker) only needs toobe provisioned tooaccommodate average load rather than peak load.</span></span> <span data-ttu-id="49daf-137">hello kön hello djup växer och dras samman allt eftersom hello inkommande belastningen varierar.</span><span class="sxs-lookup"><span data-stu-id="49daf-137">hello depth of hello queue grows and contracts as hello incoming load varies.</span></span> <span data-ttu-id="49daf-138">Detta sparar direkt pengar beträffande hello mängden infrastruktur krävs tooservice hello programinläsning.</span><span class="sxs-lookup"><span data-stu-id="49daf-138">This directly saves money in terms of hello amount of infrastructure required tooservice hello application load.</span></span>
* <span data-ttu-id="49daf-139">**Belastningsutjämning.**</span><span class="sxs-lookup"><span data-stu-id="49daf-139">**Load balancing.**</span></span> <span data-ttu-id="49daf-140">Eftersom belastningen ökar kan läggas fler arbetsprocesser tooread från hello kö.</span><span class="sxs-lookup"><span data-stu-id="49daf-140">As load increases, more worker processes can be added tooread from hello queue.</span></span> <span data-ttu-id="49daf-141">Varje meddelande bearbetas av bara en av hello arbetsprocesser.</span><span class="sxs-lookup"><span data-stu-id="49daf-141">Each message is processed by only one of hello worker processes.</span></span> <span data-ttu-id="49daf-142">Dessutom gör den här pull-baserade belastningsbalanseringen att optimal användning av arbetsdatorerna hello även om arbetsdatorerna skiljer sig åt vad gäller processorkraft: de hämtar meddelanden med sina egna högsta hastighet.</span><span class="sxs-lookup"><span data-stu-id="49daf-142">Furthermore, this pull-based load balancing enables optimal use of hello worker machines even if the worker machines differ in terms of processing power, as they will pull messages at their own maximum rate.</span></span> <span data-ttu-id="49daf-143">Det här mönstret kallas ofta hello *konkurrerande konsument* mönster.</span><span class="sxs-lookup"><span data-stu-id="49daf-143">This pattern is often termed hello *competing consumer* pattern.</span></span>
  
  ![][2]

<span data-ttu-id="49daf-144">hello följande avsnitt beskrivs hello-kod som implementerar denna arkitektur.</span><span class="sxs-lookup"><span data-stu-id="49daf-144">hello following sections discuss hello code that implements this architecture.</span></span>

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="49daf-145">Ställa in hello utvecklingsmiljö</span><span class="sxs-lookup"><span data-stu-id="49daf-145">Set up hello development environment</span></span>
<span data-ttu-id="49daf-146">Innan du kan börja utveckla Azure-program, hämta hello verktyg och Ställ in din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="49daf-146">Before you can begin developing Azure applications, get hello tools and set up your development environment.</span></span>

1. <span data-ttu-id="49daf-147">Installera hello Azure SDK för .NET från hello SDK [Nedladdningssida](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="49daf-147">Install hello Azure SDK for .NET from hello SDK [downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="49daf-148">I hello **.NET** kolumn, och klicka på hello version av [Visual Studio](http://www.visualstudio.com) du använder.</span><span class="sxs-lookup"><span data-stu-id="49daf-148">In hello **.NET** column, click hello version of [Visual Studio](http://www.visualstudio.com) you are using.</span></span> <span data-ttu-id="49daf-149">hello stegen i den här självstudiekursen används Visual Studio 2015, men de fungerar även med Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="49daf-149">hello steps in this tutorial use Visual Studio 2015, but they also work with Visual Studio 2017.</span></span>
3. <span data-ttu-id="49daf-150">När du uppmanas toorun eller spara hello installer, klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="49daf-150">When prompted toorun or save hello installer, click **Run**.</span></span>
4. <span data-ttu-id="49daf-151">I hello **installationsprogram för webbplattform**, klickar du på **installera** och fortsätta med installationen hello.</span><span class="sxs-lookup"><span data-stu-id="49daf-151">In hello **Web Platform Installer**, click **Install** and proceed with hello installation.</span></span>
5. <span data-ttu-id="49daf-152">När hello installationen är klar har du allt nödvändigt toostart toodevelop hello app.</span><span class="sxs-lookup"><span data-stu-id="49daf-152">Once hello installation is complete, you will have everything necessary toostart toodevelop hello app.</span></span> <span data-ttu-id="49daf-153">hello SDK inkluderar verktyg som låter dig utveckla Azure-program i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49daf-153">hello SDK includes tools that let you easily develop Azure applications in Visual Studio.</span></span>

## <a name="create-a-namespace"></a><span data-ttu-id="49daf-154">Skapa ett namnområde</span><span class="sxs-lookup"><span data-stu-id="49daf-154">Create a namespace</span></span>
<span data-ttu-id="49daf-155">hello nästa steg är toocreate ett namnområde för tjänsten och få en delad signatur åtkomst (SAS)-nyckel.</span><span class="sxs-lookup"><span data-stu-id="49daf-155">hello next step is toocreate a service namespace, and obtain a Shared Access Signature (SAS) key.</span></span> <span data-ttu-id="49daf-156">Ett namnområde ger en appgräns för varje app som exponeras via Service Bus.</span><span class="sxs-lookup"><span data-stu-id="49daf-156">A namespace provides an application boundary for each application exposed through Service Bus.</span></span> <span data-ttu-id="49daf-157">SAS-nyckeln genereras av hello systemet när ett namnområde har skapats.</span><span class="sxs-lookup"><span data-stu-id="49daf-157">A SAS key is generated by hello system when a namespace is created.</span></span> <span data-ttu-id="49daf-158">hello kombinationen av namnområdet och SAS-nyckeln ger hello autentiseringsuppgifter för Service Bus tooauthenticate åtkomst tooan program.</span><span class="sxs-lookup"><span data-stu-id="49daf-158">hello combination of namespace and SAS key provides hello credentials for Service Bus tooauthenticate access tooan application.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a><span data-ttu-id="49daf-159">Skapa en webbroll</span><span class="sxs-lookup"><span data-stu-id="49daf-159">Create a web role</span></span>
<span data-ttu-id="49daf-160">I det här avsnittet skapar du hello klientdelen för ditt program.</span><span class="sxs-lookup"><span data-stu-id="49daf-160">In this section, you build hello front end of your application.</span></span> <span data-ttu-id="49daf-161">Först skapar du hello sidor som din app visar.</span><span class="sxs-lookup"><span data-stu-id="49daf-161">First, you create hello pages that your application displays.</span></span>
<span data-ttu-id="49daf-162">Efter det att lägga till kod som skickar objekt tooa Service Bus-kö och visar statusinformation om hello kö.</span><span class="sxs-lookup"><span data-stu-id="49daf-162">After that, add code that submits items tooa Service Bus queue and displays status information about hello queue.</span></span>

### <a name="create-hello-project"></a><span data-ttu-id="49daf-163">Skapa hello-projekt</span><span class="sxs-lookup"><span data-stu-id="49daf-163">Create hello project</span></span>
1. <span data-ttu-id="49daf-164">Starta Visual Studio med administratörsbehörighet: Högerklicka på hello **Visual Studio** programikonen och klicka sedan på **kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="49daf-164">Using administrator privileges, start Visual Studio: right-click hello **Visual Studio** program icon, and then click **Run as administrator**.</span></span> <span data-ttu-id="49daf-165">hello Azure compute emulator, som beskrivs senare i den här artikeln kräver att Visual Studio startas med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="49daf-165">hello Azure compute emulator, discussed later in this article, requires that Visual Studio be started with administrator privileges.</span></span>
   
   <span data-ttu-id="49daf-166">I Visual Studio på hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="49daf-166">In Visual Studio, on hello **File** menu, click **New**, and then click **Project**.</span></span>
2. <span data-ttu-id="49daf-167">Från **Installerade mallar**, under **Visual C#**, klickar du på **Moln** och sedan på **Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="49daf-167">From **Installed Templates**, under **Visual C#**, click **Cloud** and then click **Azure Cloud Service**.</span></span> <span data-ttu-id="49daf-168">Namnet hello projektet **MultiTierApp**.</span><span class="sxs-lookup"><span data-stu-id="49daf-168">Name hello project **MultiTierApp**.</span></span> <span data-ttu-id="49daf-169">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="49daf-169">Then click **OK**.</span></span>
   
   ![][9]
3. <span data-ttu-id="49daf-170">Dubbelklicka på **ASP.NET Web Role** från rollerna **.NET Framework 4.5**.</span><span class="sxs-lookup"><span data-stu-id="49daf-170">From **.NET Framework 4.5** roles, double-click **ASP.NET Web Role**.</span></span>
   
   ![][10]
4. <span data-ttu-id="49daf-171">Hovra över **WebRole1** under **Azure Cloud Service solution**och klicka på pennikonen hello Byt namn på hello webbroll för**FrontendWebRole**.</span><span class="sxs-lookup"><span data-stu-id="49daf-171">Hover over **WebRole1** under **Azure Cloud Service solution**, click hello pencil icon, and rename hello web role too**FrontendWebRole**.</span></span> <span data-ttu-id="49daf-172">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="49daf-172">Then click **OK**.</span></span> <span data-ttu-id="49daf-173">(Kontrollera att du anger ”Frontend” med ett litet ”e”, det vill säga, inte ”FrontEnd”).</span><span class="sxs-lookup"><span data-stu-id="49daf-173">(Make sure you enter "Frontend" with a lower-case 'e,' not "FrontEnd".)</span></span>
   
   ![][11]
5. <span data-ttu-id="49daf-174">Från hello **nytt ASP.NET-projekt** i dialogrutan hello **Välj en mall** klickar du på **MVC**.</span><span class="sxs-lookup"><span data-stu-id="49daf-174">From hello **New ASP.NET Project** dialog box, in hello **Select a template** list, click **MVC**.</span></span>
   
   ![][12]
6. <span data-ttu-id="49daf-175">Fortfarande i hello **nytt ASP.NET-projekt** dialogrutan klickar du på hello **ändra autentisering** knappen.</span><span class="sxs-lookup"><span data-stu-id="49daf-175">Still in hello **New ASP.NET Project** dialog box, click hello **Change Authentication** button.</span></span> <span data-ttu-id="49daf-176">I hello **ändra autentisering** dialogrutan klickar du på **ingen autentisering**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="49daf-176">In hello **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.</span></span> <span data-ttu-id="49daf-177">För den här självstudiekursen distribuerar du en app som inte kräver någon användarinloggning.</span><span class="sxs-lookup"><span data-stu-id="49daf-177">For this tutorial, you're deploying an app that doesn't need a user login.</span></span>
   
    ![][16]
7. <span data-ttu-id="49daf-178">Tillbaka i hello **nytt ASP.NET-projekt** dialogrutan klickar du på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="49daf-178">Back in hello **New ASP.NET Project** dialog box, click **OK** toocreate hello project.</span></span>
8. <span data-ttu-id="49daf-179">I **Solution Explorer**, i hello **FrontendWebRole** projekt, högerklicka på **referenser**, klicka på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="49daf-179">In **Solution Explorer**, in hello **FrontendWebRole** project, right-click **References**, then click **Manage NuGet Packages**.</span></span>
9. <span data-ttu-id="49daf-180">Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="49daf-180">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="49daf-181">Välj hello **WindowsAzure.ServiceBus** paketet genom att klicka på **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="49daf-181">Select hello **WindowsAzure.ServiceBus** package, click **Install**, and accept hello terms of use.</span></span>
   
   ![][13]
   
   <span data-ttu-id="49daf-182">Observera att hello krävs klientsammansättningarna nu refereras och vissa nya kodfiler har lagts till.</span><span class="sxs-lookup"><span data-stu-id="49daf-182">Note that hello required client assemblies are now referenced and some new code files have been added.</span></span>
10. <span data-ttu-id="49daf-183">I **Solution Explorer** högerklickar du på **Modeller**. Klicka sedan på **Lägg till** och på **Klass**.</span><span class="sxs-lookup"><span data-stu-id="49daf-183">In **Solution Explorer**, right-click **Models** and click **Add**, then click **Class**.</span></span> <span data-ttu-id="49daf-184">I hello **namn** rutan, hello typnamn **OnlineOrder.cs**.</span><span class="sxs-lookup"><span data-stu-id="49daf-184">In hello **Name** box, type hello name **OnlineOrder.cs**.</span></span> <span data-ttu-id="49daf-185">Klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="49daf-185">Then click **Add**.</span></span>

### <a name="write-hello-code-for-your-web-role"></a><span data-ttu-id="49daf-186">Skriva hello koden för webbrollen</span><span class="sxs-lookup"><span data-stu-id="49daf-186">Write hello code for your web role</span></span>
<span data-ttu-id="49daf-187">I det här avsnittet skapar du hello olika sidor som din app visar.</span><span class="sxs-lookup"><span data-stu-id="49daf-187">In this section, you create hello various pages that your application displays.</span></span>

1. <span data-ttu-id="49daf-188">Ersätt den befintliga definitionen för namnområdet med följande kod hello i hello OnlineOrder.cs-filen i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="49daf-188">In hello OnlineOrder.cs file in Visual Studio, replace the existing namespace definition with hello following code:</span></span>
   
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
2. <span data-ttu-id="49daf-189">I **Solution Explorer** dubbelklickar du på **Controllers\HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="49daf-189">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span> <span data-ttu-id="49daf-190">Lägg till följande hello **med** instruktioner överst hello i hello filen tooinclude hello namnområdena för den modell som du precis skapade, samt Service Bus.</span><span class="sxs-lookup"><span data-stu-id="49daf-190">Add hello following **using** statements at hello top of hello file tooinclude hello namespaces for the model you just created, as well as Service Bus.</span></span>
   
   ```csharp
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. <span data-ttu-id="49daf-191">Ersätt även den befintliga definitionen för namnområdet med följande kod hello i hello HomeController.cs filen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49daf-191">Also in hello HomeController.cs file in Visual Studio, replace the existing namespace definition with hello following code.</span></span> <span data-ttu-id="49daf-192">Den här koden innehåller metoder för att hantera hello överföring av objekt toohello kö.</span><span class="sxs-lookup"><span data-stu-id="49daf-192">This code contains methods for handling hello submission of items toohello queue.</span></span>
   
   ```csharp
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect tooSubmit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for hello submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from hello submission
           // form.
           [HttpPost]
           // Attribute toohelp prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting tooqueue here.
   
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
4. <span data-ttu-id="49daf-193">Från hello **skapa** -menyn klickar du på **skapa lösning** tootest hello noggrannhet arbete hittills.</span><span class="sxs-lookup"><span data-stu-id="49daf-193">From hello **Build** menu, click **Build Solution** tootest hello accuracy of your work so far.</span></span>
5. <span data-ttu-id="49daf-194">Nu skapar hello vy för hello `Submit()` metod som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="49daf-194">Now, create hello view for hello `Submit()` method you created earlier.</span></span> <span data-ttu-id="49daf-195">Högerklicka inne hello `Submit()` metod (hello överlagring för `Submit()` som tar några parametrar), och välj sedan **Lägg till vy**.</span><span class="sxs-lookup"><span data-stu-id="49daf-195">Right-click within hello `Submit()` method (hello overload of `Submit()` that takes no parameters), and then choose **Add View**.</span></span>
   
   ![][14]
6. <span data-ttu-id="49daf-196">En dialogruta för att skapa hello vyn.</span><span class="sxs-lookup"><span data-stu-id="49daf-196">A dialog box appears for creating hello view.</span></span> <span data-ttu-id="49daf-197">I hello **mallen** Välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="49daf-197">In hello **Template** list, choose **Create**.</span></span> <span data-ttu-id="49daf-198">I hello **Modellklass** klickar du på hello **OnlineOrder** klass.</span><span class="sxs-lookup"><span data-stu-id="49daf-198">In hello **Model class** list, click hello **OnlineOrder** class.</span></span>
   
   ![][15]
7. <span data-ttu-id="49daf-199">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="49daf-199">Click **Add**.</span></span>
8. <span data-ttu-id="49daf-200">Nu ska du ändra hello visas namnet på ditt program.</span><span class="sxs-lookup"><span data-stu-id="49daf-200">Now, change hello displayed name of your application.</span></span> <span data-ttu-id="49daf-201">I **Solution Explorer**, dubbelklicka på den **Views\Shared\\_Layout.cshtml** filen tooopen i hello Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="49daf-201">In **Solution Explorer**, double-click the **Views\Shared\\_Layout.cshtml** file tooopen it in hello Visual Studio editor.</span></span>
9. <span data-ttu-id="49daf-202">Ersätt alla förekomster av **My ASP.NET Application** med **LITWARE'S Products**.</span><span class="sxs-lookup"><span data-stu-id="49daf-202">Replace all occurrences of **My ASP.NET Application** with **LITWARE'S Products**.</span></span>
10. <span data-ttu-id="49daf-203">Ta bort hello **Start**, **om**, och **Kontakta** länkar.</span><span class="sxs-lookup"><span data-stu-id="49daf-203">Remove hello **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="49daf-204">Ta bort hello markerade koden:</span><span class="sxs-lookup"><span data-stu-id="49daf-204">Delete hello highlighted code:</span></span>
    
    ![][28]
11. <span data-ttu-id="49daf-205">Slutligen ändra hello skickas sidan tooinclude viss information om hello kö.</span><span class="sxs-lookup"><span data-stu-id="49daf-205">Finally, modify hello submission page tooinclude some information about hello queue.</span></span> <span data-ttu-id="49daf-206">I **Solution Explorer**, dubbelklicka på den **Views\Home\Submit.cshtml** filen tooopen i hello Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="49daf-206">In **Solution Explorer**, double-click the **Views\Home\Submit.cshtml** file tooopen it in hello Visual Studio editor.</span></span> <span data-ttu-id="49daf-207">Lägg till följande rad efter hello `<h2>Submit</h2>`.</span><span class="sxs-lookup"><span data-stu-id="49daf-207">Add hello following line after `<h2>Submit</h2>`.</span></span> <span data-ttu-id="49daf-208">Nu hello `ViewBag.MessageCount` är tom.</span><span class="sxs-lookup"><span data-stu-id="49daf-208">For now, hello `ViewBag.MessageCount` is empty.</span></span> <span data-ttu-id="49daf-209">Du kommer att fylla i denna senare.</span><span class="sxs-lookup"><span data-stu-id="49daf-209">You will populate it later.</span></span>
    
    ```html
    <p>Current number of orders in queue waiting toobe processed: @ViewBag.MessageCount</p>
    ```
12. <span data-ttu-id="49daf-210">Du har nu implementerat ditt användargränssnitt (UI).</span><span class="sxs-lookup"><span data-stu-id="49daf-210">You now have implemented your UI.</span></span> <span data-ttu-id="49daf-211">Du kan trycka på **F5** toorun programmet och bekräfta att det ser ut som förväntat.</span><span class="sxs-lookup"><span data-stu-id="49daf-211">You can press **F5** toorun your application and confirm that it looks as expected.</span></span>
    
    ![][17]

### <a name="write-hello-code-for-submitting-items-tooa-service-bus-queue"></a><span data-ttu-id="49daf-212">Skriva hello-kod för att skicka objekt tooa Service Bus-kö</span><span class="sxs-lookup"><span data-stu-id="49daf-212">Write hello code for submitting items tooa Service Bus queue</span></span>
<span data-ttu-id="49daf-213">Lägg nu till kod för att skicka objekt tooa kön.</span><span class="sxs-lookup"><span data-stu-id="49daf-213">Now, add code for submitting items tooa queue.</span></span> <span data-ttu-id="49daf-214">Först måste du skapa en klass som innehåller anslutningsinformation för Service Bus-kön.</span><span class="sxs-lookup"><span data-stu-id="49daf-214">First, you create a class that contains your Service Bus queue connection information.</span></span> <span data-ttu-id="49daf-215">Sedan initierar du anslutningen från Global.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="49daf-215">Then, initialize your connection from Global.aspx.cs.</span></span> <span data-ttu-id="49daf-216">Slutligen uppdatera hello överföringskod som du skapade tidigare i HomeController.cs tooactually skicka objekt tooa Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="49daf-216">Finally, update hello submission code you created earlier in HomeController.cs tooactually submit items tooa Service Bus queue.</span></span>

1. <span data-ttu-id="49daf-217">I **Solution Explorer**, högerklicka på **FrontendWebRole** (Högerklicka på hello projektet, inte hello rollen).</span><span class="sxs-lookup"><span data-stu-id="49daf-217">In **Solution Explorer**, right-click **FrontendWebRole** (right-click hello project, not hello role).</span></span> <span data-ttu-id="49daf-218">Klicka på **Lägg till** och sedan på **Klass**.</span><span class="sxs-lookup"><span data-stu-id="49daf-218">Click **Add**, and then click **Class**.</span></span>
2. <span data-ttu-id="49daf-219">Namnge hello klassen **QueueConnector.cs**.</span><span class="sxs-lookup"><span data-stu-id="49daf-219">Name hello class **QueueConnector.cs**.</span></span> <span data-ttu-id="49daf-220">Klicka på **Lägg till** toocreate hello-klassen.</span><span class="sxs-lookup"><span data-stu-id="49daf-220">Click **Add** toocreate hello class.</span></span>
3. <span data-ttu-id="49daf-221">Lägg nu till kod som kapslar in hello anslutningsinformationen och initierar hello anslutning tooa Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="49daf-221">Now, add code that encapsulates hello connection information and initializes hello connection tooa Service Bus queue.</span></span> <span data-ttu-id="49daf-222">Ersätt hello hela innehållet i QueueConnector.cs med följande kod hello och ange värden för `your Service Bus namespace` (namnet på ditt namnområde) och `yourKey`, vilket är hello **primärnyckel** du tidigare fick från hello Azure portalen.</span><span class="sxs-lookup"><span data-stu-id="49daf-222">Replace hello entire contents of QueueConnector.cs with hello following code, and enter values for `your Service Bus namespace` (your namespace name) and `yourKey`, which is hello **primary key** you previously obtained from hello Azure portal.</span></span>
   
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
   
           // Obtain these values from hello portal.
           public const string Namespace = "your Service Bus namespace";
   
           // hello name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create hello namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http toobe friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create hello namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create hello queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client toohello queue.
               var messagingFactory = MessagingFactory.Create(
                   namespaceManager.Address,
                   namespaceManager.Settings.TokenProvider);
               OrdersQueueClient = messagingFactory.CreateQueueClient(
                   "OrdersQueue");
           }
       }
   }
   ```
4. <span data-ttu-id="49daf-223">Se nu till att din **initiera**-metod anropas.</span><span class="sxs-lookup"><span data-stu-id="49daf-223">Now, ensure that your **Initialize** method gets called.</span></span> <span data-ttu-id="49daf-224">Dubbelklicka på **Global.asax\Global.asax.cs** i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="49daf-224">In **Solution Explorer**, double-click **Global.asax\Global.asax.cs**.</span></span>
5. <span data-ttu-id="49daf-225">Lägg till följande kodrad i slutet av hello av hello hello **Application_Start** metod.</span><span class="sxs-lookup"><span data-stu-id="49daf-225">Add hello following line of code at hello end of hello **Application_Start** method.</span></span>
   
   ```csharp
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. <span data-ttu-id="49daf-226">Slutligen uppdatera hello web-kod som du skapade tidigare, för att skicka objekt toohello kön.</span><span class="sxs-lookup"><span data-stu-id="49daf-226">Finally, update hello web code you created earlier, to submit items toohello queue.</span></span> <span data-ttu-id="49daf-227">I **Solution Explorer** dubbelklickar du på **Controllers\HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="49daf-227">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span>
7. <span data-ttu-id="49daf-228">Uppdatera hello `Submit()` metod (hello överlagring som inte tar några parametrar) enligt följande tooget hello-meddelande räkna hello kön.</span><span class="sxs-lookup"><span data-stu-id="49daf-228">Update hello `Submit()` method (hello overload that takes no parameters) as follows tooget hello message count for hello queue.</span></span>
   
   ```csharp
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you tooperform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get hello queue, and obtain hello message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. <span data-ttu-id="49daf-229">Uppdatera hello `Submit(OnlineOrder order)` metod (hello överlagring som tar några parametrar) enligt följande toosubmit ordning information toohello kön.</span><span class="sxs-lookup"><span data-stu-id="49daf-229">Update hello `Submit(OnlineOrder order)` method (hello overload that takes one parameter) as follows toosubmit order information toohello queue.</span></span>
   
   ```csharp
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from hello order.
           var message = new BrokeredMessage(order);
   
           // Submit hello order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. <span data-ttu-id="49daf-230">Du kan nu köra hello programmet igen.</span><span class="sxs-lookup"><span data-stu-id="49daf-230">You can now run hello application again.</span></span> <span data-ttu-id="49daf-231">Varje gång du skickar en order, ökar antal för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="49daf-231">Each time you submit an order, hello message count increases.</span></span>
   
   ![][18]

## <a name="create-hello-worker-role"></a><span data-ttu-id="49daf-232">Skapa hello worker-rollen</span><span class="sxs-lookup"><span data-stu-id="49daf-232">Create hello worker role</span></span>
<span data-ttu-id="49daf-233">Nu ska du skapa hello arbetsroll som behandlar orderöverföringen hello.</span><span class="sxs-lookup"><span data-stu-id="49daf-233">You will now create hello worker role that processes hello order submissions.</span></span> <span data-ttu-id="49daf-234">Det här exemplet används hello **Arbetsroll med Service Bus-kö** projektmall för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49daf-234">This example uses hello **Worker Role with Service Bus Queue** Visual Studio project template.</span></span> <span data-ttu-id="49daf-235">Du har redan fått hello krävs autentiseringsuppgifter från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="49daf-235">You already obtained hello required credentials from hello portal.</span></span>

1. <span data-ttu-id="49daf-236">Kontrollera att du har anslutit Visual Studio tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="49daf-236">Make sure you have connected Visual Studio tooyour Azure account.</span></span>
2. <span data-ttu-id="49daf-237">I Visual Studio i **Solution Explorer** högerklickar du på den **roller** mapp under hello **MultiTierApp** projekt.</span><span class="sxs-lookup"><span data-stu-id="49daf-237">In Visual Studio, in **Solution Explorer** right-click the **Roles** folder under hello **MultiTierApp** project.</span></span>
3. <span data-ttu-id="49daf-238">Klicka på **Lägg till** och sedan på **Nytt arbetsrollprojekt**.</span><span class="sxs-lookup"><span data-stu-id="49daf-238">Click **Add**, and then click **New Worker Role Project**.</span></span> <span data-ttu-id="49daf-239">Hej **Lägg till nytt Rollprojekt** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="49daf-239">hello **Add New Role Project** dialog box appears.</span></span>
   
   ![][26]
4. <span data-ttu-id="49daf-240">I hello **Lägg till nytt Rollprojekt** dialogrutan klickar du på **Arbetsroll med Service Bus-kö**.</span><span class="sxs-lookup"><span data-stu-id="49daf-240">In hello **Add New Role Project** dialog box, click **Worker Role with Service Bus Queue**.</span></span>
   
   ![][23]
5. <span data-ttu-id="49daf-241">I hello **namn** rutan, namnet hello projekt **OrderProcessingRole**.</span><span class="sxs-lookup"><span data-stu-id="49daf-241">In hello **Name** box, name hello project **OrderProcessingRole**.</span></span> <span data-ttu-id="49daf-242">Klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="49daf-242">Then click **Add**.</span></span>
6. <span data-ttu-id="49daf-243">Kopiera hello anslutningssträng som du hämtade i steg 9 av hello ”skapa ett namnområde för Service Bus” avsnittet toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="49daf-243">Copy hello connection string that you obtained in step 9 of hello "Create a Service Bus namespace" section toohello clipboard.</span></span>
7. <span data-ttu-id="49daf-244">I **Solution Explorer**, högerklicka på hello **OrderProcessingRole** du skapade i steg 5 (se till att du högerklickar på **OrderProcessingRole** under **Roller**, och inte hello klassen).</span><span class="sxs-lookup"><span data-stu-id="49daf-244">In **Solution Explorer**, right-click hello **OrderProcessingRole** you created in step 5 (make sure that you right-click **OrderProcessingRole** under **Roles**, and not hello class).</span></span> <span data-ttu-id="49daf-245">Klicka sedan på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="49daf-245">Then click **Properties**.</span></span>
8. <span data-ttu-id="49daf-246">På hello **inställningar** för hello **egenskaper** dialogrutan klickar du inuti hello **värdet** för **Microsoft.ServiceBus.ConnectionString**, och klistra in hello slutpunktsvärdet som du kopierade i steg 6.</span><span class="sxs-lookup"><span data-stu-id="49daf-246">On hello **Settings** tab of hello **Properties** dialog box, click inside hello **Value** box for **Microsoft.ServiceBus.ConnectionString**, and then paste hello endpoint value you copied in step 6.</span></span>
   
   ![][25]
9. <span data-ttu-id="49daf-247">Skapa en **OnlineOrder** klassen toorepresent hello order som du behandlar dem från hello kö.</span><span class="sxs-lookup"><span data-stu-id="49daf-247">Create an **OnlineOrder** class toorepresent hello orders as you process them from hello queue.</span></span> <span data-ttu-id="49daf-248">Du kan återanvända en klass som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="49daf-248">You can reuse a class you have already created.</span></span> <span data-ttu-id="49daf-249">I **Solution Explorer**, högerklicka på hello **OrderProcessingRole** klass (Högerklicka på hello klassikonen, inte hello rollen).</span><span class="sxs-lookup"><span data-stu-id="49daf-249">In **Solution Explorer**, right-click hello **OrderProcessingRole** class (right-click hello class icon, not hello role).</span></span> <span data-ttu-id="49daf-250">Klicka på **Lägg till** och sedan på **Befintligt objekt**.</span><span class="sxs-lookup"><span data-stu-id="49daf-250">Click **Add**, then click **Existing Item**.</span></span>
10. <span data-ttu-id="49daf-251">Bläddra toohello undermapp för **FrontendWebRole\Models**, och dubbelklicka sedan på **OnlineOrder.cs** tooadd den toothis projekt.</span><span class="sxs-lookup"><span data-stu-id="49daf-251">Browse toohello subfolder for **FrontendWebRole\Models**, and then double-click **OnlineOrder.cs** tooadd it toothis project.</span></span>
11. <span data-ttu-id="49daf-252">I **WorkerRole.cs**, ändra hello värdet på hello **könamn** variabel från `"ProcessingQueue"` för`"OrdersQueue"` enligt hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="49daf-252">In **WorkerRole.cs**, change hello value of hello **QueueName** variable from `"ProcessingQueue"` too`"OrdersQueue"` as shown in hello following code.</span></span>
    
    ```csharp
    // hello name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. <span data-ttu-id="49daf-253">Lägg till hello följande med instruktionen hello överst i hello WorkerRole.cs fil.</span><span class="sxs-lookup"><span data-stu-id="49daf-253">Add hello following using statement at hello top of hello WorkerRole.cs file.</span></span>
    
    ```csharp
    using FrontendWebRole.Models;
    ```
13. <span data-ttu-id="49daf-254">I hello `Run()` funktionen inuti hello `OnMessage()` anropa, ersätter hello innehållet i hello `try` -sats med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="49daf-254">In hello `Run()` function, inside hello `OnMessage()` call, replace hello contents of hello `try` clause with hello following code.</span></span>
    
    ```csharp
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View hello message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. <span data-ttu-id="49daf-255">Du har slutfört hello program.</span><span class="sxs-lookup"><span data-stu-id="49daf-255">You have completed hello application.</span></span> <span data-ttu-id="49daf-256">Du kan testa hello fullständiga programmet genom att högerklicka på hello MultiTierApp-projektet i Solution Explorer välja **Set as Startup Project**, och sedan trycka på F5.</span><span class="sxs-lookup"><span data-stu-id="49daf-256">You can test hello full application by right-clicking hello MultiTierApp project in Solution Explorer, selecting **Set as Startup Project**, and then pressing F5.</span></span> <span data-ttu-id="49daf-257">Observera att antalet meddelanden inte ökar, eftersom hello arbetsrollen behandlar objekt från kön hello och markerar dem som slutförd.</span><span class="sxs-lookup"><span data-stu-id="49daf-257">Note that the message count does not increment, because hello worker role processes items from hello queue and marks them as complete.</span></span> <span data-ttu-id="49daf-258">Du kan se hello spårningsutdata från arbetsrollen genom att visa hello Azure Compute Emulator UI.</span><span class="sxs-lookup"><span data-stu-id="49daf-258">You can see hello trace output of your worker role by viewing hello Azure Compute Emulator UI.</span></span> <span data-ttu-id="49daf-259">Du kan göra detta genom att högerklicka hello emulatorikonen i hello meddelandefältet i Aktivitetsfältet och välja **visa Compute Emulator UI**.</span><span class="sxs-lookup"><span data-stu-id="49daf-259">You can do this by right-clicking hello emulator icon in hello notification area of your taskbar and selecting **Show Compute Emulator UI**.</span></span>
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a><span data-ttu-id="49daf-260">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49daf-260">Next steps</span></span>
<span data-ttu-id="49daf-261">toolearn mer om Service Bus finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="49daf-261">toolearn more about Service Bus, see hello following resources:</span></span>  

* <span data-ttu-id="49daf-262">[Dokumentation för Azure Service Bus][sbdocs]</span><span class="sxs-lookup"><span data-stu-id="49daf-262">[Azure Service Bus documentation][sbdocs]</span></span>  
* <span data-ttu-id="49daf-263">[Tjänstesida för Service Bus][sbacom]</span><span class="sxs-lookup"><span data-stu-id="49daf-263">[Service Bus service page][sbacom]</span></span>  
* <span data-ttu-id="49daf-264">[Hur tooUse Service Bus-köer][sbacomqhowto]</span><span class="sxs-lookup"><span data-stu-id="49daf-264">[How tooUse Service Bus Queues][sbacomqhowto]</span></span>  

<span data-ttu-id="49daf-265">toolearn mer om flernivåscenarier, se:</span><span class="sxs-lookup"><span data-stu-id="49daf-265">toolearn more about multi-tier scenarios, see:</span></span>  

* <span data-ttu-id="49daf-266">[.NET-flernivåapp med hjälp av lagringstabeller, köer och blob-objekt][mutitierstorage]</span><span class="sxs-lookup"><span data-stu-id="49daf-266">[.NET Multi-Tier Application Using Storage Tables, Queues, and Blobs][mutitierstorage]</span></span>  

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
