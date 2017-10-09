---
title: 'aaaUsing .NET SDK tooaccess Azure Mobile Engagement Service API: er'
description: 'Beskriver hur toouse hello Mobile Engagement .NET SDK tooaccess Azure Mobile Engagement Service API: er'
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: c07728aa-43f2-4238-8b4a-c9eddf9d838b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 812be6f507b29f7b2de6454e06face8082c2d161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-net-sdk-tooaccess-azure-mobile-engagement-service-apis"></a><span data-ttu-id="d8267-103">Med hjälp av .NET SDK tooaccess Azure Mobile Engagement Service API: er</span><span class="sxs-lookup"><span data-stu-id="d8267-103">Using .NET SDK tooaccess Azure Mobile Engagement Service APIs</span></span>
<span data-ttu-id="d8267-104">Azure Mobile Engagement Exponerar en uppsättning API du toomanage enheter Reach/Push kampanjer etc. toointeract med dessa API: er, vi också ge dig en [Swagger-filen](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) som du kan använda med verktygen toogenerate SDK för din önskade språk.</span><span class="sxs-lookup"><span data-stu-id="d8267-104">Azure Mobile Engagement exposes a set of APIs for you toomanage Devices, Reach/Push campaigns etc. toointeract with these APIs, we also provide you a [Swagger file](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) that you can use with tools toogenerate SDKs for your preferred language.</span></span> <span data-ttu-id="d8267-105">Vi rekommenderar att du använder hello [AutoRest](https://github.com/Azure/AutoRest) verktyget toogenerate din SDK från våra Swagger-fil.</span><span class="sxs-lookup"><span data-stu-id="d8267-105">We recommend using hello [AutoRest](https://github.com/Azure/AutoRest) tool toogenerate your SDK from our Swagger file.</span></span>

> [!NOTE]
> <span data-ttu-id="d8267-106">hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder.</span><span class="sxs-lookup"><span data-stu-id="d8267-106">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="d8267-107">Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="d8267-107">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="d8267-108">Vi har skapat en .NET-SDK på liknande sätt som gör att du toointeract med dessa API: er med hjälp av en C#-omslutning och du har inte toodo hello autentisering token förhandling och uppdatera själv.</span><span class="sxs-lookup"><span data-stu-id="d8267-108">We have created a .NET SDK in a similar manner which allows you toointeract with these APIs using a C# wrapper and you don't have toodo hello authentication token negotiation and refresh yourself.</span></span>  

<span data-ttu-id="d8267-109">Det här exemplet passerar hello uppsättning steg toofollow toouse hello .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="d8267-109">This sample goes through hello set of steps toofollow toouse hello .NET SDK:</span></span>

1. <span data-ttu-id="d8267-110">För alla, måste du först toosetup hello autentisering för dina API: er med hjälp av hello Azure Active Directory enligt beskrivningen [här](mobile-engagement-api-authentication.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="d8267-110">First of all, you need toosetup hello authentication for your APIs using hello Azure Active Directory as described [here](mobile-engagement-api-authentication.md#authentication).</span></span> <span data-ttu-id="d8267-111">Hello slutet av dessa steg, bör du ha en giltig **SubscriptionId**, **TenantId**, **ApplicationId** och **hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="d8267-111">At hello end of these steps, you should have a valid **SubscriptionId**, **TenantId**, **ApplicationId** and **Secret**.</span></span> 
2. <span data-ttu-id="d8267-112">Vi använder en enkel konsolen Windows app toodemonstrate arbeta med hello .NET SDK med hello scenario för att skapa ett meddelande kampanj.</span><span class="sxs-lookup"><span data-stu-id="d8267-112">We will use a simple Windows Console app toodemonstrate working with hello .NET SDK with hello scenario of creating an Announcement campaign.</span></span> <span data-ttu-id="d8267-113">Så öppnar Visual Studio och skapa en **konsolprogram**.</span><span class="sxs-lookup"><span data-stu-id="d8267-113">So open up Visual Studio and create a **Console Application**.</span></span>   
3. <span data-ttu-id="d8267-114">Därefter behöver du toodownload hello .NET SDK som är tillgängliga som **bibliotek för Microsoft Azure Engagement** i hello Nuget-galleriet [här](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).</span><span class="sxs-lookup"><span data-stu-id="d8267-114">Next you need toodownload hello .NET SDK which is available as **Microsoft Azure Engagement Management Library** in hello Nuget gallery [here](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).</span></span>
   <span data-ttu-id="d8267-115">Om du installerar hello Nuget från Visual Studio, behöver du tooensure att du har kontroll markerats hello **inkludera förhandsversion** alternativet vid sökning efter hello paketet:</span><span class="sxs-lookup"><span data-stu-id="d8267-115">If you are installing hello Nuget from Visual Studio, you need tooensure that you have check marked hello **Include prerelease** option while searching for hello package:</span></span>
   
    ![][1]
4. <span data-ttu-id="d8267-116">I hello `Program.cs` lägger du till hello följande namnområden:</span><span class="sxs-lookup"><span data-stu-id="d8267-116">In hello `Program.cs` file, add hello following namespaces:</span></span>
   
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;
5. <span data-ttu-id="d8267-117">Därefter behöver du toodefine hello följande konstanter som vi ska använda för autentisering och interagerar med hello Mobile Engagement-App som du skapar hello-meddelande kampanj:</span><span class="sxs-lookup"><span data-stu-id="d8267-117">Next you need toodefine hello following constants that we will use for authentication and interacting with hello Mobile Engagement App in which you are creating hello Announcement campaign:</span></span>
   
        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";
   
        // This is hello Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";
   
        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using hello one as specified in hello Azure portal (NOT hello App Name)
        const string APP_RESOURCE_NAME = "";
6. <span data-ttu-id="d8267-118">Definiera hello `EngagementManagementClient` variabel som ska användas för toocall hello Mobile Engagement SDK metoder:</span><span class="sxs-lookup"><span data-stu-id="d8267-118">Define hello `EngagementManagementClient` variable which we will use toocall hello Mobile Engagement SDK methods:</span></span>
   
        static EngagementManagementClient engagementClient; 
7. <span data-ttu-id="d8267-119">Lägg till följande tooyour hello `Main` metoden:</span><span class="sxs-lookup"><span data-stu-id="d8267-119">Add hello following tooyour `Main` method:</span></span>
   
        try
            {
                // Initialize hello Engagement SDK toocall out APIs. 
                InitEngagementClient().Wait();
   
                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }
8. <span data-ttu-id="d8267-120">Definiera hello följande metod som tar hand om initierar hello `EngagementManagementClient` genom att först autentisera och sedan associera med hello Mobile Engagement-App som du planerar toocreate hello-meddelande kampanj:</span><span class="sxs-lookup"><span data-stu-id="d8267-120">Define hello following method which takes care of initializing hello `EngagementManagementClient` by first authenticating and then associating itself with hello Mobile Engagement App in which you plan toocreate hello Announcement campaign:</span></span>
   
        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
   
            // This is hello Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;
   
            // This is your Mobile Engagement App Collection & App Resource Name in which you create hello campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }
   
   > [!IMPORTANT]
   > <span data-ttu-id="d8267-121">Observera att du toouse hello **App resursnamnet** som definierats i hello Azure-hanteringsportalen för hello AppName parametern.</span><span class="sxs-lookup"><span data-stu-id="d8267-121">Note that you need toouse hello **App Resource Name** as defined in hello Azure management portal for hello AppName parameter.</span></span> 
   > 
   > 
9. <span data-ttu-id="d8267-122">Slutligen, definiera hello CreateCampaign metod som tar hand om med hello tidigare initierad EngagementClient toocreate en enkel **när som helst** & **endast meddelande** kampanj med en rubrik och meddelande:</span><span class="sxs-lookup"><span data-stu-id="d8267-122">Lastly, define hello CreateCampaign method which will take care of using hello previously initialized EngagementClient toocreate a simple **AnyTime** & **Notification-only** campaign with a title and message:</span></span> 
   
        private async static Task CreateCampaign()
        {
            //  Refer toohello Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all hello non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing hello app!",
                type:"only_notif",
                deliveryTime:"any"
                );
   
            // Refer toohello Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }
10. <span data-ttu-id="d8267-123">Kör hello konsolprogram och du ser följande hello på hello kampanj har skapats:</span><span class="sxs-lookup"><span data-stu-id="d8267-123">Run hello console app and you will see hello following on successful creation of hello campaign:</span></span>
    
    <span data-ttu-id="d8267-124">**Kampanj-Id '159' skapades och det är i tillståndet 'utkast.**</span><span class="sxs-lookup"><span data-stu-id="d8267-124">**Campaign Id '159' was created successfully and it is in 'draft' state**</span></span>

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
