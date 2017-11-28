---
title: aaaAuthenticate med Mobile Engagement REST API - manuell installation
description: "Beskriver hur toomanually in autentisering för Mobile Engagement REST API: er"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3884f94afcd6b9a62bfcf498fb6ee84bb6e837b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a><span data-ttu-id="d429f-103">Autentisera med Mobile Engagement REST API - manuell installation</span><span class="sxs-lookup"><span data-stu-id="d429f-103">Authenticate with Mobile Engagement REST APIs - manual setup</span></span>
<span data-ttu-id="d429f-104">Detta är ett tillägg dokumentationen för[autentisera med Mobile Engagement REST API: er](mobile-engagement-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="d429f-104">This is an appendix documentation too[Authenticate with Mobile Engagement REST APIs](mobile-engagement-api-authentication.md).</span></span> <span data-ttu-id="d429f-105">Kontrollera att du läser den första tooget hello kontext.</span><span class="sxs-lookup"><span data-stu-id="d429f-105">Make sure you read it first tooget hello context.</span></span> <span data-ttu-id="d429f-106">Här beskrivs ett annat sätt toodo hello enstaka installationsprogrammet för att skapa autentiseringen för hello Mobile Engagement REST API: er med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d429f-106">This describes an alternate way toodo hello One-time setup for setting up your authentication for hello Mobile Engagement REST APIs using hello Azure Portal.</span></span> 

> [!NOTE]
> <span data-ttu-id="d429f-107">hello anvisningarna nedan baseras på detta [guide för Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md) och anpassas för vad som krävs för autentisering för Mobile Engagement API: er.</span><span class="sxs-lookup"><span data-stu-id="d429f-107">hello instructions below are based on this [Active Directory guide](../azure-resource-manager/resource-group-create-service-principal-portal.md) and customized for what is required for authentication for Mobile Engagement APIs.</span></span> <span data-ttu-id="d429f-108">Läs tooit så om du vill toounderstand hello stegen nedan i detalj.</span><span class="sxs-lookup"><span data-stu-id="d429f-108">So refer tooit if you want toounderstand hello steps below in detail.</span></span> 
> 
> 

1. <span data-ttu-id="d429f-109">Inloggningen tooyour Azure-konto via hello [klassiska portalen](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="d429f-109">Login tooyour Azure Account through hello [classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="d429f-110">Välj **Active Directory** från hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="d429f-110">Select **Active Directory** from hello left pane.</span></span>
   
     ![Välj Active Directory][1]
3. <span data-ttu-id="d429f-112">Välj hello **standard Active Directory** i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d429f-112">Choose hello **Default Active Directory** in your Azure portal.</span></span> 
   
     ![Välj katalog][2]
   
   > [!IMPORTANT]
   > <span data-ttu-id="d429f-114">Den här metoden fungerar bara när du arbetar i hello standard Active Directory för ditt konto och fungerar inte om du vill göra detta i en Active Directory som du har skapat i ditt konto.</span><span class="sxs-lookup"><span data-stu-id="d429f-114">This approach works only when you are working in hello default Active Directory of your account and will not work if you are doing this in an Active Directory that you have created in your account.</span></span> 
   > 
   > 
4. <span data-ttu-id="d429f-115">tooview hello program i din katalog klickar du på **program**.</span><span class="sxs-lookup"><span data-stu-id="d429f-115">tooview hello applications in your directory, click on **Applications**.</span></span>
   
     ![Visa program][3]
5. <span data-ttu-id="d429f-117">Klicka på **lägga till**.</span><span class="sxs-lookup"><span data-stu-id="d429f-117">Click on **ADD**.</span></span> 
   
     ![Lägg till program][4]
6. <span data-ttu-id="d429f-119">Klicka på **Lägg till ett program som min organisation utvecklar**</span><span class="sxs-lookup"><span data-stu-id="d429f-119">Click on **Add an application my organization is developing**</span></span>
   
     ![nytt program][5]
7. <span data-ttu-id="d429f-121">Fyll i namnet på programmet hello och välj hello typ av program som **WEB APPLICATION och/eller webb-API** och klicka på knappen för nästa hello.</span><span class="sxs-lookup"><span data-stu-id="d429f-121">Fill in name of hello application and select hello type of application as **WEB APPLICATION AND/OR WEB API** and click hello next button.</span></span>
   
     ![namn på program][6]
8. <span data-ttu-id="d429f-123">Du kan ange eventuella dummy URL: er för **SIGN-ON-URL** och **APP-ID URI**.</span><span class="sxs-lookup"><span data-stu-id="d429f-123">You can provide any dummy URLs for **SIGN-ON URL** and **APP ID URI**.</span></span> <span data-ttu-id="d429f-124">Används inte för vårt scenario och hello URL: er själva valideras inte.</span><span class="sxs-lookup"><span data-stu-id="d429f-124">They are not used for our scenario and hello URLs themselves are not validated.</span></span>  
   
     ![Egenskaper för program][7]
9. <span data-ttu-id="d429f-126">Hello slutet av det här har du en AAD-app med hello namn du angav tidigare som hello nedan.</span><span class="sxs-lookup"><span data-stu-id="d429f-126">At hello end of this, you will have an AAD app with hello name you provided previously like hello following.</span></span> <span data-ttu-id="d429f-127">Det här är din **AD\_APP\_namn** och anteckna den.</span><span class="sxs-lookup"><span data-stu-id="d429f-127">This is your **AD\_APP\_NAME** and make a note of it.</span></span>  
   
     ![Appnamn][8]
10. <span data-ttu-id="d429f-129">Klicka på hello appens namn och klicka på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="d429f-129">Click on hello app name and click on **Configure**.</span></span>
    
      ![Konfigurera appen][9]
11. <span data-ttu-id="d429f-131">Anteckna hello klient-ID som ska användas som **klienten\_ID** för din API-anrop.</span><span class="sxs-lookup"><span data-stu-id="d429f-131">Make a note of hello CLIENT ID that will be used as **CLIENT\_ID** for your API calls.</span></span> 
    
     ![Konfigurera appen][10]
12. <span data-ttu-id="d429f-133">Bläddra nedåt toohello **nycklar** avsnittet och lägga till en nyckel med helst 2 år (upphör att gälla) varaktighet och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="d429f-133">Scroll down toohello **Keys** section and add a key with preferably 2 years (expiry) duration and click **Save**.</span></span> 
    
     ![Konfigurera appen][11]
13. <span data-ttu-id="d429f-135">Kopiera omedelbart hello-värde som visas för hello nyckeln som det visas bara nu och lagras inte så visas inte någonsin igen.</span><span class="sxs-lookup"><span data-stu-id="d429f-135">Immediately copy hello value which is shown for hello key as it is only shown now and is not stored so will not be displayed ever again.</span></span> <span data-ttu-id="d429f-136">Om du förlorar den att kommer du ha toogenerate en ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="d429f-136">If you lose it then you will have toogenerate a new key.</span></span> <span data-ttu-id="d429f-137">Detta blir hello **CLIENT_SECRET** för din API-anrop.</span><span class="sxs-lookup"><span data-stu-id="d429f-137">This will be hello **CLIENT_SECRET** for your API calls.</span></span> 
    
     ![Konfigurera appen][12]
    
    > [!IMPORTANT]
    > <span data-ttu-id="d429f-139">Den här nyckeln kommer att upphöra hello slutet av hello varaktighet som du angav så att kontrollera toorenew som du när hello tid annars kommer API-autentisering inte fungerar längre.</span><span class="sxs-lookup"><span data-stu-id="d429f-139">This key will expire at hello end of hello duration that you specified so make sure toorenew it when hello time comes otherwise your API authentication will not work anymore.</span></span> <span data-ttu-id="d429f-140">Du kan också ta bort och återskapa den här nyckeln om du tror att den har komprometterats.</span><span class="sxs-lookup"><span data-stu-id="d429f-140">You can also delete and recreate this key if you think that it has been compromised.</span></span>
    > 
    > 
14. <span data-ttu-id="d429f-141">Klicka på **visa SLUTPUNKTER** knappen nu som öppnas hello **App slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d429f-141">Click on **VIEW ENDPOINTS** button now which will open up hello **App Endpoints** dialog box.</span></span> 
    
    ![][13]
15. <span data-ttu-id="d429f-142">Kopiera från hello App slutpunkter i dialogrutan hello **OAUTH 2.0-TOKEN för SLUTPUNKT**.</span><span class="sxs-lookup"><span data-stu-id="d429f-142">From hello App Endpoints dialog box, copy hello **OAUTH 2.0 TOKEN ENDPOINT**.</span></span> 
    
    ![][14]
16. <span data-ttu-id="d429f-143">Den här slutpunkten blir i hello följande formulär där hello GUID i hello URL är din **TENANT_ID** så att anteckna den:</span><span class="sxs-lookup"><span data-stu-id="d429f-143">This endpoint will be in hello following form where hello GUID in hello URL is your **TENANT_ID** so make a note of it:</span></span> 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. <span data-ttu-id="d429f-144">Nu ska vi fortsätta tooconfigure hello behörigheter på den här appen.</span><span class="sxs-lookup"><span data-stu-id="d429f-144">Now we will proceed tooconfigure hello permissions on this app.</span></span> <span data-ttu-id="d429f-145">För detta måste tooopen in hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d429f-145">For this you will have tooopen up hello [Azure portal](https://portal.azure.com).</span></span> 
18. <span data-ttu-id="d429f-146">Klicka på **resursgrupper** och hitta hello **Mobile Engagement** resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d429f-146">Click on **Resource Groups** and find hello **Mobile Engagement** resource group.</span></span>  
    
    ![][15]
19. <span data-ttu-id="d429f-147">Klicka på hello **Mobile Engagement** resurs gruppen och navigera toohello **inställningar** här bladet.</span><span class="sxs-lookup"><span data-stu-id="d429f-147">Click hello **Mobile Engagement** resource group and navigate toohello **Settings** blade here.</span></span> 
    
    ![][16]
20. <span data-ttu-id="d429f-148">Klicka på **användare** i hello inställningsbladet och klicka sedan på **Lägg till** tooadd en användare.</span><span class="sxs-lookup"><span data-stu-id="d429f-148">Click on **Users** in hello Settings blade and then click on **Add** tooadd a user.</span></span> 
    
    ![][17]
21. <span data-ttu-id="d429f-149">Klicka på **Välj en roll**</span><span class="sxs-lookup"><span data-stu-id="d429f-149">Click on **Select a role**</span></span>
    
    ![][18]
22. <span data-ttu-id="d429f-150">Klicka på **ägare**</span><span class="sxs-lookup"><span data-stu-id="d429f-150">Click on **Owner**</span></span>
    
    ![][19]
23. <span data-ttu-id="d429f-151">Sök efter hello namnet på ditt program **AD\_APP\_namn** i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="d429f-151">Search for hello name of your application **AD\_APP\_NAME** in hello Search box.</span></span> <span data-ttu-id="d429f-152">Du kommer inte se det som standard här.</span><span class="sxs-lookup"><span data-stu-id="d429f-152">You will not see this by default here.</span></span> <span data-ttu-id="d429f-153">När du har hittat markerar du den och klicka på **Välj** längst hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="d429f-153">Once you find it, select it and click on **Select** at hello bottom of hello blade.</span></span> 
    
    ![][20]
24. <span data-ttu-id="d429f-154">På hello **Lägg till åtkomst** bladet den visas som **1 användare, 0 grupper**.</span><span class="sxs-lookup"><span data-stu-id="d429f-154">On hello **Add Access** blade, it will show up as **1 user, 0 groups**.</span></span> <span data-ttu-id="d429f-155">Klicka på **OK** på bladet tooconfirm hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="d429f-155">Click **OK** on this blade tooconfirm hello change.</span></span> 
    
    ![][21]

<span data-ttu-id="d429f-156">Nu har du slutfört hello krävs AAD-konfiguration och du kan alla set toocall hello API: er.</span><span class="sxs-lookup"><span data-stu-id="d429f-156">You have now completed hello required AAD configuration and you are all set toocall hello APIs.</span></span> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



