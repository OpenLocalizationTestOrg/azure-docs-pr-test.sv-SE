---
title: "aaaHow toouse Applösenord i Azure MFA? | Microsoft Docs"
description: "Den här sidan hjälper användarna att förstå vad applösenord finns och hur de kan användas för med beaktande tooAzure MFA."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: bf2c11edc0ca81f2950eff0f7a3a24c8a5b34623
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a><span data-ttu-id="db5fa-104">Vad är Applösenord i Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="db5fa-104">What are App Passwords in Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="db5fa-105">Vissa icke-webbläsarappar, till exempel hello Apple interna e-klient som använder Exchange Active Sync stöder för tillfället inte multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="db5fa-105">Certain non-browser apps, such as hello Apple native email client that uses Exchange Active Sync, currently do not support multi-factor authentication.</span></span> <span data-ttu-id="db5fa-106">Multifaktorautentisering aktiveras per användare.</span><span class="sxs-lookup"><span data-stu-id="db5fa-106">Multi-factor authentication is enabled per user.</span></span> <span data-ttu-id="db5fa-107">Det innebär att om en användare har aktiverats för multifaktorautentisering och de försöker toouse icke-webbläsarbaserade appar, kommer de att toodo så.</span><span class="sxs-lookup"><span data-stu-id="db5fa-107">This means that if a user has been enabled for multi-factor authentication and they are attempting toouse non-browser apps, they will be unable toodo so.</span></span> <span data-ttu-id="db5fa-108">Ett applösenord gör detta toooccur.</span><span class="sxs-lookup"><span data-stu-id="db5fa-108">An app password allows this toooccur.</span></span>

<span data-ttu-id="db5fa-109">När du har ett applösenord kan använda du den i stället för det ursprungliga lösenordet med dessa icke-webbläsarbaserade appar.</span><span class="sxs-lookup"><span data-stu-id="db5fa-109">Once you have an app password, you use this in place of your original password with these non-browser apps.</span></span> <span data-ttu-id="db5fa-110">Det beror på att när du registrerar dig för tvåstegsverifiering du om Microsoft inte toolet någon logga in med ditt lösenord om de inte kan också utföra andra hello-verifiering.</span><span class="sxs-lookup"><span data-stu-id="db5fa-110">This is because when you register for two-step verification, you're telling Microsoft not toolet anyone sign in with your password if they can't also perform hello second verification.</span></span> <span data-ttu-id="db5fa-111">hello Apple interna e-postklienten på telefonen logga inte in dig eftersom den inte kan fråga efter tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="db5fa-111">hello Apple native email client on your phone can't sign in as you because it can't ask for two-step verification.</span></span> <span data-ttu-id="db5fa-112">hello lösning för detta är toocreate ett säkrare applösenord som du inte använder dagliga, men bara för de appar som inte stöder tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="db5fa-112">hello solution for this is toocreate a more secure app password that you don't use day-to-day, but only for those apps that can't support two-step verification.</span></span> <span data-ttu-id="db5fa-113">Använd hello lösenord så att appar kan hoppa över multifaktorautentisering och fortsätta toowork.</span><span class="sxs-lookup"><span data-stu-id="db5fa-113">Use hello app password so that apps can bypass multi-factor authentication and continue toowork.</span></span>

> [!NOTE]
> <span data-ttu-id="db5fa-114">Office 2013-klienter (inklusive Outlook) stöder nya autentiseringsprotokoll och kan användas med tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="db5fa-114">Office 2013 clients (including Outlook) support new authentication protocols and can be used with two-step verification.</span></span>  <span data-ttu-id="db5fa-115">Det innebär att när du har aktiverat, applösenord inte krävs för användning med Office 2013-klienter.</span><span class="sxs-lookup"><span data-stu-id="db5fa-115">This means that once enabled, app passwords are not required for use with Office 2013 clients.</span></span>  <span data-ttu-id="db5fa-116">Mer information finns i [Office 2013 modern autentisering offentlig förhandsgranskning av](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span><span class="sxs-lookup"><span data-stu-id="db5fa-116">For more information, see [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span></span>


## <a name="how-toouse-app-passwords"></a><span data-ttu-id="db5fa-117">Hur toouse applösenord</span><span class="sxs-lookup"><span data-stu-id="db5fa-117">How toouse app passwords</span></span>
<span data-ttu-id="db5fa-118">hello nedan följer några saker tooremember om hur toouse applösenord.</span><span class="sxs-lookup"><span data-stu-id="db5fa-118">hello following are some things tooremember on how toouse app passwords.</span></span>

* <span data-ttu-id="db5fa-119">Du skapa inte egna applösenord.</span><span class="sxs-lookup"><span data-stu-id="db5fa-119">You don't create your own app passwords.</span></span> <span data-ttu-id="db5fa-120">I stället genereras de automatiskt.</span><span class="sxs-lookup"><span data-stu-id="db5fa-120">Instead, they are automatically generated.</span></span> <span data-ttu-id="db5fa-121">Eftersom du bara ha tooenter hello lösenord en gång per app är säkrare toouse ett mer komplexa lösenord som skapas automatiskt i stället för att göra en som du kan komma ihåg.</span><span class="sxs-lookup"><span data-stu-id="db5fa-121">Since you only have tooenter hello app password once per app, it's safer toouse a more complex, automatically generated password rather than making one that you can remember.</span></span>
* <span data-ttu-id="db5fa-122">Det finns för närvarande en gräns på 40 lösenord per användare.</span><span class="sxs-lookup"><span data-stu-id="db5fa-122">Currently there is a limit of 40 passwords per user.</span></span> <span data-ttu-id="db5fa-123">Om du försöker toocreate en när hello gränsen har nåtts, kan du ange toodelete en av dina befintliga applösenord innan du skapar en ny.</span><span class="sxs-lookup"><span data-stu-id="db5fa-123">If you attempt toocreate one after you have reached hello limit, you will be prompted toodelete one of your existing app passwords before you create a new one.</span></span>
* <span data-ttu-id="db5fa-124">Du bör använda ett applösenord per enhet, inte per program.</span><span class="sxs-lookup"><span data-stu-id="db5fa-124">You should use one app password per device, not per application.</span></span> <span data-ttu-id="db5fa-125">Du kan till exempel skapa ett applösenord för din bärbara dator och använda det applösenordet för alla program på den bärbara datorn.</span><span class="sxs-lookup"><span data-stu-id="db5fa-125">For example, you can create one app password for your laptop and use that app password for all of your applications on that laptop.</span></span> <span data-ttu-id="db5fa-126">Skapa sedan en andra app lösenord toouse för alla dina appar på ditt skrivbord.</span><span class="sxs-lookup"><span data-stu-id="db5fa-126">Then, create a second app password toouse for all your apps on your desktop.</span></span> 
* <span data-ttu-id="db5fa-127">Du får en app lösenord hello första gången du registrerar dig för tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="db5fa-127">You are given one app password hello first time you register for two-step verification.</span></span>  <span data-ttu-id="db5fa-128">Om du behöver ytterligare mallar kan skapa du dem.</span><span class="sxs-lookup"><span data-stu-id="db5fa-128">If you need additional ones, you can create them.</span></span>



## <a name="creating-and-deleting-app-passwords"></a><span data-ttu-id="db5fa-129">Skapa och ta bort applösenord</span><span class="sxs-lookup"><span data-stu-id="db5fa-129">Creating and deleting app passwords</span></span>
<span data-ttu-id="db5fa-130">Vid första inloggningen ges ett applösenord som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="db5fa-130">During your initial sign-in you are given an app password that you can use.</span></span>  <span data-ttu-id="db5fa-131">Dessutom kan du också skapa och ta bort applösenord vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="db5fa-131">Additionally you can also create and delete app passwords later on.</span></span>  <span data-ttu-id="db5fa-132">Hur du gör detta beror på hur du använder multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="db5fa-132">How you do this depends on how you use multi-factor authentication.</span></span> <span data-ttu-id="db5fa-133">Svaret hello följande frågor toodetermine där du ska gå toomanage applösenord:</span><span class="sxs-lookup"><span data-stu-id="db5fa-133">Answer hello following questions toodetermine where you should go toomanage app passwords:</span></span> 

1. <span data-ttu-id="db5fa-134">Använder du tvåstegsverifiering för ditt personliga Microsoft-konto?</span><span class="sxs-lookup"><span data-stu-id="db5fa-134">Do you use two-step verification for your personal Microsoft account?</span></span> <span data-ttu-id="db5fa-135">Om Ja, bör du läsa toohello [applösenord och tvåstegsverifiering](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) artikeln för hjälp.</span><span class="sxs-lookup"><span data-stu-id="db5fa-135">If yes, you should refer toohello [App passwords and two-step verification](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) article for help.</span></span> <span data-ttu-id="db5fa-136">Om Nej, Fortsätt tooquestion två.</span><span class="sxs-lookup"><span data-stu-id="db5fa-136">If no, continue tooquestion two.</span></span>

2. <span data-ttu-id="db5fa-137">OK så att du kan använda tvåstegsverifiering för ditt arbets- eller skolkonto konto.</span><span class="sxs-lookup"><span data-stu-id="db5fa-137">Ok, so you use two-step verification for your work or school account.</span></span> <span data-ttu-id="db5fa-138">Du använder den toosign i tooOffice 365 appar?</span><span class="sxs-lookup"><span data-stu-id="db5fa-138">Do you use it toosign in tooOffice 365 apps?</span></span> <span data-ttu-id="db5fa-139">Om Ja, bör du läsa för[skapa ett applösenord för Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) för hjälp.</span><span class="sxs-lookup"><span data-stu-id="db5fa-139">If yes, you should refer too[Create an app password for Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) for help.</span></span> <span data-ttu-id="db5fa-140">Om Nej, Fortsätt tooquestion tre.</span><span class="sxs-lookup"><span data-stu-id="db5fa-140">If no, continue tooquestion three.</span></span> 

3. <span data-ttu-id="db5fa-141">Använder du tvåstegsverifiering med Microsoft Azure?</span><span class="sxs-lookup"><span data-stu-id="db5fa-141">Do you use two-step verification with Microsoft Azure?</span></span> <span data-ttu-id="db5fa-142">Om Ja, Fortsätt toohello [hantera applösenord i hello Azure-portalen](#manage-app-passwords-in-the-Azure-portal) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="db5fa-142">If yes, continue toohello [Manage app passwords in hello Azure portal](#manage-app-passwords-in-the-Azure-portal) section of this article.</span></span> <span data-ttu-id="db5fa-143">Om Nej, Fortsätt tooquestion fyra.</span><span class="sxs-lookup"><span data-stu-id="db5fa-143">If no, continue tooquestion four.</span></span>

4. <span data-ttu-id="db5fa-144">Osäker på om du använder tvåstegsverifiering?</span><span class="sxs-lookup"><span data-stu-id="db5fa-144">Not sure where you use two-step verification?</span></span> <span data-ttu-id="db5fa-145">Fortsätta toohello [hantera applösenord med hello MyApps portal](#manage-app-passwords-with-the-myapps-portal) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="db5fa-145">Continue toohello [Manage app passwords with hello MyApps portal](#manage-app-passwords-with-the-myapps-portal) section of this article.</span></span> 


## <a name="manage-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="db5fa-146">Hantera lösenord i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="db5fa-146">Manage app passwords in hello Azure portal</span></span>
<span data-ttu-id="db5fa-147">Om du använder tvåstegsverifiering med Azure kan du toocreate applösenord via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="db5fa-147">If you use two-step verification with Azure, you want toocreate app passwords through hello Azure portal.</span></span>

### <a name="toocreate-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="db5fa-148">toocreate applösenord i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="db5fa-148">toocreate app passwords in hello Azure portal</span></span>
1. <span data-ttu-id="db5fa-149">Logga in toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="db5fa-149">Sign in toohello Azure classic portal.</span></span>
2. <span data-ttu-id="db5fa-150">Högerklicka på ditt användarnamn hello överst och välj ytterligare säkerhetskontroll.</span><span class="sxs-lookup"><span data-stu-id="db5fa-150">At hello top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="db5fa-151">Välj applösenord på hello proofup sidan hello överst</span><span class="sxs-lookup"><span data-stu-id="db5fa-151">On hello proofup page, at hello top, select app passwords</span></span>
4. <span data-ttu-id="db5fa-152">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-152">Click **Create**.</span></span>
5. <span data-ttu-id="db5fa-153">Ange ett namn för hello lösenord och klicka på **nästa**</span><span class="sxs-lookup"><span data-stu-id="db5fa-153">Enter a name for hello app password and click **Next**</span></span>
6. <span data-ttu-id="db5fa-154">Kopiera hello app lösenord toohello Urklipp och klistrar in den i din app.</span><span class="sxs-lookup"><span data-stu-id="db5fa-154">Copy hello app password toohello clipboard and paste it into your app.</span></span>
   
   ![Molnet](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="toodelete-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="db5fa-156">toodelete applösenord i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="db5fa-156">toodelete app passwords in hello Azure portal</span></span>
1. <span data-ttu-id="db5fa-157">Logga in toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="db5fa-157">Sign in toohello Azure classic portal.</span></span>
2. <span data-ttu-id="db5fa-158">Högerklicka på ditt användarnamn hello överst och välj ytterligare säkerhetskontroll.</span><span class="sxs-lookup"><span data-stu-id="db5fa-158">At hello top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="db5fa-159">Överst hello, nästa tooadditional säkerhetskontroll Välj **applösenord.**</span><span class="sxs-lookup"><span data-stu-id="db5fa-159">At hello top, next tooadditional security verification, select **app passwords.**</span></span>
4. <span data-ttu-id="db5fa-160">Nästa toohello applösenord som du vill toodelete, Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-160">Next toohello app password you want toodelete, select **Delete**.</span></span>
5. <span data-ttu-id="db5fa-161">Bekräfta borttagning av hello genom att klicka på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-161">Confirm hello deletion by clicking **yes**.</span></span>
6. <span data-ttu-id="db5fa-162">När hello applösenord har tagits bort, kan du klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-162">Once hello app password is deleted, you can click **close**.</span></span>


## <a name="manage-app-passwords-with-hello-myapps-portal"></a><span data-ttu-id="db5fa-163">Hantera applösenord med hello MyApps portal.</span><span class="sxs-lookup"><span data-stu-id="db5fa-163">Manage app passwords with hello MyApps portal.</span></span>
<span data-ttu-id="db5fa-164">Om du inte är säker på hur du använder multi-Factor authentication kan sedan du alltid skapa och ta bort applösenord hello myapps-portalen.</span><span class="sxs-lookup"><span data-stu-id="db5fa-164">If you are not sure how you use multi-factor authentication, then you can always create and delete app passwords through hello myapps portal.</span></span>

### <a name="toocreate-an-app-password-using-hello-myapps-portal"></a><span data-ttu-id="db5fa-165">en app lösenord med hjälp av toocreate hello Myapps portal</span><span class="sxs-lookup"><span data-stu-id="db5fa-165">toocreate an app password using hello Myapps portal</span></span>
1. <span data-ttu-id="db5fa-166">Logga in för[https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="db5fa-166">Sign in too[https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="db5fa-167">Klicka på ditt namn på hello längst upp till höger och välj **profil**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-167">Click your name at hello top right, and choose **Profile**.</span></span>
3. <span data-ttu-id="db5fa-168">Välj **ytterligare säkerhetsverifiering**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-168">Select **Additional Security Verification**.</span></span>
   <span data-ttu-id="db5fa-169">![Välj ytterligare säkerhetskontroll – skärmbild](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span><span class="sxs-lookup"><span data-stu-id="db5fa-169">![Select additional security verification - screenshot](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span></span>

4. <span data-ttu-id="db5fa-170">Välj **applösenord**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-170">Select **app passwords**.</span></span>
   <span data-ttu-id="db5fa-171">![Välj applösenord – skärmbild](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span><span class="sxs-lookup"><span data-stu-id="db5fa-171">![Select app passwords - screenshot](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span></span>

5. <span data-ttu-id="db5fa-172">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-172">Click **Create**.</span></span>
6. <span data-ttu-id="db5fa-173">Ange ett namn för hello lösenord och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-173">Enter a name for hello app password and click **Next**.</span></span>
7. <span data-ttu-id="db5fa-174">Kopiera hello app lösenord toohello Urklipp och klistrar in den i din app.</span><span class="sxs-lookup"><span data-stu-id="db5fa-174">Copy hello app password toohello clipboard and paste it into your app.</span></span>
   <span data-ttu-id="db5fa-175">![Skapa ett applösenord](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span><span class="sxs-lookup"><span data-stu-id="db5fa-175">![Create an app password](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span></span>

### <a name="toodelete-an-app-password-using-hello-myapps-portal"></a><span data-ttu-id="db5fa-176">en app lösenord med hjälp av toodelete hello Myapps portal</span><span class="sxs-lookup"><span data-stu-id="db5fa-176">toodelete an app password using hello Myapps portal</span></span>
1. <span data-ttu-id="db5fa-177">Logga in för[https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="db5fa-177">Sign in too[https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="db5fa-178">Välj profil hello överst.</span><span class="sxs-lookup"><span data-stu-id="db5fa-178">At hello top, select profile.</span></span>
3. <span data-ttu-id="db5fa-179">Välj **ytterligare säkerhetsverifiering**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-179">Select **Additional Security Verification**.</span></span>

   ![Välj ytterligare säkerhetskontroll – skärmbild](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. <span data-ttu-id="db5fa-181">Välj **applösenord**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-181">Select **app passwords**.</span></span>

   ![Välj applösenord – skärmbild](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. <span data-ttu-id="db5fa-183">Nästa toohello applösenord som du vill toodelete, klickar du på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-183">Next toohello app password you want toodelete, click **Delete**.</span></span>

   ![Ta bort ett applösenord](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. <span data-ttu-id="db5fa-185">Bekräfta att du vill toodelete lösenordet genom att klicka på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-185">Confirm that you want toodelete that password by clicking **yes**.</span></span>
7. <span data-ttu-id="db5fa-186">När hello applösenord har tagits bort, kan du klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="db5fa-186">Once hello app password is deleted, you can click **close**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db5fa-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db5fa-187">Next steps</span></span>

- [<span data-ttu-id="db5fa-188">Hantera dina inställningar för tvåstegsverifiering</span><span class="sxs-lookup"><span data-stu-id="db5fa-188">Manage your two-step verification settings</span></span>](multi-factor-authentication-end-user-manage-settings.md)

- <span data-ttu-id="db5fa-189">Testa hello [Microsoft Authenticator-appen](microsoft-authenticator-app-how-to.md) tooverify din inloggningar med app-meddelanden i stället för att ta emot texter eller samtal.</span><span class="sxs-lookup"><span data-stu-id="db5fa-189">Try out hello [Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) tooverify your sign-ins with app notifications, instead of receiving texts or calls.</span></span> 
