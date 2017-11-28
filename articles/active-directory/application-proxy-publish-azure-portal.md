---
title: aaaPublish appar med Azure AD Application Proxy | Microsoft Docs
description: Publicera lokala program toohello moln med Azure AD Application Proxy i hello Azure-portalen.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ed5458467fb7d4376f65a222f1ba5f23cfdfc57c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a><span data-ttu-id="1bd63-103">Publicera program med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="1bd63-103">Publish applications using Azure AD Application Proxy</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1bd63-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1bd63-104">Azure portal</span></span>](application-proxy-publish-azure-portal.md)
> * [<span data-ttu-id="1bd63-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="1bd63-105">Azure classic portal</span></span>](active-directory-application-proxy-publish.md)

<span data-ttu-id="1bd63-106">Azure Active Directory (AD) Application Proxy hjälper dig att stöd för fjärranvändare genom att publicera lokala program toobe nås via hello internet.</span><span class="sxs-lookup"><span data-stu-id="1bd63-106">Azure Active Directory (AD) Application Proxy helps you support remote workers by publishing on-premises applications toobe accessed over hello internet.</span></span> <span data-ttu-id="1bd63-107">Du kan publicera programmen via hello Azure portal tooprovide säker fjärråtkomst utanför nätverket.</span><span class="sxs-lookup"><span data-stu-id="1bd63-107">You can publish these applications through hello Azure portal tooprovide secure remote access from outside your network.</span></span>

<span data-ttu-id="1bd63-108">Den här artikeln vägleder dig genom hello steg toopublish ett lokalt program med Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="1bd63-108">This article walks you through hello steps toopublish an on-premises app with Application Proxy.</span></span> <span data-ttu-id="1bd63-109">När du har slutfört den här artikeln användarna kommer att kunna tooaccess appen via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="1bd63-109">After you complete this article, your users will be able tooaccess your app remotely.</span></span> <span data-ttu-id="1bd63-110">Och du kan klara tooconfigure extra funktioner för programmet hello som enkel inloggning, personlig information och säkerhetskrav.</span><span class="sxs-lookup"><span data-stu-id="1bd63-110">And you'll be ready tooconfigure extra features for hello application like single sign-on, personalized information, and security requirements.</span></span>

<span data-ttu-id="1bd63-111">Om du är ny tooApplication Proxy, mer information om den här funktionen med hello artikel [hur säkra tooprovide fjärråtkomst tooon lokala program](active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1bd63-111">If you're new tooApplication Proxy, learn more about this feature with hello article [How tooprovide secure remote access tooon-premises applications](active-directory-application-proxy-get-started.md).</span></span>


## <a name="publish-an-on-premises-app-for-remote-access"></a><span data-ttu-id="1bd63-112">Publicera en lokal app för fjärråtkomst</span><span class="sxs-lookup"><span data-stu-id="1bd63-112">Publish an on-premises app for remote access</span></span>

<span data-ttu-id="1bd63-113">Följ dessa steg toopublish dina appar med Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="1bd63-113">Follow these steps toopublish your apps with Application Proxy.</span></span> <span data-ttu-id="1bd63-114">Om du inte har redan hämtats och konfigurerat en koppling för din organisation, gå för[komma igång med Application Proxy och installera hello connector](active-directory-application-proxy-enable.md) först och sedan publicera din app.</span><span class="sxs-lookup"><span data-stu-id="1bd63-114">If you haven't already downloaded and configured a connector for your organization, go too[Get started with Application Proxy and install hello connector](active-directory-application-proxy-enable.md) first, and then publish your app.</span></span>

> [!TIP]
> <span data-ttu-id="1bd63-115">Om du testar programproxy för hello första gången, väljer du ett program som har ställts in för lösenordsbaserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="1bd63-115">If you're testing out Application Proxy for hello first time, choose an application that's set up for password-based authentication.</span></span> <span data-ttu-id="1bd63-116">Application Proxy stöder andra typer av autentisering, men lösenordsbaserade appar är hello enklaste tooget dig snabbt och köra.</span><span class="sxs-lookup"><span data-stu-id="1bd63-116">Application Proxy supports other types of authentication, but password-based apps are hello easiest tooget up and running quickly.</span></span> 

1. <span data-ttu-id="1bd63-117">Logga in som administratör i hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1bd63-117">Sign in as an administrator in hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="1bd63-118">Välj **Azure Active Directory** > **företagsprogram** > **nytt program**.</span><span class="sxs-lookup"><span data-stu-id="1bd63-118">Select **Azure Active Directory** > **Enterprise applications** > **New application**.</span></span>

  ![Lägg till ett företagsprogram](./media/application-proxy-publish-azure-portal/add-app.png)

3. <span data-ttu-id="1bd63-120">Välj **alla**och välj **lokalt program**.</span><span class="sxs-lookup"><span data-stu-id="1bd63-120">Select **All**, then select **On-premises application**.</span></span>  

  ![Lägg till ditt eget program](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. <span data-ttu-id="1bd63-122">Ange följande information om programmet hello:</span><span class="sxs-lookup"><span data-stu-id="1bd63-122">Provide hello following information about your application:</span></span>

   - <span data-ttu-id="1bd63-123">**Namnet**: hello namnet på hello-program som ska visas på hello åtkomstpanelen och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1bd63-123">**Name**: hello name of hello application that will appear on hello access panel and in hello Azure portal.</span></span> 

   - <span data-ttu-id="1bd63-124">**Intern URL**: hello URL som du använder tooaccess hello programmet från i ditt privata nätverk.</span><span class="sxs-lookup"><span data-stu-id="1bd63-124">**Internal URL**: hello URL that you use tooaccess hello application from inside your private network.</span></span> <span data-ttu-id="1bd63-125">Du kan ange en specifik sökväg på hello backend-servern toopublish medan hello resten av hello-servern är opublicerad.</span><span class="sxs-lookup"><span data-stu-id="1bd63-125">You can provide a specific path on hello backend server toopublish, while hello rest of hello server is unpublished.</span></span> <span data-ttu-id="1bd63-126">På så sätt kan du publicera olika platser på hello samma server som olika appar och ge vart och ett eget namn och regler.</span><span class="sxs-lookup"><span data-stu-id="1bd63-126">In this way, you can publish different sites on hello same server as different apps, and give each one its own name and access rules.</span></span>

     > [!TIP]
     > <span data-ttu-id="1bd63-127">Om du publicerar en sökväg, kontrollerar du att den innehåller alla nödvändiga hello-avbildningar, skript och formatmallar för ditt program.</span><span class="sxs-lookup"><span data-stu-id="1bd63-127">If you publish a path, make sure that it includes all hello necessary images, scripts, and style sheets for your application.</span></span> <span data-ttu-id="1bd63-128">Till exempel om din app på https://yourapp/app och använder avbildningar som finns på https://yourapp/media, bör du publicera https://yourapp/ som hello sökväg.</span><span class="sxs-lookup"><span data-stu-id="1bd63-128">For example, if your app is at https://yourapp/app and uses images located at https://yourapp/media, then you should publish https://yourapp/ as hello path.</span></span> <span data-ttu-id="1bd63-129">Intern URL: en har inte toobe hello landningssida användarna finns.</span><span class="sxs-lookup"><span data-stu-id="1bd63-129">This internal URL doesn't have toobe hello landing page your users see.</span></span> <span data-ttu-id="1bd63-130">Mer information finns i [ange en anpassad hemsida för publicerade appar](application-proxy-office365-app-launcher.md).</span><span class="sxs-lookup"><span data-stu-id="1bd63-130">For more information, see [Set a custom home page for published apps](application-proxy-office365-app-launcher.md).</span></span>

   - <span data-ttu-id="1bd63-131">**Extern URL**: hello adress som användarna kommer att gå tooin ordning tooaccess hello-app från utanför nätverket.</span><span class="sxs-lookup"><span data-stu-id="1bd63-131">**External URL**: hello address your users will go tooin order tooaccess hello app from outside your network.</span></span> <span data-ttu-id="1bd63-132">Om du inte vill toouse hello standarddomän Application Proxy kan läsa om [anpassade domäner i Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md).</span><span class="sxs-lookup"><span data-stu-id="1bd63-132">If you don't want toouse hello default Application Proxy domain, read about [custom domains in Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md).</span></span>
   - <span data-ttu-id="1bd63-133">**Förautentisering**: hur Application Proxy verifierar användare innan du ger dem åtkomst tooyour program.</span><span class="sxs-lookup"><span data-stu-id="1bd63-133">**Pre Authentication**: How Application Proxy verifies users before giving them access tooyour application.</span></span> 

     - <span data-ttu-id="1bd63-134">Azure Active Directory: Application Proxy dirigerar användarna toosign med Azure AD, som autentiserar deras behörigheter för hello katalog och program.</span><span class="sxs-lookup"><span data-stu-id="1bd63-134">Azure Active Directory: Application Proxy redirects users toosign in with Azure AD, which authenticates their permissions for hello directory and application.</span></span> <span data-ttu-id="1bd63-135">Vi rekommenderar detta alternativ som standard hello så att du kan dra nytta av säkerhetsfunktionerna i Azure AD som villkorlig åtkomst och Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="1bd63-135">We recommend keeping this option as hello default, so that you can take advantage of Azure AD security features like conditional access and Multi-Factor Authentication.</span></span>
     - <span data-ttu-id="1bd63-136">Genomströmning: Användarna har inte tooauthenticate mot Azure Active Directory tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="1bd63-136">Passthrough: Users don't have tooauthenticate against Azure Active Directory tooaccess hello application.</span></span> <span data-ttu-id="1bd63-137">Du kan fortfarande installera autentiseringskrav på hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="1bd63-137">You can still set up authentication requirements on hello backend.</span></span>
   - <span data-ttu-id="1bd63-138">**Kopplingen grupp**: kopplingar hello fjärråtkomst tooyour tillämpningsprogram och connector grupper kan du organisera kopplingar och appar efter region, nätverk eller syfte.</span><span class="sxs-lookup"><span data-stu-id="1bd63-138">**Connector Group**: Connectors process hello remote access tooyour application, and connector groups help you organize connectors and apps by region, network, or purpose.</span></span> <span data-ttu-id="1bd63-139">Om du inte har någon koppling grupper som skapats ännu appen tilldelas för**standard**.</span><span class="sxs-lookup"><span data-stu-id="1bd63-139">If you don't have any connector groups created yet, your app is assigned too**Default**.</span></span>

   ![Konfigurera ditt program](./media/application-proxy-publish-azure-portal/configure-app.png)
5. <span data-ttu-id="1bd63-141">Om det behövs kan du konfigurera ytterligare inställningar.</span><span class="sxs-lookup"><span data-stu-id="1bd63-141">If necessary, configure additional settings.</span></span> <span data-ttu-id="1bd63-142">De flesta fall bör du behålla inställningarna i sina standardtillstånd.</span><span class="sxs-lookup"><span data-stu-id="1bd63-142">For most applications, you should keep these settings in their default states.</span></span> 
   - <span data-ttu-id="1bd63-143">**Tidsgränsen för backend-programmet**: Ange ett värde för**lång** endast om ditt program är långsam tooauthenticate och ansluta.</span><span class="sxs-lookup"><span data-stu-id="1bd63-143">**Backend Application Timeout**: Set this value too**Long** only if your application is slow tooauthenticate and connect.</span></span> 
   - <span data-ttu-id="1bd63-144">**Översätta URL: er i sidhuvuden**: behålla det här värdet som **Ja** om ditt program krävs hello ursprungliga värdhuvudet i hello autentiseringsbegäran.</span><span class="sxs-lookup"><span data-stu-id="1bd63-144">**Translate URLs in Headers**: Keep this value as **Yes** unless your application required hello original host header in hello authentication request.</span></span>
   - <span data-ttu-id="1bd63-145">**Översätta URL: er i programmet brödtext**: behålla det här värdet som **nr** om du har hårdkodad HTML länkar tooother lokala program och Använd inte anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="1bd63-145">**Translate URLs in Application Body**: Keep this value as **No** unless you have hardcoded HTML links tooother on-premises applications, and don't use custom domains.</span></span> <span data-ttu-id="1bd63-146">Mer information finns i [länka översättning med Application Proxy](application-proxy-link-translation.md).</span><span class="sxs-lookup"><span data-stu-id="1bd63-146">For more information, see [Link translation with Application Proxy](application-proxy-link-translation.md).</span></span>
   
   ![Konfigurera ditt program](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. <span data-ttu-id="1bd63-148">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1bd63-148">Select **Add**.</span></span>


## <a name="add-a-test-user"></a><span data-ttu-id="1bd63-149">Lägg till en testanvändare</span><span class="sxs-lookup"><span data-stu-id="1bd63-149">Add a test user</span></span> 

<span data-ttu-id="1bd63-150">tootest att appen har publicerats korrekt, lägga till ett användarkonto för testet.</span><span class="sxs-lookup"><span data-stu-id="1bd63-150">tootest that your app was published correctly, add a test user account.</span></span> <span data-ttu-id="1bd63-151">Kontrollera att det här kontot redan har behörigheter tooaccess hello-app från företagsnätverk hello.</span><span class="sxs-lookup"><span data-stu-id="1bd63-151">Verify that this account already has permissions tooaccess hello app from inside hello corporate network.</span></span>

1. <span data-ttu-id="1bd63-152">Tillbaka på hello Snabbstart-bladet välj **tilldela en användare för att testa**.</span><span class="sxs-lookup"><span data-stu-id="1bd63-152">Back on hello Quick start blade, select **Assign a user for testing**.</span></span>

  ![Tilldela en användare för testning](./media/application-proxy-publish-azure-portal/assign-user.png)

2. <span data-ttu-id="1bd63-154">Markera på hello användare och grupper bladet **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1bd63-154">On hello Users and groups blade, select **Add**.</span></span>

  ![Lägg till en användare eller grupp](./media/application-proxy-publish-azure-portal/add-user.png)

3. <span data-ttu-id="1bd63-156">Hello Lägg till tilldelning av bladet välj **användare och grupper** Välj hello-konto som du vill tooadd.</span><span class="sxs-lookup"><span data-stu-id="1bd63-156">On hello Add assignment blade, select **Users and groups** then choose hello account you want tooadd.</span></span> 
4. <span data-ttu-id="1bd63-157">Välj **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="1bd63-157">Select **Assign**.</span></span>

## <a name="test-your-published-app"></a><span data-ttu-id="1bd63-158">Testa din publicerade app</span><span class="sxs-lookup"><span data-stu-id="1bd63-158">Test your published app</span></span>

<span data-ttu-id="1bd63-159">Navigera i din webbläsare toohello extern URL som du konfigurerade under hello publicera steg.</span><span class="sxs-lookup"><span data-stu-id="1bd63-159">In your browser, navigate toohello external URL that you configured during hello publish step.</span></span> <span data-ttu-id="1bd63-160">Du bör se hello startskärmen och kan toosign in med hello testkonto du ställas in.</span><span class="sxs-lookup"><span data-stu-id="1bd63-160">You should see hello start screen, and be able toosign in with hello test account you set up.</span></span>

![Testa din publicerade app](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a><span data-ttu-id="1bd63-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1bd63-162">Next steps</span></span>
- <span data-ttu-id="1bd63-163">[Hämta kopplingar](active-directory-application-proxy-enable.md) och [skapa grupper för anslutningstjänsten](active-directory-application-proxy-connectors-azure-portal.md) toopublish program på separata nätverk och platser.</span><span class="sxs-lookup"><span data-stu-id="1bd63-163">[Download connectors](active-directory-application-proxy-enable.md) and [create connector groups](active-directory-application-proxy-connectors-azure-portal.md) toopublish applications on separate networks and locations.</span></span>

- <span data-ttu-id="1bd63-164">[Konfigurera enkel inloggning](application-proxy-sso-azure-portal.md) för din nyligen publicerade app</span><span class="sxs-lookup"><span data-stu-id="1bd63-164">[Set up single sign-on](application-proxy-sso-azure-portal.md) for your newly published app</span></span>
