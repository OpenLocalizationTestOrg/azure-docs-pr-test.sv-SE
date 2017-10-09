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
# <a name="using-net-sdk-tooaccess-azure-mobile-engagement-service-apis"></a>Med hjälp av .NET SDK tooaccess Azure Mobile Engagement Service API: er
Azure Mobile Engagement Exponerar en uppsättning API du toomanage enheter Reach/Push kampanjer etc. toointeract med dessa API: er, vi också ge dig en [Swagger-filen](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) som du kan använda med verktygen toogenerate SDK för din önskade språk. Vi rekommenderar att du använder hello [AutoRest](https://github.com/Azure/AutoRest) verktyget toogenerate din SDK från våra Swagger-fil.

> [!NOTE]
> hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder. Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Vi har skapat en .NET-SDK på liknande sätt som gör att du toointeract med dessa API: er med hjälp av en C#-omslutning och du har inte toodo hello autentisering token förhandling och uppdatera själv.  

Det här exemplet passerar hello uppsättning steg toofollow toouse hello .NET SDK:

1. För alla, måste du först toosetup hello autentisering för dina API: er med hjälp av hello Azure Active Directory enligt beskrivningen [här](mobile-engagement-api-authentication.md#authentication). Hello slutet av dessa steg, bör du ha en giltig **SubscriptionId**, **TenantId**, **ApplicationId** och **hemlighet**. 
2. Vi använder en enkel konsolen Windows app toodemonstrate arbeta med hello .NET SDK med hello scenario för att skapa ett meddelande kampanj. Så öppnar Visual Studio och skapa en **konsolprogram**.   
3. Därefter behöver du toodownload hello .NET SDK som är tillgängliga som **bibliotek för Microsoft Azure Engagement** i hello Nuget-galleriet [här](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
   Om du installerar hello Nuget från Visual Studio, behöver du tooensure att du har kontroll markerats hello **inkludera förhandsversion** alternativet vid sökning efter hello paketet:
   
    ![][1]
4. I hello `Program.cs` lägger du till hello följande namnområden:
   
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;
5. Därefter behöver du toodefine hello följande konstanter som vi ska använda för autentisering och interagerar med hello Mobile Engagement-App som du skapar hello-meddelande kampanj:
   
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
6. Definiera hello `EngagementManagementClient` variabel som ska användas för toocall hello Mobile Engagement SDK metoder:
   
        static EngagementManagementClient engagementClient; 
7. Lägg till följande tooyour hello `Main` metoden:
   
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
8. Definiera hello följande metod som tar hand om initierar hello `EngagementManagementClient` genom att först autentisera och sedan associera med hello Mobile Engagement-App som du planerar toocreate hello-meddelande kampanj:
   
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
   > Observera att du toouse hello **App resursnamnet** som definierats i hello Azure-hanteringsportalen för hello AppName parametern. 
   > 
   > 
9. Slutligen, definiera hello CreateCampaign metod som tar hand om med hello tidigare initierad EngagementClient toocreate en enkel **när som helst** & **endast meddelande** kampanj med en rubrik och meddelande: 
   
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
10. Kör hello konsolprogram och du ser följande hello på hello kampanj har skapats:
    
    **Kampanj-Id '159' skapades och det är i tillståndet 'utkast.**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
