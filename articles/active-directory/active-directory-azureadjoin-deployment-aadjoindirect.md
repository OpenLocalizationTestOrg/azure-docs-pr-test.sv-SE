---
title: "aaaUsage scenarier och överväganden vid distribution för Azure AD Join | Microsoft Docs"
description: "Förklarar hur administratörer kan konfigurera Azure AD Join för slutanvändarna (anställda, studenter, andra användare). Här beskrivs också hello olika verkliga scenarier för att använda Azure AD Join."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 7e57971481aa312ebf8a69999d194f9dcc3d4708
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Användningsscenarier och överväganden vid distribution för Azure AD Join
## <a name="usage-scenarios-for-azure-ad-join"></a>Användningsscenarier för Azure AD Join
### <a name="scenario-1-businesses-largely-in-hello-cloud"></a>Scenario 1: Företag i stort sett i hello moln
Azure Active Directory Join (Azure AD Join) du kan ha nytta om du för närvarande fungerar och hantera identiteter för ditt företag i molnet hello eller flyttar toohello moln snart. Du kan använda ett konto som du har skapat i Azure AD toosign i tooWindows 10. Via [hello först köra experience (FRX) processen](active-directory-azureadjoin-user-frx.md), eller genom att ansluta till Azure AD från [hello inställningsmenyn](active-directory-azureadjoin-user-upgrade.md), dina användare kan ansluta till sina datorer tooAzure AD.  Användarna kan också få enkel inloggning (SSO) åtkomst för molnresurser som Office 365 i webbläsaren eller i Office-program.

### <a name="scenario-2-educational-institutions"></a>Scenario 2: Utbildningssyfte
Utbildningssyfte har vanligtvis två användartyper: lärare och övrig personal och studenter. Fakultetsmedlemmar anses långsiktig medlemmar i hello organisation. Skapa lokala konton för dem. är önskvärt. Men studenter tillhör shorter-term hello organisation och sina konton kan hanteras i Azure AD. Det innebär att directory skala flyttas toohello moln i stället för att lagras lokalt. Det innebär också att studenter som kommer att kunna toosign i tooWindows med sina Azure AD-konton och få åtkomst till tooOffice 365 resurser i Office-program.

### <a name="scenario-3-retail-businesses"></a>Scenario 3: Retail företag
Retail företag har säsongsbaserade arbetare och långsiktig anställda. Du vanligtvis skapa lokala konton och använda domänanslutna datorer för långsiktig heltidsanställda. Men när anställda tillhör shorter-term hello organisation, och det är önskvärt toomanage sina konton där användarlicenser kan enkelt flytta runt. När du skapar sina konton i hello moln med Office 365 licenser kan få dessa användare hello fördelarna med att logga in tooWindows och Office-program med Azure AD-kontot, samtidigt som du behåller mer flexibelt med licenser när de lämnar.

### <a name="scenario-4-additional-scenarios"></a>Scenario 4: Fler scenarier
Tillsammans med hello fördelar som tidigare diskuterats, dra nytta av att användarna ansluta sina enheter tooAzure AD på grund av en förenklad anslutande upplevelse, effektiv enhetshantering, automatisk hantering av mobilenhetsregistrering och tooAzure för enkel inloggning AD och lokala resurser.  

## <a name="deployment-considerations-for-azure-ad-join"></a>Överväganden vid distribution för Azure AD Join
### <a name="enable-your-users-toojoin-a-company-owned-device-directly-tooazure-ad"></a>Aktivera din användare toojoin en företagsägd enhet direkt tooAzure AD
Företag kan tillhandahålla molnbaserad konton toopartner företag och organisationer. Dessa partners kan sedan enkelt komma åt företagets appar och resurser med enkel inloggning. Det här scenariot är tillämpliga toousers som har åtkomst till resurser i första hand i hello molnet, till exempel Office 365 eller SaaS-appar som förlitar sig på Azure AD för autentisering.

### <a name="prerequisites"></a>Krav
**På företagsnivå hello (administratör)**

* Azure-prenumeration med Azure Active Directory  

**På användarnivå hello**

* Windows 10 (Professional och Enterprise Edition)

### <a name="administrator-tasks"></a>Administratörsåtgärder
* [Konfigurera enhetsregistrering](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Användaruppgifter
* [Skapa en ny Windows 10-enhet med Azure AD under installationen](active-directory-azureadjoin-user-frx.md)
* [Konfigurera en Windows 10-enhet med Azure AD hello inställningsmenyn](active-directory-azureadjoin-user-upgrade.md)
* [Ansluta till en personlig tooyour organisation för Windows 10-enhet](active-directory-azureadjoin-personal-device.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a>Aktivera BYOD i din organisation för Windows 10
Du kan ställa in din användare och anställda toouse sina personliga Windows-enheter (BYOD) tooaccess företagets appar och resurser. Användarna kan lägga till sina Azure AD-konton (arbets- eller skolkonto konton) tooa personliga Windows tooaccess resurser på ett sätt som skyddade och kompatibla.

### <a name="prerequisites"></a>Krav
**På företagsnivå hello (administratör)**

* Azure AD-prenumeration

**På användarnivå hello**

* Windows 10 (Professional och Enterprise Edition)

### <a name="administrator-tasks"></a>Administratörsåtgärder
* [Konfigurera enhetsregistrering](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Användaruppgifter
* [Ansluta till en personlig tooyour organisation för Windows 10-enhet](active-directory-azureadjoin-personal-device.md)

## <a name="additional-information"></a>Ytterligare information
* [Windows 10 för hello företaget: sätt toouse enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Autentisera identiteter utan lösenord via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

