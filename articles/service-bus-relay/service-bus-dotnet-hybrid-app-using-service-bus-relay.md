---
title: Azure WCF Relay-hybridprogram lokalt/i molnet (.NET) | Microsoft Docs
description: "Lär dig hur du skapar ett lokalt eller molnbaserat .NET-hybridprogram med hjälp av Azure WCF Relay."
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
ms.openlocfilehash: 366922a083b9d18ef50e04eb8b459d2725315e1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="net-on-premisescloud-hybrid-application-using-azure-wcf-relay"></a><span data-ttu-id="4e2b9-103">Lokalt eller molnbaserat .NET-hybridprogram med hjälp av Azure WCF Relay</span><span class="sxs-lookup"><span data-stu-id="4e2b9-103">.NET on-premises/cloud hybrid application using Azure WCF Relay</span></span>
## <a name="introduction"></a><span data-ttu-id="4e2b9-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="4e2b9-104">Introduction</span></span>

<span data-ttu-id="4e2b9-105">Den här artikeln visar hur du skapar ett hybridprogram i molnet med Microsoft Azure och Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-105">This article shows how to build a hybrid cloud application with Microsoft Azure and Visual Studio.</span></span> <span data-ttu-id="4e2b9-106">Den här självstudiekursen förutsätter att du inte har några tidigare erfarenheter av att använda Azure.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-106">The tutorial assumes you have no prior experience using Azure.</span></span> <span data-ttu-id="4e2b9-107">På mindre än 30 minuter kommer du att ha ett program färdigt i molnet som använder en rad Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-107">In less than 30 minutes, you will have an application that uses multiple Azure resources up and running in the cloud.</span></span>

<span data-ttu-id="4e2b9-108">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="4e2b9-108">You will learn:</span></span>

* <span data-ttu-id="4e2b9-109">Hur du skapar eller anpassar en befintlig webbtjänst för att den ska kunna användas av en webblösning.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-109">How to create or adapt an existing web service for consumption by a web solution.</span></span>
* <span data-ttu-id="4e2b9-110">Lär dig hur du använder Azure WCF Relay-tjänsten för att dela data mellan ett Azure-program och en värdbaserad webbtjänst som finns någon annanstans.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-110">How to use the Azure WCF Relay service to share data between an Azure application and a web service hosted elsewhere.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a><span data-ttu-id="4e2b9-111">Så här hjälper Azure Relay dig med hybridlösningar</span><span class="sxs-lookup"><span data-stu-id="4e2b9-111">How Azure Relay helps with hybrid solutions</span></span>

<span data-ttu-id="4e2b9-112">Företagslösningar består normalt av en kombination av anpassad kod – som skrivs för att ta itu med nya och unika verksamhetskrav – och befintliga funktioner som kommer från lösningar och system som redan finns på plats.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-112">Business solutions are typically composed of a combination of custom code written to tackle new and unique business requirements and existing functionality provided by solutions and systems that are already in place.</span></span>

<span data-ttu-id="4e2b9-113">Lösningsarkitekter har börjat använda molnet för enklare hantering av skalkrav och lägre driftskostnader.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-113">Solution architects are starting to use the cloud for easier handling of scale requirements and lower operational costs.</span></span> <span data-ttu-id="4e2b9-114">När de gör detta upptäcker de att de befintliga tjänstetillgångarna, som de vill utnyttja som byggblock för sina lösningar, ligger innanför företagets brandvägg och därför är svåra att nå för de lösningar som ligger i molnet.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-114">In doing so, they find that existing service assets they'd like to leverage as building blocks for their solutions are inside the corporate firewall and out of easy reach for access by the cloud solution.</span></span> <span data-ttu-id="4e2b9-115">Många interna tjänster är inte skapade eller värdbaserade på ett sätt som gör att de enkelt kan exponeras vid företagets nätverksgräns.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-115">Many internal services are not built or hosted in a way that they can be easily exposed at the corporate network edge.</span></span>

<span data-ttu-id="4e2b9-116">[Azure Relay](https://azure.microsoft.com/services/service-bus/) är avsett för användningsfall där befintliga WCF-webbtjänster (Windows Communication Foundation) på ett säkert sätt görs tillgängliga för lösningar som finns utanför företagets perimeter, utan behov av störande ändringar i företagets nätverksinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-116">[Azure Relay](https://azure.microsoft.com/services/service-bus/) is designed for the use-case of taking existing Windows Communication Foundation (WCF) web services and making those services securely accessible to solutions that reside outside the corporate perimeter without requiring intrusive changes to the corporate network infrastructure.</span></span> <span data-ttu-id="4e2b9-117">Sådana relätjänster finns fortfarande i deras befintliga miljö, men de delegerar lyssnandet efter inkommande sessioner och förfrågningar till relätjänsten i molnet.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-117">Such relay services are still hosted inside their existing environment, but they delegate listening for incoming sessions and requests to the cloud-hosted relay service.</span></span> <span data-ttu-id="4e2b9-118">Azure Relay skyddar även tjänsterna mot obehörig åtkomst med hjälp av [signatur för delad åtkomst (SAS)](../service-bus-messaging/service-bus-sas.md)-autentisering.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-118">Azure Relay also protects those services from unauthorized access by using [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) authentication.</span></span>

## <a name="solution-scenario"></a><span data-ttu-id="4e2b9-119">Lösningsscenario</span><span class="sxs-lookup"><span data-stu-id="4e2b9-119">Solution scenario</span></span>
<span data-ttu-id="4e2b9-120">I den här självstudiekursen kommer du att skapa en ASP.NET-webbplats som gör att du kan se en lista över produkter på sidan för inventarieförteckningar.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-120">In this tutorial, you will create an ASP.NET website that enables you to see a list of products on the product inventory page.</span></span>

![][0]

<span data-ttu-id="4e2b9-121">I självstudiekursen förutsätter vi att du har produktinformation i ett befintligt, lokalt system och använder Azure Relay för att få åtkomst till det systemet.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-121">The tutorial assumes that you have product information in an existing on-premises system, and uses Azure Relay to reach into that system.</span></span> <span data-ttu-id="4e2b9-122">Det här simuleras av en webbtjänst som körs i ett enkelt konsolprogram och backas upp av en minnesintern uppsättning av produkter.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-122">This is simulated by a web service that runs in a simple console application and is backed by an in-memory set of products.</span></span> <span data-ttu-id="4e2b9-123">Du kommer att kunna köra detta konsolprogram på din dator och distribuera webbrollen till Azure.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-123">You will be able to run this console application on your own computer and deploy the web role into Azure.</span></span> <span data-ttu-id="4e2b9-124">När du gör detta, kommer du att se hur den webbroll som körs i Azures datacenter verkligen anropar din datorn, även om datorn sannolikt ligger bakom en brandvägg och ett NAT-lager (Network Address Translation).</span><span class="sxs-lookup"><span data-stu-id="4e2b9-124">By doing so, you will see how the web role running in the Azure datacenter will indeed call into your computer, even though your computer will almost certainly reside behind at least one firewall and a network address translation (NAT) layer.</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="4e2b9-125">Konfigurera utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="4e2b9-125">Set up the development environment</span></span>

<span data-ttu-id="4e2b9-126">Innan du kan börja utveckla Azure-program måste du hämta de verktyg som krävs och ställa in din utvecklingsmiljö:</span><span class="sxs-lookup"><span data-stu-id="4e2b9-126">Before you can begin developing Azure applications, download the tools and set up your development environment:</span></span>

1. <span data-ttu-id="4e2b9-127">Installera Azure SDK för .NET från [hämtningssidan](https://azure.microsoft.com/downloads/) för SDK.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-127">Install the Azure SDK for .NET from the SDK [downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="4e2b9-128">Klicka på den version av [Visual Studio](http://www.visualstudio.com) du använder i kolumnen **.NET**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-128">In the **.NET** column, click the version of [Visual Studio](http://www.visualstudio.com) you are using.</span></span> <span data-ttu-id="4e2b9-129">Stegen i den här handledningen använder Visual Studio 2015, men det går lika bra med Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-129">The steps in this tutorial use Visual Studio 2015, but they also work with Visual Studio 2017.</span></span>
3. <span data-ttu-id="4e2b9-130">När du uppmanas att köra eller spara installationsprogrammet, klickar du på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-130">When prompted to run or save the installer, click **Run**.</span></span>
4. <span data-ttu-id="4e2b9-131">I **Installationsprogram för webbplattform** klickar du på **Installera** och fortsätter med installationen.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-131">In the **Web Platform Installer**, click **Install** and proceed with the installation.</span></span>
5. <span data-ttu-id="4e2b9-132">När installationen är klar har du allt som behövs för att börja utveckla appen.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-132">Once the installation is complete, you will have everything necessary to start to develop the app.</span></span> <span data-ttu-id="4e2b9-133">SDK inkluderar verktyg som låter dig utveckla Azure-program i Visual Studio på ett enkelt sätt.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-133">The SDK includes tools that let you easily develop Azure applications in Visual Studio.</span></span>

## <a name="create-a-namespace"></a><span data-ttu-id="4e2b9-134">Skapa ett namnområde</span><span class="sxs-lookup"><span data-stu-id="4e2b9-134">Create a namespace</span></span>

<span data-ttu-id="4e2b9-135">För att komma igång med reläfunktionerna i Azure måste du först skapa ett namnområde för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-135">To begin using the relay features in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="4e2b9-136">Ett namnområde tillhandahåller en omfångsbehållare för adressering av Azure-resurser i ditt program.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-136">A namespace provides a scoping container for addressing Azure resources within your application.</span></span> <span data-ttu-id="4e2b9-137">Följ [anvisningarna här](relay-create-namespace-portal.md) för att skapa ett Relay-namnområde.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-137">Follow the [instructions here](relay-create-namespace-portal.md) to create a Relay namespace.</span></span>

## <a name="create-an-on-premises-server"></a><span data-ttu-id="4e2b9-138">Skapa en lokal server</span><span class="sxs-lookup"><span data-stu-id="4e2b9-138">Create an on-premises server</span></span>

<span data-ttu-id="4e2b9-139">Först ska du skapa ett lokalt produktkatalogsystem (ett fingerat sådant).</span><span class="sxs-lookup"><span data-stu-id="4e2b9-139">First, you will build a (mock) on-premises product catalog system.</span></span> <span data-ttu-id="4e2b9-140">Det kan vara ett ganska enkel system. Du kan se detta som en representation av ett faktiskt, lokalt produktkatalogsystem med en fullständig serviceyta som vi försöker integrera.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-140">It will be fairly simple; you can see this as representing an actual on-premises product catalog system with a complete service surface that we're trying to integrate.</span></span>

<span data-ttu-id="4e2b9-141">Det här projektet är ett konsolprogram för Visual Studio som använder [Azure Service Bus NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) för att inkludera Service Bus-bibliotek och -konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-141">This project is a Visual Studio console application, and uses the [Azure Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) to include the Service Bus libraries and configuration settings.</span></span>

### <a name="create-the-project"></a><span data-ttu-id="4e2b9-142">Skapa projektet</span><span class="sxs-lookup"><span data-stu-id="4e2b9-142">Create the project</span></span>

1. <span data-ttu-id="4e2b9-143">Starta Microsoft Visual Studio med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-143">Using administrator privileges, start Microsoft Visual Studio.</span></span> <span data-ttu-id="4e2b9-144">För att göra det högerklickar du på programikonen för Visual Studio och sedan på **Kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-144">To do so, right-click the Visual Studio program icon, and then click **Run as administrator**.</span></span>
2. <span data-ttu-id="4e2b9-145">I Visual Studio klickar du på **Nytt** i menyn **Arkiv** och sedan på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-145">In Visual Studio, on the **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="4e2b9-146">Klicka på **Konsolprogram (.NET Framework** från **Installerade mallar** under **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-146">From **Installed Templates**, under **Visual C#**, click **Console App (.NET Framework)**.</span></span> <span data-ttu-id="4e2b9-147">I rutan **Namn** anger du namnet **ProductsServer**:</span><span class="sxs-lookup"><span data-stu-id="4e2b9-147">In the **Name** box, type the name **ProductsServer**:</span></span>

   ![][11]
4. <span data-ttu-id="4e2b9-148">Klicka på **OK** för att skapa **ProductsServer**-projektet.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-148">Click **OK** to create the **ProductsServer** project.</span></span>
5. <span data-ttu-id="4e2b9-149">Om du redan har installerat NuGet Package Manager för Visual Studio kan du hoppa över nästa steg.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-149">If you have already installed the NuGet package manager for Visual Studio, skip to the next step.</span></span> <span data-ttu-id="4e2b9-150">Annars går du till [NuGet][NuGet] och klickar på [Installera NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c).</span><span class="sxs-lookup"><span data-stu-id="4e2b9-150">Otherwise, visit [NuGet][NuGet] and click [Install NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c).</span></span> <span data-ttu-id="4e2b9-151">Följ anvisningarna för att installera NuGet Package Manager och starta sedan om Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-151">Follow the prompts to install the NuGet package manager, then re-start Visual Studio.</span></span>
6. <span data-ttu-id="4e2b9-152">Högerklicka på projektet **ProductsServer** i Solution Explorer och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-152">In Solution Explorer, right-click the **ProductsServer** project, then click **Manage NuGet Packages**.</span></span>
7. <span data-ttu-id="4e2b9-153">Klicka på **Bläddra**-fliken och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-153">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="4e2b9-154">Välj paketet **WindowsAzure.ServiceBus**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-154">Select the **WindowsAzure.ServiceBus** package.</span></span>
8. <span data-ttu-id="4e2b9-155">Klicka på **Installera** och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-155">Click **Install**, and accept the terms of use.</span></span>

   ![][13]

   <span data-ttu-id="4e2b9-156">Observera att alla nödvändiga klientsammansättningar nu är refererade.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-156">Note that the required client assemblies are now referenced.</span></span>
8. <span data-ttu-id="4e2b9-157">Lägg till en ny klass för ditt produktkontrakt.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-157">Add a new class for your product contract.</span></span> <span data-ttu-id="4e2b9-158">Högerklicka på projektet **ProductsServer** i Solution Explorer. Klicka sedan på **Lägg till** och på **Klass**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-158">In Solution Explorer, right-click the **ProductsServer** project and click **Add**, and then click **Class**.</span></span>
9. <span data-ttu-id="4e2b9-159">I rutan **Namn** anger du namnet **ProductsServer.cs**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-159">In the **Name** box, type the name **ProductsContract.cs**.</span></span> <span data-ttu-id="4e2b9-160">Klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-160">Then click **Add**.</span></span>
10. <span data-ttu-id="4e2b9-161">I **ProductsContract.cs** ersätter du definitionen för namnområdet med följande kod. Den definierar kontraktet för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-161">In **ProductsContract.cs**, replace the namespace definition with the following code, which defines the contract for the service.</span></span>

    ```csharp
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;

        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }

        // Define the service contract.
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
11. <span data-ttu-id="4e2b9-162">Ersätt definitionen för namnområdet i Program.cs med följande kod. Den lägger till profiltjänsten och värden för den.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-162">In Program.cs, replace the namespace definition with the following code, which adds the profile service and the host for it.</span></span>

    ```csharp
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;

        // Implement the IProducts interface.
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

            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }

        }

        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();

                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();

                sh.Close();
            }
        }
    }
    ```
12. <span data-ttu-id="4e2b9-163">Dubbelklicka på filen **App.config** i Solution Explorer för att öppna den i Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-163">In Solution Explorer, double-click the **App.config** file to open it in the Visual Studio editor.</span></span> <span data-ttu-id="4e2b9-164">Längst ned i elementet `<system.ServiceModel>` (men fortfarande i `<system.ServiceModel>`), lägger du till följande XML-kod.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-164">At the bottom of the `<system.ServiceModel>` element (but still within `<system.ServiceModel>`), add the following XML code.</span></span> <span data-ttu-id="4e2b9-165">Se till att ersätta *yourServiceNamespace* med namnet på ditt namnområdet och *yourKey* med den SAS-nyckel som du tidigare hämtade från portalen:</span><span class="sxs-lookup"><span data-stu-id="4e2b9-165">Be sure to replace *yourServiceNamespace* with the name of your namespace, and *yourKey* with the SAS key you retrieved earlier from the portal:</span></span>

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
13. <span data-ttu-id="4e2b9-166">Medan du fortfarande är kvar i App.config, i elementet `<appSettings>`, byter du ut värdet för anslutningssträngen mot den sträng som du tidigare fick från portalen.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-166">Still in App.config, in the `<appSettings>` element, replace the connection string value with the connection string you previously obtained from the portal.</span></span>

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```
14. <span data-ttu-id="4e2b9-167">Tryck på **Ctrl+Shift+B**, eller på **Skapa lösning** från menyn **Skapa**, för att skapa programmet och kontrollera att allt du gjort hittills är korrekt.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-167">Press **Ctrl+Shift+B** or from the **Build** menu, click **Build Solution** to build the application and verify the accuracy of your work so far.</span></span>

## <a name="create-an-aspnet-application"></a><span data-ttu-id="4e2b9-168">Skapa ett ASP.NET-program</span><span class="sxs-lookup"><span data-stu-id="4e2b9-168">Create an ASP.NET application</span></span>

<span data-ttu-id="4e2b9-169">I det här avsnittet skapar du ett enkelt ASP.NET-program som visar data som hämtats från din produkttjänst.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-169">In this section you will build a simple ASP.NET application that displays data retrieved from your product service.</span></span>

### <a name="create-the-project"></a><span data-ttu-id="4e2b9-170">Skapa projektet</span><span class="sxs-lookup"><span data-stu-id="4e2b9-170">Create the project</span></span>

1. <span data-ttu-id="4e2b9-171">Se till att Visual Studio körs med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-171">Ensure that Visual Studio is running with administrator privileges.</span></span>
2. <span data-ttu-id="4e2b9-172">I Visual Studio klickar du på **Nytt** i menyn **Arkiv** och sedan på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-172">In Visual Studio, on the **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="4e2b9-173">Klicka på **ASP.NET webbapp (.NET Framwork)** från **Installerade mallar** under **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-173">From **Installed Templates**, under **Visual C#**, click **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="4e2b9-174">Ge projektet namnet **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-174">Name the project **ProductsPortal**.</span></span> <span data-ttu-id="4e2b9-175">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-175">Then click **OK**.</span></span>

   ![][15]

4. <span data-ttu-id="4e2b9-176">Från listan **ASP.NET-mallar** i dialogrutan **Nytt ASP.NET Web Application** dialogrutan klickar du på **MVC**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-176">From the **ASP.NET Templates** list in the **New ASP.NET Web Application** dialog, click **MVC**.</span></span>

   ![][16]

6. <span data-ttu-id="4e2b9-177">Klicka på knappen **Ändra autentisering**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-177">Click the **Change Authentication** button.</span></span> <span data-ttu-id="4e2b9-178">I dialogrutan **Ändra autentisering** kontrollerar du att **Ingen autentisering** har markerats och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-178">In the **Change Authentication** dialog box, ensure that **No Authentication** is selected, and then click **OK**.</span></span> <span data-ttu-id="4e2b9-179">För den här självstudiekursen distribuerar du en app som inte kräver någon användarinloggning.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-179">For this tutorial, you're deploying an app that does not need a user login.</span></span>

    ![][18]

7. <span data-ttu-id="4e2b9-180">I dialogrutan **Ny ASP.NET Web Application** klickar du på **OK** för att skapa MVC-app.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-180">Back in the **New ASP.NET Web Application** dialog, click **OK** to create the MVC app.</span></span>
8. <span data-ttu-id="4e2b9-181">Nu måste du konfigurera Azure-resurserna för en ny webbapp.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-181">Now you must configure Azure resources for a new web app.</span></span> <span data-ttu-id="4e2b9-182">Följ stegen i avsnittet [Publicera till Azure i den här artikeln](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="4e2b9-182">Follow the steps in the [Publish to Azure section of this article](../app-service-web/app-service-web-get-started-dotnet.md).</span></span> <span data-ttu-id="4e2b9-183">Gå sedan tillbaka till den här självstudien och gå vidare till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-183">Then, return to this tutorial and proceed to the next step.</span></span>
10. <span data-ttu-id="4e2b9-184">I Solution Explorer högerklickar du på **Modeller**. Klicka sedan på **Lägg till** och på **Klass**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-184">In Solution Explorer, right-click **Models** and then click **Add**, then click **Class**.</span></span> <span data-ttu-id="4e2b9-185">Ange namnet **Product.cs** i rutan **Namn**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-185">In the **Name** box, type the name **Product.cs**.</span></span> <span data-ttu-id="4e2b9-186">Klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-186">Then click **Add**.</span></span>

    ![][17]

### <a name="modify-the-web-application"></a><span data-ttu-id="4e2b9-187">Göra ändringar i webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="4e2b9-187">Modify the web application</span></span>

1. <span data-ttu-id="4e2b9-188">Ersätt den befintliga definitionen för namnområdet med följande kod i filen Product.cs i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-188">In the Product.cs file in Visual Studio, replace the existing namespace definition with the following code.</span></span>

   ```csharp
    // Declare properties for the products inventory.
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
2. <span data-ttu-id="4e2b9-189">I Solution Explorer expanderar du mappen **Controllers** och sedan dubbelklickar du på filen **HomeController.cs** för att öppna den i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-189">In Solution Explorer, expand the **Controllers** folder, then double-click the **HomeController.cs** file to open it in Visual Studio.</span></span>
3. <span data-ttu-id="4e2b9-190">Ersätt den befintliga definitionen för namnområdet med följande kod i **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-190">In **HomeController.cs**, replace the existing namespace definition with the following code.</span></span>

    ```csharp
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;

        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```
4. <span data-ttu-id="4e2b9-191">Expandera mappen Views\Shared i Solution Explorer och dubbelklicka sedan på **_Layout.cshtml** så att den öppnas i Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-191">In Solution Explorer, expand the Views\Shared folder, then double-click **_Layout.cshtml** to open it in the Visual Studio editor.</span></span>
5. <span data-ttu-id="4e2b9-192">Ändra alla förekomster av **My ASP.NET Application** till **LITWARE's Products**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-192">Change all occurrences of **My ASP.NET Application** to **LITWARE's Products**.</span></span>
6. <span data-ttu-id="4e2b9-193">Ta bort länkarna för **Start**, **Om** och **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-193">Remove the **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="4e2b9-194">Ta bort den markerade koden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-194">In the following example, delete the highlighted code.</span></span>

    ![][41]

7. <span data-ttu-id="4e2b9-195">Expandera mappen Views\Home i Solution Explorer och dubbelklicka sedan på **Index.cshtml** så att den öppnas i Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-195">In Solution Explorer, expand the Views\Home folder, then double-click **Index.cshtml** to open it in the Visual Studio editor.</span></span> <span data-ttu-id="4e2b9-196">Ersätt hela innehållet i filen med följande kod.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-196">Replace the entire contents of the file with the following code.</span></span>

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
8. <span data-ttu-id="4e2b9-197">För att kontrollera om det arbete du gjort hittills är korrekt trycker du på **Ctrl + Skift + B** att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-197">To verify the accuracy of your work so far, you can press **Ctrl+Shift+B** to build the project.</span></span>

### <a name="run-the-app-locally"></a><span data-ttu-id="4e2b9-198">Köra appen lokalt</span><span class="sxs-lookup"><span data-stu-id="4e2b9-198">Run the app locally</span></span>

<span data-ttu-id="4e2b9-199">Kör programmet för att kontrollera att det fungerar.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-199">Run the application to verify that it works.</span></span>

1. <span data-ttu-id="4e2b9-200">Se till att **ProductsPortal** är det aktiva projektet.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-200">Ensure that **ProductsPortal** is the active project.</span></span> <span data-ttu-id="4e2b9-201">Högerklicka på projektnamnet på i Solution Explorer och markera **Ställ in som startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-201">Right-click the project name in Solution Explorer and select **Set As Startup Project**.</span></span>
2. <span data-ttu-id="4e2b9-202">I Visual Studio trycker du på **F5**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-202">In Visual Studio, press **F5**.</span></span>
3. <span data-ttu-id="4e2b9-203">Programmet bör visas och körs i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-203">Your application should appear, running in a browser.</span></span>

   ![][21]

## <a name="put-the-pieces-together"></a><span data-ttu-id="4e2b9-204">Knyta ihop säcken</span><span class="sxs-lookup"><span data-stu-id="4e2b9-204">Put the pieces together</span></span>

<span data-ttu-id="4e2b9-205">Nästa steg är att koppla samman den lokala produktservern med ASP.NET-programmet.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-205">The next step is to hook up the on-premises products server with the ASP.NET application.</span></span>

1. <span data-ttu-id="4e2b9-206">Om programmet inte redan är öppet öppnar du det **ProductsPortal**-projekt som du skapade i avsnittet [Skapa ett ASP.NET-program](#create-an-aspnet-application) igen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-206">If it is not already open, in Visual Studio re-open the **ProductsPortal** project you created in the [Create an ASP.NET application](#create-an-aspnet-application) section.</span></span>
2. <span data-ttu-id="4e2b9-207">Lägg till NuGet-paketet i projektreferenserna på ett liknande sätt som i steget i avsnittet ”Skapa en lokal server”.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-207">Similar to the step in the "Create an On-Premises Server" section, add the NuGet package to the project references.</span></span> <span data-ttu-id="4e2b9-208">Högerklicka på projektet **ProductsPortal** i Solution Explorer och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-208">In Solution Explorer, right-click the **ProductsPortal** project, then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="4e2b9-209">Sök efter "Service Bus" och markera posten **WindowsAzure Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-209">Search for "Service Bus" and select the **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="4e2b9-210">Slutför sedan installationen och stäng den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-210">Then complete the installation and close this dialog box.</span></span>
4. <span data-ttu-id="4e2b9-211">Högerklicka på projektet **ProductsPortal** i Solution Explorer och klicka sedan på **Lägg till** och **Befintligt objekt**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-211">In Solution Explorer, right-click the **ProductsPortal** project, then click **Add**, then **Existing Item**.</span></span>
5. <span data-ttu-id="4e2b9-212">Navigera till filen **ProductsContract.cs** från konsolprojektet **ProductsServer**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-212">Navigate to the **ProductsContract.cs** file from the **ProductsServer** console project.</span></span> <span data-ttu-id="4e2b9-213">Klicka för att markera ProductsContract.cs.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-213">Click to highlight ProductsContract.cs.</span></span> <span data-ttu-id="4e2b9-214">Klicka på nedåtpilen bredvid **Lägg till** och sedan på **Lägg till som länk**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-214">Click the down arrow next to **Add**, then click **Add as Link**.</span></span>

   ![][24]

6. <span data-ttu-id="4e2b9-215">Öppna nu filen **HomeController.cs** i Visual Studio-redigeraren och ersätt definitionen för namnområde med följande kod.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-215">Now open the **HomeController.cs** file in the Visual Studio editor and replace the namespace definition with the following code.</span></span> <span data-ttu-id="4e2b9-216">Se till att ersätta *yourServiceNamespace* med namnet på ditt namnområde för tjänsten och *yourKey* med din SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-216">Be sure to replace *yourServiceNamespace* with the name of your service namespace, and *yourKey* with your SAS key.</span></span> <span data-ttu-id="4e2b9-217">Detta aktiverar klienten för att anropa den lokala tjänsten och returnera resultatet av anropet.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-217">This will enable the client to call the on-premises service, returning the result of the call.</span></span>

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
           // Declare the channel factory.
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
                   // Return a view of the products inventory.
                   return this.View(from prod in channel.GetProducts()
                                    select
                                        new Product { Id = prod.Id, Name = prod.Name,
                                            Quantity = prod.Quantity });
               }
           }
       }
   }
   ```
7. <span data-ttu-id="4e2b9-218">I Solution Explorer högerklickar du på lösningen **ProductsPortal** (se till att du högerklickar på lösningen, inte projektet).</span><span class="sxs-lookup"><span data-stu-id="4e2b9-218">In Solution Explorer, right-click the **ProductsPortal** solution (make sure to right-click the solution, not the project).</span></span> <span data-ttu-id="4e2b9-219">Klicka på **Lägg till** och sedan på **Befintligt projekt**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-219">Click **Add**, then click **Existing Project**.</span></span>
8. <span data-ttu-id="4e2b9-220">Navigera till projektet **ProductsServer** och dubbelklicka sedan på lösningsfilen **ProductsServer.csproj** för att lägga till den.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-220">Navigate to the **ProductsServer** project, then double-click the **ProductsServer.csproj** solution file to add it.</span></span>
9. <span data-ttu-id="4e2b9-221">**ProductsServer** måste köras för att kunna visa data på **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-221">**ProductsServer** must be running in order to display the data on **ProductsPortal**.</span></span> <span data-ttu-id="4e2b9-222">Högerklicka på lösningen **ProductsPortal** i Solution Explorer och klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-222">In Solution Explorer, right-click the **ProductsPortal** solution and click **Properties**.</span></span> <span data-ttu-id="4e2b9-223">Dialogrutan **Egenskapssidor** visas.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-223">The **Property Pages** dialog box is displayed.</span></span>
10. <span data-ttu-id="4e2b9-224">Klicka på **Startprojekt** på den vänstra sidan.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-224">On the left side, click **Startup Project**.</span></span> <span data-ttu-id="4e2b9-225">Klicka på **Flera startprojekt** på den högra sidan.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-225">On the right side, click **Multiple startup projects**.</span></span> <span data-ttu-id="4e2b9-226">Se till att **ProductsServer** och **ProductsPortal** visas, och i den ordningen. Åtgärden för båda ska vara **Starta**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-226">Ensure that **ProductsServer** and **ProductsPortal** appear, in that order, with **Start** set as the action for both.</span></span>

      ![][25]

11. <span data-ttu-id="4e2b9-227">Medan du fortfarande är kvar i dialogrutan **Egenskaper**, klickar du på **Projektberoenden** på den vänstra sidan.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-227">Still in the **Properties** dialog box, click **Project Dependencies** on the left side.</span></span>
12. <span data-ttu-id="4e2b9-228">Klicka på **ProductsServer** i listan **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-228">In the **Projects** list, click **ProductsServer**.</span></span> <span data-ttu-id="4e2b9-229">Se till att **ProductsPortal** inte är markerat.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-229">Ensure that **ProductsPortal** is not selected.</span></span>
13. <span data-ttu-id="4e2b9-230">Klicka på **ProductsPortal** i listan **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-230">In the **Projects** list, click **ProductsPortal**.</span></span> <span data-ttu-id="4e2b9-231">Se till att **ProductsServer** är markerat.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-231">Ensure that **ProductsServer** is selected.</span></span>

    ![][26]

14. <span data-ttu-id="4e2b9-232">Klicka på **OK** i dialogrutan **Egenskapssidor**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-232">Click **OK** in the **Property Pages** dialog box.</span></span>

## <a name="run-the-project-locally"></a><span data-ttu-id="4e2b9-233">Köra projektet lokalt</span><span class="sxs-lookup"><span data-stu-id="4e2b9-233">Run the project locally</span></span>

<span data-ttu-id="4e2b9-234">Tryck på **F5** i Visual Studio för att testa programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-234">To test the application locally, in Visual Studio press **F5**.</span></span> <span data-ttu-id="4e2b9-235">Den lokala servern (**ProductsServer**) bör starta först och sedan ska programmet **ProductsPortal** starta i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-235">The on-premises server (**ProductsServer**) should start first, then the **ProductsPortal** application should start in a browser window.</span></span> <span data-ttu-id="4e2b9-236">Den här gången ser du att data för produktinventarielistorna hämtats från det lokala systemet för produkttjänster.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-236">This time, you will see that the product inventory lists data retrieved from the product service on-premises system.</span></span>

![][10]

<span data-ttu-id="4e2b9-237">Tryck på **Uppdatera** på sidan **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-237">Press **Refresh** on the **ProductsPortal** page.</span></span> <span data-ttu-id="4e2b9-238">Varje gång du uppdaterar sidan kommer du att se att appservern visar ett meddelande när `GetProducts()` från **ProductsServer** anropas.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-238">Each time you refresh the page, you'll see the server app display a message when `GetProducts()` from **ProductsServer** is called.</span></span>

<span data-ttu-id="4e2b9-239">Stäng båda programmen innan du fortsätter till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-239">Close both applications before proceeding to the next step.</span></span>

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a><span data-ttu-id="4e2b9-240">Distribuera ProductsPortal-projektet till en Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="4e2b9-240">Deploy the ProductsPortal project to an Azure web app</span></span>

<span data-ttu-id="4e2b9-241">Nästa steg är att publicera om klientdelen för **ProductsPortal** som en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-241">The next step is to republish the Azure Web app **ProductsPortal** frontend.</span></span> <span data-ttu-id="4e2b9-242">Gör följande:</span><span class="sxs-lookup"><span data-stu-id="4e2b9-242">Do the following:</span></span>

1. <span data-ttu-id="4e2b9-243">Högerklicka på lösningen **ProductsPortal** i Solution Explorer och klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-243">In Solution Explorer, right-click the **ProductsPortal** project, and click **Publish**.</span></span> <span data-ttu-id="4e2b9-244">Klicka på **Publicera** på **publiceringssidan**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-244">Then, click **Publish** on the **Publish** page.</span></span>

  > [!NOTE]
  > <span data-ttu-id="4e2b9-245">Du kanske får upp ett felmeddelande i webbläsarfönstret när webbprojektet **ProductsPortal** startas automatiskt efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-245">You may see an error message in the browser window when the **ProductsPortal** web project is automatically launched after the deployment.</span></span> <span data-ttu-id="4e2b9-246">Detta är normalt och beror på att **ProductsServer**-programmet ännu inte körs.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-246">This is expected, and occurs because the **ProductsServer** application isn't running yet.</span></span>
>
>

2. <span data-ttu-id="4e2b9-247">Kopiera den distribuerade webbappens URL eftersom du kommer att behöva URL-adressen i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-247">Copy the URL of the deployed web app, as you will need the URL in the next step.</span></span> <span data-ttu-id="4e2b9-248">Du kan även hämta denna URL från aktivitetsfönstret Azure App Service i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4e2b9-248">You can also obtain this URL from the Azure App Service Activity window in Visual Studio:</span></span>

  ![][9]

3. <span data-ttu-id="4e2b9-249">Stäng webbläsarfönstret för att stoppa programmet som körs.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-249">Close the browser window to stop the running application.</span></span>

### <a name="set-productsportal-as-web-app"></a><span data-ttu-id="4e2b9-250">Ställa in ProductsPortal som en webbapp</span><span class="sxs-lookup"><span data-stu-id="4e2b9-250">Set ProductsPortal as web app</span></span>

<span data-ttu-id="4e2b9-251">Innan du kör programmet i molnet måste du se till att **ProductsPortal** startas från Visual Studio som en webbapp.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-251">Before running the application in the cloud, you must ensure that **ProductsPortal** is launched from within Visual Studio as a web app.</span></span>

1. <span data-ttu-id="4e2b9-252">Högerklicka på projektet **ProductsPortal** i Visual Studio och klicka sedan på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-252">In Visual Studio, right-click the **ProductsPortal** project and then click **Properties**.</span></span>
2. <span data-ttu-id="4e2b9-253">Klicka på **Webb** i den vänstra kolumnen.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-253">In the left-hand column, click **Web**.</span></span>
3. <span data-ttu-id="4e2b9-254">I avsnittet **Starta åtgärden** klickar du på knappen **Starta URL**. Ange URL-adressen för den webbapp som du distribuerade i de föregående stegen i textrutan, till exempel `http://productsportal1234567890.azurewebsites.net/`.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-254">In the **Start Action** section, click the **Start URL** button, and in the text box enter the URL for your previously deployed web app; for example, `http://productsportal1234567890.azurewebsites.net/`.</span></span>

    ![][27]

4. <span data-ttu-id="4e2b9-255">Klicka på **Spara alla** från menyn **Arkiv** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-255">From the **File** menu in Visual Studio, click **Save All**.</span></span>
5. <span data-ttu-id="4e2b9-256">Klicka på **Återskapa lösning** från menyn Skapa i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-256">From the Build menu in Visual Studio, click **Rebuild Solution**.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="4e2b9-257">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="4e2b9-257">Run the application</span></span>

1. <span data-ttu-id="4e2b9-258">Tryck på F5 för att skapa och köra programmet.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-258">Press F5 to build and run the application.</span></span> <span data-ttu-id="4e2b9-259">Den lokala servern (konsolprogrammet **ProductsServer**) bör starta först och sedan ska programmet **ProductsPortal** starta i ett webbläsarfönster, som visas på följande skärmdump.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-259">The on-premises server (the **ProductsServer** console application) should start first, then the **ProductsPortal** application should start in a browser window, as shown in the following screen shot.</span></span> <span data-ttu-id="4e2b9-260">Återigen ser du att data för produktinventarielistorna hämtats från det lokala systemet för produkttjänster. Dessa data visas sedan i webbappen.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-260">Notice again that the product inventory lists data retrieved from the product service on-premises system, and displays that data in the web app.</span></span> <span data-ttu-id="4e2b9-261">Kontrollera URL:en för att se till att **ProductsPortal** körs i molnet, som en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-261">Check the URL to make sure that **ProductsPortal** is running in the cloud, as an Azure web app.</span></span>

   ![][1]

   > [!IMPORTANT]
   > <span data-ttu-id="4e2b9-262">Konsolprogrammet **ProductsServer** måste köras och det måste kunna skicka data till programmet **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-262">The **ProductsServer** console application must be running and able to serve the data to the **ProductsPortal** application.</span></span> <span data-ttu-id="4e2b9-263">Om ett felmeddelande visas i webbläsaren ska du vänta i några sekunder för att **ProductsServer** ska läsas in. Den visar sedan följande meddelande.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-263">If the browser displays an error, wait a few more seconds for **ProductsServer** to load and display the following message.</span></span> <span data-ttu-id="4e2b9-264">Tryck sedan på **Uppdatera** i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-264">Then press **Refresh** in the browser.</span></span>
   >
   >

   ![][37]
2. <span data-ttu-id="4e2b9-265">När du har gått tillbaka till webbläsaren trycker du på **Uppdatera** på sidan **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-265">Back in the browser, press **Refresh** on the **ProductsPortal** page.</span></span> <span data-ttu-id="4e2b9-266">Varje gång du uppdaterar sidan kommer du att se att appservern visar ett meddelande när `GetProducts()` från **ProductsServer** anropas.</span><span class="sxs-lookup"><span data-stu-id="4e2b9-266">Each time you refresh the page, you'll see the server app display a message when `GetProducts()` from **ProductsServer** is called.</span></span>

    ![][38]

## <a name="next-steps"></a><span data-ttu-id="4e2b9-267">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4e2b9-267">Next steps</span></span>

<span data-ttu-id="4e2b9-268">Mer information om Azure Relay finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="4e2b9-268">To learn more about Azure Relay, see the following resources:</span></span>  

* [<span data-ttu-id="4e2b9-269">Vad är Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="4e2b9-269">What is Azure Relay?</span></span>](relay-what-is-it.md)  
* [<span data-ttu-id="4e2b9-270">Använda vidarebefordran</span><span class="sxs-lookup"><span data-stu-id="4e2b9-270">How to use Relay</span></span>](service-bus-dotnet-how-to-use-relay.md)  

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
