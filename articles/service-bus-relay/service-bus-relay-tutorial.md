---
title: aaaAzure Service Bus WCF Relay kursen | Microsoft Docs
description: "Skapa en Service Bus-klient och en tjänst med hjälp av WCF Relay."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 53dfd236-97f1-4778-b376-be91aa14b842
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: sethm
ms.openlocfilehash: 78cd52ef51e9fcfcda2f13ec54bde3af50d76476
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-wcf-relay-tutorial"></a><span data-ttu-id="354bb-103">Azure vidarebefordrande WCF-självstudier</span><span class="sxs-lookup"><span data-stu-id="354bb-103">Azure WCF Relay tutorial</span></span>

<span data-ttu-id="354bb-104">Den här självstudiekursen beskriver hur toobuild en enkel WCF-relä klientprogram och en tjänst med Azure Relay.</span><span class="sxs-lookup"><span data-stu-id="354bb-104">This tutorial describes how toobuild a simple WCF Relay client application and service using Azure Relay.</span></span> <span data-ttu-id="354bb-105">För en liknande självstudiekurs som använder [Service Bus-meddelanden](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), se [komma igång med Service Bus-köer](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="354bb-105">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="354bb-106">Gå igenom den här kursen får du förstå hello steg som är nödvändiga toocreate vidarebefordrande WCF-klienten och tjänsten program.</span><span class="sxs-lookup"><span data-stu-id="354bb-106">Working through this tutorial gives you an understanding of hello steps that are required toocreate a WCF Relay client and service application.</span></span> <span data-ttu-id="354bb-107">En tjänst är en konstruktion som Exponerar en eller flera slutpunkter som Exponerar en eller flera tjänsteåtgärder som deras ursprungliga motsvarigheter i WCF.</span><span class="sxs-lookup"><span data-stu-id="354bb-107">Like their original WCF counterparts, a service is a construct that exposes one or more endpoints, each of which exposes one or more service operations.</span></span> <span data-ttu-id="354bb-108">hello-slutpunkten för en tjänst anger en adress där tjänsten hello hittar du en bindning som innehåller hello information som en klient måste kommunicera med hello-tjänsten och ett kontrakt som definierar hello funktionalitet som tillhandahålls av hello service tooits klienter.</span><span class="sxs-lookup"><span data-stu-id="354bb-108">hello endpoint of a service specifies an address where hello service can be found, a binding that contains hello information that a client must communicate with hello service, and a contract that defines hello functionality provided by hello service tooits clients.</span></span> <span data-ttu-id="354bb-109">hello största skillnaden mellan WCF- och WCF Relay är att hello slutpunkten exponeras i hello molnet i stället för lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="354bb-109">hello main difference between WCF and WCF Relay is that hello endpoint is exposed in hello cloud instead of locally on your computer.</span></span>

<span data-ttu-id="354bb-110">När du arbetar med hello sekvens av rubriker i den här kursen har du en aktiv tjänst och en klient som kan anropa hello åtgärder av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-110">After you work through hello sequence of topics in this tutorial, you will have a running service, and a client that can invoke hello operations of hello service.</span></span> <span data-ttu-id="354bb-111">hello det första avsnittet beskriver hur tooset ett konto.</span><span class="sxs-lookup"><span data-stu-id="354bb-111">hello first topic describes how tooset up an account.</span></span> <span data-ttu-id="354bb-112">hello nästa steg beskriver hur toodefine en tjänst som använder ett kontrakt, hur tooimplement hello tjänsten och hur tooconfigure hello tjänsten i kod.</span><span class="sxs-lookup"><span data-stu-id="354bb-112">hello next steps describe how toodefine a service that uses a contract, how tooimplement hello service, and how tooconfigure hello service in code.</span></span> <span data-ttu-id="354bb-113">Här beskrivs också hur toohost och kör hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-113">They also describe how toohost and run hello service.</span></span> <span data-ttu-id="354bb-114">hello tjänsten som skapas är själva värdbaserade och hello klienten och tjänsten körs på hello samma dator.</span><span class="sxs-lookup"><span data-stu-id="354bb-114">hello service that is created is self-hosted and hello client and service run on hello same computer.</span></span> <span data-ttu-id="354bb-115">Du kan konfigurera hello tjänsten med hjälp av koden eller en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="354bb-115">You can configure hello service by using either code or a configuration file.</span></span>

<span data-ttu-id="354bb-116">hello de tre sista stegen beskriver hur toocreate ett klientprogram, konfigurera hello klientprogrammet och skapar och använder en klient som kan komma åt hello funktionerna i hello värden.</span><span class="sxs-lookup"><span data-stu-id="354bb-116">hello final three steps describe how toocreate a client application, configure hello client application, and create and use a client that can access hello functionality of hello host.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="354bb-117">Krav</span><span class="sxs-lookup"><span data-stu-id="354bb-117">Prerequisites</span></span>

<span data-ttu-id="354bb-118">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="354bb-118">toocomplete this tutorial, you'll need hello following:</span></span>

* <span data-ttu-id="354bb-119">[Microsoft Visual Studio 2015 eller senare](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="354bb-119">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="354bb-120">Den här självstudiekursen använder Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="354bb-120">This tutorial uses Visual Studio 2017.</span></span>
* <span data-ttu-id="354bb-121">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="354bb-121">An active Azure account.</span></span> <span data-ttu-id="354bb-122">Om du inte har något konto kan du skapa ett utan kostnad på ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="354bb-122">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="354bb-123">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="354bb-123">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-a-service-namespace"></a><span data-ttu-id="354bb-124">Skapa ett namnområde för tjänsten</span><span class="sxs-lookup"><span data-stu-id="354bb-124">Create a service namespace</span></span>

<span data-ttu-id="354bb-125">hello första steget är toocreate ett namnområde och tooobtain en [delade signatur åtkomst (SAS)](../service-bus-messaging/service-bus-sas.md) nyckel.</span><span class="sxs-lookup"><span data-stu-id="354bb-125">hello first step is toocreate a namespace, and tooobtain a [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) key.</span></span> <span data-ttu-id="354bb-126">Ett namnområde ger en programgräns för varje program som exponeras via hello vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-126">A namespace provides an application boundary for each application exposed through hello relay service.</span></span> <span data-ttu-id="354bb-127">SAS-nyckeln genereras automatiskt av hello systemet när ett namnområde för tjänsten har skapats.</span><span class="sxs-lookup"><span data-stu-id="354bb-127">A SAS key is automatically generated by hello system when a service namespace is created.</span></span> <span data-ttu-id="354bb-128">hello kombinationen av namnområdet för tjänsten och SAS-nyckeln ger hello autentiseringsuppgifter för Azure tooauthenticate åtkomst tooan program.</span><span class="sxs-lookup"><span data-stu-id="354bb-128">hello combination of service namespace and SAS key provides hello credentials for Azure tooauthenticate access tooan application.</span></span> <span data-ttu-id="354bb-129">Följ hello [anvisningarna här](relay-create-namespace-portal.md) toocreate en Relay-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="354bb-129">Follow hello [instructions here](relay-create-namespace-portal.md) toocreate a Relay namespace.</span></span>

## <a name="define-a-wcf-service-contract"></a><span data-ttu-id="354bb-130">Definiera ett WCF-tjänstekontrakt</span><span class="sxs-lookup"><span data-stu-id="354bb-130">Define a WCF service contract</span></span>

<span data-ttu-id="354bb-131">hello tjänstekontraktet anger vilka åtgärder (hello webbserviceterminologin för metoder eller funktioner) hello tjänsten stöder.</span><span class="sxs-lookup"><span data-stu-id="354bb-131">hello service contract specifies what operations (hello web service terminology for methods or functions) hello service supports.</span></span> <span data-ttu-id="354bb-132">Kontrakt skapas genom att definiera ett gränssnitt för C++, C# eller Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="354bb-132">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="354bb-133">Varje metod i gränssnittet hello motsvarar tooa specifik tjänsteåtgärd.</span><span class="sxs-lookup"><span data-stu-id="354bb-133">Each method in hello interface corresponds tooa specific service operation.</span></span> <span data-ttu-id="354bb-134">Varje gränssnitt måste ha hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) använda attributet tooit och varje åtgärd måste ha hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) tooit-attribut som används.</span><span class="sxs-lookup"><span data-stu-id="354bb-134">Each interface must have hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute applied tooit, and each operation must have hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute applied tooit.</span></span> <span data-ttu-id="354bb-135">Om en metod i ett gränssnitt som har hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) har ingen hälsningspaket [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attributet, inte den metoden är exponerad.</span><span class="sxs-lookup"><span data-stu-id="354bb-135">If a method in an interface that has hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute does not have hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute, that method is not exposed.</span></span> <span data-ttu-id="354bb-136">hello-koden för dessa uppgifter finns i hello-exemplet som följer hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="354bb-136">hello code for these tasks is provided in hello example following hello procedure.</span></span> <span data-ttu-id="354bb-137">En utförlig beskrivning av kontrakt och tjänster finns [utforma och implementera tjänster](https://msdn.microsoft.com/library/ms729746.aspx) i hello WCF-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="354bb-137">For a larger discussion of contracts and services, see [Designing and Implementing Services](https://msdn.microsoft.com/library/ms729746.aspx) in hello WCF documentation.</span></span>

### <a name="create-a-relay-contract-with-an-interface"></a><span data-ttu-id="354bb-138">Skapa en relay-kontrakt med ett gränssnitt</span><span class="sxs-lookup"><span data-stu-id="354bb-138">Create a relay contract with an interface</span></span>

1. <span data-ttu-id="354bb-139">Öppna Visual Studio som administratör genom att högerklicka på programmet hello i hello **starta** menyn och välja **kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="354bb-139">Open Visual Studio as an administrator by right-clicking hello program in hello **Start** menu and selecting **Run as administrator**.</span></span>
2. <span data-ttu-id="354bb-140">Skapa ett nytt konsolappsrojekt.</span><span class="sxs-lookup"><span data-stu-id="354bb-140">Create a new console application project.</span></span> <span data-ttu-id="354bb-141">Klicka på hello **filen** -menyn och välj **ny**, klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="354bb-141">Click hello **File** menu and select **New**, then click **Project**.</span></span> <span data-ttu-id="354bb-142">I hello **nytt projekt** dialogrutan klickar du på **Visual C#** (om **Visual C#** inte visas, tittar du under **andra språk**).</span><span class="sxs-lookup"><span data-stu-id="354bb-142">In hello **New Project** dialog, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**).</span></span> <span data-ttu-id="354bb-143">Klicka på hello **Konsolapp (.NET Framework)** mall, och ger den namnet **EchoService**.</span><span class="sxs-lookup"><span data-stu-id="354bb-143">Click hello **Console App (.NET Framework)** template, and name it **EchoService**.</span></span> <span data-ttu-id="354bb-144">Klicka på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="354bb-144">Click **OK** toocreate hello project.</span></span>

    ![][2]

3. <span data-ttu-id="354bb-145">Installera hello Service Bus NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="354bb-145">Install hello Service Bus NuGet package.</span></span> <span data-ttu-id="354bb-146">Det här paketet lägger automatiskt till referenser toohello Service Bus-bibliotek, samt hello WCF **System.ServiceModel**.</span><span class="sxs-lookup"><span data-stu-id="354bb-146">This package automatically adds references toohello Service Bus libraries, as well as hello WCF **System.ServiceModel**.</span></span> <span data-ttu-id="354bb-147">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) hello-namnområde som du kan använda tooprogrammatically åtkomst hello grundläggande funktioner i WCF.</span><span class="sxs-lookup"><span data-stu-id="354bb-147">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is hello namespace that enables you tooprogrammatically access hello basic features of WCF.</span></span> <span data-ttu-id="354bb-148">Service Bus använder många av hello objekt och attribut för WCF toodefine tjänstekontrakt.</span><span class="sxs-lookup"><span data-stu-id="354bb-148">Service Bus uses many of hello objects and attributes of WCF toodefine service contracts.</span></span>

    <span data-ttu-id="354bb-149">Högerklicka på hello projektet i Solution Explorer och klicka sedan på **hantera NuGet-paket...** . Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="354bb-149">In Solution Explorer, right-click hello project, and then click **Manage NuGet Packages...**. Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="354bb-150">Se till att hello projektnamnet är markerat i hello **versioner** rutan.</span><span class="sxs-lookup"><span data-stu-id="354bb-150">Ensure that hello project name is selected in hello **Version(s)** box.</span></span> <span data-ttu-id="354bb-151">Klicka på **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="354bb-151">Click **Install**, and accept hello terms of use.</span></span>

    ![][3]
4. <span data-ttu-id="354bb-152">I Solution Explorer dubbelklickar du på hello Program.cs-filen tooopen den i Redigeraren för hello, om den inte redan är öppen.</span><span class="sxs-lookup"><span data-stu-id="354bb-152">In Solution Explorer, double-click hello Program.cs file tooopen it in hello editor, if it is not already open.</span></span>
5. <span data-ttu-id="354bb-153">Lägg till hello följande using-instruktioner överst hello i hello-filen:</span><span class="sxs-lookup"><span data-stu-id="354bb-153">Add hello following using statements at hello top of hello file:</span></span>

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. <span data-ttu-id="354bb-154">Ändra hello namnområdesnamnet från standardnamnet **EchoService** för**Microsoft.ServiceBus.Samples**.</span><span class="sxs-lookup"><span data-stu-id="354bb-154">Change hello namespace name from its default name of **EchoService** too**Microsoft.ServiceBus.Samples**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="354bb-155">Den här kursen använder hello C#-namnområdet **Microsoft.ServiceBus.Samples**, som är hello namnområdet för hello kontrakt-baserade hanterad typ som används i hello konfigurationsfilen i hello [konfigurera hello WCF-klient](#configure-the-wcf-client) steg.</span><span class="sxs-lookup"><span data-stu-id="354bb-155">This tutorial uses hello C# namespace **Microsoft.ServiceBus.Samples**, which is hello namespace of hello contract-based managed type that is used in hello configuration file in hello [Configure hello WCF client](#configure-the-wcf-client) step.</span></span> <span data-ttu-id="354bb-156">Du kan ange vilken namnområden du vill när du skapar det här exemplet; hello kursen fungerar inte om du ändrar hello namnområden hello kontrakt- och därför i hello programmets konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="354bb-156">You can specify any namespace you want when you build this sample; however, hello tutorial will not work unless you then modify hello namespaces of hello contract and service accordingly, in hello application configuration file.</span></span> <span data-ttu-id="354bb-157">hello namnområdet som angetts i hello App.config-filen måste vara hello samma som hello namnområdet som angetts i C#-filerna.</span><span class="sxs-lookup"><span data-stu-id="354bb-157">hello namespace specified in hello App.config file must be hello same as hello namespace specified in your C# files.</span></span>
   >
   >
7. <span data-ttu-id="354bb-158">Direkt efter hello `Microsoft.ServiceBus.Samples` namnområdesdeklaration, men i hello-namnområdet, definierar du ett nytt gränssnitt med namnet `IEchoContract` och tillämpa hello `ServiceContractAttribute` attributet toohello gränssnittet med ett namnområdesvärde på `http://samples.microsoft.com/ServiceModel/Relay/`.</span><span class="sxs-lookup"><span data-stu-id="354bb-158">Directly after hello `Microsoft.ServiceBus.Samples` namespace declaration, but within hello namespace, define a new interface named `IEchoContract` and apply hello `ServiceContractAttribute` attribute toohello interface with a namespace value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="354bb-159">Hej namnområdesvärdet skiljer sig från hello-namnområde som du använder under hela hello omfånget för din kod.</span><span class="sxs-lookup"><span data-stu-id="354bb-159">hello namespace value differs from hello namespace that you use throughout hello scope of your code.</span></span> <span data-ttu-id="354bb-160">I stället används hello namnområdesvärdet som en unik identifierare för det här kontraktet.</span><span class="sxs-lookup"><span data-stu-id="354bb-160">Instead, hello namespace value is used as a unique identifier for this contract.</span></span> <span data-ttu-id="354bb-161">Att ange hello namnområdet uttryckligen förhindrar att hello standardvärdet för namnområdet som läggs till toohello kontraktnamn.</span><span class="sxs-lookup"><span data-stu-id="354bb-161">Specifying hello namespace explicitly prevents hello default namespace value from being added toohello contract name.</span></span> <span data-ttu-id="354bb-162">Klistra in följande kod efter hello namnområdesdeklaration hello:</span><span class="sxs-lookup"><span data-stu-id="354bb-162">Paste hello following code after hello namespace declaration:</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="354bb-163">Hello namnområdet för tjänstekontraktet innehåller vanligtvis ett namngivningsschema som inkluderar information om versionen.</span><span class="sxs-lookup"><span data-stu-id="354bb-163">Typically, hello service contract namespace contains a naming scheme that includes version information.</span></span> <span data-ttu-id="354bb-164">Med versionsinformation i namnområdet för tjänstekontraktet hello kan tjänster tooisolate större ändringar genom att definiera ett nytt tjänstkontrakt med ett nytt namnområde och exponera det på en ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="354bb-164">Including version information in hello service contract namespace enables services tooisolate major changes by defining a new service contract with a new namespace and exposing it on a new endpoint.</span></span> <span data-ttu-id="354bb-165">På så sätt kan klienter fortsätta toouse hello gamla tjänstkontraktet utan toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="354bb-165">In this manner, clients can continue toouse hello old service contract without having toobe updated.</span></span> <span data-ttu-id="354bb-166">Versionsinformation kan bestå av ett datum eller ett build-nummer.</span><span class="sxs-lookup"><span data-stu-id="354bb-166">Version information can consist of a date or a build number.</span></span> <span data-ttu-id="354bb-167">Mer information finns i [Versionshantering för tjänster](http://go.microsoft.com/fwlink/?LinkID=180498).</span><span class="sxs-lookup"><span data-stu-id="354bb-167">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="354bb-168">För hello i den här kursen är innehåller hello naming schemat för namnområdet för tjänstekontraktet hello inte versionsinformation.</span><span class="sxs-lookup"><span data-stu-id="354bb-168">For hello purposes of this tutorial, hello naming scheme of hello service contract namespace does not contain version information.</span></span>
   >
   >
8. <span data-ttu-id="354bb-169">Inom hello `IEchoContract` gränssnitt, deklarera en metod för hello gång hello `IEchoContract` kontrakt visar i hello gränssnitt och tillämpa hello `OperationContractAttribute` attributet toohello metod som du vill tooexpose som en del av hello offentliga WCF-relä minimera, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="354bb-169">Within hello `IEchoContract` interface, declare a method for hello single operation hello `IEchoContract` contract exposes in hello interface and apply hello `OperationContractAttribute` attribute toohello method that you want tooexpose as part of hello public WCF Relay contract, as follows:</span></span>

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. <span data-ttu-id="354bb-170">Direkt efter hello `IEchoContract` deklarerar du en kanal som ärver från både `IEchoContract` och även toohello `IClientChannel` gränssnitt som visas här:</span><span class="sxs-lookup"><span data-stu-id="354bb-170">Directly after hello `IEchoContract` interface definition, declare a channel that inherits from both `IEchoContract` and also toohello `IClientChannel` interface, as shown here:</span></span>

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    <span data-ttu-id="354bb-171">En kanal är hello WCF-objekt via vilken hello värden och klienten skickar information tooeach andra.</span><span class="sxs-lookup"><span data-stu-id="354bb-171">A channel is hello WCF object through which hello host and client pass information tooeach other.</span></span> <span data-ttu-id="354bb-172">Senare kan skriva du kod mot hello kanal tooecho information mellan två hello-program.</span><span class="sxs-lookup"><span data-stu-id="354bb-172">Later, you will write code against hello channel tooecho information between hello two applications.</span></span>
10. <span data-ttu-id="354bb-173">Från hello **skapa** -menyn klickar du på **skapa lösning** eller tryck på **Ctrl + Skift + B** tooconfirm hello noggrannhet arbete hittills.</span><span class="sxs-lookup"><span data-stu-id="354bb-173">From hello **Build** menu, click **Build Solution** or press **Ctrl+Shift+B** tooconfirm hello accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="354bb-174">Exempel</span><span class="sxs-lookup"><span data-stu-id="354bb-174">Example</span></span>

<span data-ttu-id="354bb-175">hello följande kod visar ett grundläggande gränssnitt som definierar ett vidarebefordrande WCF-kontrakt.</span><span class="sxs-lookup"><span data-stu-id="354bb-175">hello following code shows a basic interface that defines a WCF Relay contract.</span></span>

```csharp
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

<span data-ttu-id="354bb-176">Nu när hello gränssnittet har skapats kan implementera du hello-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="354bb-176">Now that hello interface is created, you can implement hello interface.</span></span>

## <a name="implement-hello-wcf-contract"></a><span data-ttu-id="354bb-177">Implementera hello WCF-tjänstekontrakt</span><span class="sxs-lookup"><span data-stu-id="354bb-177">Implement hello WCF contract</span></span>

<span data-ttu-id="354bb-178">Skapa ett Azure relay måste du först skapa hello kontrakt som definieras med hjälp av ett gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="354bb-178">Creating an Azure relay requires that you first create hello contract, which is defined by using an interface.</span></span> <span data-ttu-id="354bb-179">Mer information om hur du skapar hello-gränssnittet finns hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="354bb-179">For more information about creating hello interface, see hello previous step.</span></span> <span data-ttu-id="354bb-180">hello nästa steg är tooimplement hello gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="354bb-180">hello next step is tooimplement hello interface.</span></span> <span data-ttu-id="354bb-181">Detta innebär att du skapar en klass som heter `EchoService` som implementerar hello användardefinierade `IEchoContract` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="354bb-181">This involves creating a class named `EchoService` that implements hello user-defined `IEchoContract` interface.</span></span> <span data-ttu-id="354bb-182">När du implementerar gränssnittet hello, konfigurerar du sedan hello-gränssnittet genom att använda konfigurationsfilen App.config.</span><span class="sxs-lookup"><span data-stu-id="354bb-182">After you implement hello interface, you then configure hello interface using an App.config configuration file.</span></span> <span data-ttu-id="354bb-183">hello konfigurationsfilen innehåller information som behövs för hello program, till exempel hello namnet på hello tjänst, hello namn hello kontraktet och hello typ av protokoll som används toocommunicate med hello vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-183">hello configuration file contains necessary information for hello application, such as hello name of hello service, hello name of hello contract, and hello type of protocol that is used toocommunicate with hello relay service.</span></span> <span data-ttu-id="354bb-184">hello-kod som används för dessa uppgifter finns i hello-exemplet som följer hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="354bb-184">hello code used for these tasks is provided in hello example following hello procedure.</span></span> <span data-ttu-id="354bb-185">En mer allmän diskussion om hur tooimplement en tjänst minimera finns [implementera tjänstekontrakt](https://msdn.microsoft.com/library/ms733764.aspx) i hello WCF-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="354bb-185">For a more general discussion about how tooimplement a service contract, see [Implementing Service Contracts](https://msdn.microsoft.com/library/ms733764.aspx) in hello WCF documentation.</span></span>

1. <span data-ttu-id="354bb-186">Skapa en ny klass med namnet `EchoService` direkt efter hello definition av hello `IEchoContract` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="354bb-186">Create a new class named `EchoService` directly after hello definition of hello `IEchoContract` interface.</span></span> <span data-ttu-id="354bb-187">Hej `EchoService` klassen implementerar hello `IEchoContract` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="354bb-187">hello `EchoService` class implements hello `IEchoContract` interface.</span></span>

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    <span data-ttu-id="354bb-188">Liknande tooother gränssnittsimplementationer du kan implementera hello definition i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="354bb-188">Similar tooother interface implementations, you can implement hello definition in a different file.</span></span> <span data-ttu-id="354bb-189">Men i den här självstudien hello implementeringen i samma fil som gränssnittsdefinitionen hello och hello hello `Main` metod.</span><span class="sxs-lookup"><span data-stu-id="354bb-189">However, for this tutorial, hello implementation is located in hello same file as hello interface definition and hello `Main` method.</span></span>
2. <span data-ttu-id="354bb-190">Tillämpa hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attributet toohello `IEchoContract` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="354bb-190">Apply hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute toohello `IEchoContract` interface.</span></span> <span data-ttu-id="354bb-191">hello attributet anger hello tjänstens namn och namnområde.</span><span class="sxs-lookup"><span data-stu-id="354bb-191">hello attribute specifies hello service name and namespace.</span></span> <span data-ttu-id="354bb-192">När du har gjort det, hello `EchoService` klass visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="354bb-192">After doing so, hello `EchoService` class appears as follows:</span></span>

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. <span data-ttu-id="354bb-193">Implementera hello `Echo` metod som definieras i hello `IEchoContract` gränssnittet i hello `EchoService` klass.</span><span class="sxs-lookup"><span data-stu-id="354bb-193">Implement hello `Echo` method defined in hello `IEchoContract` interface in hello `EchoService` class.</span></span>

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. <span data-ttu-id="354bb-194">Klicka på **skapa**, klicka på **skapa lösning** tooconfirm hello korrektheten i ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="354bb-194">Click **Build**, then click **Build Solution** tooconfirm hello accuracy of your work.</span></span>

### <a name="define-hello-configuration-for-hello-service-host"></a><span data-ttu-id="354bb-195">Definiera hello konfiguration för hello tjänstvärden</span><span class="sxs-lookup"><span data-stu-id="354bb-195">Define hello configuration for hello service host</span></span>

1. <span data-ttu-id="354bb-196">hello-konfigurationsfilen är mycket lik tooa WCF-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="354bb-196">hello configuration file is very similar tooa WCF configuration file.</span></span> <span data-ttu-id="354bb-197">Den innehåller hello namnet på tjänsten, slutpunkten (det vill säga hello plats som Azure Relay visar för klienter och värdar toocommunicate med varandra) och hello bindning (hello typ av protokoll som används toocommunicate).</span><span class="sxs-lookup"><span data-stu-id="354bb-197">It includes hello service name, endpoint (that is, hello location that Azure Relay exposes for clients and hosts toocommunicate with each other), and hello binding (hello type of protocol that is used toocommunicate).</span></span> <span data-ttu-id="354bb-198">hello största skillnaden är att den här konfigurerade tjänstslutpunkten refererar tooa [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) bindning som inte är en del av hello .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="354bb-198">hello main difference is that this configured service endpoint refers tooa [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) binding, which is not part of hello .NET Framework.</span></span> <span data-ttu-id="354bb-199">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) är en av hello bindningar som definierats av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-199">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) is one of hello bindings defined by hello service.</span></span>
2. <span data-ttu-id="354bb-200">I **Solution Explorer**, dubbelklicka på hello App.config-filen tooopen i hello Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="354bb-200">In **Solution Explorer**, double-click hello App.config file tooopen it in hello Visual Studio editor.</span></span>
3. <span data-ttu-id="354bb-201">I hello `<appSettings>` element, Ersätt hello-platshållare med hello namnet på namnområdet för tjänsten och hello SAS-nyckel som du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="354bb-201">In hello `<appSettings>` element, replace hello placeholders with hello name of your service namespace, and hello SAS key that you copied in an earlier step.</span></span>
4. <span data-ttu-id="354bb-202">Inom hello `<system.serviceModel>` taggar, lägga till en `<services>` element.</span><span class="sxs-lookup"><span data-stu-id="354bb-202">Within hello `<system.serviceModel>` tags, add a `<services>` element.</span></span> <span data-ttu-id="354bb-203">Du kan definiera flera relay-program i en enda konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="354bb-203">You can define multiple relay applications in a single configuration file.</span></span> <span data-ttu-id="354bb-204">I den här självstudiekursen definieras dock bara en.</span><span class="sxs-lookup"><span data-stu-id="354bb-204">However, this tutorial defines only one.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. <span data-ttu-id="354bb-205">Inom hello `<services>` element, lägga till en `<service>` toodefine hello elementnamnet hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-205">Within hello `<services>` element, add a `<service>` element toodefine hello name of hello service.</span></span>

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. <span data-ttu-id="354bb-206">Inom hello `<service>` element definiera hello platsen för slutpunktskontraktet hello och även hello typen av bindning för hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="354bb-206">Within hello `<service>` element, define hello location of hello endpoint contract, and also hello type of binding for hello endpoint.</span></span>

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="354bb-207">hello slutpunkten definierar var hello klienten ska söka efter värdprogrammet hello.</span><span class="sxs-lookup"><span data-stu-id="354bb-207">hello endpoint defines where hello client will look for hello host application.</span></span> <span data-ttu-id="354bb-208">Senare använder hello kursen det här steget toocreate en URI som visar hello värd via Azure Relay.</span><span class="sxs-lookup"><span data-stu-id="354bb-208">Later, hello tutorial uses this step toocreate a URI that fully exposes hello host through Azure Relay.</span></span> <span data-ttu-id="354bb-209">hello bindningen deklarerar att vi använder TCP som hello protokollet toocommunicate med hello vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-209">hello binding declares that we are using TCP as hello protocol toocommunicate with hello relay service.</span></span>
7. <span data-ttu-id="354bb-210">Från hello **skapa** -menyn klickar du på **skapa lösning** tooconfirm hello korrektheten i ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="354bb-210">From hello **Build** menu, click **Build Solution** tooconfirm hello accuracy of your work.</span></span>

### <a name="example"></a><span data-ttu-id="354bb-211">Exempel</span><span class="sxs-lookup"><span data-stu-id="354bb-211">Example</span></span>

<span data-ttu-id="354bb-212">hello visar följande kod hello implementering av hello tjänstkontrakt.</span><span class="sxs-lookup"><span data-stu-id="354bb-212">hello following code shows hello implementation of hello service contract.</span></span>

```csharp
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

<span data-ttu-id="354bb-213">hello visar följande kod hello grundläggande formatet för hello App.config-fil som är associerad med hello tjänstvärden.</span><span class="sxs-lookup"><span data-stu-id="354bb-213">hello following code shows hello basic format of hello App.config file associated with hello service host.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-tooregister-with-hello-relay-service"></a><span data-ttu-id="354bb-214">Värd för och köra en grundläggande web service tooregister med hello vidarebefordrande tjänsten</span><span class="sxs-lookup"><span data-stu-id="354bb-214">Host and run a basic web service tooregister with hello relay service</span></span>

<span data-ttu-id="354bb-215">Det här steget beskriver hur toorun en Azure-relä service.</span><span class="sxs-lookup"><span data-stu-id="354bb-215">This step describes how toorun an Azure Relay service.</span></span>

### <a name="create-hello-relay-credentials"></a><span data-ttu-id="354bb-216">Skapa hello relay autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="354bb-216">Create hello relay credentials</span></span>

1. <span data-ttu-id="354bb-217">I `Main()`, skapa två variabler i vilka toostore hello namnområdet och SAS-nyckeln som läses från konsolfönstret hello hello.</span><span class="sxs-lookup"><span data-stu-id="354bb-217">In `Main()`, create two variables in which toostore hello namespace and hello SAS key that are read from hello console window.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    <span data-ttu-id="354bb-218">hello SAS-nyckeln kommer att användas senare tooaccess projektet.</span><span class="sxs-lookup"><span data-stu-id="354bb-218">hello SAS key will be used later tooaccess your project.</span></span> <span data-ttu-id="354bb-219">hello namnområdet skickas som en parameter för`CreateServiceUri` toocreate en URI för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-219">hello namespace is passed as a parameter too`CreateServiceUri` toocreate a service URI.</span></span>
2. <span data-ttu-id="354bb-220">Med hjälp av en [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) objekt, deklarera att du kommer att använda en SAS-nyckel som autentiseringstyp hello.</span><span class="sxs-lookup"><span data-stu-id="354bb-220">Using a [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) object, declare that you will be using a SAS key as hello credential type.</span></span> <span data-ttu-id="354bb-221">Lägg till följande kod direkt efter hello lade till i hello sista steget hello.</span><span class="sxs-lookup"><span data-stu-id="354bb-221">Add hello following code directly after hello code added in hello last step.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-hello-service"></a><span data-ttu-id="354bb-222">Skapa en basadress för hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="354bb-222">Create a base address for hello service</span></span>

<span data-ttu-id="354bb-223">När hello-kod som du lade till i hello sista steget, skapa en `Uri` hello service-instans för hello basadress.</span><span class="sxs-lookup"><span data-stu-id="354bb-223">After hello code you added in hello last step, create a `Uri` instance for hello base address of hello service.</span></span> <span data-ttu-id="354bb-224">Den här URI anger hello Service Bus-schemat, hello namnområde och hello sökväg för hello service-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="354bb-224">This URI specifies hello Service Bus scheme, hello namespace, and hello path of hello service interface.</span></span>

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

<span data-ttu-id="354bb-225">”sb” är en förkortning av hello Service Bus-schemat och anger att vi använder TCP som protokoll hello.</span><span class="sxs-lookup"><span data-stu-id="354bb-225">"sb" is an abbreviation for hello Service Bus scheme, and indicates that we are using TCP as hello protocol.</span></span> <span data-ttu-id="354bb-226">Detta har även tidigare angetts i konfigurationsfilen för hello när [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) har angetts som hello bindning.</span><span class="sxs-lookup"><span data-stu-id="354bb-226">This was also previously indicated in hello configuration file, when [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) was specified as hello binding.</span></span>

<span data-ttu-id="354bb-227">Den här självstudien hello URI är `sb://putServiceNamespaceHere.windows.net/EchoService`.</span><span class="sxs-lookup"><span data-stu-id="354bb-227">For this tutorial, hello URI is `sb://putServiceNamespaceHere.windows.net/EchoService`.</span></span>

### <a name="create-and-configure-hello-service-host"></a><span data-ttu-id="354bb-228">Skapa och konfigurera hello tjänstvärden</span><span class="sxs-lookup"><span data-stu-id="354bb-228">Create and configure hello service host</span></span>

1. <span data-ttu-id="354bb-229">Ange hello anslutningsläget för`AutoDetect`.</span><span class="sxs-lookup"><span data-stu-id="354bb-229">Set hello connectivity mode too`AutoDetect`.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    <span data-ttu-id="354bb-230">hello anslutningsläget beskriver hello protokollet hello tjänsten använder toocommunicate med hello vidarebefordrande tjänsten; antingen HTTP eller TCP.</span><span class="sxs-lookup"><span data-stu-id="354bb-230">hello connectivity mode describes hello protocol hello service uses toocommunicate with hello relay service; either HTTP or TCP.</span></span> <span data-ttu-id="354bb-231">Med hjälp av hello standardinställningen `AutoDetect`, hello om försöker tjänsten tooconnect tooAzure Relay över TCP, om den är tillgänglig och HTTP om TCP inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="354bb-231">Using hello default setting `AutoDetect`, hello service attempts tooconnect tooAzure Relay over TCP if it is available, and HTTP if TCP is not available.</span></span> <span data-ttu-id="354bb-232">Observera att detta skiljer sig från hello hello-protokolltjänsten anger för klientkommunikation.</span><span class="sxs-lookup"><span data-stu-id="354bb-232">Note that this differs from hello protocol hello service specifies for client communication.</span></span> <span data-ttu-id="354bb-233">Det här protokollet bestäms av hello bindningen som används.</span><span class="sxs-lookup"><span data-stu-id="354bb-233">That protocol is determined by hello binding used.</span></span> <span data-ttu-id="354bb-234">En tjänst kan till exempel använda hello [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) bindning som anger att dess slutpunkt kommunicerar med klienterna via HTTP.</span><span class="sxs-lookup"><span data-stu-id="354bb-234">For example, a service can use hello [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) binding, which specifies that its endpoint communicates with clients over HTTP.</span></span> <span data-ttu-id="354bb-235">Att samma tjänst skulle kunna ange **ConnectivityMode.AutoDetect** så att hello service kommunicerar med Azure Relay via TCP.</span><span class="sxs-lookup"><span data-stu-id="354bb-235">That same service could specify **ConnectivityMode.AutoDetect** so that hello service communicates with Azure Relay over TCP.</span></span>
2. <span data-ttu-id="354bb-236">Skapa hello tjänstvärden med hello URI skapade tidigare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="354bb-236">Create hello service host, using hello URI created earlier in this section.</span></span>

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    <span data-ttu-id="354bb-237">hello tjänstvärden är hello WCF-objekt som instantierar hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-237">hello service host is hello WCF object that instantiates hello service.</span></span> <span data-ttu-id="354bb-238">Här kan du skicka den hello typen tjänsten som du vill toocreate (en `EchoService` typ), och även toohello adress som du vill tooexpose hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-238">Here, you pass it hello type of service you want toocreate (an `EchoService` type), and also toohello address at which you want tooexpose hello service.</span></span>
3. <span data-ttu-id="354bb-239">Hello över hello Program.cs-filen lägger du till referenser för[System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) och [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).</span><span class="sxs-lookup"><span data-stu-id="354bb-239">At hello top of hello Program.cs file, add references too[System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) and [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).</span></span>

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. <span data-ttu-id="354bb-240">Tillbaka i `Main()`, konfigurera hello endpoint tooenable offentlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="354bb-240">Back in `Main()`, configure hello endpoint tooenable public access.</span></span>

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    <span data-ttu-id="354bb-241">Det här steget meddelar hello vidarebefordrande tjänsten att programmet kan hittas offentligt genom att undersöka hello ATOM-flödet för projektet.</span><span class="sxs-lookup"><span data-stu-id="354bb-241">This step informs hello relay service that your application can be found publicly by examining hello ATOM feed for your project.</span></span> <span data-ttu-id="354bb-242">Om du ställer in **DiscoveryType** för**privata**, en klient fortfarande att kunna tooaccess hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-242">If you set **DiscoveryType** too**private**, a client would still be able tooaccess hello service.</span></span> <span data-ttu-id="354bb-243">Hello-tjänsten skulle dock inte visas när den söker hello Relay namnområde.</span><span class="sxs-lookup"><span data-stu-id="354bb-243">However, hello service would not appear when it searches hello Relay namespace.</span></span> <span data-ttu-id="354bb-244">I stället hello klienten måste då tooknow hello slutpunktsökväg i förväg.</span><span class="sxs-lookup"><span data-stu-id="354bb-244">Instead, hello client would have tooknow hello endpoint path beforehand.</span></span>
5. <span data-ttu-id="354bb-245">Tillämpa hello service autentiseringsuppgifter toohello slutpunkter definieras i hello App.config-fil:</span><span class="sxs-lookup"><span data-stu-id="354bb-245">Apply hello service credentials toohello service endpoints defined in hello App.config file:</span></span>

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    <span data-ttu-id="354bb-246">Som anges i föregående steg i hello ha du deklarerat flera tjänster och slutpunkter i hello konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="354bb-246">As stated in hello previous step, you could have declared multiple services and endpoints in hello configuration file.</span></span> <span data-ttu-id="354bb-247">Om du har den här koden skulle passerar hello konfigurationsfil och sökning för varje slutpunkt toowhich ska den tillämpas dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="354bb-247">If you had, this code would traverse hello configuration file and search for every endpoint toowhich it should apply your credentials.</span></span> <span data-ttu-id="354bb-248">För den här självstudiekursen har konfigurationsfilen hello dock endast en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="354bb-248">However, for this tutorial, hello configuration file has only one endpoint.</span></span>

### <a name="open-hello-service-host"></a><span data-ttu-id="354bb-249">Öppna hello tjänstvärden</span><span class="sxs-lookup"><span data-stu-id="354bb-249">Open hello service host</span></span>

1. <span data-ttu-id="354bb-250">Öppna hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-250">Open hello service.</span></span>

    ```csharp
    host.Open();
    ```
2. <span data-ttu-id="354bb-251">Informera hello användare som hello tjänsten körs och förklara hur tooshut ned hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-251">Inform hello user that hello service is running, and explain how tooshut down hello service.</span></span>

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="354bb-252">När du är klar stänger du hello tjänstvärden.</span><span class="sxs-lookup"><span data-stu-id="354bb-252">When finished, close hello service host.</span></span>

    ```csharp
    host.Close();
    ```
4. <span data-ttu-id="354bb-253">Tryck på **Ctrl + Skift + B** toobuild hello projektet.</span><span class="sxs-lookup"><span data-stu-id="354bb-253">Press **Ctrl+Shift+B** toobuild hello project.</span></span>

### <a name="example"></a><span data-ttu-id="354bb-254">Exempel</span><span class="sxs-lookup"><span data-stu-id="354bb-254">Example</span></span>

<span data-ttu-id="354bb-255">Koden för slutförda tjänsten ska visas på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="354bb-255">Your completed service code should appear as follows.</span></span> <span data-ttu-id="354bb-256">hello kod innehåller hello tjänstekontraktet och implementeringen från föregående steg i självstudiekursen hello och värdar hello tjänst i ett konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="354bb-256">hello code includes hello service contract and implementation from previous steps in hello tutorial, and hosts hello service in a console application.</span></span>

```csharp
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         

            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create hello credentials object for hello endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create hello service URI based on hello service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create hello service host reading hello configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create hello ServiceRegistrySettings behavior for hello endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add hello Relay credentials tooall endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open hello service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] tooexit");
            Console.ReadLine();

            // Close hello service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-hello-service-contract"></a><span data-ttu-id="354bb-257">Skapa en WCF-klient för tjänstekontraktet hello</span><span class="sxs-lookup"><span data-stu-id="354bb-257">Create a WCF client for hello service contract</span></span>

<span data-ttu-id="354bb-258">hello nästa steg är toocreate ett klientprogram och definiera hello-tjänstekontrakt som du kommer att implementera i senare steg.</span><span class="sxs-lookup"><span data-stu-id="354bb-258">hello next step is toocreate a client application and define hello service contract you will implement in later steps.</span></span> <span data-ttu-id="354bb-259">Observera att många av de här stegen liknar hello steg använda toocreate en tjänst: definiera ett kontrakt, redigera en App.config-fil, använder autentiseringsuppgifter tooconnect toohello vidarebefordrande tjänst, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="354bb-259">Note that many of these steps resemble hello steps used toocreate a service: defining a contract, editing an App.config file, using credentials tooconnect toohello relay service, and so on.</span></span> <span data-ttu-id="354bb-260">hello-kod som används för dessa uppgifter finns i hello-exemplet som följer hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="354bb-260">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

1. <span data-ttu-id="354bb-261">Skapa ett nytt projekt i hello aktuella Visual Studio-lösning för hello klient hello följande:</span><span class="sxs-lookup"><span data-stu-id="354bb-261">Create a new project in hello current Visual Studio solution for hello client by doing hello following:</span></span>

   1. <span data-ttu-id="354bb-262">I Solution Explorer i hello samma lösning som innehåller hello-tjänsten och högerklicka på hello aktuella lösningen (inte hello projekt), klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="354bb-262">In Solution Explorer, in hello same solution that contains hello service, right-click hello current solution (not hello project), and click **Add**.</span></span> <span data-ttu-id="354bb-263">Klicka sedan på **Nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="354bb-263">Then click **New Project**.</span></span>
   2. <span data-ttu-id="354bb-264">I hello **Lägg till nytt projekt** dialogrutan klickar du på **Visual C#** (om **Visual C#** inte visas, tittar du under **andra språk**), Välj hello **Konsolapp (.NET Framework)** mall, och ger den namnet **EchoClient**.</span><span class="sxs-lookup"><span data-stu-id="354bb-264">In hello **Add New Project** dialog box, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**), select hello **Console App (.NET Framework)** template, and name it **EchoClient**.</span></span>
   3. <span data-ttu-id="354bb-265">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="354bb-265">Click **OK**.</span></span>
      <br />
2. <span data-ttu-id="354bb-266">I Solution Explorer dubbelklickar du på hello Program.cs-filen i hello **EchoClient** projektet tooopen den i Redigeraren för hello, om den inte redan är öppen.</span><span class="sxs-lookup"><span data-stu-id="354bb-266">In Solution Explorer, double-click hello Program.cs file in hello **EchoClient** project tooopen it in hello editor, if it is not already open.</span></span>
3. <span data-ttu-id="354bb-267">Ändra hello namnområdesnamnet från standardnamnet `EchoClient` för`Microsoft.ServiceBus.Samples`.</span><span class="sxs-lookup"><span data-stu-id="354bb-267">Change hello namespace name from its default name of `EchoClient` too`Microsoft.ServiceBus.Samples`.</span></span>
4. <span data-ttu-id="354bb-268">Installera hello [Service Bus NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus): i Solution Explorer högerklickar du på hello **EchoClient** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="354bb-268">Install hello [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus): in Solution Explorer, right-click hello **EchoClient** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="354bb-269">Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="354bb-269">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="354bb-270">Klicka på **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="354bb-270">Click **Install**, and accept hello terms of use.</span></span>

    ![][3]
5. <span data-ttu-id="354bb-271">Lägg till en `using` -instruktion för hello [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) namnområde i hello Program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="354bb-271">Add a `using` statement for hello [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) namespace in hello Program.cs file.</span></span>

    ```csharp
    using System.ServiceModel;
    ```
6. <span data-ttu-id="354bb-272">Lägg till hello definition toohello namnområdet för tjänstekontraktet, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="354bb-272">Add hello service contract definition toohello namespace, as shown in hello following example.</span></span> <span data-ttu-id="354bb-273">Observera att den här definitionen är identiska toohello definitionen i hello **Service** projekt.</span><span class="sxs-lookup"><span data-stu-id="354bb-273">Note that this definition is identical toohello definition used in hello **Service** project.</span></span> <span data-ttu-id="354bb-274">Du bör lägga till den här koden hello överst i hello `Microsoft.ServiceBus.Samples` namnområde.</span><span class="sxs-lookup"><span data-stu-id="354bb-274">You should add this code at hello top of hello `Microsoft.ServiceBus.Samples` namespace.</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. <span data-ttu-id="354bb-275">Tryck på **Ctrl + Skift + B** toobuild hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="354bb-275">Press **Ctrl+Shift+B** toobuild hello client.</span></span>

### <a name="example"></a><span data-ttu-id="354bb-276">Exempel</span><span class="sxs-lookup"><span data-stu-id="354bb-276">Example</span></span>

<span data-ttu-id="354bb-277">hello följande kod visar hello aktuell status för hello Program.cs-filen i hello **EchoClient** projekt.</span><span class="sxs-lookup"><span data-stu-id="354bb-277">hello following code shows hello current status of hello Program.cs file in hello **EchoClient** project.</span></span>

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-hello-wcf-client"></a><span data-ttu-id="354bb-278">Konfigurera hello WCF-klient</span><span class="sxs-lookup"><span data-stu-id="354bb-278">Configure hello WCF client</span></span>

<span data-ttu-id="354bb-279">I det här steget skapar du en App.config-fil för ett grundläggande klientprogrammet som ansluter till hello-tjänst som skapades tidigare i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="354bb-279">In this step, you create an App.config file for a basic client application that accesses hello service created previously in this tutorial.</span></span> <span data-ttu-id="354bb-280">Den här App.config-filen definierar hello kontraktet, bindningen och namnet på hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="354bb-280">This App.config file defines hello contract, binding, and name of hello endpoint.</span></span> <span data-ttu-id="354bb-281">hello-kod som används för dessa uppgifter finns i hello-exemplet som följer hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="354bb-281">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

1. <span data-ttu-id="354bb-282">I Solution Explorer i hello **EchoClient** projektet genom att dubbelklicka på **App.config** tooopen hello-filen i hello Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="354bb-282">In Solution Explorer, in hello **EchoClient** project, double-click **App.config** tooopen hello file in hello Visual Studio editor.</span></span>
2. <span data-ttu-id="354bb-283">I hello `<appSettings>` element, Ersätt hello-platshållare med hello namnet på namnområdet för tjänsten och hello SAS-nyckel som du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="354bb-283">In hello `<appSettings>` element, replace hello placeholders with hello name of your service namespace, and hello SAS key that you copied in an earlier step.</span></span>
3. <span data-ttu-id="354bb-284">Inom elementet system.serviceModel hello lägger du till en `<client>` element.</span><span class="sxs-lookup"><span data-stu-id="354bb-284">Within hello system.serviceModel element, add a `<client>` element.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    <span data-ttu-id="354bb-285">I det här steget anger du att du definierar ett klientprogram i WCF-format.</span><span class="sxs-lookup"><span data-stu-id="354bb-285">This step declares that you are defining a WCF-style client application.</span></span>
4. <span data-ttu-id="354bb-286">Inom hello `client` element definiera hello namnet, kontraktet och bindningstypen för slutpunkten hello.</span><span class="sxs-lookup"><span data-stu-id="354bb-286">Within hello `client` element, define hello name, contract, and binding type for hello endpoint.</span></span>

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="354bb-287">Det här steget definierar hello slutpunkt, hello kontrakt som definierats i hello-tjänsten och hello faktum att hello klientprogrammet använder TCP toocommunicate med Azure Relay hello namn.</span><span class="sxs-lookup"><span data-stu-id="354bb-287">This step defines hello name of hello endpoint, hello contract defined in hello service, and hello fact that hello client application uses TCP toocommunicate with Azure Relay.</span></span> <span data-ttu-id="354bb-288">hello namnet på slutpunkten används i hello nästa steg toolink denna slutpunktskonfiguration med URI för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-288">hello endpoint name is used in hello next step toolink this endpoint configuration with hello service URI.</span></span>
5. <span data-ttu-id="354bb-289">Klicka på **filen**, klicka på **spara alla**.</span><span class="sxs-lookup"><span data-stu-id="354bb-289">Click **File**, then click **Save All**.</span></span>

## <a name="example"></a><span data-ttu-id="354bb-290">Exempel</span><span class="sxs-lookup"><span data-stu-id="354bb-290">Example</span></span>

<span data-ttu-id="354bb-291">hello visar följande kod hello App.config-fil för hello Echo-klienten.</span><span class="sxs-lookup"><span data-stu-id="354bb-291">hello following code shows hello App.config file for hello Echo client.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-hello-wcf-client"></a><span data-ttu-id="354bb-292">Implementera hello WCF-klient</span><span class="sxs-lookup"><span data-stu-id="354bb-292">Implement hello WCF client</span></span>
<span data-ttu-id="354bb-293">I det här steget kan implementera du ett grundläggande klientprogrammet som ansluter till hello-tjänst som du skapade tidigare i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="354bb-293">In this step, you implement a basic client application that accesses hello service you created previously in this tutorial.</span></span> <span data-ttu-id="354bb-294">Liknande toohello service hello utför klienten många av hello samma åtgärder tooaccess Azure Relay:</span><span class="sxs-lookup"><span data-stu-id="354bb-294">Similar toohello service, hello client performs many of hello same operations tooaccess Azure Relay:</span></span>

1. <span data-ttu-id="354bb-295">Anger hello anslutningsläget.</span><span class="sxs-lookup"><span data-stu-id="354bb-295">Sets hello connectivity mode.</span></span>
2. <span data-ttu-id="354bb-296">Skapar hello URI som lokaliserar värdtjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="354bb-296">Creates hello URI that locates hello host service.</span></span>
3. <span data-ttu-id="354bb-297">Definierar hello säkerhetsreferenser.</span><span class="sxs-lookup"><span data-stu-id="354bb-297">Defines hello security credentials.</span></span>
4. <span data-ttu-id="354bb-298">Gäller hello autentiseringsuppgifter toohello anslutning.</span><span class="sxs-lookup"><span data-stu-id="354bb-298">Applies hello credentials toohello connection.</span></span>
5. <span data-ttu-id="354bb-299">Öppnar hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="354bb-299">Opens hello connection.</span></span>
6. <span data-ttu-id="354bb-300">Utför hello programspecifika uppgifterna.</span><span class="sxs-lookup"><span data-stu-id="354bb-300">Performs hello application-specific tasks.</span></span>
7. <span data-ttu-id="354bb-301">Stänger hello anslutningen.</span><span class="sxs-lookup"><span data-stu-id="354bb-301">Closes hello connection.</span></span>

<span data-ttu-id="354bb-302">En av hello viktigaste skillnaderna är dock att hello klientprogrammet använder en kanal tooconnect toohello vidarebefordrande tjänst, medan hello tjänsten använder ett anrop för**ServiceHost**.</span><span class="sxs-lookup"><span data-stu-id="354bb-302">However, one of hello main differences is that hello client application uses a channel tooconnect toohello relay service, whereas hello service uses a call too**ServiceHost**.</span></span> <span data-ttu-id="354bb-303">hello-kod som används för dessa uppgifter finns i hello-exemplet som följer hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="354bb-303">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

### <a name="implement-a-client-application"></a><span data-ttu-id="354bb-304">Implementera ett klientprogram</span><span class="sxs-lookup"><span data-stu-id="354bb-304">Implement a client application</span></span>
1. <span data-ttu-id="354bb-305">Ange hello anslutningsläget för**AutoDetect**.</span><span class="sxs-lookup"><span data-stu-id="354bb-305">Set hello connectivity mode too**AutoDetect**.</span></span> <span data-ttu-id="354bb-306">Lägg till följande kod i hello hello `Main()` metod för hello **EchoClient** program.</span><span class="sxs-lookup"><span data-stu-id="354bb-306">Add hello following code inside hello `Main()` method of hello **EchoClient** application.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. <span data-ttu-id="354bb-307">Definiera variabler toohold hello värden för hello tjänstens namnområde och SAS-nyckeln som läses från hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="354bb-307">Define variables toohold hello values for hello service namespace, and SAS key that are read from hello console.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. <span data-ttu-id="354bb-308">Skapa hello URI som definierar hello platsen för hello värden i projektet Relay.</span><span class="sxs-lookup"><span data-stu-id="354bb-308">Create hello URI that defines hello location of hello host in your Relay project.</span></span>

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. <span data-ttu-id="354bb-309">Skapa hello-autentiseringsobjekt för tjänstens namnområde-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="354bb-309">Create hello credential object for your service namespace endpoint.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. <span data-ttu-id="354bb-310">Skapa hello kanalfabrik som läser in hello konfigurationen som beskrivs i hello App.config-fil.</span><span class="sxs-lookup"><span data-stu-id="354bb-310">Create hello channel factory that loads hello configuration described in hello App.config file.</span></span>

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    <span data-ttu-id="354bb-311">En kanalfabriken är ett WCF-objekt som skapar en kanal via vilken hello tjänsten och klienten kan kommunicera.</span><span class="sxs-lookup"><span data-stu-id="354bb-311">A channel factory is a WCF object that creates a channel through which hello service and client applications communicate.</span></span>
6. <span data-ttu-id="354bb-312">Tillämpa hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="354bb-312">Apply hello credentials.</span></span>

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. <span data-ttu-id="354bb-313">Skapa och öppna hello kanal toohello service.</span><span class="sxs-lookup"><span data-stu-id="354bb-313">Create and open hello channel toohello service.</span></span>

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. <span data-ttu-id="354bb-314">Skriva hello grundläggande gränssnittet och funktionerna för hello echo.</span><span class="sxs-lookup"><span data-stu-id="354bb-314">Write hello basic user interface and functionality for hello echo.</span></span>

    ```csharp
    Console.WriteLine("Enter text tooecho (or [Enter] tooexit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    <span data-ttu-id="354bb-315">Observera att hello koden använder hello instans av hello kanal-objektet som en proxy för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="354bb-315">Note that hello code uses hello instance of hello channel object as a proxy for hello service.</span></span>
9. <span data-ttu-id="354bb-316">Stäng hello kanalen och Stäng hello fabriken.</span><span class="sxs-lookup"><span data-stu-id="354bb-316">Close hello channel, and close hello factory.</span></span>

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

## <a name="example"></a><span data-ttu-id="354bb-317">Exempel</span><span class="sxs-lookup"><span data-stu-id="354bb-317">Example</span></span>

<span data-ttu-id="354bb-318">Den färdiga koden ska se ut så här visar hur toocreate ett klientprogram, hur toocall hello drift av hello-tjänsten och hur tooclose hello klienten efter hello åtgärden anropa är klar.</span><span class="sxs-lookup"><span data-stu-id="354bb-318">Your completed code should appear as follows, showing how toocreate a client application, how toocall hello operations of hello service, and how tooclose hello client after hello operation call is finished.</span></span>

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;


            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text tooecho (or [Enter] tooexit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="run-hello-applications"></a><span data-ttu-id="354bb-319">Köra hello program</span><span class="sxs-lookup"><span data-stu-id="354bb-319">Run hello applications</span></span>

1. <span data-ttu-id="354bb-320">Tryck på **Ctrl + Skift + B** toobuild hello lösning.</span><span class="sxs-lookup"><span data-stu-id="354bb-320">Press **Ctrl+Shift+B** toobuild hello solution.</span></span> <span data-ttu-id="354bb-321">Detta skapar både hello klientprojektet och hello service-projekt som du skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="354bb-321">This builds both hello client project and hello service project that you created in hello previous steps.</span></span>
2. <span data-ttu-id="354bb-322">Innan du kör hello-klientprogrammet måste du se till att programmet hello körs.</span><span class="sxs-lookup"><span data-stu-id="354bb-322">Before running hello client application, you must make sure that hello service application is running.</span></span> <span data-ttu-id="354bb-323">I Solution Explorer i Visual Studio högerklickar du på hello **EchoService** lösning, klicka på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="354bb-323">In Solution Explorer in Visual Studio, right-click hello **EchoService** solution, then click **Properties**.</span></span>
3. <span data-ttu-id="354bb-324">I egenskapsdialogrutan för hello lösning, klickar du på **Startprojekt**, klicka på hello **flera Startprojekt** knappen.</span><span class="sxs-lookup"><span data-stu-id="354bb-324">In hello solution properties dialog box, click **Startup Project**, then click hello **Multiple startup projects** button.</span></span> <span data-ttu-id="354bb-325">Kontrollera att **EchoService** visas först i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="354bb-325">Make sure **EchoService** appears first in hello list.</span></span>
4. <span data-ttu-id="354bb-326">Ange hello **åtgärd** för båda hello **EchoService** och **EchoClient** projekt för**starta**.</span><span class="sxs-lookup"><span data-stu-id="354bb-326">Set hello **Action** box for both hello **EchoService** and **EchoClient** projects too**Start**.</span></span>

    ![][5]
5. <span data-ttu-id="354bb-327">Klicka på **Projektberoenden**.</span><span class="sxs-lookup"><span data-stu-id="354bb-327">Click **Project Dependencies**.</span></span> <span data-ttu-id="354bb-328">I hello **projekt** väljer **EchoClient**.</span><span class="sxs-lookup"><span data-stu-id="354bb-328">In hello **Projects** box, select **EchoClient**.</span></span> <span data-ttu-id="354bb-329">I hello **beror på** Kontrollera **EchoService** är markerad.</span><span class="sxs-lookup"><span data-stu-id="354bb-329">In hello **Depends on** box, make sure **EchoService** is checked.</span></span>

    ![][6]
6. <span data-ttu-id="354bb-330">Klicka på **OK** toodismiss hello **egenskaper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="354bb-330">Click **OK** toodismiss hello **Properties** dialog.</span></span>
7. <span data-ttu-id="354bb-331">Tryck på **F5** toorun båda projekten.</span><span class="sxs-lookup"><span data-stu-id="354bb-331">Press **F5** toorun both projects.</span></span>
8. <span data-ttu-id="354bb-332">Båda konsolfönstren öppnas och du uppmanas att ange hello namnområdesnamnet.</span><span class="sxs-lookup"><span data-stu-id="354bb-332">Both console windows open and prompt you for hello namespace name.</span></span> <span data-ttu-id="354bb-333">hello-tjänsten måste köras först, så i hello **EchoService** konsolfönstret, ange hello namnområde och tryck sedan på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="354bb-333">hello service must run first, so in hello **EchoService** console window, enter hello namespace and then press **Enter**.</span></span>
9. <span data-ttu-id="354bb-334">Därefter uppmanas du ange din SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="354bb-334">Next, you are prompted for your SAS key.</span></span> <span data-ttu-id="354bb-335">Ange hello SAS-nyckeln och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="354bb-335">Enter hello SAS key and press ENTER.</span></span>

    <span data-ttu-id="354bb-336">Här är exempel på utdata från hello konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="354bb-336">Here is example output from hello console window.</span></span> <span data-ttu-id="354bb-337">Observera att hello värdena som anges här är till exempel endast.</span><span class="sxs-lookup"><span data-stu-id="354bb-337">Note that hello values provided here are for example purposes only.</span></span>

    <span data-ttu-id="354bb-338">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span><span class="sxs-lookup"><span data-stu-id="354bb-338">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span></span>

    <span data-ttu-id="354bb-339">hello Tjänsteprogrammet skriver toohello fönstret hello adressen som lyssnar, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="354bb-339">hello service application prints toohello console window hello address on which it's listening, as seen in hello following example.</span></span>

    <span data-ttu-id="354bb-340">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] tooexit`</span><span class="sxs-lookup"><span data-stu-id="354bb-340">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] tooexit`</span></span>
10. <span data-ttu-id="354bb-341">I hello **EchoClient** konsolfönstret, ange hello samma information som du angav tidigare för hello-tjänstprogrammet.</span><span class="sxs-lookup"><span data-stu-id="354bb-341">In hello **EchoClient** console window, enter hello same information that you entered previously for hello service application.</span></span> <span data-ttu-id="354bb-342">Följ hello föregående steg tooenter hello samma namnområde för tjänsten och SAS-nyckeln värden för hello-klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="354bb-342">Follow hello previous steps tooenter hello same service namespace and SAS key values for hello client application.</span></span>
11. <span data-ttu-id="354bb-343">När du har angett dessa värden hello klienten öppnar en kanal toohello tjänst och frågar du tooenter lite text i hello följande utmatningsexempel för konsolen.</span><span class="sxs-lookup"><span data-stu-id="354bb-343">After entering these values, hello client opens a channel toohello service and prompts you tooenter some text as seen in hello following console output example.</span></span>

    `Enter text tooecho (or [Enter] tooexit):`

    <span data-ttu-id="354bb-344">Ange vissa text toosend toohello-tjänstprogrammet och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="354bb-344">Enter some text toosend toohello service application and press Enter.</span></span> <span data-ttu-id="354bb-345">Den här texten skickas toohello tjänsten via hello tjänståtgärd Echo och visas i hello tjänstekonsolfönstret hello följande exempel på utdata.</span><span class="sxs-lookup"><span data-stu-id="354bb-345">This text is sent toohello service through hello Echo service operation and appears in hello service console window as in hello following example output.</span></span>

    `Echoing: My sample text`

    <span data-ttu-id="354bb-346">hello klientprogrammet får hello returvärdet för hello `Echo` åtgärden som hello ursprungliga texten och skriver ut det tooits konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="354bb-346">hello client application receives hello return value of hello `Echo` operation, which is hello original text, and prints it tooits console window.</span></span> <span data-ttu-id="354bb-347">hello följande är exempel på utdata från hello klientens konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="354bb-347">hello following is example output from hello client console window.</span></span>

    `Server echoed: My sample text`
12. <span data-ttu-id="354bb-348">Du kan fortsätta att skicka textmeddelanden från hello toohello-klienttjänsten i det här sättet.</span><span class="sxs-lookup"><span data-stu-id="354bb-348">You can continue sending text messages from hello client toohello service in this manner.</span></span> <span data-ttu-id="354bb-349">När du är klar, tryck på RETUR i hello klienten och tjänsten konsolen windows tooend båda programmen.</span><span class="sxs-lookup"><span data-stu-id="354bb-349">When you are finished, press Enter in hello client and service console windows tooend both applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="354bb-350">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="354bb-350">Next steps</span></span>

<span data-ttu-id="354bb-351">Den här självstudiekursen visades hur toobuild ett Azure Relay-klientprogram och tjänsten använder hello vidarebefordrande WCF-funktionerna i Service Bus.</span><span class="sxs-lookup"><span data-stu-id="354bb-351">This tutorial showed how toobuild an Azure Relay client application and service using hello WCF Relay capabilities of Service Bus.</span></span> <span data-ttu-id="354bb-352">För en liknande självstudiekurs som använder [Service Bus-meddelanden](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), se [komma igång med Service Bus-köer](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="354bb-352">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="354bb-353">toolearn mer om Azure Relay finns hello följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="354bb-353">toolearn more about Azure Relay, see hello following topics.</span></span>

* [<span data-ttu-id="354bb-354">Azure Service Bus-Arkitekturöversikt</span><span class="sxs-lookup"><span data-stu-id="354bb-354">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [<span data-ttu-id="354bb-355">Översikt över Azure Relay</span><span class="sxs-lookup"><span data-stu-id="354bb-355">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="354bb-356">Hur toouse hello WCF vidarebefordrande tjänsten med .NET</span><span class="sxs-lookup"><span data-stu-id="354bb-356">How toouse hello WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure classic portal]: http://manage.windowsazure.com

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
