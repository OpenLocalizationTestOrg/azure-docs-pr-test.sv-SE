---
title: "aaaConnect domänanslutna enheter tooAzure AD för Windows 10-upplevelser | Microsoft Docs"
description: "Förklarar hur administratörer kan konfigurera en grupprincip tooenable enheter toobe domänanslutna toohello företagets nätverk."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 2ff29f3e-5325-4f43-9baa-6ae8d6bad3e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 9766aa702352dea2ecad3a9a0bdf8d3286ee6d91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-domain-joined-devices-tooazure-ad-for-windows-10-experiences"></a>Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser
Anslut till domän är hello traditionella sätt organisationer har anslutna enheter för arbete hello senaste 15 år och mycket mer. Den har aktiverats användare toosign i tootheir enheter med hjälp av Windows Server Active Directory (Active Directory) arbetet eller skolkonton och tillåtna IT toofully hantera dessa enheter. Organisationer bygger vanligtvis på imaging metoder tooprovision enheter toousers och vanligtvis använder System Center Configuration Manager (SCCM) eller en grupprincip toomanage dem.


Anslut till domän i Windows 10 ger dig hello följande fördelar när du ansluter enheter tooAzure Active Directory (AD Azure):

* Enkel inloggning (SSO) tooAzure AD resurser från valfri plats
* Komma åt toohello enterprise Windows Store med hjälp av arbetet eller skolan konton (ingen Microsoft-konto krävs)
* Enterprise-kompatibel roaming av användarinställningar över enheter med hjälp av arbetet eller skolan konton (ingen Microsoft-konto krävs)
* Stark autentisering och smidig inloggning för arbetet eller skolan konton med Windows Hello för företag och Windows Hello
* Möjlighet toorestrict komma åt toodevices som uppfyller organisationens Enhetsinställningar för Grupprincip

## <a name="prerequisites"></a>Krav
Domänanslutning fortsätter toobe användbart. Dock tooget hello Azure AD fördelarna med enkel inloggning, centrala inställningar med arbetsplats eller skolkonton, och komma åt tooWindows Store med arbete eller skola konton, måste hello följande:

* Azure AD-prenumeration
* Azure AD Connect tooextend hello lokalt directory tooAzure AD
* Princip som har angetts tooconnect domänanslutna enheter tooAzure AD
* Windows 10-version (build 10551 eller senare) för enheter

tooenable Windows Hello för företag och Windows Hello, måste du också hello följande:

- **Infrastruktur för offentliga nycklar (PKI)** för utfärdande av certifikat för användaren.

- **System Center Configuration Manager Current Branch** -du behöver tooinstall version 1606 eller bättre.  
Mer information finns i: 
    - [Dokumentation för System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx)
    - [System Center Configuration Manager Teamblogg](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [Windows Hello för företag-inställningar i System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

Som ett alternativ toohello PKI-distributionskrav, kan du göra hello följande:

* Har några domänkontrollanter med Windows Server 2016 Active Directory Domain Services.

tooenable villkorlig åtkomst, kan du skapa grupprincipinställningar som tillåter åtkomst toodomain-anslutna enheter med några ytterligare distributioner. toomanage åtkomstkontroll baserat på hello enhets efterlevnad, behöver du hello följande:

* System Center Configuration Manager Current Branch (1606 eller senare) för Windows Hello för företag-scenarier

## <a name="deployment-instructions"></a>Anvisningar för distribution

toodeploy, följ hello stegen i [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="next-step"></a>Nästa steg
* [Windows 10 för hello företaget: sätt toouse enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

