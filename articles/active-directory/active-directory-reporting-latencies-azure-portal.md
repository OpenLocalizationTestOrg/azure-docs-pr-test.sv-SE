---
title: aaaAzure Active Directory reporting svarstiderna | Microsoft Docs
description: "Lär dig mer om hello lång tid det tar för rapportering händelser tooshow in i din Azure-portalen"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi;dhanyahk
ms.reviewer: dhanyahk
ms.openlocfilehash: eee959331262ba59b313dd038cb54699dbef48a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-latencies"></a>Azure Active Directory reporting svarstider

Med [reporting](active-directory-preview-explainer.md) i hello Azure Active Directory, du får alla hello information som du behöver toodetermine hur din miljö gör. hello mängden tid det tar för att rapportera data tooshow upp i hello Azure-portalen kallas även svarstid. 

Det här avsnittet listar hello latensinformation för hello alla reporting kategorier i hello Azure-portalen. 


## <a name="activity-reports"></a>Aktivitetsrapporter

Det finns två områden av aktiviteten reporting:

- **Logga in aktiviteter** – Information om hello användning av hanterade program och användaren loggar in aktiviteter
- **Granskningsloggar** – Granska information om systemaktivitet för användare och grupphantering, dina hanterade program och katalogaktiviteter

hello i den följande tabellen listas hello latensinformation för aktivitetsrapporter.

| Rapport | Minimum | Genomsnittlig | Maximalt |
| :-- | --- | --- | --- |
| Granskningsloggar             | 30 minuter  | 45 minuter | 1 timme     |
| Inloggningar               | 15 minuter  | 15 minuter | 2 timmar *   |

>[!NOTE]
> För vissa inloggningar aktivitetsdata från äldre office-program, kan det ta too8 timmar för hello rapporterar data tooshow. 


## <a name="security-reports"></a>Säkerhetsrapporter

Det finns två områden av säkerhet reporting:

- **Riskfyllda inloggningar** -riskfyllda loggar in är en indikator för en inloggning försök som kan ha utförts av någon som inte är hello legitima ägare för ett användarkonto. 
- **Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats. 

hello i den följande tabellen listas hello latensinformation för säkerhetsrapporter.

| Rapport | Minimum | Genomsnittlig | Maximalt |
| :-- | --- | --- | --- |
| Användare i farozonen          | 5 minuter   | 15 minuter  | 2 timmar  |
| Riskfyllda inloggningar         | 5 minuter   | 15 minuter  | 2 timmar  |

## <a name="risk-events"></a>Riskhändelser

Azure Active Directory använder anpassningsbar maskininlärning algoritmer och heuristik toodetect misstänkta åtgärder som är relaterade tooyour användarkonton. Varje upptäckt misstänkt åtgärd lagras i en post kallas risk händelse.

hello i den följande tabellen listas hello latensinformation för riskhändelser.

| Rapport | Minimum | Genomsnittlig | Maximalt |
| :-- | --- | --- | --- |
| Inloggningar från anonyma IP-adresser |5 minuter |15 minuter |2 timmar |
| Inloggningar från okända platser |5 minuter |15 minuter |2 timmar |
| Används med läckta autentiseringsuppgifter |2 timmar |4 timmar |8 timmar |
| Omöjlig resa tooatypical platser |5 minuter |1 timme |8 timmar  |
| Inloggningar från angripna enheter |2 timmar |4 timmar |8 timmar  |
| Inloggningar från IP-adresser med misstänkt aktivitet |2 timmar |4 timmar |8 timmar  |



## <a name="next-steps"></a>Nästa steg

Om du vill tooknow mer om hello aktivitetsrapporter i hello Azure-portalen, se:

- [Inloggningsaktivitet rapporter i hello Azure Active Directory-portalen](active-directory-reporting-activity-sign-ins.md)
- [Granska aktivitetsrapporter hello Azure Active Directory-portalen](active-directory-reporting-activity-audit-logs.md)

Om du vill tooknow mer om hello säkerhetsrapporter i hello Azure-portalen, se:

- [Användare på risk säkerhetsrapporten hello Azure Active Directory-portalen](active-directory-reporting-security-user-at-risk.md)
- [Riskfyllda inloggningar rapporten i hello Azure Active Directory-portalen](active-directory-reporting-security-risky-sign-ins.md)

Om du vill tooknow mer om riskhändelser finns [Azure Active Directory riskhändelser](active-directory-reporting-risk-events.md).
