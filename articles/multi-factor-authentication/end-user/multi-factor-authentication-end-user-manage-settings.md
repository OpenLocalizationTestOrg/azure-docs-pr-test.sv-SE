---
title: "aaaManage dina inställningar för tvåstegsverifiering | Microsoft Docs"
description: "Hantera hur du använder Azure Multi-Factor Authentication inklusive ändrar kontaktinformation eller konfigurera dina enheter."
services: multi-factor-authentication
keywords: flerfunktionsautentisering klienten, problem med autentisering, Korrelations-ID
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 2c974b08c584943f3c5a6b9bf16497d1706e8329
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-settings-for-two-step-verification"></a><span data-ttu-id="a773b-104">Hantera inställningar för tvåstegsverifiering</span><span class="sxs-lookup"><span data-stu-id="a773b-104">Manage your settings for two-step verification</span></span>
<span data-ttu-id="a773b-105">Den här artikeln innehåller svar på frågor om hur tooupdate inställningar för tvåstegsverifiering verifieringen eller Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="a773b-105">This article answers questions about how tooupdate settings for two-step verification or multi-factor authentication.</span></span> <span data-ttu-id="a773b-106">Om du har problem med inloggning tooyour konto finns för[har problem med tvåstegsverifiering](multi-factor-authentication-end-user-troubleshoot.md) för hjälp med felsökning.</span><span class="sxs-lookup"><span data-stu-id="a773b-106">If you are having issues signing in tooyour account, refer too[Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md) for troubleshooting help.</span></span>

## <a name="where-toofind-hello-settings-page"></a><span data-ttu-id="a773b-107">Där toofind hello inställningssidan</span><span class="sxs-lookup"><span data-stu-id="a773b-107">Where toofind hello settings page</span></span>
<span data-ttu-id="a773b-108">Beroende på hur företaget du konfigurerar Azure Multi-Factor Authentication, finns det några platser där du kan ändra inställningarna som ditt telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="a773b-108">Depending on how your company set up Azure Multi-Factor Authentication, there are a few places where you can change your settings like your phone number.</span></span>

<span data-ttu-id="a773b-109">Om IT-administratören skickas en specifik URL eller steg toomanage tvåstegsverifiering, följer du dessa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="a773b-109">If your IT admin sent out a specific URL or steps toomanage two-step verification, follow those instructions.</span></span> <span data-ttu-id="a773b-110">Annars bör hello följa anvisningar fungerar för alla andra.</span><span class="sxs-lookup"><span data-stu-id="a773b-110">Otherwise, hello following instructions should work for everybody else.</span></span> <span data-ttu-id="a773b-111">Om du gör så här men inte ser hello samma inställningar som innebär att ditt arbete eller skola anpassad sina egna portal.</span><span class="sxs-lookup"><span data-stu-id="a773b-111">If you follow these steps but don't see hello same options, that means that your work or school customized their own portal.</span></span> <span data-ttu-id="a773b-112">Be din administratör för hello länken tooyour flerfunktionsautentisering Azure portal.</span><span class="sxs-lookup"><span data-stu-id="a773b-112">Ask your admin for hello link tooyour Azure Multi-Factor Authentication portal.</span></span>

1. <span data-ttu-id="a773b-113">Logga in för[https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="a773b-113">Sign in too[https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>  
2. <span data-ttu-id="a773b-114">Välj namnet på ditt konto i hello upp till höger och sedan **profil**.</span><span class="sxs-lookup"><span data-stu-id="a773b-114">Select your account name in hello top right, then select **profile**.</span></span>  
3. <span data-ttu-id="a773b-115">Välj **ytterligare säkerhetsverifiering**.</span><span class="sxs-lookup"><span data-stu-id="a773b-115">Select **Additional security verification**.</span></span>  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. <span data-ttu-id="a773b-117">hello ytterligare säkerhet verifiering sidan läses in med dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="a773b-117">hello Additional security verification page loads with your settings.</span></span>

    ![Proofup](./media/multi-factor-authentication-end-user-manage/proofup.png)

## <a name="i-want-toochange-my-phone-number-or-add-a-secondary-number"></a><span data-ttu-id="a773b-119">Jag vill toochange mitt telefonnummer eller lägga till en sekundär tal</span><span class="sxs-lookup"><span data-stu-id="a773b-119">I want toochange my phone number, or add a secondary number</span></span>
<span data-ttu-id="a773b-120">Det är viktigt tooconfigure ett telefonnummer för sekundära autentiseringen.</span><span class="sxs-lookup"><span data-stu-id="a773b-120">It is important tooconfigure a secondary authentication phone number.</span></span>  <span data-ttu-id="a773b-121">Eftersom din primära telefonnummer nummer och din mobila app är förmodligen på hello samma telefon, hello sekundära telefonnumret är hello endast hur du ska kunna tooget tillbaka till ditt konto om telefonen tappas bort eller blir stulen.</span><span class="sxs-lookup"><span data-stu-id="a773b-121">Because your primary phone number and your mobile app are probably on hello same phone, hello secondary phone number is hello only way you will be able tooget back into your account if your phone is lost or stolen.</span></span>

> [!NOTE]
> <span data-ttu-id="a773b-122">Om du inte har åtkomst tooyour primära telefonnummer och behöver hjälp med att komma i tooyour konto, finns våra hjälpavsnitten i [har problem med tvåstegsverifiering](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="a773b-122">If you don't have access tooyour primary phone number, and need help getting in tooyour account, see our help topics in [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md).</span></span>  

<span data-ttu-id="a773b-123">**toochange din primära telefonnummer:**</span><span class="sxs-lookup"><span data-stu-id="a773b-123">**toochange your primary phone number:**</span></span>  

1. <span data-ttu-id="a773b-124">Hello ytterligare säkerhet kontroll på sidan Välj hello textruta med ditt aktuella telefonnummer och redigera den med ditt nya telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="a773b-124">On hello Additional security verification page, select hello text box with your current phone number and edit it with your new phone number.</span></span>  
2. <span data-ttu-id="a773b-125">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a773b-125">Select **Save**.</span></span>  
3. <span data-ttu-id="a773b-126">Om detta är hello-nummer som du använder för din verifieringsalternativet har du tooverify hello nytt nummer innan du kan spara den.</span><span class="sxs-lookup"><span data-stu-id="a773b-126">If this is hello number that you use for your preferred verification option, you have tooverify hello new number before you can save it.</span></span>  

<span data-ttu-id="a773b-127">**tooadd ett sekundärt telefonnummer:**</span><span class="sxs-lookup"><span data-stu-id="a773b-127">**tooadd a secondary phone number:**</span></span>  

1. <span data-ttu-id="a773b-128">Hello ytterligare säkerhet kontroll på sidan kryssrutan hello bredvid för**telefon för alternativa autentisering.**</span><span class="sxs-lookup"><span data-stu-id="a773b-128">On hello Additional security verification page, check hello box next too**Alternate authentication phone.**</span></span>  
2. <span data-ttu-id="a773b-129">Ange ditt sekundära telefonnummer i hello textruta.</span><span class="sxs-lookup"><span data-stu-id="a773b-129">Enter your secondary phone number in hello text box.</span></span>  
3. <span data-ttu-id="a773b-130">Välj **spara** och ändringarna är klara.</span><span class="sxs-lookup"><span data-stu-id="a773b-130">Select **Save** and your changes are finished.</span></span>  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a><span data-ttu-id="a773b-131">Kräv tvåstegsverifiering igen på en enhet som du har markerat som betrodd</span><span class="sxs-lookup"><span data-stu-id="a773b-131">Require two-step verification again on a device you've marked as trusted</span></span>

<span data-ttu-id="a773b-132">Beroende på inställningarna för din organisation kan du ha en kryssruta med texten ”fråga inte igen för **X** dagar” när du utför tvåstegsverifiering i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="a773b-132">Depending on your organization settings, you may have a checkbox that says "Don't ask again for **X** days" when you perform two-step verification on your browser.</span></span> <span data-ttu-id="a773b-133">Om den här kryssrutan och sedan tappar bort din enhet eller anser att ditt konto komprometteras, bör du återställa tvåstegsverifiering verifiering tooall dina enheter.</span><span class="sxs-lookup"><span data-stu-id="a773b-133">If you check this box and then lose your device or think that your account is compromised, you should restore two-step verification tooall your devices.</span></span> 

1. <span data-ttu-id="a773b-134">Hello ytterligare säkerhet kontroll på sidan Välj **Återställ multifaktorautentisering på tidigare betrodda enheter**.</span><span class="sxs-lookup"><span data-stu-id="a773b-134">On hello Additional security verification page, select **Restore multi-factor authentication on previously trusted devices**.</span></span>
2. <span data-ttu-id="a773b-135">hello kommer nästa gång du loggar in på alla enheter du att tillfrågas tooperform tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="a773b-135">hello next time you sign in on any device, you'll be prompted tooperform two-step verification.</span></span> 

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-tooa-new-one"></a><span data-ttu-id="a773b-136">Hur gör Rensa Microsoft Authenticator från min gamla enhet och flytta tooa ny?</span><span class="sxs-lookup"><span data-stu-id="a773b-136">How do I clean up Microsoft Authenticator from my old device and move tooa new one?</span></span>
<span data-ttu-id="a773b-137">När du avinstallerar hello app från din enhet eller Återställ hello enhet tas inte bort hello-aktivering på hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="a773b-137">When you uninstall hello app from your device or reset hello device, it does not remove hello activation on hello back end.</span></span> <span data-ttu-id="a773b-138">Mer information finns i [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="a773b-138">For more information, see [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a773b-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a773b-139">Next steps</span></span>
* <span data-ttu-id="a773b-140">Hämta felsökningstips och hjälpa på [har problem med tvåstegsverifiering](multi-factor-authentication-end-user-troubleshoot.md)</span><span class="sxs-lookup"><span data-stu-id="a773b-140">Get troubleshooting tips and help on [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md)</span></span>
* <span data-ttu-id="a773b-141">Ställ in [applösenord](multi-factor-authentication-end-user-app-passwords.md) för alla program som inte stöder tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="a773b-141">Set up [app passwords](multi-factor-authentication-end-user-app-passwords.md) for any apps that don't support two-step verification.</span></span>
