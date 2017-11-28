---
title: "aaaMicrosoft Authenticator-appen för mobila enheter | Microsoft Docs"
description: "Lär dig hur tooupgrade toohello senaste versionen av Azure Authenticator."
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
ms.openlocfilehash: d895d92d89613cbafd9fc09d4ff4810cbf25652e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-microsoft-authenticator-app"></a><span data-ttu-id="d19bb-103">Kom igång med hello Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="d19bb-103">Get started with hello Microsoft Authenticator app</span></span>
<span data-ttu-id="d19bb-104">hello Microsoft Authenticator-appen ger en extra nivå av säkerhet i arbets-eller skolkonto (t.ex, bsimon@contoso.com) eller Microsoft-konto (till exempel bsimon@outlook.com).</span><span class="sxs-lookup"><span data-stu-id="d19bb-104">hello Microsoft Authenticator app provides an additional level of security in your work or school account (for example, bsimon@contoso.com) or your Microsoft account (for example, bsimon@outlook.com).</span></span>

<span data-ttu-id="d19bb-105">hello appen fungerar på ett av två sätt:</span><span class="sxs-lookup"><span data-stu-id="d19bb-105">hello app works in one of two ways:</span></span>

* <span data-ttu-id="d19bb-106">**Meddelande**.</span><span class="sxs-lookup"><span data-stu-id="d19bb-106">**Notification**.</span></span> <span data-ttu-id="d19bb-107">hello app kan du förhindra obehörig åtkomst tooaccounts och stoppa olagliga transaktioner genom att skicka en avisering tooyour smartphone eller surfplatta.</span><span class="sxs-lookup"><span data-stu-id="d19bb-107">hello app can help prevent unauthorized access tooaccounts and stop fraudulent transactions by pushing a notification tooyour smartphone or tablet.</span></span> <span data-ttu-id="d19bb-108">Visa bara hello-meddelande och om det är tillförlitligt, Välj **Kontrollera**.</span><span class="sxs-lookup"><span data-stu-id="d19bb-108">Simply view hello notification, and if it's legitimate, select **Verify**.</span></span> <span data-ttu-id="d19bb-109">Välj annars **neka**.</span><span class="sxs-lookup"><span data-stu-id="d19bb-109">Otherwise, you can select **Deny**.</span></span> 
* <span data-ttu-id="d19bb-110">**Verifieringskoden**.</span><span class="sxs-lookup"><span data-stu-id="d19bb-110">**Verification code**.</span></span> <span data-ttu-id="d19bb-111">hello app kan användas som en programuppdatering token toogenerate en OAuth Verifieringskod.</span><span class="sxs-lookup"><span data-stu-id="d19bb-111">hello app can be used as a software token toogenerate an OAuth verification code.</span></span> <span data-ttu-id="d19bb-112">När du har angett ditt användarnamn och lösenord anger hello koden som tillhandahålls av hello app till hello på inloggningsskärmen.</span><span class="sxs-lookup"><span data-stu-id="d19bb-112">After you enter your username and password, you enter hello code provided by hello app into hello sign-in screen.</span></span> <span data-ttu-id="d19bb-113">hello Verifieringskod innehåller andra formen av autentisering.</span><span class="sxs-lookup"><span data-stu-id="d19bb-113">hello verification code provides a second form of authentication.</span></span>

<span data-ttu-id="d19bb-114">hello Microsoft Authenticator-appen ersätter hello Azure Authenticator-appen.</span><span class="sxs-lookup"><span data-stu-id="d19bb-114">hello Microsoft Authenticator app replaces hello Azure Authenticator app.</span></span> <span data-ttu-id="d19bb-115">hello Azure Authenticator-appen fungerar fortfarande, men om du väljer toomove toohello nya Microsoft Authenticator-appen, den här artikeln kan hjälpa dig.</span><span class="sxs-lookup"><span data-stu-id="d19bb-115">hello Azure Authenticator app still works, but if you decide toomove toohello new Microsoft Authenticator app, this article can assist you.</span></span>  

## <a name="opt-in-for-two-step-verification"></a><span data-ttu-id="d19bb-116">Välja tvåstegsverifiering</span><span class="sxs-lookup"><span data-stu-id="d19bb-116">Opt in for two-step verification</span></span>

<span data-ttu-id="d19bb-117">hello Microsoft Authenticator-appen fungerar inte med sig själv.</span><span class="sxs-lookup"><span data-stu-id="d19bb-117">hello Microsoft Authenticator app doesn't work by itself.</span></span> <span data-ttu-id="d19bb-118">Konfigurera var och en av dina konton tooprompt du för en andra verifieringsmetod när du loggar in med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="d19bb-118">Configure each of your accounts tooprompt you for a second verification method after you sign in with your username and password.</span></span> 

<span data-ttu-id="d19bb-119">För ett arbets- eller skolkonto konto får du inte vanligtvis toochoose funktionen själv.</span><span class="sxs-lookup"><span data-stu-id="d19bb-119">For a work or school account, you don't usually get toochoose this feature for yourself.</span></span> <span data-ttu-id="d19bb-120">I stället måste en administratör väljer i den för din räkning och meddelar dig tooregister verifieringsmetoderna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="d19bb-120">Instead, a security administrator opts in on your behalf and then notifies you tooregister verification methods for your account.</span></span> <span data-ttu-id="d19bb-121">Om det här scenariot gäller tooyou, Läs mer i [vad Azure Multi-Factor Authentication innebär för mig](multi-factor-authentication-end-user.md).</span><span class="sxs-lookup"><span data-stu-id="d19bb-121">If this scenario applies tooyou, learn more in [What does Azure Multi-Factor Authentication mean for me](multi-factor-authentication-end-user.md).</span></span>

<span data-ttu-id="d19bb-122">För ett personligt konto behöver du tooset tvåstegsverifiering själv.</span><span class="sxs-lookup"><span data-stu-id="d19bb-122">For a personal account, you need tooset up two-step verification for yourself.</span></span> <span data-ttu-id="d19bb-123">Om du har ett Microsoft-konto kan de här stegen finns i [om tvåstegsverifiering](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span><span class="sxs-lookup"><span data-stu-id="d19bb-123">If you have a Microsoft account, those steps are available in [About two-step verification](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span></span> 

<span data-ttu-id="d19bb-124">Du kan också använda hello Microsoft Authenticator med icke-Microsoft konton.</span><span class="sxs-lookup"><span data-stu-id="d19bb-124">You can also use hello Microsoft Authenticator with non-Microsoft accounts.</span></span> <span data-ttu-id="d19bb-125">De kan anropa hello funktionen något annat än tvåstegsverifiering, men du bör vara kan toofind det under inställningar för säkerhets- eller logga in.</span><span class="sxs-lookup"><span data-stu-id="d19bb-125">They may call hello feature something other than two-step verification, but you should be able toofind it under security or sign-in settings.</span></span> 

## <a name="install-hello-app"></a><span data-ttu-id="d19bb-126">Installera hello app</span><span class="sxs-lookup"><span data-stu-id="d19bb-126">Install hello app</span></span>
<span data-ttu-id="d19bb-127">hello Microsoft Authenticator-appen är tillgänglig för [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), och [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).</span><span class="sxs-lookup"><span data-stu-id="d19bb-127">hello Microsoft Authenticator app is available for [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), and [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).</span></span>

## <a name="add-accounts-toohello-app"></a><span data-ttu-id="d19bb-128">Lägg till konton toohello app</span><span class="sxs-lookup"><span data-stu-id="d19bb-128">Add accounts toohello app</span></span>
<span data-ttu-id="d19bb-129">För varje konto som du vill tooadd toohello Microsoft Authenticator-appen, använder du något av följande procedurer hello:</span><span class="sxs-lookup"><span data-stu-id="d19bb-129">For each account that you want tooadd toohello Microsoft Authenticator app, use one of hello following procedures:</span></span>

### <a name="add-a-personal-microsoft-account-toohello-app"></a><span data-ttu-id="d19bb-130">Lägg till en personlig Microsoft-konto toohello app</span><span class="sxs-lookup"><span data-stu-id="d19bb-130">Add a personal Microsoft account toohello app</span></span>

<span data-ttu-id="d19bb-131">För ett personligt microsoftkonto (en att du använder toosign i tooOutlook.com, Xbox, Skype, osv), du har toodo är att logga in tooyour konto i hello Microsoft Authenticator-appen.</span><span class="sxs-lookup"><span data-stu-id="d19bb-131">For a personal Microsoft account (one that you use toosign in tooOutlook.com, Xbox, Skype, etc.), all you have toodo is sign in tooyour account in hello Microsoft Authenticator app.</span></span>

### <a name="add-a-work-or-school-account-toohello-app-using-hello-qr-code-scanner"></a><span data-ttu-id="d19bb-132">Lägga till en arbets- eller skolkonto konto toohello app hello QR-kod skanner</span><span class="sxs-lookup"><span data-stu-id="d19bb-132">Add a work or school account toohello app using hello QR code scanner</span></span>
1. <span data-ttu-id="d19bb-133">Gå toohello skärmen inställningar för säkerhet verifiering.</span><span class="sxs-lookup"><span data-stu-id="d19bb-133">Go toohello security verification settings screen.</span></span>  <span data-ttu-id="d19bb-134">Mer information om hur tooget toothis skärmen finns [ändra säkerhetsinställningarna](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).</span><span class="sxs-lookup"><span data-stu-id="d19bb-134">For information on how tooget toothis screen, see [Changing your security settings](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).</span></span>
2. <span data-ttu-id="d19bb-135">Kryssrutan hello bredvid för**autentiseringsapp** Välj **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="d19bb-135">Check hello box next too**Authenticator app** then select **Configure**.</span></span>

    ![hello Konfigureringsknappen hello skärmen inställningar för säkerhet verifiering](./media/authenticator-app-how-to/azureauthe.png)

    <span data-ttu-id="d19bb-137">Nu visas en skärm med en QR-kod på den.</span><span class="sxs-lookup"><span data-stu-id="d19bb-137">This brings up a screen with a QR code on it.</span></span>

    ![Skärmen som ger hello QR-kod](./media/authenticator-app-how-to/barcode2.png)
3. <span data-ttu-id="d19bb-139">Öppna hello Microsoft Authenticator-appen.</span><span class="sxs-lookup"><span data-stu-id="d19bb-139">Open hello Microsoft Authenticator app.</span></span> <span data-ttu-id="d19bb-140">På hello **konton** väljer  **+** , och sedan ange att du vill tooadd ett arbets- eller skolkonto konto.</span><span class="sxs-lookup"><span data-stu-id="d19bb-140">On hello **accounts** screen, select **+**, and then specify that you want tooadd a work or school account.</span></span>
4. <span data-ttu-id="d19bb-141">Använda hello kamera tooscan hello QR-kod och välj sedan **klar** tooclose hello QR-kod skärmen.</span><span class="sxs-lookup"><span data-stu-id="d19bb-141">Use hello camera tooscan hello QR code, and then select **Done** tooclose hello QR code screen.</span></span>

    <span data-ttu-id="d19bb-142">Om kameran inte fungerar kan du [ange hello QR-koden och Webbadressen manuellt](#add-an-account-to-the-app-manually).</span><span class="sxs-lookup"><span data-stu-id="d19bb-142">If your camera is not working properly, you can [enter hello QR code and URL manually](#add-an-account-to-the-app-manually).</span></span>

5. <span data-ttu-id="d19bb-143">När hello app visas namnet på ditt konto med en sexsiffrig kod under den är du klar.</span><span class="sxs-lookup"><span data-stu-id="d19bb-143">When hello app shows your account name with a six-digit code underneath it, you're done.</span></span> 

    ![Konton skärmen](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-manually"></a><span data-ttu-id="d19bb-145">Lägg till ett konto toohello app manuellt</span><span class="sxs-lookup"><span data-stu-id="d19bb-145">Add an account toohello app manually</span></span>
1. <span data-ttu-id="d19bb-146">Gå toohello skärmen inställningar för säkerhet verifiering.</span><span class="sxs-lookup"><span data-stu-id="d19bb-146">Go toohello security verification settings screen.</span></span>  <span data-ttu-id="d19bb-147">Mer information om hur tooget toothis skärmen finns [ändra säkerhetsinställningarna](multi-factor-authentication-end-user-manage-settings.md).</span><span class="sxs-lookup"><span data-stu-id="d19bb-147">For information on how tooget toothis screen, see [Changing your security settings](multi-factor-authentication-end-user-manage-settings.md).</span></span>
2. <span data-ttu-id="d19bb-148">Välj **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="d19bb-148">Select **Configure**.</span></span>

    ![hello Konfigureringsknappen hello skärmen inställningar för säkerhet verifiering](./media/authenticator-app-how-to/azureauthe.png)

    <span data-ttu-id="d19bb-150">Nu visas en skärm med en QR-kod på den.</span><span class="sxs-lookup"><span data-stu-id="d19bb-150">This brings up a screen with a QR code on it.</span></span>  <span data-ttu-id="d19bb-151">Observera hello koden och Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="d19bb-151">Note hello code and URL.</span></span>

    ![Skärmen som ger hello QR-koden och Webbadressen](./media/authenticator-app-how-to/barcode2.png)
3. <span data-ttu-id="d19bb-153">Öppna hello Microsoft Authenticator-appen.</span><span class="sxs-lookup"><span data-stu-id="d19bb-153">Open hello Microsoft Authenticator app.</span></span> <span data-ttu-id="d19bb-154">På hello **konton** väljer  **+** , och sedan ange att du vill tooadd ett arbets- eller skolkonto konto.</span><span class="sxs-lookup"><span data-stu-id="d19bb-154">On hello **accounts** screen, select **+**, and then specify that you want tooadd a work or school account.</span></span>

4. <span data-ttu-id="d19bb-155">Markera i hello skanner **ange koden manuellt**.</span><span class="sxs-lookup"><span data-stu-id="d19bb-155">In hello scanner, select **enter code manually**.</span></span>

    ![Skärmen för att skanna QR-kod](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. <span data-ttu-id="d19bb-157">Ange hello kod och hello URL i hello rutorna i hello app och välj sedan **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="d19bb-157">Enter hello code and hello URL in hello appropriate boxes in hello app, then select **Finish**.</span></span>

    ![Skärmen för att ange koden och Webbadressen](./media/authenticator-app-how-to/manual.png)

6. <span data-ttu-id="d19bb-159">När hello app visas namnet på ditt konto med en sexsiffrig kod under den är du klar.</span><span class="sxs-lookup"><span data-stu-id="d19bb-159">When hello app shows your account name with a six-digit code underneath it, you're done.</span></span>

    ![Konton skärmen](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-using-touch-id"></a><span data-ttu-id="d19bb-161">Lägg till ett konto toohello app som använder Touch ID</span><span class="sxs-lookup"><span data-stu-id="d19bb-161">Add an account toohello app using Touch ID</span></span>
<span data-ttu-id="d19bb-162">hello Microsoft Authenticator-appen på iOS stöder Touch ID.</span><span class="sxs-lookup"><span data-stu-id="d19bb-162">hello Microsoft Authenticator app on iOS supports Touch ID.</span></span>  <span data-ttu-id="d19bb-163">Azure Multi-Factor Authentication kan organisationer toorequire PIN-koden för enheter.</span><span class="sxs-lookup"><span data-stu-id="d19bb-163">Azure Multi-Factor Authentication allows organizations toorequire a PIN for devices.</span></span> <span data-ttu-id="d19bb-164">Med Touch ID behöver iOS-användare tooenter en PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="d19bb-164">With Touch ID, iOS users don’t need tooenter a PIN.</span></span> <span data-ttu-id="d19bb-165">De kan i stället söka igenom sitt fingeravtryck och välj **Godkänn**.</span><span class="sxs-lookup"><span data-stu-id="d19bb-165">Instead, they can scan their fingerprint and select **Approve**.</span></span>

<span data-ttu-id="d19bb-166">Det är enkelt att ställa in Touch ID med Microsoft Authenticator.</span><span class="sxs-lookup"><span data-stu-id="d19bb-166">Setting up Touch ID with Microsoft Authenticator is simple.</span></span> <span data-ttu-id="d19bb-167">Du har slutfört en normal verifiering utmaning med en PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="d19bb-167">You complete a normal verification challenge with a PIN.</span></span> <span data-ttu-id="d19bb-168">Om enheten har stöd för Touch ID, konfigurerar Microsoft Authenticator den automatiskt för det kontot.</span><span class="sxs-lookup"><span data-stu-id="d19bb-168">If your device supports Touch ID, Microsoft Authenticator sets it up automatically for that account.</span></span>

![Verifiering av installationsprogrammet för Touch ID](./media/authenticator-app-how-to/touchid1.png)

<span data-ttu-id="d19bb-170">Peka framåt, som när du är krävs tooverify din inloggning, du väljer hello emot push-meddelanden och söka igenom ditt fingeravtryck istället för att ange din PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="d19bb-170">From that point forward, when you're required tooverify your sign-in, you select hello received push notification and scan your fingerprint instead of entering your PIN.</span></span>

![Push-meddelanden](./media/authenticator-app-how-to/touchid2.png)

## <a name="use-hello-app-when-you-sign-in"></a><span data-ttu-id="d19bb-172">Använda hello app när du loggar in</span><span class="sxs-lookup"><span data-stu-id="d19bb-172">Use hello app when you sign in</span></span>

<span data-ttu-id="d19bb-173">När ditt konto har lagts till toohello app, kanske ange toodo ett test verifiering toomake att allt är korrekt konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="d19bb-173">Once your account is added toohello app, you may be prompted toodo a test verification toomake sure everything was configured correctly.</span></span> <span data-ttu-id="d19bb-174">När är du klar!</span><span class="sxs-lookup"><span data-stu-id="d19bb-174">After that, you're done!</span></span> <span data-ttu-id="d19bb-175">Du behöver inte toodo något annat tills hello nästa gång du loggar in.</span><span class="sxs-lookup"><span data-stu-id="d19bb-175">You don't need toodo anything else until hello next time you sign in.</span></span>

<span data-ttu-id="d19bb-176">Om du väljer toouse verifieringskoder i hello app måste du starta toosee dem på hello startsidan.</span><span class="sxs-lookup"><span data-stu-id="d19bb-176">If you chose toouse verification codes in hello app, you start toosee them on hello home page.</span></span> <span data-ttu-id="d19bb-177">De kan ändra var 30: e sekund så att du alltid har en ny kod när du behöver en.</span><span class="sxs-lookup"><span data-stu-id="d19bb-177">They change every 30 seconds so that you always have a new code when you need one.</span></span> <span data-ttu-id="d19bb-178">Men du behöver inte toodo något med dem förrän du logga in och ange tooenter en Verifieringskod.</span><span class="sxs-lookup"><span data-stu-id="d19bb-178">But you don't need toodo anything with them until you sign in and are prompted tooenter a verification code.</span></span>  
