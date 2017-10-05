---
title: 'Anpassa: Azure AD SSPR | Microsoft Docs'
description: "Anpassning av alternativ för Azure AD self service för lösenordsåterställning"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 8b9c120815473b25140b8717f8fdd539c97ebb04
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a><span data-ttu-id="82ddc-103">Anpassa Azure AD-funktionerna för självbetjäning av återställning av lösenord</span><span class="sxs-lookup"><span data-stu-id="82ddc-103">Customize Azure AD functionality for Self-Service Password Reset</span></span>

<span data-ttu-id="82ddc-104">IT-proffs vill distribuera lösenordsåterställning via självbetjäning kan anpassa upplevelsen för att matcha sina användare.</span><span class="sxs-lookup"><span data-stu-id="82ddc-104">IT Professionals looking to deploy self-service password reset can customize the experience to match their users.</span></span>

## <a name="customize-the-contact-your-administrator-link"></a><span data-ttu-id="82ddc-105">Anpassa kontakten din administratör</span><span class="sxs-lookup"><span data-stu-id="82ddc-105">Customize the contact your administrator link</span></span>

<span data-ttu-id="82ddc-106">Även om SSPR inte aktiveras återställa användare fortfarande en ”kontaktar du administratören” länka på lösenordet portal.</span><span class="sxs-lookup"><span data-stu-id="82ddc-106">Even if SSPR is not enabled users still a "contact your administrator" link on the password reset portal.</span></span>  <span data-ttu-id="82ddc-107">Klicka på den här länken e-post dina administratörer som ber om hjälp med att ändra användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="82ddc-107">Clicking this link emails your administrators asking for assistance in changing the user's password.</span></span> <span data-ttu-id="82ddc-108">Den här e-postmeddelande skickas till följande mottagare i följande ordning:</span><span class="sxs-lookup"><span data-stu-id="82ddc-108">This email is sent to the following recipients in the following order:</span></span>

1. <span data-ttu-id="82ddc-109">Om den **lösenordsadministratör** roll har tilldelats, administratörer med den här rollen meddelas</span><span class="sxs-lookup"><span data-stu-id="82ddc-109">If the **Password administrator** role is assigned, administrators with this role are notified</span></span>
2. <span data-ttu-id="82ddc-110">Om inga lösenordsadministratörer tilldelas, sedan administratörer med den **Användaradministratör** roll meddelas</span><span class="sxs-lookup"><span data-stu-id="82ddc-110">If no Password administrators are assigned, then administrators with the **User administrator** role are notified</span></span>
3. <span data-ttu-id="82ddc-111">Om ingen av de föregående rollerna har tilldelats, sedan **globala administratörer** meddelas</span><span class="sxs-lookup"><span data-stu-id="82ddc-111">If neither of the previous roles were assigned, then **Global administrators** are notified</span></span>

<span data-ttu-id="82ddc-112">I samtliga fall meddelas högst 100 mottagare.</span><span class="sxs-lookup"><span data-stu-id="82ddc-112">In all cases, a maximum of 100 recipients are notified.</span></span>

<span data-ttu-id="82ddc-113">Ta reda på mer om olika administratören roller och tilldela dem finns i dokumentet [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md)</span><span class="sxs-lookup"><span data-stu-id="82ddc-113">To find out more about the different administrator roles and how to assign them see the document [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md)</span></span>

### <a name="disable-contact-your-administrator-emails"></a><span data-ttu-id="82ddc-114">Inaktivera Kontakta din administratör e-post</span><span class="sxs-lookup"><span data-stu-id="82ddc-114">Disable contact your administrator emails</span></span>

<span data-ttu-id="82ddc-115">Om organisationen inte vill att få information om lösenord-administratörer återställa begäranden, kan följande konfiguration aktiveras</span><span class="sxs-lookup"><span data-stu-id="82ddc-115">If your organization does not want administrators notified about password reset requests, the following configuration can be enabled</span></span>

* <span data-ttu-id="82ddc-116">Aktivera Självbetjäning för återställning av lösenord för alla användare.</span><span class="sxs-lookup"><span data-stu-id="82ddc-116">Enable self-service password reset for all end users.</span></span> <span data-ttu-id="82ddc-117">Det här alternativet är **för återställning av lösenord > Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="82ddc-117">This option is under **Password Reset > Properties**.</span></span>
    * <span data-ttu-id="82ddc-118">Om du inte vill att användare kan återställa sina lösenord kan du definiera åtkomst till en tom grupp **rekommenderas inte det här alternativet**.</span><span class="sxs-lookup"><span data-stu-id="82ddc-118">If you do not wish users to reset their own passwords, you can scope access to an empty group **we do not recommend this option**.</span></span>
* <span data-ttu-id="82ddc-119">Anpassa länken supportavdelningen om du vill ange en Webbadress eller mailto: adress som användarna kan använda för att få hjälp.</span><span class="sxs-lookup"><span data-stu-id="82ddc-119">Customize the helpdesk link to provide a web URL or mailto: address that users can use to get assistance.</span></span> <span data-ttu-id="82ddc-120">Det här alternativet är **för återställning av lösenord > Anpassning > anpassad supportavdelningen e-postadress eller URL**.</span><span class="sxs-lookup"><span data-stu-id="82ddc-120">This option is under **Password Reset > Customization > Custom helpdesk email or URL**.</span></span>

## <a name="customize-adfs-sign-in-page-for-sspr"></a><span data-ttu-id="82ddc-121">Anpassa AD FS-inloggningssida för SSPR</span><span class="sxs-lookup"><span data-stu-id="82ddc-121">Customize ADFS sign-in page for SSPR</span></span>

<span data-ttu-id="82ddc-122">AD FS-administratörer kan lägga till en länk till deras inloggningssida med hjälp av vägledningen finns i artikeln [Lägg till inloggningssidan beskrivning](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).</span><span class="sxs-lookup"><span data-stu-id="82ddc-122">ADFS Administrators can add a link to their sign-in page using the guidance found in the article [Add sign-in page description](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).</span></span>

<span data-ttu-id="82ddc-123">Kommandot som följer på AD FS-servern lägger du till en länk till inloggningssidan för AD FS att tillåta användare att ange lösenordet för självbetjäning återställning arbetsflöde direkt.</span><span class="sxs-lookup"><span data-stu-id="82ddc-123">Using the command that follows on your ADFS server adds a link to the ADFS login page allowing users to enter the self-service password reset workflow directly.</span></span>

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-and-access-panel-look-and-feel"></a><span data-ttu-id="82ddc-124">Anpassa inloggnings- och åtkomst panelen Utseende</span><span class="sxs-lookup"><span data-stu-id="82ddc-124">Customize the sign-in and access panel look and feel</span></span>

<span data-ttu-id="82ddc-125">Du kan anpassa logotypen som visas tillsammans med avbildningen inloggningssidan så att de passar din företagsanpassning när användarna har åtkomst till inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="82ddc-125">When your users access the login page, you can customize the logo that appears along with the sign-in page image to fit your company branding.</span></span>

<span data-ttu-id="82ddc-126">Dessa bilder visas i följande fall:</span><span class="sxs-lookup"><span data-stu-id="82ddc-126">These graphics are shown in the following circumstances:</span></span>

* <span data-ttu-id="82ddc-127">När en användare skriver sitt lösenord</span><span class="sxs-lookup"><span data-stu-id="82ddc-127">After a user types their username</span></span>
* <span data-ttu-id="82ddc-128">Användare kommer åt anpassade url</span><span class="sxs-lookup"><span data-stu-id="82ddc-128">User accesses customized url</span></span>
    * <span data-ttu-id="82ddc-129">Genom att skicka ”wattimmar” parametern till lösenordet för att återställa sidan som ”https://login.microsoftonline.com/?whr=contoso.com”</span><span class="sxs-lookup"><span data-stu-id="82ddc-129">By passing the "whr" parameter to the password reset page, like "https://login.microsoftonline.com/?whr=contoso.com"</span></span>
    * <span data-ttu-id="82ddc-130">Genom att skicka ”användarnamn” parametern till lösenordet återställa sidan som ”https://login.microsoftonline.com/?username=admin@contoso.com”</span><span class="sxs-lookup"><span data-stu-id="82ddc-130">By passing the "username" parameter to the password reset page, like "https://login.microsoftonline.com/?username=admin@contoso.com"</span></span>

### <a name="graphics-details"></a><span data-ttu-id="82ddc-131">Information om grafik</span><span class="sxs-lookup"><span data-stu-id="82ddc-131">Graphics details</span></span>

<span data-ttu-id="82ddc-132">Följande inställningar kan du ändra de visuella egenskaperna på sidan logga in och finns under **Azure Active Directory**, **företagets anpassning**, **Redigera företagsinformation**</span><span class="sxs-lookup"><span data-stu-id="82ddc-132">The following settings allow you to change the visual characteristics of the sign-in page and can be found under **Azure Active Directory**, **Company branding**, **Edit company branding**</span></span>

* <span data-ttu-id="82ddc-133">Inloggningssidan bilden ska vara en PNG- eller JPG-fil 1420 × 1200 bildpunkter och inte större än 500KB.</span><span class="sxs-lookup"><span data-stu-id="82ddc-133">Sign-in page image should be a PNG or JPG file 1420x1200 pixels and no larger than 500KB.</span></span> <span data-ttu-id="82ddc-134">Vi rekommenderar att det ska vara runt 200 KB för bästa resultat.</span><span class="sxs-lookup"><span data-stu-id="82ddc-134">We recommend it to be around 200 KB for best results.</span></span>
* <span data-ttu-id="82ddc-135">Bakgrundsfärg för inloggningssidan används vid tidsfördröjning anslutningar och måste vara i formatet RGB hexadecimala.</span><span class="sxs-lookup"><span data-stu-id="82ddc-135">Sign-in page background color is used when on high-latency connections and must be in the RGB hex format.</span></span>
* <span data-ttu-id="82ddc-136">Banderoll ska vara en PNG- eller JPG-fil 60 × 280 bildpunkter och inte större än 10 KB.</span><span class="sxs-lookup"><span data-stu-id="82ddc-136">Banner image should be a PNG or JPG file 60x280 pixels and no larger than 10 KB.</span></span>
* <span data-ttu-id="82ddc-137">Kvadratisk logotyp (normalt och mörkt tema) PNG- eller JPG 240 x 240 (ändringsbara) inte är större än 10 KB.</span><span class="sxs-lookup"><span data-stu-id="82ddc-137">Square logo (normal and dark theme) PNG or JPG 240x240 (resizable) no larger than 10 KB.</span></span>

### <a name="sign-in-text-options"></a><span data-ttu-id="82ddc-138">Textalternativ-inloggning</span><span class="sxs-lookup"><span data-stu-id="82ddc-138">Sign-in text options</span></span>

<span data-ttu-id="82ddc-139">Följande inställningar kan du lägga till text på inloggningssidan relevanta för din organisation.</span><span class="sxs-lookup"><span data-stu-id="82ddc-139">The following settings allow you to add text to the sign-in page relevant to your organization.</span></span> <span data-ttu-id="82ddc-140">De här inställningarna kan hittas **Azure Active Directory**, **företagets anpassning**, **Redigera företagsinformation**</span><span class="sxs-lookup"><span data-stu-id="82ddc-140">These settings can be found under **Azure Active Directory**, **Company branding**, **Edit company branding**</span></span>

* <span data-ttu-id="82ddc-141">**Användaren namnet tipset** ersätter exempeltexten av someone@example.com med något mer lämpliga för dina användare, bör lämnas standard när stöder interna och externa användare</span><span class="sxs-lookup"><span data-stu-id="82ddc-141">**User name hint** replaces the example text of someone@example.com with something more appropriate for your users, recommended to be left default when supporting internal and external users</span></span>
* <span data-ttu-id="82ddc-142">**Inloggningssidan text** är högst 256 tecken.</span><span class="sxs-lookup"><span data-stu-id="82ddc-142">**Sign-in page text** is a maximum of 256 characters in length.</span></span> <span data-ttu-id="82ddc-143">Den här texten visas var som helst inloggning för användare online och i Azure AD Join-upplevelsen i Windows 10.</span><span class="sxs-lookup"><span data-stu-id="82ddc-143">This text appears anywhere your users login online, and in the Azure AD Join experience on Windows 10.</span></span> <span data-ttu-id="82ddc-144">Använd den här texten för villkor för användning, instruktioner och tips för dina användare.</span><span class="sxs-lookup"><span data-stu-id="82ddc-144">Use this text for terms of use, instructions, and tips for your users.</span></span> <span data-ttu-id="82ddc-145">**Alla kan se din inloggningssidan så ger inte någon känslig information.**</span><span class="sxs-lookup"><span data-stu-id="82ddc-145">**Anyone can see your sign-in page so do not provide any sensitive information here.**</span></span>

### <a name="keep-me-signed-in-disabled"></a><span data-ttu-id="82ddc-146">Håll mig inloggad inaktiverat</span><span class="sxs-lookup"><span data-stu-id="82ddc-146">Keep me signed in disabled</span></span>

<span data-ttu-id="82ddc-147">Alternativet tillåter ”Håll mig inloggad inaktiverad” användare att förbli inloggad i när de Stäng och öppna sina webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="82ddc-147">The option "Keep me signed in disabled" allows users to remain signed in when they close and reopen their browser window.</span></span> <span data-ttu-id="82ddc-148">Det här alternativet påverkar inte session livslängd.</span><span class="sxs-lookup"><span data-stu-id="82ddc-148">This option does not impact session lifetimes.</span></span> <span data-ttu-id="82ddc-149">Den här inställningen finns under **Azure Active Directory > företagets anpassning > Redigera företagsinformation**.</span><span class="sxs-lookup"><span data-stu-id="82ddc-149">This setting is found under **Azure Active Directory > Company branding > Edit company branding**.</span></span>

<span data-ttu-id="82ddc-150">Vissa funktioner i SharePoint Online och Office 2010 är beroende av användare kan den här kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="82ddc-150">Some features of SharePoint Online and Office 2010 have a dependency on users being able to check this box.</span></span> <span data-ttu-id="82ddc-151">Om du dölja det här alternativet kan användarna få ytterligare och oväntat inloggning anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="82ddc-151">If you hide this option, users may get additional and unexpected sign-in prompts.</span></span>

### <a name="directory-name"></a><span data-ttu-id="82ddc-152">Katalognamnet</span><span class="sxs-lookup"><span data-stu-id="82ddc-152">Directory name</span></span>

<span data-ttu-id="82ddc-153">Du kan ändra namnattributet under **Azure Active Directory > Egenskaper** att visa ett eget organisationsnamn som visas i portalen och automatisk kommunikation.</span><span class="sxs-lookup"><span data-stu-id="82ddc-153">You can change the name attribute under **Azure Active Directory > Properties** to show a friendly organization name seen in the portal and automated communications.</span></span> <span data-ttu-id="82ddc-154">Det här alternativet är mest synliga i form av automatiserade e-postmeddelanden i formulär som följer</span><span class="sxs-lookup"><span data-stu-id="82ddc-154">This option is most visible in the form of automated emails in the forms that follow</span></span>

* <span data-ttu-id="82ddc-155">Eget namn i e-post ”Microsoft uppdrag CONTOSO demo”</span><span class="sxs-lookup"><span data-stu-id="82ddc-155">Friendly name in email “Microsoft on behalf of CONTOSO demo”</span></span>
* <span data-ttu-id="82ddc-156">Ämnesrad i e-post ”CONTOSO demo konto e-Verifieringskod”</span><span class="sxs-lookup"><span data-stu-id="82ddc-156">Subject line in email “CONTOSO demo account email verification code”</span></span>

## <a name="next-steps"></a><span data-ttu-id="82ddc-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82ddc-157">Next steps</span></span>

<span data-ttu-id="82ddc-158">Följande länkar ger ytterligare information om lösenordsåterställning med Azure AD</span><span class="sxs-lookup"><span data-stu-id="82ddc-158">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="82ddc-159">[**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD</span><span class="sxs-lookup"><span data-stu-id="82ddc-159">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="82ddc-160">[**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering</span><span class="sxs-lookup"><span data-stu-id="82ddc-160">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="82ddc-161">[**Data**](active-directory-passwords-data.md) – Förstå de data som krävs och hur de används för lösenordshantering</span><span class="sxs-lookup"><span data-stu-id="82ddc-161">[**Data**](active-directory-passwords-data.md) - Understand the data that is required and how it is used for password management</span></span>
* <span data-ttu-id="82ddc-162">[**Distribution**](active-directory-passwords-best-practices.md) – Planera och distribuera SSPR till dina användare med hjälp av informationen finns här</span><span class="sxs-lookup"><span data-stu-id="82ddc-162">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="82ddc-163">[**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="82ddc-163">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="82ddc-164">[**Tillbakaskrivning av lösenord**](active-directory-passwords-writeback.md) – Hur fungerar tillbakaskrivning av lösenord tillsammans med din lokala katalog?</span><span class="sxs-lookup"><span data-stu-id="82ddc-164">[**Password Writeback**](active-directory-passwords-writeback.md) - How does password writeback work with your on-premises directory</span></span>
* <span data-ttu-id="82ddc-165">[**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner</span><span class="sxs-lookup"><span data-stu-id="82ddc-165">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="82ddc-166">[**Teknisk djupdykning** ](active-directory-passwords-how-it-works.md) – Ta en titt bakom kulisserna för att förstå hur det hela fungerar</span><span class="sxs-lookup"><span data-stu-id="82ddc-166">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="82ddc-167">[**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man?</span><span class="sxs-lookup"><span data-stu-id="82ddc-167">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="82ddc-168">Varför?</span><span class="sxs-lookup"><span data-stu-id="82ddc-168">Why?</span></span> <span data-ttu-id="82ddc-169">Vad?</span><span class="sxs-lookup"><span data-stu-id="82ddc-169">What?</span></span> <span data-ttu-id="82ddc-170">Var?</span><span class="sxs-lookup"><span data-stu-id="82ddc-170">Where?</span></span> <span data-ttu-id="82ddc-171">Vem?</span><span class="sxs-lookup"><span data-stu-id="82ddc-171">Who?</span></span> <span data-ttu-id="82ddc-172">När?</span><span class="sxs-lookup"><span data-stu-id="82ddc-172">When?</span></span> <span data-ttu-id="82ddc-173">– Svar på allt du någonsin velat fråga</span><span class="sxs-lookup"><span data-stu-id="82ddc-173">- Answers to questions you always wanted to ask</span></span>
* <span data-ttu-id="82ddc-174">[**Felsökning** ](active-directory-passwords-troubleshoot.md) – Lär dig att lösa vanliga problem med SSPR</span><span class="sxs-lookup"><span data-stu-id="82ddc-174">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>

