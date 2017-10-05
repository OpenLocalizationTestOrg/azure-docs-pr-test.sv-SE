---
title: "Konfigurera autentisering och auktorisering för ett anpassat program som anropar Azure tid serien Insights API | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur du konfigurerar autentisering och auktorisering för ett anpassat program som anropar Azure tid serien Insights API"
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
ms.openlocfilehash: 4dd4865dc556e09a31d2cb7a32768aeb19ba9900
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a><span data-ttu-id="e74fd-103">Autentisering och auktorisering för Azure tid serien Insights API</span><span class="sxs-lookup"><span data-stu-id="e74fd-103">Authentication and authorization for Azure Time Series Insights API</span></span>

<span data-ttu-id="e74fd-104">Den här artikeln förklarar hur du konfigurerar ett anpassat program som anropar Azure tid serien Insights API.</span><span class="sxs-lookup"><span data-stu-id="e74fd-104">This article explains how to configure a custom application that calls the Azure Time Series Insights API.</span></span>

## <a name="service-principal"></a><span data-ttu-id="e74fd-105">Tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="e74fd-105">Service principal</span></span>

<span data-ttu-id="e74fd-106">Det här avsnittet beskrivs hur du konfigurerar ett program åtkomst till tid serien Insights API för programmet.</span><span class="sxs-lookup"><span data-stu-id="e74fd-106">This section explains how to configure an application to access the Time Series Insights API on behalf of the application.</span></span> <span data-ttu-id="e74fd-107">Programmet kan sedan fråga data eller publicera referensdata i tid serien insikter-miljö med referenser och inte användarens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e74fd-107">The application can then query data or publish reference data in the Time Series Insights environment with application credentials and not the user credentials.</span></span>

<span data-ttu-id="e74fd-108">När du har ett program som behöver åtkomst tiden serien insikter måste du ställa in ett Azure Active Directory-program och tilldelar åtkomstprinciper data i tid serien insikter-miljö.</span><span class="sxs-lookup"><span data-stu-id="e74fd-108">When you have an application that needs to access Time Series Insights, you must set up an Azure Active Directory application and assign the data access policies in the Time Series Insights environment.</span></span> <span data-ttu-id="e74fd-109">Den här metoden är bättre att köra appen enligt dina autentiseringsuppgifter eftersom:</span><span class="sxs-lookup"><span data-stu-id="e74fd-109">This approach is preferable to running the app under your own credentials because:</span></span>

* <span data-ttu-id="e74fd-110">Du kan tilldela behörigheter till app-identitet som skiljer sig från din egen behörighet.</span><span class="sxs-lookup"><span data-stu-id="e74fd-110">You can assign permissions to the app identity that are different from your own permissions.</span></span> <span data-ttu-id="e74fd-111">Dessa behörigheter normalt begränsad till exakt vad appen behöver göra.</span><span class="sxs-lookup"><span data-stu-id="e74fd-111">Typically, these permissions are restricted to exactly what the app needs to do.</span></span> <span data-ttu-id="e74fd-112">Du kan exempelvis tillåta appen endast läsa data i en viss tid serien insikter miljö.</span><span class="sxs-lookup"><span data-stu-id="e74fd-112">For example, you can allow the app to only read data in a particular Time Series Insights environment.</span></span>
* <span data-ttu-id="e74fd-113">Du behöver ändra appens autentiseringsuppgifterna ändrar dina ansvarsområden.</span><span class="sxs-lookup"><span data-stu-id="e74fd-113">You don't have to change the app's credentials if your responsibilities change.</span></span>
* <span data-ttu-id="e74fd-114">Du kan använda ett certifikat eller en tangent för att automatisera autentisering när du kör ett oövervakat skript.</span><span class="sxs-lookup"><span data-stu-id="e74fd-114">You can use a certificate or an application key to automate authentication when you're running an unattended script.</span></span>

<span data-ttu-id="e74fd-115">Den här artikeln visar hur du utför dessa åtgärder via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e74fd-115">This article shows you how to perform those steps through the Azure portal.</span></span> <span data-ttu-id="e74fd-116">Den fokuserar på en enskild klient program där programmet är avsett att köras i en enda organisation.</span><span class="sxs-lookup"><span data-stu-id="e74fd-116">It focuses on a single-tenant application where the application is intended to run in only one organization.</span></span> <span data-ttu-id="e74fd-117">Stöd för en innehavare program används vanligtvis för line-of-business-program som körs i din organisation.</span><span class="sxs-lookup"><span data-stu-id="e74fd-117">You typically use single-tenant applications for line-of-business applications that run in your organization.</span></span>

<span data-ttu-id="e74fd-118">Installationsprogrammet flödet består av tre huvudsakliga steg:</span><span class="sxs-lookup"><span data-stu-id="e74fd-118">The setup flow consists of three high-level steps:</span></span>

1. <span data-ttu-id="e74fd-119">Skapa ett program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e74fd-119">Create an application in Azure Active Directory.</span></span>
2. <span data-ttu-id="e74fd-120">Det här programmet behörighet att komma åt gången serien insikter-miljön.</span><span class="sxs-lookup"><span data-stu-id="e74fd-120">Authorize this application to access the Time Series Insights environment.</span></span>
3. <span data-ttu-id="e74fd-121">Använda program-ID och nyckel för att hämta en token för den `"https://api.timeseries.azure.com/"` publik eller resursen.</span><span class="sxs-lookup"><span data-stu-id="e74fd-121">Use the application ID and key to acquire a token to the `"https://api.timeseries.azure.com/"` audience or resource.</span></span> <span data-ttu-id="e74fd-122">Token kan sedan användas för att anropa tid serien Insights API.</span><span class="sxs-lookup"><span data-stu-id="e74fd-122">The token can then be used to call the Time Series Insights API.</span></span>

<span data-ttu-id="e74fd-123">Här följer detaljerade anvisningar:</span><span class="sxs-lookup"><span data-stu-id="e74fd-123">Here are the detailed steps:</span></span>

1. <span data-ttu-id="e74fd-124">Välj i Azure-portalen **Azure Active Directory** > **App registreringar** > **nya appregistrering**.</span><span class="sxs-lookup"><span data-stu-id="e74fd-124">In the Azure portal, select **Azure Active Directory** > **App registrations** > **New application registration**.</span></span>

   ![Ny programmet registrering i Azure Active Directory](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. <span data-ttu-id="e74fd-126">Namnge programmet, Välj den typ som ska vara **webbapp / API**, Välj en giltig URI för **inloggnings-URL**, och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="e74fd-126">Give the application a name, select the type to be **Web app / API**, select any valid URI for **Sign-on URL**, and click **Create**.</span></span>

   ![Skapa programmet i Azure Active Directory](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. <span data-ttu-id="e74fd-128">Välj ditt nya program och kopiera dess program-ID till valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="e74fd-128">Select your newly created application and copy its application ID to your favorite text editor.</span></span>

   ![Kopiera program-ID](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. <span data-ttu-id="e74fd-130">Välj **nycklar**, ange nyckelnamnet, Välj upphör att gälla och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="e74fd-130">Select **Keys**, enter the key name, select the expiration, and click **Save**.</span></span>

   ![Välj programnycklar](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Ange namn och upphör att gälla och klicka på Spara](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. <span data-ttu-id="e74fd-133">Kopiera nyckeln till valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="e74fd-133">Copy the key to your favorite text editor.</span></span>

   ![Kopiera nyckeln för programmet](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. <span data-ttu-id="e74fd-135">Tid serien insikter miljön, Välj **principerna dataåtkomst** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e74fd-135">For the Time Series Insights environment, select **Data Access Policies** and click **Add**.</span></span>

   ![Lägga till nya data åtkomstprincipen i tid serien insikter-miljön](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. <span data-ttu-id="e74fd-137">I den **Välj användare** dialogrutan, klistra in namnet på programmet (från steg 2) eller program-ID (från steg 3).</span><span class="sxs-lookup"><span data-stu-id="e74fd-137">In the **Select User** dialog box, paste the application name (from step 2) or application ID (from step 3).</span></span>

   ![Hitta ett program i dialogrutan Välj användare](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. <span data-ttu-id="e74fd-139">Välj rollen (**Reader** för datafrågor, **deltagare** för datafrågor och ändra referensdata) och klicka på **Ok**.</span><span class="sxs-lookup"><span data-stu-id="e74fd-139">Select the role (**Reader** for querying data, **Contributor** for querying data and changing reference data) and click **Ok**.</span></span>

   ![Välj läsaren eller deltagare i dialogrutan Välj roll](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. <span data-ttu-id="e74fd-141">Spara principen genom att klicka på **Ok**.</span><span class="sxs-lookup"><span data-stu-id="e74fd-141">Save the policy by clicking **Ok**.</span></span>

10. <span data-ttu-id="e74fd-142">Använd program-ID (från steg 3) och tangent (från steg 5) för att hämta token för programmet.</span><span class="sxs-lookup"><span data-stu-id="e74fd-142">Use the application ID (from step 3) and application key (from step 5) to acquire the token on behalf of the application.</span></span> <span data-ttu-id="e74fd-143">Token kan sedan skickas i den `Authorization` huvud när programmet anropar tid serien Insights API.</span><span class="sxs-lookup"><span data-stu-id="e74fd-143">The token can then be passed in the `Authorization` header when the application calls the Time Series Insights API.</span></span>

    <span data-ttu-id="e74fd-144">Om du använder C#, kan du använda följande kod för att hämta token för programmet.</span><span class="sxs-lookup"><span data-stu-id="e74fd-144">If you're using C#, you can use the following code to acquire the token on behalf of the application.</span></span> <span data-ttu-id="e74fd-145">Ett komplett exempel finns [fråga data med hjälp av C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="e74fd-145">For a complete sample, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

    ```csharp
    var authenticationContext = new AuthenticationContext(
        "https://login.microsoftonline.com/common",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set the resource URI to the Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/", 
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

## <a name="next-steps"></a><span data-ttu-id="e74fd-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e74fd-146">Next steps</span></span>

<span data-ttu-id="e74fd-147">Använda program-ID och nyckel i ditt program.</span><span class="sxs-lookup"><span data-stu-id="e74fd-147">Use the application ID and key in your application.</span></span> <span data-ttu-id="e74fd-148">Exempelkod som anropar tid serien Insights API finns [fråga data med hjälp av C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="e74fd-148">For sample code that calls the Time Series Insights API, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="e74fd-149">Se även</span><span class="sxs-lookup"><span data-stu-id="e74fd-149">See also</span></span>

* <span data-ttu-id="e74fd-150">[Frågor API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) fullständig fråge-API-referens</span><span class="sxs-lookup"><span data-stu-id="e74fd-150">[Query API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) for the full Query API reference</span></span>
* [<span data-ttu-id="e74fd-151">Skapa ett huvudnamn för tjänsten i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e74fd-151">Create a service principal in the Azure portal</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
