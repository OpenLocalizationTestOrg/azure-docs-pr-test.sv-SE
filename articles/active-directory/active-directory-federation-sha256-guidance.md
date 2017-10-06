---
title: "aaaChange signaturhash-algoritm för förlitande part för Office 365 | Microsoft Docs"
description: "Den här sidan innehåller riktlinjer för att ändra SHA-algoritmen för federationsförtroende med Office 365"
keywords: "SHA1, SHA256, O365, federation, aadconnect, adfs, ADFS, ändra sha federationsförtroende förtroende för förlitande part"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: samueld
editor: 
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
ms.openlocfilehash: 3333d1384aff8bdf6b3bcc894f8c633fd9ccc3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a>Ändra signaturhash-algoritm för Office 365 förtroende för förlitande part
## <a name="overview"></a>Översikt
Active Directory Federation Services (AD FS) loggar dess token tooMicrosoft Azure Active Directory-tooensure som de inte kan brytas. Denna signatur kan baseras på SHA1 eller SHA256. Azure Active Directory har nu stöd för token som signerats med ett SHA256-algoritmen och rekommenderar vi hello tokensignering algoritmen tooSHA256 för hello högsta säkerhetsnivå. Den här artikeln beskriver hello steg behövs tooset hello tokensignering algoritmen toohello säkrare SHA256-nivå.

>[!NOTE]
>Microsoft rekommenderar användning av SHA256 som hello-algoritm för att signera token som det är säkrare än SHA1 men SHA1 fortfarande är ett alternativ som stöds.

## <a name="change-hello-token-signing-algorithm"></a>Ändra certifikat för tokensignering hello-algoritmen
När du har angett hello signaturalgoritm med någon av hello två processer nedan, loggar hello token för Office 365 förtroende för förlitande part med SHA256 i AD FS. Du behöver inte toomake några extra konfigurationsändringar och den här ändringen har ingen inverkan på din möjlighet tooaccess Office 365 eller andra Azure AD-program.

### <a name="ad-fs-management-console"></a>AD FS-hanteringskonsolen
1. Öppna hello AD FS-hanteringskonsolen på hello primär AD FS-servern.
2. Expandera hello AD FS-noden och klicka på **förtroende för förlitande part**.
3. Högerklicka på ditt Office 365-/ Azure förlitande part och välj **egenskaper**.
4. Välj hello **Avancerat** fliken och väljer hello säkra hash-algoritm SHA256.
5. Klicka på **OK**.

![SHA256 Signeringsalgoritm--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS PowerShell-cmdlets
1. Öppna PowerShell med administratörsbehörighet på alla AD FS-servern.
2. Ange hello säkra hash-algoritm med hjälp av hello **Set-AdfsRelyingPartyTrust** cmdlet.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Läs också
* [Reparera Office 365 förtroende med Azure AD Connect](connect/active-directory-aadconnect-federation-management.md#repairthetrust)

