---
title: "Hur du använder Applösenord i Azure MFA? | Microsoft Docs"
description: "Den här sidan hjälper användarna att förstå vad applösenord är och vad de används med hänsyn till Azure MFA."
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
ms.openlocfilehash: 1ecc2bdef5ff7ef8ed8dded7dc12428ce9657821
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a><span data-ttu-id="dccf9-104">Vad är Applösenord i Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="dccf9-104">What are App Passwords in Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="dccf9-105">Vissa icke-webbläsarappar, till exempel Apple interna e-klienten som använder Exchange Active Sync stöder för tillfället inte multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="dccf9-105">Certain non-browser apps, such as the Apple native email client that uses Exchange Active Sync, currently do not support multi-factor authentication.</span></span> <span data-ttu-id="dccf9-106">Multifaktorautentisering aktiveras per användare.</span><span class="sxs-lookup"><span data-stu-id="dccf9-106">Multi-factor authentication is enabled per user.</span></span>  <span data-ttu-id="dccf9-107">Detta innebär att en användare inte kan använda multifaktorautentisering om:</span><span class="sxs-lookup"><span data-stu-id="dccf9-107">This means that a user can't use multi-factor authentication if:</span></span>

- <span data-ttu-id="dccf9-108">Användaren har aktiverats för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="dccf9-108">The user has been enabled for multi-factor authentication</span></span>
- <span data-ttu-id="dccf9-109">Användaren försöker använda en icke-Webbläsarprogrammet.</span><span class="sxs-lookup"><span data-stu-id="dccf9-109">The user is trying to use a non-browser app.</span></span>

<span data-ttu-id="dccf9-110">Ett applösenord gör att användaren kan använda appen.</span><span class="sxs-lookup"><span data-stu-id="dccf9-110">An app password allows the user to use the app.</span></span>

<span data-ttu-id="dccf9-111">När du har ett applösenord kan använda du den i stället för det ursprungliga lösenordet med dessa icke-webbläsarbaserade appar.</span><span class="sxs-lookup"><span data-stu-id="dccf9-111">Once you have an app password, you use it in place of your original password with these non-browser apps.</span></span> <span data-ttu-id="dccf9-112">När du registrerar dig för tvåstegsverifiering du talar om Microsoft inte att vem som helst logga in med ditt lösenord om de inte kan också utföra andra verifieringen.</span><span class="sxs-lookup"><span data-stu-id="dccf9-112">When you register for two-step verification, you're telling Microsoft not to let anyone sign in with your password if they can't also perform the second verification.</span></span> <span data-ttu-id="dccf9-113">Apple interna e-postklienten på telefonen logga inte in dig eftersom den inte kan fråga efter tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="dccf9-113">The Apple native email client on your phone can't sign in as you because it can't ask for two-step verification.</span></span> <span data-ttu-id="dccf9-114">Lösning på problemet är att skapa en säkrare applösenord som du inte använder dagliga.</span><span class="sxs-lookup"><span data-stu-id="dccf9-114">The solution to this problem is to create a more secure app password that you don't use day-to-day.</span></span> <span data-ttu-id="dccf9-115">Applösenord gäller endast de appar som inte stöder tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="dccf9-115">App passwords are only for those apps that can't support two-step verification.</span></span> <span data-ttu-id="dccf9-116">Använd applösenordet så att appar kan hoppa över multifaktorautentisering och fortsätta att fungera.</span><span class="sxs-lookup"><span data-stu-id="dccf9-116">Use the app password so that apps can bypass multi-factor authentication and continue to work.</span></span>


> [!NOTE]
> <span data-ttu-id="dccf9-117">Office 2013-klienter (inklusive Outlook) stöder nya autentiseringsprotokoll och kan användas med tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="dccf9-117">Office 2013 clients (including Outlook) support new authentication protocols and can be used with two-step verification.</span></span> <span data-ttu-id="dccf9-118">Applösenord krävs inte för användning med Office 2013-klienter.</span><span class="sxs-lookup"><span data-stu-id="dccf9-118">App passwords are not required for use with Office 2013 clients.</span></span>  <span data-ttu-id="dccf9-119">Mer information finns i [Office 2013 modern autentisering offentlig förhandsgranskning av](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span><span class="sxs-lookup"><span data-stu-id="dccf9-119">For more information, see [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span></span>


## <a name="how-to-use-app-passwords"></a><span data-ttu-id="dccf9-120">Använda applösenord</span><span class="sxs-lookup"><span data-stu-id="dccf9-120">How to use app passwords</span></span>
<span data-ttu-id="dccf9-121">Här följer några saker du behöver veta om applösenord:</span><span class="sxs-lookup"><span data-stu-id="dccf9-121">Here are some things to know about app passwords:</span></span>

* <span data-ttu-id="dccf9-122">Du skapa inte egna applösenord.</span><span class="sxs-lookup"><span data-stu-id="dccf9-122">You don't create your own app passwords.</span></span> <span data-ttu-id="dccf9-123">De genereras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="dccf9-123">They are automatically generated.</span></span>
* <span data-ttu-id="dccf9-124">Det finns för närvarande en gräns på 40 lösenord per användare.</span><span class="sxs-lookup"><span data-stu-id="dccf9-124">Currently there is a limit of 40 passwords per user.</span></span> 
* <span data-ttu-id="dccf9-125">Om du försöker skapa ett applösenord när gränsen har uppnåtts, har du ta bort en av dina befintliga applösenord innan du skapar en ny.</span><span class="sxs-lookup"><span data-stu-id="dccf9-125">If you try to create an app password after you have reached the limit, you'll have to delete one of your existing app passwords before you create a new one.</span></span>
* <span data-ttu-id="dccf9-126">Använda ett applösenord per enhet, inte per program.</span><span class="sxs-lookup"><span data-stu-id="dccf9-126">Use one app password per device, not per application.</span></span> <span data-ttu-id="dccf9-127">Du kan till exempel skapa ett applösenord för din bärbara dator och använda det applösenordet för alla program på den bärbara datorn.</span><span class="sxs-lookup"><span data-stu-id="dccf9-127">For example, you can create one app password for your laptop and use that app password for all of your applications on that laptop.</span></span> <span data-ttu-id="dccf9-128">Skapa sedan en andra lösenord ska användas för alla dina appar på ditt skrivbord.</span><span class="sxs-lookup"><span data-stu-id="dccf9-128">Then, create a second app password to use for all your apps on your desktop.</span></span> 
* <span data-ttu-id="dccf9-129">Du får ett applösenord första gången du registrerar dig för tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="dccf9-129">You are given one app password the first time you register for two-step verification.</span></span>  <span data-ttu-id="dccf9-130">Om du behöver ytterligare mallar kan skapa du dem.</span><span class="sxs-lookup"><span data-stu-id="dccf9-130">If you need additional ones, you can create them.</span></span>



## <a name="creating-and-deleting-app-passwords"></a><span data-ttu-id="dccf9-131">Skapa och ta bort applösenord</span><span class="sxs-lookup"><span data-stu-id="dccf9-131">Creating and deleting app passwords</span></span>
<span data-ttu-id="dccf9-132">Vid första inloggningen får du ett applösenord som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="dccf9-132">During your initial sign-in, you are given an app password that you can use.</span></span>  <span data-ttu-id="dccf9-133">Du kan också skapa och ta bort applösenord vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="dccf9-133">You can also create and delete app passwords later on.</span></span> <span data-ttu-id="dccf9-134">Hur du tar bort applösenord beror på hur du använder multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="dccf9-134">How you delete app passwords depends on how you use multi-factor authentication.</span></span> <span data-ttu-id="dccf9-135">Besvara följande frågor för att avgöra var du ska gå för att hantera applösenord:</span><span class="sxs-lookup"><span data-stu-id="dccf9-135">Answer the following questions to determine where you should go to manage app passwords:</span></span> 

1. <span data-ttu-id="dccf9-136">Använder du tvåstegsverifiering för ditt personliga Microsoft-konto?</span><span class="sxs-lookup"><span data-stu-id="dccf9-136">Do you use two-step verification for your personal Microsoft account?</span></span> <span data-ttu-id="dccf9-137">Om Ja, du måste referera till den [applösenord och tvåstegsverifiering](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) artikeln för hjälp.</span><span class="sxs-lookup"><span data-stu-id="dccf9-137">If yes, you should refer to the [App passwords and two-step verification](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) article for help.</span></span> <span data-ttu-id="dccf9-138">Om Nej, fortsätta att fråga två.</span><span class="sxs-lookup"><span data-stu-id="dccf9-138">If no, continue to question two.</span></span>

2. <span data-ttu-id="dccf9-139">OK så att du kan använda tvåstegsverifiering för ditt arbets- eller skolkonto konto.</span><span class="sxs-lookup"><span data-stu-id="dccf9-139">Ok, so you use two-step verification for your work or school account.</span></span> <span data-ttu-id="dccf9-140">Du använder den för att logga in på Office 365-appar?</span><span class="sxs-lookup"><span data-stu-id="dccf9-140">Do you use it to sign in to Office 365 apps?</span></span> <span data-ttu-id="dccf9-141">Om Ja, bör du gå till [skapa ett applösenord för Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) för hjälp.</span><span class="sxs-lookup"><span data-stu-id="dccf9-141">If yes, you should refer to [Create an app password for Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) for help.</span></span> <span data-ttu-id="dccf9-142">Om Nej, Fortsätt till fråga tre.</span><span class="sxs-lookup"><span data-stu-id="dccf9-142">If no, continue to question three.</span></span> 

3. <span data-ttu-id="dccf9-143">Använder du tvåstegsverifiering med Microsoft Azure?</span><span class="sxs-lookup"><span data-stu-id="dccf9-143">Do you use two-step verification with Microsoft Azure?</span></span> <span data-ttu-id="dccf9-144">Om Ja, fortsätter du till den [hantera lösenord i Azure portal](#manage-app-passwords-in-the-Azure-portal) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dccf9-144">If yes, continue to the [Manage app passwords in the Azure portal](#manage-app-passwords-in-the-Azure-portal) section of this article.</span></span> <span data-ttu-id="dccf9-145">Om Nej, fortsätta att fråga fyra.</span><span class="sxs-lookup"><span data-stu-id="dccf9-145">If no, continue to question four.</span></span>

4. <span data-ttu-id="dccf9-146">Osäker på om du använder tvåstegsverifiering?</span><span class="sxs-lookup"><span data-stu-id="dccf9-146">Not sure where you use two-step verification?</span></span> <span data-ttu-id="dccf9-147">Fortsätta att den [hantera applösenord med portalen MyApps](#manage-app-passwords-with-the-myapps-portal) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dccf9-147">Continue to the [Manage app passwords with the MyApps portal](#manage-app-passwords-with-the-myapps-portal) section of this article.</span></span> 


## <a name="manage-app-passwords-in-the-azure-portal"></a><span data-ttu-id="dccf9-148">Hantera lösenord i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dccf9-148">Manage app passwords in the Azure portal</span></span>
<span data-ttu-id="dccf9-149">Om du använder tvåstegsverifiering med Azure som du vill skapa applösenord via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dccf9-149">If you use two-step verification with Azure, you want to create app passwords through the Azure portal.</span></span>

### <a name="to-create-app-passwords-in-the-azure-portal"></a><span data-ttu-id="dccf9-150">Skapa applösenord i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dccf9-150">To create app passwords in the Azure portal</span></span>
1. <span data-ttu-id="dccf9-151">Logga in på den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dccf9-151">Sign in to the Azure classic portal.</span></span>
2. <span data-ttu-id="dccf9-152">Högerklicka på ditt användarnamn längst upp och välj ytterligare säkerhetskontroll.</span><span class="sxs-lookup"><span data-stu-id="dccf9-152">At the top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="dccf9-153">På sidan proofup överst och välj applösenord</span><span class="sxs-lookup"><span data-stu-id="dccf9-153">On the proofup page, at the top, select app passwords</span></span>
4. <span data-ttu-id="dccf9-154">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-154">Click **Create**.</span></span>
5. <span data-ttu-id="dccf9-155">Ange ett namn för applösenordet och klicka på **nästa**</span><span class="sxs-lookup"><span data-stu-id="dccf9-155">Enter a name for the app password and click **Next**</span></span>
6. <span data-ttu-id="dccf9-156">Kopiera applösenordet till Urklipp och klistrar in den i din app.</span><span class="sxs-lookup"><span data-stu-id="dccf9-156">Copy the app password to the clipboard and paste it into your app.</span></span>
   
   ![Molnet](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="to-delete-app-passwords-in-the-azure-portal"></a><span data-ttu-id="dccf9-158">Ta bort applösenord i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dccf9-158">To delete app passwords in the Azure portal</span></span>
1. <span data-ttu-id="dccf9-159">Logga in på den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dccf9-159">Sign in to the Azure classic portal.</span></span>
2. <span data-ttu-id="dccf9-160">Högerklicka på ditt användarnamn längst upp och välj ytterligare säkerhetskontroll.</span><span class="sxs-lookup"><span data-stu-id="dccf9-160">At the top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="dccf9-161">Markera överst bredvid ytterligare säkerhetsverifiering **applösenord.**</span><span class="sxs-lookup"><span data-stu-id="dccf9-161">At the top, next to additional security verification, select **app passwords.**</span></span>
4. <span data-ttu-id="dccf9-162">Bredvid applösenord som du vill ta bort, Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-162">Next to the app password you want to delete, select **Delete**.</span></span>
5. <span data-ttu-id="dccf9-163">Bekräfta borttagningen genom att klicka på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-163">Confirm the deletion by clicking **yes**.</span></span>
6. <span data-ttu-id="dccf9-164">När applösenordet har tagits bort, kan du klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-164">Once the app password is deleted, you can click **close**.</span></span>


## <a name="manage-app-passwords-with-the-myapps-portal"></a><span data-ttu-id="dccf9-165">Hantera applösenord med MyApps-portalen.</span><span class="sxs-lookup"><span data-stu-id="dccf9-165">Manage app passwords with the MyApps portal.</span></span>
<span data-ttu-id="dccf9-166">Om du inte är säker på hur du använder multi-Factor authentication kan sedan du alltid skapa och ta bort applösenord myapps-portalen.</span><span class="sxs-lookup"><span data-stu-id="dccf9-166">If you are not sure how you use multi-factor authentication, then you can always create and delete app passwords through the myapps portal.</span></span>

### <a name="to-create-an-app-password-using-the-myapps-portal"></a><span data-ttu-id="dccf9-167">Skapa ett applösenord med hjälp av Myapps-portalen</span><span class="sxs-lookup"><span data-stu-id="dccf9-167">To create an app password using the Myapps portal</span></span>
1. <span data-ttu-id="dccf9-168">Logga in på [https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="dccf9-168">Sign in to [https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="dccf9-169">Klicka på ditt namn längst upp till höger och välj **profil**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-169">Click your name at the top right, and choose **Profile**.</span></span>
3. <span data-ttu-id="dccf9-170">Välj **ytterligare säkerhetsverifiering**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-170">Select **Additional Security Verification**.</span></span>
   <span data-ttu-id="dccf9-171">![Välj ytterligare säkerhetskontroll – skärmbild](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span><span class="sxs-lookup"><span data-stu-id="dccf9-171">![Select additional security verification - screenshot](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span></span>

4. <span data-ttu-id="dccf9-172">Välj **applösenord**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-172">Select **app passwords**.</span></span>
   <span data-ttu-id="dccf9-173">![Välj applösenord – skärmbild](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span><span class="sxs-lookup"><span data-stu-id="dccf9-173">![Select app passwords - screenshot](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span></span>

5. <span data-ttu-id="dccf9-174">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-174">Click **Create**.</span></span>
6. <span data-ttu-id="dccf9-175">Ange ett namn för applösenordet och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-175">Enter a name for the app password and click **Next**.</span></span>
7. <span data-ttu-id="dccf9-176">Kopiera applösenordet till Urklipp och klistrar in den i din app.</span><span class="sxs-lookup"><span data-stu-id="dccf9-176">Copy the app password to the clipboard and paste it into your app.</span></span>
   <span data-ttu-id="dccf9-177">![Skapa ett applösenord](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span><span class="sxs-lookup"><span data-stu-id="dccf9-177">![Create an app password](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span></span>

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a><span data-ttu-id="dccf9-178">Ta bort ett applösenord med hjälp av Myapps-portalen</span><span class="sxs-lookup"><span data-stu-id="dccf9-178">To delete an app password using the Myapps portal</span></span>
1. <span data-ttu-id="dccf9-179">Logga in på [https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="dccf9-179">Sign in to [https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="dccf9-180">Välj profil överst.</span><span class="sxs-lookup"><span data-stu-id="dccf9-180">At the top, select profile.</span></span>
3. <span data-ttu-id="dccf9-181">Välj **ytterligare säkerhetsverifiering**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-181">Select **Additional Security Verification**.</span></span>

   ![Välj ytterligare säkerhetskontroll – skärmbild](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. <span data-ttu-id="dccf9-183">Välj **applösenord**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-183">Select **app passwords**.</span></span>

   ![Välj applösenord – skärmbild](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. <span data-ttu-id="dccf9-185">Bredvid applösenord som du vill ta bort, klickar du på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-185">Next to the app password you want to delete, click **Delete**.</span></span>

   ![Ta bort ett applösenord](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. <span data-ttu-id="dccf9-187">Bekräfta att du vill ta bort lösenordet genom att klicka på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-187">Confirm that you want to delete that password by clicking **yes**.</span></span>
7. <span data-ttu-id="dccf9-188">När applösenordet har tagits bort, kan du klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="dccf9-188">Once the app password is deleted, you can click **close**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dccf9-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dccf9-189">Next steps</span></span>

- [<span data-ttu-id="dccf9-190">Hantera dina inställningar för tvåstegsverifiering</span><span class="sxs-lookup"><span data-stu-id="dccf9-190">Manage your two-step verification settings</span></span>](multi-factor-authentication-end-user-manage-settings.md)

- <span data-ttu-id="dccf9-191">Testa den [Microsoft Authenticator-appen](microsoft-authenticator-app-how-to.md) att verifiera din inloggningar med app-meddelanden i stället för att ta emot texter eller samtal.</span><span class="sxs-lookup"><span data-stu-id="dccf9-191">Try out the [Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) to verify your sign-ins with app notifications, instead of receiving texts or calls.</span></span> 
