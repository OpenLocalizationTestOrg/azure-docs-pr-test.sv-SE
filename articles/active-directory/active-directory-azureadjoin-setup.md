---
title: "aaaSetting in Azure AD Join för dina användare | Microsoft Docs"
description: "Förklarar hur administratörer kan konfigurera Azure AD Join för din lokala katalog och enhetsregistrering."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: bfc5d415-c918-4d8b-afee-b3f41cc28469
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 60a5aeb11292cb6057ab1065c3ab77e5981d0cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-azure-ad-join-in-your-organization"></a>Konfigurera Azure AD Join i din organisation
Innan du konfigurerar Azure Active Directory Join (Azure AD Join) måste tooeither synkronisering upp din lokala katalog för användare toohello molnet eller manuellt skapa hanterade konton i Azure AD.

Detaljerade instruktioner för att synkronisera dina lokala användare tooAzure AD beskrivs i [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

toomanually skapa och hantera användare i Azure AD, referera för[användarhantering i Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Konfigurera enhetsregistrering
1. Logga in toohello Azure portal som administratör.
2. Hello vänster, Välj **Active Directory**.
3. På hello **Directory** , Välj din katalog.
4. Välj hello **konfigurera** fliken.
5. Gå toohello **enheter** avsnitt.
6. På hello **enheter** ställer du in hello följande:  
   * **HÖGSTA antal enheter PER användare**: Välj hello högsta antalet enheter som en användare kan ha i Azure AD.  Om användarna når den här kvoten, kommer de inte att kunna tooadd ytterligare enheter förrän en eller flera av deras befintliga enheter tagits bort.
   * **KRÄV Multi-FACTOR Authentication tooJOIN enheter**: Ange om användare som är nödvändiga tooprovide en andra autentiseringsmetod factor toojoin deras enhet tooAzure AD. Mer information om Azure Multi-Factor Authentication finns [komma igång med Azure Multi-Factor Authentication i molnet hello](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).
   * **ANVÄNDARE kan ansluta enheter för AZURE AD**: Välj hello användare och grupper som tillåts toojoin enheter tooAzure AD.
   * **YTTERLIGARE ADMINISTRATÖRER för AZURE AD-anslutna enheter**: med Azure AD Premium eller hello Enterprise Mobility Suite (EMS) kan du välja vilka användare som beviljas lokal administratörsbehörighet toohello enhet. Globala administratörer och enhetsägare beviljas lokal administratörsbehörighet som standard.

<center>![Konfigurera enhetsregistrering](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center>

När du konfigurerar Azure AD Join för dina användare, kan de ansluta tooAzure AD via deras företagets eller personliga enheter.

Följande är hello tre scenarier som du kan använda tooenable användare-tooset upp Azure AD Join:

* En användare att ansluta en företagsägd enhet direkt tooAzure AD.
* Användarna domänansluter en företagsägd enhet toohello lokala Active Directory och utökar hello enheten tooAzure AD.
* Användarna lägger till arbetet eller skolan konton tooWindows på en personlig enhet

## <a name="additional-information"></a>Ytterligare information
* [Windows 10 för hello företaget: sätt toouse enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

