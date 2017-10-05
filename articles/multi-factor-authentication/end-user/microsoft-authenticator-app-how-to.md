---
title: "Microsoft Authenticator-appen för mobila enheter | Microsoft Docs"
description: "Lär dig hur du uppgraderar till den senaste versionen av Azure Authenticator."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: H1Hack27Feb2017, end-user
ms.openlocfilehash: 6bcb6d9f7a1e9b241fa70690016b03d6eb5887ab
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a><span data-ttu-id="de365-103">Kom igång med Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="de365-103">Get started with the Microsoft Authenticator app</span></span>
<span data-ttu-id="de365-104">Ger en extra nivå av säkerhet i arbets-eller skolkonto för Microsoft Authenticator-appen (till exempel bsimon@contoso.com) eller Microsoft-konto (till exempel bsimon@outlook.com).</span><span class="sxs-lookup"><span data-stu-id="de365-104">The Microsoft Authenticator app provides an additional level of security in your work or school account (for example, bsimon@contoso.com) or your Microsoft account (for example, bsimon@outlook.com).</span></span>

<span data-ttu-id="de365-105">Appen inte fungerar i ett av två sätt:</span><span class="sxs-lookup"><span data-stu-id="de365-105">The app works in one of two ways:</span></span>

* <span data-ttu-id="de365-106">**Meddelande**.</span><span class="sxs-lookup"><span data-stu-id="de365-106">**Notification**.</span></span> <span data-ttu-id="de365-107">Appen kan du förhindra obehörig åtkomst till konton och stoppa olagliga transaktioner genom att skicka ett meddelande till din smartphone eller surfplatta.</span><span class="sxs-lookup"><span data-stu-id="de365-107">The app can help prevent unauthorized access to accounts and stop fraudulent transactions by pushing a notification to your smartphone or tablet.</span></span> <span data-ttu-id="de365-108">Bara visa meddelandet och om det är tillförlitligt, Välj **Kontrollera**.</span><span class="sxs-lookup"><span data-stu-id="de365-108">Simply view the notification, and if it's legitimate, select **Verify**.</span></span> <span data-ttu-id="de365-109">Välj annars **neka**.</span><span class="sxs-lookup"><span data-stu-id="de365-109">Otherwise, you can select **Deny**.</span></span> 
* <span data-ttu-id="de365-110">**Verifieringskoden**.</span><span class="sxs-lookup"><span data-stu-id="de365-110">**Verification code**.</span></span> <span data-ttu-id="de365-111">Appen kan användas som en programvarutoken för att generera en OAuth Verifieringskod.</span><span class="sxs-lookup"><span data-stu-id="de365-111">The app can be used as a software token to generate an OAuth verification code.</span></span> <span data-ttu-id="de365-112">När du har angett ditt användarnamn och lösenord koden du från appen till skärmen för inloggning.</span><span class="sxs-lookup"><span data-stu-id="de365-112">After you enter your username and password, you enter the code provided by the app into the sign-in screen.</span></span> <span data-ttu-id="de365-113">Verifieringskoden innehåller andra formen av autentisering.</span><span class="sxs-lookup"><span data-stu-id="de365-113">The verification code provides a second form of authentication.</span></span>

<span data-ttu-id="de365-114">Microsoft Authenticator-appen ersätter Azure Authenticator-appen.</span><span class="sxs-lookup"><span data-stu-id="de365-114">The Microsoft Authenticator app replaces the Azure Authenticator app.</span></span> <span data-ttu-id="de365-115">Azure Authenticator-appen fungerar fortfarande, men om du vill flytta till den nya Microsoft Authenticator-appen i den här artikeln kan hjälpa dig.</span><span class="sxs-lookup"><span data-stu-id="de365-115">The Azure Authenticator app still works, but if you decide to move to the new Microsoft Authenticator app, this article can assist you.</span></span>  

## <a name="opt-in-for-two-step-verification"></a><span data-ttu-id="de365-116">Välja tvåstegsverifiering</span><span class="sxs-lookup"><span data-stu-id="de365-116">Opt in for two-step verification</span></span>

<span data-ttu-id="de365-117">Microsoft Authenticator-appen fungerar inte med sig själv.</span><span class="sxs-lookup"><span data-stu-id="de365-117">The Microsoft Authenticator app doesn't work by itself.</span></span> <span data-ttu-id="de365-118">Konfigurera var och en av dina konton för att begära en andra verifieringsmetod när du loggar in med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="de365-118">Configure each of your accounts to prompt you for a second verification method after you sign in with your username and password.</span></span> 

<span data-ttu-id="de365-119">För ett konto för arbetet eller skolan dig inte vanligtvis att välja den här funktionen för dig själv.</span><span class="sxs-lookup"><span data-stu-id="de365-119">For a work or school account, you don't usually get to choose this feature for yourself.</span></span> <span data-ttu-id="de365-120">I stället en administratör väljer i den för din räkning och meddelar dig att registrera verifieringsmetoderna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="de365-120">Instead, a security administrator opts in on your behalf and then notifies you to register verification methods for your account.</span></span> <span data-ttu-id="de365-121">Om det här scenariot gäller för dig, Läs mer i [vad Azure Multi-Factor Authentication innebär för mig](multi-factor-authentication-end-user.md).</span><span class="sxs-lookup"><span data-stu-id="de365-121">If this scenario applies to you, learn more in [What does Azure Multi-Factor Authentication mean for me](multi-factor-authentication-end-user.md).</span></span>

<span data-ttu-id="de365-122">Du måste ställa in tvåstegsverifiering själv för ett personligt konto.</span><span class="sxs-lookup"><span data-stu-id="de365-122">For a personal account, you need to set up two-step verification for yourself.</span></span> <span data-ttu-id="de365-123">Om du har ett Microsoft-konto kan de här stegen finns i [om tvåstegsverifiering](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span><span class="sxs-lookup"><span data-stu-id="de365-123">If you have a Microsoft account, those steps are available in [About two-step verification](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span></span> 

<span data-ttu-id="de365-124">Du kan också använda Microsoft Authenticator med icke-Microsoft konton.</span><span class="sxs-lookup"><span data-stu-id="de365-124">You can also use the Microsoft Authenticator with non-Microsoft accounts.</span></span> <span data-ttu-id="de365-125">De kan anropa funktionen något annat än tvåstegsverifiering, men du bör kunna hitta under inloggning inställningar eller säkerhet.</span><span class="sxs-lookup"><span data-stu-id="de365-125">They may call the feature something other than two-step verification, but you should be able to find it under security or sign-in settings.</span></span> 

## <a name="install-the-app"></a><span data-ttu-id="de365-126">Installera appen</span><span class="sxs-lookup"><span data-stu-id="de365-126">Install the app</span></span>
<span data-ttu-id="de365-127">Microsoft Authenticator-appen är tillgänglig för [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), och [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).</span><span class="sxs-lookup"><span data-stu-id="de365-127">The Microsoft Authenticator app is available for [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), and [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).</span></span>

## <a name="add-accounts-to-the-app"></a><span data-ttu-id="de365-128">Lägga till konton till appen</span><span class="sxs-lookup"><span data-stu-id="de365-128">Add accounts to the app</span></span>
<span data-ttu-id="de365-129">Använd någon av följande procedurer för varje konto som du vill lägga till Microsoft Authenticator-appen:</span><span class="sxs-lookup"><span data-stu-id="de365-129">For each account that you want to add to the Microsoft Authenticator app, use one of the following procedures:</span></span>

### <a name="add-a-personal-microsoft-account-to-the-app"></a><span data-ttu-id="de365-130">Lägg till ett personligt microsoftkonto i appen</span><span class="sxs-lookup"><span data-stu-id="de365-130">Add a personal Microsoft account to the app</span></span>

<span data-ttu-id="de365-131">För ett personligt microsoftkonto (en som används för att logga in på Outlook.com, Xbox, Skype, etc.), är du behöver logga in på ditt konto i Microsoft Authenticator-appen.</span><span class="sxs-lookup"><span data-stu-id="de365-131">For a personal Microsoft account (one that you use to sign in to Outlook.com, Xbox, Skype, etc.), all you have to do is sign in to your account in the Microsoft Authenticator app.</span></span>

### <a name="add-a-work-or-school-account-to-the-app-using-the-qr-code-scanner"></a><span data-ttu-id="de365-132">Lägg till ett arbets- eller skolkonto konto i appen med skanner för QR-kod</span><span class="sxs-lookup"><span data-stu-id="de365-132">Add a work or school account to the app using the QR code scanner</span></span>
1. <span data-ttu-id="de365-133">Gå till skärmen för säkerhetsinställningar verifiering.</span><span class="sxs-lookup"><span data-stu-id="de365-133">Go to the security verification settings screen.</span></span>  <span data-ttu-id="de365-134">Mer information om hur du kommer till den här skärmen finns [ändra säkerhetsinställningarna](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).</span><span class="sxs-lookup"><span data-stu-id="de365-134">For information on how to get to this screen, see [Changing your security settings](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).</span></span>
2. <span data-ttu-id="de365-135">Markera kryssrutan bredvid **autentiseringsapp** Välj **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="de365-135">Check the box next to **Authenticator app** then select **Configure**.</span></span>

    ![Knappen Konfigurera på skärmen för säkerhetsinställningar verifiering](./media/authenticator-app-how-to/azureauthe.png)

    <span data-ttu-id="de365-137">Nu visas en skärm med en QR-kod på den.</span><span class="sxs-lookup"><span data-stu-id="de365-137">This brings up a screen with a QR code on it.</span></span>

    ![Skärmen som ger QR-kod](./media/authenticator-app-how-to/barcode2.png)
3. <span data-ttu-id="de365-139">Öppna Microsoft Authenticator-appen.</span><span class="sxs-lookup"><span data-stu-id="de365-139">Open the Microsoft Authenticator app.</span></span> <span data-ttu-id="de365-140">På den **konton** väljer  **+** , och sedan ange att du vill lägga till ett konto för arbetet eller skolan.</span><span class="sxs-lookup"><span data-stu-id="de365-140">On the **accounts** screen, select **+**, and then specify that you want to add a work or school account.</span></span>
4. <span data-ttu-id="de365-141">Använd kameran för att skanna QR-koden och välj sedan **klar** att stänga skärmen QR-kod.</span><span class="sxs-lookup"><span data-stu-id="de365-141">Use the camera to scan the QR code, and then select **Done** to close the QR code screen.</span></span>

    <span data-ttu-id="de365-142">Om kameran inte fungerar kan du [ange QR-koden och Webbadressen manuellt](#add-an-account-to-the-app-manually).</span><span class="sxs-lookup"><span data-stu-id="de365-142">If your camera is not working properly, you can [enter the QR code and URL manually](#add-an-account-to-the-app-manually).</span></span>

5. <span data-ttu-id="de365-143">När appen visas namnet på ditt konto med en sexsiffrig kod under den är du klar.</span><span class="sxs-lookup"><span data-stu-id="de365-143">When the app shows your account name with a six-digit code underneath it, you're done.</span></span> 

    ![Konton skärmen](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a><span data-ttu-id="de365-145">Lägga till ett konto i appen manuellt</span><span class="sxs-lookup"><span data-stu-id="de365-145">Add an account to the app manually</span></span>
1. <span data-ttu-id="de365-146">Gå till skärmen för säkerhetsinställningar verifiering.</span><span class="sxs-lookup"><span data-stu-id="de365-146">Go to the security verification settings screen.</span></span>  <span data-ttu-id="de365-147">Mer information om hur du kommer till den här skärmen finns [ändra säkerhetsinställningarna](multi-factor-authentication-end-user-manage-settings.md).</span><span class="sxs-lookup"><span data-stu-id="de365-147">For information on how to get to this screen, see [Changing your security settings](multi-factor-authentication-end-user-manage-settings.md).</span></span>
2. <span data-ttu-id="de365-148">Välj **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="de365-148">Select **Configure**.</span></span>

    ![Knappen Konfigurera på skärmen för säkerhetsinställningar verifiering](./media/authenticator-app-how-to/azureauthe.png)

    <span data-ttu-id="de365-150">Nu visas en skärm med en QR-kod på den.</span><span class="sxs-lookup"><span data-stu-id="de365-150">This brings up a screen with a QR code on it.</span></span>  <span data-ttu-id="de365-151">Observera att koden och Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="de365-151">Note the code and URL.</span></span>

    ![Skärmen som ger en QR-kod och URL](./media/authenticator-app-how-to/barcode2.png)
3. <span data-ttu-id="de365-153">Öppna Microsoft Authenticator-appen.</span><span class="sxs-lookup"><span data-stu-id="de365-153">Open the Microsoft Authenticator app.</span></span> <span data-ttu-id="de365-154">På den **konton** väljer  **+** , och sedan ange att du vill lägga till ett konto för arbetet eller skolan.</span><span class="sxs-lookup"><span data-stu-id="de365-154">On the **accounts** screen, select **+**, and then specify that you want to add a work or school account.</span></span>

4. <span data-ttu-id="de365-155">Välj i skannern **ange koden manuellt**.</span><span class="sxs-lookup"><span data-stu-id="de365-155">In the scanner, select **enter code manually**.</span></span>

    ![Skärmen för att skanna QR-kod](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. <span data-ttu-id="de365-157">Ange koden och URL-Adressen i motsvarande rutor i appen och välj sedan **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="de365-157">Enter the code and the URL in the appropriate boxes in the app, then select **Finish**.</span></span>

    ![Skärmen för att ange koden och Webbadressen](./media/authenticator-app-how-to/manual.png)

6. <span data-ttu-id="de365-159">När appen visas namnet på ditt konto med en sexsiffrig kod under den är du klar.</span><span class="sxs-lookup"><span data-stu-id="de365-159">When the app shows your account name with a six-digit code underneath it, you're done.</span></span>

    ![Konton skärmen](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-using-touch-id"></a><span data-ttu-id="de365-161">Lägg till ett konto i appen med Touch ID</span><span class="sxs-lookup"><span data-stu-id="de365-161">Add an account to the app using Touch ID</span></span>
<span data-ttu-id="de365-162">Microsoft Authenticator-appen på iOS stöder Touch ID.</span><span class="sxs-lookup"><span data-stu-id="de365-162">The Microsoft Authenticator app on iOS supports Touch ID.</span></span>  <span data-ttu-id="de365-163">Azure Multi-Factor Authentication gör att organisationer kan kräva en PIN-kod för enheter.</span><span class="sxs-lookup"><span data-stu-id="de365-163">Azure Multi-Factor Authentication allows organizations to require a PIN for devices.</span></span> <span data-ttu-id="de365-164">Med Touch ID behöver iOS-användare inte ange en PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="de365-164">With Touch ID, iOS users don’t need to enter a PIN.</span></span> <span data-ttu-id="de365-165">De kan i stället söka igenom sitt fingeravtryck och välj **Godkänn**.</span><span class="sxs-lookup"><span data-stu-id="de365-165">Instead, they can scan their fingerprint and select **Approve**.</span></span>

<span data-ttu-id="de365-166">Det är enkelt att ställa in Touch ID med Microsoft Authenticator.</span><span class="sxs-lookup"><span data-stu-id="de365-166">Setting up Touch ID with Microsoft Authenticator is simple.</span></span> <span data-ttu-id="de365-167">Du har slutfört en normal verifiering utmaning med en PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="de365-167">You complete a normal verification challenge with a PIN.</span></span> <span data-ttu-id="de365-168">Om enheten har stöd för Touch ID, konfigurerar Microsoft Authenticator den automatiskt för det kontot.</span><span class="sxs-lookup"><span data-stu-id="de365-168">If your device supports Touch ID, Microsoft Authenticator sets it up automatically for that account.</span></span>

![Verifiering av installationsprogrammet för Touch ID](./media/authenticator-app-how-to/touchid1.png)

<span data-ttu-id="de365-170">Peka framåt från som, när du måste verifiera din inloggning, väljer mottagna push-meddelanden och söka igenom ditt fingeravtryck istället för att ange din PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="de365-170">From that point forward, when you're required to verify your sign-in, you select the received push notification and scan your fingerprint instead of entering your PIN.</span></span>

![Push-meddelanden](./media/authenticator-app-how-to/touchid2.png)

## <a name="use-the-app-when-you-sign-in"></a><span data-ttu-id="de365-172">Använda appen när du loggar in</span><span class="sxs-lookup"><span data-stu-id="de365-172">Use the app when you sign in</span></span>

<span data-ttu-id="de365-173">När ditt konto har lagts till i appen, uppmanas du att göra en test-verifiering för att kontrollera att allt är korrekt konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="de365-173">Once your account is added to the app, you may be prompted to do a test verification to make sure everything was configured correctly.</span></span> <span data-ttu-id="de365-174">När är du klar!</span><span class="sxs-lookup"><span data-stu-id="de365-174">After that, you're done!</span></span> <span data-ttu-id="de365-175">Du behöver inte göra något annat förrän nästa gång du loggar in.</span><span class="sxs-lookup"><span data-stu-id="de365-175">You don't need to do anything else until the next time you sign in.</span></span>

<span data-ttu-id="de365-176">Om du väljer att använda verifieringskoder i appen du vill visa dem på startsidan.</span><span class="sxs-lookup"><span data-stu-id="de365-176">If you chose to use verification codes in the app, you start to see them on the home page.</span></span> <span data-ttu-id="de365-177">De kan ändra var 30: e sekund så att du alltid har en ny kod när du behöver en.</span><span class="sxs-lookup"><span data-stu-id="de365-177">They change every 30 seconds so that you always have a new code when you need one.</span></span> <span data-ttu-id="de365-178">Men du behöver inte göra något med dem förrän du logga in och uppmanas att ange en Verifieringskod.</span><span class="sxs-lookup"><span data-stu-id="de365-178">But you don't need to do anything with them until you sign in and are prompted to enter a verification code.</span></span>  