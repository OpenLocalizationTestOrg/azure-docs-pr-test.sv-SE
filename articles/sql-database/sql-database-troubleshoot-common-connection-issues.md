---
title: "gemensamma aaaTroubleshoot-anslutningen utfärdar tooAzure SQL-databas"
description: "Steg tooidentify och Lös vanliga anslutningsfel för Azure SQL Database."
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: ac463d1c-aec8-443d-b66e-fa5eadcccfa8
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: eb5f2d7b123a76942c7e4a84a7f475344fbcb144
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-connection-issues-tooazure-sql-database"></a>Felsöka anslutning problem tooAzure SQL-databas
När hello anslutning tooAzure SQL-databas misslyckas visas [felmeddelanden](sql-database-develop-error-messages.md). Den här artikeln är en centraliserad artikel som hjälper dig att felsöka anslutningsproblem för Azure SQL Database. Det inför [hello vanliga orsaker](#cause) av anslutningsproblem, rekommenderar [ett verktyg för felsökning](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) som hjälper dig att identitet hello problem och ger felsökning steg toosolve [ tillfälliga fel](#troubleshoot-transient-errors) och [beständiga eller inte är tillfällig fel](#troubleshoot-persistent-errors). 

Om du får problem med anslutningen hello försök hello felsökning som beskrivs i den här artikeln.
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Orsak
Anslutningsproblem kan orsakas av något av följande hello:

* Fel tooapply bästa praxis och designriktlinjer under hello designprocessen för programmet.  Se [Utvecklingsöversikt för SQL-databasen](sql-database-develop-overview.md) tooget igång.
* Azure SQL Database omkonfiguration
* Brandväggsinställningar
* Timeout-värde
* Felaktig inloggningsinformation
* Högsta gränsen på vissa Azure SQL Database-resurser

I allmänhet anslutning problem tooAzure SQL-databas kan klassificeras enligt följande:

* [Tillfälligt fel (tillfällig eller återkommande)](#troubleshoot-transient-errors)
* [Beständiga eller inte är tillfällig fel (fel regelbundet återkommande)](#troubleshoot-persistent-errors)

## <a name="try-hello-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Försök hello felsökaren för Azure SQL Database-anslutningsproblem
Om du stöter på en specifik anslutningsfel, försök [verktyget](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), som kommer att du snabbt identitet och lösa problemet.

## <a name="troubleshoot-transient-errors"></a>Felsök tillfälliga fel

När ett program ansluter tooan Azure SQL-databas, får du hello följande felmeddelande:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry hello connection later. If hello problem persists, contact customer support, and provide them hello session tracing ID of <z>"
```

> [!NOTE]
> Det här felmeddelandet är vanligen övergående (tillfällig).
> 
> 

Felet uppstår när hello Azure-databas håller på att flytta (eller omkonfigureras) och ditt program förlorar sin anslutning toohello SQL-databas. SQL-databas omkonfiguration händelser inträffar på grund av en planerad åtgärd (till exempel programuppdatering) eller en oväntad händelse (till exempel en process krasch eller belastningsutjämning). De flesta omkonfiguration händelser är vanligtvis tillfällig och högst ska slutföras på mindre än 60 sekunder. Dessa händelser kan ibland ta längre toofinish, till exempel när en stor transaktion medför en tidskrävande återställning.

### <a name="steps-tooresolve-transient-connectivity-issues"></a>Steg tooresolve tillfälligt anslutningsproblem

1. Kontrollera hello [Microsoft Azure Service instrumentpanelen](https://azure.microsoft.com/status) för alla kända avbrott som uppstod under hello tid under vilken hello fel rapporterades av programmet hello.
2. Program som ansluter tooa molntjänst som Azure SQL Database ska förvänta sig periodiska omkonfiguration händelser och implementera försök logik toohandle felen i stället för att visa dessa som programmet fel toousers. Granska hello [tillfälliga fel](sql-database-connectivity-issues.md) avsnittet hello bästa praxis och utforma riktlinjer på [Utvecklingsöversikt för SQL-databasen](sql-database-develop-overview.md) försök strategier för mer information och allmänna. Kontrollera kodexempel på [anslutningsbibliotek för SQL Database och SQL Server](sql-database-libraries.md) för specifik information.
3. När en databas närmar sig gränsen resurs, kan det verka toobe ett tillfälligt anslutningsproblem. Se [felsökning av prestandaproblem med](sql-database-troubleshoot-performance.md).
4. Om problem med nätverksanslutningen Fortsätt, eller om hello varaktighet som hello fel uppstår i ditt program är längre än 60 sekunder eller om du ser flera förekomster av hello fel i en viss dag, filen en Azure-supportbegäran genom att välja **få stöd för** på hello [Azure-supporten](https://azure.microsoft.com/support/options) plats.

## <a name="troubleshoot-persistent-errors"></a>Felsöka beständiga
Om programmet hello överträder tooconnect tooAzure SQL-databas, visar vanligtvis ett problem med en av följande hello:

* Brandväggskonfigurationen. hello Azure SQL database eller klientsidan brandväggen blockerar anslutningar tooAzure SQL-databas.
* Nätverk omkonfiguration på klientsidan för hello: till exempel en ny IP-adress eller en proxyserver.
* Fel: till exempel skrev du fel anslutningsparametrar, till exempel hello servernamnet i anslutningssträngen för hello.

### <a name="steps-tooresolve-persistent-connectivity-issues"></a>Steg tooresolve beständiga anslutningsproblem
1. Ställ in [regler i brandväggen](sql-database-configure-firewall-settings.md) tooallow hello klientens IP-adress. Konfigurera en brandväggsregel som använder 0.0.0.0 som hello starta IP-adressintervall och använda 255.255.255.255 som hello avslutande IP-adressintervall för tillfälliga testning. Då öppnas hello tooall IP-adresser. Om det löser problemet med anslutningen, ta bort den här regeln och skapa en brandväggsregel för en korrekt begränsade IP-adressen eller adressintervallet. 
2. Kontrollera att port 1433 är öppen för utgående anslutningar på alla brandväggar mellan hello-klienten och hello Internet. Granska [konfigurera hello Windows-brandväggen tooAllow SQL Server-åtkomst](https://msdn.microsoft.com/library/cc646023.aspx) och [Hybrid Identity krävs portar och protokoll](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports) för ytterligare pekare med tooadditional portar som du behöver tooopen för Azure Active Directory-autentisering.
3. Kontrollera anslutningssträngen och andra inställningar. Se hello anslutningssträngen avsnitt i hello [anslutning problem avsnittet](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4. Kontrollera tjänstens hälsa i hello instrumentpanelen. Om du tror att det finns ett regionalt strömavbrott, se [återställa från avbrott](sql-database-disaster-recovery.md) för steg toorecover tooa nya regionen.

## <a name="next-steps"></a>Nästa steg
* [Felsökning av problem med Azure SQL Database prestanda](sql-database-troubleshoot-performance.md)
* [Sök hello dokumentationen på Microsoft Azure](http://azure.microsoft.com/search/documentation/)
* [Visa hello uppdateringar senaste toohello Azure SQL Database-tjänsten](http://azure.microsoft.com/updates/?service=sql-database)

## <a name="additional-resources"></a>Ytterligare resurser
* [Översikt över SQL Database-utveckling](sql-database-develop-overview.md)
* [Allmänna riktlinjer för tillfälliga fel-hantering](../best-practices-retry-general.md)
* [Anslutningsbibliotek för SQL Database och SQL Server](sql-database-libraries.md)

