---
title: "aaaAzure MFA-inloggning med tvåstegsverifiering | Microsoft Docs"
description: "Den här sidan ger du riktlinjer för där toogo toosee hello olika inloggning metoder med Azure MFA."
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
ms.openlocfilehash: fcd5eb5e8426eda537db9e099bf247bde29c195b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-sign-in-experience-with-azure-multi-factor-authentication"></a><span data-ttu-id="615dc-104">hello inloggning med Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="615dc-104">hello sign-in experience with Azure Multi-Factor Authentication</span></span>
> [!NOTE]
> <span data-ttu-id="615dc-105">hello syftet med den här artikeln är toowalk via en typisk inloggningen.</span><span class="sxs-lookup"><span data-stu-id="615dc-105">hello purpose of this article is toowalk through a typical sign-in experience.</span></span> <span data-ttu-id="615dc-106">Hjälp med att logga in eller tootroubleshoot problem finns i [har problem med Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="615dc-106">For help with signing in, or tootroubleshoot problems, see [Having trouble with Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span></span>

## <a name="what-will-your-sign-in-experience-be"></a><span data-ttu-id="615dc-107">Vad är din inloggning?</span><span class="sxs-lookup"><span data-stu-id="615dc-107">What will your sign-in experience be?</span></span>
<span data-ttu-id="615dc-108">Din inloggning skiljer sig åt beroende på vad du väljer toouse som din andra faktor: ett telefonsamtal eller ett authentication-appen texter.</span><span class="sxs-lookup"><span data-stu-id="615dc-108">Your sign-in experience differs depending on what you choose toouse as your second factor: a phone call, an authentication app, or texts.</span></span> <span data-ttu-id="615dc-109">Välj hello-alternativ som bäst beskriver vad du vill göra:</span><span class="sxs-lookup"><span data-stu-id="615dc-109">Choose hello option that best describes what you are doing:</span></span>

| <span data-ttu-id="615dc-110">Hur loggar du in?</span><span class="sxs-lookup"><span data-stu-id="615dc-110">How do you sign in?</span></span> | 
| --- |
| [<span data-ttu-id="615dc-111">Med ett telefonsamtal toomy mobile- eller office telefon</span><span class="sxs-lookup"><span data-stu-id="615dc-111">With a phone call toomy mobile or office phone</span></span>](#signing-in-with-a-phone-call) |
| [<span data-ttu-id="615dc-112">Med en text toomy mobiltelefon</span><span class="sxs-lookup"><span data-stu-id="615dc-112">With a text toomy mobile phone</span></span>](#signing-in-with-a-text-message)
| [<span data-ttu-id="615dc-113">Med meddelanden från hello Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="615dc-113">With notifications from hello Microsoft Authenticator app</span></span>](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [<span data-ttu-id="615dc-114">Med verifieringskoder från hello Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="615dc-114">With verification codes from hello Microsoft Authenticator app</span></span>](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [<span data-ttu-id="615dc-115">Med en alternativ metod, eftersom det inte kan använda min verifieringsmetod just nu</span><span class="sxs-lookup"><span data-stu-id="615dc-115">With an alternate method, because I can't use my preferred method right now</span></span>](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a><span data-ttu-id="615dc-116">Logga in med ett telefonsamtal</span><span class="sxs-lookup"><span data-stu-id="615dc-116">Signing in with a phone call</span></span>
<span data-ttu-id="615dc-117">hello följande information beskriver hello tvåstegsverifiering verifiering erfarenhet av ett anrop tooyour mobile eller Arbetstelefon.</span><span class="sxs-lookup"><span data-stu-id="615dc-117">hello following information describes hello two-step verification experience with a call tooyour mobile or office phone.</span></span>

1. <span data-ttu-id="615dc-118">Logga in tooan program eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="615dc-118">Sign in tooan application or service such as Office 365 using your username and password.</span></span>  
2. <span data-ttu-id="615dc-119">Microsoft anropar du.</span><span class="sxs-lookup"><span data-stu-id="615dc-119">Microsoft calls you.</span></span>  
3. <span data-ttu-id="615dc-120">Besvara hello telefon och tryck hello #-knappen.</span><span class="sxs-lookup"><span data-stu-id="615dc-120">Answer hello phone and hit hello # key.</span></span>  

## <a name="signing-in-with-a-text-message"></a><span data-ttu-id="615dc-121">Logga in med ett textmeddelande</span><span class="sxs-lookup"><span data-stu-id="615dc-121">Signing in with a text message</span></span>
<span data-ttu-id="615dc-122">hello följande information beskriver hello tvåstegsverifiering verifiering erfarenhet av text meddelandet tooyour mobiltelefon:</span><span class="sxs-lookup"><span data-stu-id="615dc-122">hello following information describes hello two-step verification experience with a text message tooyour mobile phone:</span></span>

1. <span data-ttu-id="615dc-123">Logga in tooan program eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="615dc-123">Sign in tooan application or service such as Office 365 using your username and password.</span></span> 
2. <span data-ttu-id="615dc-124">Microsoft skickar ett textmeddelande som innehåller ett antal kod.</span><span class="sxs-lookup"><span data-stu-id="615dc-124">Microsoft sends you a text message that contains a number code.</span></span> 
3. <span data-ttu-id="615dc-125">Ange hello kod i hello rutan på hello-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="615dc-125">Enter hello code in hello box provided on hello sign-in page.</span></span> 

## <a name="signing-in-with-hello-microsoft-authenticator-app"></a><span data-ttu-id="615dc-126">Logga in med hello Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="615dc-126">Signing in with hello Microsoft Authenticator app</span></span> 
<span data-ttu-id="615dc-127">hello beskriver följande information hello upplevelse av hello Microsoft Authenticator-appen för tvåstegsverifiering kontroller.</span><span class="sxs-lookup"><span data-stu-id="615dc-127">hello following information describes hello experience of using hello Microsoft Authenticator app for two-step verifications.</span></span> <span data-ttu-id="615dc-128">Det finns två olika sätt toouse hello app.</span><span class="sxs-lookup"><span data-stu-id="615dc-128">There are two different ways toouse hello app.</span></span> <span data-ttu-id="615dc-129">Du kan ta emot push-meddelanden på enheten eller så kan du öppna hello app tooget en Verifieringskod.</span><span class="sxs-lookup"><span data-stu-id="615dc-129">You can receive push notifications on your device, or you can open hello app tooget a verification code.</span></span>

### <a name="toosign-in-with-a-notification-from-hello-microsoft-authenticator-app"></a><span data-ttu-id="615dc-130">toosign in med ett meddelande från hello Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="615dc-130">toosign in with a notification from hello Microsoft Authenticator app</span></span>
1. <span data-ttu-id="615dc-131">Logga in tooan program eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="615dc-131">Sign in tooan application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="615dc-132">Microsoft skickar ett meddelande toohello Microsoft Authenticator-appen på enheten.</span><span class="sxs-lookup"><span data-stu-id="615dc-132">Microsoft sends a notification toohello Microsoft Authenticator app on your device.</span></span>

  ![Microsoft skickar meddelanden](./media/multi-factor-authentication-end-user-signin/notify.png)

3. <span data-ttu-id="615dc-134">Öppna hello-meddelande på din telefon och välj hello **Kontrollera** nyckel.</span><span class="sxs-lookup"><span data-stu-id="615dc-134">Open hello notification on your phone and select hello **Verify** key.</span></span> <span data-ttu-id="615dc-135">Om ditt företag kräver en PIN-kod kan du ange det här.</span><span class="sxs-lookup"><span data-stu-id="615dc-135">If your company requires a PIN, enter it here.</span></span>
4. <span data-ttu-id="615dc-136">Du bör nu vara inloggad.</span><span class="sxs-lookup"><span data-stu-id="615dc-136">You should now be signed in.</span></span>

### <a name="toosign-in-using-a-verification-code-with-hello-microsoft-authenticator-app"></a><span data-ttu-id="615dc-137">toosign in med en Verifieringskod har hello Microsoft Authenticator-appen</span><span class="sxs-lookup"><span data-stu-id="615dc-137">toosign in using a verification code with hello Microsoft Authenticator app</span></span>

<span data-ttu-id="615dc-138">Om du använder hello Microsoft Authenticator app tooget verifieringskoder sedan visas när du öppnar hello app ett tal under namnet på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="615dc-138">If you use hello Microsoft Authenticator app tooget verification codes, then when you open hello app you see a number under your account name.</span></span> <span data-ttu-id="615dc-139">Det här talet ändras med 30 sekunders mellanrum så att du inte använder hello samma tal två gånger.</span><span class="sxs-lookup"><span data-stu-id="615dc-139">This number changes every 30 seconds so that you don't use hello same number twice.</span></span> <span data-ttu-id="615dc-140">När du uppmanas för en Verifieringskod öppna hello app och använda tal som visas för närvarande.</span><span class="sxs-lookup"><span data-stu-id="615dc-140">When you're asked for a verification code, open hello app and use whatever number is currently displayed.</span></span> 

1. <span data-ttu-id="615dc-141">Logga in tooan program eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="615dc-141">Sign in tooan application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="615dc-142">Microsoft efterfrågar en Verifieringskod.</span><span class="sxs-lookup"><span data-stu-id="615dc-142">Microsoft prompts you for a verification code.</span></span>

  ![Ange verifieringskoden](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. <span data-ttu-id="615dc-144">Öppna hello Microsoft Authenticator-appen på din telefon och ange koden hello hello i rutan där du loggar in.</span><span class="sxs-lookup"><span data-stu-id="615dc-144">Open hello Microsoft Authenticator app on your phone and enter hello code in hello box where you are signing in.</span></span>

## <a name="signing-in-with-an-alternate-method"></a><span data-ttu-id="615dc-145">Logga in med en alternativ metod</span><span class="sxs-lookup"><span data-stu-id="615dc-145">Signing in with an alternate method</span></span>
<span data-ttu-id="615dc-146">Du saknar ibland hello telefon eller enhet som du ställer in som din primära verifieringsmetod.</span><span class="sxs-lookup"><span data-stu-id="615dc-146">Sometimes you don't have hello phone or device that you set up as your preferred verification method.</span></span> <span data-ttu-id="615dc-147">Den här situationen är därför rekommenderar vi att du konfigurerar metoder för säkerhetskopiering för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="615dc-147">This situation is why we recommend that you set up backup methods for your account.</span></span> <span data-ttu-id="615dc-148">hello följande avsnitt visas hur toosign in med en alternativ metod när den primära metoden är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="615dc-148">hello following section shows you how toosign in with an alternate method when your primary method may not be available.</span></span>

1. <span data-ttu-id="615dc-149">Logga in tooan program eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="615dc-149">Sign in tooan application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="615dc-150">Välj **använder ett annat verifieringsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="615dc-150">Select **Use a different verification option**.</span></span> <span data-ttu-id="615dc-151">Du kan se olika verifieringsalternativ baserat på hur många du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="615dc-151">You see different verification options based on how many you have setup.</span></span>
3. <span data-ttu-id="615dc-152">Välj en alternativ metod och logga in.</span><span class="sxs-lookup"><span data-stu-id="615dc-152">Choose an alternate method and sign in.</span></span>

  ![Använd alternativ metod](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a><span data-ttu-id="615dc-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="615dc-154">Next steps</span></span>

<span data-ttu-id="615dc-155">Om du har problem med att logga in med tvåstegsverifiering kan få mer information på [har problem med Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="615dc-155">If you have problems signing in with two-step verification, get more information at [Having trouble with Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span></span>

<span data-ttu-id="615dc-156">Lär dig hur för[hantera dina inställningar för tvåstegsverifiering](multi-factor-authentication-end-user-manage-settings.md).</span><span class="sxs-lookup"><span data-stu-id="615dc-156">Learn how too[Manage your two-step verification settings](multi-factor-authentication-end-user-manage-settings.md).</span></span>

<span data-ttu-id="615dc-157">Ta reda på hur för[Kom igång med hello Microsoft Authenticator-appen](microsoft-authenticator-app-how-to.md) så att du kan använda meddelanden toosign i, i stället för texter och telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="615dc-157">Find out how too[Get started with hello Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) so that you can use notifications toosign in, instead of texts and phone calls.</span></span> 
