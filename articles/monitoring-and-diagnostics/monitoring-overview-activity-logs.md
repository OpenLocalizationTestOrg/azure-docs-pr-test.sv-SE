---
title: aaaOverview av hello Azure-aktivitetsloggen | Microsoft Docs
description: "Lär dig vilka hello Azure-aktivitetsloggen är och hur du kan använda den toounderstand händelser i din Azure-prenumeration."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c274782f-039d-4c28-9ddb-f89ce21052c7
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: johnkem
ms.openlocfilehash: cba79b7f6dc0833ef588382e763761fd77d43307
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-subscription-activity-with-hello-azure-activity-log"></a>Övervaka aktiviteten prenumerationen med hello Azure-aktivitetsloggen
Hej **Azure-aktivitetsloggen** är en prenumerationslogg som ger inblick i prenumerationsnivån händelser som har inträffat i Azure. Detta omfattar en mängd data från Azure Resource Manager användningsdata tooupdates på händelser för Hälsotjänst. hello aktivitetsloggen tidigare kallades ”granskningsloggar” eller ”operativa loggar” eftersom hello administrativa kategorin rapporter kontroll-plan händelser för dina prenumerationer. Använder hello aktivitetsloggen, kan du bestämma hello ' vad som, och när ' för alla skrivåtgärder (PUT, POST, ta bort) vidtagits hello resurser i din prenumeration. Du kan också förstå hello status för hello igen och andra relevanta egenskaper. hello aktivitetsloggen omfattar inte läsåtgärder (GET) eller åtgärder för resurser som använder hello klassisk / ”RDFE” modellen.

![Aktiviteten loggar eller andra typer av loggar ](./media/monitoring-overview-activity-logs/Activity_Log_vs_other_logs_v5.png)

Bild 1: Skicka aktivitetsloggar vs andra typer av loggar

hello aktivitetsloggen skiljer sig från [diagnostikloggar](monitoring-overview-of-diagnostic-logs.md). Aktivitetsloggar ger information om hello åtgärder på en resurs från hello utanför (hello ”kontrollplan”). Diagnostik loggar orsakat av en resurs och ger information om hello åtgärden för den resursen (hello ”dataplan”).

Du kan hämta händelser från din aktivitetsloggen med hello Azure-portalen CLI, PowerShell-cmdletar och REST-API för Azure-Monitor.


> [!WARNING]
> hello Azure-aktivitetsloggen är främst för aktiviteter som sker i Azure Resource Manager. Den spårar inte resurser med hjälp av hello klassisk/RDFE-modellen. Vissa typer av klassiska resurser har en proxy-resursprovidern i Azure Resource Manager (till exempel Microsoft.ClassicCompute). Om du interagerar med en klassisk resurstyp via Azure Resource Manager med hjälp av dessa providers för proxy-resurs visas hello åtgärder i hello aktivitetsloggen. Om du interagerar med en klassisk resurstyp i hello klassisk portal eller på annat sätt utanför hello Azure Resource Manager-proxyservrar, dina åtgärder endast registreras i hello Åtgärdslogg. Hej Åtgärdsloggen är endast tillgänglig i hello klassisk portal.
>
>

Visa hello följande video introducing hello aktivitetsloggen.
> [!VIDEO https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz/player]
> 
>

## <a name="categories-in-hello-activity-log"></a>Kategorier i hello aktivitetsloggen
hello aktivitetsloggen innehåller flera kategorier av data. Mer information om hello scheman av dessa kategorier [finns i den här artikeln](monitoring-activity-log-schema.md). Exempel på dessa är:
* **Administrativa** -den här kategorin innehåller hello post för alla skapa, uppdatera, ta bort och åtgärden åtgärder utföras via Resource Manager. Exempel på typer av händelser som visas i den här kategorin omfattar hello ”Skapa virtuell dator” och ”ta bort nätverkssäkerhetsgruppen” varje åtgärd som en användare eller program med hjälp av hanteraren för filserverresurser modelleras som en åtgärd på en viss resurstyp. Om hello åtgärd av typen skriva, ta bort eller åtgärden hello-poster för både hello start- och lyckas eller misslyckas av åtgärden registreras i hello administrativa kategorin. hello administrativa kategorin omfattar även eventuella ändringar toorole-baserad åtkomstkontroll i en prenumeration.
* **Tjänstens hälsa** -den här kategorin innehåller hello post för varje service hälsa incidenter som har inträffat i Azure. Ett exempel på hello typen av händelse visas i den här kategorin är ”SQL Azure i östra USA upplever driftstopp”. Hälsa av tjänsten-händelser finns i fem sorter: åtgärd krävs stödd återställning, Incident, underhåll, Information eller säkerhet, och visas endast om du har en resurs i hello-prenumeration som skulle påverkas av hello-händelse.
* **Varning** -den här kategorin innehåller hello post för alla aktiveringar av Azure-aviseringar. Ett exempel på hello typen av händelse visas i den här kategorin är ”processor på myVM har över 80 för hello senaste 5 minuter”. En mängd Azure system har en aviseringar begrepp – du kan definiera en regel av något slag och ett meddelande när villkor matchar regeln. Varje gång en stöds Azure aviseringstyp 'aktiveras,' eller hello villkor är uppfyllda toogenerate ett meddelande, en post för hello aktiveringen skickas också hello aktivitetsloggen toothis kategori.
* **Autoskala** -den här kategorin innehåller hello post för varje händelser relaterade toohello användning av hello Autoskala motorn baserat på automatiska inställningar du har definierat i din prenumeration. Ett exempel på hello typen av händelse visas i den här kategorin är ”Autoskala skalas upp misslyckades”. Använda autoskalning kan du automatiskt skala ut eller skala i hello antalet instanser i en stöds resurstyp baserat på tid på dagen och/eller belastningen (mått) data med hjälp av en autoskalningsinställning. Tooscale uppåt eller nedåt, hello start och lyckades eller misslyckades händelser registreras i den här kategorin när hello villkor är uppfyllda.
* **Rekommendation** -den här kategorin innehåller rekommendation händelser från vissa typer av resurser, till exempel webbplatser och SQL-servrar. Dessa händelser ger rekommendationer för hur toobetter utnyttjar resurserna. Du får bara händelser för den här typen om du har resurser som genererar rekommendationer.
* **Princip-, säkerhets- och Resource Health** -dessa kategorier innehåller inte några händelser, de är reserverad för framtida användning.

## <a name="event-schema-per-category"></a>Händelseschema per kategori
[Se den här artikeln toounderstand hello aktivitetsloggen Händelseschema per kategori.](monitoring-activity-log-schema.md)

## <a name="what-you-can-do-with-hello-activity-log"></a>Vad du kan göra med hello aktivitetsloggen
Här följer några av hello saker du kan göra med hello aktivitetsloggen:

![Azure-aktivitetsloggen](./media/monitoring-overview-activity-logs/Activity_Log_Overview_v3.png)


* Fråga efter och visa den i hello **Azure-portalen**.
* [Skapa en avisering för en aktivitet händelselogg.](monitoring-activity-log-alerts.md)
* [Strömma den tooan **Händelsehubb** ](monitoring-stream-activity-logs-event-hubs.md) för införandet av en tjänst från tredje part eller anpassade analytics lösning, till exempel PowerBI.
* Analysera i PowerBI med hello [ **PowerBI Innehållspaketet**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
* [Spara den tooa **Lagringskonto** för arkivering eller Manuell kontroll](monitoring-archive-activity-log.md). Du kan ange hello kvarhållningstiden (i dagar) med hjälp av hello **loggen profil**.
* Fråga den via PowerShell-cmdleten, CLI eller REST API.

## <a name="query-hello-activity-log-in-hello-azure-portal"></a>Frågan hello aktivitetsloggen i hello Azure-portalen
Du kan visa din aktivitetsloggen på flera platser inom hello Azure-portalen:
* Hej **aktivitetsloggen bladet**, du kan komma åt genom att söka efter hello aktivitetsloggen under ”fler tjänster” hello vänstra navigeringsfönstret.
* Hej **övervakaren bladet**, som visas som standard i hello vänstra navigeringsfönstret. hello aktivitetsloggen är en del av den här Azure-Monitor-bladet.
* Alla resurser **resursbladet**, till exempel hello configuration bladet för en virtuell dator. hello aktivitetsloggen är hello avsnitt i de flesta av dessa resurs-blad, och klicka på den automatiskt filtrerar hello händelser toothose relaterade toothat viss resurs.

Du kan filtrera din aktivitetsloggen av fälten i hello Azure-portalen:
* TimeSpan - hello start- och -tid för händelser.
* Kategori – hello händelsekategori enligt beskrivningen ovan.
* Prenumerationen - namn för en eller flera Azure-prenumeration.
* Resursgrupp – en eller flera resursgrupper i dessa prenumerationer.
* Resurs (namn) - hello namnet på en viss resurs.
* Resurstyp - hello typen av resurs, till exempel Microsoft.Compute/virtualmachines.
* Åtgärdsnamnet - hello namnet på en Azure Resource Manager-åtgärd, till exempel Microsoft.SQL/servers/Write.
* Allvarlighetsgrad - hello allvarlighetsgraden hello-händelse (kritisk information, varning, fel,).
* Händelse som initieras av - hello anroparen eller användaren som utförde åtgärden hello.
* Öppna sökning - detta är en öppen Sök textruta som söker efter strängen i alla fält i alla händelser.

När du har definierat en uppsättning filter kan du spara den som en fråga som är beständiga mellan sessioner om du skulle tooperform hello samma fråga med dessa filter igen i hello framtida. Du kan även fästa en fråga tooyour Azure instrumentpanelen tooalways Håll koll på specifika händelser.

Klicka på ”Använd” kör frågan och visa alla matchande händelser. Klicka på alla händelser i hello lista visar hello sammanfattning av händelsen samt hello fullständig rådata JSON av händelsen.

För ännu mer kraft kan du klicka på hello **loggen Sök** ikon som visar data aktivitetsloggen i hello [Log Analytics aktivitet logganalys lösning](../log-analytics/log-analytics-activity.md). hello aktivitetsloggen bladet erbjuder en grundläggande filter/Bläddra upplevelse på loggar, men logganalys aktiverar toopivot, fråga och visualisera dina data mer kraftfulla sätt.

## <a name="export-hello-activity-log-with-a-log-profile"></a>Exportera hello aktivitetsloggen med en logg-profil
En **loggen profil** styr hur din aktivitetsloggen exporteras. Med en logg-profil kan konfigurera du:

* Där hello aktivitetsloggen ska skickas (Storage-konto eller Händelsehubbar)
* Vilka kategorier (, ta bort, skrivåtgärd) ska skickas. *hello betydelse ”kategori” i loggen profiler och aktivitetsloggen händelser är olika. I hello loggen profil representerar ”kategori” hello åtgärdstypen (, ta bort, skrivåtgärd). I en aktivitet händelselogg representerar hello ”” Kategoriegenskaper hello käll- eller typen av händelse (till exempel Administration, ServiceHealth, aviseringen och mer).*
* Vilka regioner (platser) ska exporteras. Gör att tooinclude ”globala” som många händelser i hello aktivitetsloggen är globala händelser.
* Hur länge hello aktivitetsloggen ska behållas i ett Lagringskonto.
    - En kvarhållning av noll dagar innebär loggar behålls alltid. Annars kan hello värdet vara valfritt antal dagar mellan 1 och 2147483647.
    - Om bevarandeprinciper har angetts men lagring loggar i ett Storage-konto har inaktiverats (till exempel om endast Händelsehubbar eller OMS-alternativen är markerade), har hello bevarandeprinciper ingen effekt.
    - Bevarandeprinciper tillämpade per dag, så vid hello slutet på dagen (UTC) loggar från hello dag som inte har nu hello bevarandeprincip tas bort. Till exempel om du har en bevarandeprincip på en dag, skulle hello början av hello dagen idag hello loggar från hello dag före igår tas bort.

Du kan använda ett lagringskonto eller event hub-namnområde som inte är i hello samma prenumeration som hello en avger loggar. hello-användare som konfigurerar hello inställning måste ha hello lämpliga RBAC åtkomst tooboth prenumerationer.

De här inställningarna kan konfigureras via hello ”Export” alternativ i hello aktivitetsloggen bladet i hello portal. De kan också konfigureras via programmering [med hello Azure övervakaren REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell-cmdlets eller CLI. En prenumeration kan bara ha en logg-profil.

### <a name="configure-log-profiles-using-hello-azure-portal"></a>Konfigurera loggen profiler som använder hello Azure-portalen
Du kan strömma hello aktivitetsloggen tooan Event Hub eller lagra dem i ett Lagringskonto med hjälp av hello ”Export” alternativet i hello Azure-portalen.

1. Navigera toohello **aktivitetsloggen** blad med hjälp av hello-menyn på vänster sida av hello portal hello.

    ![Navigera tooActivity logg i portalen](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klicka på hello **exportera** knappen hello överst i hello-bladet.

    ![Exportera-knappen i portalen](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Du kan välja i hello-bladet som visas:  
  * regioner som du vill att tooexport händelser
  * Hej Lagringskonto toowhich som toosave händelser
  * hello antal dagar som du vill ha tooretain dessa händelser i lagringen. En inställning på 0 dagar behåller alltid hello loggar.
  * hello Service Bus Namespace som du vill att en Händelsehubb toobe som skapats för direktuppspelning av dessa händelser.

     ![Exportera aktivitetsloggen bladet](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klicka på **spara** toosave inställningarna. hello-inställningarna är omedelbart vara tillämpade tooyour prenumeration.

### <a name="configure-log-profiles-using-hello-azure-powershell-cmdlets"></a>Konfigurera loggen profiler som använder hello Azure PowerShell-Cmdlets
#### <a name="get-existing-log-profile"></a>Hämta befintlig logg-profil
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Lägg till en logg-profil
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Egenskap | Krävs | Beskrivning |
| --- | --- | --- |
| Namn |Ja |Namnet på loggen profilen. |
| StorageAccountId |Nej |Resurs-ID för hello Lagringskonto toowhich hello aktivitetsloggen ska sparas. |
| serviceBusRuleId |Nej |Du vill att toohave händelsehubbar som skapats i Service Bus regel-ID för hello Service Bus-namnrymd. Är en sträng med formatet: `{service bus resource ID}/authorizationrules/{key name}`. |
| Platser |Ja |Kommaavgränsad lista över regioner som du vill att toocollect aktivitetsloggen händelser. |
| retentionInDays |Ja |Antal dagar för vilka händelser som ska behållas, mellan 1 och 2147483647. Värdet noll lagrar hello loggar på obestämd tid (alltid). |
| Kategorier |Nej |Kommaavgränsad lista över kategorier som ska samlas in. Möjliga värden är skriva och ta bort åtgärd. |

#### <a name="remove-a-log-profile"></a>Ta bort en logg-profil
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-hello-azure-cross-platform-cli"></a>Konfigurera loggen profiler med hjälp av hello Azure plattformsoberoende CLI
#### <a name="get-existing-log-profile"></a>Hämta befintlig logg-profil
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
Hej `name` egenskapen ska vara hello namnet på loggen profilen.

#### <a name="add-a-log-profile"></a>Lägg till en logg-profil
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Egenskap | Krävs | Beskrivning |
| --- | --- | --- |
| namn |Ja |Namnet på loggen profilen. |
| storageId |Nej |Resurs-ID för hello Lagringskonto toowhich hello aktivitetsloggen ska sparas. |
| serviceBusRuleId |Nej |Du vill att toohave händelsehubbar som skapats i Service Bus regel-ID för hello Service Bus-namnrymd. Är en sträng med formatet: `{service bus resource ID}/authorizationrules/{key name}`. |
| Platser |Ja |Kommaavgränsad lista över regioner som du vill att toocollect aktivitetsloggen händelser. |
| retentionInDays |Ja |Antal dagar för vilka händelser som ska behållas, mellan 1 och 2147483647. Värdet noll lagrar hello loggar på obestämd tid (alltid). |
| Kategorier |Nej |Kommaavgränsad lista över kategorier som ska samlas in. Möjliga värden är skriva och ta bort åtgärd. |

#### <a name="remove-a-log-profile"></a>Ta bort en logg-profil
```
azure insights logprofile delete --name my_log_profile
```

## <a name="next-steps"></a>Nästa steg
* [Mer information om hello aktivitetsloggen (tidigare granskningsloggar)](../azure-resource-manager/resource-group-audit.md)
* [Strömma hello Azure-aktivitetsloggen tooEvent hubbar](monitoring-stream-activity-logs-event-hubs.md)
