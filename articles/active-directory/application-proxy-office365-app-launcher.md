---
title: "Ange en anpassad hemsida för publicerade appar med hjälp av Azure AD Application Proxy | Microsoft Docs"
description: Beskriver grunderna om Azure AD Application Proxy-kopplingar
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 9069166259265f5d2b43043b75039e239f397f6c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a><span data-ttu-id="9be10-103">Ange en anpassad hemsida för publicerade appar med hjälp av Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="9be10-103">Set a custom home page for published apps by using Azure AD Application Proxy</span></span>

<span data-ttu-id="9be10-104">Den här artikeln beskrivs hur du konfigurerar appar för att dirigera användare till en anpassad startsida.</span><span class="sxs-lookup"><span data-stu-id="9be10-104">This article discusses how to configure apps to direct users to a custom home page.</span></span> <span data-ttu-id="9be10-105">När du publicerar ett program med Application Proxy kan ange du en intern URL men ibland som inte är sidan användarna ska se först.</span><span class="sxs-lookup"><span data-stu-id="9be10-105">When you publish an application with Application Proxy, you set an internal URL but sometimes that's not the page your users should see first.</span></span> <span data-ttu-id="9be10-106">Ange en anpassad hemsida så att dina användare går du till höger sida när de kommer åt appar från åtkomstpanelen Azure Active Directory eller Office 365 app starta.</span><span class="sxs-lookup"><span data-stu-id="9be10-106">Set a custom home page so that your users go to the right page when they access the apps from the Azure Active Directory Access Panel or the Office 365 app launcher.</span></span>

<span data-ttu-id="9be10-107">När användarna startar appen, är de riktade som standard till rot-URL för den publicerade app för domänen.</span><span class="sxs-lookup"><span data-stu-id="9be10-107">When users launch the app, they're directed by default to the root domain URL for the published app.</span></span> <span data-ttu-id="9be10-108">Landningssida anges vanligtvis som URL för startsidan.</span><span class="sxs-lookup"><span data-stu-id="9be10-108">The landing page is typically set as the home page URL.</span></span> <span data-ttu-id="9be10-109">Använda Azure AD PowerShell-modulen för att definiera anpassade startsidan URL: er när du vill att användarna ska hamna på en viss sida i appen.</span><span class="sxs-lookup"><span data-stu-id="9be10-109">Use the Azure AD PowerShell module to define custom home page URLs when you want app users to land on a specific page within the app.</span></span> 

<span data-ttu-id="9be10-110">Exempel:</span><span class="sxs-lookup"><span data-stu-id="9be10-110">For example:</span></span>
- <span data-ttu-id="9be10-111">I företagsnätverket, användare går du till *https://ExpenseApp/login/login.aspx* att logga in och komma åt din app.</span><span class="sxs-lookup"><span data-stu-id="9be10-111">Inside your corporate network, users go to *https://ExpenseApp/login/login.aspx* to sign in and access your app.</span></span>
- <span data-ttu-id="9be10-112">Eftersom du har andra tillgångar som avbildningar som Application Proxy behöver åtkomst till på den översta nivån av mappstrukturen kan du publicera appen med *https://ExpenseApp* som intern URL.</span><span class="sxs-lookup"><span data-stu-id="9be10-112">Because you have other assets like images that Application Proxy needs to access at the top level of the folder structure, you publish the app with *https://ExpenseApp* as the internal URL.</span></span>
- <span data-ttu-id="9be10-113">Standard-externa URL: en är *https://ExpenseApp-contoso.msappproxy.net*, som tar inte användarna att tecknet på sidan.</span><span class="sxs-lookup"><span data-stu-id="9be10-113">The default external URL is *https://ExpenseApp-contoso.msappproxy.net*, which doesn't take your users to the sign in page.</span></span>  
- <span data-ttu-id="9be10-114">Ange *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* som URL till startsidan för att ge användarna en integrerad upplevelse.</span><span class="sxs-lookup"><span data-stu-id="9be10-114">Set *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* as the home page URL to give your users a seamless experience.</span></span> 

>[!NOTE]
><span data-ttu-id="9be10-115">När du ger användare åtkomst till publicerade appar appar visas i den [Azure AD-åtkomstpanelen](active-directory-saas-access-panel-introduction.md) och [Office 365 app starta](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span><span class="sxs-lookup"><span data-stu-id="9be10-115">When you give users access to published apps, the apps are displayed in the [Azure AD Access Panel](active-directory-saas-access-panel-introduction.md) and the [Office 365 app launcher](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="9be10-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="9be10-116">Before you start</span></span>

<span data-ttu-id="9be10-117">Innan du anger URL till startsidan, Tänk på följande krav:</span><span class="sxs-lookup"><span data-stu-id="9be10-117">Before you set the home page URL, keep in mind the following requirements:</span></span>

* <span data-ttu-id="9be10-118">Kontrollera att den angivna sökvägen är en underdomän sökväg av domän rot-URL.</span><span class="sxs-lookup"><span data-stu-id="9be10-118">Ensure that the path you specify is a subdomain path of the root domain URL.</span></span>

  <span data-ttu-id="9be10-119">Om rotdomänen URL: en är till exempel https://apps.contoso.com/app1/, den URL-Adressen som du konfigurerar måste börja med https://apps.contoso.com/app1/.</span><span class="sxs-lookup"><span data-stu-id="9be10-119">If the root-domain URL is, for example, https://apps.contoso.com/app1/, the home page URL that you configure must start with https://apps.contoso.com/app1/.</span></span>

* <span data-ttu-id="9be10-120">Om du gör en ändring i appen publicerade kan ändringen återställa värdet för URL till startsidan.</span><span class="sxs-lookup"><span data-stu-id="9be10-120">If you make a change to the published app, the change might reset the value of the home page URL.</span></span> <span data-ttu-id="9be10-121">När du uppdaterar appen i framtiden, bör du kontrollera och, om det behövs, uppdatera URL till startsidan.</span><span class="sxs-lookup"><span data-stu-id="9be10-121">When you update the app in the future, you should recheck and, if necessary, update the home page URL.</span></span>

## <a name="change-the-home-page-in-the-azure-portal"></a><span data-ttu-id="9be10-122">Ändra startsidan i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9be10-122">Change the home page in the Azure portal</span></span>

1. <span data-ttu-id="9be10-123">Logga in på [Azure Portal](https://portal.azure.com) som administratör.</span><span class="sxs-lookup"><span data-stu-id="9be10-123">Sign in to the [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="9be10-124">Gå till **Azure Active Directory** > **App registreringar** och välj ditt program i listan.</span><span class="sxs-lookup"><span data-stu-id="9be10-124">Navigate to **Azure Active Directory** > **App registrations** and choose your application from the list.</span></span> 
3. <span data-ttu-id="9be10-125">Välj **egenskaper** från inställningarna.</span><span class="sxs-lookup"><span data-stu-id="9be10-125">Select **Properties** from the settings.</span></span>
4. <span data-ttu-id="9be10-126">Uppdatering av **URL-Adressen** med den nya sökvägen.</span><span class="sxs-lookup"><span data-stu-id="9be10-126">Update the **Home page URL** field with your new path.</span></span> 

   ![Ange ny URL-Adressen](./media/application-proxy-office365-app-launcher/homepage.png)

5. <span data-ttu-id="9be10-128">Välj **spara**</span><span class="sxs-lookup"><span data-stu-id="9be10-128">Select **Save**</span></span>

## <a name="change-the-home-page-with-powershell"></a><span data-ttu-id="9be10-129">Ändra startsidan med PowerShell</span><span class="sxs-lookup"><span data-stu-id="9be10-129">Change the home page with PowerShell</span></span>

### <a name="install-the-azure-ad-powershell-module"></a><span data-ttu-id="9be10-130">Installera Azure AD PowerShell-modulen</span><span class="sxs-lookup"><span data-stu-id="9be10-130">Install the Azure AD PowerShell module</span></span>

<span data-ttu-id="9be10-131">Installera Azure AD PowerShell-modulen innan du definierar en anpassad URL-Adressen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9be10-131">Before you define a custom home page URL by using PowerShell, install the Azure AD PowerShell module.</span></span> <span data-ttu-id="9be10-132">Du kan hämta paketet från den [PowerShell-galleriet](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), som använder Graph API-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="9be10-132">You can download the package from the [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), which uses the Graph API endpoint.</span></span> 

<span data-ttu-id="9be10-133">Följ dessa steg för att installera paketet:</span><span class="sxs-lookup"><span data-stu-id="9be10-133">To install the package, follow these steps:</span></span>

1. <span data-ttu-id="9be10-134">Öppna ett PowerShell-fönster som standard och kör sedan följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9be10-134">Open a standard PowerShell window, and then run the following command:</span></span>

    ```
     Install-Module -Name AzureAD
    ```
    <span data-ttu-id="9be10-135">Om du använder kommandot som icke-administratörer kan använda den `-scope currentuser` alternativet.</span><span class="sxs-lookup"><span data-stu-id="9be10-135">If you're running the command as a non-admin, use the `-scope currentuser` option.</span></span>
2. <span data-ttu-id="9be10-136">Under installationen, Välj **Y** att installera två paket från Nuget.org. Båda paketen krävs.</span><span class="sxs-lookup"><span data-stu-id="9be10-136">During the installation, select **Y** to install two packages from Nuget.org. Both packages are required.</span></span> 

### <a name="find-the-objectid-of-the-app"></a><span data-ttu-id="9be10-137">Sök efter objekt-ID för appen</span><span class="sxs-lookup"><span data-stu-id="9be10-137">Find the ObjectID of the app</span></span>

<span data-ttu-id="9be10-138">Hämta ObjectID för appen och sök sedan efter appen genom att dess hemsida.</span><span class="sxs-lookup"><span data-stu-id="9be10-138">Obtain the ObjectID of the app, and then search for the app by its home page.</span></span>

1. <span data-ttu-id="9be10-139">Öppna PowerShell och importera Azure AD-modulen.</span><span class="sxs-lookup"><span data-stu-id="9be10-139">Open PowerShell and import the Azure AD module.</span></span>

    ```
    Import-Module AzureAD
    ```

2. <span data-ttu-id="9be10-140">Logga in på Azure AD-modulen som Innehavaradministratör.</span><span class="sxs-lookup"><span data-stu-id="9be10-140">Sign in to the Azure AD module as the tenant administrator.</span></span>

    ```
    Connect-AzureAD
    ```
3. <span data-ttu-id="9be10-141">Hitta appen baserat på dess URL-Adressen.</span><span class="sxs-lookup"><span data-stu-id="9be10-141">Find the app based on its home page URL.</span></span> <span data-ttu-id="9be10-142">Du hittar Webbadressen i portalen genom att gå till **Azure Active Directory** > **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9be10-142">You can find the URL in the portal by going to **Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span> <span data-ttu-id="9be10-143">Det här exemplet används *sharepoint iddemo*.</span><span class="sxs-lookup"><span data-stu-id="9be10-143">This example uses *sharepoint-iddemo*.</span></span>

    ```
    Get-AzureADApplication | where { $_.Homepage -like “sharepoint-iddemo” } | fl DisplayName, Homepage, ObjectID
    ```
4. <span data-ttu-id="9be10-144">Du bör få ett resultat som liknar den som visas här.</span><span class="sxs-lookup"><span data-stu-id="9be10-144">You should get a result that's similar to the one shown here.</span></span> <span data-ttu-id="9be10-145">Kopiera ObjectID GUID för användning i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9be10-145">Copy the ObjectID GUID to use in the next section.</span></span>

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-the-home-page-url"></a><span data-ttu-id="9be10-146">Uppdatera URL till startsidan</span><span class="sxs-lookup"><span data-stu-id="9be10-146">Update the home page URL</span></span>

<span data-ttu-id="9be10-147">Utför följande steg i samma PowerShell-modulen som du använde för steg 1:</span><span class="sxs-lookup"><span data-stu-id="9be10-147">In the same PowerShell module that you used for step 1, perform the following steps:</span></span>

1. <span data-ttu-id="9be10-148">Bekräfta att du har rätt appen och Ersätt *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* med objekt-ID som du kopierade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="9be10-148">Confirm that you have the correct app, and replace *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* with the ObjectID that you copied in the preceding step.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 <span data-ttu-id="9be10-149">Nu när du har bekräftat appen, är du redo att uppdatera sidan, enligt följande.</span><span class="sxs-lookup"><span data-stu-id="9be10-149">Now that you've confirmed the app, you're ready to update the home page, as follows.</span></span>

2. <span data-ttu-id="9be10-150">Skapa en tom programobjektet för att lagra de ändringar som du vill göra.</span><span class="sxs-lookup"><span data-stu-id="9be10-150">Create a blank application object to hold the changes that you want to make.</span></span> <span data-ttu-id="9be10-151">Den här variabeln innehåller de värden som du vill uppdatera.</span><span class="sxs-lookup"><span data-stu-id="9be10-151">This variable holds the values that you want to update.</span></span> <span data-ttu-id="9be10-152">Inget har skapats i det här steget.</span><span class="sxs-lookup"><span data-stu-id="9be10-152">Nothing is created in this step.</span></span>

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. <span data-ttu-id="9be10-153">Ange den URL-Adressen till det värde som du vill.</span><span class="sxs-lookup"><span data-stu-id="9be10-153">Set the home page URL to the value that you want.</span></span> <span data-ttu-id="9be10-154">Värdet måste vara en underdomän sökvägen till publicerade appen.</span><span class="sxs-lookup"><span data-stu-id="9be10-154">The value must be a subdomain path of the published app.</span></span> <span data-ttu-id="9be10-155">Till exempel om du ändrar den URL-Adressen från *https://sharepoint-iddemo.msappproxy.net/* till *https://sharepoint-iddemo.msappproxy.net/hybrid/*, appanvändare gå direkt till sidan med anpassade.</span><span class="sxs-lookup"><span data-stu-id="9be10-155">For example, if you change the home page URL from *https://sharepoint-iddemo.msappproxy.net/* to *https://sharepoint-iddemo.msappproxy.net/hybrid/*, app users go directly to the custom home page.</span></span>

    ```
    $homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. <span data-ttu-id="9be10-156">Gör uppdateringen med hjälp av GUID (objekt-ID) som du kopierade i ”steg 1: hitta ObjectID för appen”.</span><span class="sxs-lookup"><span data-stu-id="9be10-156">Make the update by using the GUID (ObjectID) that you copied in "Step 1: Find the ObjectID of the app."</span></span>

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. <span data-ttu-id="9be10-157">Starta om appen för att bekräfta att ändringen har lyckats.</span><span class="sxs-lookup"><span data-stu-id="9be10-157">To confirm that the change was successful, restart the app.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
><span data-ttu-id="9be10-158">Alla ändringar du gör i appen kan återställa URL till startsidan.</span><span class="sxs-lookup"><span data-stu-id="9be10-158">Any changes that you make to the app might reset the home page URL.</span></span> <span data-ttu-id="9be10-159">Om din startsida URL återställer upprepar du steg 2.</span><span class="sxs-lookup"><span data-stu-id="9be10-159">If your home page URL resets, repeat step 2.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9be10-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9be10-160">Next steps</span></span>

- [<span data-ttu-id="9be10-161">Aktivera fjärråtkomst och SharePoint med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="9be10-161">Enable remote access to SharePoint with Azure AD Application Proxy</span></span>](application-proxy-enable-remote-access-sharepoint.md)
- [<span data-ttu-id="9be10-162">Aktivera Application Proxy på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9be10-162">Enable Application Proxy in the Azure portal</span></span>](active-directory-application-proxy-enable.md)
