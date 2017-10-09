---
title: hantering av aaaIntroduction toodevice i Azure Active Directory | Microsoft Docs
description: "Lär dig hur enhetshantering kan du tooget kontroll över hello-enheter som har åtkomst till resurser i din miljö."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: e2fc0a3e8d00dc69cf01db9074e34427e396cfcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toodevice-management-in-azure-active-directory"></a>Introduktion toodevice management i Azure Active Directory

I mobile första, molnet först världen kan Azure Active Directory (Azure AD) toodevices för enkel inloggning, appar och tjänster från var som helst. Med hello ökande mängden av enheter – inklusive Bring Your Own Device (BYOD), inför IT-proffs två andra mål:

- Ge hello slutanvändare toobe produktiva var och när
- Skydda hello företagets tillgångar när som helst

Via enheter kan får användarna åtkomst tooyour företagets tillgångar. tooprotect din företagets tillgångar som IT-administratör vill du toohave kontroll över enheterna. Detta gör att du toomake till att dina användare har åtkomst till dina resurser från enheter som uppfyller dina krav för säkerhet och efterlevnad. 

Hantering av enheter är också hello grunden för [enhetsbaserad villkorlig åtkomst](active-directory-conditional-access-policy-connected-applications.md). Med enhetsbaserad villkorlig åtkomst kan du se till att åtkomst tooresources i din miljö är endast möjlig med betrodda enheter.   

Det här avsnittet beskrivs hur enhetshantering i Azure Active Directory fungerar.

## <a name="getting-devices-under-hello-control-of-azure-ad"></a>Hantera enheter under hello kontroll av Azure AD

tooget en enhet under hello kontroll av Azure AD, har du två alternativ:

- Registrera 
- Koppla

**Registrera** en enhet tooAzure AD kan du toomanage identitet för en enhet. När en enhet registreras, ger Azure AD-enhetsregistrering hello-enhet med en identitet som används tooauthenticate hello-enhet när en användare loggar in tooAzure AD. Du kan använda hello identitet tooenable eller inaktivera en enhet.

När den kombineras med en enhetsattributen lösning för mobila enheter, till exempel Microsoft Intune, uppdateras hello attributen i Azure AD med ytterligare information om hello enhet. Detta ger dig toocreate regler för villkorlig åtkomst som säkerställer att åtkomsten från enheter toomeet dina krav för säkerhet och efterlevnad. Mer information om registrerar enheter i Microsoft Intune finns i registrera enheter för hantering i Intune.

**Koppla** en enhet är ett tillägg tooregistering en enhet. Det innebär den ger dig alla hello fördelar med att registrera en enhet och tillägg toothis, ändras även hello lokala tillståndet för en enhet. Om du ändrar hello lokala tillstånd kan användare toosign i tooa enheten med ett organisations arbets- eller skolkonto i stället för ett personligt konto.

## <a name="azure-ad-registered-devices"></a>Azure AD som registrerade enheter   

hello målet med Azure AD som registrerade enheter är du med stöd för hello tooprovide **Bring Your Own Device (BYOD)** scenario. I det här scenariot kan en användare åtkomst till organisationens Azure Active Directory styrs resurser med hjälp av en personlig enhet.  

![Azure AD som registrerade enheter](./media/device-management-introduction/03.png)

hello åtkomst baserat på ett arbets- eller skolkonto konto som har angetts på hello enhet.  
Windows 10 kan till exempel användare tooadd ett arbets- eller Skol-konto tooa dator-, tablet- eller telefon.  
När en användare har lagt till en arbets eller skolkonto, hello enhet är registrerad med Azure AD och du kan också registreras i hello mobila enheter (MDM) hanteringssystem som din organisation har konfigurerats. Användare i organisationen kan lägga till ett arbetsplats eller skola konto tooa personlig enhet lätt:

- Vid åtkomst till ett arbetsplats-program för hello första gången
- Manuellt via hello **inställningar** -menyn i hello skiftläget för Windows 10 

Du kan konfigurera Azure AD som registrerade enheter för Windows 10, iOS, Android och macOS.

## <a name="azure-ad-joined-devices"></a>Azure AD-anslutna enheter

hello målet med Azure AD anslutna enheter är toosimplify:

- Windows-distributioner av enheter som ägs av arbetet 
- Åtkomst tooorganizational appar och resurser från valfri windowsenhet

![Azure AD som registrerade enheter](./media/device-management-introduction/02.png)


Detta sker genom att erbjuda användarna en upplevelse som Självbetjäning för att hämta enheter som ägs av arbetet under hello kontroll av Azure AD.  
**Azure AD-anslutning** är avsedd för organisationer som är moln första / endast molnbaserad. Detta är vanligtvis små och medelstora företag som inte har en lokal Windows Server Active Directory-infrastruktur. 

Implementera Azure AD anslutna enheter ger dig hello följande fördelar:

- **Enkel inloggning (SSO)** tooyour Azure hanteras SaaS-appar och tjänster. Användarna ser anvisningarna för ytterligare autentisering vid åtkomst till arbetsresurser. hello SSO funktioner är även när de inte är anslutna toohello domännätverk som är tillgängliga.

- **Enterprise kompatibla centrala** av användarinställningar för domänanslutna enheter. Användarna behöver inte tooconnect ett Microsoft-konto (till exempel Hotmail) toosee inställningar över enheter.

- **Åtkomst till tooWindows Store för företag** med hjälp av AD-kontot. Användarna kan välja från en inventering av program som är markerat av hello organisation.

- **Windows Hello** stöd för säker och smidig åtkomst toowork resurser.

- **Begränsning av åtkomst** tooapps från enheter som uppfyller policy för efterlevnad.

Medan Azure AD-anslutning är främst avsedd för organisationer som inte har en lokal Windows Server Active Directory-infrastruktur, kan fungera utmärkt också använda den i scenarier där:

- Du kan inte använda ett lokalt domänanslutning, till exempel om du behöver tooget mobila enheter som surfplattor och telefoner under kontroll.

- Användarna behöver främst tooaccess Office 365 eller andra SaaS-appar som är integrerade med Azure AD.

- Vill du toomanage en grupp användare i Azure AD i stället för i Active Directory. Detta kan till exempel gälla tooseasonal anställda, leverantörer eller studenter.

- Vill du tooprovide anslutande funktioner tooworkers i fjärranslutna avdelningskontor med begränsad lokal infrastruktur.

Du kan konfigurera Azure AD anslutna enheter för Windows 10-enheter.


## <a name="hybrid-azure-ad-joined-devices"></a>Hybrid Azure AD-anslutna enheter

För mer än en tio åren har har många organisationer använt hello domän koppling tootheir lokala Active Directory tooenable:

- IT-avdelningar toomanage arbete ägda enheter från en central plats.

- Användare toosign i tootheir enheter med deras Active Directory arbets- eller skolkonton. 

Normalt organisationer med ett lokalt storleken är beroende av imaging metoder tooprovision enheter och de använder ofta **System Center Configuration Manager (SCCM)** eller **Grupprincip (GP)** toomanage dem.

Om din miljö har en lokal AD storleken och du även vill utnyttja hello funktionerna i Azure Active Directory, du kan implementera hybrid Azure AD anslutna enheter. Dessa är enheter som är både, domänanslutna tooyour lokala Active Directory och Azure Active Directory.

![Azure AD som registrerade enheter](./media/device-management-introduction/01.png)


Du bör använda Azure AD hybrid anslutna enheter om:

- Win32-appar distribuerade toothese enheter som använder NTLM / Kerberos.

- Du behöver GP eller SCCM / DCM toomanage enheter.

- Du vill toocontinue toouse avbildning lösningar tooconfigure enheter för dina anställda.

Du kan konfigurera Hybrid Azure AD-anslutna enheter för Windows 10 och äldre enheter, till exempel Windows 8 och Windows 7.

## <a name="summary"></a>Sammanfattning

Med hantering av enheter i Azure AD kan du: 

- Förenkla hello processen för att göra enheter under hello kontroll av Azure AD

- Ge användarna en enkel toouse åtkomst tooyour organisations molnbaserade resurser

Som regel i en USB bör du använda:

- Azure AD som registrerade enheter för personliga enheter

- Azure AD-anslutna enheter för enheter som inte är anslutna tooan lokala AD 

- Hybrid Azure AD-anslutna enheter för enheter som är anslutna tooan lokala AD     




## <a name="next-steps"></a>Nästa steg

- tooget en översikt över hur hello toomanage enhet i Azure portal, se [hantera enheter med hjälp av hello Azure-portalen](device-management-azure-portal.md)

- toolearn mer om enhetsbaserad villkorlig åtkomst finns [konfigurera Azure Active Directory enhetsbaserad villkorliga åtkomstprinciper](active-directory-conditional-access-policy-connected-applications.md).

- toosetup hybrid Azure AD anslutna enheter, se [hur tooconfigure hybrid Azure Active Directory-anslutna enheter](device-management-hybrid-azuread-joined-devices-setup.md).


