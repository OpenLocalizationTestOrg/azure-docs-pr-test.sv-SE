---
title: "aaaSet en anpassad hemsida för publicerade appar med hjälp av Azure AD Application Proxy | Microsoft Docs"
description: Beskriver hello grunderna om Azure AD Application Proxy-kopplingar
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
ms.openlocfilehash: 5bb695e904d285c3b440520f107c7bf63ba5cac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a><span data-ttu-id="2e1ff-103">Ange en anpassad hemsida för publicerade appar med hjälp av Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="2e1ff-103">Set a custom home page for published apps by using Azure AD Application Proxy</span></span>

<span data-ttu-id="2e1ff-104">Den här artikeln beskrivs hur tooconfigure appar toodirect användare tooa anpassad startsida.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-104">This article discusses how tooconfigure apps toodirect users tooa custom home page.</span></span> <span data-ttu-id="2e1ff-105">När du publicerar ett program med Application Proxy kan du ställa in en intern URL men ibland som är inte hello sidan som användarna ska se först.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-105">When you publish an application with Application Proxy, you set an internal URL but sometimes that's not hello page your users should see first.</span></span> <span data-ttu-id="2e1ff-106">Ange en anpassad hemsida så att användarna gå toohello höger sida när de kommer åt hello appar från hello Azure Active Directory-åtkomstpanelen eller hello Office 365 app starta.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-106">Set a custom home page so that your users go toohello right page when they access hello apps from hello Azure Active Directory Access Panel or hello Office 365 app launcher.</span></span>

<span data-ttu-id="2e1ff-107">När användarna startar hello app, är de riktade som standard toohello rot domän-URL för hello publicerade app.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-107">When users launch hello app, they're directed by default toohello root domain URL for hello published app.</span></span> <span data-ttu-id="2e1ff-108">hello landningssida anges vanligtvis som hello URL-Adressen.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-108">hello landing page is typically set as hello home page URL.</span></span> <span data-ttu-id="2e1ff-109">Använd hello Azure AD PowerShell-modulen toodefine anpassad startsida webbadresser när du vill app användare tooland på en viss sida i hello app.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-109">Use hello Azure AD PowerShell module toodefine custom home page URLs when you want app users tooland on a specific page within hello app.</span></span> 

<span data-ttu-id="2e1ff-110">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2e1ff-110">For example:</span></span>
- <span data-ttu-id="2e1ff-111">I företagsnätverket, användarna gå för*https://ExpenseApp/login/login.aspx* toosign i och åtkomst till din app.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-111">Inside your corporate network, users go too*https://ExpenseApp/login/login.aspx* toosign in and access your app.</span></span>
- <span data-ttu-id="2e1ff-112">Eftersom du har andra tillgångar som bilder att Application Proxy måste tooaccess toppnivå hello för hello mappstrukturen du publicerar hello app med *https://ExpenseApp* som hello Intern URL.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-112">Because you have other assets like images that Application Proxy needs tooaccess at hello top level of hello folder structure, you publish hello app with *https://ExpenseApp* as hello internal URL.</span></span>
- <span data-ttu-id="2e1ff-113">Hej standard externa URL: en är *https://ExpenseApp-contoso.msappproxy.net*, som tar inte användare toohello inloggningen på sidan.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-113">hello default external URL is *https://ExpenseApp-contoso.msappproxy.net*, which doesn't take your users toohello sign in page.</span></span>  
- <span data-ttu-id="2e1ff-114">Ange *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* som hello startsidan URL toogive användarna smidigt.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-114">Set *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* as hello home page URL toogive your users a seamless experience.</span></span> 

>[!NOTE]
><span data-ttu-id="2e1ff-115">När du ge användare åtkomst toopublished appar hello appar visas i hello [Azure AD-åtkomstpanelen](active-directory-saas-access-panel-introduction.md) och hello [Office 365 app starta](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span><span class="sxs-lookup"><span data-stu-id="2e1ff-115">When you give users access toopublished apps, hello apps are displayed in hello [Azure AD Access Panel](active-directory-saas-access-panel-introduction.md) and hello [Office 365 app launcher](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="2e1ff-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="2e1ff-116">Before you start</span></span>

<span data-ttu-id="2e1ff-117">Innan du kan ange hello URL-Adressen måste ha i åtanke hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="2e1ff-117">Before you set hello home page URL, keep in mind hello following requirements:</span></span>

* <span data-ttu-id="2e1ff-118">Se till att hello sökvägen som du anger är en underdomän sökväg av hello rot domän-URL.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-118">Ensure that hello path you specify is a subdomain path of hello root domain URL.</span></span>

  <span data-ttu-id="2e1ff-119">Om hello rotdomänen Webbadress är, till exempel https://apps.contoso.com/app1/, hello URL-Adressen som du konfigurerar måste börja med https://apps.contoso.com/app1/.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-119">If hello root-domain URL is, for example, https://apps.contoso.com/app1/, hello home page URL that you configure must start with https://apps.contoso.com/app1/.</span></span>

* <span data-ttu-id="2e1ff-120">Om du ändrar toohello publicerade app, hello ändring kan återställa hello värdet för hello URL-Adressen.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-120">If you make a change toohello published app, hello change might reset hello value of hello home page URL.</span></span> <span data-ttu-id="2e1ff-121">När du uppdaterar hello app i hello framtida bör du kontrollera och, om det behövs, uppdatera hello URL-Adressen.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-121">When you update hello app in hello future, you should recheck and, if necessary, update hello home page URL.</span></span>

## <a name="change-hello-home-page-in-hello-azure-portal"></a><span data-ttu-id="2e1ff-122">Ändra hello startsida i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2e1ff-122">Change hello home page in hello Azure portal</span></span>

1. <span data-ttu-id="2e1ff-123">Logga in toohello [Azure-portalen](https://portal.azure.com) som administratör.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-123">Sign in toohello [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="2e1ff-124">Navigera för**Azure Active Directory** > **App registreringar** och välja programmet hello-listan.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-124">Navigate too**Azure Active Directory** > **App registrations** and choose your application from hello list.</span></span> 
3. <span data-ttu-id="2e1ff-125">Välj **egenskaper** från hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-125">Select **Properties** from hello settings.</span></span>
4. <span data-ttu-id="2e1ff-126">Uppdatera hello **URL-Adressen** med den nya sökvägen.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-126">Update hello **Home page URL** field with your new path.</span></span> 

   ![Ange ny URL-Adressen](./media/application-proxy-office365-app-launcher/homepage.png)

5. <span data-ttu-id="2e1ff-128">Välj **spara**</span><span class="sxs-lookup"><span data-stu-id="2e1ff-128">Select **Save**</span></span>

## <a name="change-hello-home-page-with-powershell"></a><span data-ttu-id="2e1ff-129">Ändra hello startsida med PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e1ff-129">Change hello home page with PowerShell</span></span>

### <a name="install-hello-azure-ad-powershell-module"></a><span data-ttu-id="2e1ff-130">Installera hello Azure AD PowerShell-modulen</span><span class="sxs-lookup"><span data-stu-id="2e1ff-130">Install hello Azure AD PowerShell module</span></span>

<span data-ttu-id="2e1ff-131">Innan du definierar en anpassad URL-Adressen med hjälp av PowerShell installera hello Azure AD PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-131">Before you define a custom home page URL by using PowerShell, install hello Azure AD PowerShell module.</span></span> <span data-ttu-id="2e1ff-132">Du kan hämta hello paketet från hello [PowerShell-galleriet](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), som använder hello Graph API-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-132">You can download hello package from hello [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), which uses hello Graph API endpoint.</span></span> 

<span data-ttu-id="2e1ff-133">tooinstall hello paketet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="2e1ff-133">tooinstall hello package, follow these steps:</span></span>

1. <span data-ttu-id="2e1ff-134">Öppna ett PowerShell-fönster som standard och kör sedan följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="2e1ff-134">Open a standard PowerShell window, and then run hello following command:</span></span>

    ```
     Install-Module -Name AzureAD
    ```
    <span data-ttu-id="2e1ff-135">Om du använder kommandot hello som icke-administratörer kan använda hello `-scope currentuser` alternativet.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-135">If you're running hello command as a non-admin, use hello `-scope currentuser` option.</span></span>
2. <span data-ttu-id="2e1ff-136">Under installationen av hello väljer **Y** tooinstall två paket från Nuget.org. Båda paketen krävs.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-136">During hello installation, select **Y** tooinstall two packages from Nuget.org. Both packages are required.</span></span> 

### <a name="find-hello-objectid-of-hello-app"></a><span data-ttu-id="2e1ff-137">Hitta hello ObjectID hello-appen</span><span class="sxs-lookup"><span data-stu-id="2e1ff-137">Find hello ObjectID of hello app</span></span>

<span data-ttu-id="2e1ff-138">Hämta hello ObjectID hello-appen och sök sedan efter hello app av dess hemsida.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-138">Obtain hello ObjectID of hello app, and then search for hello app by its home page.</span></span>

1. <span data-ttu-id="2e1ff-139">Öppna PowerShell och importera hello Azure AD-modulen.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-139">Open PowerShell and import hello Azure AD module.</span></span>

    ```
    Import-Module AzureAD
    ```

2. <span data-ttu-id="2e1ff-140">Logga in toohello Azure AD-modulen som hello Innehavaradministratör.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-140">Sign in toohello Azure AD module as hello tenant administrator.</span></span>

    ```
    Connect-AzureAD
    ```
3. <span data-ttu-id="2e1ff-141">Hitta hello app baserat på dess URL-Adressen.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-141">Find hello app based on its home page URL.</span></span> <span data-ttu-id="2e1ff-142">Du kan hitta hello URL i hello portal genom att gå för**Azure Active Directory** > **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-142">You can find hello URL in hello portal by going too**Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span> <span data-ttu-id="2e1ff-143">Det här exemplet används *sharepoint iddemo*.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-143">This example uses *sharepoint-iddemo*.</span></span>

    ```
    Get-AzureADApplication | where { $_.Homepage -like “sharepoint-iddemo” } | fl DisplayName, Homepage, ObjectID
    ```
4. <span data-ttu-id="2e1ff-144">Du bör få ett resultat som är liknande toohello som visas här.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-144">You should get a result that's similar toohello one shown here.</span></span> <span data-ttu-id="2e1ff-145">Kopiera hello ObjectID GUID toouse i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-145">Copy hello ObjectID GUID toouse in hello next section.</span></span>

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-hello-home-page-url"></a><span data-ttu-id="2e1ff-146">Uppdatera hello URL-Adressen</span><span class="sxs-lookup"><span data-stu-id="2e1ff-146">Update hello home page URL</span></span>

<span data-ttu-id="2e1ff-147">I hello utföra samma PowerShell-modul som du använde för steg 1, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2e1ff-147">In hello same PowerShell module that you used for step 1, perform hello following steps:</span></span>

1. <span data-ttu-id="2e1ff-148">Bekräfta att du har hello korrigera app och Ersätt *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* med hello ObjectID som du kopierade i föregående steg hello.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-148">Confirm that you have hello correct app, and replace *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* with hello ObjectID that you copied in hello preceding step.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 <span data-ttu-id="2e1ff-149">Nu när du har bekräftat hello app, är du redo tooupdate hello startsidan på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-149">Now that you've confirmed hello app, you're ready tooupdate hello home page, as follows.</span></span>

2. <span data-ttu-id="2e1ff-150">Skapa en tom programmet objektet toohold hello toomake önskade ändringar.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-150">Create a blank application object toohold hello changes that you want toomake.</span></span> <span data-ttu-id="2e1ff-151">Den här variabeln innehåller hello värden som du vill tooupdate.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-151">This variable holds hello values that you want tooupdate.</span></span> <span data-ttu-id="2e1ff-152">Inget har skapats i det här steget.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-152">Nothing is created in this step.</span></span>

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. <span data-ttu-id="2e1ff-153">Ange hello startsidan URL toohello-värdet som du vill.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-153">Set hello home page URL toohello value that you want.</span></span> <span data-ttu-id="2e1ff-154">hello-värdet måste vara en underdomän sökvägen hello publicerade appen.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-154">hello value must be a subdomain path of hello published app.</span></span> <span data-ttu-id="2e1ff-155">Till exempel om du ändrar hello URL-Adressen från *https://sharepoint-iddemo.msappproxy.net/* för*https://sharepoint-iddemo.msappproxy.net/hybrid/*, appanvändare gå direkt toohello anpassad startsidan.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-155">For example, if you change hello home page URL from *https://sharepoint-iddemo.msappproxy.net/* too*https://sharepoint-iddemo.msappproxy.net/hybrid/*, app users go directly toohello custom home page.</span></span>

    ```
    $homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. <span data-ttu-id="2e1ff-156">Se hello uppdateringen med hjälp av hello GUID (objekt-ID) som du kopierade i ”steg 1: hitta hello ObjectID hello-appen”.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-156">Make hello update by using hello GUID (ObjectID) that you copied in "Step 1: Find hello ObjectID of hello app."</span></span>

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. <span data-ttu-id="2e1ff-157">tooconfirm att ändra hello lyckades, starta om hello app.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-157">tooconfirm that hello change was successful, restart hello app.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
><span data-ttu-id="2e1ff-158">Alla ändringar du gör toohello app kan återställa hello URL-Adressen.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-158">Any changes that you make toohello app might reset hello home page URL.</span></span> <span data-ttu-id="2e1ff-159">Om din startsida URL återställer upprepar du steg 2.</span><span class="sxs-lookup"><span data-stu-id="2e1ff-159">If your home page URL resets, repeat step 2.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e1ff-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2e1ff-160">Next steps</span></span>

- [<span data-ttu-id="2e1ff-161">Aktivera fjärråtkomst tooSharePoint med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="2e1ff-161">Enable remote access tooSharePoint with Azure AD Application Proxy</span></span>](application-proxy-enable-remote-access-sharepoint.md)
- [<span data-ttu-id="2e1ff-162">Aktivera Application Proxy i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2e1ff-162">Enable Application Proxy in hello Azure portal</span></span>](active-directory-application-proxy-enable.md)
