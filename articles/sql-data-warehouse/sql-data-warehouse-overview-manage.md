---
title: aaaManage databaser i Azure SQL Data Warehouse | Microsoft Docs
description: "Översikt över hantering av SQL Data Warehouse-databaser. Innehåller hanteringsverktyg, dwu: er och skalbar prestanda, felsökning frågeprestanda, etablera bra säkerhetsprinciper och återställa en databas från skadade data eller från ett regionalt strömavbrott."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 8576fdb3-71fe-4b3b-a4e0-5e8a7f148acf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: caec6572c4ab395107c3b095adc69a53eae8bd88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Hantera databaser i Azure SQL Data Warehouse
SQL Data Warehouse automatiserar många aspekter av att hantera dina databaser. Till exempel göra tooscale prestanda du behöver bara tooadjust betala för hello rätt nivå av beräkningsresurser och därefter låta SQL Data Warehouse alla hello arbetet med skala ut och skalning tillbaka.

Otvivelaktigt vill toomonitor din arbetsbelastning tooidentify prestandan måste samt felsöka tidskrävande frågor. Du måste också tooperform några uppgifter toomanage behörigheter för användare och inloggningar.

Den här översikten beskrivs dessa aspekter av att hantera SQL Data Warehouse.

* Hanteringsverktyg
* Skala bearbetning
* Pausa och återuppta
* Bästa praxis för prestanda
* Övervakning av frågan
* Säkerhet
* Säkerhetskopiering och återställning

## <a name="management-tools"></a>Hanteringsverktyg
Du kan använda flera olika verktyg toomanage databaser i SQL Data Warehouse. När du hanterar databaser du utvecklar inställningar för varje typ av uppgift som du behöver tooperform.

### <a name="azure-portal"></a>Azure Portal
Hej [Azure-portalen] [ Azure portal] är en webbaserad portal där du kan skapa, uppdatera, och ta bort databaser och övervaka databasresurser. Det här verktyget är bra om du precis har börjat med Azure, hantera ett litet antal datalagerdatabaserna eller behöver tooquickly göra något.

tooget igång med hello Azure-portalen finns [skapa ett SQL Data Warehouse (Azure portal)][Create a SQL Data Warehouse (Azure portal)].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools i Visual Studio
[SQL Server Data Tools] [ SQL Server Data Tools] (SSDT) i Visual Studio kan du tooconnect att hantera och utveckla dina databaser. Om du är programutvecklare som är bekant med Visual Studio eller andra integrerad utvecklingsmiljöer (IDEs) kan du använda SSDT i Visual Studio.

SSDT innehåller hello SQL Server Object Explorer, vilket gör att du toovisualize, ansluta och köra skript mot SQL Data Warehouse-databaser. tooquickly ansluta tooSQL Data Warehouse, klickar du bara hello **öppna i Visual Studio** knappen i kommandofältet hello när du visar hello databasinformation i hello klassiska Azure-portalen.  

tooget igång med SSDT i Visual Studio finns [frågan Azure SQL Data Warehouse med Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].

### <a name="command-line-tools"></a>Kommandoradsverktyg
Kommandoradsverktyg lämpar sig för att automatisera dina arbetsbelastningar.  PowerShell och sqlcmd är två bra sätt tooautomate dina processer.  Vi rekommenderar att dessa verktyg för att hantera ett stort antal logiska servrar och distribuera resursändringar i en produktionsmiljö som hello nödvändiga åtgärder kan användas av skriptet och sedan automatiseras.

### <a name="dynamic-management-views"></a>Dynamiska hanteringsvyer
Av DMV: er är hello bröd och smör för att hantera SQL Data Warehouse. Nästan alla information som hämtar hello-portalen är beroende av DMV: er. toosee en lista över SQL Data Warehouse av DMV: er, se [SQL Data Warehouse systemvyer][SQL Data Warehouse system views].

tooget igång, se [Anslut och fråga med sqlcmd][Connect and query with sqlcmd], och [skapa en databas (PowerShell)][Create a database (PowerShell)].

## <a name="scale-compute"></a>Skala bearbetning
Du kan snabbt skala ut eller igen genom att öka eller minska beräkningsresurser av CPU, minne och i/o-bandbredd i SQL Data Warehouse. tooscale prestanda, allt du behöver toodo är justera hello antalet informationslagerenheter (dwu: er) som SQL Data Warehouse allokerar tooyour databas. SQL Data Warehouse gör hello ändra snabbt och hanterar alla hello underliggande ändringar toohardware eller programvara.

toolearn mer om att skala dwu: er, se [skala prestanda].

## <a name="pause-and-resume"></a>Pausa och återuppta
toosave kostnader, du kan pausa och återuppta beräkna resurser på begäran. Till exempel om du inte använder hello databasen under hello natten och helger, kan du pausa under dessa tider och återuppta under hello dag. Du kommer inte att debiteras för dwu: er medan hello databasen har pausats.

Mer information finns i [pausa beräkning][Pause compute], och [återuppta databearbetning][Resume compute].

## <a name="performance-best-practices"></a>Bästa praxis för prestanda
När du börjar arbeta med en ny teknik, kan identifiera hello tips och råd som fungerar bäst direkt från hello start spara mycket tid.  Du hittar bästa praxis i många av våra artiklar.

toosee många en sammanfattning av hello viktigaste att tänka på när du utvecklar din arbetsbelastning finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].

## <a name="query-monitoring"></a>Övervakning av frågan
En fråga körs ibland för länge, men du är inte säker på vilken som är hello sällan. SQL Data Warehouse har dynamiska hanteringsvyer (av DMV: er) som du kan använda toofigure reda på vilka frågan tar för lång.

toofind tidskrävande frågor finns [övervaka din arbetsbelastning med av DMV: er][Monitor your workload using DMVs].

## <a name="security"></a>Säkerhet
toomaintain ett säkerhetssystem som du måste vara på hello avisering och skydda datorn mot alla typer av obehörig åtkomst. Ett säkerhetssystem måste toomake till brandväggsregler som finns på plats så att endast auktoriserade IP-adresser kan ansluta. Den måste korrekt autentisering av användarautentiseringsuppgifter. När en användare har anslutits toohello databasen hello användare ska bara ha behörigheter tooperform ett minimalt antal åtgärder. toosecure data, kan du använda kryptering. Det är också viktigt toohave granskning och spåra så att du kan gå händelser om det inte finns några misstänkt aktivitet.

toolearn om att hantera säkerhet, head över toohello [Säkerhetsöversikt][Security overview].

## <a name="backup-and-restore"></a>Säkerhetskopiering och återställning
Med tillförlitlig backps av dina data är en viktig del av en produktionsdatabas. SQL Data Warehouse hålls data säker genom att säkerhetskopiera active databaserna automatiskt med jämna mellanrum. Dessa säkerhetskopior kan du toorecover från scenarier där du har skadade data eller av misstag bort data eller databasen.  För hello data schemat för säkerhetskopiering, bevarandeprincip och hur toorestore en databas, se [återställning från ögonblicksbild][Restore from snapshot].

## <a name="next-steps"></a>Nästa steg
Med bra databas principer gör det enklare toomanage dina databaser i SQL Data Warehouse. fler toolearn head över toohello [utvecklingsöversikt][Development overview].

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Create a database (PowerShell)]: sql-data-warehouse-get-started-provision-powershell.md
[connection]: sql-data-warehouse-develop-connections.md
[Query Azure SQL Data Warehouse with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Connect and query with sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Development overview]: sql-data-warehouse-overview-develop.md
[Monitor your workload using DMVs]: sql-data-warehouse-manage-monitor.md
[Pause compute]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Restore from snapshot]: sql-data-warehouse-restore-database-overview.md
[Resume compute]: sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[skala prestanda]: sql-data-warehouse-manage-compute-overview.md#scale-compute
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse Best Practices]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse system views]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
