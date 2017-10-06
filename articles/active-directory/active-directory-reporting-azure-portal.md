---
title: aaaAzure Active Directory reporting | Microsoft Docs
description: "En allmän översikt över Azure Active Directory-rapportering."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c91813acbdc4b0bfcd164169b0b575accac227d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting"></a>Azure Active Directory-rapportering

Med Azure Active Directory-rapportering kan du få insikter om din miljös prestanda.  
hello som data kan du:

- Avgör hur din app och dina tjänster används av dina användare
- Identifiera potentiella risker som påverkar hello hälsotillståndet för din miljö
- Felsöka problem som hindrar användare från att uträtta sitt arbete  

hello reporting arkitektur förlitar sig på två huvudsakliga pelare:

- Säkerhetsrapporter
- Aktivitetsrapporter

![Rapportering](./media/active-directory-reporting-azure-portal/01.png)



## <a name="security-reports"></a>Säkerhetsrapporter

hello säkerhetsrapporter i Azure Active Directory kan du tooprotect din organisations identiteter.  
Det finns två typer av säkerhetsrapporter i Azure Active Directory:

- **Användare som har flaggats för risk** - från hello [användare som har flaggats för risk säkerhetsrapporten](active-directory-reporting-security-user-at-risk.md), du få en översikt över användarkonton som kan ha drabbats.

- **Riskfyllda inloggningar** – med hello [riskfyllda inloggning säkerhetsrapporten](active-directory-reporting-security-risky-sign-ins.md), du får en indikator för inloggningsförsök som kan ha utförts av någon som är inte hello legitima ägaren till ett användarkonto. 

**Vilka Azure AD-licens behöver du tooaccess en säkerhetsrapporten?**  
Alla utgåvor av Azure Active Directory ger rapporter över användare som har flaggats för risk och riskfyllda inloggningar.  
Hello nivå av rapportens granularitet varierar dock mellan hello versioner: 

- I hello **Azure Active Directory ledigt och grundläggande**, du redan ha en lista över användare som har flaggats för risk och riskfyllda inloggningar. 

- Hej **Azure Active Directory Premium 1** edition utökar den här modellen genom att du även tooexamine vissa hello underliggande riskhändelser som har identifierats för varje rapport. 

- Hej **Azure Active Directory Premium 2** versionen ger dig med hello mest detaljerad information om hello underliggande riskhändelser och du kan också tooconfigure säkerhetsprinciper som automatiskt svarar tooconfigured risknivåer.


## <a name="activity-reports"></a>Aktivitetsrapporter

Det finns två typer av aktivitetsrapporter i Azure Active Directory:

- **Granskningsloggar** - hello [granskningsloggar aktivitetsrapport](active-directory-reporting-activity-audit-logs.md) ger dig åtkomst toohello historik för varje aktivitet i din klient.

- **Inloggningar** – med hello [inloggningar aktivitetsrapport](active-directory-reporting-activity-sign-ins.md), du kan bestämma vem som har genomfört hello uppgifter som rapporterats av hello kontrollrapport loggar.



Hej **granskningsloggar rapporten** ger dig poster av systemaktiviteter för kompatibilitet.
Bland annat tillhandahålls hello data kan du tooaddress vanliga scenarier som:

- Någon i min klient fick åtkomst tooan administratörsgrupp. Vem som gav användaren åtkomst? 

- Jag vill tooknow hello lista över användare som loggar in på en viss app sedan jag nyligen publicerats så hello app och vill tooknow om det fungerar bra

- Jag vill tooknow hur många lösenord återställer sker i min klient


**Vilka Azure AD-licens behöver du tooaccess hello granska loggarna rapporten?**  
hello granska loggarna rapporten är tillgängligt för funktioner som du har licenser. Om du har en licens för en specifik funktion kan ha du också åtkomst toohello granska informationen i felloggen för den.

Mer information finns i **jämföra allmänt tillgängliga funktioner i hello lediga, Basic eller Premium Edition** i [Azure Active Directory-funktioner som](https://www.microsoft.com/cloud-platform/azure-active-directory-features).   



Hej **inloggningar aktivitetsrapport** aktiverar tootoofind svar tooquestions som:

- Vad är hello inloggning mönstret för en användare?
- Hur många användare har en användare loggat in under en vecka?
- Vad är hello statusen för dessa inloggningar?


**Vilka Azure AD-licens gör du behöver tooaccess hello inloggningar Aktivitetsrapport?**  
tooaccess Hej inloggningar aktivitetsrapport, din klient måste ha en Azure AD Premium-licens som är kopplade till den.


## <a name="programmatic-access"></a>Programmässig åtkomst

I användargränssnittet för tillägg toohello Azure Active Directory reporting ger dig också med [Programmeringsåtkomst](active-directory-reporting-api-getting-started-azure-portal.md) toohello rapportdata. hello data för de här rapporterna kan vara användbar tooyour program, till exempel SIEM-system, granskning och business intelligence-verktyg. hello Azure AD reporting API: er ger Programmeringsåtkomst toohello data via en uppsättning REST-baserad API: er. Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg. 


## <a name="next-steps"></a>Nästa steg

Om du vill tooknow mer om hello olika rapporttyper i Azure Active Directory, se:

- [Användare som har flaggats för risk](active-directory-reporting-security-user-at-risk.md)
- [Rapport över riskfyllda inloggningar](active-directory-reporting-security-risky-sign-ins.md)
- [Granskningsloggar](active-directory-reporting-activity-audit-logs.md)
- [Inloggningsrapport](active-directory-reporting-activity-sign-ins.md)

Om du vill tooknow mer om att komma åt hello hello reporting data med hjälp av hello reporting API, se: 

- [Komma igång med hello Azure Active Directory reporting API](active-directory-reporting-api-getting-started-azure-portal.md)


<!--Image references-->
[1]: ./media/active-directory-reporting-azure-portal/ic195031.png