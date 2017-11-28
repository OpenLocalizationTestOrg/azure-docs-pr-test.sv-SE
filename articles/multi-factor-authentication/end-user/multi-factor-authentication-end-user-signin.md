---
title: "Azure MFA-inloggning med tvåstegsverifiering | Microsoft Docs"
description: "Den här sidan ger du riktlinjer för hur du går till olika inloggning tillgängliga metoder visas med Azure MFA."
keywords: "autentisering av användare, inloggning, logga in med mobiltelefon, logga in med Arbetstelefon"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: d12115be61ca00dfb86dd822ccae9f9096fa796a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a><span data-ttu-id="89b7b-104">Inloggning med Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="89b7b-104">The sign-in experience with Azure Multi-Factor Authentication</span></span>
> [!NOTE]
> <span data-ttu-id="89b7b-105">Syftet med den här artikeln är att gå igenom ett typiskt inloggningen.</span><span class="sxs-lookup"><span data-stu-id="89b7b-105">The purpose of this article is to walk through a typical sign-in experience.</span></span> <span data-ttu-id="89b7b-106">Hjälp med att logga in eller att felsöka problem finns i [har problem med Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="89b7b-106">For help with signing in, or to troubleshoot problems, see [Having trouble with Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span></span>

## <a name="what-will-your-sign-in-experience-be"></a><span data-ttu-id="89b7b-107">Vad är din inloggning?</span><span class="sxs-lookup"><span data-stu-id="89b7b-107">What will your sign-in experience be?</span></span>
<span data-ttu-id="89b7b-108">Din inloggning skiljer sig åt beroende på vad du väljer att använda som din tvåfaktorsautentisering: ett telefonsamtal eller ett authentication-appen texter.</span><span class="sxs-lookup"><span data-stu-id="89b7b-108">Your sign-in experience differs depending on what you choose to use as your second factor: a phone call, an authentication app, or texts.</span></span> <span data-ttu-id="89b7b-109">Välj det alternativ som bäst beskriver vad du vill göra:</span><span class="sxs-lookup"><span data-stu-id="89b7b-109">Choose the option that best describes what you are doing:</span></span>

| <span data-ttu-id="89b7b-110">Hur loggar du in?</span><span class="sxs-lookup"><span data-stu-id="89b7b-110">How do you sign in?</span></span> | 
| --- |
| [<span data-ttu-id="89b7b-111">Med ett telefonsamtal till min mobil eller office telefon</span><span class="sxs-lookup"><span data-stu-id="89b7b-111">With a phone call to my mobile or office phone</span></span>](#signing-in-with-a-phone-call) |
| [<span data-ttu-id="89b7b-112">Med text till min mobiltelefon</span><span class="sxs-lookup"><span data-stu-id="89b7b-112">With a text to my mobile phone</span></span>](#signing-in-with-a-text-message)
| [<span data-ttu-id="89b7b-113">Med meddelanden från Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="89b7b-113">With notifications from the Microsoft Authenticator app</span></span>](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [<span data-ttu-id="89b7b-114">Med verifieringskoder från Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="89b7b-114">With verification codes from the Microsoft Authenticator app</span></span>](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [<span data-ttu-id="89b7b-115">Med en alternativ metod, eftersom det inte kan använda min verifieringsmetod just nu</span><span class="sxs-lookup"><span data-stu-id="89b7b-115">With an alternate method, because I can't use my preferred method right now</span></span>](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a><span data-ttu-id="89b7b-116">Logga in med ett telefonsamtal</span><span class="sxs-lookup"><span data-stu-id="89b7b-116">Signing in with a phone call</span></span>
<span data-ttu-id="89b7b-117">Följande information beskriver tvåstegsverifiering verifiering upplevelse med ett samtal till din mobil- eller office telefon.</span><span class="sxs-lookup"><span data-stu-id="89b7b-117">The following information describes the two-step verification experience with a call to your mobile or office phone.</span></span>

1. <span data-ttu-id="89b7b-118">Logga in på en applikation eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="89b7b-118">Sign in to an application or service such as Office 365 using your username and password.</span></span>  
2. <span data-ttu-id="89b7b-119">Microsoft anropar du.</span><span class="sxs-lookup"><span data-stu-id="89b7b-119">Microsoft calls you.</span></span>  
3. <span data-ttu-id="89b7b-120">Besvara samtalet och tryck på #-knappen.</span><span class="sxs-lookup"><span data-stu-id="89b7b-120">Answer the phone and hit the # key.</span></span>  

## <a name="signing-in-with-a-text-message"></a><span data-ttu-id="89b7b-121">Logga in med ett textmeddelande</span><span class="sxs-lookup"><span data-stu-id="89b7b-121">Signing in with a text message</span></span>
<span data-ttu-id="89b7b-122">Följande information beskriver tvåstegsverifiering verifiering upplevelse med ett SMS till din mobiltelefon:</span><span class="sxs-lookup"><span data-stu-id="89b7b-122">The following information describes the two-step verification experience with a text message to your mobile phone:</span></span>

1. <span data-ttu-id="89b7b-123">Logga in på en applikation eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="89b7b-123">Sign in to an application or service such as Office 365 using your username and password.</span></span> 
2. <span data-ttu-id="89b7b-124">Microsoft skickar ett textmeddelande som innehåller ett antal kod.</span><span class="sxs-lookup"><span data-stu-id="89b7b-124">Microsoft sends you a text message that contains a number code.</span></span> 
3. <span data-ttu-id="89b7b-125">Ange koden i rutan på sidan logga in.</span><span class="sxs-lookup"><span data-stu-id="89b7b-125">Enter the code in the box provided on the sign-in page.</span></span> 

## <a name="signing-in-with-the-microsoft-authenticator-app"></a><span data-ttu-id="89b7b-126">Logga in med Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="89b7b-126">Signing in with the Microsoft Authenticator app</span></span> 
<span data-ttu-id="89b7b-127">Följande information beskriver användningen av Microsoft Authenticator-appen för tvåstegsverifiering kontroller.</span><span class="sxs-lookup"><span data-stu-id="89b7b-127">The following information describes the experience of using the Microsoft Authenticator app for two-step verifications.</span></span> <span data-ttu-id="89b7b-128">Det finns två sätt att använda appen.</span><span class="sxs-lookup"><span data-stu-id="89b7b-128">There are two different ways to use the app.</span></span> <span data-ttu-id="89b7b-129">Du kan ta emot push-meddelanden på enheten eller så kan du öppna appen för att få en Verifieringskod.</span><span class="sxs-lookup"><span data-stu-id="89b7b-129">You can receive push notifications on your device, or you can open the app to get a verification code.</span></span>

### <a name="to-sign-in-with-a-notification-from-the-microsoft-authenticator-app"></a><span data-ttu-id="89b7b-130">Logga in med ett meddelande från Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="89b7b-130">To sign in with a notification from the Microsoft Authenticator app</span></span>
1. <span data-ttu-id="89b7b-131">Logga in på en applikation eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="89b7b-131">Sign in to an application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="89b7b-132">Microsoft skickar ett meddelande till Microsoft Authenticator-appen på enheten.</span><span class="sxs-lookup"><span data-stu-id="89b7b-132">Microsoft sends a notification to the Microsoft Authenticator app on your device.</span></span>

  ![Microsoft skickar meddelanden](./media/multi-factor-authentication-end-user-signin/notify.png)

3. <span data-ttu-id="89b7b-134">Öppna meddelandet på din telefon och välj den **Kontrollera** nyckel.</span><span class="sxs-lookup"><span data-stu-id="89b7b-134">Open the notification on your phone and select the **Verify** key.</span></span> <span data-ttu-id="89b7b-135">Om ditt företag kräver en PIN-kod kan du ange det här.</span><span class="sxs-lookup"><span data-stu-id="89b7b-135">If your company requires a PIN, enter it here.</span></span>
4. <span data-ttu-id="89b7b-136">Du bör nu vara inloggad.</span><span class="sxs-lookup"><span data-stu-id="89b7b-136">You should now be signed in.</span></span>

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a><span data-ttu-id="89b7b-137">Logga in med en Verifieringskod med Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="89b7b-137">To sign in using a verification code with the Microsoft Authenticator app</span></span>

<span data-ttu-id="89b7b-138">Om du använder Microsoft Authenticator-appen för att hämta verifieringskoder, när du öppnar appen se du ett tal under namnet på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="89b7b-138">If you use the Microsoft Authenticator app to get verification codes, then when you open the app you see a number under your account name.</span></span> <span data-ttu-id="89b7b-139">Det här talet ändras med 30 sekunders mellanrum så att du inte använder samma nummer två gånger.</span><span class="sxs-lookup"><span data-stu-id="89b7b-139">This number changes every 30 seconds so that you don't use the same number twice.</span></span> <span data-ttu-id="89b7b-140">När du uppmanas för en Verifieringskod öppnar appen och använda tal som visas för närvarande.</span><span class="sxs-lookup"><span data-stu-id="89b7b-140">When you're asked for a verification code, open the app and use whatever number is currently displayed.</span></span> 

1. <span data-ttu-id="89b7b-141">Logga in på en applikation eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="89b7b-141">Sign in to an application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="89b7b-142">Microsoft efterfrågar en Verifieringskod.</span><span class="sxs-lookup"><span data-stu-id="89b7b-142">Microsoft prompts you for a verification code.</span></span>

  ![Ange verifieringskoden](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. <span data-ttu-id="89b7b-144">Öppna Microsoft Authenticator-appen på din telefon och ange koden i rutan där du loggar in.</span><span class="sxs-lookup"><span data-stu-id="89b7b-144">Open the Microsoft Authenticator app on your phone and enter the code in the box where you are signing in.</span></span>

## <a name="signing-in-with-an-alternate-method"></a><span data-ttu-id="89b7b-145">Logga in med en alternativ metod</span><span class="sxs-lookup"><span data-stu-id="89b7b-145">Signing in with an alternate method</span></span>
<span data-ttu-id="89b7b-146">Du saknar ibland telefon eller enhet som du ställer in som din primära verifieringsmetod.</span><span class="sxs-lookup"><span data-stu-id="89b7b-146">Sometimes you don't have the phone or device that you set up as your preferred verification method.</span></span> <span data-ttu-id="89b7b-147">Den här situationen är därför rekommenderar vi att du konfigurerar metoder för säkerhetskopiering för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="89b7b-147">This situation is why we recommend that you set up backup methods for your account.</span></span> <span data-ttu-id="89b7b-148">I följande avsnitt beskrivs hur du loggar in med en alternativ metod när den primära metoden är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="89b7b-148">The following section shows you how to sign in with an alternate method when your primary method may not be available.</span></span>

1. <span data-ttu-id="89b7b-149">Logga in på en applikation eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="89b7b-149">Sign in to an application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="89b7b-150">Välj **använder ett annat verifieringsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="89b7b-150">Select **Use a different verification option**.</span></span> <span data-ttu-id="89b7b-151">Du kan se olika verifieringsalternativ baserat på hur många du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="89b7b-151">You see different verification options based on how many you have setup.</span></span>
3. <span data-ttu-id="89b7b-152">Välj en alternativ metod och logga in.</span><span class="sxs-lookup"><span data-stu-id="89b7b-152">Choose an alternate method and sign in.</span></span>

  ![Använd alternativ metod](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a><span data-ttu-id="89b7b-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89b7b-154">Next steps</span></span>

<span data-ttu-id="89b7b-155">Om du har problem med att logga in med tvåstegsverifiering kan få mer information på [har problem med Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="89b7b-155">If you have problems signing in with two-step verification, get more information at [Having trouble with Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span></span>

<span data-ttu-id="89b7b-156">Lär dig hur du [hantera dina inställningar för tvåstegsverifiering](multi-factor-authentication-end-user-manage-settings.md).</span><span class="sxs-lookup"><span data-stu-id="89b7b-156">Learn how to [Manage your two-step verification settings](multi-factor-authentication-end-user-manage-settings.md).</span></span>

<span data-ttu-id="89b7b-157">Lär dig hur du [komma igång med Microsoft Authenticator-appen](microsoft-authenticator-app-how-to.md) så att du kan använda meddelanden för att logga in, i stället för texter och telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="89b7b-157">Find out how to [Get started with the Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) so that you can use notifications to sign in, instead of texts and phone calls.</span></span> 