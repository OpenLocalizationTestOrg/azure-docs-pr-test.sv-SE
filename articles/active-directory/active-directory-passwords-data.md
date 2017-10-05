---
title: "Krav för Azure AD SSPR data | Microsoft Docs"
description: "Uppgifter som krävs för Azure AD-lösenordet för självbetjäning återställning och hur du uppfylla dem."
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
ms.openlocfilehash: 2d1afd2d1265b371e0d311ed70fffbc55874b0a7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a><span data-ttu-id="84d78-103">Distribuera utan slutanvändarregistrering för återställning av lösenord</span><span class="sxs-lookup"><span data-stu-id="84d78-103">Deploy password reset without requiring end-user registration</span></span>

<span data-ttu-id="84d78-104">Distribution av självbetjäningsportalen lösenord återställa (SSPR) kräver autentiseringsdata finnas.</span><span class="sxs-lookup"><span data-stu-id="84d78-104">Deploying Self-Service Password Reset (SSPR) requires authentication data to be present.</span></span> <span data-ttu-id="84d78-105">Vissa organisationer har användarna ange sina autentiseringsdata själva, men många organisationer föredrar att synkronisera med befintliga data i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="84d78-105">Some organizations have their users enter their authentication data themselves, but many organizations prefer to synchronize with existing data in Active Directory.</span></span> <span data-ttu-id="84d78-106">Om du har korrekt formaterade data i din lokala katalog och konfigurerar [Azure AD Connect med standardinställningar](./connect/active-directory-aadconnect-get-started-express.md), att data görs tillgänglig för Azure AD och SSPR utan användaråtgärder krävs.</span><span class="sxs-lookup"><span data-stu-id="84d78-106">If you have properly formatted data in your on-premises directory, and configure [Azure AD Connect using express settings](./connect/active-directory-aadconnect-get-started-express.md), that data is made available to Azure AD and SSPR with no user interaction required.</span></span>

<span data-ttu-id="84d78-107">Alla telefonnummer måste vara i formatet + CountryCode PhoneNumber exempel: + 1 4255551234 ska fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="84d78-107">Any phone numbers must be in the format +CountryCode PhoneNumber Example: +1 4255551234 to work properly.</span></span>

> [!NOTE]
> <span data-ttu-id="84d78-108">Återställning av lösenord stöder inte telefonanknytningar.</span><span class="sxs-lookup"><span data-stu-id="84d78-108">Password reset does not support phone extensions.</span></span> <span data-ttu-id="84d78-109">Även i formatet + 1 4255551234 X 12345 tas tillägg bort innan anropet görs.</span><span class="sxs-lookup"><span data-stu-id="84d78-109">Even in the +1 4255551234X12345 format, extensions are removed before the call is placed.</span></span>

## <a name="fields-populated"></a><span data-ttu-id="84d78-110">Fält</span><span class="sxs-lookup"><span data-stu-id="84d78-110">Fields populated</span></span>

<span data-ttu-id="84d78-111">Om du använder standardinställningarna i Azure AD Connect görs följande avbildningar.</span><span class="sxs-lookup"><span data-stu-id="84d78-111">If you use the default settings in Azure AD Connect the following mappings are made.</span></span>

| <span data-ttu-id="84d78-112">Lokala AD</span><span class="sxs-lookup"><span data-stu-id="84d78-112">On-premises AD</span></span> | <span data-ttu-id="84d78-113">Azure AD</span><span class="sxs-lookup"><span data-stu-id="84d78-113">Azure AD</span></span> | <span data-ttu-id="84d78-114">Azure AD Authentication kontaktinformation</span><span class="sxs-lookup"><span data-stu-id="84d78-114">Azure AD Authentication contact info</span></span> |
| --- | --- | --- |
| <span data-ttu-id="84d78-115">telephoneNumber</span><span class="sxs-lookup"><span data-stu-id="84d78-115">telephoneNumber</span></span> | <span data-ttu-id="84d78-116">Arbetstelefon</span><span class="sxs-lookup"><span data-stu-id="84d78-116">Office phone</span></span> | <span data-ttu-id="84d78-117">Telefon</span><span class="sxs-lookup"><span data-stu-id="84d78-117">Alternate phone</span></span> |
| <span data-ttu-id="84d78-118">mobila</span><span class="sxs-lookup"><span data-stu-id="84d78-118">mobile</span></span> | <span data-ttu-id="84d78-119">Mobiltelefon</span><span class="sxs-lookup"><span data-stu-id="84d78-119">Mobile phone</span></span> | <span data-ttu-id="84d78-120">Telefon</span><span class="sxs-lookup"><span data-stu-id="84d78-120">Phone</span></span> |


## <a name="security-questions-and-answers"></a><span data-ttu-id="84d78-121">Säkerhetsfrågor och svar</span><span class="sxs-lookup"><span data-stu-id="84d78-121">Security questions and answers</span></span>

<span data-ttu-id="84d78-122">Säkerhetsfrågor och svar lagras på ett säkert sätt i Azure AD-klienten och är bara tillgänglig för användare via den [SSPR-registreringsportalen](https://aka.ms/ssprsetup).</span><span class="sxs-lookup"><span data-stu-id="84d78-122">Security questions and answers are stored securely in your Azure AD tenant and are only accessible to users via the [SSPR registration portal](https://aka.ms/ssprsetup).</span></span> <span data-ttu-id="84d78-123">Administratörer kan inte se eller ändra innehållet i en annan användare frågor och svar.</span><span class="sxs-lookup"><span data-stu-id="84d78-123">Administrators can't see or modify the contents of another users questions and answers.</span></span>

### <a name="what-happens-when-a-user-registers"></a><span data-ttu-id="84d78-124">Vad händer när en användare som registrerar</span><span class="sxs-lookup"><span data-stu-id="84d78-124">What happens when a user registers</span></span>

<span data-ttu-id="84d78-125">När en användare som registrerar anger registreringssidan följande fält:</span><span class="sxs-lookup"><span data-stu-id="84d78-125">When a user registers, the registration page sets the following fields:</span></span>

* <span data-ttu-id="84d78-126">Telefon för autentisering</span><span class="sxs-lookup"><span data-stu-id="84d78-126">Authentication Phone</span></span>
* <span data-ttu-id="84d78-127">E-post för autentisering</span><span class="sxs-lookup"><span data-stu-id="84d78-127">Authentication Email</span></span>
* <span data-ttu-id="84d78-128">Säkerhetsfrågor och svar</span><span class="sxs-lookup"><span data-stu-id="84d78-128">Security Questions and Answers</span></span>

<span data-ttu-id="84d78-129">Om du har angett ett värde för **mobiltelefon** eller **alternativa e-postadress**, omedelbart fjärranvändning av dessa värden kan återställa sina lösenord, även om de inte har registrerats för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="84d78-129">If you have provided a value for **Mobile Phone** or **Alternate Email**, users can immediately use those values to reset their passwords, even if they haven't registered for the service.</span></span> <span data-ttu-id="84d78-130">Dessutom kan användare se dessa värden när du registrerar dig för första gången och ändra dem om de vill.</span><span class="sxs-lookup"><span data-stu-id="84d78-130">In addition, users see those values when registering for the first time, and modify them if they wish.</span></span> <span data-ttu-id="84d78-131">När de har registrerat värdena kommer att sparas i den **telefon för autentisering** och **autentisering e-post** respektive fält.</span><span class="sxs-lookup"><span data-stu-id="84d78-131">After they successfully register, these values will be persisted in the **Authentication Phone** and **Authentication Email** fields, respectively.</span></span>

## <a name="set-and-read-authentication-data-using-powershell"></a><span data-ttu-id="84d78-132">Ange och läsa autentiseringsdata med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="84d78-132">Set and read authentication data using PowerShell</span></span>

<span data-ttu-id="84d78-133">Följande fält kan anges med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="84d78-133">The following fields can be set using PowerShell</span></span>

* <span data-ttu-id="84d78-134">Alternativ e-postadress</span><span class="sxs-lookup"><span data-stu-id="84d78-134">Alternate Email</span></span>
* <span data-ttu-id="84d78-135">Mobiltelefon</span><span class="sxs-lookup"><span data-stu-id="84d78-135">Mobile Phone</span></span>
* <span data-ttu-id="84d78-136">Arbetstelefon - kan bara anges om inte synkroniseras med en lokal katalog</span><span class="sxs-lookup"><span data-stu-id="84d78-136">Office Phone - Can only be set if not synchronizing with an on-premises directory</span></span>

### <a name="using-powershell-v1"></a><span data-ttu-id="84d78-137">Med hjälp av PowerShell V1</span><span class="sxs-lookup"><span data-stu-id="84d78-137">Using PowerShell V1</span></span>

<span data-ttu-id="84d78-138">Om du vill komma igång behöver du [ladda ned och installera Azure AD PowerShell-modulen](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).</span><span class="sxs-lookup"><span data-stu-id="84d78-138">To get started, you need to [download and install the Azure AD PowerShell module](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).</span></span> <span data-ttu-id="84d78-139">När du har installerat kan följa du de steg som följer för att konfigurera varje fält.</span><span class="sxs-lookup"><span data-stu-id="84d78-139">Once you have it installed, you can follow the steps that follow to configure each field.</span></span>

#### <a name="set-authentication-data-with-powershell-v1"></a><span data-ttu-id="84d78-140">Ange autentiseringsdata med PowerShell V1</span><span class="sxs-lookup"><span data-stu-id="84d78-140">Set Authentication Data with PowerShell V1</span></span>

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-authentication-data-with-powershellpowershell-v1"></a><span data-ttu-id="84d78-141">Läs autentiseringsdata i PowerShellPowerShell V1</span><span class="sxs-lookup"><span data-stu-id="84d78-141">Read Authentication Data with PowerShellPowerShell V1</span></span>

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-the-commands-that-follow"></a><span data-ttu-id="84d78-142">Telefon för autentisering och autentisering e-post kan endast läsas med hjälp av Powershell V1 med de kommandon som följer</span><span class="sxs-lookup"><span data-stu-id="84d78-142">Authentication Phone and Authentication Email can only be read using Powershell V1 using the commands that follow</span></span>

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a><span data-ttu-id="84d78-143">Med hjälp av PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="84d78-143">Using PowerShell V2</span></span>

<span data-ttu-id="84d78-144">Om du vill komma igång behöver du [ladda ned och installera Azure AD-V2 PowerShell-modulen](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md).</span><span class="sxs-lookup"><span data-stu-id="84d78-144">To get started, you need to [download and install the Azure AD V2 PowerShell module](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md).</span></span> <span data-ttu-id="84d78-145">När du har installerat kan följa du de steg som följer för att konfigurera varje fält.</span><span class="sxs-lookup"><span data-stu-id="84d78-145">Once you have it installed, you can follow the steps that follow to configure each field.</span></span>

<span data-ttu-id="84d78-146">Om du vill installera snabbt från senare versioner av PowerShell som stöder installera modulen, kör du kommandona (den första raden bara kontrollerar om den är redan installerad):</span><span class="sxs-lookup"><span data-stu-id="84d78-146">To install quickly from recent versions of PowerShell that support Install-Module, run these commands (the first line simply checks to see if it's installed already):</span></span>

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-authentication-data-with-powershell-v2"></a><span data-ttu-id="84d78-147">Ange autentiseringsdata med PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="84d78-147">Set Authentication Data with PowerShell V2</span></span>

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

### <a name="read-authentication-data-with-powershell-v2"></a><span data-ttu-id="84d78-148">Läs autentiseringsdata med PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="84d78-148">Read Authentication Data with PowerShell V2</span></span>

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a><span data-ttu-id="84d78-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="84d78-149">Next steps</span></span>

<span data-ttu-id="84d78-150">Följande länkar ger ytterligare information om lösenordsåterställning med Azure AD</span><span class="sxs-lookup"><span data-stu-id="84d78-150">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="84d78-151">[**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD</span><span class="sxs-lookup"><span data-stu-id="84d78-151">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="84d78-152">[**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering</span><span class="sxs-lookup"><span data-stu-id="84d78-152">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="84d78-153">[**Distribution**](active-directory-passwords-best-practices.md) – Planera och distribuera SSPR till dina användare med hjälp av informationen finns här</span><span class="sxs-lookup"><span data-stu-id="84d78-153">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="84d78-154">[**Anpassa**](active-directory-passwords-customize.md) – Anpassa utseendet för företagets SSPR-funktion.</span><span class="sxs-lookup"><span data-stu-id="84d78-154">[**Customize**](active-directory-passwords-customize.md) - Customize the look and feel of the SSPR experience for your company.</span></span>
* <span data-ttu-id="84d78-155">[**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="84d78-155">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="84d78-156">[**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner</span><span class="sxs-lookup"><span data-stu-id="84d78-156">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="84d78-157">[**Teknisk djupdykning** ](active-directory-passwords-how-it-works.md) – Ta en titt bakom kulisserna för att förstå hur det hela fungerar</span><span class="sxs-lookup"><span data-stu-id="84d78-157">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="84d78-158">[**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man?</span><span class="sxs-lookup"><span data-stu-id="84d78-158">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="84d78-159">Varför?</span><span class="sxs-lookup"><span data-stu-id="84d78-159">Why?</span></span> <span data-ttu-id="84d78-160">Vad?</span><span class="sxs-lookup"><span data-stu-id="84d78-160">What?</span></span> <span data-ttu-id="84d78-161">Var?</span><span class="sxs-lookup"><span data-stu-id="84d78-161">Where?</span></span> <span data-ttu-id="84d78-162">Vem?</span><span class="sxs-lookup"><span data-stu-id="84d78-162">Who?</span></span> <span data-ttu-id="84d78-163">När?</span><span class="sxs-lookup"><span data-stu-id="84d78-163">When?</span></span> <span data-ttu-id="84d78-164">– Svar på allt du någonsin velat fråga</span><span class="sxs-lookup"><span data-stu-id="84d78-164">- Answers to questions you always wanted to ask</span></span>
* <span data-ttu-id="84d78-165">[**Felsökning** ](active-directory-passwords-troubleshoot.md) – Lär dig att lösa vanliga problem med SSPR</span><span class="sxs-lookup"><span data-stu-id="84d78-165">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>
