---
title: "Ansluta domänanslutna enheter till Azure AD för Windows 10-upplevelser | Microsoft Docs"
description: "Förklarar hur administratörer kan konfigurera en grupprincip för att aktivera enheter måste vara domänansluten till företagets nätverk."
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
ms.openlocfilehash: 9c91579d20bb84701f6d0b97d944728c84044adf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10
Anslut till domän är det traditionella sätt organisationer har anslutit enheter för arbete för de senaste 15 åren och mycket mer. Det har aktiverat användare att logga in på sina enheter med hjälp av Windows Server Active Directory (Active Directory) arbetet eller skolan konton och tillåtna IT för att hantera dessa enheter. Organisationer bygger vanligtvis på avbildning metoder för att etablera enheter till användare och vanligtvis använda System Center Configuration Manager (SCCM) eller en grupprincip för att hantera dem.


Anslut till domän i Windows 10 ger följande fördelar när du ansluter enheter till Azure Active Directory (AD Azure):

* Enkel inloggning (SSO) till Azure AD-resurser från valfri plats
* Åtkomst till Windows Store för företag med hjälp av arbetet eller skolan konton (ingen Microsoft-konto krävs)
* Enterprise-kompatibel roaming av användarinställningar över enheter med hjälp av arbetet eller skolan konton (ingen Microsoft-konto krävs)
* Stark autentisering och smidig inloggning för arbetet eller skolan konton med Windows Hello för företag och Windows Hello
* Möjligheten att begränsa åtkomst till enheter som uppfyller organisationens Enhetsinställningar för Grupprincip

## <a name="prerequisites"></a>Krav
Domänanslutning fortsätter att vara användbara. Emellertid att hämta Azure AD-fördelarna med enkel inloggning, roaming av inställningar med arbetets eller skolans konton och åtkomst till Windows Store med arbets-eller skolkonton, behöver du följande:

* Azure AD-prenumeration
* Azure AD Connect att utöka lokala katalog till Azure AD
* Princip som har angetts till ansluta domänanslutna enheter till Azure AD
* Windows 10-version (build 10551 eller senare) för enheter

Om du vill aktivera Windows Hello för företag och Windows Hello, måste du också följande:

- **Infrastruktur för offentliga nycklar (PKI)** för utfärdande av certifikat för användaren.

- **System Center Configuration Manager Current Branch** -måste du installera versionen 1606 eller bättre.  
Mer information finns i: 
    - [Dokumentation för System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx)
    - [System Center Configuration Manager Teamblogg](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [Windows Hello för företag-inställningar i System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

Som ett alternativ till kravet på PKI-distribution kan du göra följande:

* Har några domänkontrollanter med Windows Server 2016 Active Directory Domain Services.

Om du vill aktivera villkorlig åtkomst, kan du skapa grupprincipinställningar som tillåter åtkomst till domänanslutna enheter med några ytterligare distributioner. Om du vill hantera åtkomstkontroll utifrån kompatibilitet, behöver du följande:

* System Center Configuration Manager Current Branch (1606 eller senare) för Windows Hello för företag-scenarier

## <a name="deployment-instructions"></a>Anvisningar för distribution

Om du vill distribuera, följer du stegen i [hur du konfigurerar automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="next-step"></a>Nästa steg
* [Windows 10 för företaget: Sätt att använda enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

