---
title: "aaaAzure tjänsten Security Center och Azure SQL Database | Microsoft Docs"
description: "Den här artikeln visar hur Security Center kan hjälpa dig att skydda dina databaser i Azure SQL Database."
services: sql-database
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f109adfd-daed-4257-9692-2042a1399480
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 173590500f0ce64140f214ada24b9692e01dbd4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure Security Center och Azure SQL Database-tjänsten
[Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) hjälper dig att förebygga, upptäcka och åtgärda toothreats. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

Den här artikeln visar hur Security Center kan hjälpa dig att skydda dina databaser i Azure SQL Database.

## <a name="why-use-security-center"></a>Varför ska man använda Security Center?
Security Center hjälper dig att skydda data i SQL-databas genom att tillhandahålla insyn i hello säkerheten för servrar och databaser. Med Security Center kan du:

* Definiera principer för kryptering av SQL-databas och granskning.
* Övervaka hello säkerheten för SQL Database-resurser i alla dina prenumerationer.
* Snabbt identifiera och åtgärda säkerhetsproblem.
* Integrera aviseringar från [Azure SQL Database hotidentifiering](../sql-database/sql-database-threat-detection.md).

Dessutom toohelping skydda din SQL Database-resurser, Security Center innehåller också säkerhetsövervakning och hantering av virtuella datorer i Azure Cloud Services, Apptjänster, virtuella nätverk och mer. Lär dig mer om Security Center [här](security-center-intro.md).

## <a name="prerequisites"></a>Krav
tooget igång med Security Center, måste du ha en prenumeration tooMicrosoft Azure. hello kostnadsfria nivån av Security Center aktiveras med din prenumeration. Läs mer om Security Center lediga och Standard nivåer [Security Center priser](https://azure.microsoft.com/pricing/details/security-center/).

Security Center stöder rollbaserad åtkomst. toolearn mer information om rollbaserad åtkomstkontroll (RBAC) i Azure, se [Azure Active Directory-rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md). hello Security Center FAQ innehåller information om [hur behörigheter ska hanteras i Security Center](security-center-faq.md#permissions).

## <a name="access-security-center"></a>Öppna Security Center
Security Center öppnas från hello [Azure-portalen](https://azure.microsoft.com/features/azure-portal/). [Logga in toohello portal](https://portal.azure.com/) och välj hello **Security Center alternativet**.

![Security Center-alternativet][1]

Hej **Security Center** blad öppnas.
![Security Center-bladet][2]

## <a name="set-security-policy"></a>Ange säkerhetsprincip
En säkerhetsprincip definierar hello kontroller som rekommenderas för resurser inom hello angivna prenumerationen eller resursen grupp. I Security Center kan du definiera principer för dina prenumerationer eller resursgrupper enligt tooyour företagets säkerhetskrav och hello typ av program eller känslighet hello data i varje prenumeration.

Du kan ange en princip tooshow rekommendationer för SQL-granskning och SQL transparent datakryptering (TDE).

* När du aktiverar **SQL-granskning och Hotidentifiering**, Security Center rekommenderar att granskning av åtkomst tooAzure databasen aktiveras för regelefterlevnad, avancerad identifiering och undersökningsskäl.
* När du aktiverar **SQL transparent datakryptering**, Security Center rekommenderar att kryptering i vila aktiveras för din Azure SQL-databas, tillhörande säkerhetskopior och transaktionsloggfiler.

tooset en säkerhetsprincip väljer hello **princip** panelen på hello Security Center-bladet. På hello **säkerhetsprincip** bladet, Välj hello prenumeration som du vill använda tooenable hello säkerhetsprincip. Välj **skyddsprincip** och aktivera **på** hello säkerhetsrekommendationer som du vill toouse för den här prenumerationen.
![Säkerhetsprincip][3]

Det finns fler toolearn [ställa in säkerhetsprinciper](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Hantera säkerhetsrekommendation
Security Center analyserar regelbundet hello säkerhetstillståndet hos dina Azure-resurser. När Security Center identifierar potentiella säkerhetsproblem skapas rekommendationer. hello rekommendationer leder dig igenom hello konfigureringen av hello behövs kontroller.

När du ställer in en säkerhetsprincip för analyserar Security Center hello säkerhetsstatus för dina resurser tooidentify potentiella säkerhetsrisker. hello rekommendationer visas i tabellformat där varje rad motsvarar en viss rekommendation. Använd hello i den följande tabellen som en referens toohelp du förstår hello tillgängliga rekommendationer för Azure SQL Database och vad varje rekommendation inte om du använder den. Markera en rekommendation tar dig tooan artikel som förklarar hur tooimplement hello rekommendation i Security Center.

| Rekommendation | Beskrivning |
| --- | --- |
| [Aktivera granskning och Hotidentifiering identifiering på SQL-servrar](security-center-enable-auditing-on-sql-servers.md) |Rekommenderar att du aktiverar granskning och hotidentifiering identifiering för SQL Database-servrar. (Endast SQL Database-tjänsten. Inkluderar inte Microsoft SQL Server körs på virtuella datorer.) |
| [Aktivera granskning och Hotidentifiering identifiering på SQL-databaser](security-center-enable-auditing-on-sql-databases.md) |Rekommenderar att du aktiverar granskning och hotidentifiering identifiering för databaser i SQL-databas. (Endast SQL Database-tjänsten. Inkluderar inte Microsoft SQL Server körs på virtuella datorer.) |
| [Aktivera transparent datakryptering](security-center-enable-transparent-data-encryption.md) |Rekommenderar att du aktiverar kryptering för SQL-databaser. (Endast SQL Database-tjänsten.) |

toosee rekommendationer för din Azure-resurser, Välj hello **rekommendationer** panelen på hello Security Center-bladet. På hello **rekommendationer** bladet Välj en rekommendation toosee information. I det här exemplet ska vi väljer **aktivera Auditing & Threat detection på SQL-servrar**.

![Rekommendationer][4]

Som visas nedan, Security Center visar hello av SQL-servrar där granskning och hotidentifiering inte har aktiverats. När du aktiverar granskning kan du konfigurera inställningar för Hotidentifiering och e-inställningar tooreceive säkerhetsaviseringar. Hotidentifiering varnar dig när den identifierar avvikande databasaktiviteter som indikerar potentiella hot toohello databasen. hello aviseringar visas i instrumentpanelen för hello Security Center.
![Granskning och hotidentifiering][5]

Gör så hello i [hotidentifiering för SQL-databas i hello Azure-portalen](../sql-database/sql-database-threat-detection-portal.md) tooturn på och konfigurera hotidentifiering och tooconfigure hello lista över e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande aktiviteter.

toolearn mer om rekommendationer finns [hantera säkerhetsrekommendationer](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Övervaka säkerhetshälsa
När du har aktiverat [säkerhetsprinciper](security-center-policies.md) för en prenumeration resurser, Security Center analyserar hello säkerheten för dina resurser tooidentify potentiella säkerhetsproblem.  Du kan visa hello säkerhetsstatus för dina resurser i hello **resurssäkerhetshälsa** panelen. När du klickar på **Data** i hello **resurssäkerhetshälsa** panelen, hello **dataresurser** blad öppnas med SQL rekommendationer för problem, till exempel granskning och transparent datakryptering inte har aktiverats. Här finns även rekommendationer för hello allmänna hälsotillstånd hello-databasen.
![Resurssäkerhetshälsa][6]

Det finns fler toolearn [övervakning av säkerhetshälsa](security-center-monitoring.md).

## <a name="manage-and-respond-toosecurity-alerts"></a>Hanterar och åtgärdar toosecurity aviseringar
Security Center automatiskt samlar in, analyseras och integreras loggdata från [Azure SQL-Hotidentifiering](../sql-database/sql-database-threat-detection.md), som även andra Azure-resurser, toodetect verkliga hot och falsklarm. En lista med prioritetssorterade säkerhetsaviseringar visas i Security Center tillsammans med hello information du behöver tooquickly undersöka hello problem och rekommendationer för hur tooremediate angreppet.

toosee aviseringar, väljer hello **säkerhetsaviseringar** panelen på hello Security Center-bladet. På hello **säkerhetsaviseringar** bladet Välj en avisering toolearn mer om hello händelser som utlöses hello avisering och vad, om sådant finns, hur du behöver tootake tooremediate angreppet. I det här exemplet ska vi väljer **potentiella SQL injection**.
![Säkerhetsaviseringar][7]

Enligt nedan Security Center innehåller ytterligare information som ger inblick i vilka utlösta hello-avisering hello mål resurs, om tillämpligt hello käll-IP-adress och rekommendationer om hur tooremediate.
![Potentiella SQL injection][8]

Det finns fler toolearn [hantering och svarar toosecurity aviseringar](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Nästa steg
* [Vanliga frågor om Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.
* [Security Center planerings- och operations guide](security-center-planning-and-operations-guide.md) – följer en uppsättning steg och aktiviteter toooptimize din användning av Security Center utifrån din organisations säkerhetskrav och molnhanteringsmodell.
* [Security Center datasäkerhet](security-center-data-security.md) – Lär dig hur Security Center samlar in och bearbetar data om din Azure-resurser, inklusive konfigurationsinformation, metadata, händelseloggar, kraschdumpfiler och mycket mer.
* [Hantering av säkerhetsincidenter](security-center-incident.md) -Lär dig hur toouse hello säkerhet Varna kapaciteten i Security Center tooassist du för att hantera säkerhetsincidenter.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
