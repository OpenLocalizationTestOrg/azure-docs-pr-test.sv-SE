---
title: "aaaComputer grupper i logganalys logga sökningar | Microsoft Docs"
description: "Datorgrupper i logganalys Tillåt tooscope loggen sökningar tooa viss uppsättning datorer.  Den här artikeln beskriver hello olika metoder som du kan använda toocreate datorgrupper och hur toouse dem i en logg för att söka."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: 7dafea9829e541f5582a1d855fafb82aa4d94430
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Datorgrupper i logganalys logga sökningar

>[!NOTE]
> Den här artikeln beskriver hello använder datorgrupper med hello aktuella loggen Anayltics frågespråk.    Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), datorgrupper fungera på olika sätt.  Information finns i den här artikeln med hello annan syntax och beteende hello nya query Language.  


Datorgrupper i logganalys Tillåt tooscope [logga sökningar](log-analytics-log-searches.md) tooa viss uppsättning datorer.  Varje grupp fylls med datorer som antingen använder en fråga som du definierar eller genom att importera grupper från olika källor.  När hello gruppen ingår i en logg-sökning, är hello resultaten begränsad toorecords som matchar hello datorer i hello-gruppen.

## <a name="creating-a-computer-group"></a>Skapa en datorgrupp
Du kan skapa en datorgrupp i Log Analytics med hjälp av hello metoder i hello i den följande tabellen.  Information om varje metod finns i hello avsnitten nedan. 

| Metod | Beskrivning |
|:--- |:--- |
| Loggsökning |Skapa en logg sökning som returnerar en lista med datorer och spara hello resultat som en datorgrupp. |
| Loggsöknings-API |Använd hello loggen Sök API tooprogrammatically skapa en datorgrupp baserat på hello resultaten av en logg. |
| Active Directory |Sök igenom hello gruppmedlemskap för alla agentdatorer som som är medlemmar i en Active Directory-domän och skapa en grupp i Log Analytics för varje säkerhetsgrupp automatiskt. |
| WSUS |Skanna WSUS-servrar eller klienter för att rikta sig till grupper och skapa en grupp i Log Analytics för varje automatiskt. |

### <a name="log-search"></a>Loggsökning
Datorgrupper som skapas från en sökning i loggen innehåller alla hello-datorer som returneras av en fråga som du definierar.  Den här frågan körs varje gång hello datorgrupp används så att ändringar eftersom hello grupp har skapats visas.

Använd hello följande procedur toocreate en datorgrupp från en logg-sökning.

1. [Skapa en logg sökning](log-analytics-log-searches.md) som returnerar en lista med datorer.  hello sökning måste returnera en specifik uppsättning datorer med hjälp av något som liknar **distinkta datorn** eller **mäta count() per dator** i hello-frågan.  
2. Klicka på hello **spara** knappen hello överst i hello-skärmen.
3. Välj **Ja** för**spara frågan som en datorgrupp**.
4. Ange en **namn** och en **kategori** för hello grupp.  Om en sökning med hello samma namn och kategori redan finns, kommer du att ange toooverwrite den.  Du kan ha flera sökningar med hello samma namn i olika kategorier. 

Följande är exempelsökningar som du kan spara som en datorgrupp.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md) sedan hello följande ändringar har gjorts toohello proceduren toocreate en ny datorgrupp.
>  
> - Hej frågan toocreate en datorgrupp måste innehålla `distinct Computer`.  Följande är ett exempel på en fråga toocreate en datorgrupp.<br>`Heartbeat | where Computer contains "srv" `
> - När du skapar en ny datorgrupp måste du ange ett alias i tillägg toohello namn.  Du kan använda hello-alias när du använder hello datorgrupp i en fråga som beskrivs nedan.  

### <a name="log-search-api"></a>Logga Sök-API
Datorgrupper som skapats med hello loggen Sök API är hello samma som sökningar som skapats med en logg-sökning.

Mer information om hur du skapar en datorgrupp med hello loggen Sök-API finns [datorgrupper i logganalys-loggen Sök REST API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory
När du konfigurerar logganalys tooimport Active Directory-gruppmedlemskap, analyserar hello gruppmedlemskap för alla domänanslutna datorer med hello OMS-agent.  En datorgrupp skapas i Log Analytics för varje säkerhetsgrupp i Active Directory och varje dator läggs toohello datorgrupper motsvarande toohello säkerhetsgrupper som de är medlemmar i.  Det här medlemskapet uppdateras kontinuerligt var fjärde timme.  

Du konfigurerar logganalys tooimport Active Directory-säkerhetsgrupper från hello **datorgrupper** -menyn i logganalys **inställningar**.  Välj **Automation** och sedan **importera Active Directory-gruppmedlemskap från datorer**.  Det krävs ingen ytterligare konfiguration.

![Datorgrupper från Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

När grupper har importerats hello menyn visar hello antal datorer med gruppmedlemskap identifierat och hello antalet grupper som importeras.  Du kan klicka på någon av dessa länkar tooreturn hello **ComputerGroup** poster med den här informationen.

### <a name="windows-server-update-service"></a>Windows Server Update Service
När du konfigurerar logganalys tooimport WSUS-gruppmedlemskap, analyseras hello gruppmedlemskap för alla datorer med hello OMS-agent som mål.  Om du använder klientsidan har målobjekt för alla datorer som är anslutna tooOMS och ingår i alla WSUS rikta sig till grupper dess gruppmedlemskap importeras tooLog Analytics. Om du använder serversidan importerade riktad hello OMS-agenten ska installeras på hello WSUS server för hello gruppmedlemskap information toobe tooOMS.  Det här medlemskapet uppdateras kontinuerligt var fjärde timme. 

Du konfigurerar logganalys tooimport Active Directory-säkerhetsgrupper från hello **datorgrupper** -menyn i logganalys **inställningar**.  Välj **Active Directory** och sedan **importera Active Directory-gruppmedlemskap från datorer**.  Det krävs ingen ytterligare konfiguration.

![Datorgrupper från Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

När grupper har importerats hello menyn visar hello antal datorer med gruppmedlemskap identifierat och hello antalet grupper som importeras.  Du kan klicka på någon av dessa länkar tooreturn hello **ComputerGroup** poster med den här informationen.

## <a name="managing-computer-groups"></a>Hantera datorgrupper
Du kan visa datorgrupper som skapades från en logg sökning eller hello loggen Sök-API från hello **datorgrupper** -menyn i logganalys **inställningar**.  Klicka på hello **x** i hello **ta bort** kolumnen toodelete hello datorgrupp.  Klicka på hello **visa medlemmar** ikonen för en grupp toorun hello grupps loggen sökning som returnerar dess medlemmar. 

![Sparade datorgrupper](media/log-analytics-computer-groups/configure-saved.png)

toomodify hello, skapa en ny grupp med hello samma **kategori** och **namn** toooverwrite hello ursprungliga gruppen.

## <a name="using-a-computer-group-in-a-log-search"></a>Använda en datorgrupp i en logg-sökning
Du kan använda hello följande syntax toorefer tooa datorgrupp i en sökning i loggen.  Att ange hello **kategori** är valfria och endast krävs om du har datorgrupper med hello samma namn i olika kategorier. 

    $ComputerGroups[Category: Name]

När en sökning körs matchas hello medlemmar i alla datorgrupper som ingår i hello Sök först.  Om hello grupp baserad på en logg-sökning, sedan körs sökningen tooreturn hello medlemmar i gruppen för hello innan du utför hello översta loggen sökning.

Datorgrupper används vanligtvis med hello **IN** -satsen i hello loggen Sök som i följande exempel hello:

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), kan du använda en datorgrupp i en fråga genom att behandla dess alias som en funktion som i följande exempel hello:
> 
>  `UpdateSummary | where Computer IN (MyComputerGroup)`

## <a name="computer-group-records"></a>Gruppen datorposter
En post har skapats i hello OMS-databasen för varje datorgruppmedlemskap som skapats från Active Directory eller WSUS.  Dessa poster har en typ av **ComputerGroup** och ha hello egenskaper i hello i den följande tabellen.  Poster skapas inte för datorgrupper baserat på loggen sökningar.

| Egenskap | Beskrivning |
|:--- |:--- |
| Typ |*ComputerGroup* |
| SourceSystem |*SourceSystem* |
| Dator |Namnet på hello medlemsdator. |
| Grupp |Namnet på hello grupp. |
| GroupFullName |Fullständig sökväg toohello grupp inklusive hello käll- och namnet på datakällan. |
| GroupSource |Datakällan gruppen har samlats in från. <br><br>ActiveDirectory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName |Namnet på hello-källa som hello grupp samlats in från.  Detta är hello domännamnet för Active Directory. |
| ManagementGroupName |Namnet på hanteringsgruppen för hello för SCOM-agenter.  För andra agenter är AOI -\<arbetsyte-ID\> |
| TimeGenerated |Datum och tid hello datorgrupp skapats eller uppdaterats. |

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar.  

