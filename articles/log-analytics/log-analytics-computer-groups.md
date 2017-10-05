---
title: "Datorgrupper i logganalys logga sökningar | Microsoft Docs"
description: "Datorgrupper i logganalys kan du omfång loggen sökningen till en viss uppsättning datorer.  Den här artikeln beskriver de olika metoder som du kan använda för att skapa datorgrupper och hur de används i en sökning i loggen."
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
ms.openlocfilehash: a2ddc932343d54963a378ee27dc962a790326b2a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Datorgrupper i logganalys logga sökningar

>[!NOTE]
> Den här artikeln beskriver användningen av datorgrupper med hjälp av det aktuella loggen Anayltics frågespråket.    Om ditt arbetsområde har uppgraderats till den [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), datorgrupper fungera på olika sätt.  Anteckningar finns i den här artikeln med olika syntax och beteende för det nya språket i fråga.  


Datorgrupper i Log Analytics gör att du kan scope [logga sökningar](log-analytics-log-searches.md) till en viss uppsättning datorer.  Varje grupp fylls med datorer som antingen använder en fråga som du definierar eller genom att importera grupper från olika källor.  När gruppen ingår i en logg-sökning, är resultatet begränsade till poster som matchar datorerna i gruppen.

## <a name="creating-a-computer-group"></a>Skapa en datorgrupp
Du kan skapa en datorgrupp i Log Analytics med hjälp av någon av metoderna i följande tabell.  Information om varje metod finns i avsnitten nedan. 

| Metod | Beskrivning |
|:--- |:--- |
| Loggsökning |Skapa en logg sökning som returnerar en lista med datorer och spara resultatet som en datorgrupp. |
| Loggsöknings-API |Använd loggen Sök API för att programmatiskt skapa en datorgrupp baserat på resultatet av en sökning i loggen. |
| Active Directory |Sök igenom gruppmedlemskap för alla agentdatorer som som är medlemmar i en Active Directory-domän och skapa en grupp i Log Analytics för varje säkerhetsgrupp automatiskt. |
| WSUS |Skanna WSUS-servrar eller klienter för att rikta sig till grupper och skapa en grupp i Log Analytics för varje automatiskt. |

### <a name="log-search"></a>Loggsökning
Datorgrupper som skapas från en sökning i loggen innehåller alla datorer som returneras av en fråga som du definierar.  Den här frågan körs varje gång datorgruppen används så att ändringar eftersom gruppen har skapats visas.

Använd följande procedur för att skapa en datorgrupp från en logg-sökning.

1. [Skapa en logg sökning](log-analytics-log-searches.md) som returnerar en lista med datorer.  Sökningen måste returnera en specifik uppsättning datorer med hjälp av något som liknar **distinkta datorn** eller **mäta count() per dator** i frågan.  
2. Klicka på den **spara** längst upp på skärmen.
3. Välj **Ja** till **spara frågan som en datorgrupp**.
4. Ange en **namn** och en **kategori** för gruppen.  Om det redan finns en sökning med samma namn och kategori att du uppmanas att skriva över den.  Du kan ha flera sökningar med samma namn i olika kategorier. 

Följande är exempelsökningar som du kan spara som en datorgrupp.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

>[!NOTE]
> Om ditt arbetsområde har uppgraderats till den [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md) sedan följande ändringar görs i proceduren att skapa en ny datorgrupp.
>  
> - Frågan för att skapa en datorgrupp måste innehålla `distinct Computer`.  Följande är ett exempel på en fråga för att skapa en datorgrupp.<br>`Heartbeat | where Computer contains "srv" `
> - Du måste ange ett alias förutom namnet när du skapar en ny datorgrupp.  Du kan använda detta alias när du använder datorgruppen i en fråga som beskrivs nedan.  

### <a name="log-search-api"></a>Logga Sök-API
Datorgrupper som skapats med loggen Sök API är samma som sökningar som skapats med en logg-sökning.

Mer information om hur du skapar en datorgrupp med hjälp av loggen Sök-API finns [datorgrupper i logganalys-loggen Sök REST API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory
När du konfigurerar logganalys importera Active Directory-gruppmedlemskap, analyserar gruppmedlemskap för alla domänanslutna datorer med OMS-agent.  En datorgrupp skapas i Log Analytics för varje säkerhetsgrupp i Active Directory och varje dator läggs till datorgrupper som motsvarar de säkerhetsgrupper som de är medlemmar i.  Det här medlemskapet uppdateras kontinuerligt var fjärde timme.  

Du konfigurerar Log Analytics för att importera Active Directory-säkerhetsgrupper från den **datorgrupper** -menyn i logganalys **inställningar**.  Välj **Automation** och sedan **importera Active Directory-gruppmedlemskap från datorer**.  Det krävs ingen ytterligare konfiguration.

![Datorgrupper från Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

När grupper har importerats visas på menyn hur många datorer med gruppmedlemskap har identifierats och antalet grupper som importeras.  Du kan klicka på någon av dessa länkar för att returnera den **ComputerGroup** poster med den här informationen.

### <a name="windows-server-update-service"></a>Windows Server Update Service
När du konfigurerar logganalys importera WSUS-gruppmedlemskap, analyserar målobjekt gruppmedlemskap för alla datorer med OMS-agent.  Om du använder klientsidan har målobjekt för alla datorer som är ansluten till OMS och ingår i alla WSUS riktad grupper dess gruppmedlemskap importeras till logganalys. Om du använder serversidan ska inriktning på OMS agenten installeras på WSUS-servern för information som ska importeras till OMS gruppmedlemskap.  Det här medlemskapet uppdateras kontinuerligt var fjärde timme. 

Du konfigurerar Log Analytics för att importera Active Directory-säkerhetsgrupper från den **datorgrupper** -menyn i logganalys **inställningar**.  Välj **Active Directory** och sedan **importera Active Directory-gruppmedlemskap från datorer**.  Det krävs ingen ytterligare konfiguration.

![Datorgrupper från Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

När grupper har importerats visas på menyn hur många datorer med gruppmedlemskap har identifierats och antalet grupper som importeras.  Du kan klicka på någon av dessa länkar för att returnera den **ComputerGroup** poster med den här informationen.

## <a name="managing-computer-groups"></a>Hantera datorgrupper
Du kan visa datorgrupper som skapades från en logg sökning eller loggen Sök-API från den **datorgrupper** -menyn i logganalys **inställningar**.  Klicka på den **x** i den **ta bort** kolumnen att ta bort gruppen.  Klicka på den **visa medlemmar** ikonen för en grupp för att utföra gruppens loggen sökning som returnerar dess medlemmar. 

![Sparade datorgrupper](media/log-analytics-computer-groups/configure-saved.png)

Om du vill ändra gruppen kan du skapa en ny grupp med samma **kategori** och **namn** att skriva över den ursprungliga gruppen.

## <a name="using-a-computer-group-in-a-log-search"></a>Använda en datorgrupp i en logg-sökning
Du kan använda följande syntax för att referera till en datorgrupp i en sökning i loggen.  Ange den **kategori** är valfria och endast krävs om du har datorgrupper med samma namn i olika kategorier. 

    $ComputerGroups[Category: Name]

När en sökning körs matchas först medlemmarna i alla datorgrupper som ingår i sökningen.  Om gruppen är baserad på en logg-sökning, köra sökningen för att returnera medlemmar i gruppen innan du utför översta log-sökning.

Datorgrupper används vanligtvis med den **IN** satsen Sök i loggfilen som i följande exempel:

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

>[!NOTE]
> Om ditt arbetsområde har uppgraderats till den [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), kan du använda en datorgrupp i en fråga genom att behandla dess alias som en funktion som i följande exempel:
> 
>  `UpdateSummary | where Computer IN (MyComputerGroup)`

## <a name="computer-group-records"></a>Gruppen datorposter
En post har skapats i OMS-databasen för varje datorgruppmedlemskap som skapats från Active Directory eller WSUS.  Dessa poster har en typ av **ComputerGroup** och ha egenskaper i följande tabell.  Poster skapas inte för datorgrupper baserat på loggen sökningar.

| Egenskap | Beskrivning |
|:--- |:--- |
| Typ |*ComputerGroup* |
| SourceSystem |*SourceSystem* |
| Dator |Namnet på datorn som är medlem. |
| Grupp |Namnet på gruppen. |
| GroupFullName |Fullständig sökväg till gruppen inklusive käll- och namnet på datakällan. |
| GroupSource |Datakällan gruppen har samlats in från. <br><br>ActiveDirectory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName |Namnet på källan som gruppen har samlats in från.  Detta är domännamnet för Active Directory. |
| ManagementGroupName |Namnet på hanteringsgruppen för SCOM-agenter.  För andra agenter är AOI -\<arbetsyte-ID\> |
| TimeGenerated |Datum och tid datorgruppen skapats eller uppdaterats. |

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) att analysera data som samlas in från datakällor och lösningar.  

