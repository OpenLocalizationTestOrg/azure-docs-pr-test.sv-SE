---
title: "Krav för Azure AD SSPR data | Microsoft Docs"
description: "Återställa datakrav för Azure AD-lösenordet för självbetjäning och hur toosatisfy dem."
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
ms.openlocfilehash: b68a1d7914dcd0bb4509d0e94914dc4309f4463a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a><span data-ttu-id="c689d-103">Distribuera utan slutanvändarregistrering för återställning av lösenord</span><span class="sxs-lookup"><span data-stu-id="c689d-103">Deploy password reset without requiring end-user registration</span></span>

<span data-ttu-id="c689d-104">Distribution av självbetjäningsportalen lösenord återställa (SSPR) kräver autentisering data toobe finns.</span><span class="sxs-lookup"><span data-stu-id="c689d-104">Deploying Self-Service Password Reset (SSPR) requires authentication data toobe present.</span></span> <span data-ttu-id="c689d-105">Vissa organisationer har användarna ange sina autentiseringsdata själva, men många organisationer föredrar toosynchronize med befintliga data i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c689d-105">Some organizations have their users enter their authentication data themselves, but many organizations prefer toosynchronize with existing data in Active Directory.</span></span> <span data-ttu-id="c689d-106">Om du har korrekt formaterade data i din lokala katalog och konfigurerar [Azure AD Connect med standardinställningar](./connect/active-directory-aadconnect-get-started-express.md), data blir tillgängliga tooAzure AD och SSPR utan användaråtgärder krävs.</span><span class="sxs-lookup"><span data-stu-id="c689d-106">If you have properly formatted data in your on-premises directory, and configure [Azure AD Connect using express settings](./connect/active-directory-aadconnect-get-started-express.md), that data is made available tooAzure AD and SSPR with no user interaction required.</span></span>

<span data-ttu-id="c689d-107">Alla telefonnummer måste vara i formatet hello + CountryCode PhoneNumber exempel: + 1 4255551234 toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="c689d-107">Any phone numbers must be in hello format +CountryCode PhoneNumber Example: +1 4255551234 toowork properly.</span></span>

> [!NOTE]
> <span data-ttu-id="c689d-108">Återställning av lösenord stöder inte telefonanknytningar.</span><span class="sxs-lookup"><span data-stu-id="c689d-108">Password reset does not support phone extensions.</span></span> <span data-ttu-id="c689d-109">Tillägg tas bort innan hello görs även i hello + 1 4255551234 X 12345-format.</span><span class="sxs-lookup"><span data-stu-id="c689d-109">Even in hello +1 4255551234X12345 format, extensions are removed before hello call is placed.</span></span>

## <a name="fields-populated"></a><span data-ttu-id="c689d-110">Fält</span><span class="sxs-lookup"><span data-stu-id="c689d-110">Fields populated</span></span>

<span data-ttu-id="c689d-111">Om du använder standardinställningarna för hello i Azure AD Connect hello följande görs mappningar.</span><span class="sxs-lookup"><span data-stu-id="c689d-111">If you use hello default settings in Azure AD Connect hello following mappings are made.</span></span>

| <span data-ttu-id="c689d-112">Lokala AD</span><span class="sxs-lookup"><span data-stu-id="c689d-112">On-premises AD</span></span> | <span data-ttu-id="c689d-113">Azure AD</span><span class="sxs-lookup"><span data-stu-id="c689d-113">Azure AD</span></span> | <span data-ttu-id="c689d-114">Azure AD Authentication kontaktinformation</span><span class="sxs-lookup"><span data-stu-id="c689d-114">Azure AD Authentication contact info</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c689d-115">telephoneNumber</span><span class="sxs-lookup"><span data-stu-id="c689d-115">telephoneNumber</span></span> | <span data-ttu-id="c689d-116">Arbetstelefon</span><span class="sxs-lookup"><span data-stu-id="c689d-116">Office phone</span></span> | <span data-ttu-id="c689d-117">Telefon</span><span class="sxs-lookup"><span data-stu-id="c689d-117">Alternate phone</span></span> |
| <span data-ttu-id="c689d-118">mobila</span><span class="sxs-lookup"><span data-stu-id="c689d-118">mobile</span></span> | <span data-ttu-id="c689d-119">Mobiltelefon</span><span class="sxs-lookup"><span data-stu-id="c689d-119">Mobile phone</span></span> | <span data-ttu-id="c689d-120">Telefon</span><span class="sxs-lookup"><span data-stu-id="c689d-120">Phone</span></span> |


## <a name="security-questions-and-answers"></a><span data-ttu-id="c689d-121">Säkerhetsfrågor och svar</span><span class="sxs-lookup"><span data-stu-id="c689d-121">Security questions and answers</span></span>

<span data-ttu-id="c689d-122">Säkerhetsfrågor och svar lagras på ett säkert sätt i Azure AD-klienten och är endast tillgänglig toousers via hello [SSPR-registreringsportalen](https://aka.ms/ssprsetup).</span><span class="sxs-lookup"><span data-stu-id="c689d-122">Security questions and answers are stored securely in your Azure AD tenant and are only accessible toousers via hello [SSPR registration portal](https://aka.ms/ssprsetup).</span></span> <span data-ttu-id="c689d-123">Administratörer kan inte se eller ändra hello innehållet i en annan användare frågor och svar.</span><span class="sxs-lookup"><span data-stu-id="c689d-123">Administrators can't see or modify hello contents of another users questions and answers.</span></span>

### <a name="what-happens-when-a-user-registers"></a><span data-ttu-id="c689d-124">Vad händer när en användare som registrerar</span><span class="sxs-lookup"><span data-stu-id="c689d-124">What happens when a user registers</span></span>

<span data-ttu-id="c689d-125">När en användare som registrerar anger hello registreringssidan hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="c689d-125">When a user registers, hello registration page sets hello following fields:</span></span>

* <span data-ttu-id="c689d-126">Telefon för autentisering</span><span class="sxs-lookup"><span data-stu-id="c689d-126">Authentication Phone</span></span>
* <span data-ttu-id="c689d-127">E-post för autentisering</span><span class="sxs-lookup"><span data-stu-id="c689d-127">Authentication Email</span></span>
* <span data-ttu-id="c689d-128">Säkerhetsfrågor och svar</span><span class="sxs-lookup"><span data-stu-id="c689d-128">Security Questions and Answers</span></span>

<span data-ttu-id="c689d-129">Om du har angett ett värde för **mobiltelefon** eller **alternativa e-postadress**, omedelbart fjärranvändning av dessa värden tooreset sina lösenord, även om de inte har registrerats för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c689d-129">If you have provided a value for **Mobile Phone** or **Alternate Email**, users can immediately use those values tooreset their passwords, even if they haven't registered for hello service.</span></span> <span data-ttu-id="c689d-130">Dessutom kan användare se dessa värden när du registrerar dig för hello första gången och ändra dem om de vill.</span><span class="sxs-lookup"><span data-stu-id="c689d-130">In addition, users see those values when registering for hello first time, and modify them if they wish.</span></span> <span data-ttu-id="c689d-131">När de har registrerat värdena kommer att sparas i hello **telefon för autentisering** och **autentisering e-post** respektive fält.</span><span class="sxs-lookup"><span data-stu-id="c689d-131">After they successfully register, these values will be persisted in hello **Authentication Phone** and **Authentication Email** fields, respectively.</span></span>

## <a name="set-and-read-authentication-data-using-powershell"></a><span data-ttu-id="c689d-132">Ange och läsa autentiseringsdata med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="c689d-132">Set and read authentication data using PowerShell</span></span>

<span data-ttu-id="c689d-133">hello följande fält kan anges med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="c689d-133">hello following fields can be set using PowerShell</span></span>

* <span data-ttu-id="c689d-134">Alternativ e-postadress</span><span class="sxs-lookup"><span data-stu-id="c689d-134">Alternate Email</span></span>
* <span data-ttu-id="c689d-135">Mobiltelefon</span><span class="sxs-lookup"><span data-stu-id="c689d-135">Mobile Phone</span></span>
* <span data-ttu-id="c689d-136">Arbetstelefon - kan bara anges om inte synkroniseras med en lokal katalog</span><span class="sxs-lookup"><span data-stu-id="c689d-136">Office Phone - Can only be set if not synchronizing with an on-premises directory</span></span>

### <a name="using-powershell-v1"></a><span data-ttu-id="c689d-137">Med hjälp av PowerShell V1</span><span class="sxs-lookup"><span data-stu-id="c689d-137">Using PowerShell V1</span></span>

<span data-ttu-id="c689d-138">tooget igång, du behöver för[ladda ned och installera hello Azure AD PowerShell-modulen](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).</span><span class="sxs-lookup"><span data-stu-id="c689d-138">tooget started, you need too[download and install hello Azure AD PowerShell module](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).</span></span> <span data-ttu-id="c689d-139">När du har installerat kan följa du hello steg som följer tooconfigure varje fält.</span><span class="sxs-lookup"><span data-stu-id="c689d-139">Once you have it installed, you can follow hello steps that follow tooconfigure each field.</span></span>

#### <a name="set-authentication-data-with-powershell-v1"></a><span data-ttu-id="c689d-140">Ange autentiseringsdata med PowerShell V1</span><span class="sxs-lookup"><span data-stu-id="c689d-140">Set Authentication Data with PowerShell V1</span></span>

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-authentication-data-with-powershellpowershell-v1"></a><span data-ttu-id="c689d-141">Läs autentiseringsdata i PowerShellPowerShell V1</span><span class="sxs-lookup"><span data-stu-id="c689d-141">Read Authentication Data with PowerShellPowerShell V1</span></span>

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-hello-commands-that-follow"></a><span data-ttu-id="c689d-142">Telefon för autentisering och autentisering e-post kan endast läsas med hjälp av Powershell V1 med hello kommandon som följer</span><span class="sxs-lookup"><span data-stu-id="c689d-142">Authentication Phone and Authentication Email can only be read using Powershell V1 using hello commands that follow</span></span>

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a><span data-ttu-id="c689d-143">Med hjälp av PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="c689d-143">Using PowerShell V2</span></span>

<span data-ttu-id="c689d-144">tooget igång, du behöver för[ladda ned och installera hello V2 för Azure AD PowerShell-modulen](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md).</span><span class="sxs-lookup"><span data-stu-id="c689d-144">tooget started, you need too[download and install hello Azure AD V2 PowerShell module](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md).</span></span> <span data-ttu-id="c689d-145">När du har installerat kan följa du hello steg som följer tooconfigure varje fält.</span><span class="sxs-lookup"><span data-stu-id="c689d-145">Once you have it installed, you can follow hello steps that follow tooconfigure each field.</span></span>

<span data-ttu-id="c689d-146">tooinstall snabbt från senare versioner av PowerShell som har stöd för installation-modulen kör kommandona (hello första raden bara kontrolleras toosee om det redan är installerat):</span><span class="sxs-lookup"><span data-stu-id="c689d-146">tooinstall quickly from recent versions of PowerShell that support Install-Module, run these commands (hello first line simply checks toosee if it's installed already):</span></span>

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-authentication-data-with-powershell-v2"></a><span data-ttu-id="c689d-147">Ange autentiseringsdata med PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="c689d-147">Set Authentication Data with PowerShell V2</span></span>

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

### <a name="read-authentication-data-with-powershell-v2"></a><span data-ttu-id="c689d-148">Läs autentiseringsdata med PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="c689d-148">Read Authentication Data with PowerShell V2</span></span>

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a><span data-ttu-id="c689d-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c689d-149">Next steps</span></span>

<span data-ttu-id="c689d-150">hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD</span><span class="sxs-lookup"><span data-stu-id="c689d-150">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="c689d-151">[**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c689d-151">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="c689d-152">[**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering</span><span class="sxs-lookup"><span data-stu-id="c689d-152">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="c689d-153">[**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här</span><span class="sxs-lookup"><span data-stu-id="c689d-153">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="c689d-154">[**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.</span><span class="sxs-lookup"><span data-stu-id="c689d-154">[**Customize**](active-directory-passwords-customize.md) - Customize hello look and feel of hello SSPR experience for your company.</span></span>
* <span data-ttu-id="c689d-155">[**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="c689d-155">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="c689d-156">[**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner</span><span class="sxs-lookup"><span data-stu-id="c689d-156">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="c689d-157">[**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="c689d-157">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="c689d-158">[**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man?</span><span class="sxs-lookup"><span data-stu-id="c689d-158">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="c689d-159">Varför?</span><span class="sxs-lookup"><span data-stu-id="c689d-159">Why?</span></span> <span data-ttu-id="c689d-160">Vad?</span><span class="sxs-lookup"><span data-stu-id="c689d-160">What?</span></span> <span data-ttu-id="c689d-161">Var?</span><span class="sxs-lookup"><span data-stu-id="c689d-161">Where?</span></span> <span data-ttu-id="c689d-162">Vem?</span><span class="sxs-lookup"><span data-stu-id="c689d-162">Who?</span></span> <span data-ttu-id="c689d-163">När?</span><span class="sxs-lookup"><span data-stu-id="c689d-163">When?</span></span> <span data-ttu-id="c689d-164">-Svar tooquestions du alltid vill ha tooask</span><span class="sxs-lookup"><span data-stu-id="c689d-164">- Answers tooquestions you always wanted tooask</span></span>
* <span data-ttu-id="c689d-165">[**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR</span><span class="sxs-lookup"><span data-stu-id="c689d-165">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>
