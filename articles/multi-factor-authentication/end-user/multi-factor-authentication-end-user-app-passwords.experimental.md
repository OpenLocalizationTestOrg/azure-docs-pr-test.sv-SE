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
ms.openlocfilehash: 3afa2003d8e87576f035bf9440a1dba67bd85f5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a><span data-ttu-id="18b09-104">Vad är Applösenord i Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="18b09-104">What are App Passwords in Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="18b09-105">Vissa icke-webbläsarappar, till exempel hello Apple interna e-klient som använder Exchange Active Sync stöder för tillfället inte multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="18b09-105">Certain non-browser apps, such as hello Apple native email client that uses Exchange Active Sync, currently do not support multi-factor authentication.</span></span> <span data-ttu-id="18b09-106">Multifaktorautentisering aktiveras per användare.</span><span class="sxs-lookup"><span data-stu-id="18b09-106">Multi-factor authentication is enabled per user.</span></span>  <span data-ttu-id="18b09-107">Detta innebär att en användare inte kan använda multifaktorautentisering om:</span><span class="sxs-lookup"><span data-stu-id="18b09-107">This means that a user can't use multi-factor authentication if:</span></span>

- <span data-ttu-id="18b09-108">hello användaren har aktiverats för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="18b09-108">hello user has been enabled for multi-factor authentication</span></span>
- <span data-ttu-id="18b09-109">hello användaren försöker toouse en icke-Webbläsarprogrammet.</span><span class="sxs-lookup"><span data-stu-id="18b09-109">hello user is trying toouse a non-browser app.</span></span>

<span data-ttu-id="18b09-110">Ett applösenord kan hello användaren toouse hello app.</span><span class="sxs-lookup"><span data-stu-id="18b09-110">An app password allows hello user toouse hello app.</span></span>

<span data-ttu-id="18b09-111">När du har ett applösenord kan använda du den i stället för det ursprungliga lösenordet med dessa icke-webbläsarbaserade appar.</span><span class="sxs-lookup"><span data-stu-id="18b09-111">Once you have an app password, you use it in place of your original password with these non-browser apps.</span></span> <span data-ttu-id="18b09-112">När du registrerar dig för tvåstegsverifiering du talar om Microsoft inte toolet någon logga in med ditt lösenord om de inte kan också utföra andra hello-verifiering.</span><span class="sxs-lookup"><span data-stu-id="18b09-112">When you register for two-step verification, you're telling Microsoft not toolet anyone sign in with your password if they can't also perform hello second verification.</span></span> <span data-ttu-id="18b09-113">hello Apple interna e-postklienten på telefonen logga inte in dig eftersom den inte kan fråga efter tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="18b09-113">hello Apple native email client on your phone can't sign in as you because it can't ask for two-step verification.</span></span> <span data-ttu-id="18b09-114">hello lösning toothis problemet är toocreate ett säkrare applösenord som du inte använder dagliga.</span><span class="sxs-lookup"><span data-stu-id="18b09-114">hello solution toothis problem is toocreate a more secure app password that you don't use day-to-day.</span></span> <span data-ttu-id="18b09-115">Applösenord gäller endast de appar som inte stöder tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="18b09-115">App passwords are only for those apps that can't support two-step verification.</span></span> <span data-ttu-id="18b09-116">Använd hello lösenord så att appar kan hoppa över multifaktorautentisering och fortsätta toowork.</span><span class="sxs-lookup"><span data-stu-id="18b09-116">Use hello app password so that apps can bypass multi-factor authentication and continue toowork.</span></span>


> [!NOTE]
> <span data-ttu-id="18b09-117">Office 2013-klienter (inklusive Outlook) stöder nya autentiseringsprotokoll och kan användas med tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="18b09-117">Office 2013 clients (including Outlook) support new authentication protocols and can be used with two-step verification.</span></span> <span data-ttu-id="18b09-118">Applösenord krävs inte för användning med Office 2013-klienter.</span><span class="sxs-lookup"><span data-stu-id="18b09-118">App passwords are not required for use with Office 2013 clients.</span></span>  <span data-ttu-id="18b09-119">Mer information finns i [Office 2013 modern autentisering offentlig förhandsgranskning av](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span><span class="sxs-lookup"><span data-stu-id="18b09-119">For more information, see [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span></span>


## <a name="how-toouse-app-passwords"></a><span data-ttu-id="18b09-120">Hur toouse applösenord</span><span class="sxs-lookup"><span data-stu-id="18b09-120">How toouse app passwords</span></span>
<span data-ttu-id="18b09-121">Här följer några saker tooknow om applösenord:</span><span class="sxs-lookup"><span data-stu-id="18b09-121">Here are some things tooknow about app passwords:</span></span>

* <span data-ttu-id="18b09-122">Du skapa inte egna applösenord.</span><span class="sxs-lookup"><span data-stu-id="18b09-122">You don't create your own app passwords.</span></span> <span data-ttu-id="18b09-123">De genereras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="18b09-123">They are automatically generated.</span></span>
* <span data-ttu-id="18b09-124">Det finns för närvarande en gräns på 40 lösenord per användare.</span><span class="sxs-lookup"><span data-stu-id="18b09-124">Currently there is a limit of 40 passwords per user.</span></span> 
* <span data-ttu-id="18b09-125">Om du försöker toocreate ett applösenord när hello gränsen har nåtts, har du toodelete en av dina befintliga applösenord innan du skapar en ny.</span><span class="sxs-lookup"><span data-stu-id="18b09-125">If you try toocreate an app password after you have reached hello limit, you'll have toodelete one of your existing app passwords before you create a new one.</span></span>
* <span data-ttu-id="18b09-126">Använda ett applösenord per enhet, inte per program.</span><span class="sxs-lookup"><span data-stu-id="18b09-126">Use one app password per device, not per application.</span></span> <span data-ttu-id="18b09-127">Du kan till exempel skapa ett applösenord för din bärbara dator och använda det applösenordet för alla program på den bärbara datorn.</span><span class="sxs-lookup"><span data-stu-id="18b09-127">For example, you can create one app password for your laptop and use that app password for all of your applications on that laptop.</span></span> <span data-ttu-id="18b09-128">Skapa sedan en andra app lösenord toouse för alla dina appar på ditt skrivbord.</span><span class="sxs-lookup"><span data-stu-id="18b09-128">Then, create a second app password toouse for all your apps on your desktop.</span></span> 
* <span data-ttu-id="18b09-129">Du får en app lösenord hello första gången du registrerar dig för tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="18b09-129">You are given one app password hello first time you register for two-step verification.</span></span>  <span data-ttu-id="18b09-130">Om du behöver ytterligare mallar kan skapa du dem.</span><span class="sxs-lookup"><span data-stu-id="18b09-130">If you need additional ones, you can create them.</span></span>



## <a name="creating-and-deleting-app-passwords"></a><span data-ttu-id="18b09-131">Skapa och ta bort applösenord</span><span class="sxs-lookup"><span data-stu-id="18b09-131">Creating and deleting app passwords</span></span>
<span data-ttu-id="18b09-132">Vid första inloggningen får du ett applösenord som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="18b09-132">During your initial sign-in, you are given an app password that you can use.</span></span>  <span data-ttu-id="18b09-133">Du kan också skapa och ta bort applösenord vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="18b09-133">You can also create and delete app passwords later on.</span></span> <span data-ttu-id="18b09-134">Hur du tar bort applösenord beror på hur du använder multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="18b09-134">How you delete app passwords depends on how you use multi-factor authentication.</span></span> <span data-ttu-id="18b09-135">Svaret hello följande frågor toodetermine där du ska gå toomanage applösenord:</span><span class="sxs-lookup"><span data-stu-id="18b09-135">Answer hello following questions toodetermine where you should go toomanage app passwords:</span></span> 

1. <span data-ttu-id="18b09-136">Använder du tvåstegsverifiering för ditt personliga Microsoft-konto?</span><span class="sxs-lookup"><span data-stu-id="18b09-136">Do you use two-step verification for your personal Microsoft account?</span></span> <span data-ttu-id="18b09-137">Om Ja, bör du läsa toohello [applösenord och tvåstegsverifiering](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) artikeln för hjälp.</span><span class="sxs-lookup"><span data-stu-id="18b09-137">If yes, you should refer toohello [App passwords and two-step verification](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) article for help.</span></span> <span data-ttu-id="18b09-138">Om Nej, Fortsätt tooquestion två.</span><span class="sxs-lookup"><span data-stu-id="18b09-138">If no, continue tooquestion two.</span></span>

2. <span data-ttu-id="18b09-139">OK så att du kan använda tvåstegsverifiering för ditt arbets- eller skolkonto konto.</span><span class="sxs-lookup"><span data-stu-id="18b09-139">Ok, so you use two-step verification for your work or school account.</span></span> <span data-ttu-id="18b09-140">Du använder den toosign i tooOffice 365 appar?</span><span class="sxs-lookup"><span data-stu-id="18b09-140">Do you use it toosign in tooOffice 365 apps?</span></span> <span data-ttu-id="18b09-141">Om Ja, bör du läsa för[skapa ett applösenord för Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) för hjälp.</span><span class="sxs-lookup"><span data-stu-id="18b09-141">If yes, you should refer too[Create an app password for Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) for help.</span></span> <span data-ttu-id="18b09-142">Om Nej, Fortsätt tooquestion tre.</span><span class="sxs-lookup"><span data-stu-id="18b09-142">If no, continue tooquestion three.</span></span> 

3. <span data-ttu-id="18b09-143">Använder du tvåstegsverifiering med Microsoft Azure?</span><span class="sxs-lookup"><span data-stu-id="18b09-143">Do you use two-step verification with Microsoft Azure?</span></span> <span data-ttu-id="18b09-144">Om Ja, Fortsätt toohello [hantera applösenord i hello Azure-portalen](#manage-app-passwords-in-the-Azure-portal) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="18b09-144">If yes, continue toohello [Manage app passwords in hello Azure portal](#manage-app-passwords-in-the-Azure-portal) section of this article.</span></span> <span data-ttu-id="18b09-145">Om Nej, Fortsätt tooquestion fyra.</span><span class="sxs-lookup"><span data-stu-id="18b09-145">If no, continue tooquestion four.</span></span>

4. <span data-ttu-id="18b09-146">Osäker på om du använder tvåstegsverifiering?</span><span class="sxs-lookup"><span data-stu-id="18b09-146">Not sure where you use two-step verification?</span></span> <span data-ttu-id="18b09-147">Fortsätta toohello [hantera applösenord med hello MyApps portal](#manage-app-passwords-with-the-myapps-portal) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="18b09-147">Continue toohello [Manage app passwords with hello MyApps portal](#manage-app-passwords-with-the-myapps-portal) section of this article.</span></span> 


## <a name="manage-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="18b09-148">Hantera lösenord i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="18b09-148">Manage app passwords in hello Azure portal</span></span>
<span data-ttu-id="18b09-149">Om du använder tvåstegsverifiering med Azure kan du toocreate applösenord via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18b09-149">If you use two-step verification with Azure, you want toocreate app passwords through hello Azure portal.</span></span>

### <a name="toocreate-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="18b09-150">toocreate applösenord i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="18b09-150">toocreate app passwords in hello Azure portal</span></span>
1. <span data-ttu-id="18b09-151">Logga in toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18b09-151">Sign in toohello Azure classic portal.</span></span>
2. <span data-ttu-id="18b09-152">Högerklicka på ditt användarnamn hello överst och välj ytterligare säkerhetskontroll.</span><span class="sxs-lookup"><span data-stu-id="18b09-152">At hello top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="18b09-153">Välj applösenord på hello proofup sidan hello överst</span><span class="sxs-lookup"><span data-stu-id="18b09-153">On hello proofup page, at hello top, select app passwords</span></span>
4. <span data-ttu-id="18b09-154">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="18b09-154">Click **Create**.</span></span>
5. <span data-ttu-id="18b09-155">Ange ett namn för hello lösenord och klicka på **nästa**</span><span class="sxs-lookup"><span data-stu-id="18b09-155">Enter a name for hello app password and click **Next**</span></span>
6. <span data-ttu-id="18b09-156">Kopiera hello app lösenord toohello Urklipp och klistrar in den i din app.</span><span class="sxs-lookup"><span data-stu-id="18b09-156">Copy hello app password toohello clipboard and paste it into your app.</span></span>
   
   ![Molnet](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="toodelete-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="18b09-158">toodelete applösenord i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="18b09-158">toodelete app passwords in hello Azure portal</span></span>
1. <span data-ttu-id="18b09-159">Logga in toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18b09-159">Sign in toohello Azure classic portal.</span></span>
2. <span data-ttu-id="18b09-160">Högerklicka på ditt användarnamn hello överst och välj ytterligare säkerhetskontroll.</span><span class="sxs-lookup"><span data-stu-id="18b09-160">At hello top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="18b09-161">Överst hello, nästa tooadditional säkerhetskontroll Välj **applösenord.**</span><span class="sxs-lookup"><span data-stu-id="18b09-161">At hello top, next tooadditional security verification, select **app passwords.**</span></span>
4. <span data-ttu-id="18b09-162">Nästa toohello applösenord som du vill toodelete, Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="18b09-162">Next toohello app password you want toodelete, select **Delete**.</span></span>
5. <span data-ttu-id="18b09-163">Bekräfta borttagning av hello genom att klicka på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="18b09-163">Confirm hello deletion by clicking **yes**.</span></span>
6. <span data-ttu-id="18b09-164">När hello applösenord har tagits bort, kan du klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="18b09-164">Once hello app password is deleted, you can click **close**.</span></span>


## <a name="manage-app-passwords-with-hello-myapps-portal"></a><span data-ttu-id="18b09-165">Hantera applösenord med hello MyApps portal.</span><span class="sxs-lookup"><span data-stu-id="18b09-165">Manage app passwords with hello MyApps portal.</span></span>
<span data-ttu-id="18b09-166">Om du inte är säker på hur du använder multi-Factor authentication kan sedan du alltid skapa och ta bort applösenord hello myapps-portalen.</span><span class="sxs-lookup"><span data-stu-id="18b09-166">If you are not sure how you use multi-factor authentication, then you can always create and delete app passwords through hello myapps portal.</span></span>

### <a name="toocreate-an-app-password-using-hello-myapps-portal"></a><span data-ttu-id="18b09-167">en app lösenord med hjälp av toocreate hello Myapps portal</span><span class="sxs-lookup"><span data-stu-id="18b09-167">toocreate an app password using hello Myapps portal</span></span>
1. <span data-ttu-id="18b09-168">Logga in för[https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="18b09-168">Sign in too[https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="18b09-169">Klicka på ditt namn på hello längst upp till höger och välj **profil**.</span><span class="sxs-lookup"><span data-stu-id="18b09-169">Click your name at hello top right, and choose **Profile**.</span></span>
3. <span data-ttu-id="18b09-170">Välj **ytterligare säkerhetsverifiering**.</span><span class="sxs-lookup"><span data-stu-id="18b09-170">Select **Additional Security Verification**.</span></span>
   <span data-ttu-id="18b09-171">![Välj ytterligare säkerhetskontroll – skärmbild](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span><span class="sxs-lookup"><span data-stu-id="18b09-171">![Select additional security verification - screenshot](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span></span>

4. <span data-ttu-id="18b09-172">Välj **applösenord**.</span><span class="sxs-lookup"><span data-stu-id="18b09-172">Select **app passwords**.</span></span>
   <span data-ttu-id="18b09-173">![Välj applösenord – skärmbild](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span><span class="sxs-lookup"><span data-stu-id="18b09-173">![Select app passwords - screenshot](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span></span>

5. <span data-ttu-id="18b09-174">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="18b09-174">Click **Create**.</span></span>
6. <span data-ttu-id="18b09-175">Ange ett namn för hello lösenord och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="18b09-175">Enter a name for hello app password and click **Next**.</span></span>
7. <span data-ttu-id="18b09-176">Kopiera hello app lösenord toohello Urklipp och klistrar in den i din app.</span><span class="sxs-lookup"><span data-stu-id="18b09-176">Copy hello app password toohello clipboard and paste it into your app.</span></span>
   <span data-ttu-id="18b09-177">![Skapa ett applösenord](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span><span class="sxs-lookup"><span data-stu-id="18b09-177">![Create an app password](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span></span>

### <a name="toodelete-an-app-password-using-hello-myapps-portal"></a><span data-ttu-id="18b09-178">en app lösenord med hjälp av toodelete hello Myapps portal</span><span class="sxs-lookup"><span data-stu-id="18b09-178">toodelete an app password using hello Myapps portal</span></span>
1. <span data-ttu-id="18b09-179">Logga in för[https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="18b09-179">Sign in too[https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="18b09-180">Välj profil hello överst.</span><span class="sxs-lookup"><span data-stu-id="18b09-180">At hello top, select profile.</span></span>
3. <span data-ttu-id="18b09-181">Välj **ytterligare säkerhetsverifiering**.</span><span class="sxs-lookup"><span data-stu-id="18b09-181">Select **Additional Security Verification**.</span></span>

   ![Välj ytterligare säkerhetskontroll – skärmbild](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. <span data-ttu-id="18b09-183">Välj **applösenord**.</span><span class="sxs-lookup"><span data-stu-id="18b09-183">Select **app passwords**.</span></span>

   ![Välj applösenord – skärmbild](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. <span data-ttu-id="18b09-185">Nästa toohello applösenord som du vill toodelete, klickar du på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="18b09-185">Next toohello app password you want toodelete, click **Delete**.</span></span>

   ![Ta bort ett applösenord](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. <span data-ttu-id="18b09-187">Bekräfta att du vill toodelete lösenordet genom att klicka på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="18b09-187">Confirm that you want toodelete that password by clicking **yes**.</span></span>
7. <span data-ttu-id="18b09-188">När hello applösenord har tagits bort, kan du klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="18b09-188">Once hello app password is deleted, you can click **close**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18b09-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18b09-189">Next steps</span></span>

- [<span data-ttu-id="18b09-190">Hantera dina inställningar för tvåstegsverifiering</span><span class="sxs-lookup"><span data-stu-id="18b09-190">Manage your two-step verification settings</span></span>](multi-factor-authentication-end-user-manage-settings.md)

- <span data-ttu-id="18b09-191">Testa hello [Microsoft Authenticator-appen](microsoft-authenticator-app-how-to.md) tooverify din inloggningar med app-meddelanden i stället för att ta emot texter eller samtal.</span><span class="sxs-lookup"><span data-stu-id="18b09-191">Try out hello [Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) tooverify your sign-ins with app notifications, instead of receiving texts or calls.</span></span> 
