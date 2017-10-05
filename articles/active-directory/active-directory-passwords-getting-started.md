---
title: 'Snabbstart: Azure AD SSPR | Microsoft Docs'
description: "Distribuera återställning av lösenord för självbetjäning i Azure AD snabbt"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 07c7f3ad066c735054cb339f6e09aa4d7d23f23a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="quickstart-azure-ad-self-service-password-reset"></a><span data-ttu-id="29f06-103">Snabbstart: återställning av lösenord för Azure AD-självbetjäning</span><span class="sxs-lookup"><span data-stu-id="29f06-103">Quickstart: Azure AD self-service password reset</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29f06-104">**Är du här eftersom du har problem med att logga in?**</span><span class="sxs-lookup"><span data-stu-id="29f06-104">**Are you here because you're having problems signing in?**</span></span> <span data-ttu-id="29f06-105">I så fall är det [här som du ser hur du kan ändra och återställa ditt eget lösenord](active-directory-passwords-update-your-own-password.md).</span><span class="sxs-lookup"><span data-stu-id="29f06-105">If so, [here's how you can change and reset your own password](active-directory-passwords-update-your-own-password.md).</span></span>

## <a name="rapidly-deploy-self-service-password-reset"></a><span data-ttu-id="29f06-106">Distribuera återställning av lösenord för självbetjäning snabbt</span><span class="sxs-lookup"><span data-stu-id="29f06-106">Rapidly deploy self-service password reset</span></span>

<span data-ttu-id="29f06-107">Återställning av lösenord för självbetjäning (SSPR) erbjuder ett enkelt sätt för IT-administratörer att låta användarna återställa eller låsa upp sina lösenord eller sina konton.</span><span class="sxs-lookup"><span data-stu-id="29f06-107">Self-service password reset (SSPR) offers a simple means for IT administrators to empower users to reset or unlock their passwords or accounts.</span></span> <span data-ttu-id="29f06-108">Systemet innehåller detaljerade rapporter för att spåra när användare använder systemet tillsammans med aviseringar som informerar om missbruk.</span><span class="sxs-lookup"><span data-stu-id="29f06-108">The system includes detailed reporting to track when users use the system along with notifications to alert you to misuse or abuse.</span></span>

<span data-ttu-id="29f06-109">I den här handboken förutsätts att du redan har en aktiv utvärderingsversion eller licensierad Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="29f06-109">This guide assumes you already have a working trial or licensed Azure AD tenant.</span></span> <span data-ttu-id="29f06-110">Om du behöver hjälp med att konfigurera Azure AD kan du läsa artikeln [Komma igång med Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).</span><span class="sxs-lookup"><span data-stu-id="29f06-110">If you need help setting up Azure AD, see the article [Getting Started with Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).</span></span>

1. <span data-ttu-id="29f06-111">Från din befintliga Azure AD-klient väljer du **"Återställning av lösenord"**</span><span class="sxs-lookup"><span data-stu-id="29f06-111">From your existing Azure AD tenant, select **"Password reset"**</span></span>

2. <span data-ttu-id="29f06-112">På skärmen **"Egenskaper"**, under alternativet "Återställning av lösenord via självbetjäning har aktiverats" väljer du något av följande</span><span class="sxs-lookup"><span data-stu-id="29f06-112">From the **"Properties"** screen, under the option "Self Service Password Reset Enabled" choose one of the following</span></span>
    * <span data-ttu-id="29f06-113">Ingen – Ingen kan använda SSPR-funktionen</span><span class="sxs-lookup"><span data-stu-id="29f06-113">Nobody - No one is able to use SSPR functionality</span></span>
    * <span data-ttu-id="29f06-114">En grupp – Endast medlemmar av en viss Azure AD-grupp som du väljer kan använda SSPR-funktionen</span><span class="sxs-lookup"><span data-stu-id="29f06-114">A group - Only members of a specific Azure AD group that you choose are able to use SSPR functionality</span></span>
    * <span data-ttu-id="29f06-115">Alla – Alla användare med konton i din Azure AD-klient kan använda SSPR-funktionen</span><span class="sxs-lookup"><span data-stu-id="29f06-115">Everybody - All users with accounts in your Azure AD tenant are able to use SSPR functionality</span></span>

3. <span data-ttu-id="29f06-116">Från skärmen **"Autentiseringsmetoder"** väljer du</span><span class="sxs-lookup"><span data-stu-id="29f06-116">From the **"Authentication methods"** screen choose</span></span>
    * <span data-ttu-id="29f06-117">Antal metoder som krävs för återställning – Vi stöder minst en eller högst två</span><span class="sxs-lookup"><span data-stu-id="29f06-117">Number of methods required to reset - We support a minimum of one or a maximum of two</span></span>
    * <span data-ttu-id="29f06-118">Metoder som finns tillgängliga för användare – Vi behöver minst en, men det skadar inte att ha ett tillgängligt alternativ</span><span class="sxs-lookup"><span data-stu-id="29f06-118">Methods available to users - We need at least one but it never hurts to have an extra choice available</span></span>
        * <span data-ttu-id="29f06-119">**E-post** skickar ett e-postmeddelande med en kod till användarens konfigurerade e-postadress för autentisering</span><span class="sxs-lookup"><span data-stu-id="29f06-119">**Email** sends an email with a code to the user's configured authentication email address</span></span>
        * <span data-ttu-id="29f06-120">**Mobiltelefon** ger användaren möjlighet att ta emot ett samtal eller SMS med en kod till hans/hennes konfigurerade mobiltelefonnummer</span><span class="sxs-lookup"><span data-stu-id="29f06-120">**Mobile Phone** gives the user the choice to receive a call or text with a code to their configured mobile phone number</span></span>
        * <span data-ttu-id="29f06-121">**Arbetstelefon** ringer användaren med en kod till användarens konfigurerade arbetstelefonnummer</span><span class="sxs-lookup"><span data-stu-id="29f06-121">**Office Phone** calls the user with a code to their configured office phone number</span></span>
        * <span data-ttu-id="29f06-122">Med **Säkerhetsfrågor** kan du välja</span><span class="sxs-lookup"><span data-stu-id="29f06-122">**Security Questions** requires you to choose</span></span>
            * <span data-ttu-id="29f06-123">Antal frågor som krävs för registrering – Minimum för en lyckad registrering, vilket betyder att en användare kan välja att besvara flera för att skapa en pool med frågor att hämta från.</span><span class="sxs-lookup"><span data-stu-id="29f06-123">Number of questions required to register - The minimum for successful registration, meaning a user can choose to answer more to create a pool of questions to pull from.</span></span> <span data-ttu-id="29f06-124">Det här alternativet går att ställa in från 3 till 5 och måste vara större än eller lika med antalet frågor som krävs för att återställa.</span><span class="sxs-lookup"><span data-stu-id="29f06-124">This option can be set from 3-5 and must be greater than or equal to the number of questions required to reset.</span></span>
                * <span data-ttu-id="29f06-125">Det går att lägga till anpassade frågor genom att klicka på knappen ”Custom” (Anpassad) när du väljer säkerhetsfrågor</span><span class="sxs-lookup"><span data-stu-id="29f06-125">Custom questions can be added by clicking the "Custom" button when selecting security questions</span></span>
            * <span data-ttu-id="29f06-126">Antal frågor som krävs för återställning – Kan ställas in från 3 till 5 frågor som ska besvaras korrekt innan en användares lösenord kan återställas eller låsas upp.</span><span class="sxs-lookup"><span data-stu-id="29f06-126">Number of questions required to reset - Can be set from 3-5 questions to be answered correctly before allowing a users password to be reset or unlocked.</span></span>

4. <span data-ttu-id="29f06-127">REKOMMENDERAT: **"Anpassning"** gör att du kan ändra länken "Kontakta administratören" så att den pekar på en sida eller e-postadress som du definierar</span><span class="sxs-lookup"><span data-stu-id="29f06-127">RECOMMENDED: **"Customization"** allows you to change the "Contact your administrator" link to point to a page or email address you define</span></span>

5. <span data-ttu-id="29f06-128">VALFRITT: Skärmen **"Registrering"** ger administratörer alternativ för:</span><span class="sxs-lookup"><span data-stu-id="29f06-128">OPTIONAL: The **"Registration"** screen provides administrators the options for:</span></span>
    * <span data-ttu-id="29f06-129">Kräv att användare registrerar sig vid inloggning</span><span class="sxs-lookup"><span data-stu-id="29f06-129">Require users to register when signing in</span></span>
    * <span data-ttu-id="29f06-130">Antal dagar innan användare uppmanas att bekräfta sin autentiseringsinformation</span><span class="sxs-lookup"><span data-stu-id="29f06-130">Number of days before users are asked to reconfirm their authentication information</span></span>

6. <span data-ttu-id="29f06-131">VALFRITT: Skärmen **"Meddelande"** ger administratörer alternativ för att:</span><span class="sxs-lookup"><span data-stu-id="29f06-131">OPTIONAL: The **"Notification"** screen provides administrators the options to:</span></span>
    * <span data-ttu-id="29f06-132">Meddela användare om lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="29f06-132">Notify users on password resets</span></span>
    * <span data-ttu-id="29f06-133">Meddela alla administratörer när andra administratörer återställer sina lösenord</span><span class="sxs-lookup"><span data-stu-id="29f06-133">Notify all admins when other admins reset their password</span></span>

<span data-ttu-id="29f06-134">**Nu har du konfigurerat SSPR för din Azure AD-klient**.</span><span class="sxs-lookup"><span data-stu-id="29f06-134">**At this point, you have configured SSPR for your Azure AD tenant**.</span></span> <span data-ttu-id="29f06-135">Du kan avbryta här eller fortsätta att konfigurera synkroniseringen av lösenord till en lokal AD-domän.</span><span class="sxs-lookup"><span data-stu-id="29f06-135">You can stop here or continue on to configure synchronization of passwords to an on-premises AD domain.</span></span>

> [!NOTE]
> <span data-ttu-id="29f06-136">Testa SSPR med en användare och inte en administratör eftersom Microsoft tillämpar starka autentiseringskrav för Azure-konton av administratörstyp.</span><span class="sxs-lookup"><span data-stu-id="29f06-136">Test SSPR with a user and not an administrator as Microsoft enforces strong authentication requirements for Azure administrator type accounts.</span></span> <span data-ttu-id="29f06-137">Mer information om lösenordsprinciper för administratörer finns i vår [artikel om lösenordsprinciper](active-directory-passwords-policy.md#administrator-password-policy-differences).</span><span class="sxs-lookup"><span data-stu-id="29f06-137">For more information regarding the administrator password policy, see our [password policy article](active-directory-passwords-policy.md#administrator-password-policy-differences).</span></span>

## <a name="configure-synchronization-to-existing-identity-source"></a><span data-ttu-id="29f06-138">Konfigurera synkronisering till befintlig identitetskälla</span><span class="sxs-lookup"><span data-stu-id="29f06-138">Configure synchronization to existing identity source</span></span>

<span data-ttu-id="29f06-139">För att aktivera lokal identitetssynkronisering till Azure AD måste du installera och konfigurera [Azure AD Connect](./connect/active-directory-aadconnect.md) på en server i din organisation.</span><span class="sxs-lookup"><span data-stu-id="29f06-139">To enable on-premises identity synchronization to Azure AD, you need to install and configure [Azure AD Connect](./connect/active-directory-aadconnect.md) on a server in your organization.</span></span> <span data-ttu-id="29f06-140">Det här programmet hanterar synkronisering av användare och grupper från din befintliga identitetskälla till Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="29f06-140">This application handles synchronizing users and groups from your existing identity source to your Azure AD tenant.</span></span>

* [<span data-ttu-id="29f06-141">Uppgradera från DirSync eller AD Sync till Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="29f06-141">Upgrade from DirSync or Azure AD Sync to Azure AD Connect</span></span>](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [<span data-ttu-id="29f06-142">Komma igång med Azure AD Connect med standardinställningar</span><span class="sxs-lookup"><span data-stu-id="29f06-142">Getting started with Azure AD Connect using express settings</span></span>](./connect/active-directory-aadconnect-get-started-express.md)
* <span data-ttu-id="29f06-143">[Konfigurera tillbakaskrivning av lösenord](active-directory-passwords-writeback.md#configuring-password-writeback) för att skriva lösenord från Azure AD tillbaka till din lokala katalog.</span><span class="sxs-lookup"><span data-stu-id="29f06-143">[Configure password writeback](active-directory-passwords-writeback.md#configuring-password-writeback) to write passwords from Azure AD back to your on-premises directory.</span></span>

## <a name="disabling-self-service-password-reset"></a><span data-ttu-id="29f06-144">Inaktivera självbetjäningsfunktionen för återställning av lösenord</span><span class="sxs-lookup"><span data-stu-id="29f06-144">Disabling self-service password reset</span></span>

<span data-ttu-id="29f06-145">Om du vill inaktivera självbetjäningsfunktionen för återställning av lösenord öppnar du Azure AD-klienten och går till **Återställning av lösenord > Egenskaper** > och väljer **Ingen** under **Återställning av lösenord via självbetjäning har aktiverats**</span><span class="sxs-lookup"><span data-stu-id="29f06-145">Disabling self-service password reset is as simple as opening your Azure AD tenant and going to **Password Reset > Properties** > choose **None** under **Self Service Password Reset Enabled**</span></span>

### <a name="learn-more"></a><span data-ttu-id="29f06-146">Läs mer</span><span class="sxs-lookup"><span data-stu-id="29f06-146">Learn more</span></span>
<span data-ttu-id="29f06-147">Följande länkar ger ytterligare information om lösenordsåterställning med Azure AD</span><span class="sxs-lookup"><span data-stu-id="29f06-147">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="29f06-148">[**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering</span><span class="sxs-lookup"><span data-stu-id="29f06-148">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="29f06-149">[**Data**](active-directory-passwords-data.md) – Förstå de data som krävs och hur de används för lösenordshantering</span><span class="sxs-lookup"><span data-stu-id="29f06-149">[**Data**](active-directory-passwords-data.md) - Understand the data that is required and how it is used for password management</span></span>
* <span data-ttu-id="29f06-150">[**Distribution**](active-directory-passwords-best-practices.md) – Planera och distribuera SSPR till dina användare med hjälp av informationen finns här</span><span class="sxs-lookup"><span data-stu-id="29f06-150">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="29f06-151">[**Anpassa**](active-directory-passwords-customize.md) – Anpassa utseendet för företagets SSPR-funktion.</span><span class="sxs-lookup"><span data-stu-id="29f06-151">[**Customize**](active-directory-passwords-customize.md) - Customize the look and feel of the SSPR experience for your company.</span></span>
* <span data-ttu-id="29f06-152">[**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="29f06-152">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="29f06-153">[**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner</span><span class="sxs-lookup"><span data-stu-id="29f06-153">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="29f06-154">[**Teknisk djupdykning** ](active-directory-passwords-how-it-works.md) – Ta en titt bakom kulisserna för att förstå hur det hela fungerar</span><span class="sxs-lookup"><span data-stu-id="29f06-154">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="29f06-155">[**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man?</span><span class="sxs-lookup"><span data-stu-id="29f06-155">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="29f06-156">Varför?</span><span class="sxs-lookup"><span data-stu-id="29f06-156">Why?</span></span> <span data-ttu-id="29f06-157">Vad?</span><span class="sxs-lookup"><span data-stu-id="29f06-157">What?</span></span> <span data-ttu-id="29f06-158">Var?</span><span class="sxs-lookup"><span data-stu-id="29f06-158">Where?</span></span> <span data-ttu-id="29f06-159">Vem?</span><span class="sxs-lookup"><span data-stu-id="29f06-159">Who?</span></span> <span data-ttu-id="29f06-160">När?</span><span class="sxs-lookup"><span data-stu-id="29f06-160">When?</span></span> <span data-ttu-id="29f06-161">– Svar på allt du någonsin velat fråga</span><span class="sxs-lookup"><span data-stu-id="29f06-161">- Answers to questions you always wanted to ask</span></span>
* <span data-ttu-id="29f06-162">[**Felsökning** ](active-directory-passwords-troubleshoot.md) – Lär dig att lösa vanliga problem med SSPR</span><span class="sxs-lookup"><span data-stu-id="29f06-162">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>

## <a name="next-steps"></a><span data-ttu-id="29f06-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29f06-163">Next steps</span></span>

<span data-ttu-id="29f06-164">I den här snabbstarten har du lärt dig hur du ställer in självbetjäning för återställning av lösenord för användarna.</span><span class="sxs-lookup"><span data-stu-id="29f06-164">In this quickstart, you’ve learned how to configure self-service password reset for your users.</span></span> <span data-ttu-id="29f06-165">Följ länken nedan till Azure Portal om du vill fortsätta att följa de här anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="29f06-165">To continue to the Azure portal to complete these steps follow the link below to the portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="29f06-166">Aktivera lösenordsåterställning via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="29f06-166">Enable self-service password reset</span></span>](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

