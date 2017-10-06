---
title: "aaaAuthenticating identiteter utan lösenord via Windows Hello för företag och Azure AD | Microsoft Docs"
description: "Innehåller en översikt över Windows Hello för företag och ytterligare information om hur du distribuerar Windows Hello för företag."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: f907bb90-8776-46ca-9e12-279949af66ff
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 7c1c52e10b7ab7a89ec3226ffa7cf01896267871
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticating-identities-without-passwords-through-windows-hello-for-business"></a>Autentisera identiteter utan lösenord via Windows Hello för företag
hello aktuella metoder för autentisering med bara lösenord är inte tillräckligt tookeep användare säker. Användare återanvända och glömt lösenord. Lösenord är breachable, phishable, felbenägna toocracks och fjärråtkomstlösning. De också få svårt tooremember och felbenägna tooattacks som ”[skicka hello hash](https://technet.microsoft.com/dn785092.aspx)”.

## <a name="about-windows-hello-for-business"></a>Om Windows Hello för företag
Windows Hello för företag är en privata och offentliga nyckel eller certifikatbaserad autentisering strategi för organisationer och konsumenter utöver lösenord. Den här typen av autentisering är beroende av nyckelpar autentiseringsuppgifter som kan ersätta lösenord och står emot toobreaches, stöld och nätfiske.

 Windows Hello för företag, kan användaren autentiseras tooa Microsoft-konto, ett Windows Server Active Directory-konto, ett konto med Microsoft Azure Active Directory (AD Azure) eller en icke-Microsoft-tjänst som stöder snabb identitet Online (FIDO) autentisering. Windows Hello för företag har ställts in på hello användarens enhet efter en inledande tvåstegsverifiering under Windows Hello för företag-registrering, och hello användaren anger en gest som kan vara Windows Hello eller en PIN-kod. hello användaren anger hello gest tooverify sin identitet. Windows använder Windows Hello för företag tooauthenticate hello användaren sedan och hjälpa dem att tooaccess skyddade resurser och tjänster.

hello privat nyckel görs tillgänglig enbart via en ”användargest” som en PIN-kod, biometrik, eller en fjärransluten enhet som ett smartkort som hello användaren använder toosign i toohello enhet. Den här informationen är länkade tooa certifikat eller ett asymmetriskt nyckelpar. hello privata nyckel är maskinvara som godkänd av om hello enheten har ett TPM-chip Trusted Platform Module (TPM). hello privat nyckel lämnar aldrig hello enhet.

hello offentliga nyckel är registrerad med Azure Active Directory och Windows Server Active Directory (för lokala). Identitetsleverantörer (IDPs) verifiera hello användare genom att mappa hello offentliga nyckeln för hello toohello privata användarnyckel och ange inloggningsinformation via en tid lösenord (OTP), PhoneFactor eller en annan notification mekanism.

## <a name="why-enterprises-should-adopt-windows-hello-for-business"></a>Varför företag bör anta Windows Hello för företag
Genom att aktivera Windows Hello för företag, kan företag göra sina resurser som ännu säkrare genom att:

* Konfigurera Windows Hello för företag med ett maskinvaru-önskade alternativ. Det innebär att nycklar genereras på TPM 1.2 eller TPM 2.0 när det är tillgängligt. När TPM inte är tillgänglig genererar programvara hello nyckel.
* Definiera hello komplexitet och längd hello fästa och om Hello användning är aktiverad i din organisation.
* Konfigurera Windows Hello för företag toosupport smartkort-liknande scenarier med hjälp av certifikatbaserad förtroende.

## <a name="how-windows-hello-for-business-works"></a>Hur Windows Hello för företag fungerar
1. Nycklar genereras på hello maskinvara med TPM- eller programvara. Många enheter har en inbyggd TPM-chip säkrar hello maskinvara genom att integrera krypteringsnycklar i enheter. TPM 1.2 eller TPM 2.0 genererar nycklar eller certifikat som skapas från hello genererade nycklar.
2. hello TPM intygar nycklarna maskinvara gränsen.
3. En enda unlock-gest låser hello enhet. Den här gest kan komma åt resurser i toomultiple om hello-enheten är ansluten till domänen eller Azure AD-anslutna.

## <a name="how-hello-windows-hello-for-business-lifecycle-works"></a>Så här fungerar hello Windows Hello för företag livscykel
![Windows Hello för företag livscykel](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

hello illustrerar föregående diagram hello privata och offentliga nyckelpar och hello validering av hello identitetsleverantör. Var och en av de här stegen beskrivs här:

1. hello användare bevisar deras identitet via flera inbyggda språkverktyg metoder (gester, fysiska smartkort, multifaktorautentisering) och skickar den här informationen tooan identitet Provider (IDP) som Azure Active Directory eller lokal Active Directory.
2. hello enheten sedan skapar hello nyckel, intygar hello nyckel, tar hello offentliga delen av den här nyckeln, bifogar med station instruktioner, loggar in och skickar det toohello IDP tooregister hello nyckel.
3. Så snart hello IDP registrerar hello offentliga delen av hello nyckel, hello hello IDP utmaningar enheten toosign med hello privata delen av hello nyckel.
4. hello IDP verifierar sedan och problem hello autentiseringstoken som gör att användaren hello och hello åtkomst hello skyddade resurser. IDPs kan skriva plattformsoberoende appar eller webbläsare stöd (via JavaScript/Webcrypto API: er) toocreate och använda Windows Hello för företag autentiseringsuppgifterna för sina användare.

## <a name="hello-deployment-requirements-for-windows-hello-for-business"></a>hello distributionskrav för Windows Hello för företag
### <a name="at-hello-enterprise-level"></a>På företagsnivå hello
* hello företaget har en Azure-prenumeration.

### <a name="at-hello-user-level"></a>På användarnivå hello
* hello användarens dator kör Windows 10 Professional och Enterprise.

Detaljerade anvisningar för distribution, se [aktivera Windows Hello för företag i hello organisation](active-directory-azureadjoin-passport-deployment.md).

## <a name="additional-information"></a>Ytterligare information
* [Windows 10 för hello företaget: sätt toouse enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

