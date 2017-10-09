---
title: "aaaService Bus REST-självstudiekurs med hjälp av Azure Relay | Microsoft Docs"
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
ms.openlocfilehash: b68650993a0390e7cef891ccb4236095cd86d4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-wcf-relay-rest-tutorial"></a><span data-ttu-id="bbd2e-103">Självstudiekurs för Azure WCF Relay REST</span><span class="sxs-lookup"><span data-stu-id="bbd2e-103">Azure WCF Relay REST tutorial</span></span>

<span data-ttu-id="bbd2e-104">Den här självstudiekursen beskrivs hur toobuild ett enkelt Azure-relä värd för program som visar ett REST-baserat gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-104">This tutorial describes how toobuild a simple Azure Relay host application that exposes a REST-based interface.</span></span> <span data-ttu-id="bbd2e-105">REST kan du ge en webbklient, till exempel en webbläsare, tooaccess hello Service Bus-API: er via HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-105">REST enables a web client, such as a web browser, tooaccess hello Service Bus APIs through HTTP requests.</span></span>

<span data-ttu-id="bbd2e-106">hello kursen använder hello Windows Communication Foundation (WCF) REST API modellen tooconstruct en REST-tjänst på Service Bus.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-106">hello tutorial uses hello Windows Communication Foundation (WCF) REST programming model tooconstruct a REST service on Service Bus.</span></span> <span data-ttu-id="bbd2e-107">Mer information finns i [programmeringsmodellen WCF REST](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) och [utforma och implementera tjänster](/dotnet/framework/wcf/designing-and-implementing-services) i hello WCF-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-107">For more information, see [WCF REST Programming Model](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) and [Designing and Implementing Services](/dotnet/framework/wcf/designing-and-implementing-services) in hello WCF documentation.</span></span>

## <a name="step-1-create-a-namespace"></a><span data-ttu-id="bbd2e-108">Steg 1: Skapa ett namnområde</span><span class="sxs-lookup"><span data-stu-id="bbd2e-108">Step 1: Create a namespace</span></span>

<span data-ttu-id="bbd2e-109">toobegin med Hej relay funktioner i Azure, måste du först skapa ett namnområde för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-109">toobegin using hello relay features in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="bbd2e-110">Ett namnområde tillhandahåller en omfångsbehållare för adressering av Azure-resurser i ditt program.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-110">A namespace provides a scoping container for addressing Azure resources within your application.</span></span> <span data-ttu-id="bbd2e-111">Följ hello [anvisningarna här](relay-create-namespace-portal.md) toocreate en Relay-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-111">Follow hello [instructions here](relay-create-namespace-portal.md) toocreate a Relay namespace.</span></span>

## <a name="step-2-define-a-rest-based-wcf-service-contract-toouse-with-azure-relay"></a><span data-ttu-id="bbd2e-112">Steg 2: Definiera en REST-baserat WCF-tjänsten kontraktet toouse med Azure-relä</span><span class="sxs-lookup"><span data-stu-id="bbd2e-112">Step 2: Define a REST-based WCF service contract toouse with Azure Relay</span></span>

<span data-ttu-id="bbd2e-113">När du skapar en tjänst för WCF REST-format, måste du definiera hello kontraktet.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-113">When you create a WCF REST-style service, you must define hello contract.</span></span> <span data-ttu-id="bbd2e-114">hello kontraktet anger vilka åtgärder hello värden stöder.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-114">hello contract specifies what operations hello host supports.</span></span> <span data-ttu-id="bbd2e-115">En tjänsteåtgärd kan anses vara en webbtjänstemetod.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-115">A service operation can be thought of as a web service method.</span></span> <span data-ttu-id="bbd2e-116">Kontrakt skapas genom att definiera ett gränssnitt för C++, C# eller Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-116">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="bbd2e-117">Varje metod i gränssnittet hello motsvarar tooa specifik tjänsteåtgärd.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-117">Each method in hello interface corresponds tooa specific service operation.</span></span> <span data-ttu-id="bbd2e-118">Hej [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribut måste vara tillämpade tooeach gränssnitt och hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attributet måste vara tillämpade tooeach igen.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-118">hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute must be applied tooeach interface, and hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute must be applied tooeach operation.</span></span> <span data-ttu-id="bbd2e-119">Om en metod i ett gränssnitt som har hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) saknar hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), inte exponeras metoden.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-119">If a method in an interface that has hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) does not have hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), that method is not exposed.</span></span> <span data-ttu-id="bbd2e-120">hello-kod som används för dessa aktiviteter visas i hello-exemplet som följer hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-120">hello code used for these tasks is shown in hello example following hello procedure.</span></span>

<span data-ttu-id="bbd2e-121">hello främsta skillnaden mellan en WCF-kontrakt och ett kontrakt i REST-format är hello lägga till en egenskap toohello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span><span class="sxs-lookup"><span data-stu-id="bbd2e-121">hello primary difference between a WCF contract and a REST-style contract is hello addition of a property toohello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span></span> <span data-ttu-id="bbd2e-122">Den här egenskapen gör toomap en metod i gränssnittet tooa metoden på hello andra sidan av hello-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-122">This property enables you toomap a method in your interface tooa method on hello other side of hello interface.</span></span> <span data-ttu-id="bbd2e-123">I det här fallet vi använder [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) toolink metod-tooHTTP GET.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-123">In this case, we will use [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) toolink a method tooHTTP GET.</span></span> <span data-ttu-id="bbd2e-124">Detta gör att Service Bus tooaccurately hämta och tolka kommandon som skickas toohello gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-124">This allows Service Bus tooaccurately retrieve and interpret commands sent toohello interface.</span></span>

### <a name="toocreate-a-contract-with-an-interface"></a><span data-ttu-id="bbd2e-125">toocreate ett kontrakt med ett gränssnitt</span><span class="sxs-lookup"><span data-stu-id="bbd2e-125">toocreate a contract with an interface</span></span>

1. <span data-ttu-id="bbd2e-126">Öppna Visual Studio som administratör: Högerklicka på hello programmet i hello **starta** -menyn och klicka sedan på **kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-126">Open Visual Studio as an administrator: right-click hello program in hello **Start** menu, and then click **Run as administrator**.</span></span>
2. <span data-ttu-id="bbd2e-127">Skapa ett nytt konsolappsrojekt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-127">Create a new console application project.</span></span> <span data-ttu-id="bbd2e-128">Klicka på hello **filen** -menyn och välj **ny**och välj **projekt**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-128">Click hello **File** menu and select **New**, then select **Project**.</span></span> <span data-ttu-id="bbd2e-129">I hello **nytt projekt** dialogrutan klickar du på **Visual C#**väljer hello **konsolprogram** mall, och ger den namnet **ImageListener**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-129">In hello **New Project** dialog box, click **Visual C#**, select hello **Console Application** template, and name it **ImageListener**.</span></span> <span data-ttu-id="bbd2e-130">Använd hello standard **plats**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-130">Use hello default **Location**.</span></span> <span data-ttu-id="bbd2e-131">Klicka på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-131">Click **OK** toocreate hello project.</span></span>
3. <span data-ttu-id="bbd2e-132">Visual Studio skapar en `Program.cs`-fil för ett C#-projekt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-132">For a C# project, Visual Studio creates a `Program.cs` file.</span></span> <span data-ttu-id="bbd2e-133">Den här klassen innehåller en tom `Main()` metod som krävs för en konsol programmet projektet toobuild korrekt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-133">This class contains an empty `Main()` method, required for a console application project toobuild correctly.</span></span>
4. <span data-ttu-id="bbd2e-134">Lägg till referenser tooService Bus och **System.ServiceModel.dll** toohello projektet genom att installera hello Service Bus NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-134">Add references tooService Bus and **System.ServiceModel.dll** toohello project by installing hello Service Bus NuGet package.</span></span> <span data-ttu-id="bbd2e-135">Det här paketet lägger automatiskt till referenser toohello Service Bus-bibliotek, samt hello WCF **System.ServiceModel**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-135">This package automatically adds references toohello Service Bus libraries, as well as hello WCF **System.ServiceModel**.</span></span> <span data-ttu-id="bbd2e-136">I Solution Explorer högerklickar du på hello **ImageListener** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-136">In Solution Explorer, right-click hello **ImageListener** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="bbd2e-137">Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-137">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="bbd2e-138">Klicka på **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-138">Click **Install**, and accept hello terms of use.</span></span>
5. <span data-ttu-id="bbd2e-139">Du måste uttryckligen lägga till en referens för**System.ServiceModel.Web.dll** toohello projektet:</span><span class="sxs-lookup"><span data-stu-id="bbd2e-139">You must explicitly add a reference too**System.ServiceModel.Web.dll** toohello project:</span></span>
   
    <span data-ttu-id="bbd2e-140">a.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-140">a.</span></span> <span data-ttu-id="bbd2e-141">I Solution Explorer högerklickar du på hello **referenser** mapp under hello projektmappen, och klicka sedan på **Lägg till referens**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-141">In Solution Explorer, right-click hello **References** folder under hello project folder and then click **Add Reference**.</span></span>
   
    <span data-ttu-id="bbd2e-142">b.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-142">b.</span></span> <span data-ttu-id="bbd2e-143">I hello **Lägg till referens** dialogrutan klickar du på hello **Framework** fliken hello vänster och hello **Sök** skriver **System.ServiceModel.Web** .</span><span class="sxs-lookup"><span data-stu-id="bbd2e-143">In hello **Add Reference** dialog box, click hello **Framework** tab on hello left-hand side and in hello **Search** box, type **System.ServiceModel.Web**.</span></span> <span data-ttu-id="bbd2e-144">Välj hello **System.ServiceModel.Web** kryssrutan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-144">Select hello **System.ServiceModel.Web** check box, then click **OK**.</span></span>
6. <span data-ttu-id="bbd2e-145">Lägg till följande hello `using` instruktioner överst hello i hello Program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-145">Add hello following `using` statements at hello top of hello Program.cs file.</span></span>
   
    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    <span data-ttu-id="bbd2e-146">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) är hello-namnområde som ger programmatisk åtkomst toobasic funktioner i WCF.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-146">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is hello namespace that enables programmatic access toobasic features of WCF.</span></span> <span data-ttu-id="bbd2e-147">Vidarebefordrande WCF använder många av hello objekt och attribut för WCF toodefine tjänstekontrakt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-147">WCF Relay uses many of hello objects and attributes of WCF toodefine service contracts.</span></span> <span data-ttu-id="bbd2e-148">Du använder det här namnområdet i de flesta relay-appar.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-148">You will use this namespace in most of your relay applications.</span></span> <span data-ttu-id="bbd2e-149">På liknande sätt [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) hjälper dig att definiera hello kanalen, vilket är hello-objekt som du kommunicera med Azure Relay och hello klientens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-149">Similarly, [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) helps define hello channel, which is hello object through which you communicate with Azure Relay and hello client web browser.</span></span> <span data-ttu-id="bbd2e-150">Slutligen [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) innehåller hello-typer som gör att du toocreate webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-150">Finally, [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) contains hello types that enable you toocreate web-based applications.</span></span>
7. <span data-ttu-id="bbd2e-151">Byt namn på hello `ImageListener` namnområde för**Microsoft.ServiceBus.Samples**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-151">Rename hello `ImageListener` namespace too**Microsoft.ServiceBus.Samples**.</span></span>
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. <span data-ttu-id="bbd2e-152">Direkt efter hello öppna klammerparentesen för namnområdesdeklarationen hello, definiera ett nytt gränssnitt med namnet **IImageContract** och tillämpa hello **ServiceContractAttribute** attributet toohello gränssnitt med en värdet för `http://samples.microsoft.com/ServiceModel/Relay/`.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-152">Directly after hello opening curly brace of hello namespace declaration, define a new interface named **IImageContract** and apply hello **ServiceContractAttribute** attribute toohello interface with a value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="bbd2e-153">Hej namnområdesvärdet skiljer sig från hello-namnområde som du använder under hela hello omfånget för din kod.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-153">hello namespace value differs from hello namespace that you use throughout hello scope of your code.</span></span> <span data-ttu-id="bbd2e-154">hello namnområdesvärdet används som en unik identifierare för det här kontraktet och bör innehålla versionsinformation.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-154">hello namespace value is used as a unique identifier for this contract, and should have version information.</span></span> <span data-ttu-id="bbd2e-155">Mer information finns i [Versionshantering för tjänster](http://go.microsoft.com/fwlink/?LinkID=180498).</span><span class="sxs-lookup"><span data-stu-id="bbd2e-155">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="bbd2e-156">Att ange hello namnområdet uttryckligen förhindrar att hello standardvärdet för namnområdet som läggs till toohello kontraktnamn.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-156">Specifying hello namespace explicitly prevents hello default namespace value from being added toohello contract name.</span></span>
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. <span data-ttu-id="bbd2e-157">Inom hello `IImageContract` gränssnitt, deklarera en metod för hello gång hello `IImageContract` kontrakt visar i hello gränssnitt och tillämpa hello `OperationContractAttribute` attributet toohello metod som du vill tooexpose som en del av hello offentliga Service Bus kontrakt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-157">Within hello `IImageContract` interface, declare a method for hello single operation hello `IImageContract` contract exposes in hello interface and apply hello `OperationContractAttribute` attribute toohello method that you want tooexpose as part of hello public Service Bus contract.</span></span>
   
    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. <span data-ttu-id="bbd2e-158">I hello **OperationContract** attribut, lägga till hello **WebGet** värde.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-158">In hello **OperationContract** attribute, add hello **WebGet** value.</span></span>
    
    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    <span data-ttu-id="bbd2e-159">Om du gör det möjliggör hello vidarebefordrande tjänsten tooroute HTTP GET-begäranden för`GetImage`, och tootranslate hello returnera värden av `GetImage` till en HTTP GETRESPONSE svaret.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-159">Doing so enables hello relay service tooroute HTTP GET requests too`GetImage`, and tootranslate hello return values of `GetImage` into an HTTP GETRESPONSE reply.</span></span> <span data-ttu-id="bbd2e-160">Senare i hello självstudiekursen ska du använda en web webbläsare tooaccess den här metoden och toodisplay hello avbildningen i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-160">Later in hello tutorial, you will use a web browser tooaccess this method, and toodisplay hello image in hello browser.</span></span>
11. <span data-ttu-id="bbd2e-161">Direkt efter hello `IImageContract` definition, deklarerar du en kanal som ärver från både hello `IImageContract` och `IClientChannel` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-161">Directly after hello `IImageContract` definition, declare a channel that inherits from both hello `IImageContract` and `IClientChannel` interfaces.</span></span>
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    <span data-ttu-id="bbd2e-162">En kanal är hello WCF-objekt via vilken hello-tjänsten och klienten skickar information tooeach andra.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-162">A channel is hello WCF object through which hello service and client pass information tooeach other.</span></span> <span data-ttu-id="bbd2e-163">Senare skapar du hello kanalen i din värdapp.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-163">Later, you will create hello channel in your host application.</span></span> <span data-ttu-id="bbd2e-164">Azure Relay använder sedan den här kanalen toopass hello HTTP GET-förfrågningar från hello webbläsare tooyour **GetImage** implementering.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-164">Azure Relay then uses this channel toopass hello HTTP GET requests from hello browser tooyour **GetImage** implementation.</span></span> <span data-ttu-id="bbd2e-165">hello relay använder också hello kanal tootake hello **GetImage** returvärde och omvandla det till en HTTP GETRESPONSE för hello klientens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-165">hello relay also uses hello channel tootake hello **GetImage** return value and translate it into an HTTP GETRESPONSE for hello client browser.</span></span>
12. <span data-ttu-id="bbd2e-166">Från hello **skapa** -menyn klickar du på **skapa lösning** tooconfirm hello noggrannhet arbete hittills.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-166">From hello **Build** menu, click **Build Solution** tooconfirm hello accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="bbd2e-167">Exempel</span><span class="sxs-lookup"><span data-stu-id="bbd2e-167">Example</span></span>
<span data-ttu-id="bbd2e-168">hello följande kod visar ett grundläggande gränssnitt som definierar ett vidarebefordrande WCF-kontrakt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-168">hello following code shows a basic interface that defines a WCF Relay contract.</span></span>

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

## <a name="step-3-implement-a-rest-based-wcf-service-contract-toouse-service-bus"></a><span data-ttu-id="bbd2e-169">Steg 3: Implementera ett kontrakt toouse för REST-baserat WCF-tjänsten Service Bus</span><span class="sxs-lookup"><span data-stu-id="bbd2e-169">Step 3: Implement a REST-based WCF service contract toouse Service Bus</span></span>
<span data-ttu-id="bbd2e-170">Skapa ett REST-format WCF vidarebefordrande tjänsten måste du först skapa hello kontrakt som definieras med hjälp av ett gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-170">Creating a REST-style WCF Relay service requires that you first create hello contract, which is defined by using an interface.</span></span> <span data-ttu-id="bbd2e-171">hello nästa steg är tooimplement hello gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-171">hello next step is tooimplement hello interface.</span></span> <span data-ttu-id="bbd2e-172">Detta innebär att du skapar en klass som heter **ImageService** som implementerar hello användardefinierade **IImageContract** gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-172">This involves creating a class named **ImageService** that implements hello user-defined **IImageContract** interface.</span></span> <span data-ttu-id="bbd2e-173">När du implementerar hello kontraktet, konfigurerar du sedan hello-gränssnittet genom att använda en App.config-fil.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-173">After you implement hello contract, you then configure hello interface using an App.config file.</span></span> <span data-ttu-id="bbd2e-174">hello konfigurationsfilen innehåller information som behövs för hello program, till exempel hello namnet på hello tjänst, hello namn hello kontraktet och hello typ av protokoll som används toocommunicate med hello vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-174">hello configuration file contains necessary information for hello application, such as hello name of hello service, hello name of hello contract, and hello type of protocol that is used toocommunicate with hello relay service.</span></span> <span data-ttu-id="bbd2e-175">hello-kod som används för dessa uppgifter finns i hello-exemplet som följer hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-175">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

<span data-ttu-id="bbd2e-176">Som med hello föregående steg, det är mycket lite skillnad mellan att implementera ett kontrakt i REST-format och ett vidarebefordrande WCF-kontrakt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-176">As with hello previous steps, there is very little difference between implementing a REST-style contract and a WCF Relay contract.</span></span>

### <a name="tooimplement-a-rest-style-service-bus-contract"></a><span data-ttu-id="bbd2e-177">Service Bus-kontrakt tooimplement REST-format</span><span class="sxs-lookup"><span data-stu-id="bbd2e-177">tooimplement a REST-style Service Bus contract</span></span>
1. <span data-ttu-id="bbd2e-178">Skapa en ny klass med namnet **ImageService** direkt efter hello definition av hello **IImageContract** gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-178">Create a new class named **ImageService** directly after hello definition of hello **IImageContract** interface.</span></span> <span data-ttu-id="bbd2e-179">Hej **ImageService** klassen implementerar hello **IImageContract** gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-179">hello **ImageService** class implements hello **IImageContract** interface.</span></span>
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    <span data-ttu-id="bbd2e-180">Liknande tooother gränssnittsimplementationer du kan implementera hello definition i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-180">Similar tooother interface implementations, you can implement hello definition in a different file.</span></span> <span data-ttu-id="bbd2e-181">Men för den här självstudiekursen hello visas implementeringen i samma fil som gränssnittsdefinitionen hello hello och `Main()` metod.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-181">However, for this tutorial, hello implementation appears in hello same file as hello interface definition and `Main()` method.</span></span>
2. <span data-ttu-id="bbd2e-182">Tillämpa hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attributet toohello **IImageService** klassen tooindicate som hello klassen är en implementering av ett WCF-kontrakt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-182">Apply hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute toohello **IImageService** class tooindicate that hello class is an implementation of a WCF contract.</span></span>
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    <span data-ttu-id="bbd2e-183">Som tidigare nämnts är det här namnområdet inte ett traditionellt namnområde.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-183">As mentioned previously, this namespace is not a traditional namespace.</span></span> <span data-ttu-id="bbd2e-184">I stället är den del av hello WCF-arkitekturen som identifierar hello kontraktet.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-184">Instead, it is part of hello WCF architecture that identifies hello contract.</span></span> <span data-ttu-id="bbd2e-185">Mer information finns i hello [datakontraktnamn](https://msdn.microsoft.com/library/ms731045.aspx) -avsnittet i hello WCF-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-185">For more information, see hello [Data Contract Names](https://msdn.microsoft.com/library/ms731045.aspx) topic in hello WCF documentation.</span></span>
3. <span data-ttu-id="bbd2e-186">Lägg till en .jpg-bild tooyour projektet.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-186">Add a .jpg image tooyour project.</span></span>  
   
    <span data-ttu-id="bbd2e-187">Det här är en bild som hello tjänsten visar i hello mottagning av webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-187">This is a picture that hello service displays in hello receiving browser.</span></span> <span data-ttu-id="bbd2e-188">Högerklicka på ditt projekt och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-188">Right-click your project, then click **Add**.</span></span> <span data-ttu-id="bbd2e-189">Klicka därefter på **Befintligt objekt**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-189">Then click **Existing Item**.</span></span> <span data-ttu-id="bbd2e-190">Använd hello **Lägg till befintligt objekt** dialogrutan rutan toobrowse tooan lämpliga .jpg och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-190">Use hello **Add Existing Item** dialog box toobrowse tooan appropriate .jpg, and then click **Add**.</span></span>
   
    <span data-ttu-id="bbd2e-191">När du lägger till hello-fil, se till att **alla filer** väljs i hello listrutan nästa toohello **filnamn:** fält.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-191">When adding hello file, make sure that **All Files** is selected in hello drop-down list next toohello **File name:** field.</span></span> <span data-ttu-id="bbd2e-192">hello resten av den här kursen förutsätter att hello namn på hello bild är ”image.jpg”.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-192">hello rest of this tutorial assumes that hello name of hello image is "image.jpg".</span></span> <span data-ttu-id="bbd2e-193">Om du har en annan fil, kommer du ha toorename hello bild eller ändra toocompensate din kod.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-193">If you have a different file, you will have toorename hello image, or change your code toocompensate.</span></span>
4. <span data-ttu-id="bbd2e-194">toomake till att hello kör tjänsten hittar hello bildfil i **Solution Explorer** Högerklicka hello image-filen och klicka på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-194">toomake sure that hello running service can find hello image file, in **Solution Explorer** right-click hello image file, then click **Properties**.</span></span> <span data-ttu-id="bbd2e-195">I hello **egenskaper** ställer du in **kopiera tooOutput Directory** för**kopiera om nyare**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-195">In hello **Properties** pane, set **Copy tooOutput Directory** too**Copy if newer**.</span></span>
5. <span data-ttu-id="bbd2e-196">Lägg till en referens toohello **System.Drawing.dll** sammansättningen toohello projekt och Lägg även till hello följande associerade `using` instruktioner.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-196">Add a reference toohello **System.Drawing.dll** assembly toohello project, and also add hello following associated `using` statements.</span></span>  
   
    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. <span data-ttu-id="bbd2e-197">I hello **ImageService** klassen, lägga till hello följande konstruktor att belastningar hello bitmappen och förbereder toosend den toohello klientens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-197">In hello **ImageService** class, add hello following constructor that loads hello bitmap and prepares toosend it toohello client browser.</span></span>
   
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
7. <span data-ttu-id="bbd2e-198">Direkt efter föregående kod hello Lägg till följande hello **GetImage** metod i hello **ImageService** klassen tooreturn ett HTTP-meddelande som innehåller hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-198">Directly after hello previous code, add hello following **GetImage** method in hello **ImageService** class tooreturn an HTTP message that contains hello image.</span></span>
   
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
   
    <span data-ttu-id="bbd2e-199">Den här implementeringen använder **MemoryStream** tooretrieve hello bilden och förbereda den för strömning toohello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-199">This implementation uses **MemoryStream** tooretrieve hello image and prepare it for streaming toohello browser.</span></span> <span data-ttu-id="bbd2e-200">Den börjar hello strömningspositionen vid noll, deklarerar hello ströminnehållet som jpeg och strömmar hello information.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-200">It starts hello stream position at zero, declares hello stream content as a jpeg, and streams hello information.</span></span>
8. <span data-ttu-id="bbd2e-201">Från hello **skapa** -menyn klickar du på **skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-201">From hello **Build** menu, click **Build Solution**.</span></span>

### <a name="toodefine-hello-configuration-for-running-hello-web-service-on-service-bus"></a><span data-ttu-id="bbd2e-202">toodefine hello konfiguration för att köra hello webbtjänsten på Service Bus</span><span class="sxs-lookup"><span data-stu-id="bbd2e-202">toodefine hello configuration for running hello web service on Service Bus</span></span>
1. <span data-ttu-id="bbd2e-203">I **Solution Explorer**, dubbelklicka på **App.config** tooopen i hello Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-203">In **Solution Explorer**, double-click **App.config** tooopen it in hello Visual Studio editor.</span></span>
   
    <span data-ttu-id="bbd2e-204">Hej **App.config** filen innehåller hello tjänstnamn, slutpunkten (det vill säga hello plats som Azure Relay visar för klienter och värdar toocommunicate med varandra) och bindningen (hello typ av protokoll som används toocommunicate).</span><span class="sxs-lookup"><span data-stu-id="bbd2e-204">hello **App.config** file includes hello service name, endpoint (that is, hello location Azure Relay exposes for clients and hosts toocommunicate with each other), and binding (hello type of protocol that is used toocommunicate).</span></span> <span data-ttu-id="bbd2e-205">hello största skillnaden här är den hello konfigurerade tjänstslutpunkten refererar tooa [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) bindning.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-205">hello main difference here is that hello configured service endpoint refers tooa [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding.</span></span>
2. <span data-ttu-id="bbd2e-206">Hej `<system.serviceModel>` XML-elementet är ett WCF-element som definierar en eller flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-206">hello `<system.serviceModel>` XML element is a WCF element that defines one or more services.</span></span> <span data-ttu-id="bbd2e-207">Det kan använda toodefine hello tjänstenamnet och slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-207">Here, it is used toodefine hello service name and endpoint.</span></span> <span data-ttu-id="bbd2e-208">Längst ned hello hello `<system.serviceModel>` element (men fortfarande inom `<system.serviceModel>`), Lägg till en `<bindings>` element som har hello följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-208">At hello bottom of hello `<system.serviceModel>` element (but still within `<system.serviceModel>`), add a `<bindings>` element that has hello following content.</span></span> <span data-ttu-id="bbd2e-209">Detta definierar hello bindningar som används i hello program.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-209">This defines hello bindings used in hello application.</span></span> <span data-ttu-id="bbd2e-210">Du kan definiera flera bindningar men i den här kursen definierar du bara en.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-210">You can define multiple bindings, but for this tutorial you are defining only one.</span></span>
   
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
   
    <span data-ttu-id="bbd2e-211">hello föregående kod definierar en WCF-relä [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) bindning med **relayClientAuthenticationType** ställa in också**ingen**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-211">hello previous code defines a WCF Relay [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding with **relayClientAuthenticationType** set too**None**.</span></span> <span data-ttu-id="bbd2e-212">Den här inställningen anger att en slutpunkt som använder den här bindningen inte kräver autentiseringsuppgifter för klienten.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-212">This setting indicates that an endpoint using this binding does not require a client credential.</span></span>
3. <span data-ttu-id="bbd2e-213">Efter hello `<bindings>` element, lägga till en `<services>` element.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-213">After hello `<bindings>` element, add a `<services>` element.</span></span> <span data-ttu-id="bbd2e-214">Liknande toohello bindningar kan du definiera flera tjänster i en enda konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-214">Similar toohello bindings, you can define multiple services in a single configuration file.</span></span> <span data-ttu-id="bbd2e-215">I den här självstudiekursen kommer du dock bara att definiera en.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-215">However, for this tutorial, you define only one.</span></span>
   
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
   
    <span data-ttu-id="bbd2e-216">Det här steget konfigurerar en tjänst som använder hello som tidigare definierats standard **webHttpRelayBinding**.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-216">This step configures a service that uses hello previously defined default **webHttpRelayBinding**.</span></span> <span data-ttu-id="bbd2e-217">Använder också hello standard **sbTokenProvider**, som har definierats i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-217">It also uses hello default **sbTokenProvider**, which is defined in hello next step.</span></span>
4. <span data-ttu-id="bbd2e-218">Efter hello `<services>` elementet, skapar en `<behaviors>` element med hello efter innehåll, Ersätt ”SAS_KEY” med hello *signatur för delad åtkomst* (SAS)-nyckel som du tidigare fick från hello [Azure-portalen ][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="bbd2e-218">After hello `<services>` element, create a `<behaviors>` element with hello following content, replacing "SAS_KEY" with hello *Shared Access Signature* (SAS) key you previously obtained from hello [Azure portal][Azure portal].</span></span>
   
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
5. <span data-ttu-id="bbd2e-219">Kvar i App.config i hello `<appSettings>` element, Ersätt hello hela anslutningen strängvärde med hello anslutningssträng som du tidigare fick från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-219">Still in App.config, in hello `<appSettings>` element, replace hello entire connection string value with hello connection string you previously obtained from hello portal.</span></span> 
   
    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```
6. <span data-ttu-id="bbd2e-220">Från hello **skapa** -menyn klickar du på **skapa lösning** toobuild hello hela lösningen.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-220">From hello **Build** menu, click **Build Solution** toobuild hello entire solution.</span></span>

### <a name="example"></a><span data-ttu-id="bbd2e-221">Exempel</span><span class="sxs-lookup"><span data-stu-id="bbd2e-221">Example</span></span>
<span data-ttu-id="bbd2e-222">hello följande kod visar hello kontrakt- och tjänsteimplementeringen för en REST-baserad tjänst som körs på Service Bus använder hello **WebHttpRelayBinding** bindning.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-222">hello following code shows hello contract and service implementation for a REST-based service that is running on  Service Bus using hello **WebHttpRelayBinding** binding.</span></span>

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

<span data-ttu-id="bbd2e-223">hello visar följande exempel hello App.config-fil som är associerad med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-223">hello following example shows hello App.config file associated with hello service.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove hello ones they don't need. -->
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

## <a name="step-4-host-hello-rest-based-wcf-service-toouse-azure-relay"></a><span data-ttu-id="bbd2e-224">Steg 4: Värdbasera hello REST-baserat WCF-tjänsten toouse Azure Relay</span><span class="sxs-lookup"><span data-stu-id="bbd2e-224">Step 4: Host hello REST-based WCF service toouse Azure Relay</span></span>
<span data-ttu-id="bbd2e-225">Det här steget beskriver hur toorun en tjänst med hjälp av ett konsolprogram med WCF Relay.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-225">This step describes how toorun a web service using a console application with WCF Relay.</span></span> <span data-ttu-id="bbd2e-226">En fullständig lista över hello-kod som skrivs i det här steget finns i hello-exemplet som följer hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-226">A complete listing of hello code written in this step is provided in hello example following hello procedure.</span></span>

### <a name="toocreate-a-base-address-for-hello-service"></a><span data-ttu-id="bbd2e-227">toocreate en basadress för hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="bbd2e-227">toocreate a base address for hello service</span></span>
1. <span data-ttu-id="bbd2e-228">I hello `Main()` funktionsdeklarationen, skapa en variabel toostore hello namnområdet för ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-228">In hello `Main()` function declaration, create a variable toostore hello namespace of your project.</span></span> <span data-ttu-id="bbd2e-229">Se till att tooreplace `yourNamespace` med hello namnet hello Relay-namnområde som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-229">Make sure tooreplace `yourNamespace` with hello name of hello Relay namespace you created previously.</span></span>
   
    ```csharp
    string serviceNamespace = "yourNamespace";
    ```
    <span data-ttu-id="bbd2e-230">Service Bus använder hello namnet på ditt namnområde toocreate ett unikt URI.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-230">Service Bus uses hello name of your namespace toocreate a unique URI.</span></span>
2. <span data-ttu-id="bbd2e-231">Skapa en `Uri` hello service som baseras på namnområdet hello-instans för hello basadress.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-231">Create a `Uri` instance for hello base address of hello service that is based on hello namespace.</span></span>
   
    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="toocreate-and-configure-hello-web-service-host"></a><span data-ttu-id="bbd2e-232">toocreate och konfigurera hello webbtjänstevärden</span><span class="sxs-lookup"><span data-stu-id="bbd2e-232">toocreate and configure hello web service host</span></span>
* <span data-ttu-id="bbd2e-233">Skapa hello webbtjänstevärden med hjälp av hello URI-adress som skapade tidigare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-233">Create hello web service host, using hello URI address created earlier in this section.</span></span>
  
    ```csharp
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    <span data-ttu-id="bbd2e-234">hello tjänstvärden är hello WCF-objekt som instantierar värdprogrammet hello.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-234">hello service host is hello WCF object that instantiates hello host application.</span></span> <span data-ttu-id="bbd2e-235">Det här exemplet skickar den hello typen värden som du vill toocreate (en **ImageService**), och även hello adress som du vill tooexpose hello värdprogrammet.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-235">This example passes it hello type of host you want toocreate (an **ImageService**), and also hello address at which you want tooexpose hello host application.</span></span>

### <a name="toorun-hello-web-service-host"></a><span data-ttu-id="bbd2e-236">toorun hello webbtjänstevärden</span><span class="sxs-lookup"><span data-stu-id="bbd2e-236">toorun hello web service host</span></span>
1. <span data-ttu-id="bbd2e-237">Öppna hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-237">Open hello service.</span></span>
   
    ```csharp
    host.Open();
    ```
    <span data-ttu-id="bbd2e-238">hello-tjänsten körs nu.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-238">hello service is now running.</span></span>
2. <span data-ttu-id="bbd2e-239">Visa ett meddelande som anger att hello-tjänsten körs och hur toostop hello service.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-239">Display a message indicating that hello service is running, and how toostop hello service.</span></span>
   
    ```csharp
    Console.WriteLine("Copy hello following address into a browser toosee hello image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="bbd2e-240">När du är klar stänger du hello tjänstvärden.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-240">When finished, close hello service host.</span></span>
   
    ```csharp
    host.Close();
    ```

## <a name="example"></a><span data-ttu-id="bbd2e-241">Exempel</span><span class="sxs-lookup"><span data-stu-id="bbd2e-241">Example</span></span>
<span data-ttu-id="bbd2e-242">hello följande exempel innehåller hello tjänstekontraktet och implementeringen från föregående steg i hello självstudier och värdar hello tjänsten i ett konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-242">hello following example includes hello service contract and implementation from previous steps in hello tutorial and hosts hello service in a console application.</span></span> <span data-ttu-id="bbd2e-243">Kompilera hello följande kod i en körbar fil med namnet ImageListener.exe.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-243">Compile hello following code into an executable named ImageListener.exe.</span></span>

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

            Console.WriteLine("Copy hello following address into a browser toosee hello image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] tooexit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-hello-code"></a><span data-ttu-id="bbd2e-244">Kompilera koden hello</span><span class="sxs-lookup"><span data-stu-id="bbd2e-244">Compiling hello code</span></span>
<span data-ttu-id="bbd2e-245">När du har skapat hello lösningen hello följande toorun hello program:</span><span class="sxs-lookup"><span data-stu-id="bbd2e-245">After building hello solution, do hello following toorun hello application:</span></span>

1. <span data-ttu-id="bbd2e-246">Tryck på **F5**, eller bläddra toohello körbara filplatsen (ImageListener\bin\Debug\ImageListener.exe) toorun hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-246">Press **F5**, or browse toohello executable file location (ImageListener\bin\Debug\ImageListener.exe), toorun hello service.</span></span> <span data-ttu-id="bbd2e-247">Behåll hello app körs, eftersom det är nödvändigt tooperform hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-247">Keep hello app running, as it's required tooperform hello next step.</span></span>
2. <span data-ttu-id="bbd2e-248">Kopiera och klistra in hello-adress från hello Kommandotolken i en webbläsare toosee hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-248">Copy and paste hello address from hello command prompt into a browser toosee hello image.</span></span>
3. <span data-ttu-id="bbd2e-249">När du är klar trycker du på **RETUR** i hello kommandotolk-fönster tooclose hello app.</span><span class="sxs-lookup"><span data-stu-id="bbd2e-249">When you are finished, press **Enter** in hello command prompt window tooclose hello app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bbd2e-250">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bbd2e-250">Next steps</span></span>
<span data-ttu-id="bbd2e-251">Nu när du har skapat ett program som använder hello Service Bus relay-tjänst finns i följande artiklar toolearn mer om Azure Relay hello:</span><span class="sxs-lookup"><span data-stu-id="bbd2e-251">Now that you've built an application that uses hello Service Bus relay service, see hello following articles toolearn more about Azure Relay:</span></span>

* [<span data-ttu-id="bbd2e-252">Azure Service Bus-Arkitekturöversikt</span><span class="sxs-lookup"><span data-stu-id="bbd2e-252">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [<span data-ttu-id="bbd2e-253">Översikt över Azure Relay</span><span class="sxs-lookup"><span data-stu-id="bbd2e-253">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="bbd2e-254">Hur toouse hello WCF vidarebefordrande tjänsten med .NET</span><span class="sxs-lookup"><span data-stu-id="bbd2e-254">How toouse hello WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure portal]: https://portal.azure.com
