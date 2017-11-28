---
title: "Service Bus REST-självstudiekurs med hjälp av Azure Relay | Microsoft Docs"
description: "Skapa en enkel Azure Service Bus relay värdapp som visar ett REST-baserat gränssnitt."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1312b2db-94c4-4a48-b815-c5deb5b77a6a
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2017
ms.author: sethm
ms.openlocfilehash: 0db9dbd2d2743907e3f0b259228201d4f5d0c3c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-wcf-relay-rest-tutorial"></a><span data-ttu-id="d25c4-103">Självstudiekurs för Azure WCF Relay REST</span><span class="sxs-lookup"><span data-stu-id="d25c4-103">Azure WCF Relay REST tutorial</span></span>

<span data-ttu-id="d25c4-104">Den här självstudiekursen beskrivs hur du skapar en enkel värdapp för Azure Relay som visar ett REST-baserat gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="d25c4-104">This tutorial describes how to build a simple Azure Relay host application that exposes a REST-based interface.</span></span> <span data-ttu-id="d25c4-105">Med hjälp av REST kan du ge en webbklient, till exempel en webbläsare, åtkomst till API:er för Service Bus via HTTP-förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="d25c4-105">REST enables a web client, such as a web browser, to access the Service Bus APIs through HTTP requests.</span></span>

<span data-ttu-id="d25c4-106">I självstudiekursen används Windows Communication Foundation (WCF) RESTEN programmeringsmodell för att skapa ett REST-tjänst på Service Bus.</span><span class="sxs-lookup"><span data-stu-id="d25c4-106">The tutorial uses the Windows Communication Foundation (WCF) REST programming model to construct a REST service on Service Bus.</span></span> <span data-ttu-id="d25c4-107">Mer information finns i [Programmeringsmodellen WCF REST](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) och [Utforma och implementera tjänster](/dotnet/framework/wcf/designing-and-implementing-services) i WCF-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="d25c4-107">For more information, see [WCF REST Programming Model](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) and [Designing and Implementing Services](/dotnet/framework/wcf/designing-and-implementing-services) in the WCF documentation.</span></span>

## <a name="step-1-create-a-namespace"></a><span data-ttu-id="d25c4-108">Steg 1: Skapa ett namnområde</span><span class="sxs-lookup"><span data-stu-id="d25c4-108">Step 1: Create a namespace</span></span>

<span data-ttu-id="d25c4-109">För att komma igång med reläfunktionerna i Azure måste du först skapa ett namnområde för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d25c4-109">To begin using the relay features in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="d25c4-110">Ett namnområde tillhandahåller en omfångsbehållare för adressering av Azure-resurser i ditt program.</span><span class="sxs-lookup"><span data-stu-id="d25c4-110">A namespace provides a scoping container for addressing Azure resources within your application.</span></span> <span data-ttu-id="d25c4-111">Följ [anvisningarna här](relay-create-namespace-portal.md) för att skapa ett Relay-namnområde.</span><span class="sxs-lookup"><span data-stu-id="d25c4-111">Follow the [instructions here](relay-create-namespace-portal.md) to create a Relay namespace.</span></span>

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-azure-relay"></a><span data-ttu-id="d25c4-112">Steg 2: Definiera en REST-baserat WCF-tjänstekontrakt för användning med Azure-relä</span><span class="sxs-lookup"><span data-stu-id="d25c4-112">Step 2: Define a REST-based WCF service contract to use with Azure Relay</span></span>

<span data-ttu-id="d25c4-113">När du skapar en tjänst för WCF REST-format, måste du definiera kontraktet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-113">When you create a WCF REST-style service, you must define the contract.</span></span> <span data-ttu-id="d25c4-114">Kontraktet anger vilka åtgärder som värden stöder.</span><span class="sxs-lookup"><span data-stu-id="d25c4-114">The contract specifies what operations the host supports.</span></span> <span data-ttu-id="d25c4-115">En tjänsteåtgärd kan anses vara en webbtjänstemetod.</span><span class="sxs-lookup"><span data-stu-id="d25c4-115">A service operation can be thought of as a web service method.</span></span> <span data-ttu-id="d25c4-116">Kontrakt skapas genom att definiera ett gränssnitt för C++, C# eller Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d25c4-116">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="d25c4-117">Varje metod i gränssnittet motsvarar en viss tjänsteåtgärd.</span><span class="sxs-lookup"><span data-stu-id="d25c4-117">Each method in the interface corresponds to a specific service operation.</span></span> <span data-ttu-id="d25c4-118">Attributet [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) måste tillämpas på varje gränssnitt och attributet [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) måste tillämpas på varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="d25c4-118">The [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute must be applied to each interface, and the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute must be applied to each operation.</span></span> <span data-ttu-id="d25c4-119">Om en metod i ett gränssnitt som har [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) inte har [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), är inte den metoden exponerad.</span><span class="sxs-lookup"><span data-stu-id="d25c4-119">If a method in an interface that has the [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) does not have the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), that method is not exposed.</span></span> <span data-ttu-id="d25c4-120">Den kod som används för dessa aktiviteter visas i exemplet som följer efter proceduren.</span><span class="sxs-lookup"><span data-stu-id="d25c4-120">The code used for these tasks is shown in the example following the procedure.</span></span>

<span data-ttu-id="d25c4-121">Den viktigaste skillnaden mellan en WCF-kontrakt och ett kontrakt i REST-format är att lägga till en egenskap för den [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span><span class="sxs-lookup"><span data-stu-id="d25c4-121">The primary difference between a WCF contract and a REST-style contract is the addition of a property to the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span></span> <span data-ttu-id="d25c4-122">Den här egenskapen gör att du kan mappa en metod i gränssnittet till en metod på andra sidan av gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-122">This property enables you to map a method in your interface to a method on the other side of the interface.</span></span> <span data-ttu-id="d25c4-123">I det här fallet kommer vi att använda [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) för att länka en metod och hämta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="d25c4-123">In this case, we will use [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) to link a method to HTTP GET.</span></span> <span data-ttu-id="d25c4-124">Detta gör att Service Bus kan hämta och tolka kommandon som skickas till gränssnittet på ett korrekt sätt.</span><span class="sxs-lookup"><span data-stu-id="d25c4-124">This allows Service Bus to accurately retrieve and interpret commands sent to the interface.</span></span>

### <a name="to-create-a-contract-with-an-interface"></a><span data-ttu-id="d25c4-125">Skapa ett kontrakt med ett gränssnitt</span><span class="sxs-lookup"><span data-stu-id="d25c4-125">To create a contract with an interface</span></span>

1. <span data-ttu-id="d25c4-126">Öppna Visual Studio som administratör: Högerklicka på programmet i **Start**-menyn och klicka sedan på **Kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-126">Open Visual Studio as an administrator: right-click the program in the **Start** menu, and then click **Run as administrator**.</span></span>
2. <span data-ttu-id="d25c4-127">Skapa ett nytt konsolapprojekt.</span><span class="sxs-lookup"><span data-stu-id="d25c4-127">Create a new console application project.</span></span> <span data-ttu-id="d25c4-128">Klicka på **Arkiv**-menyn, välj **Nytt** och sedan **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-128">Click the **File** menu and select **New**, then select **Project**.</span></span> <span data-ttu-id="d25c4-129">I dialogrutan **Nytt projekt** klickar du på **Visual C#**, väljer mallen **Konsolprogram** och ger den namnet **ImageListener**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-129">In the **New Project** dialog box, click **Visual C#**, select the **Console Application** template, and name it **ImageListener**.</span></span> <span data-ttu-id="d25c4-130">Använd standardinställningen **Plats**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-130">Use the default **Location**.</span></span> <span data-ttu-id="d25c4-131">Klicka på **OK** för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-131">Click **OK** to create the project.</span></span>
3. <span data-ttu-id="d25c4-132">Visual Studio skapar en `Program.cs`-fil för ett C#-projekt.</span><span class="sxs-lookup"><span data-stu-id="d25c4-132">For a C# project, Visual Studio creates a `Program.cs` file.</span></span> <span data-ttu-id="d25c4-133">Den här klassen innehåller en tom `Main()`-metod, ett krav för att ett konsolapprojekt ska kunna skapas på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="d25c4-133">This class contains an empty `Main()` method, required for a console application project to build correctly.</span></span>
4. <span data-ttu-id="d25c4-134">Lägg till referenser till Service Bus och **System.ServiceModel.dll** till projektet genom att installera Service Bus NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-134">Add references to Service Bus and **System.ServiceModel.dll** to the project by installing the Service Bus NuGet package.</span></span> <span data-ttu-id="d25c4-135">Det här paketet lägger automatiskt till referenser till Service Bus-bibliotek, samt även WCF **System.ServiceModel**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-135">This package automatically adds references to the Service Bus libraries, as well as the WCF **System.ServiceModel**.</span></span> <span data-ttu-id="d25c4-136">Högerklicka på projektet **ImageListener** i Solution Explorer och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-136">In Solution Explorer, right-click the **ImageListener** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="d25c4-137">Klicka på **Bläddra**-fliken och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="d25c4-137">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="d25c4-138">Klicka på **Installera** och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="d25c4-138">Click **Install**, and accept the terms of use.</span></span>
5. <span data-ttu-id="d25c4-139">Du måste uttryckligen lägga till en referens för **System.ServiceModel.Web.dll** i projektet:</span><span class="sxs-lookup"><span data-stu-id="d25c4-139">You must explicitly add a reference to **System.ServiceModel.Web.dll** to the project:</span></span>
   
    <span data-ttu-id="d25c4-140">a.</span><span class="sxs-lookup"><span data-stu-id="d25c4-140">a.</span></span> <span data-ttu-id="d25c4-141">Högerklicka på mappen **Referenser** i Solution Explorer, under projektmappen, och klicka sedan på **Lägg till referens**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-141">In Solution Explorer, right-click the **References** folder under the project folder and then click **Add Reference**.</span></span>
   
    <span data-ttu-id="d25c4-142">b.</span><span class="sxs-lookup"><span data-stu-id="d25c4-142">b.</span></span> <span data-ttu-id="d25c4-143">I dialogrutan **Lägg till referens** klickar du på fliken **Framework** till vänster. Sedan skriver du in **System.ServiceModel.Web** i rutan **Sök**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-143">In the **Add Reference** dialog box, click the **Framework** tab on the left-hand side and in the **Search** box, type **System.ServiceModel.Web**.</span></span> <span data-ttu-id="d25c4-144">Markera kryssrutan **System.ServiceModel.Web** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-144">Select the **System.ServiceModel.Web** check box, then click **OK**.</span></span>
6. <span data-ttu-id="d25c4-145">Lägg till följande `using`-uttryck högst upp i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="d25c4-145">Add the following `using` statements at the top of the Program.cs file.</span></span>
   
    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    <span data-ttu-id="d25c4-146">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) är det namnområde som genom programmering ger åtkomst till grundläggande funktioner i WCF.</span><span class="sxs-lookup"><span data-stu-id="d25c4-146">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is the namespace that enables programmatic access to basic features of WCF.</span></span> <span data-ttu-id="d25c4-147">Vidarebefordrande WCF använder många av objekt och attribut i WCF för att definiera tjänstekontrakt.</span><span class="sxs-lookup"><span data-stu-id="d25c4-147">WCF Relay uses many of the objects and attributes of WCF to define service contracts.</span></span> <span data-ttu-id="d25c4-148">Du använder det här namnområdet i de flesta relay-appar.</span><span class="sxs-lookup"><span data-stu-id="d25c4-148">You will use this namespace in most of your relay applications.</span></span> <span data-ttu-id="d25c4-149">På liknande sätt [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) hjälper dig att definiera kanalen, vilket är det objekt som du kommunicera med Azure Relay och klientens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d25c4-149">Similarly, [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) helps define the channel, which is the object through which you communicate with Azure Relay and the client web browser.</span></span> <span data-ttu-id="d25c4-150">Slutligen är det [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) som innehåller de typer som gör att du kan skapa webbaserade appar.</span><span class="sxs-lookup"><span data-stu-id="d25c4-150">Finally, [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) contains the types that enable you to create web-based applications.</span></span>
7. <span data-ttu-id="d25c4-151">Byt namn på `ImageListener` namnområdet till **Microsoft.ServiceBus.Samples**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-151">Rename the `ImageListener` namespace to **Microsoft.ServiceBus.Samples**.</span></span>
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. <span data-ttu-id="d25c4-152">Direkt efter den inledande klammerparentesen för namnområdesdeklarationen definierar du ett nytt gränssnitt med namnet **IImageContract** och sedan tillämpar du attributet **ServiceContractAttribute** på gränssnittet med ett värde av `http://samples.microsoft.com/ServiceModel/Relay/`.</span><span class="sxs-lookup"><span data-stu-id="d25c4-152">Directly after the opening curly brace of the namespace declaration, define a new interface named **IImageContract** and apply the **ServiceContractAttribute** attribute to the interface with a value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="d25c4-153">Namnområdesvärdet skiljer sig från det namnområde som du använder under hela intervallet för din kod.</span><span class="sxs-lookup"><span data-stu-id="d25c4-153">The namespace value differs from the namespace that you use throughout the scope of your code.</span></span> <span data-ttu-id="d25c4-154">Namnområdesvärdet används som en unik identifierare för det här kontraktet och bör innehålla versionsinformation.</span><span class="sxs-lookup"><span data-stu-id="d25c4-154">The namespace value is used as a unique identifier for this contract, and should have version information.</span></span> <span data-ttu-id="d25c4-155">Mer information finns i [Versionshantering för tjänster](http://go.microsoft.com/fwlink/?LinkID=180498).</span><span class="sxs-lookup"><span data-stu-id="d25c4-155">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="d25c4-156">Genom att ange namnområdet uttryckligen förhindrar du att det förvalda namnområdesvärdet läggs till i kontraktnamnet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-156">Specifying the namespace explicitly prevents the default namespace value from being added to the contract name.</span></span>
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. <span data-ttu-id="d25c4-157">Inne i gränssnittet `IImageContract` deklarerar du en metod för den enkla åtgärd som `IImageContract`-kontraktet exponerar i gränssnittet. Sedan tillämpar du attributet `OperationContractAttribute` på den metod som du vill exponera som en del av det offentliga Service Bus-kontraktet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-157">Within the `IImageContract` interface, declare a method for the single operation the `IImageContract` contract exposes in the interface and apply the `OperationContractAttribute` attribute to the method that you want to expose as part of the public Service Bus contract.</span></span>
   
    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. <span data-ttu-id="d25c4-158">Lägg till **WebGet**-värdet i attributet **OperationContract**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-158">In the **OperationContract** attribute, add the **WebGet** value.</span></span>
    
    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    <span data-ttu-id="d25c4-159">Gör så gör den vidarebefordrande tjänsten på vägen HTTP GET-begäranden till `GetImage`, och översätta returvärdena för `GetImage` till en HTTP GETRESPONSE svaret.</span><span class="sxs-lookup"><span data-stu-id="d25c4-159">Doing so enables the relay service to route HTTP GET requests to `GetImage`, and to translate the return values of `GetImage` into an HTTP GETRESPONSE reply.</span></span> <span data-ttu-id="d25c4-160">Lite längre fram i denna självstudiekurs kommer du att använda en webbläsare för att få tillgång till denna metod. Sedan visar du bilden i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d25c4-160">Later in the tutorial, you will use a web browser to access this method, and to display the image in the browser.</span></span>
11. <span data-ttu-id="d25c4-161">Direkt efter definitionen `IImageContract` deklarerar du en kanal som ärver från både `IImageContract`- och `IClientChannel`-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-161">Directly after the `IImageContract` definition, declare a channel that inherits from both the `IImageContract` and `IClientChannel` interfaces.</span></span>
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    <span data-ttu-id="d25c4-162">En kanal är det WCF-objekt via vilken tjänsten och klienten skickar information till varandra.</span><span class="sxs-lookup"><span data-stu-id="d25c4-162">A channel is the WCF object through which the service and client pass information to each other.</span></span> <span data-ttu-id="d25c4-163">Lite längre fram kommer du att skapa kanalen i din värdapp.</span><span class="sxs-lookup"><span data-stu-id="d25c4-163">Later, you will create the channel in your host application.</span></span> <span data-ttu-id="d25c4-164">Azure Relay sedan använder den här kanalen för att skicka HTTP GET-förfrågningar från webbläsaren till dina **GetImage** implementering.</span><span class="sxs-lookup"><span data-stu-id="d25c4-164">Azure Relay then uses this channel to pass the HTTP GET requests from the browser to your **GetImage** implementation.</span></span> <span data-ttu-id="d25c4-165">Vidarebefordran använder också kanalen för att ta den **GetImage** returvärde och omvandla det till en HTTP GETRESPONSE för klientens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d25c4-165">The relay also uses the channel to take the **GetImage** return value and translate it into an HTTP GETRESPONSE for the client browser.</span></span>
12. <span data-ttu-id="d25c4-166">Från menyn **Skapa** klickar du på **Skapa lösning** för att bekräfta att det arbete du utfört hittills är korrekt.</span><span class="sxs-lookup"><span data-stu-id="d25c4-166">From the **Build** menu, click **Build Solution** to confirm the accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="d25c4-167">Exempel</span><span class="sxs-lookup"><span data-stu-id="d25c4-167">Example</span></span>
<span data-ttu-id="d25c4-168">Följande kod visar ett grundläggande gränssnitt som definierar ett vidarebefordrande WCF-kontrakt.</span><span class="sxs-lookup"><span data-stu-id="d25c4-168">The following code shows a basic interface that defines a WCF Relay contract.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a><span data-ttu-id="d25c4-169">Steg 3: Implementera ett REST-baserat WCF-tjänstekontrakt för användning av Service Bus</span><span class="sxs-lookup"><span data-stu-id="d25c4-169">Step 3: Implement a REST-based WCF service contract to use Service Bus</span></span>
<span data-ttu-id="d25c4-170">Skapa ett REST-format WCF vidarebefordrande tjänsten måste du först skapa det kontrakt som definieras med hjälp av ett gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="d25c4-170">Creating a REST-style WCF Relay service requires that you first create the contract, which is defined by using an interface.</span></span> <span data-ttu-id="d25c4-171">Nästa steg är att implementera gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-171">The next step is to implement the interface.</span></span> <span data-ttu-id="d25c4-172">Detta innebär att du skapar en klass som heter **ImageService** som implementerar det användardefinierade **IImageContract**-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-172">This involves creating a class named **ImageService** that implements the user-defined **IImageContract** interface.</span></span> <span data-ttu-id="d25c4-173">Efter att du har implementerat kontraktet, konfigurerar du gränssnittet genom att använda en App.config-fil.</span><span class="sxs-lookup"><span data-stu-id="d25c4-173">After you implement the contract, you then configure the interface using an App.config file.</span></span> <span data-ttu-id="d25c4-174">Konfigurationsfilen innehåller information som behövs för programmet, till exempel namnet på tjänsten, namnet på kontraktet och vilken typ av protokoll som används för att kommunicera med den vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d25c4-174">The configuration file contains necessary information for the application, such as the name of the service, the name of the contract, and the type of protocol that is used to communicate with the relay service.</span></span> <span data-ttu-id="d25c4-175">Den kod som används för dessa arbetsuppgifter visas i exemplet som följer efter proceduren.</span><span class="sxs-lookup"><span data-stu-id="d25c4-175">The code used for these tasks is provided in the example following the procedure.</span></span>

<span data-ttu-id="d25c4-176">Det finns mycket lite skillnad mellan att implementera ett kontrakt i REST-format och ett vidarebefordrande WCF-kontrakt som föregående steg.</span><span class="sxs-lookup"><span data-stu-id="d25c4-176">As with the previous steps, there is very little difference between implementing a REST-style contract and a WCF Relay contract.</span></span>

### <a name="to-implement-a-rest-style-service-bus-contract"></a><span data-ttu-id="d25c4-177">Implementera ett Service Bus-kontrakt i REST-format</span><span class="sxs-lookup"><span data-stu-id="d25c4-177">To implement a REST-style Service Bus contract</span></span>
1. <span data-ttu-id="d25c4-178">Skapa en ny klass med namnet **ImageService** direkt efter definitionen av gränssnittet **IImageContract**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-178">Create a new class named **ImageService** directly after the definition of the **IImageContract** interface.</span></span> <span data-ttu-id="d25c4-179">Klassen **ImageService** implementerar gränssnittet **IImageContract**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-179">The **ImageService** class implements the **IImageContract** interface.</span></span>
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    <span data-ttu-id="d25c4-180">Precis som med andra gränssnittsimplementeringar kan du implementera definitionen i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="d25c4-180">Similar to other interface implementations, you can implement the definition in a different file.</span></span> <span data-ttu-id="d25c4-181">Men i den här självstudiekursen visas implementeringen i samma fil som gränssnittsdefinitionen och `Main()`-metoden.</span><span class="sxs-lookup"><span data-stu-id="d25c4-181">However, for this tutorial, the implementation appears in the same file as the interface definition and `Main()` method.</span></span>
2. <span data-ttu-id="d25c4-182">Tillämpa attributet [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) i klassen **IImageService** för att ange att klassen är en implementering av ett WCF-kontrakt.</span><span class="sxs-lookup"><span data-stu-id="d25c4-182">Apply the [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute to the **IImageService** class to indicate that the class is an implementation of a WCF contract.</span></span>
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    <span data-ttu-id="d25c4-183">Som tidigare nämnts är det här namnområdet inte ett traditionellt namnområde.</span><span class="sxs-lookup"><span data-stu-id="d25c4-183">As mentioned previously, this namespace is not a traditional namespace.</span></span> <span data-ttu-id="d25c4-184">Det är istället en del av WCF-arkitekturen som identifierar kontraktet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-184">Instead, it is part of the WCF architecture that identifies the contract.</span></span> <span data-ttu-id="d25c4-185">Mer information finns i [Datakontraktnamn](https://msdn.microsoft.com/library/ms731045.aspx) i WCF-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="d25c4-185">For more information, see the [Data Contract Names](https://msdn.microsoft.com/library/ms731045.aspx) topic in the WCF documentation.</span></span>
3. <span data-ttu-id="d25c4-186">Lägg till en .jpg-bild i projektet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-186">Add a .jpg image to your project.</span></span>  
   
    <span data-ttu-id="d25c4-187">Det här är en bild som tjänsten visar i den mottagande webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d25c4-187">This is a picture that the service displays in the receiving browser.</span></span> <span data-ttu-id="d25c4-188">Högerklicka på ditt projekt och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-188">Right-click your project, then click **Add**.</span></span> <span data-ttu-id="d25c4-189">Klicka därefter på **Befintligt objekt**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-189">Then click **Existing Item**.</span></span> <span data-ttu-id="d25c4-190">Använd dialogrutan **Lägg till befintligt objekt** för att bläddra till en lämplig .jpg-bild och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-190">Use the **Add Existing Item** dialog box to browse to an appropriate .jpg, and then click **Add**.</span></span>
   
    <span data-ttu-id="d25c4-191">När du lägger till filen, måste du se till att **Alla filer** är markerad i listrutan bredvid fältet **Filnamn:**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-191">When adding the file, make sure that **All Files** is selected in the drop-down list next to the **File name:** field.</span></span> <span data-ttu-id="d25c4-192">I resten av den här självstudiekursen förutsätter vi att namnet på denna bild är ”image.jpg”.</span><span class="sxs-lookup"><span data-stu-id="d25c4-192">The rest of this tutorial assumes that the name of the image is "image.jpg".</span></span> <span data-ttu-id="d25c4-193">Om du har en annan fil måste du byta namn på bilden, eller ändra koden för att kompensera för detta.</span><span class="sxs-lookup"><span data-stu-id="d25c4-193">If you have a different file, you will have to rename the image, or change your code to compensate.</span></span>
4. <span data-ttu-id="d25c4-194">För att försäkra dig om att den tjänst som körs kan hitta bildfilen kan du högerklicka på bildfilen i **Solution Explorer** och sedan klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-194">To make sure that the running service can find the image file, in **Solution Explorer** right-click the image file, then click **Properties**.</span></span> <span data-ttu-id="d25c4-195">I rutan **Egenskaper** ställer du in **Kopiera till utdatakatalog** på **Kopiera om nyare**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-195">In the **Properties** pane, set **Copy to Output Directory** to **Copy if newer**.</span></span>
5. <span data-ttu-id="d25c4-196">Lägg till en referens till sammansättningen **System.Drawing.dll** i projektet och lägg även till följande associerade `using`-uttryck.</span><span class="sxs-lookup"><span data-stu-id="d25c4-196">Add a reference to the **System.Drawing.dll** assembly to the project, and also add the following associated `using` statements.</span></span>  
   
    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. <span data-ttu-id="d25c4-197">I klassen **ImageService** lägger du till följande konstruktor som läser in bitmappen och förbereder den för att skicka den till klientens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d25c4-197">In the **ImageService** class, add the following constructor that loads the bitmap and prepares to send it to the client browser.</span></span>
   
    ```csharp
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";
   
        Image bitmap;
   
        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```
7. <span data-ttu-id="d25c4-198">Direkt efter föregående kod lägger du till följande **GetImage**-metod i klassen **ImageService** för att returnera ett HTTP-meddelande som innehåller bilden.</span><span class="sxs-lookup"><span data-stu-id="d25c4-198">Directly after the previous code, add the following **GetImage** method in the **ImageService** class to return an HTTP message that contains the image.</span></span>
   
    ```csharp
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);
   
        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";
   
        return stream;
    }
    ```
   
    <span data-ttu-id="d25c4-199">Den här implementeringen använder **MemoryStream** för att hämta bilden och förbereda den för strömning till webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d25c4-199">This implementation uses **MemoryStream** to retrieve the image and prepare it for streaming to the browser.</span></span> <span data-ttu-id="d25c4-200">Den startar strömningspositionen vid noll, deklarerar ströminnehållet som jpeg och strömmar sedan informationen.</span><span class="sxs-lookup"><span data-stu-id="d25c4-200">It starts the stream position at zero, declares the stream content as a jpeg, and streams the information.</span></span>
8. <span data-ttu-id="d25c4-201">Klicka på **Skapa lösning** från **Skapa**-menyn.</span><span class="sxs-lookup"><span data-stu-id="d25c4-201">From the **Build** menu, click **Build Solution**.</span></span>

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a><span data-ttu-id="d25c4-202">Definiera konfigurationen för att köra webbtjänsten på Service Bus</span><span class="sxs-lookup"><span data-stu-id="d25c4-202">To define the configuration for running the web service on Service Bus</span></span>
1. <span data-ttu-id="d25c4-203">Dubbelklicka på filen **App.config** i **Solution Explorer** för att öppna den i Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="d25c4-203">In **Solution Explorer**, double-click **App.config** to open it in the Visual Studio editor.</span></span>
   
    <span data-ttu-id="d25c4-204">Den **App.config** filen innehåller tjänstnamn, slutpunkten (det vill säga den plats som Azure Relay visar för klienter och värdar att kommunicera med varandra) och bindningen (typ av protokoll som används för att kommunicera).</span><span class="sxs-lookup"><span data-stu-id="d25c4-204">The **App.config** file includes the service name, endpoint (that is, the location Azure Relay exposes for clients and hosts to communicate with each other), and binding (the type of protocol that is used to communicate).</span></span> <span data-ttu-id="d25c4-205">Den största skillnaden är att den konfigurerade tjänstslutpunkten refererar till en [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) bindning.</span><span class="sxs-lookup"><span data-stu-id="d25c4-205">The main difference here is that the configured service endpoint refers to a [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding.</span></span>
2. <span data-ttu-id="d25c4-206">XML-elementet `<system.serviceModel>` är ett WCF-element som definierar en eller flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="d25c4-206">The `<system.serviceModel>` XML element is a WCF element that defines one or more services.</span></span> <span data-ttu-id="d25c4-207">Här används det för att definiera tjänstenamnet och slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="d25c4-207">Here, it is used to define the service name and endpoint.</span></span> <span data-ttu-id="d25c4-208">Längst ned i elementet `<system.serviceModel>` (men fortfarande i `<system.serviceModel>`), lägger du till ett `<bindings>`-element som har följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="d25c4-208">At the bottom of the `<system.serviceModel>` element (but still within `<system.serviceModel>`), add a `<bindings>` element that has the following content.</span></span> <span data-ttu-id="d25c4-209">Detta definierar de bindningar som används i appen.</span><span class="sxs-lookup"><span data-stu-id="d25c4-209">This defines the bindings used in the application.</span></span> <span data-ttu-id="d25c4-210">Du kan definiera flera bindningar men i den här kursen definierar du bara en.</span><span class="sxs-lookup"><span data-stu-id="d25c4-210">You can define multiple bindings, but for this tutorial you are defining only one.</span></span>
   
    ```xml
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```
   
    <span data-ttu-id="d25c4-211">Föregående kod definierar en WCF-relä [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) bindning med **relayClientAuthenticationType** inställd på **ingen**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-211">The previous code defines a WCF Relay [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding with **relayClientAuthenticationType** set to **None**.</span></span> <span data-ttu-id="d25c4-212">Den här inställningen anger att en slutpunkt som använder den här bindningen inte kräver autentiseringsuppgifter för klienten.</span><span class="sxs-lookup"><span data-stu-id="d25c4-212">This setting indicates that an endpoint using this binding does not require a client credential.</span></span>
3. <span data-ttu-id="d25c4-213">Lägg till ett `<services>`-element efter elementet `<bindings>`.</span><span class="sxs-lookup"><span data-stu-id="d25c4-213">After the `<bindings>` element, add a `<services>` element.</span></span> <span data-ttu-id="d25c4-214">På ett liknande sätt som med bindningarna kan du definiera flera tjänster i en enda konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="d25c4-214">Similar to the bindings, you can define multiple services in a single configuration file.</span></span> <span data-ttu-id="d25c4-215">I den här självstudiekursen kommer du dock bara att definiera en.</span><span class="sxs-lookup"><span data-stu-id="d25c4-215">However, for this tutorial, you define only one.</span></span>
   
    ```xml
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```
   
    <span data-ttu-id="d25c4-216">I det här steget konfigurerar vi en tjänst som använder den förvalda **webHttpRelayBinding** som definierats tidigare.</span><span class="sxs-lookup"><span data-stu-id="d25c4-216">This step configures a service that uses the previously defined default **webHttpRelayBinding**.</span></span> <span data-ttu-id="d25c4-217">Den använder också den förvalda **sbTokenProvider**, som definieras i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="d25c4-217">It also uses the default **sbTokenProvider**, which is defined in the next step.</span></span>
4. <span data-ttu-id="d25c4-218">Efter den `<services>` element, skapa en `<behaviors>` element med följande innehåll, Ersätt ”SAS_KEY” med den *signatur för delad åtkomst* nyckeln (SAS) du tidigare fick från den [Azure-portalen][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="d25c4-218">After the `<services>` element, create a `<behaviors>` element with the following content, replacing "SAS_KEY" with the *Shared Access Signature* (SAS) key you previously obtained from the [Azure portal][Azure portal].</span></span>
   
    ```xml
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```
5. <span data-ttu-id="d25c4-219">Medan du fortfarande är kvar i App.config, i elementet `<appSettings>`, byter du ut hela värdet för anslutningssträngen mot den sträng som du tidigare fick från portalen.</span><span class="sxs-lookup"><span data-stu-id="d25c4-219">Still in App.config, in the `<appSettings>` element, replace the entire connection string value with the connection string you previously obtained from the portal.</span></span> 
   
    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```
6. <span data-ttu-id="d25c4-220">Klicka på **Skapa lösning** för att skapa hela lösningen från menyn **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-220">From the **Build** menu, click **Build Solution** to build the entire solution.</span></span>

### <a name="example"></a><span data-ttu-id="d25c4-221">Exempel</span><span class="sxs-lookup"><span data-stu-id="d25c4-221">Example</span></span>
<span data-ttu-id="d25c4-222">Följande kod visar kontrakt- och tjänsteimplementeringen för en REST-baserad tjänst som körs på Service Bus med hjälp av bindningen **WebHttpRelayBinding**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-222">The following code shows the contract and service implementation for a REST-based service that is running on  Service Bus using the **WebHttpRelayBinding** binding.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

<span data-ttu-id="d25c4-223">I följande exempel visas den App.config-fil som är associerad med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d25c4-223">The following example shows the App.config file associated with the service.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey="YOUR_SAS_KEY"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-azure-relay"></a><span data-ttu-id="d25c4-224">Steg 4: Värdbasera REST-baserat WCF-tjänst om du vill använda Azure Relay</span><span class="sxs-lookup"><span data-stu-id="d25c4-224">Step 4: Host the REST-based WCF service to use Azure Relay</span></span>
<span data-ttu-id="d25c4-225">Det här steget beskriver hur du kör en webbtjänst som använder ett konsolprogram med WCF Relay.</span><span class="sxs-lookup"><span data-stu-id="d25c4-225">This step describes how to run a web service using a console application with WCF Relay.</span></span> <span data-ttu-id="d25c4-226">En fullständig lista över den kod som skrivs i det här steget finns i det exempel som följer efter proceduren.</span><span class="sxs-lookup"><span data-stu-id="d25c4-226">A complete listing of the code written in this step is provided in the example following the procedure.</span></span>

### <a name="to-create-a-base-address-for-the-service"></a><span data-ttu-id="d25c4-227">Så här skapar du en basadress för tjänsten</span><span class="sxs-lookup"><span data-stu-id="d25c4-227">To create a base address for the service</span></span>
1. <span data-ttu-id="d25c4-228">I den `Main()` funktionsdeklarationen, skapa en variabel för att lagra namnområdet för ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="d25c4-228">In the `Main()` function declaration, create a variable to store the namespace of your project.</span></span> <span data-ttu-id="d25c4-229">Ersätt `yourNamespace` med namnet på namnområdet Relay som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="d25c4-229">Make sure to replace `yourNamespace` with the name of the Relay namespace you created previously.</span></span>
   
    ```csharp
    string serviceNamespace = "yourNamespace";
    ```
    <span data-ttu-id="d25c4-230">Service Bus använder namnet på ditt namnområde för att skapa ett unikt URI.</span><span class="sxs-lookup"><span data-stu-id="d25c4-230">Service Bus uses the name of your namespace to create a unique URI.</span></span>
2. <span data-ttu-id="d25c4-231">Skapa en `Uri`-instans för tjänstens basadress som baseras på namnområdet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-231">Create a `Uri` instance for the base address of the service that is based on the namespace.</span></span>
   
    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a><span data-ttu-id="d25c4-232">Så här skapar och konfigurerar du webbtjänstevärden</span><span class="sxs-lookup"><span data-stu-id="d25c4-232">To create and configure the web service host</span></span>
* <span data-ttu-id="d25c4-233">Skapa webbtjänstevärden med hjälp av den URI-adress som du skapade tidigare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-233">Create the web service host, using the URI address created earlier in this section.</span></span>
  
    ```csharp
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    <span data-ttu-id="d25c4-234">Tjänstevärden är det WCF-objekt som instantierar värdprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-234">The service host is the WCF object that instantiates the host application.</span></span> <span data-ttu-id="d25c4-235">Det här exemplet överför den typ av värd som du vill skapa (en **ImageService**), och även adressen som du vill exponera värdprogrammet på.</span><span class="sxs-lookup"><span data-stu-id="d25c4-235">This example passes it the type of host you want to create (an **ImageService**), and also the address at which you want to expose the host application.</span></span>

### <a name="to-run-the-web-service-host"></a><span data-ttu-id="d25c4-236">Så här kör du webbtjänstevärden</span><span class="sxs-lookup"><span data-stu-id="d25c4-236">To run the web service host</span></span>
1. <span data-ttu-id="d25c4-237">Öppna tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d25c4-237">Open the service.</span></span>
   
    ```csharp
    host.Open();
    ```
    <span data-ttu-id="d25c4-238">Tjänsten körs nu.</span><span class="sxs-lookup"><span data-stu-id="d25c4-238">The service is now running.</span></span>
2. <span data-ttu-id="d25c4-239">Visa ett meddelande som anger att tjänsten körs och hur du stoppar tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d25c4-239">Display a message indicating that the service is running, and how to stop the service.</span></span>
   
    ```csharp
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="d25c4-240">Stäng tjänstevärden när du är klar.</span><span class="sxs-lookup"><span data-stu-id="d25c4-240">When finished, close the service host.</span></span>
   
    ```csharp
    host.Close();
    ```

## <a name="example"></a><span data-ttu-id="d25c4-241">Exempel</span><span class="sxs-lookup"><span data-stu-id="d25c4-241">Example</span></span>
<span data-ttu-id="d25c4-242">Följande exempel innehåller tjänstekontraktet och implementeringen från föregående steg i självstudiekursen och fungerar som värd för tjänsten i ett konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="d25c4-242">The following example includes the service contract and implementation from previous steps in the tutorial and hosts the service in a console application.</span></span> <span data-ttu-id="d25c4-243">Sammanställ följande kod i en körbar fil med namnet ImageListener.exe.</span><span class="sxs-lookup"><span data-stu-id="d25c4-243">Compile the following code into an executable named ImageListener.exe.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a><span data-ttu-id="d25c4-244">Kompilera koden</span><span class="sxs-lookup"><span data-stu-id="d25c4-244">Compiling the code</span></span>
<span data-ttu-id="d25c4-245">Gör följande för att köra appen när du har skapat lösningen:</span><span class="sxs-lookup"><span data-stu-id="d25c4-245">After building the solution, do the following to run the application:</span></span>

1. <span data-ttu-id="d25c4-246">Tryck på **F5**, eller bläddra till den körbara filplatsen (ImageListener\bin\Debug\ImageListener.exe) för att köra tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d25c4-246">Press **F5**, or browse to the executable file location (ImageListener\bin\Debug\ImageListener.exe), to run the service.</span></span> <span data-ttu-id="d25c4-247">Fortsätt att köra appen eftersom det är nödvändigt för att kunna utföra nästa steg.</span><span class="sxs-lookup"><span data-stu-id="d25c4-247">Keep the app running, as it's required to perform the next step.</span></span>
2. <span data-ttu-id="d25c4-248">Kopiera och klistra in adressen från kommandotolken i en webbläsare för att se bilden.</span><span class="sxs-lookup"><span data-stu-id="d25c4-248">Copy and paste the address from the command prompt into a browser to see the image.</span></span>
3. <span data-ttu-id="d25c4-249">När du är klar trycker du på **Retur** i kommandotolken för att stänga appen.</span><span class="sxs-lookup"><span data-stu-id="d25c4-249">When you are finished, press **Enter** in the command prompt window to close the app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d25c4-250">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d25c4-250">Next steps</span></span>
<span data-ttu-id="d25c4-251">Nu när du har skapat ett program som använder tjänsten Service Bus relay finns i följande artiklar för att lära dig mer om Azure Relay:</span><span class="sxs-lookup"><span data-stu-id="d25c4-251">Now that you've built an application that uses the Service Bus relay service, see the following articles to learn more about Azure Relay:</span></span>

* [<span data-ttu-id="d25c4-252">Azure Service Bus-Arkitekturöversikt</span><span class="sxs-lookup"><span data-stu-id="d25c4-252">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [<span data-ttu-id="d25c4-253">Översikt över Azure Relay</span><span class="sxs-lookup"><span data-stu-id="d25c4-253">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="d25c4-254">Hur du använder tjänsten WCF relay med .NET</span><span class="sxs-lookup"><span data-stu-id="d25c4-254">How to use the WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure portal]: https://portal.azure.com
