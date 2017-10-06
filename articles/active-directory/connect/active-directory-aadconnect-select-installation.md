---
title: "Azure AD Connect: Välj installationstypen | Microsoft Docs"
description: "Det här avsnittet vägleder dig igenom hur tooselect hello installationen skriver toouse för Azure AD Connect"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: a700e59eb05947ee1dbd9993141200c9340b2f1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="select-which-installation-type-toouse-for-azure-ad-connect"></a>Välj vilka installationen typen toouse för Azure AD Connect
Azure AD Connect har två typer av appinstallationer för nyinstallation: Express och anpassas. Det här avsnittet hjälper dig att toodecide som alternativet toouse under installationen.

## <a name="express"></a>Express
Snabb är de vanligaste hello-alternativ och används av ungefär 90% av alla nya installationer. Det var utformad tooprovide en konfiguration som passar för hello vanligaste scenarierna för kunden.

Vi utgår från:

- Du har en enda Active Directory-skogen lokalt.
- Du har ett enterprise-administratörskonto som du kan använda för hello installation.
- Du har mindre än 100 000 objekt i din lokala Active Directory.

Du får:

- [Lösenordssynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md) från lokala tooAzure AD för enkel inloggning.
- En konfiguration som synkroniserar [användare, grupper, kontakter och Windows 10-datorer](active-directory-aadconnectsync-understanding-default-configuration.md).
- Synkroniseringen av alla tillgängliga objekt i alla domäner och alla organisationsenheter.
- [Automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md) är aktiverade toomake att du alltid använder hello senaste tillgängliga versionen.

Alternativ där du kan fortfarande använda Express:

- Om du inte vill toosynchronize alla organisationsenheter, du kan fortfarande använda snabb och på hello sista sidan, avmarkera **starta synkroniseringsprocessen hello...** *. Kör hello installationsguiden igen och ändra hello organisationsenheter i [konfigurationsalternativ](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) och aktivera schemalagd synkronisering.
- Vill du tooenable en av hello funktionerna i Azure AD Premium, till exempel tillbakaskrivning av lösenord. Först gå igenom express tooget hello inledande installationen har slutförts. Kör hello installationsguiden igen och ändra hello [konfigurationsalternativ](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).

## <a name="custom"></a>Anpassat
hello anpassade sökvägen tillåter många fler alternativ än express. Det ska användas i samtliga fall där hello konfigurationen som beskrivs i föregående avsnitt för snabba inte är representativa för din organisation.

Använd när:

- Du har inte åtkomst tooan företagsadministratörskonto i Active Directory.
- Du har mer än en skog eller om du planerar toosynchronize mer än en skog i hello framtida.
- Du har domäner i skogen kan inte nås från hello Connect-servern.
- Du planerar toouse federation eller direktautentisering för användarinloggning.
- Du har mer än 100 000 objekt och behöver toouse en fullständig SQL Server.
- Du planerar toouse gruppbaserade filtrering och inte bara domän eller OU-baserade filtrering.

## <a name="upgrade-from-dirsync"></a>Uppgradera från DirSync
Om du använder DirSync, följer du hello steg i [uppgradera från DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) tooupgrade den befintliga konfigurationen. Det finns två olika uppgraderingsalternativ:

- Uppgradering på plats-tooinstall ansluta på hello samma server.
- Parallell distribution tooinstall Connect på en ny server medan hello befintlig DirSync-server är fortfarande fungerar.

## <a name="upgrade-from-azure-ad-sync"></a>Uppgradera från Azure AD Sync
Om du använder Azure AD Sync, så du kan följa hello [likadant](active-directory-aadconnect-upgrade-previous-version.md) som när du uppgraderar från en Anslut version tooa senare. Det finns två olika uppgraderingsalternativ:

- Uppgradering på plats-tooinstall ansluta på hello samma server.
- Öppning migrering tooinstall Connect på en ny server när hello befintliga Azure AD Sync-server är fortfarande fungerar.

## <a name="migrate-from-fim2010-or-mim2016"></a>Migrera från FIM2010 eller MIM2016
Om du använder Forefront Identity Manager 2010 eller Microsoft Identity Manager 2016 med hello Azure AD-koppling, är det enda alternativet en migrering. Följ hello stegen som beskrivs i [öppning migrering](active-directory-aadconnect-upgrade-previous-version.md#swing-migration). Ersätt nämns Azure AD Sync med FIM2010/MIM2016 i hello steg.

## <a name="next-steps"></a>Nästa steg
Beroende på hello alternativ du har valt toouse använder hello tabell av innehåll toohello vänstra toofind din artikel med hello detaljerade steg.
