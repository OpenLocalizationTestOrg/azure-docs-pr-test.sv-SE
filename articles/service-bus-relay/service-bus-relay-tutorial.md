---
title: "Azure Service Bus Relay för WCF-kursen | Microsoft Docs"
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
ms.openlocfilehash: 5347bf85cad32b59677369d51a1f36529aef6662
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-wcf-relay-tutorial"></a><span data-ttu-id="a8c82-103">Azure vidarebefordrande WCF-självstudier</span><span class="sxs-lookup"><span data-stu-id="a8c82-103">Azure WCF Relay tutorial</span></span>

<span data-ttu-id="a8c82-104">Den här självstudiekursen beskrivs hur du skapar en enkel vidarebefordrande WCF-klient och en tjänst med hjälp av Azure Relay.</span><span class="sxs-lookup"><span data-stu-id="a8c82-104">This tutorial describes how to build a simple WCF Relay client application and service using Azure Relay.</span></span> <span data-ttu-id="a8c82-105">För en liknande självstudiekurs som använder [Service Bus-meddelanden](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), se [komma igång med Service Bus-köer](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="a8c82-105">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="a8c82-106">Gå igenom den här kursen får du en förståelse av de steg som krävs för att skapa ett vidarebefordrande WCF-klienten och tjänsten program.</span><span class="sxs-lookup"><span data-stu-id="a8c82-106">Working through this tutorial gives you an understanding of the steps that are required to create a WCF Relay client and service application.</span></span> <span data-ttu-id="a8c82-107">En tjänst är en konstruktion som Exponerar en eller flera slutpunkter som Exponerar en eller flera tjänsteåtgärder som deras ursprungliga motsvarigheter i WCF.</span><span class="sxs-lookup"><span data-stu-id="a8c82-107">Like their original WCF counterparts, a service is a construct that exposes one or more endpoints, each of which exposes one or more service operations.</span></span> <span data-ttu-id="a8c82-108">Slutpunkten för en tjänst anger en adress där tjänsten ligger, en bindning som innehåller den information som en klient måste använda för att kommunicera med tjänsten och ett kontrakt som definierar de funktioner som tjänsten levererar till sina klienter.</span><span class="sxs-lookup"><span data-stu-id="a8c82-108">The endpoint of a service specifies an address where the service can be found, a binding that contains the information that a client must communicate with the service, and a contract that defines the functionality provided by the service to its clients.</span></span> <span data-ttu-id="a8c82-109">Den största skillnaden mellan WCF- och WCF Relay är att slutpunkten exponeras i molnet i stället för lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="a8c82-109">The main difference between WCF and WCF Relay is that the endpoint is exposed in the cloud instead of locally on your computer.</span></span>

<span data-ttu-id="a8c82-110">När du har gått igenom alla teman i den här självstudiekursen, kommer du att ha en tjänst om är redo att köras samt en klient som kan anropa tjänstens funktionsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="a8c82-110">After you work through the sequence of topics in this tutorial, you will have a running service, and a client that can invoke the operations of the service.</span></span> <span data-ttu-id="a8c82-111">I det första avsnittet beskriver vi hur du skapar ett konto.</span><span class="sxs-lookup"><span data-stu-id="a8c82-111">The first topic describes how to set up an account.</span></span> <span data-ttu-id="a8c82-112">Nästa steg beskriver hur du definierar en tjänst som använder ett kontrakt, hur du implementerar tjänsten och hur du konfigurerar tjänsten i kod.</span><span class="sxs-lookup"><span data-stu-id="a8c82-112">The next steps describe how to define a service that uses a contract, how to implement the service, and how to configure the service in code.</span></span> <span data-ttu-id="a8c82-113">Här beskrivs också hur du hyser in och kör tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-113">They also describe how to host and run the service.</span></span> <span data-ttu-id="a8c82-114">Tjänsten som skapas är egenvärdsbaserad (self-hosted) och klienten och tjänsten körs på samma dator.</span><span class="sxs-lookup"><span data-stu-id="a8c82-114">The service that is created is self-hosted and the client and service run on the same computer.</span></span> <span data-ttu-id="a8c82-115">Du kan konfigurera tjänsten genom att antingen använda koden eller en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="a8c82-115">You can configure the service by using either code or a configuration file.</span></span>

<span data-ttu-id="a8c82-116">I de tre sista stegen beskriver vi hur du skapar ett klientprogram, konfigurerar detta och skapar och använder en klient som har åtkomst till värdens funktioner.</span><span class="sxs-lookup"><span data-stu-id="a8c82-116">The final three steps describe how to create a client application, configure the client application, and create and use a client that can access the functionality of the host.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8c82-117">Krav</span><span class="sxs-lookup"><span data-stu-id="a8c82-117">Prerequisites</span></span>

<span data-ttu-id="a8c82-118">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a8c82-118">To complete this tutorial, you'll need the following:</span></span>

* <span data-ttu-id="a8c82-119">[Microsoft Visual Studio 2015 eller senare](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="a8c82-119">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="a8c82-120">Den här självstudiekursen använder Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a8c82-120">This tutorial uses Visual Studio 2017.</span></span>
* <span data-ttu-id="a8c82-121">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a8c82-121">An active Azure account.</span></span> <span data-ttu-id="a8c82-122">Om du inte har något konto kan du skapa ett utan kostnad på ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="a8c82-122">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="a8c82-123">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="a8c82-123">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-a-service-namespace"></a><span data-ttu-id="a8c82-124">Skapa ett namnområde för tjänsten</span><span class="sxs-lookup"><span data-stu-id="a8c82-124">Create a service namespace</span></span>

<span data-ttu-id="a8c82-125">Det första steget är att skapa ett namnområde och få en [delade signatur åtkomst (SAS)](../service-bus-messaging/service-bus-sas.md) nyckel.</span><span class="sxs-lookup"><span data-stu-id="a8c82-125">The first step is to create a namespace, and to obtain a [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) key.</span></span> <span data-ttu-id="a8c82-126">Ett namnområde ger en programgräns för varje program som exponeras via den vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-126">A namespace provides an application boundary for each application exposed through the relay service.</span></span> <span data-ttu-id="a8c82-127">SAS-nyckeln genereras automatiskt av systemet när ett namnområde för tjänsten har skapats.</span><span class="sxs-lookup"><span data-stu-id="a8c82-127">A SAS key is automatically generated by the system when a service namespace is created.</span></span> <span data-ttu-id="a8c82-128">Kombinationen av namnområdet för tjänsten och SAS-nyckeln ger referensen för Azure för att autentisera åtkomst till ett program.</span><span class="sxs-lookup"><span data-stu-id="a8c82-128">The combination of service namespace and SAS key provides the credentials for Azure to authenticate access to an application.</span></span> <span data-ttu-id="a8c82-129">Följ [anvisningarna här](relay-create-namespace-portal.md) för att skapa ett Relay-namnområde.</span><span class="sxs-lookup"><span data-stu-id="a8c82-129">Follow the [instructions here](relay-create-namespace-portal.md) to create a Relay namespace.</span></span>

## <a name="define-a-wcf-service-contract"></a><span data-ttu-id="a8c82-130">Definiera ett WCF-tjänstekontrakt</span><span class="sxs-lookup"><span data-stu-id="a8c82-130">Define a WCF service contract</span></span>

<span data-ttu-id="a8c82-131">Tjänstekontraktet anger vilka åtgärder (webbserviceterminologin för metoder eller funktioner) tjänsten stöder.</span><span class="sxs-lookup"><span data-stu-id="a8c82-131">The service contract specifies what operations (the web service terminology for methods or functions) the service supports.</span></span> <span data-ttu-id="a8c82-132">Kontrakt skapas genom att definiera ett gränssnitt för C++, C# eller Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="a8c82-132">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="a8c82-133">Varje metod i gränssnittet motsvarar en viss tjänsteåtgärd.</span><span class="sxs-lookup"><span data-stu-id="a8c82-133">Each method in the interface corresponds to a specific service operation.</span></span> <span data-ttu-id="a8c82-134">Attributet [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) måste tillämpas på varje gränssnitt och attributet [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) måste tillämpas på varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a8c82-134">Each interface must have the [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute applied to it, and each operation must have the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute applied to it.</span></span> <span data-ttu-id="a8c82-135">Om en metod i ett gränssnitt som har attributet [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) inte har attributet [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), är inte den metoden exponerad.</span><span class="sxs-lookup"><span data-stu-id="a8c82-135">If a method in an interface that has the [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute does not have the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute, that method is not exposed.</span></span> <span data-ttu-id="a8c82-136">Koden för dessa arbetsuppgifter visas in exemplet som följer efter proceduren.</span><span class="sxs-lookup"><span data-stu-id="a8c82-136">The code for these tasks is provided in the example following the procedure.</span></span> <span data-ttu-id="a8c82-137">En mer utförlig beskrivning av kontrakt och tjänster finns i [Utforma och implementera tjänster](https://msdn.microsoft.com/library/ms729746.aspx) i WCF-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-137">For a larger discussion of contracts and services, see [Designing and Implementing Services](https://msdn.microsoft.com/library/ms729746.aspx) in the WCF documentation.</span></span>

### <a name="create-a-relay-contract-with-an-interface"></a><span data-ttu-id="a8c82-138">Skapa en relay-kontrakt med ett gränssnitt</span><span class="sxs-lookup"><span data-stu-id="a8c82-138">Create a relay contract with an interface</span></span>

1. <span data-ttu-id="a8c82-139">Öppna Visual Studio som administratör genom att högerklicka på programmet i **Start**-menyn och välja **Kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-139">Open Visual Studio as an administrator by right-clicking the program in the **Start** menu and selecting **Run as administrator**.</span></span>
2. <span data-ttu-id="a8c82-140">Skapa ett nytt konsolprogramsprojekt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-140">Create a new console application project.</span></span> <span data-ttu-id="a8c82-141">Klicka på **Arkiv**-menyn, välj **Nytt** och klicka sedan på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-141">Click the **File** menu and select **New**, then click **Project**.</span></span> <span data-ttu-id="a8c82-142">Klicka på **Visual C#** i dialogrutan **Nytt projekt** (om **Visual C#** inte visas, tittar du under **Andra språk**).</span><span class="sxs-lookup"><span data-stu-id="a8c82-142">In the **New Project** dialog, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**).</span></span> <span data-ttu-id="a8c82-143">Klicka på den **Konsolapp (.NET Framework)** mall, och ger den namnet **EchoService**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-143">Click the **Console App (.NET Framework)** template, and name it **EchoService**.</span></span> <span data-ttu-id="a8c82-144">Klicka på **OK** för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-144">Click **OK** to create the project.</span></span>

    ![][2]

3. <span data-ttu-id="a8c82-145">Installera Service Bus NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-145">Install the Service Bus NuGet package.</span></span> <span data-ttu-id="a8c82-146">Det här paketet lägger automatiskt till referenser till Service Bus-bibliotek, samt även WCF **System.ServiceModel**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-146">This package automatically adds references to the Service Bus libraries, as well as the WCF **System.ServiceModel**.</span></span> <span data-ttu-id="a8c82-147">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) är det namnområde som ger dig programmatisk åtkomst till de grundläggande funktionerna i WCF.</span><span class="sxs-lookup"><span data-stu-id="a8c82-147">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is the namespace that enables you to programmatically access the basic features of WCF.</span></span> <span data-ttu-id="a8c82-148">Service Bus använder många av WFC:s objekt och attribut för att definiera tjänstekontrakt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-148">Service Bus uses many of the objects and attributes of WCF to define service contracts.</span></span>

    <span data-ttu-id="a8c82-149">Högerklicka på projektet i Solution Explorer och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="a8c82-149">In Solution Explorer, right-click the project, and then click **Manage NuGet Packages...**.</span></span> <span data-ttu-id="a8c82-150">Klicka på **Bläddra**-fliken och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-150">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="a8c82-151">Kontrollera att projektnamnet är markerat i rutan **Versioner**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-151">Ensure that the project name is selected in the **Version(s)** box.</span></span> <span data-ttu-id="a8c82-152">Klicka på **Installera** och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="a8c82-152">Click **Install**, and accept the terms of use.</span></span>

    ![][3]
4. <span data-ttu-id="a8c82-153">Dubbelklicka på filen Program.cs i Solution Explorer för att öppna den i redigeraren, om den inte redan är öppen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-153">In Solution Explorer, double-click the Program.cs file to open it in the editor, if it is not already open.</span></span>
5. <span data-ttu-id="a8c82-154">Lägg till följande using-uttryck högst upp i filen:</span><span class="sxs-lookup"><span data-stu-id="a8c82-154">Add the following using statements at the top of the file:</span></span>

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. <span data-ttu-id="a8c82-155">Ändra namnet för namnområdet från dess standardnamn **EchoService** till **Microsoft.ServiceBus.Samples**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-155">Change the namespace name from its default name of **EchoService** to **Microsoft.ServiceBus.Samples**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="a8c82-156">Den här kursen använder C#-namnområdet **Microsoft.ServiceBus.Samples**, vilket är namnområdet för kontrakt-baserade hanterade typen som används i konfigurationsfilen i den [konfigurerar WCF-klienten](#configure-the-wcf-client) steg.</span><span class="sxs-lookup"><span data-stu-id="a8c82-156">This tutorial uses the C# namespace **Microsoft.ServiceBus.Samples**, which is the namespace of the contract-based managed type that is used in the configuration file in the [Configure the WCF client](#configure-the-wcf-client) step.</span></span> <span data-ttu-id="a8c82-157">Du kan ange vilken namnområden du vill när du skapar det här exemplet. Men självstudiekursen kommer inte att fungera om du inte sedan ändrar kontraktets namnområden och tjänster därefter. Det här görs i programmets konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="a8c82-157">You can specify any namespace you want when you build this sample; however, the tutorial will not work unless you then modify the namespaces of the contract and service accordingly, in the application configuration file.</span></span> <span data-ttu-id="a8c82-158">Det namnområde som anges i filen App.config måste vara samma som det namnområde som angavs i C#-filerna.</span><span class="sxs-lookup"><span data-stu-id="a8c82-158">The namespace specified in the App.config file must be the same as the namespace specified in your C# files.</span></span>
   >
   >
7. <span data-ttu-id="a8c82-159">Direkt efter den `Microsoft.ServiceBus.Samples` namnområdesdeklaration, men inom namnområdet, definierar du ett nytt gränssnitt med namnet `IEchoContract` och tillämpa den `ServiceContractAttribute` attributet på gränssnittet med ett namnområdesvärde på `http://samples.microsoft.com/ServiceModel/Relay/`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-159">Directly after the `Microsoft.ServiceBus.Samples` namespace declaration, but within the namespace, define a new interface named `IEchoContract` and apply the `ServiceContractAttribute` attribute to the interface with a namespace value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="a8c82-160">Namnområdesvärdet skiljer sig från det namnområde som du använder under hela intervallet för din kod.</span><span class="sxs-lookup"><span data-stu-id="a8c82-160">The namespace value differs from the namespace that you use throughout the scope of your code.</span></span> <span data-ttu-id="a8c82-161">Istället används namnområdesvärdet som en unik identifierare för det här kontraktet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-161">Instead, the namespace value is used as a unique identifier for this contract.</span></span> <span data-ttu-id="a8c82-162">Genom att ange namnområdet uttryckligen förhindrar du att det förvalda namnområdesvärdet läggs till i kontraktnamnet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-162">Specifying the namespace explicitly prevents the default namespace value from being added to the contract name.</span></span> <span data-ttu-id="a8c82-163">Klistra in följande kod efter namnområdesdeklarationen:</span><span class="sxs-lookup"><span data-stu-id="a8c82-163">Paste the following code after the namespace declaration:</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="a8c82-164">Namnområdet för tjänstekontraktet innehåller vanligtvis ett namngivningsschema som inkluderar information om versionen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-164">Typically, the service contract namespace contains a naming scheme that includes version information.</span></span> <span data-ttu-id="a8c82-165">Om du tar med versionsinformation i namnområdet för tjänstekontraktet kan tjänsterna isolera större ändringar genom att definiera ett nytt tjänstkontrakt med ett nytt namnområde och sedan exponera det på en ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-165">Including version information in the service contract namespace enables services to isolate major changes by defining a new service contract with a new namespace and exposing it on a new endpoint.</span></span> <span data-ttu-id="a8c82-166">På så sätt kan klienter fortsätta att använda det gamla tjänstkontraktet utan att behöva uppdateras.</span><span class="sxs-lookup"><span data-stu-id="a8c82-166">In this manner, clients can continue to use the old service contract without having to be updated.</span></span> <span data-ttu-id="a8c82-167">Versionsinformation kan bestå av ett datum eller ett build-nummer.</span><span class="sxs-lookup"><span data-stu-id="a8c82-167">Version information can consist of a date or a build number.</span></span> <span data-ttu-id="a8c82-168">Mer information finns i [Versionhantering för tjänster](http://go.microsoft.com/fwlink/?LinkID=180498).</span><span class="sxs-lookup"><span data-stu-id="a8c82-168">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="a8c82-169">I just den här självstudiekursen innehåller namngivningsschemat för tjänstekontraktets namnområde inte någon information om versionerna.</span><span class="sxs-lookup"><span data-stu-id="a8c82-169">For the purposes of this tutorial, the naming scheme of the service contract namespace does not contain version information.</span></span>
   >
   >
8. <span data-ttu-id="a8c82-170">I den `IEchoContract` gränssnitt, deklarera en metod för den enkla åtgärden i `IEchoContract` kontraktet exponerar i gränssnittet och tillämpa den `OperationContractAttribute` attribut på den metod som du vill exponera som en del av det offentliga vidarebefordrande WCF-kontraktet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a8c82-170">Within the `IEchoContract` interface, declare a method for the single operation the `IEchoContract` contract exposes in the interface and apply the `OperationContractAttribute` attribute to the method that you want to expose as part of the public WCF Relay contract, as follows:</span></span>

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. <span data-ttu-id="a8c82-171">Direkt efter gränssnittsdefinitionen `IEchoContract` deklarerar du en kanal som ärver egenskaper från båda `IEchoContract` och även från gränssnittet `IClientChannel`, som visas här:</span><span class="sxs-lookup"><span data-stu-id="a8c82-171">Directly after the `IEchoContract` interface definition, declare a channel that inherits from both `IEchoContract` and also to the `IClientChannel` interface, as shown here:</span></span>

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    <span data-ttu-id="a8c82-172">En kanal är det WCF-objekt via vilken värden och klienten skickar information till varandra.</span><span class="sxs-lookup"><span data-stu-id="a8c82-172">A channel is the WCF object through which the host and client pass information to each other.</span></span> <span data-ttu-id="a8c82-173">Lite längre fram ska du skriva kod mot kanalen för att skapa ett eko av information mellan de två programmen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-173">Later, you will write code against the channel to echo information between the two applications.</span></span>
10. <span data-ttu-id="a8c82-174">Från menyn **Skapa** klickar du på **Skapa lösning** eller trycker på **Ctrl + Skift + B** för att bekräfta att det arbete du gjort hittills är korrekt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-174">From the **Build** menu, click **Build Solution** or press **Ctrl+Shift+B** to confirm the accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="a8c82-175">Exempel</span><span class="sxs-lookup"><span data-stu-id="a8c82-175">Example</span></span>

<span data-ttu-id="a8c82-176">Följande kod visar ett grundläggande gränssnitt som definierar ett vidarebefordrande WCF-kontrakt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-176">The following code shows a basic interface that defines a WCF Relay contract.</span></span>

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

<span data-ttu-id="a8c82-177">Nu när gränssnittet har skapats kan du implementera det.</span><span class="sxs-lookup"><span data-stu-id="a8c82-177">Now that the interface is created, you can implement the interface.</span></span>

## <a name="implement-the-wcf-contract"></a><span data-ttu-id="a8c82-178">Implementera WC-kontraktet</span><span class="sxs-lookup"><span data-stu-id="a8c82-178">Implement the WCF contract</span></span>

<span data-ttu-id="a8c82-179">Skapa ett Azure relay måste du först skapa det kontrakt som definieras med hjälp av ett gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-179">Creating an Azure relay requires that you first create the contract, which is defined by using an interface.</span></span> <span data-ttu-id="a8c82-180">Mer information om hur du skapar gränssnittet finns i det förra steget.</span><span class="sxs-lookup"><span data-stu-id="a8c82-180">For more information about creating the interface, see the previous step.</span></span> <span data-ttu-id="a8c82-181">Nästa steg är att implementera gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-181">The next step is to implement the interface.</span></span> <span data-ttu-id="a8c82-182">Detta innebär att du skapar en klass som heter `EchoService` och som implementerar det användardefinierade gränssnittet `IEchoContract`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-182">This involves creating a class named `EchoService` that implements the user-defined `IEchoContract` interface.</span></span> <span data-ttu-id="a8c82-183">Efter att du har implementerat gränssnittet, konfigurerar du det genom att använda konfigurationsfilen App.config.</span><span class="sxs-lookup"><span data-stu-id="a8c82-183">After you implement the interface, you then configure the interface using an App.config configuration file.</span></span> <span data-ttu-id="a8c82-184">Konfigurationsfilen innehåller information som behövs för programmet, till exempel namnet på tjänsten, namnet på kontraktet och vilken typ av protokoll som används för att kommunicera med den vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-184">The configuration file contains necessary information for the application, such as the name of the service, the name of the contract, and the type of protocol that is used to communicate with the relay service.</span></span> <span data-ttu-id="a8c82-185">Den kod som används för dessa arbetsuppgifter visas i exemplet som följer efter proceduren.</span><span class="sxs-lookup"><span data-stu-id="a8c82-185">The code used for these tasks is provided in the example following the procedure.</span></span> <span data-ttu-id="a8c82-186">En mer allmän diskussion om hur du implementerar ett tjänstekontrakt finns i [Implementera tjänstekontrakt](https://msdn.microsoft.com/library/ms733764.aspx) i WCF-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-186">For a more general discussion about how to implement a service contract, see [Implementing Service Contracts](https://msdn.microsoft.com/library/ms733764.aspx) in the WCF documentation.</span></span>

1. <span data-ttu-id="a8c82-187">Skapa en ny klass med namnet `EchoService` direkt efter definitionen av gränssnittet `IEchoContract`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-187">Create a new class named `EchoService` directly after the definition of the `IEchoContract` interface.</span></span> <span data-ttu-id="a8c82-188">Klassen `EchoService` implementerar gränssnittet `IEchoContract`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-188">The `EchoService` class implements the `IEchoContract` interface.</span></span>

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    <span data-ttu-id="a8c82-189">Precis som med andra gränssnittsimplementeringar kan du implementera definitionen i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="a8c82-189">Similar to other interface implementations, you can implement the definition in a different file.</span></span> <span data-ttu-id="a8c82-190">Men i den här självstudiekursen ligger implementeringen i samma fil som gränssnittsdefinitionen och `Main`-metoden.</span><span class="sxs-lookup"><span data-stu-id="a8c82-190">However, for this tutorial, the implementation is located in the same file as the interface definition and the `Main` method.</span></span>
2. <span data-ttu-id="a8c82-191">Tillämpa attributet [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) på gränssnittet `IEchoContract`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-191">Apply the [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute to the `IEchoContract` interface.</span></span> <span data-ttu-id="a8c82-192">Attributet anger namnet på tjänsten och på namnområdet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-192">The attribute specifies the service name and namespace.</span></span> <span data-ttu-id="a8c82-193">När du har gjort det, visas klassen `EchoService` på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a8c82-193">After doing so, the `EchoService` class appears as follows:</span></span>

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. <span data-ttu-id="a8c82-194">Implementera metoden `Echo` som definierats i gränssnittet `IEchoContract` i klassen `EchoService`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-194">Implement the `Echo` method defined in the `IEchoContract` interface in the `EchoService` class.</span></span>

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. <span data-ttu-id="a8c82-195">Klicka på **Skapa** och sedan på **Skapa lösning** för att bekräfta att det arbete du utfört är korrekt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-195">Click **Build**, then click **Build Solution** to confirm the accuracy of your work.</span></span>

### <a name="define-the-configuration-for-the-service-host"></a><span data-ttu-id="a8c82-196">Definiera konfigurationen för tjänstevärden</span><span class="sxs-lookup"><span data-stu-id="a8c82-196">Define the configuration for the service host</span></span>

1. <span data-ttu-id="a8c82-197">Konfigurationsfilen liknar till stora delar en WCF-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="a8c82-197">The configuration file is very similar to a WCF configuration file.</span></span> <span data-ttu-id="a8c82-198">Den innehåller namnet på tjänsten, slutpunkten (det vill säga den plats som Azure Relay visar för klienter och värdar att kommunicera med varandra) och bindningen (typ av protokoll som används för att kommunicera).</span><span class="sxs-lookup"><span data-stu-id="a8c82-198">It includes the service name, endpoint (that is, the location that Azure Relay exposes for clients and hosts to communicate with each other), and the binding (the type of protocol that is used to communicate).</span></span> <span data-ttu-id="a8c82-199">Den största skillnaden är att den här konfigurerade tjänstslutpunkten refererar till en [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding)-bindning och denna är inte en del av .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a8c82-199">The main difference is that this configured service endpoint refers to a [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) binding, which is not part of the .NET Framework.</span></span> <span data-ttu-id="a8c82-200">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) är en av de bindningar som definierats av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-200">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) is one of the bindings defined by the service.</span></span>
2. <span data-ttu-id="a8c82-201">Dubbelklicka på filen App.config i **Solution Explorer** för att öppna den i Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a8c82-201">In **Solution Explorer**, double-click the App.config file to open it in the Visual Studio editor.</span></span>
3. <span data-ttu-id="a8c82-202">I elementet `<appSettings>` ersätter du platshållarna med namnet på ditt namnområde för tjänsten och den SAS-nyckel som du kopierade i ett av de föregående stegen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-202">In the `<appSettings>` element, replace the placeholders with the name of your service namespace, and the SAS key that you copied in an earlier step.</span></span>
4. <span data-ttu-id="a8c82-203">Lägg till ett `<services>`-element inom taggarna `<system.serviceModel>`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-203">Within the `<system.serviceModel>` tags, add a `<services>` element.</span></span> <span data-ttu-id="a8c82-204">Du kan definiera flera relay-program i en enda konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="a8c82-204">You can define multiple relay applications in a single configuration file.</span></span> <span data-ttu-id="a8c82-205">I den här självstudiekursen definieras dock bara en.</span><span class="sxs-lookup"><span data-stu-id="a8c82-205">However, this tutorial defines only one.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. <span data-ttu-id="a8c82-206">Inne i elementet `<services>` element lägger du till ett `<service>`-element för att definiera namnet på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-206">Within the `<services>` element, add a `<service>` element to define the name of the service.</span></span>

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. <span data-ttu-id="a8c82-207">Definiera platsen för slutpunktskontraktet inne i elementet `<service>` och även typen av bindning för slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-207">Within the `<service>` element, define the location of the endpoint contract, and also the type of binding for the endpoint.</span></span>

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="a8c82-208">Slutpunkten definierar var klienten ska söka efter värdprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-208">The endpoint defines where the client will look for the host application.</span></span> <span data-ttu-id="a8c82-209">Kursen använder senare i det här steget för att skapa en URI som exponerar värden via Azure-relä helt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-209">Later, the tutorial uses this step to create a URI that fully exposes the host through Azure Relay.</span></span> <span data-ttu-id="a8c82-210">Bindningen deklarerar att vi använder TCP som protokoll för att kommunicera med den vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-210">The binding declares that we are using TCP as the protocol to communicate with the relay service.</span></span>
7. <span data-ttu-id="a8c82-211">Från menyn **Skapa** klickar du på **Skapa lösning** för att bekräfta att det arbete du utfört är korrekt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-211">From the **Build** menu, click **Build Solution** to confirm the accuracy of your work.</span></span>

### <a name="example"></a><span data-ttu-id="a8c82-212">Exempel</span><span class="sxs-lookup"><span data-stu-id="a8c82-212">Example</span></span>

<span data-ttu-id="a8c82-213">Följande kod visar implementeringen av tjänstekontraktet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-213">The following code shows the implementation of the service contract.</span></span>

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

<span data-ttu-id="a8c82-214">Följande kod visar det grundläggande formatet för den App.config-fil som är associerad med tjänstevärden.</span><span class="sxs-lookup"><span data-stu-id="a8c82-214">The following code shows the basic format of the App.config file associated with the service host.</span></span>

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

## <a name="host-and-run-a-basic-web-service-to-register-with-the-relay-service"></a><span data-ttu-id="a8c82-215">Värd för och köra en grundläggande webbtjänst för att registrera med den vidarebefordrande tjänsten</span><span class="sxs-lookup"><span data-stu-id="a8c82-215">Host and run a basic web service to register with the relay service</span></span>

<span data-ttu-id="a8c82-216">Det här steget beskriver hur du kör en Relay i Azure-tjänst.</span><span class="sxs-lookup"><span data-stu-id="a8c82-216">This step describes how to run an Azure Relay service.</span></span>

### <a name="create-the-relay-credentials"></a><span data-ttu-id="a8c82-217">Skapa autentiseringsuppgifter för relä</span><span class="sxs-lookup"><span data-stu-id="a8c82-217">Create the relay credentials</span></span>

1. <span data-ttu-id="a8c82-218">Skapa två variabler i `Main()` där du ska lagra namnområdet och SAS-nyckeln som läses från konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="a8c82-218">In `Main()`, create two variables in which to store the namespace and the SAS key that are read from the console window.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    <span data-ttu-id="a8c82-219">SAS-nyckeln kommer att användas senare åtkomst till ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-219">The SAS key will be used later to access your project.</span></span> <span data-ttu-id="a8c82-220">Namnområdet skickas som en parameter till `CreateServiceUri` för att skapa en URI för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-220">The namespace is passed as a parameter to `CreateServiceUri` to create a service URI.</span></span>
2. <span data-ttu-id="a8c82-221">Med hjälp av ett objekt av typen [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) anger du att du kommer att använda en SAS-nyckel som autentiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="a8c82-221">Using a [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) object, declare that you will be using a SAS key as the credential type.</span></span> <span data-ttu-id="a8c82-222">Lägg till följande kod direkt efter den som lades till i det senaste steget.</span><span class="sxs-lookup"><span data-stu-id="a8c82-222">Add the following code directly after the code added in the last step.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-the-service"></a><span data-ttu-id="a8c82-223">Skapa en basadress för tjänsten</span><span class="sxs-lookup"><span data-stu-id="a8c82-223">Create a base address for the service</span></span>

<span data-ttu-id="a8c82-224">Efter den kod som du lade till i det sista steget, skapa en `Uri` instans för tjänstens basadress för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-224">After the code you added in the last step, create a `Uri` instance for the base address of the service.</span></span> <span data-ttu-id="a8c82-225">Den här URI anger Service Bus-schemat, namnområdet och sökvägen för tjänstegränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-225">This URI specifies the Service Bus scheme, the namespace, and the path of the service interface.</span></span>

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

<span data-ttu-id="a8c82-226">"sb" är en förkortning för Service Bus-schemat och anger att vi använder TCP som protokoll.</span><span class="sxs-lookup"><span data-stu-id="a8c82-226">"sb" is an abbreviation for the Service Bus scheme, and indicates that we are using TCP as the protocol.</span></span> <span data-ttu-id="a8c82-227">Detta har även tidigare angetts i konfigurationsfilen när [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) angavs som bindning.</span><span class="sxs-lookup"><span data-stu-id="a8c82-227">This was also previously indicated in the configuration file, when [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) was specified as the binding.</span></span>

<span data-ttu-id="a8c82-228">URI är `sb://putServiceNamespaceHere.windows.net/EchoService` för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-228">For this tutorial, the URI is `sb://putServiceNamespaceHere.windows.net/EchoService`.</span></span>

### <a name="create-and-configure-the-service-host"></a><span data-ttu-id="a8c82-229">Skapa och konfigurera värden för tjänsten</span><span class="sxs-lookup"><span data-stu-id="a8c82-229">Create and configure the service host</span></span>

1. <span data-ttu-id="a8c82-230">Ställ in anslutningsläget på `AutoDetect`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-230">Set the connectivity mode to `AutoDetect`.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    <span data-ttu-id="a8c82-231">Anslutningsläget beskriver det protokoll som tjänsten använder för att kommunicera med den vidarebefordrande tjänsten; antingen HTTP eller TCP.</span><span class="sxs-lookup"><span data-stu-id="a8c82-231">The connectivity mode describes the protocol the service uses to communicate with the relay service; either HTTP or TCP.</span></span> <span data-ttu-id="a8c82-232">Genom att använda standardinställningen `AutoDetect`, försöker tjänsten ansluta till Azure-relä via TCP, om den är tillgänglig och HTTP om TCP inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="a8c82-232">Using the default setting `AutoDetect`, the service attempts to connect to Azure Relay over TCP if it is available, and HTTP if TCP is not available.</span></span> <span data-ttu-id="a8c82-233">Observera att detta skiljer sig från det protokoll som tjänsten anger för klientkommunikation.</span><span class="sxs-lookup"><span data-stu-id="a8c82-233">Note that this differs from the protocol the service specifies for client communication.</span></span> <span data-ttu-id="a8c82-234">Det här protokollet bestäms av den bindning som används.</span><span class="sxs-lookup"><span data-stu-id="a8c82-234">That protocol is determined by the binding used.</span></span> <span data-ttu-id="a8c82-235">En tjänst kan till exempel använda den [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) bindning som anger att dess slutpunkt kommunicerar med klienterna via HTTP.</span><span class="sxs-lookup"><span data-stu-id="a8c82-235">For example, a service can use the [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) binding, which specifies that its endpoint communicates with clients over HTTP.</span></span> <span data-ttu-id="a8c82-236">Att samma tjänst skulle kunna ange **ConnectivityMode.AutoDetect** så att tjänsten kommunicerar med Azure Relay via TCP.</span><span class="sxs-lookup"><span data-stu-id="a8c82-236">That same service could specify **ConnectivityMode.AutoDetect** so that the service communicates with Azure Relay over TCP.</span></span>
2. <span data-ttu-id="a8c82-237">Skapa tjänstevärden med hjälp av den URI som du skapade tidigare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-237">Create the service host, using the URI created earlier in this section.</span></span>

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    <span data-ttu-id="a8c82-238">Tjänstevärden är det WCF-objekt som instantierar tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-238">The service host is the WCF object that instantiates the service.</span></span> <span data-ttu-id="a8c82-239">Här kan du skicka över den typ av tjänst som du vill skapa (en `EchoService`-typ) samt även den adress som du vill exponera tjänsten på.</span><span class="sxs-lookup"><span data-stu-id="a8c82-239">Here, you pass it the type of service you want to create (an `EchoService` type), and also to the address at which you want to expose the service.</span></span>
3. <span data-ttu-id="a8c82-240">Lägg till referenser i [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) och [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description) högst upp i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="a8c82-240">At the top of the Program.cs file, add references to [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) and [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).</span></span>

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. <span data-ttu-id="a8c82-241">När du är tillbaka i `Main()`, konfigurerar du slutpunkten för att ge allmän åtkomst till den.</span><span class="sxs-lookup"><span data-stu-id="a8c82-241">Back in `Main()`, configure the endpoint to enable public access.</span></span>

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    <span data-ttu-id="a8c82-242">Det här steget meddelar den vidarebefordrande tjänsten som programmet kan hittas offentligt genom att undersöka ATOM-feeden för projektet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-242">This step informs the relay service that your application can be found publicly by examining the ATOM feed for your project.</span></span> <span data-ttu-id="a8c82-243">Om du ställer in **DiscoveryType** till **private**, kommer en klient fortfarande att kunna komma åt tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-243">If you set **DiscoveryType** to **private**, a client would still be able to access the service.</span></span> <span data-ttu-id="a8c82-244">Tjänsten skulle dock inte visas när den söker Relay-namnområdet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-244">However, the service would not appear when it searches the Relay namespace.</span></span> <span data-ttu-id="a8c82-245">Klienten måste då i stället känna till sökvägen för slutpunkten på förhand.</span><span class="sxs-lookup"><span data-stu-id="a8c82-245">Instead, the client would have to know the endpoint path beforehand.</span></span>
5. <span data-ttu-id="a8c82-246">Tillämpa autentiseringsuppgifterna för tjänsten på de slutpunkter för tjänsten som finns angivna i filen App.config:</span><span class="sxs-lookup"><span data-stu-id="a8c82-246">Apply the service credentials to the service endpoints defined in the App.config file:</span></span>

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    <span data-ttu-id="a8c82-247">Som vi sa i det förra steget kan du ha deklarerat flera tjänster och slutpunkter i konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-247">As stated in the previous step, you could have declared multiple services and endpoints in the configuration file.</span></span> <span data-ttu-id="a8c82-248">Om du hade gjort detta skulle den här koden ha bläddrat igenom hela konfigurationsfilen och ha sökt efter varje slutpunkt som den skulle ha tillämpat på dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a8c82-248">If you had, this code would traverse the configuration file and search for every endpoint to which it should apply your credentials.</span></span> <span data-ttu-id="a8c82-249">Men i den här självstudiekursen har konfigurationsfilen endast en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-249">However, for this tutorial, the configuration file has only one endpoint.</span></span>

### <a name="open-the-service-host"></a><span data-ttu-id="a8c82-250">Öppna tjänstevärden</span><span class="sxs-lookup"><span data-stu-id="a8c82-250">Open the service host</span></span>

1. <span data-ttu-id="a8c82-251">Öppna tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-251">Open the service.</span></span>

    ```csharp
    host.Open();
    ```
2. <span data-ttu-id="a8c82-252">Informera användaren om att tjänsten körs och förklara hur tjänsten stängs av.</span><span class="sxs-lookup"><span data-stu-id="a8c82-252">Inform the user that the service is running, and explain how to shut down the service.</span></span>

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="a8c82-253">Stäng tjänstevärden när du är klar.</span><span class="sxs-lookup"><span data-stu-id="a8c82-253">When finished, close the service host.</span></span>

    ```csharp
    host.Close();
    ```
4. <span data-ttu-id="a8c82-254">Tryck på **Ctrl+Shift+B** för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-254">Press **Ctrl+Shift+B** to build the project.</span></span>

### <a name="example"></a><span data-ttu-id="a8c82-255">Exempel</span><span class="sxs-lookup"><span data-stu-id="a8c82-255">Example</span></span>

<span data-ttu-id="a8c82-256">Koden för slutförda tjänsten ska visas på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-256">Your completed service code should appear as follows.</span></span> <span data-ttu-id="a8c82-257">Koden innehåller tjänstekontraktet och implementeringen från föregående steg i självstudiekursen och hyser in tjänsten i ett konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="a8c82-257">The code includes the service contract and implementation from previous steps in the tutorial, and hosts the service in a console application.</span></span>

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

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Relay credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a><span data-ttu-id="a8c82-258">Skapa en WCF-klient för tjänstekontraktet</span><span class="sxs-lookup"><span data-stu-id="a8c82-258">Create a WCF client for the service contract</span></span>

<span data-ttu-id="a8c82-259">Nästa steg är att skapa ett klientprogram och definiera det tjänstekontrakt som du kommer att implementera i senare steg.</span><span class="sxs-lookup"><span data-stu-id="a8c82-259">The next step is to create a client application and define the service contract you will implement in later steps.</span></span> <span data-ttu-id="a8c82-260">Observera att många av de här stegen liknar de steg som används för att skapa en tjänst: definiera ett kontrakt, redigera en App.config-fil, använda autentiseringsuppgifter för att ansluta till den vidarebefordrande tjänsten och så vidare.</span><span class="sxs-lookup"><span data-stu-id="a8c82-260">Note that many of these steps resemble the steps used to create a service: defining a contract, editing an App.config file, using credentials to connect to the relay service, and so on.</span></span> <span data-ttu-id="a8c82-261">Den kod som används för dessa arbetsuppgifter visas i exemplet som följer efter proceduren.</span><span class="sxs-lookup"><span data-stu-id="a8c82-261">The code used for these tasks is provided in the example following the procedure.</span></span>

1. <span data-ttu-id="a8c82-262">Skapa ett nytt projekt i den befintliga Visual Studio-lösningen för klienten genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="a8c82-262">Create a new project in the current Visual Studio solution for the client by doing the following:</span></span>

   1. <span data-ttu-id="a8c82-263">Högerklicka på den aktuella lösningen (inte projektet) i Solution Explorer, i samma lösning som innehåller tjänsten, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-263">In Solution Explorer, in the same solution that contains the service, right-click the current solution (not the project), and click **Add**.</span></span> <span data-ttu-id="a8c82-264">Klicka sedan på **Nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-264">Then click **New Project**.</span></span>
   2. <span data-ttu-id="a8c82-265">I den **Lägg till nytt projekt** dialogrutan klickar du på **Visual C#** (om **Visual C#** inte visas, tittar du under **andra språk**), Välj den **Konsolapp (.NET Framework)** mall, och ger den namnet **EchoClient**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-265">In the **Add New Project** dialog box, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**), select the **Console App (.NET Framework)** template, and name it **EchoClient**.</span></span>
   3. <span data-ttu-id="a8c82-266">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-266">Click **OK**.</span></span>
      <br />
2. <span data-ttu-id="a8c82-267">Dubbelklicka på filen Program.cs i projektet **EchoClient** i Solution Explorer för att öppna den i redigeraren, om den inte redan är öppen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-267">In Solution Explorer, double-click the Program.cs file in the **EchoClient** project to open it in the editor, if it is not already open.</span></span>
3. <span data-ttu-id="a8c82-268">Ändra namnet på namnområdet från standardnamnet `EchoClient` till `Microsoft.ServiceBus.Samples`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-268">Change the namespace name from its default name of `EchoClient` to `Microsoft.ServiceBus.Samples`.</span></span>
4. <span data-ttu-id="a8c82-269">Installera den [Service Bus NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus): i Solution Explorer högerklickar du på den **EchoClient** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-269">Install the [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus): in Solution Explorer, right-click the **EchoClient** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="a8c82-270">Klicka på **Bläddra**-fliken och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-270">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="a8c82-271">Klicka på **Installera** och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="a8c82-271">Click **Install**, and accept the terms of use.</span></span>

    ![][3]
5. <span data-ttu-id="a8c82-272">Lägg till ett `using`-uttryck för namnområdet [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="a8c82-272">Add a `using` statement for the [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) namespace in the Program.cs file.</span></span>

    ```csharp
    using System.ServiceModel;
    ```
6. <span data-ttu-id="a8c82-273">Lägga till en definition för tjänstekontraktet i namnområdet, som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="a8c82-273">Add the service contract definition to the namespace, as shown in the following example.</span></span> <span data-ttu-id="a8c82-274">Observera att den här definitionen är identisk med den som används i projektet **Service**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-274">Note that this definition is identical to the definition used in the **Service** project.</span></span> <span data-ttu-id="a8c82-275">Du måste lägga till den här koden längst upp i namnområdet `Microsoft.ServiceBus.Samples`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-275">You should add this code at the top of the `Microsoft.ServiceBus.Samples` namespace.</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. <span data-ttu-id="a8c82-276">Tryck på **Ctrl+Shift+B** för att skapa klienten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-276">Press **Ctrl+Shift+B** to build the client.</span></span>

### <a name="example"></a><span data-ttu-id="a8c82-277">Exempel</span><span class="sxs-lookup"><span data-stu-id="a8c82-277">Example</span></span>

<span data-ttu-id="a8c82-278">Följande kod visar den aktuella statusen för filen Program.cs i den **EchoClient** projekt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-278">The following code shows the current status of the Program.cs file in the **EchoClient** project.</span></span>

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

## <a name="configure-the-wcf-client"></a><span data-ttu-id="a8c82-279">Konfigurera WCF-klienten</span><span class="sxs-lookup"><span data-stu-id="a8c82-279">Configure the WCF client</span></span>

<span data-ttu-id="a8c82-280">I det här steget ska du skapa en App.config-fil för ett grundläggande klientprogrammet som ansluter till den tjänst som skapades tidigare under självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-280">In this step, you create an App.config file for a basic client application that accesses the service created previously in this tutorial.</span></span> <span data-ttu-id="a8c82-281">Den här App.config-filen definierar kontraktet, bindningen och namnet på slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-281">This App.config file defines the contract, binding, and name of the endpoint.</span></span> <span data-ttu-id="a8c82-282">Den kod som används för dessa arbetsuppgifter visas i exemplet som följer efter proceduren.</span><span class="sxs-lookup"><span data-stu-id="a8c82-282">The code used for these tasks is provided in the example following the procedure.</span></span>

1. <span data-ttu-id="a8c82-283">Dubbelklicka på filen **App.config** i projektet **EchoClient** i Solution Explorer för att öppna filen i Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a8c82-283">In Solution Explorer, in the **EchoClient** project, double-click **App.config** to open the file in the Visual Studio editor.</span></span>
2. <span data-ttu-id="a8c82-284">I elementet `<appSettings>` ersätter du platshållarna med namnet på ditt namnområde för tjänsten och den SAS-nyckel som du kopierade i ett av de föregående stegen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-284">In the `<appSettings>` element, replace the placeholders with the name of your service namespace, and the SAS key that you copied in an earlier step.</span></span>
3. <span data-ttu-id="a8c82-285">Lägg till ett `<client>`-element inom elementet  system.serviceModel.</span><span class="sxs-lookup"><span data-stu-id="a8c82-285">Within the system.serviceModel element, add a `<client>` element.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    <span data-ttu-id="a8c82-286">I det här steget anger du att du definierar ett klientprogram i WCF-format.</span><span class="sxs-lookup"><span data-stu-id="a8c82-286">This step declares that you are defining a WCF-style client application.</span></span>
4. <span data-ttu-id="a8c82-287">Definiera namnet, kontraktet och bindningstypen för slutpunkten inom elementet `client`.</span><span class="sxs-lookup"><span data-stu-id="a8c82-287">Within the `client` element, define the name, contract, and binding type for the endpoint.</span></span>

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="a8c82-288">Det här steget definierar namnet på slutpunkten, det kontrakt som definierats i tjänsten och det faktum att klientprogrammet använder TCP för att kommunicera med Azure Relay.</span><span class="sxs-lookup"><span data-stu-id="a8c82-288">This step defines the name of the endpoint, the contract defined in the service, and the fact that the client application uses TCP to communicate with Azure Relay.</span></span> <span data-ttu-id="a8c82-289">Namnet på slutpunkten används i nästa steg för att länka samman denna slutpunktskonfiguration med URI:n för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-289">The endpoint name is used in the next step to link this endpoint configuration with the service URI.</span></span>
5. <span data-ttu-id="a8c82-290">Klicka på **filen**, klicka på **spara alla**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-290">Click **File**, then click **Save All**.</span></span>

## <a name="example"></a><span data-ttu-id="a8c82-291">Exempel</span><span class="sxs-lookup"><span data-stu-id="a8c82-291">Example</span></span>

<span data-ttu-id="a8c82-292">Följande kod visar filen App.config för Echo-klienten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-292">The following code shows the App.config file for the Echo client.</span></span>

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

## <a name="implement-the-wcf-client"></a><span data-ttu-id="a8c82-293">Implementera WCF-klienten</span><span class="sxs-lookup"><span data-stu-id="a8c82-293">Implement the WCF client</span></span>
<span data-ttu-id="a8c82-294">I det här steget ska du implementera ett grundläggande klientprogrammet som ansluter till den tjänst som du skapade tidigare under självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-294">In this step, you implement a basic client application that accesses the service you created previously in this tutorial.</span></span> <span data-ttu-id="a8c82-295">Liknar tjänsten, utför klienten många av samma åtgärder för att komma åt Azure Relay:</span><span class="sxs-lookup"><span data-stu-id="a8c82-295">Similar to the service, the client performs many of the same operations to access Azure Relay:</span></span>

1. <span data-ttu-id="a8c82-296">Ställer in anslutningsläget.</span><span class="sxs-lookup"><span data-stu-id="a8c82-296">Sets the connectivity mode.</span></span>
2. <span data-ttu-id="a8c82-297">Skapar den URI som lokaliserar värdtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-297">Creates the URI that locates the host service.</span></span>
3. <span data-ttu-id="a8c82-298">Definierar säkerhetsautentiseringen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-298">Defines the security credentials.</span></span>
4. <span data-ttu-id="a8c82-299">Tillämpar autentiseringsuppgifterna på anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-299">Applies the credentials to the connection.</span></span>
5. <span data-ttu-id="a8c82-300">Öppnar anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-300">Opens the connection.</span></span>
6. <span data-ttu-id="a8c82-301">Utför de programspecifika uppgifterna.</span><span class="sxs-lookup"><span data-stu-id="a8c82-301">Performs the application-specific tasks.</span></span>
7. <span data-ttu-id="a8c82-302">Stänger ned anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-302">Closes the connection.</span></span>

<span data-ttu-id="a8c82-303">En av de viktigaste skillnaderna är dock att klientprogrammet använder en kanal för att ansluta till den vidarebefordrande tjänsten medan tjänsten använder ett anrop till **ServiceHost**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-303">However, one of the main differences is that the client application uses a channel to connect to the relay service, whereas the service uses a call to **ServiceHost**.</span></span> <span data-ttu-id="a8c82-304">Den kod som används för dessa arbetsuppgifter visas i exemplet som följer efter proceduren.</span><span class="sxs-lookup"><span data-stu-id="a8c82-304">The code used for these tasks is provided in the example following the procedure.</span></span>

### <a name="implement-a-client-application"></a><span data-ttu-id="a8c82-305">Implementera ett klientprogram</span><span class="sxs-lookup"><span data-stu-id="a8c82-305">Implement a client application</span></span>
1. <span data-ttu-id="a8c82-306">Ställ in anslutningsläget på **AutoDetect**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-306">Set the connectivity mode to **AutoDetect**.</span></span> <span data-ttu-id="a8c82-307">Lägg till följande kod inne i `Main()`-metoden för **EchoClient**-programmet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-307">Add the following code inside the `Main()` method of the **EchoClient** application.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. <span data-ttu-id="a8c82-308">Definiera variabler för att lagra värdena för tjänstens namnområde och den SAS-nyckel som läses från konsolen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-308">Define variables to hold the values for the service namespace, and SAS key that are read from the console.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. <span data-ttu-id="a8c82-309">Skapa den URI som definierar platsen för värden i projektet Relay.</span><span class="sxs-lookup"><span data-stu-id="a8c82-309">Create the URI that defines the location of the host in your Relay project.</span></span>

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. <span data-ttu-id="a8c82-310">Skapa autentiseringsobjektet för slutpunkten för tjänstens namnområde.</span><span class="sxs-lookup"><span data-stu-id="a8c82-310">Create the credential object for your service namespace endpoint.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. <span data-ttu-id="a8c82-311">Skapa den kanalfabrik som läser in konfigurationen som beskrivs i filen App.config.</span><span class="sxs-lookup"><span data-stu-id="a8c82-311">Create the channel factory that loads the configuration described in the App.config file.</span></span>

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    <span data-ttu-id="a8c82-312">En kanalfabriken är ett WCF-objekt som skapar en kanal via vilken tjänsten och klientprogrammet kan kommunicera.</span><span class="sxs-lookup"><span data-stu-id="a8c82-312">A channel factory is a WCF object that creates a channel through which the service and client applications communicate.</span></span>
6. <span data-ttu-id="a8c82-313">Tillämpa autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="a8c82-313">Apply the credentials.</span></span>

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. <span data-ttu-id="a8c82-314">Skapa och öppna kanalen till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-314">Create and open the channel to the service.</span></span>

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. <span data-ttu-id="a8c82-315">Skriv det grundläggande gränssnittet och funktionerna för ekot.</span><span class="sxs-lookup"><span data-stu-id="a8c82-315">Write the basic user interface and functionality for the echo.</span></span>

    ```csharp
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
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

    <span data-ttu-id="a8c82-316">Observera att koden använder kanalobjektets instans som proxy för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-316">Note that the code uses the instance of the channel object as a proxy for the service.</span></span>
9. <span data-ttu-id="a8c82-317">Stäng kanalen och stäng fabriken.</span><span class="sxs-lookup"><span data-stu-id="a8c82-317">Close the channel, and close the factory.</span></span>

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

## <a name="example"></a><span data-ttu-id="a8c82-318">Exempel</span><span class="sxs-lookup"><span data-stu-id="a8c82-318">Example</span></span>

<span data-ttu-id="a8c82-319">Den färdiga koden ska se ut så här visar hur du skapar ett klientprogram, hur du anropar tjänstens funktioner för tjänsten och hur du stänger klienten när funktionsanropet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a8c82-319">Your completed code should appear as follows, showing how to create a client application, how to call the operations of the service, and how to close the client after the operation call is finished.</span></span>

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

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
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

## <a name="run-the-applications"></a><span data-ttu-id="a8c82-320">Köra programmen</span><span class="sxs-lookup"><span data-stu-id="a8c82-320">Run the applications</span></span>

1. <span data-ttu-id="a8c82-321">Tryck på **Ctrl+Shift+B** för att skapa lösningen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-321">Press **Ctrl+Shift+B** to build the solution.</span></span> <span data-ttu-id="a8c82-322">Detta skapar både det klientprojekt och det tjänsteprojekt som du skapade i det förra steget.</span><span class="sxs-lookup"><span data-stu-id="a8c82-322">This builds both the client project and the service project that you created in the previous steps.</span></span>
2. <span data-ttu-id="a8c82-323">Du måste se till att tjänsteprogrammet körs innan du startar klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-323">Before running the client application, you must make sure that the service application is running.</span></span> <span data-ttu-id="a8c82-324">Högerklicka på lösningen **EchoService** och klicka sedan på **Egenskaper** i Solution Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8c82-324">In Solution Explorer in Visual Studio, right-click the **EchoService** solution, then click **Properties**.</span></span>
3. <span data-ttu-id="a8c82-325">Klicka på **Startprojekt** och sedan på knappen **Flera startprojekt** i dialogrutan för lösningsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="a8c82-325">In the solution properties dialog box, click **Startup Project**, then click the **Multiple startup projects** button.</span></span> <span data-ttu-id="a8c82-326">Kontrollera att **EchoService** visas först i listan.</span><span class="sxs-lookup"><span data-stu-id="a8c82-326">Make sure **EchoService** appears first in the list.</span></span>
4. <span data-ttu-id="a8c82-327">Ställ in rutan **Åtgärd** för både **EchoService**- och **EchoClient**-projektet på **Starta**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-327">Set the **Action** box for both the **EchoService** and **EchoClient** projects to **Start**.</span></span>

    ![][5]
5. <span data-ttu-id="a8c82-328">Klicka på **Projektberoenden**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-328">Click **Project Dependencies**.</span></span> <span data-ttu-id="a8c82-329">Markera **EchoClient** i rutan **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-329">In the **Projects** box, select **EchoClient**.</span></span> <span data-ttu-id="a8c82-330">Se till att **EchoService** är markerad i rutan **Är beroende av**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-330">In the **Depends on** box, make sure **EchoService** is checked.</span></span>

    ![][6]
6. <span data-ttu-id="a8c82-331">Klicka på **OK** för att stänga dialogrutan **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-331">Click **OK** to dismiss the **Properties** dialog.</span></span>
7. <span data-ttu-id="a8c82-332">Tryck på **F5** att köra båda projekten.</span><span class="sxs-lookup"><span data-stu-id="a8c82-332">Press **F5** to run both projects.</span></span>
8. <span data-ttu-id="a8c82-333">Båda konsolfönstren öppnas och uppmanar dig att ange namnet på namnområdet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-333">Both console windows open and prompt you for the namespace name.</span></span> <span data-ttu-id="a8c82-334">Tjänsten måste köras först och därför anger du namnområdet i konsolfönstret för **EchoService** och sedan trycker du på **Retur**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-334">The service must run first, so in the **EchoService** console window, enter the namespace and then press **Enter**.</span></span>
9. <span data-ttu-id="a8c82-335">Därefter uppmanas du ange din SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="a8c82-335">Next, you are prompted for your SAS key.</span></span> <span data-ttu-id="a8c82-336">Ange SAS-nyckeln och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="a8c82-336">Enter the SAS key and press ENTER.</span></span>

    <span data-ttu-id="a8c82-337">Här är exempel på vad som kan matas ut från konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="a8c82-337">Here is example output from the console window.</span></span> <span data-ttu-id="a8c82-338">Observera att de värdena som anges här endast visas i exempelsyfte.</span><span class="sxs-lookup"><span data-stu-id="a8c82-338">Note that the values provided here are for example purposes only.</span></span>

    <span data-ttu-id="a8c82-339">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span><span class="sxs-lookup"><span data-stu-id="a8c82-339">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span></span>

    <span data-ttu-id="a8c82-340">Tjänsteprogrammet skriver till konsolfönstret, till adressen som den lyssnar på, som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="a8c82-340">The service application prints to the console window the address on which it's listening, as seen in the following example.</span></span>

    <span data-ttu-id="a8c82-341">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] to exit`</span><span class="sxs-lookup"><span data-stu-id="a8c82-341">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] to exit`</span></span>
10. <span data-ttu-id="a8c82-342">Ange samma information som du angav tidigare för tjänsteprogrammet i konsolfönstret för **EchoClient**.</span><span class="sxs-lookup"><span data-stu-id="a8c82-342">In the **EchoClient** console window, enter the same information that you entered previously for the service application.</span></span> <span data-ttu-id="a8c82-343">Följ de föregående stegen för att ange samma värden för namnområde för tjänsten och SAS-nyckeln för klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a8c82-343">Follow the previous steps to enter the same service namespace and SAS key values for the client application.</span></span>
11. <span data-ttu-id="a8c82-344">När du har angett dessa värden, öppnar klienten en kanal till tjänsten och du uppmanas att ange lite text. Följ anvisningarna i följande utmatningsexempel för konsolen.</span><span class="sxs-lookup"><span data-stu-id="a8c82-344">After entering these values, the client opens a channel to the service and prompts you to enter some text as seen in the following console output example.</span></span>

    `Enter text to echo (or [Enter] to exit):`

    <span data-ttu-id="a8c82-345">Ange text för att skicka den till tjänsteprogrammet och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="a8c82-345">Enter some text to send to the service application and press Enter.</span></span> <span data-ttu-id="a8c82-346">Texten skickas till tjänsten via tjänsteåtgärden Echo och visas i tjänstekonsolfönstret, som i följande exempelutmatning.</span><span class="sxs-lookup"><span data-stu-id="a8c82-346">This text is sent to the service through the Echo service operation and appears in the service console window as in the following example output.</span></span>

    `Echoing: My sample text`

    <span data-ttu-id="a8c82-347">Klientprogrammet får returvärdet för `Echo`-åtgärden, som är den ursprungliga texten, och skriver ut denna i sitt konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="a8c82-347">The client application receives the return value of the `Echo` operation, which is the original text, and prints it to its console window.</span></span> <span data-ttu-id="a8c82-348">Följande är ett utmatningsexempel från klientens konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="a8c82-348">The following is example output from the client console window.</span></span>

    `Server echoed: My sample text`
12. <span data-ttu-id="a8c82-349">Du kan fortsätta att skicka textmeddelanden från klienten till tjänsten på detta sätt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-349">You can continue sending text messages from the client to the service in this manner.</span></span> <span data-ttu-id="a8c82-350">Tryck på Retur i konsolfönstren för klienten och tjänsten för att stänga båda programmen när du är klar.</span><span class="sxs-lookup"><span data-stu-id="a8c82-350">When you are finished, press Enter in the client and service console windows to end both applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8c82-351">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8c82-351">Next steps</span></span>

<span data-ttu-id="a8c82-352">Den här självstudiekursen visades hur du skapar en Relay i Azure-klient och en tjänst med hjälp av funktionerna i Service Bus Relay WCF.</span><span class="sxs-lookup"><span data-stu-id="a8c82-352">This tutorial showed how to build an Azure Relay client application and service using the WCF Relay capabilities of Service Bus.</span></span> <span data-ttu-id="a8c82-353">För en liknande självstudiekurs som använder [Service Bus-meddelanden](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), se [komma igång med Service Bus-köer](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="a8c82-353">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="a8c82-354">Mer information om Azure Relay finns i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8c82-354">To learn more about Azure Relay, see the following topics.</span></span>

* [<span data-ttu-id="a8c82-355">Azure Service Bus-Arkitekturöversikt</span><span class="sxs-lookup"><span data-stu-id="a8c82-355">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [<span data-ttu-id="a8c82-356">Översikt över Azure Relay</span><span class="sxs-lookup"><span data-stu-id="a8c82-356">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="a8c82-357">Hur du använder tjänsten WCF relay med .NET</span><span class="sxs-lookup"><span data-stu-id="a8c82-357">How to use the WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure classic portal]: http://manage.windowsazure.com

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
