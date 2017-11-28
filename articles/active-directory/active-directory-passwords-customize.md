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
ms.openlocfilehash: 4762fffef040f9b409355f9ee0e8cc593e3eea0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a><span data-ttu-id="77de4-103">Anpassa Azure AD-funktionerna för självbetjäning av återställning av lösenord</span><span class="sxs-lookup"><span data-stu-id="77de4-103">Customize Azure AD functionality for Self-Service Password Reset</span></span>

<span data-ttu-id="77de4-104">IT-proffs söka toodeploy lösenordsåterställning via självbetjäning kan användarna anpassa hello upplevelse toomatch.</span><span class="sxs-lookup"><span data-stu-id="77de4-104">IT Professionals looking toodeploy self-service password reset can customize hello experience toomatch their users.</span></span>

## <a name="customize-hello-contact-your-administrator-link"></a><span data-ttu-id="77de4-105">Anpassa hello Kontakta din administratör</span><span class="sxs-lookup"><span data-stu-id="77de4-105">Customize hello contact your administrator link</span></span>

<span data-ttu-id="77de4-106">Även om SSPR inte är aktiverade användare återställa fortfarande en ”kontaktar du administratören” länk på hello lösenord portal.</span><span class="sxs-lookup"><span data-stu-id="77de4-106">Even if SSPR is not enabled users still a "contact your administrator" link on hello password reset portal.</span></span>  <span data-ttu-id="77de4-107">Klicka på den här länken e-post dina administratörer som ber om hjälp med att ändra hello användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="77de4-107">Clicking this link emails your administrators asking for assistance in changing hello user's password.</span></span> <span data-ttu-id="77de4-108">Den här e-postmeddelande skickas toohello följande mottagare i hello följande ordning:</span><span class="sxs-lookup"><span data-stu-id="77de4-108">This email is sent toohello following recipients in hello following order:</span></span>

1. <span data-ttu-id="77de4-109">Om hello **lösenordsadministratör** roll har tilldelats, administratörer med den här rollen meddelas</span><span class="sxs-lookup"><span data-stu-id="77de4-109">If hello **Password administrator** role is assigned, administrators with this role are notified</span></span>
2. <span data-ttu-id="77de4-110">Om inga lösenordsadministratörer tilldelas, sedan administratörer med hello **Användaradministratör** roll meddelas</span><span class="sxs-lookup"><span data-stu-id="77de4-110">If no Password administrators are assigned, then administrators with hello **User administrator** role are notified</span></span>
3. <span data-ttu-id="77de4-111">Om inget av föregående hello-roller har tilldelats, sedan **globala administratörer** meddelas</span><span class="sxs-lookup"><span data-stu-id="77de4-111">If neither of hello previous roles were assigned, then **Global administrators** are notified</span></span>

<span data-ttu-id="77de4-112">I samtliga fall meddelas högst 100 mottagare.</span><span class="sxs-lookup"><span data-stu-id="77de4-112">In all cases, a maximum of 100 recipients are notified.</span></span>

<span data-ttu-id="77de4-113">toofind mer information om hello olika administratörsroller och hur tooassign dem finns hello dokumentet [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md)</span><span class="sxs-lookup"><span data-stu-id="77de4-113">toofind out more about hello different administrator roles and how tooassign them see hello document [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md)</span></span>

### <a name="disable-contact-your-administrator-emails"></a><span data-ttu-id="77de4-114">Inaktivera Kontakta din administratör e-post</span><span class="sxs-lookup"><span data-stu-id="77de4-114">Disable contact your administrator emails</span></span>

<span data-ttu-id="77de4-115">Om organisationen inte vill att få information om lösenord-administratörer återställa begäranden, kan hello följande konfiguration aktiveras</span><span class="sxs-lookup"><span data-stu-id="77de4-115">If your organization does not want administrators notified about password reset requests, hello following configuration can be enabled</span></span>

* <span data-ttu-id="77de4-116">Aktivera Självbetjäning för återställning av lösenord för alla användare.</span><span class="sxs-lookup"><span data-stu-id="77de4-116">Enable self-service password reset for all end users.</span></span> <span data-ttu-id="77de4-117">Det här alternativet är **för återställning av lösenord > Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="77de4-117">This option is under **Password Reset > Properties**.</span></span>
    * <span data-ttu-id="77de4-118">Om du inte vill att användarna tooreset sina egna lösenord, du kan definiera åtkomst tooan tom grupp **rekommenderas inte det här alternativet**.</span><span class="sxs-lookup"><span data-stu-id="77de4-118">If you do not wish users tooreset their own passwords, you can scope access tooan empty group **we do not recommend this option**.</span></span>
* <span data-ttu-id="77de4-119">Anpassa hello supportavdelningen länken tooprovide en Webbadress eller mailto: adress som användare kan använda tooget hjälp.</span><span class="sxs-lookup"><span data-stu-id="77de4-119">Customize hello helpdesk link tooprovide a web URL or mailto: address that users can use tooget assistance.</span></span> <span data-ttu-id="77de4-120">Det här alternativet är **för återställning av lösenord > Anpassning > anpassad supportavdelningen e-postadress eller URL**.</span><span class="sxs-lookup"><span data-stu-id="77de4-120">This option is under **Password Reset > Customization > Custom helpdesk email or URL**.</span></span>

## <a name="customize-adfs-sign-in-page-for-sspr"></a><span data-ttu-id="77de4-121">Anpassa AD FS-inloggningssida för SSPR</span><span class="sxs-lookup"><span data-stu-id="77de4-121">Customize ADFS sign-in page for SSPR</span></span>

<span data-ttu-id="77de4-122">AD FS-administratörer kan lägga till en länk tootheir inloggningssidan med hello vägledning finns i artikel hello [Lägg till inloggningssidan beskrivning](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).</span><span class="sxs-lookup"><span data-stu-id="77de4-122">ADFS Administrators can add a link tootheir sign-in page using hello guidance found in hello article [Add sign-in page description](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).</span></span>

<span data-ttu-id="77de4-123">Kommandot hello som följer på AD FS-servern lägger till en länk toohello ADFS inloggningssidan så att användare tooenter hello självbetjäning lösenord Återställ arbetsflöde direkt.</span><span class="sxs-lookup"><span data-stu-id="77de4-123">Using hello command that follows on your ADFS server adds a link toohello ADFS login page allowing users tooenter hello self-service password reset workflow directly.</span></span>

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-hello-sign-in-and-access-panel-look-and-feel"></a><span data-ttu-id="77de4-124">Anpassa hello inloggning och åtkomst panelen Utseende</span><span class="sxs-lookup"><span data-stu-id="77de4-124">Customize hello sign-in and access panel look and feel</span></span>

<span data-ttu-id="77de4-125">Du kan anpassa hello logotyp som visas tillsammans med hello-inloggningssida avbildningen toofit din företagsanpassning när användarna har åtkomst till hello inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="77de4-125">When your users access hello login page, you can customize hello logo that appears along with hello sign-in page image toofit your company branding.</span></span>

<span data-ttu-id="77de4-126">Dessa bilder visas i hello följande omständigheter:</span><span class="sxs-lookup"><span data-stu-id="77de4-126">These graphics are shown in hello following circumstances:</span></span>

* <span data-ttu-id="77de4-127">När en användare skriver sitt lösenord</span><span class="sxs-lookup"><span data-stu-id="77de4-127">After a user types their username</span></span>
* <span data-ttu-id="77de4-128">Användare kommer åt anpassade url</span><span class="sxs-lookup"><span data-stu-id="77de4-128">User accesses customized url</span></span>
    * <span data-ttu-id="77de4-129">Genom att skicka hello lösenordsåterställning ”wattimmar” parametern toohello sida som ”https://login.microsoftonline.com/?whr=contoso.com”</span><span class="sxs-lookup"><span data-stu-id="77de4-129">By passing hello "whr" parameter toohello password reset page, like "https://login.microsoftonline.com/?whr=contoso.com"</span></span>
    * <span data-ttu-id="77de4-130">Genom att skicka hello ”användarnamn” parametern toohello lösenordsåterställning sida, som ”https://login.microsoftonline.com/?username=admin@contoso.com”</span><span class="sxs-lookup"><span data-stu-id="77de4-130">By passing hello "username" parameter toohello password reset page, like "https://login.microsoftonline.com/?username=admin@contoso.com"</span></span>

### <a name="graphics-details"></a><span data-ttu-id="77de4-131">Information om grafik</span><span class="sxs-lookup"><span data-stu-id="77de4-131">Graphics details</span></span>

<span data-ttu-id="77de4-132">hello följande inställningar kan du toochange hello visuella egenskaper hello inloggningssidan och kan hittas **Azure Active Directory**, **företagets anpassning**, **redigera företag anpassning**</span><span class="sxs-lookup"><span data-stu-id="77de4-132">hello following settings allow you toochange hello visual characteristics of hello sign-in page and can be found under **Azure Active Directory**, **Company branding**, **Edit company branding**</span></span>

* <span data-ttu-id="77de4-133">Inloggningssidan bilden ska vara en PNG- eller JPG-fil 1420 × 1200 bildpunkter och inte större än 500KB.</span><span class="sxs-lookup"><span data-stu-id="77de4-133">Sign-in page image should be a PNG or JPG file 1420x1200 pixels and no larger than 500KB.</span></span> <span data-ttu-id="77de4-134">Vi rekommenderar att den toobe runt 200 KB för bästa resultat.</span><span class="sxs-lookup"><span data-stu-id="77de4-134">We recommend it toobe around 200 KB for best results.</span></span>
* <span data-ttu-id="77de4-135">Bakgrundsfärg för inloggningssidan används vid tidsfördröjning anslutningar och måste vara i hello RGB-hexadecimalt format.</span><span class="sxs-lookup"><span data-stu-id="77de4-135">Sign-in page background color is used when on high-latency connections and must be in hello RGB hex format.</span></span>
* <span data-ttu-id="77de4-136">Banderoll ska vara en PNG- eller JPG-fil 60 × 280 bildpunkter och inte större än 10 KB.</span><span class="sxs-lookup"><span data-stu-id="77de4-136">Banner image should be a PNG or JPG file 60x280 pixels and no larger than 10 KB.</span></span>
* <span data-ttu-id="77de4-137">Kvadratisk logotyp (normalt och mörkt tema) PNG- eller JPG 240 x 240 (ändringsbara) inte är större än 10 KB.</span><span class="sxs-lookup"><span data-stu-id="77de4-137">Square logo (normal and dark theme) PNG or JPG 240x240 (resizable) no larger than 10 KB.</span></span>

### <a name="sign-in-text-options"></a><span data-ttu-id="77de4-138">Textalternativ-inloggning</span><span class="sxs-lookup"><span data-stu-id="77de4-138">Sign-in text options</span></span>

<span data-ttu-id="77de4-139">hello följande inställningar kan du tooadd text toohello inloggningssidan relevanta tooyour organisation.</span><span class="sxs-lookup"><span data-stu-id="77de4-139">hello following settings allow you tooadd text toohello sign-in page relevant tooyour organization.</span></span> <span data-ttu-id="77de4-140">De här inställningarna kan hittas **Azure Active Directory**, **företagets anpassning**, **Redigera företagsinformation**</span><span class="sxs-lookup"><span data-stu-id="77de4-140">These settings can be found under **Azure Active Directory**, **Company branding**, **Edit company branding**</span></span>

* <span data-ttu-id="77de4-141">**Användaren namnet tipset** ersätter hello exempeltexten av someone@example.com med något mer lämpliga för dina användare, bör toobe vänstra standard när stöder interna och externa användare</span><span class="sxs-lookup"><span data-stu-id="77de4-141">**User name hint** replaces hello example text of someone@example.com with something more appropriate for your users, recommended toobe left default when supporting internal and external users</span></span>
* <span data-ttu-id="77de4-142">**Inloggningssidan text** är högst 256 tecken.</span><span class="sxs-lookup"><span data-stu-id="77de4-142">**Sign-in page text** is a maximum of 256 characters in length.</span></span> <span data-ttu-id="77de4-143">Den här texten visas någonstans inloggning för användare online och i hello Azure AD Join-upplevelsen i Windows 10.</span><span class="sxs-lookup"><span data-stu-id="77de4-143">This text appears anywhere your users login online, and in hello Azure AD Join experience on Windows 10.</span></span> <span data-ttu-id="77de4-144">Använd den här texten för villkor för användning, instruktioner och tips för dina användare.</span><span class="sxs-lookup"><span data-stu-id="77de4-144">Use this text for terms of use, instructions, and tips for your users.</span></span> <span data-ttu-id="77de4-145">**Alla kan se din inloggningssidan så ger inte någon känslig information.**</span><span class="sxs-lookup"><span data-stu-id="77de4-145">**Anyone can see your sign-in page so do not provide any sensitive information here.**</span></span>

### <a name="keep-me-signed-in-disabled"></a><span data-ttu-id="77de4-146">Håll mig inloggad inaktiverat</span><span class="sxs-lookup"><span data-stu-id="77de4-146">Keep me signed in disabled</span></span>

<span data-ttu-id="77de4-147">hello alternativet ”Jag vill förbli inloggad inaktiverat” kan användare tooremain inloggad när de Stäng och öppna sina webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="77de4-147">hello option "Keep me signed in disabled" allows users tooremain signed in when they close and reopen their browser window.</span></span> <span data-ttu-id="77de4-148">Det här alternativet påverkar inte session livslängd.</span><span class="sxs-lookup"><span data-stu-id="77de4-148">This option does not impact session lifetimes.</span></span> <span data-ttu-id="77de4-149">Den här inställningen finns under **Azure Active Directory > företagets anpassning > Redigera företagsinformation**.</span><span class="sxs-lookup"><span data-stu-id="77de4-149">This setting is found under **Azure Active Directory > Company branding > Edit company branding**.</span></span>

<span data-ttu-id="77de4-150">Vissa funktioner i SharePoint Online och Office 2010 är beroende av användare kan toocheck den här rutan.</span><span class="sxs-lookup"><span data-stu-id="77de4-150">Some features of SharePoint Online and Office 2010 have a dependency on users being able toocheck this box.</span></span> <span data-ttu-id="77de4-151">Om du dölja det här alternativet kan användarna få ytterligare och oväntat inloggning anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="77de4-151">If you hide this option, users may get additional and unexpected sign-in prompts.</span></span>

### <a name="directory-name"></a><span data-ttu-id="77de4-152">Katalognamnet</span><span class="sxs-lookup"><span data-stu-id="77de4-152">Directory name</span></span>

<span data-ttu-id="77de4-153">Du kan ändra hello namnattributet under **Azure Active Directory > Egenskaper** tooshow ett eget organisationsnamn ses i hello portal och automatisk kommunikation.</span><span class="sxs-lookup"><span data-stu-id="77de4-153">You can change hello name attribute under **Azure Active Directory > Properties** tooshow a friendly organization name seen in hello portal and automated communications.</span></span> <span data-ttu-id="77de4-154">Det här alternativet är mest synliga i hello form av automatiserade e-postmeddelanden i hello formulär som följer</span><span class="sxs-lookup"><span data-stu-id="77de4-154">This option is most visible in hello form of automated emails in hello forms that follow</span></span>

* <span data-ttu-id="77de4-155">Eget namn i e-post ”Microsoft uppdrag CONTOSO demo”</span><span class="sxs-lookup"><span data-stu-id="77de4-155">Friendly name in email “Microsoft on behalf of CONTOSO demo”</span></span>
* <span data-ttu-id="77de4-156">Ämnesrad i e-post ”CONTOSO demo konto e-Verifieringskod”</span><span class="sxs-lookup"><span data-stu-id="77de4-156">Subject line in email “CONTOSO demo account email verification code”</span></span>

## <a name="next-steps"></a><span data-ttu-id="77de4-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="77de4-157">Next steps</span></span>

<span data-ttu-id="77de4-158">hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD</span><span class="sxs-lookup"><span data-stu-id="77de4-158">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="77de4-159">[**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD</span><span class="sxs-lookup"><span data-stu-id="77de4-159">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="77de4-160">[**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering</span><span class="sxs-lookup"><span data-stu-id="77de4-160">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="77de4-161">[**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering</span><span class="sxs-lookup"><span data-stu-id="77de4-161">[**Data**](active-directory-passwords-data.md) - Understand hello data that is required and how it is used for password management</span></span>
* <span data-ttu-id="77de4-162">[**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här</span><span class="sxs-lookup"><span data-stu-id="77de4-162">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="77de4-163">[**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="77de4-163">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="77de4-164">[**Tillbakaskrivning av lösenord**](active-directory-passwords-writeback.md) – Hur fungerar tillbakaskrivning av lösenord tillsammans med din lokala katalog?</span><span class="sxs-lookup"><span data-stu-id="77de4-164">[**Password Writeback**](active-directory-passwords-writeback.md) - How does password writeback work with your on-premises directory</span></span>
* <span data-ttu-id="77de4-165">[**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner</span><span class="sxs-lookup"><span data-stu-id="77de4-165">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="77de4-166">[**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="77de4-166">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="77de4-167">[**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man?</span><span class="sxs-lookup"><span data-stu-id="77de4-167">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="77de4-168">Varför?</span><span class="sxs-lookup"><span data-stu-id="77de4-168">Why?</span></span> <span data-ttu-id="77de4-169">Vad?</span><span class="sxs-lookup"><span data-stu-id="77de4-169">What?</span></span> <span data-ttu-id="77de4-170">Var?</span><span class="sxs-lookup"><span data-stu-id="77de4-170">Where?</span></span> <span data-ttu-id="77de4-171">Vem?</span><span class="sxs-lookup"><span data-stu-id="77de4-171">Who?</span></span> <span data-ttu-id="77de4-172">När?</span><span class="sxs-lookup"><span data-stu-id="77de4-172">When?</span></span> <span data-ttu-id="77de4-173">-Svar tooquestions du alltid vill ha tooask</span><span class="sxs-lookup"><span data-stu-id="77de4-173">- Answers tooquestions you always wanted tooask</span></span>
* <span data-ttu-id="77de4-174">[**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR</span><span class="sxs-lookup"><span data-stu-id="77de4-174">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>

