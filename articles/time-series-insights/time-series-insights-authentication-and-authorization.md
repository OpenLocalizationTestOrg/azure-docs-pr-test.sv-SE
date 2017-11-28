---
title: "aaaConfigure autentisering och auktorisering för ett anpassat program som anropar hello Azure tid serien Insights API | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur tooconfigure autentisering och auktorisering för ett anpassat program som anropar hello Azure tid serien Insights API"
keywords: 
services: time-series-insights
documentationcenter: 
author: dmdenmsft
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/24/2017
ms.author: dmden
ms.openlocfilehash: 5043468bfc2af3c0d27e8602508d92ba2848409e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a><span data-ttu-id="3c2dc-103">Autentisering och auktorisering för Azure tid serien Insights API</span><span class="sxs-lookup"><span data-stu-id="3c2dc-103">Authentication and authorization for Azure Time Series Insights API</span></span>

<span data-ttu-id="3c2dc-104">Den här artikeln förklarar hur tooconfigure ett anpassat program som anropar hello Azure tid serien Insights API.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-104">This article explains how tooconfigure a custom application that calls hello Azure Time Series Insights API.</span></span>

## <a name="service-principal"></a><span data-ttu-id="3c2dc-105">Tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="3c2dc-105">Service principal</span></span>

<span data-ttu-id="3c2dc-106">Det här avsnittet beskrivs hur tooconfigure ett program tooaccess hello tid serien Insights API uppdrag hello program.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-106">This section explains how tooconfigure an application tooaccess hello Time Series Insights API on behalf of hello application.</span></span> <span data-ttu-id="3c2dc-107">hello program kan sedan fråga data eller publicera referensdata i hello tid serien insikter miljö med autentiseringsuppgifter och inte hello användarens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-107">hello application can then query data or publish reference data in hello Time Series Insights environment with application credentials and not hello user credentials.</span></span>

<span data-ttu-id="3c2dc-108">När du har ett program som behöver tooaccess tid serien insikter måste du konfigurera ett program för Azure Active Directory och tilldelar hello principerna dataåtkomst i hello tid serien insikter miljö.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-108">When you have an application that needs tooaccess Time Series Insights, you must set up an Azure Active Directory application and assign hello data access policies in hello Time Series Insights environment.</span></span> <span data-ttu-id="3c2dc-109">Den här metoden är bättre toorunning hello app enligt dina autentiseringsuppgifter eftersom:</span><span class="sxs-lookup"><span data-stu-id="3c2dc-109">This approach is preferable toorunning hello app under your own credentials because:</span></span>

* <span data-ttu-id="3c2dc-110">Du kan tilldela behörigheter toohello app identitet som skiljer sig från din egen behörigheter.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-110">You can assign permissions toohello app identity that are different from your own permissions.</span></span> <span data-ttu-id="3c2dc-111">Dessa behörigheter normalt begränsade tooexactly vilka hello program behöver toodo.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-111">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span> <span data-ttu-id="3c2dc-112">Du kan till exempel tillåta hello app tooonly läsa data i en viss tid serien insikter-miljö.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-112">For example, you can allow hello app tooonly read data in a particular Time Series Insights environment.</span></span>
* <span data-ttu-id="3c2dc-113">Du har inte toochange hello app autentiseringsuppgifter ändrar dina ansvarsområden.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-113">You don't have toochange hello app's credentials if your responsibilities change.</span></span>
* <span data-ttu-id="3c2dc-114">Du kan använda ett certifikat eller en nyckel tooautomate autentisering när du kör ett oövervakat skript.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-114">You can use a certificate or an application key tooautomate authentication when you're running an unattended script.</span></span>

<span data-ttu-id="3c2dc-115">Den här artikeln visar hur tooperform de går igenom hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-115">This article shows you how tooperform those steps through hello Azure portal.</span></span> <span data-ttu-id="3c2dc-116">Den fokuserar på en enskild klient-program där programmet hello är avsedda toorun i en enda organisation.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-116">It focuses on a single-tenant application where hello application is intended toorun in only one organization.</span></span> <span data-ttu-id="3c2dc-117">Stöd för en innehavare program används vanligtvis för line-of-business-program som körs i din organisation.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-117">You typically use single-tenant applications for line-of-business applications that run in your organization.</span></span>

<span data-ttu-id="3c2dc-118">hello installationsprogrammet flödet består av tre huvudsakliga steg:</span><span class="sxs-lookup"><span data-stu-id="3c2dc-118">hello setup flow consists of three high-level steps:</span></span>

1. <span data-ttu-id="3c2dc-119">Skapa ett program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-119">Create an application in Azure Active Directory.</span></span>
2. <span data-ttu-id="3c2dc-120">Auktorisera den här tooaccess hello tid serien insikter programmiljö.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-120">Authorize this application tooaccess hello Time Series Insights environment.</span></span>
3. <span data-ttu-id="3c2dc-121">Använd hello program-ID och nyckel tooacquire token toohello `"https://api.timeseries.azure.com/"` publik eller resursen.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-121">Use hello application ID and key tooacquire a token toohello `"https://api.timeseries.azure.com/"` audience or resource.</span></span> <span data-ttu-id="3c2dc-122">hello token kan sedan använda toocall hello tid serien Insights API.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-122">hello token can then be used toocall hello Time Series Insights API.</span></span>

<span data-ttu-id="3c2dc-123">Här följer hello detaljerade anvisningar:</span><span class="sxs-lookup"><span data-stu-id="3c2dc-123">Here are hello detailed steps:</span></span>

1. <span data-ttu-id="3c2dc-124">Markera i hello Azure-portalen, **Azure Active Directory** > **App registreringar** > **nya appregistrering**.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-124">In hello Azure portal, select **Azure Active Directory** > **App registrations** > **New application registration**.</span></span>

   ![Ny programmet registrering i Azure Active Directory](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. <span data-ttu-id="3c2dc-126">Ge hello programmet ett namn, Välj hello typen toobe **webbapp / API**, Välj en giltig URI för **inloggnings-URL**, och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-126">Give hello application a name, select hello type toobe **Web app / API**, select any valid URI for **Sign-on URL**, and click **Create**.</span></span>

   ![Skapa hello program i Azure Active Directory](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. <span data-ttu-id="3c2dc-128">Välj ditt nya program och kopiera dess program-ID tooyour favoritprogram för textredigering.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-128">Select your newly created application and copy its application ID tooyour favorite text editor.</span></span>

   ![Kopiera hello program-ID](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. <span data-ttu-id="3c2dc-130">Välj **nycklar**, ange hello nyckelnamn, Välj hello förfallodatum, och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-130">Select **Keys**, enter hello key name, select hello expiration, and click **Save**.</span></span>

   ![Välj programnycklar](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Ange hello nyckelnamn och upphör att gälla och klicka på Spara](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. <span data-ttu-id="3c2dc-133">Kopiera hello viktiga tooyour favoritprogram för textredigering.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-133">Copy hello key tooyour favorite text editor.</span></span>

   ![Kopiera hello tangent](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. <span data-ttu-id="3c2dc-135">Hello tid serien insikter miljö, Välj **principerna dataåtkomst** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-135">For hello Time Series Insights environment, select **Data Access Policies** and click **Add**.</span></span>

   ![Lägga till nya data access princip toohello tid serien insikter miljön](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. <span data-ttu-id="3c2dc-137">I hello **Välj användare** dialogrutan, klistra in hello programnamn (från steg 2) eller program-ID (från steg 3).</span><span class="sxs-lookup"><span data-stu-id="3c2dc-137">In hello **Select User** dialog box, paste hello application name (from step 2) or application ID (from step 3).</span></span>

   ![Hitta ett program i dialogrutan Välj användare för hello](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. <span data-ttu-id="3c2dc-139">Välj hello roll (**Reader** för datafrågor, **deltagare** för datafrågor och ändra referensdata) och klicka på **Ok**.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-139">Select hello role (**Reader** for querying data, **Contributor** for querying data and changing reference data) and click **Ok**.</span></span>

   ![Välj läsaren eller deltagare i dialogrutan för hello Välj roll](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. <span data-ttu-id="3c2dc-141">Spara hello princip genom att klicka på **Ok**.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-141">Save hello policy by clicking **Ok**.</span></span>

10. <span data-ttu-id="3c2dc-142">Använd hello program-ID (från steg 3) och applikationstoken nyckel (från steg 5) tooacquire hello uppdrag hello program.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-142">Use hello application ID (from step 3) and application key (from step 5) tooacquire hello token on behalf of hello application.</span></span> <span data-ttu-id="3c2dc-143">hello token kan sedan skickas i hello `Authorization` huvud när hello programmet anrop hello tid serien Insights API.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-143">hello token can then be passed in hello `Authorization` header when hello application calls hello Time Series Insights API.</span></span>

    <span data-ttu-id="3c2dc-144">Om du använder C#, kan du använda följande kod tooacquire hello token uppdrag hello programmet hello.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-144">If you're using C#, you can use hello following code tooacquire hello token on behalf of hello application.</span></span> <span data-ttu-id="3c2dc-145">Ett komplett exempel finns [fråga data med hjälp av C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="3c2dc-145">For a complete sample, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

    ```csharp
    var authenticationContext = new AuthenticationContext(
        "https://login.microsoftonline.com/common",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set hello resource URI toohello Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/", 
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of hello application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

## <a name="next-steps"></a><span data-ttu-id="3c2dc-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c2dc-146">Next steps</span></span>

<span data-ttu-id="3c2dc-147">Använda hello program-ID och nyckel i ditt program.</span><span class="sxs-lookup"><span data-stu-id="3c2dc-147">Use hello application ID and key in your application.</span></span> <span data-ttu-id="3c2dc-148">Exempelkod som anropar hello tid serien Insights API finns [fråga data med hjälp av C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="3c2dc-148">For sample code that calls hello Time Series Insights API, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="3c2dc-149">Se även</span><span class="sxs-lookup"><span data-stu-id="3c2dc-149">See also</span></span>

* <span data-ttu-id="3c2dc-150">[Frågor API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) hello fullständig fråge-API-referens</span><span class="sxs-lookup"><span data-stu-id="3c2dc-150">[Query API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) for hello full Query API reference</span></span>
* [<span data-ttu-id="3c2dc-151">Skapa en tjänstens huvudnamn i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3c2dc-151">Create a service principal in hello Azure portal</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
