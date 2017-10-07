---
title: "aaaGroup principen och MDM-inställningar | Microsoft Docs"
description: "Innehåller information om grupprinciper och mobila enheter (MDM) inställningar som ska användas på företagsägda enheter. Dessa principer är tillämpade toohello hela enheten."
services: active-directory
keywords: "Vad är grupp princip och MDM-inställningar för Enterprise tillstånd Roaming, Enterprise tillstånd Roaming, windows molnet"
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 762419b47014b1fb4d92ac528785e20078afe689
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="group-policy-and-mdm-settings"></a>Inställningar för Grupprincip och MDM
Dessa Grupprincip och hanteringsinställningar för mobila enheter (MDM) endast användas på företagsägda enheter eftersom dessa principer används toohello hela enheten. Använda en MDM-princip toodisable synkronisera inställningar för en personlig ska enhet som ägs av användare påverka hello användning av enheten. Dessutom påverkas även andra konton på hello enhet av hello princip.

Företag som vill toomanage roaming för personliga (ohanterade) enheter kan använda hello Azure portal tooenable eller inaktivera centrala i stället för att använda en grupprincip eller MDM.
hello följande tabeller beskrivs hello principinställningar som är tillgängliga.

## <a name="mdm-settings"></a>MDM-inställningar
hello MDM principinställningarna gäller tooboth Windows 10 och Windows 10 Mobile.  Det finns stöd för Windows 10 Mobile endast för Microsoft-konto baserat centrala via användarens OneDrive-konto.  Se för avsnittet ”enheter och slutpunkter” mer information om vilka enheter som stöds för Azure AD baseras synkroniseras.

| Namn | Beskrivning |
| --- | --- |
| Tillåt Microsoft-konto-anslutning |Tillåter användare tooauthenticate med ett Microsoft-konto på hello-enhet |
| Tillåt synkronisering av Mina inställningar |Gör inställningar för Windows-användare tooroam och appdata. Om du inaktiverar den här principen inaktiverar synkronisering samt säkerhetskopieringar på mobila enheter |

## <a name="group-policy-settings"></a>Grupprincipinställningar
hello grupprincipinställningar tillämpas tooWindows 10-enheter som är kopplade tooan Active Directory-domän. hello tabellen innehåller också äldre inställningar som visas toomanage synkroniseringsinställningar, men som inte arbetar för Enterprise tillstånd Roaming för Windows 10, som är märkta med ”Använd inte” i hello beskrivning.

| Namn | Beskrivning |
| --- | --- |
| Konton: Blockera Microsoft-konton |Den här inställningen förhindrar att användare lägger till nya Microsoft-konton på den här datorn |
| Är inte synkroniserade |Förhindrar att användare tooroam Windows-inställningar och AppData |
| Synkroniserar inte anpassa |Inaktiverar synkroniseringen av hello teman grupp |
| Synkronisera inte inställningar för webbläsaren |Inaktiverar synkroniseringen av hello Internet Explorer-grupp |
| Synkronisera inte lösenord |Inaktiverar synkronisering av lösenord grupp |
| Synkronisera inte andra Windows-inställningar |Inaktiverar synkroniseringen av gruppen för andra Windows-inställningar |
| Synkronisera inte skrivbordsanpassning |Använd inte; har ingen effekt |
| Synkronisera inte i med datapriser |Inaktiverar roaming på förbrukade anslutningar, till exempel mobil 3 G |
| Är inte synkroniserade appar |Använd inte; har ingen effekt |
| Synkronisera inte app-inställningar |Inaktiverar roaming för AppData |
| Synkronisera inte inställningarna för start |Använd inte; har ingen effekt |

## <a name="related-topics"></a>Relaterade ämnen
* [Företagsroaming](active-directory-windows-enterprise-state-roaming-overview.md)
* [Aktivera företagsroaming i Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
* [Inställningar och dataroaming vanliga frågor och svar](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Centrala referens för Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Felsökning](active-directory-windows-enterprise-state-roaming-troubleshooting.md)

