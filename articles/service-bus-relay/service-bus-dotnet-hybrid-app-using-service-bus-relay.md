---
title: "aaaAzure vidarebefordrande WCF hybridprogram på lokalt/i molnet (.NET) | Microsoft Docs"
description: "Lär dig hur toocreate en .NET på lokalt/i molnet hybridprogram med hjälp av Azure WCF Relay."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9ed02f7c-ebfb-4f39-9c97-b7dc15bcb4c1
ms.service: service-bus-relay
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: sethm
ms.openlocfilehash: aab8b1dbdc85c4edf7b0ccef0921b69524b2d306
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="net-on-premisescloud-hybrid-application-using-azure-wcf-relay"></a><span data-ttu-id="dcf7e-103">Lokalt eller molnbaserat .NET-hybridprogram med hjälp av Azure WCF Relay</span><span class="sxs-lookup"><span data-stu-id="dcf7e-103">.NET on-premises/cloud hybrid application using Azure WCF Relay</span></span>
## <a name="introduction"></a><span data-ttu-id="dcf7e-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="dcf7e-104">Introduction</span></span>

<span data-ttu-id="dcf7e-105">Den här artikeln visar hur toobuild en hybrid cloud-program med Microsoft Azure och Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-105">This article shows how toobuild a hybrid cloud application with Microsoft Azure and Visual Studio.</span></span> <span data-ttu-id="dcf7e-106">hello kursen förutsätter att du har några tidigare erfarenheter av att använda Azure.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-106">hello tutorial assumes you have no prior experience using Azure.</span></span> <span data-ttu-id="dcf7e-107">På mindre än 30 minuter har du ett program som använder flera Azure-resurser och körs i hello moln.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-107">In less than 30 minutes, you will have an application that uses multiple Azure resources up and running in hello cloud.</span></span>

<span data-ttu-id="dcf7e-108">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="dcf7e-108">You will learn:</span></span>

* <span data-ttu-id="dcf7e-109">Hur toocreate eller anpassa en befintlig webbtjänst för användning av en webblösning.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-109">How toocreate or adapt an existing web service for consumption by a web solution.</span></span>
* <span data-ttu-id="dcf7e-110">Hur toouse hello Azure WCF Relay service tooshare data mellan ett Azure-program och en webbtjänst värdbaserad någon annanstans.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-110">How toouse hello Azure WCF Relay service tooshare data between an Azure application and a web service hosted elsewhere.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a><span data-ttu-id="dcf7e-111">Så här hjälper Azure Relay dig med hybridlösningar</span><span class="sxs-lookup"><span data-stu-id="dcf7e-111">How Azure Relay helps with hybrid solutions</span></span>

<span data-ttu-id="dcf7e-112">Företagslösningar består normalt av en kombination av anpassad kod som skrivs tootackle nya och unika affärsbehov och befintliga funktioner som tillhandahålls av lösningar och system som redan finns på plats.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-112">Business solutions are typically composed of a combination of custom code written tootackle new and unique business requirements and existing functionality provided by solutions and systems that are already in place.</span></span>

<span data-ttu-id="dcf7e-113">Lösningsarkitekter har börjat toouse hello molnet för enklare hantering av skalkrav och lägre driftskostnader.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-113">Solution architects are starting toouse hello cloud for easier handling of scale requirements and lower operational costs.</span></span> <span data-ttu-id="dcf7e-114">På så sätt kan hitta de befintliga tjänstetillgångarna som de vill ha tooleverage som byggblock för sina lösningar är innanför företagets brandvägg för hello och utanför enkelt nå för av hello molnlösning.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-114">In doing so, they find that existing service assets they'd like tooleverage as building blocks for their solutions are inside hello corporate firewall and out of easy reach for access by hello cloud solution.</span></span> <span data-ttu-id="dcf7e-115">Många interna tjänster är inte inbyggda eller värdbaserade på ett sätt att de enkelt kan exponeras vid företagets nätverksgräns för hello.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-115">Many internal services are not built or hosted in a way that they can be easily exposed at hello corporate network edge.</span></span>

<span data-ttu-id="dcf7e-116">[Azure Relay](https://azure.microsoft.com/services/service-bus/) är avsedd för hello användningsfall där man tar befintliga Windows Communication Foundation (WCF) webbtjänster och göra de tjänster på ett säkert sätt komma åt toosolutions som finns utanför företagets perimeter för hello utan störande ändringar toohello företagets nätverksinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-116">[Azure Relay](https://azure.microsoft.com/services/service-bus/) is designed for hello use-case of taking existing Windows Communication Foundation (WCF) web services and making those services securely accessible toosolutions that reside outside hello corporate perimeter without requiring intrusive changes toohello corporate network infrastructure.</span></span> <span data-ttu-id="dcf7e-117">Sådana relay-tjänster är fortfarande inhysta i sin befintliga miljö men de delegerar lyssnandet efter inkommande sessioner och förfrågningar toohello molnbaserade vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-117">Such relay services are still hosted inside their existing environment, but they delegate listening for incoming sessions and requests toohello cloud-hosted relay service.</span></span> <span data-ttu-id="dcf7e-118">Azure Relay skyddar även tjänsterna mot obehörig åtkomst med hjälp av [signatur för delad åtkomst (SAS)](../service-bus-messaging/service-bus-sas.md)-autentisering.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-118">Azure Relay also protects those services from unauthorized access by using [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) authentication.</span></span>

## <a name="solution-scenario"></a><span data-ttu-id="dcf7e-119">Lösningsscenario</span><span class="sxs-lookup"><span data-stu-id="dcf7e-119">Solution scenario</span></span>
<span data-ttu-id="dcf7e-120">I den här självstudiekursen skapar du en ASP.NET-webbplats som gör att toosee en lista över produkter på hello produktsidan för inventering.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-120">In this tutorial, you will create an ASP.NET website that enables you toosee a list of products on hello product inventory page.</span></span>

![][0]

<span data-ttu-id="dcf7e-121">hello kursen förutsätter att du har produktinformation i ett befintligt lokalt system och använder Azure Relay tooreach till detta system.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-121">hello tutorial assumes that you have product information in an existing on-premises system, and uses Azure Relay tooreach into that system.</span></span> <span data-ttu-id="dcf7e-122">Det här simuleras av en webbtjänst som körs i ett enkelt konsolprogram och backas upp av en minnesintern uppsättning av produkter.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-122">This is simulated by a web service that runs in a simple console application and is backed by an in-memory set of products.</span></span> <span data-ttu-id="dcf7e-123">Du kommer att kunna toorun detta konsolprogram på din dator och distribuera hello webbrollen till Azure.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-123">You will be able toorun this console application on your own computer and deploy hello web role into Azure.</span></span> <span data-ttu-id="dcf7e-124">På så sätt, ser du hur hello-webbroll som körs i hello Azure-datacenter verkligen anropar din dator, även om datorn sannolikt ligger bakom en brandvägg och ett network address translation (NAT) lager.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-124">By doing so, you will see how hello web role running in hello Azure datacenter will indeed call into your computer, even though your computer will almost certainly reside behind at least one firewall and a network address translation (NAT) layer.</span></span>

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="dcf7e-125">Ställa in hello utvecklingsmiljö</span><span class="sxs-lookup"><span data-stu-id="dcf7e-125">Set up hello development environment</span></span>

<span data-ttu-id="dcf7e-126">Innan du kan börja utveckla Azure-program, hämta hello verktyg och ställa in din utvecklingsmiljö:</span><span class="sxs-lookup"><span data-stu-id="dcf7e-126">Before you can begin developing Azure applications, download hello tools and set up your development environment:</span></span>

1. <span data-ttu-id="dcf7e-127">Installera hello Azure SDK för .NET från hello SDK [Nedladdningssida](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="dcf7e-127">Install hello Azure SDK for .NET from hello SDK [downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="dcf7e-128">I hello **.NET** kolumn, och klicka på hello version av [Visual Studio](http://www.visualstudio.com) du använder.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-128">In hello **.NET** column, click hello version of [Visual Studio](http://www.visualstudio.com) you are using.</span></span> <span data-ttu-id="dcf7e-129">hello stegen i den här självstudiekursen används Visual Studio 2015, men de fungerar även med Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-129">hello steps in this tutorial use Visual Studio 2015, but they also work with Visual Studio 2017.</span></span>
3. <span data-ttu-id="dcf7e-130">När du uppmanas toorun eller spara hello installer, klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-130">When prompted toorun or save hello installer, click **Run**.</span></span>
4. <span data-ttu-id="dcf7e-131">I hello **installationsprogram för webbplattform**, klickar du på **installera** och fortsätta med installationen hello.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-131">In hello **Web Platform Installer**, click **Install** and proceed with hello installation.</span></span>
5. <span data-ttu-id="dcf7e-132">När hello installationen är klar har du allt nödvändigt toostart toodevelop hello app.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-132">Once hello installation is complete, you will have everything necessary toostart toodevelop hello app.</span></span> <span data-ttu-id="dcf7e-133">hello SDK inkluderar verktyg som låter dig utveckla Azure-program i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-133">hello SDK includes tools that let you easily develop Azure applications in Visual Studio.</span></span>

## <a name="create-a-namespace"></a><span data-ttu-id="dcf7e-134">Skapa ett namnområde</span><span class="sxs-lookup"><span data-stu-id="dcf7e-134">Create a namespace</span></span>

<span data-ttu-id="dcf7e-135">toobegin med Hej relay funktioner i Azure, måste du först skapa ett namnområde för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-135">toobegin using hello relay features in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="dcf7e-136">Ett namnområde tillhandahåller en omfångsbehållare för adressering av Azure-resurser i ditt program.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-136">A namespace provides a scoping container for addressing Azure resources within your application.</span></span> <span data-ttu-id="dcf7e-137">Följ hello [anvisningarna här](relay-create-namespace-portal.md) toocreate en Relay-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-137">Follow hello [instructions here](relay-create-namespace-portal.md) toocreate a Relay namespace.</span></span>

## <a name="create-an-on-premises-server"></a><span data-ttu-id="dcf7e-138">Skapa en lokal server</span><span class="sxs-lookup"><span data-stu-id="dcf7e-138">Create an on-premises server</span></span>

<span data-ttu-id="dcf7e-139">Först ska du skapa ett lokalt produktkatalogsystem (ett fingerat sådant).</span><span class="sxs-lookup"><span data-stu-id="dcf7e-139">First, you will build a (mock) on-premises product catalog system.</span></span> <span data-ttu-id="dcf7e-140">Det kan vara ganska enkel; Du kan se detta som representerar ett faktiskt, lokalt produktkatalogsystem med en fullständig serviceyta som vi försöker toointegrate.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-140">It will be fairly simple; you can see this as representing an actual on-premises product catalog system with a complete service surface that we're trying toointegrate.</span></span>

<span data-ttu-id="dcf7e-141">Det här projektet är ett konsolprogram i Visual Studio och använder hello [Azure Service Bus NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) tooinclude hello Service Bus-bibliotek och konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-141">This project is a Visual Studio console application, and uses hello [Azure Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) tooinclude hello Service Bus libraries and configuration settings.</span></span>

### <a name="create-hello-project"></a><span data-ttu-id="dcf7e-142">Skapa hello-projekt</span><span class="sxs-lookup"><span data-stu-id="dcf7e-142">Create hello project</span></span>

1. <span data-ttu-id="dcf7e-143">Starta Microsoft Visual Studio med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-143">Using administrator privileges, start Microsoft Visual Studio.</span></span> <span data-ttu-id="dcf7e-144">toodo så, högerklicka på programikonen för hello Visual Studio och klicka sedan på **kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-144">toodo so, right-click hello Visual Studio program icon, and then click **Run as administrator**.</span></span>
2. <span data-ttu-id="dcf7e-145">I Visual Studio på hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-145">In Visual Studio, on hello **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="dcf7e-146">Klicka på **Konsolprogram (.NET Framework** från **Installerade mallar** under **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-146">From **Installed Templates**, under **Visual C#**, click **Console App (.NET Framework)**.</span></span> <span data-ttu-id="dcf7e-147">I hello **namn** rutan, hello typnamn **ProductsServer**:</span><span class="sxs-lookup"><span data-stu-id="dcf7e-147">In hello **Name** box, type hello name **ProductsServer**:</span></span>

   ![][11]
4. <span data-ttu-id="dcf7e-148">Klicka på **OK** toocreate hello **ProductsServer** projekt.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-148">Click **OK** toocreate hello **ProductsServer** project.</span></span>
5. <span data-ttu-id="dcf7e-149">Hoppa över toohello nästa steg om du redan har installerat hello NuGet package manager för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-149">If you have already installed hello NuGet package manager for Visual Studio, skip toohello next step.</span></span> <span data-ttu-id="dcf7e-150">Annars går du till [NuGet][NuGet] och klickar på [Installera NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c).</span><span class="sxs-lookup"><span data-stu-id="dcf7e-150">Otherwise, visit [NuGet][NuGet] and click [Install NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c).</span></span> <span data-ttu-id="dcf7e-151">Följ hello prompter tooinstall hello NuGet-Pakethanteraren, och starta sedan om Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-151">Follow hello prompts tooinstall hello NuGet package manager, then re-start Visual Studio.</span></span>
6. <span data-ttu-id="dcf7e-152">I Solution Explorer högerklickar du på hello **ProductsServer** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-152">In Solution Explorer, right-click hello **ProductsServer** project, then click **Manage NuGet Packages**.</span></span>
7. <span data-ttu-id="dcf7e-153">Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-153">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="dcf7e-154">Välj hello **WindowsAzure.ServiceBus** paketet.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-154">Select hello **WindowsAzure.ServiceBus** package.</span></span>
8. <span data-ttu-id="dcf7e-155">Klicka på **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-155">Click **Install**, and accept hello terms of use.</span></span>

   ![][13]

   <span data-ttu-id="dcf7e-156">Observera att hello krävs klientsammansättningarna nu refereras.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-156">Note that hello required client assemblies are now referenced.</span></span>
8. <span data-ttu-id="dcf7e-157">Lägg till en ny klass för ditt produktkontrakt.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-157">Add a new class for your product contract.</span></span> <span data-ttu-id="dcf7e-158">I Solution Explorer högerklickar du på hello **ProductsServer** projektet och klicka på **Lägg till**, och klicka sedan på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-158">In Solution Explorer, right-click hello **ProductsServer** project and click **Add**, and then click **Class**.</span></span>
9. <span data-ttu-id="dcf7e-159">I hello **namn** rutan, hello typnamn **ProductsContract.cs**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-159">In hello **Name** box, type hello name **ProductsContract.cs**.</span></span> <span data-ttu-id="dcf7e-160">Klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-160">Then click **Add**.</span></span>
10. <span data-ttu-id="dcf7e-161">I **ProductsContract.cs**, Ersätt hello definitionen för namnområdet med följande kod, som definierar hello kontraktet för tjänsten hello hello.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-161">In **ProductsContract.cs**, replace hello namespace definition with hello following code, which defines hello contract for hello service.</span></span>

    ```csharp
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;

        // Define hello data contract for hello service
        [DataContract]
        // Declare hello serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }

        // Define hello service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();

        }

        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```
11. <span data-ttu-id="dcf7e-162">Ersätt hello definitionen för namnområdet i Program.cs med hello följande kod, som lägger till hello profiltjänsten och hello värden för den.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-162">In Program.cs, replace hello namespace definition with hello following code, which adds hello profile service and hello host for it.</span></span>

    ```csharp
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;

        // Implement hello IProducts interface.
        class ProductsService : IProducts
        {

            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };

            // Display a message in hello service console application
            // when hello list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }

        }

        class Program
        {
            // Define hello Main() function in hello service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();

                Console.WriteLine("Press ENTER tooclose");
                Console.ReadLine();

                sh.Close();
            }
        }
    }
    ```
12. <span data-ttu-id="dcf7e-163">I Solution Explorer dubbelklickar du på hello **App.config** filen tooopen i hello Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-163">In Solution Explorer, double-click hello **App.config** file tooopen it in hello Visual Studio editor.</span></span> <span data-ttu-id="dcf7e-164">Längst ned hello hello `<system.ServiceModel>` element (men fortfarande inom `<system.ServiceModel>`), Lägg till hello följande XML-koden.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-164">At hello bottom of hello `<system.ServiceModel>` element (but still within `<system.ServiceModel>`), add hello following XML code.</span></span> <span data-ttu-id="dcf7e-165">Vara säker på att tooreplace *yourServiceNamespace* med hello namnet på ditt namnområdet och *yourKey* med hello SAS-nyckel som du tidigare hämtade från hello portal:</span><span class="sxs-lookup"><span data-stu-id="dcf7e-165">Be sure tooreplace *yourServiceNamespace* with hello name of your namespace, and *yourKey* with hello SAS key you retrieved earlier from hello portal:</span></span>

    ```xml
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
13. <span data-ttu-id="dcf7e-166">Kvar i App.config i hello `<appSettings>` element, Ersätt hello anslutning strängvärde med hello anslutningssträng som du tidigare fick från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-166">Still in App.config, in hello `<appSettings>` element, replace hello connection string value with hello connection string you previously obtained from hello portal.</span></span>

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```
14. <span data-ttu-id="dcf7e-167">Tryck på **Ctrl + Skift + B** eller från hello **skapa** -menyn klickar du på **skapa lösning** toobuild hello program och kontrollera hello noggrannhet arbete hittills.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-167">Press **Ctrl+Shift+B** or from hello **Build** menu, click **Build Solution** toobuild hello application and verify hello accuracy of your work so far.</span></span>

## <a name="create-an-aspnet-application"></a><span data-ttu-id="dcf7e-168">Skapa ett ASP.NET-program</span><span class="sxs-lookup"><span data-stu-id="dcf7e-168">Create an ASP.NET application</span></span>

<span data-ttu-id="dcf7e-169">I det här avsnittet skapar du ett enkelt ASP.NET-program som visar data som hämtats från din produkttjänst.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-169">In this section you will build a simple ASP.NET application that displays data retrieved from your product service.</span></span>

### <a name="create-hello-project"></a><span data-ttu-id="dcf7e-170">Skapa hello-projekt</span><span class="sxs-lookup"><span data-stu-id="dcf7e-170">Create hello project</span></span>

1. <span data-ttu-id="dcf7e-171">Se till att Visual Studio körs med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-171">Ensure that Visual Studio is running with administrator privileges.</span></span>
2. <span data-ttu-id="dcf7e-172">I Visual Studio på hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-172">In Visual Studio, on hello **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="dcf7e-173">Klicka på **ASP.NET webbapp (.NET Framwork)** från **Installerade mallar** under **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-173">From **Installed Templates**, under **Visual C#**, click **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="dcf7e-174">Namnet hello projektet **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-174">Name hello project **ProductsPortal**.</span></span> <span data-ttu-id="dcf7e-175">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-175">Then click **OK**.</span></span>

   ![][15]

4. <span data-ttu-id="dcf7e-176">Från hello **ASP.NET mallar** listan i hello **nytt ASP.NET-webbprogram** dialogrutan klickar du på **MVC**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-176">From hello **ASP.NET Templates** list in hello **New ASP.NET Web Application** dialog, click **MVC**.</span></span>

   ![][16]

6. <span data-ttu-id="dcf7e-177">Klicka på hello **ändra autentisering** knappen.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-177">Click hello **Change Authentication** button.</span></span> <span data-ttu-id="dcf7e-178">I hello **ändra autentisering** dialogrutan kontrollerar du att **ingen autentisering** är markerad och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-178">In hello **Change Authentication** dialog box, ensure that **No Authentication** is selected, and then click **OK**.</span></span> <span data-ttu-id="dcf7e-179">För den här självstudiekursen distribuerar du en app som inte kräver någon användarinloggning.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-179">For this tutorial, you're deploying an app that does not need a user login.</span></span>

    ![][18]

7. <span data-ttu-id="dcf7e-180">Tillbaka i hello **nytt ASP.NET-webbprogram** dialogrutan klickar du på **OK** toocreate hello MVC-app.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-180">Back in hello **New ASP.NET Web Application** dialog, click **OK** toocreate hello MVC app.</span></span>
8. <span data-ttu-id="dcf7e-181">Nu måste du konfigurera Azure-resurserna för en ny webbapp.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-181">Now you must configure Azure resources for a new web app.</span></span> <span data-ttu-id="dcf7e-182">Åtgärderna i hello hello [publicera tooAzure avsnitt i den här artikeln](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="dcf7e-182">Follow hello steps in hello [Publish tooAzure section of this article](../app-service-web/app-service-web-get-started-dotnet.md).</span></span> <span data-ttu-id="dcf7e-183">Sedan returnera toothis självstudier och fortsätta toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-183">Then, return toothis tutorial and proceed toohello next step.</span></span>
10. <span data-ttu-id="dcf7e-184">I Solution Explorer högerklickar du på **Modeller**. Klicka sedan på **Lägg till** och på **Klass**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-184">In Solution Explorer, right-click **Models** and then click **Add**, then click **Class**.</span></span> <span data-ttu-id="dcf7e-185">I hello **namn** rutan, hello typnamn **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-185">In hello **Name** box, type hello name **Product.cs**.</span></span> <span data-ttu-id="dcf7e-186">Klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-186">Then click **Add**.</span></span>

    ![][17]

### <a name="modify-hello-web-application"></a><span data-ttu-id="dcf7e-187">Ändra hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="dcf7e-187">Modify hello web application</span></span>

1. <span data-ttu-id="dcf7e-188">Ersätt hello befintliga definitionen för namnområdet med följande kod hello i hello Product.CS i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-188">In hello Product.cs file in Visual Studio, replace hello existing namespace definition with hello following code.</span></span>

   ```csharp
    // Declare properties for hello products inventory.
    namespace ProductsWeb.Models
    {
       public class Product
       {
           public string Id { get; set; }
           public string Name { get; set; }
           public string Quantity { get; set; }
       }
    }
    ```
2. <span data-ttu-id="dcf7e-189">I Solution Explorer expanderar du hello **domänkontrollanter** mapp, dubbelklicka på hello **HomeController.cs** filen tooopen den i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-189">In Solution Explorer, expand hello **Controllers** folder, then double-click hello **HomeController.cs** file tooopen it in Visual Studio.</span></span>
3. <span data-ttu-id="dcf7e-190">I **HomeController.cs**, Ersätt hello befintliga definitionen för namnområdet med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-190">In **HomeController.cs**, replace hello existing namespace definition with hello following code.</span></span>

    ```csharp
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;

        public class HomeController : Controller
        {
            // Return a view of hello products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```
4. <span data-ttu-id="dcf7e-191">Expandera hello-mappen Views\Shared i Solution Explorer och dubbelklicka sedan på **_Layout.cshtml** tooopen i hello Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-191">In Solution Explorer, expand hello Views\Shared folder, then double-click **_Layout.cshtml** tooopen it in hello Visual Studio editor.</span></span>
5. <span data-ttu-id="dcf7e-192">Ändra alla förekomster av **My ASP.NET Application** för**LITWARE'S Products**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-192">Change all occurrences of **My ASP.NET Application** too**LITWARE's Products**.</span></span>
6. <span data-ttu-id="dcf7e-193">Ta bort hello **Start**, **om**, och **Kontakta** länkar.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-193">Remove hello **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="dcf7e-194">Ta bort hello markerade koden i hello som följande exempel.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-194">In hello following example, delete hello highlighted code.</span></span>

    ![][41]

7. <span data-ttu-id="dcf7e-195">Expandera hello-mappen Views\Home i Solution Explorer och dubbelklicka sedan på **Index.cshtml** tooopen i hello Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-195">In Solution Explorer, expand hello Views\Home folder, then double-click **Index.cshtml** tooopen it in hello Visual Studio editor.</span></span> <span data-ttu-id="dcf7e-196">Ersätt hello hela innehållet i filen hello med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-196">Replace hello entire contents of hello file with hello following code.</span></span>

   ```html
   @model IEnumerable<ProductsWeb.Models.Product>

   @{
            ViewBag.Title = "Index";
   }

   <h2>Prod Inventory</h2>

   <table>
             <tr>
                 <th>
                     @Html.DisplayNameFor(model => model.Name)
                 </th>
                 <th></th>
                 <th>
                     @Html.DisplayNameFor(model => model.Quantity)
                 </th>
             </tr>

   @foreach (var item in Model) {
             <tr>
                 <td>
                     @Html.DisplayFor(modelItem => item.Name)
                 </td>
                 <td>
                     @Html.DisplayFor(modelItem => item.Quantity)
                 </td>
             </tr>
   }

   </table>
   ```
8. <span data-ttu-id="dcf7e-197">tooverify hello noggrannhet arbete så länge du kan trycka på **Ctrl + Skift + B** toobuild hello projektet.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-197">tooverify hello accuracy of your work so far, you can press **Ctrl+Shift+B** toobuild hello project.</span></span>

### <a name="run-hello-app-locally"></a><span data-ttu-id="dcf7e-198">Kör hello appen lokalt</span><span class="sxs-lookup"><span data-stu-id="dcf7e-198">Run hello app locally</span></span>

<span data-ttu-id="dcf7e-199">Kör hello programmet tooverify att det fungerar.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-199">Run hello application tooverify that it works.</span></span>

1. <span data-ttu-id="dcf7e-200">Se till att **ProductsPortal** är hello aktiva projektet.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-200">Ensure that **ProductsPortal** is hello active project.</span></span> <span data-ttu-id="dcf7e-201">Högerklicka på hello projektnamnet i Solution Explorer och markera **Set As Startup Project**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-201">Right-click hello project name in Solution Explorer and select **Set As Startup Project**.</span></span>
2. <span data-ttu-id="dcf7e-202">I Visual Studio trycker du på **F5**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-202">In Visual Studio, press **F5**.</span></span>
3. <span data-ttu-id="dcf7e-203">Programmet bör visas och körs i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-203">Your application should appear, running in a browser.</span></span>

   ![][21]

## <a name="put-hello-pieces-together"></a><span data-ttu-id="dcf7e-204">Samla hello delar</span><span class="sxs-lookup"><span data-stu-id="dcf7e-204">Put hello pieces together</span></span>

<span data-ttu-id="dcf7e-205">hello nästa steg är toohook in hello lokala produktservern med hello ASP.NET-program.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-205">hello next step is toohook up hello on-premises products server with hello ASP.NET application.</span></span>

1. <span data-ttu-id="dcf7e-206">Om den inte redan är öppen i Visual Studio öppnar den igen hello **ProductsPortal** projekt som du skapade i hello [skapa ett ASP.NET-program](#create-an-aspnet-application) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-206">If it is not already open, in Visual Studio re-open hello **ProductsPortal** project you created in hello [Create an ASP.NET application](#create-an-aspnet-application) section.</span></span>
2. <span data-ttu-id="dcf7e-207">Liknande toohello steg i avsnittet ”Skapa en lokal Server” hello lägga till hello NuGet-paketet toohello projektreferenser.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-207">Similar toohello step in hello "Create an On-Premises Server" section, add hello NuGet package toohello project references.</span></span> <span data-ttu-id="dcf7e-208">I Solution Explorer högerklickar du på hello **ProductsPortal** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-208">In Solution Explorer, right-click hello **ProductsPortal** project, then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="dcf7e-209">Sök efter ”Service Bus” och välj hello **WindowsAzure.ServiceBus** objekt.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-209">Search for "Service Bus" and select hello **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="dcf7e-210">Sedan slutföra hello installationen och Stäng den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-210">Then complete hello installation and close this dialog box.</span></span>
4. <span data-ttu-id="dcf7e-211">I Solution Explorer högerklickar du på hello **ProductsPortal** projektet och klicka sedan på **Lägg till**, sedan **befintlig artikel**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-211">In Solution Explorer, right-click hello **ProductsPortal** project, then click **Add**, then **Existing Item**.</span></span>
5. <span data-ttu-id="dcf7e-212">Navigera toohello **ProductsContract.cs** filen från hello **ProductsServer** console-projekt.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-212">Navigate toohello **ProductsContract.cs** file from hello **ProductsServer** console project.</span></span> <span data-ttu-id="dcf7e-213">Klicka på toohighlight ProductsContract.cs.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-213">Click toohighlight ProductsContract.cs.</span></span> <span data-ttu-id="dcf7e-214">Klicka på hello nedpilen bredvid för**Lägg till**, klicka på **Lägg till som länk**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-214">Click hello down arrow next too**Add**, then click **Add as Link**.</span></span>

   ![][24]

6. <span data-ttu-id="dcf7e-215">Öppna hello **HomeController.cs** filen i hello Visual Studio-redigeraren och Ersätt hello definitionen för namnområdet med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-215">Now open hello **HomeController.cs** file in hello Visual Studio editor and replace hello namespace definition with hello following code.</span></span> <span data-ttu-id="dcf7e-216">Vara säker på att tooreplace *yourServiceNamespace* med hello namnet på namnområdet för tjänsten och *yourKey* med SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-216">Be sure tooreplace *yourServiceNamespace* with hello name of your service namespace, and *yourKey* with your SAS key.</span></span> <span data-ttu-id="dcf7e-217">Detta aktiverar hello klienten toocall hello lokala tjänsten och returnera hello resultatet av hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-217">This will enable hello client toocall hello on-premises service, returning hello result of hello call.</span></span>

   ```csharp
   namespace ProductsWeb.Controllers
   {
       using System.Linq;
       using System.ServiceModel;
       using System.Web.Mvc;
       using Microsoft.ServiceBus;
       using Models;
       using ProductsServer;

       public class HomeController : Controller
       {
           // Declare hello channel factory.
           static ChannelFactory<IProductsChannel> channelFactory;

           static HomeController()
           {
               // Create shared access signature token credentials for authentication.
               channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                   "sb://yourServiceNamespace.servicebus.windows.net/products");
               channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                   TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                       "RootManageSharedAccessKey", "yourKey") });
           }

           public ActionResult Index()
           {
               using (IProductsChannel channel = channelFactory.CreateChannel())
               {
                   // Return a view of hello products inventory.
                   return this.View(from prod in channel.GetProducts()
                                    select
                                        new Product { Id = prod.Id, Name = prod.Name,
                                            Quantity = prod.Quantity });
               }
           }
       }
   }
   ```
7. <span data-ttu-id="dcf7e-218">I Solution Explorer högerklickar du på hello **ProductsPortal** lösning (se till att tooright klickar du på hello lösning, inte hello-projekt).</span><span class="sxs-lookup"><span data-stu-id="dcf7e-218">In Solution Explorer, right-click hello **ProductsPortal** solution (make sure tooright-click hello solution, not hello project).</span></span> <span data-ttu-id="dcf7e-219">Klicka på **Lägg till** och sedan på **Befintligt projekt**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-219">Click **Add**, then click **Existing Project**.</span></span>
8. <span data-ttu-id="dcf7e-220">Navigera toohello **ProductsServer** projektet och sedan dubbelklicka på hello **ProductsServer.csproj** lösning filen tooadd den.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-220">Navigate toohello **ProductsServer** project, then double-click hello **ProductsServer.csproj** solution file tooadd it.</span></span>
9. <span data-ttu-id="dcf7e-221">**ProductsServer** måste köras i ordning toodisplay hello data på **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-221">**ProductsServer** must be running in order toodisplay hello data on **ProductsPortal**.</span></span> <span data-ttu-id="dcf7e-222">I Solution Explorer högerklickar du på hello **ProductsPortal** lösningen och klicka på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-222">In Solution Explorer, right-click hello **ProductsPortal** solution and click **Properties**.</span></span> <span data-ttu-id="dcf7e-223">Hej **egenskapssidor** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-223">hello **Property Pages** dialog box is displayed.</span></span>
10. <span data-ttu-id="dcf7e-224">Klicka på vänster sida hello, **Startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-224">On hello left side, click **Startup Project**.</span></span> <span data-ttu-id="dcf7e-225">Klicka på hello höger **flera Startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-225">On hello right side, click **Multiple startup projects**.</span></span> <span data-ttu-id="dcf7e-226">Se till att **ProductsServer** och **ProductsPortal** visas i den ordningen med **starta** som hello åtgärden för båda.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-226">Ensure that **ProductsServer** and **ProductsPortal** appear, in that order, with **Start** set as hello action for both.</span></span>

      ![][25]

11. <span data-ttu-id="dcf7e-227">Fortfarande i hello **egenskaper** dialogrutan klickar du på **projektberoenden** på hello vänster sida.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-227">Still in hello **Properties** dialog box, click **Project Dependencies** on hello left side.</span></span>
12. <span data-ttu-id="dcf7e-228">I hello **projekt** klickar du på **ProductsServer**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-228">In hello **Projects** list, click **ProductsServer**.</span></span> <span data-ttu-id="dcf7e-229">Se till att **ProductsPortal** inte är markerat.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-229">Ensure that **ProductsPortal** is not selected.</span></span>
13. <span data-ttu-id="dcf7e-230">I hello **projekt** klickar du på **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-230">In hello **Projects** list, click **ProductsPortal**.</span></span> <span data-ttu-id="dcf7e-231">Se till att **ProductsServer** är markerat.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-231">Ensure that **ProductsServer** is selected.</span></span>

    ![][26]

14. <span data-ttu-id="dcf7e-232">Klicka på **OK** i hello **egenskapssidor** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-232">Click **OK** in hello **Property Pages** dialog box.</span></span>

## <a name="run-hello-project-locally"></a><span data-ttu-id="dcf7e-233">Kör hello projektet lokalt</span><span class="sxs-lookup"><span data-stu-id="dcf7e-233">Run hello project locally</span></span>

<span data-ttu-id="dcf7e-234">tootest hello programmet lokalt, i Visual Studio trycker du på **F5**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-234">tootest hello application locally, in Visual Studio press **F5**.</span></span> <span data-ttu-id="dcf7e-235">hello lokal server (**ProductsServer**) bör starta först och sedan hello **ProductsPortal** bör programmet startas i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-235">hello on-premises server (**ProductsServer**) should start first, then hello **ProductsPortal** application should start in a browser window.</span></span> <span data-ttu-id="dcf7e-236">Den här gången ser du att hello produktinformation innehåller data som hämtats från hello produkten tjänsten lokalt system.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-236">This time, you will see that hello product inventory lists data retrieved from hello product service on-premises system.</span></span>

![][10]

<span data-ttu-id="dcf7e-237">Tryck på **uppdatera** på hello **ProductsPortal** sidan.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-237">Press **Refresh** on hello **ProductsPortal** page.</span></span> <span data-ttu-id="dcf7e-238">Varje gång du uppdaterar hello sidan ser du hello appservern visar ett meddelande när `GetProducts()` från **ProductsServer** anropas.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-238">Each time you refresh hello page, you'll see hello server app display a message when `GetProducts()` from **ProductsServer** is called.</span></span>

<span data-ttu-id="dcf7e-239">Stänga båda programmen innan du fortsätter toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-239">Close both applications before proceeding toohello next step.</span></span>

## <a name="deploy-hello-productsportal-project-tooan-azure-web-app"></a><span data-ttu-id="dcf7e-240">Distribuera hello ProductsPortal-projektet tooan Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="dcf7e-240">Deploy hello ProductsPortal project tooan Azure web app</span></span>

<span data-ttu-id="dcf7e-241">hello nästa steg är toorepublish hello Azure Web app **ProductsPortal** klientdel.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-241">hello next step is toorepublish hello Azure Web app **ProductsPortal** frontend.</span></span> <span data-ttu-id="dcf7e-242">Hej du följande:</span><span class="sxs-lookup"><span data-stu-id="dcf7e-242">Do hello following:</span></span>

1. <span data-ttu-id="dcf7e-243">I Solution Explorer högerklickar du på hello **ProductsPortal** projektet och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-243">In Solution Explorer, right-click hello **ProductsPortal** project, and click **Publish**.</span></span> <span data-ttu-id="dcf7e-244">Klicka på **publicera** på hello **publicera** sidan.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-244">Then, click **Publish** on hello **Publish** page.</span></span>

  > [!NOTE]
  > <span data-ttu-id="dcf7e-245">Du kan se ett felmeddelande i webbläsarfönstret hello när hello **ProductsPortal** webbprojekt startas automatiskt efter hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-245">You may see an error message in hello browser window when hello **ProductsPortal** web project is automatically launched after hello deployment.</span></span> <span data-ttu-id="dcf7e-246">Detta är förväntat och beror på att hello **ProductsServer** programmet ännu inte körs.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-246">This is expected, and occurs because hello **ProductsServer** application isn't running yet.</span></span>
>
>

2. <span data-ttu-id="dcf7e-247">Kopiera hello URL för hello distribueras webbapp som du behöver hello URL: en i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-247">Copy hello URL of hello deployed web app, as you will need hello URL in hello next step.</span></span> <span data-ttu-id="dcf7e-248">Du kan även hämta denna URL från hello Azure App Service aktivitetsfönstret i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="dcf7e-248">You can also obtain this URL from hello Azure App Service Activity window in Visual Studio:</span></span>

  ![][9]

3. <span data-ttu-id="dcf7e-249">Stäng hello webbläsare fönstret toostop hello program körs.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-249">Close hello browser window toostop hello running application.</span></span>

### <a name="set-productsportal-as-web-app"></a><span data-ttu-id="dcf7e-250">Ställa in ProductsPortal som en webbapp</span><span class="sxs-lookup"><span data-stu-id="dcf7e-250">Set ProductsPortal as web app</span></span>

<span data-ttu-id="dcf7e-251">Innan program som körs hello i hello molnet, måste du se till att **ProductsPortal** startas från Visual Studio som en webbapp.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-251">Before running hello application in hello cloud, you must ensure that **ProductsPortal** is launched from within Visual Studio as a web app.</span></span>

1. <span data-ttu-id="dcf7e-252">I Visual Studio högerklickar du på hello **ProductsPortal** projektet och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-252">In Visual Studio, right-click hello **ProductsPortal** project and then click **Properties**.</span></span>
2. <span data-ttu-id="dcf7e-253">Klicka på hello vänstra kolumnen **Web**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-253">In hello left-hand column, click **Web**.</span></span>
3. <span data-ttu-id="dcf7e-254">I hello **Starta åtgärden** klickar du på hello **starta URL** och i textrutan för hello anger hello URL för ditt tidigare distribuerade webbprogram, till exempel `http://productsportal1234567890.azurewebsites.net/`.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-254">In hello **Start Action** section, click hello **Start URL** button, and in hello text box enter hello URL for your previously deployed web app; for example, `http://productsportal1234567890.azurewebsites.net/`.</span></span>

    ![][27]

4. <span data-ttu-id="dcf7e-255">Från hello **filen** -menyn i Visual Studio klickar du på **spara alla**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-255">From hello **File** menu in Visual Studio, click **Save All**.</span></span>
5. <span data-ttu-id="dcf7e-256">Hello Build-menyn i Visual Studio klickar du på **återskapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-256">From hello Build menu in Visual Studio, click **Rebuild Solution**.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="dcf7e-257">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="dcf7e-257">Run hello application</span></span>

1. <span data-ttu-id="dcf7e-258">Tryck på F5 toobuild och kör programmet hello.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-258">Press F5 toobuild and run hello application.</span></span> <span data-ttu-id="dcf7e-259">hello lokal server (hello **ProductsServer** konsolprogrammet) bör starta först och sedan hello **ProductsPortal** program ska starta i ett webbläsarfönster som visas i följande skärmbild hello bild.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-259">hello on-premises server (hello **ProductsServer** console application) should start first, then hello **ProductsPortal** application should start in a browser window, as shown in hello following screen shot.</span></span> <span data-ttu-id="dcf7e-260">Observera igen att hello produktinformation visar data som hämtats från hello produkten tjänsten lokalt system och data i hello webbapp.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-260">Notice again that hello product inventory lists data retrieved from hello product service on-premises system, and displays that data in hello web app.</span></span> <span data-ttu-id="dcf7e-261">Kontrollera hello URL toomake att som **ProductsPortal** körs i hello molnet, som en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-261">Check hello URL toomake sure that **ProductsPortal** is running in hello cloud, as an Azure web app.</span></span>

   ![][1]

   > [!IMPORTANT]
   > <span data-ttu-id="dcf7e-262">Hej **ProductsServer** konsolprogram måste vara körs och kan tooserve hello data toohello **ProductsPortal** program.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-262">hello **ProductsServer** console application must be running and able tooserve hello data toohello **ProductsPortal** application.</span></span> <span data-ttu-id="dcf7e-263">Om ett felmeddelande visas i webbläsaren hello Vänta några sekunder för **ProductsServer** tooload och visa hello följande meddelande.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-263">If hello browser displays an error, wait a few more seconds for **ProductsServer** tooload and display hello following message.</span></span> <span data-ttu-id="dcf7e-264">Tryck på **uppdatera** i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-264">Then press **Refresh** in hello browser.</span></span>
   >
   >

   ![][37]
2. <span data-ttu-id="dcf7e-265">Tillbaka i hello webbläsaren trycker du på **uppdatera** på hello **ProductsPortal** sidan.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-265">Back in hello browser, press **Refresh** on hello **ProductsPortal** page.</span></span> <span data-ttu-id="dcf7e-266">Varje gång du uppdaterar hello sidan ser du hello appservern visar ett meddelande när `GetProducts()` från **ProductsServer** anropas.</span><span class="sxs-lookup"><span data-stu-id="dcf7e-266">Each time you refresh hello page, you'll see hello server app display a message when `GetProducts()` from **ProductsServer** is called.</span></span>

    ![][38]

## <a name="next-steps"></a><span data-ttu-id="dcf7e-267">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dcf7e-267">Next steps</span></span>

<span data-ttu-id="dcf7e-268">toolearn mer om Azure Relay finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="dcf7e-268">toolearn more about Azure Relay, see hello following resources:</span></span>  

* [<span data-ttu-id="dcf7e-269">Vad är Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="dcf7e-269">What is Azure Relay?</span></span>](relay-what-is-it.md)  
* [<span data-ttu-id="dcf7e-270">Hur toouse relä</span><span class="sxs-lookup"><span data-stu-id="dcf7e-270">How toouse Relay</span></span>](service-bus-dotnet-how-to-use-relay.md)  

[0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
[1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[NuGet]: http://nuget.org

[11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
[13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
[15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
[16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
[17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
[18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
[9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
[10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

[21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
[24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
[25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
[26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
[27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png

[36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
[38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
[41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
[43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png
