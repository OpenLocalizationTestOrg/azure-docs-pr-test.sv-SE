---
title: "aaaAzure MFA-versioner och förbrukning av planer | Microsoft Docs"
description: "Information om hello Multi-Factor Authentication-klienten och hello olika metoder och versioner som är tillgängliga. Information om varje förbrukning plan"
keywords: 
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: kgremban
ms.openlocfilehash: 4914747e435531b9f950356d23aa386f3d9585d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-azure-multi-factor-authentication"></a>Hur tooget Azure Multi-Factor Authentication

När det gäller tooprotecting dina konton, vara tvåstegsverifiering standard i din organisation. Den här funktionen är särskilt viktigt för administratörskonton som privilegierad åtkomst tooresources. Microsoft erbjuder grundläggande tvåstegsverifiering verifiering funktioner tooOffice 365 och Azure administratörer därför. Om du vill tooupgrade hello funktioner för dina administratörer eller utöka tvåstegsverifiering verifiering toohello resten av dina användare, kan du köpa Azure Multi-Factor Authentication. 

Den här artikeln beskriver beskriver hello skillnaden mellan hello-versioner som erbjuds tooadministrators och hello Azure MFA fullversion och anger vilka funktioner som är tillgängliga i varje. Om du är redo toodeploy hello slutföra Azure MFA erbjudande, hello senare avsnitt omfattar implementeringsalternativ och hur Microsoft beräknar förbrukning.

>[!IMPORTANT]
>Den här artikeln är avsedd toobe en guide toohelp du förstår hello olika sätt toobuy Azure Multi-Factor Authentication. Specifik information om priser och fakturering bör du alltid se toohello [Multifaktorautentisering sida med priser](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Tillgängliga versioner av Azure Multi-Factor Authentication

hello beskrivs följande tabell hello skillnaderna mellan tre versioner av Multi-Factor authentication:

| Version | Beskrivning |
| --- | --- |
| Multi-Factor Authentication för Office 365 |Den här versionen fungerar endast med Office 365-program och hanteras från hello Office 365-portalen. Administratörer kan [skydda Office 365-resurser med tvåstegsverifiering](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). Den här versionen är en del av en prenumeration på Office 365. |
| Multi-Factor Authentication för Azure-administratörer | Globala administratörer av Azure innehavare kan aktivera tvåstegsverifiering för sina globala administratörskonton utan extra kostnad.|
| Azure Multi-Factor Authentication | Ofta hänvisas tooas hello ”full” versionen, flerfunktionsautentisering Azure erbjuder hello bra uppsättning funktioner. Det ger ytterligare konfigurationsalternativ via hello [klassiska Azure-portalen](https://manage.windowsazure.com)avancerade rapportering och stöd för ett antal lokala program och molnprogram. Azure Multi-Factor Authentication ingår i Azure Active Directory Premium (P1 och P2 planer) och Enterprise Mobility + Security (E3 och E5 planer) och kan vara distribueras antingen [i hello molnet eller lokalt](multi-factor-authentication-get-started.md). |

## <a name="feature-comparison-of-versions"></a>Funktionsjämförelse mellan versioner
hello följande tabell innehåller en lista över hello-funktioner som är tillgängliga i hello olika versioner av Azure Multi-Factor Authentication.

> [!NOTE]
> Jämförelsetabellen beskrivs hello-funktioner som ingår i varje version av Multi-Factor Authentication. Om du har hello fullständig Azure Multi-Factor Authentication-tjänsten kan vissa funktioner kanske inte är tillgängliga beroende på om du använder [MFA i molnet hello eller MFA lokalt](multi-factor-authentication-get-started.md).


| Funktion | Multi-Factor Authentication för Office 365 | Multi-Factor Authentication för Azure-administratörer | Azure Multi-Factor Authentication |
| --- |:---:|:---:|:---:|
| Skydda administratörskonton med MFA |● |● (endast för konton i en Global administratör) |● |
| Mobilappar som ett andra alternativ |● |● |● |
| Telefonsamtal som ett andra alternativ |● |● |● |
| SMS som ett andra alternativ |● |● |● |
| Applösenord för klienter som inte har stöd för MFA |● |● |● |
| -Administratörer kontroll över verifieringsmetoderna |● |● |● |
| PIN-läge | | |● |
| Bedrägerivarning | | |● |
| MFA-rapporter | | |● |
| Engångsförbikoppling | | |● |
| Anpassade hälsningar för telefonsamtal | | |● |
| Anpassade Uppringarens ID för telefonsamtal | | |● |
| Tillförlitliga IP-adresser | | |● |
| MFA sparas för betrodda enheter |● |● |● |
| MFA SDK | | |● (kräver Multi-Factor Authentication provider och fullständig Azure-prenumeration) |
| MFA för lokala program | | |● |

## <a name="how-tooget-azure-multi-factor-authentication"></a>Hur tooget Azure Multi-Factor Authentication
Om du vill ha fullständig hello funktionerna som erbjuds av Azure Multi-Factor Authentication, finns det flera alternativ:

### <a name="option-1---mfa-licenses"></a>Alternativ 1 - MFA-licenser

Köpa licenser för Azure Multi-Factor Authentication och tilldela dem tooyour användare i Azure Active Directory. 

Om du använder det här alternativet kan skapa du Azure Multi-Factor Authentication-leverantör om du behöver också tooprovide tvåstegsverifiering för vissa användare som inte har licenser. Annars kan kanske du att debiteras två gånger.

### <a name="option-2---bundled-licenses-that-include-mfa"></a>Alternativ 2 - paketerade licenser som innehåller MFA

Köpa licenser som innehåller Azure Multi-Factor Authentication som Azure Active Directory Premium (P1 eller P2) eller Enterprise Mobility + Security (E3 eller E5) och tilldela dem tooyour användare i Azure Active Directory. 

Om du använder det här alternativet kan skapa du Azure Multi-Factor Authentication-leverantör om du behöver också tooprovide tvåstegsverifiering för vissa användare som inte har licenser. Annars kan kanske du att debiteras två gånger. 

### <a name="option-3---mfa-consumption-based-model"></a>Alternativ 3 - MFA förbrukningsbaserad modellen

Skapa ett Azure Multi-Factor Authentication-leverantör i en Azure-prenumeration. Azure MFA-Providers är Azure-resurser som debiteras mot din Enterprise-avtal, Azure i förskott eller kreditkort som alla andra Azure-resurser. Dessa providers kan bara skapas i kompletta Azure-prenumerationer inte begränsat Azure-prenumerationer som har ett $-0 utgiftsgräns. Begränsad prenumerationer skapas när du aktiverar licenser, som i alternativ 1 och 2. 

När du använder Azure Multi-Factor Authentication-leverantör, finns det två användning modeller som är tillgängliga som debiteras via din Azure-prenumeration:  

1. **Per användare** – för företag som vill tooenable tvåstegsverifiering för ett fast antal anställda som regelbundet behöver autentisering. Fakturering per användare är baserad på hello antalet användare som har aktiverats för Multifaktorautentisering i Azure AD-klienten och/eller Azure MFA-Server. Om användarna har aktiverats för MFA i både Azure AD och Azure MFA-Server och domän synkronisering (Azure AD Connect) är aktiverat och sedan vi räkna hello större uppsättning användare. Om domänen synkronisering har inte aktiverats, så vi räkna hello summan av alla användare som har aktiverats för Multifaktorautentisering i Azure AD och Azure MFA-Server. Fakturering är proportionellt fördelad och rapporterade toohello Commerce dagligen. 

  > [!NOTE]
  > Fakturering exempel 1: du har 5 000 användare som har aktiverats för MFA idag. Hej MFA system dividerar numret med 31 och rapporter 161.29 användare för den dagen. I morgon aktiverar du 15 fler användare, så att hello MFA system rapporter 161.77 användare för den dagen. Hello slutet av hello fakturering cykel, adderar hello totala antalet användare som faktureras mot din Azure-prenumeration tooaround 5 000. 
  >
  > Fakturering exempel 2: du har en blandning av användare med licenser och användare utan, så att du har en användarspecifik Azure MFA-leverantören toomake in hello skillnaden. Det finns 4,500 Enterprise Mobility + Security licenser på din klient, men 5 000 användare som har aktiverats för Multifaktorautentisering. Din Azure-prenumeration debiteras för 500 användare, linjärt och rapporteras dagligen som 16.13 användare. 

2. **Per autentisering** – för företag som vill tooenable tvåstegsverifiering för en stor grupp med användare som behöver autentisering mer sällan. Fakturering baseras på hello antal tvåstegsverifiering verifiering begäranden tas emot av hello Azure MFA-Molntjänsten, oavsett om dessa kontroller lyckas eller nekas. Den här fakturering visas på din användning av Azure-instruktionen i packs av 10 autentiseringar och har rapporterat toohello Commerce system varje dag. 

  > [!NOTE]
  > Fakturering exempel 3: idag hello Azure MFA-tjänsten tog emot 3,105 tvåstegsverifiering verifiering begäranden. Din Azure-prenumeration faktureras för 310.5 autentisering packs. 

Det är viktigt toonote att du kan ha Azure MFA licenser, men fortfarande få debiteras för förbrukning-baserad konfiguration. Om du ställer in en per autentisering Azure MFA-leverantören debiteras du för varje begäran om identitetsverifiering tvåstegsverifiering, även de som utförs av användare som har licenser. Om du ställer in en användarspecifik Azure MFA-Provider på en domän som inte är länkade tooyour Azure AD-klient debiteras du per aktiverad användare även om användarna har licenser på Azure AD. 

## <a name="next-steps"></a>Nästa steg

- Mer information om priser finns i [priser för Azure MFA](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

- Välj om toodeploy Azure MFA [i hello molnet eller lokalt](multi-factor-authentication-get-started.md)
