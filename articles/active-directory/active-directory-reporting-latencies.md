---
title: aaaAzure Active Directory Reporting svarstiderna | Microsoft Docs
description: "Tid det tar för rapportering händelser tooshow in i Azure Active Directory"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 346b14f8-d16d-4b07-8211-e6c5eec07062
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 14367d21dfb28359f991037cc924d416420be456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-report-latencies"></a>Rapportsvarstider i Azure Active Directory
*Den här dokumentationen är en del av hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*

| Rapport | Minimum | Genomsnittlig | Maximalt |
| --- | --- | --- | --- |
| **Säkerhetsrapporter** | | | |
| Oregelbunden inloggningsaktivitet |2 timmar |4 timmar |8 timmar |
| Inloggningar från okända källor |2 timmar |4 timmar |8 timmar |
| Inloggningar efter flera fel |2 timmar |4 timmar |8 timmar |
| Inloggningar från flera geografiska områden |2 timmar |4 timmar |8 timmar |
| Inloggningar från IP-adresser med misstänkt aktivitet |2 timmar |4 timmar |8 timmar |
| Inloggningar från potentiellt infekterade enheter |2 timmar |4 timmar |8 timmar |
| Användare med avvikande inloggningsaktivitet |2 timmar |4 timmar |8 timmar |
| Används med läckta autentiseringsuppgifter |2 timmar |4 timmar |8 timmar |
| Alla användare inloggningsaktivitet |2 timmar |4 timmar |8 timmar |
| **Rapporter över program** | | | |
| Kontot etablering aktivitet |2 timmar |4 timmar |8 timmar |
| Kontoetableringsfel |2 timmar |4 timmar |8 timmar |
| Programanvändning |2 timmar |4 timmar |8 timmar |
| Status för förnyelse av lösenord |2 timmar |4 timmar |8 timmar |
| **Granska & aktivitetsrapporter** | | | |
| Granska rapporten |1 minut |15 minuter |30 minuter |
| Återställningsaktivitet för lösenord (AD Azure) |2 timmar |4 timmar |8 timmar |
| Återställningsaktivitet för lösenord (Identity Manager) |2 timmar |4 timmar |8 timmar |
| Lösenordsåterställningsaktivitet registrering (AD Azure) |2 timmar |4 timmar |8 timmar |
| Lösenordsåterställningsaktivitet registrering (Identity Manager) |2 timmar |4 timmar |8 timmar |
| Självbetjäning grupper aktivitet (AD Azure) |2 timmar |4 timmar |8 timmar |
| Självbetjäning grupper aktivitet (Identity Manager) |2 timmar |4 timmar |8 timmar |
| **RMS-rapporter** | | | |
| Mest aktiva RMS-användare |2 timmar |4 timmar |8 timmar |
| RMS-användning |2 timmar |4 timmar |8 timmar |
| Användning av RMS-enhet |2 timmar |4 timmar |8 timmar |
| Användning av RMS-aktiverade program |2 timmar |4 timmar |8 timmar |

