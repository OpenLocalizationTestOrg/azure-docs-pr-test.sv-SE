---
title: aaaManaging Azure Automation data | Microsoft Docs
description: "Den här artikeln innehåller flera avsnitt för att hantera en Azure Automation-miljö.  För närvarande innehåller datalagring och säkerhetskopiering av Azure Automation-katastrofåterställning i Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: 2896f129-82e3-43ce-b9ee-a3860be0423a
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/02/201
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: 46a164d864c4956c90ab689ca159fff6f6c08028
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-automation-data"></a>Hantera Azure Automation-data
Den här artikeln innehåller flera avsnitt för att hantera en Azure Automation-miljö.

## <a name="data-retention"></a>Datakvarhållning
När du tar bort en resurs i Azure Automation finns den kvar i 90 dagar för granskning innan den tas bort permanent.  Du kan inte se eller använda hello resurs under denna tid.  Den här principen gäller även tooresources som tillhör tooan automation-konto som har tagits bort.

Azure Automation tas bort automatiskt och tar permanent bort jobb som är äldre än 90 dagar.

hello följande tabell sammanfattas hello bevarandeprincipen för olika resurser.

| Data | Princip |
|:--- |:--- |
| Konton |Tas bort permanent 90 dagar efter hello kontot har tagits bort av en användare. |
| Tillgångar |Tas bort permanent 90 dagar efter hello tillgången tas bort av en användare eller 90 dagar efter hello kontot som innehåller hello tillgången har tagits bort av en användare. |
| Moduler |Tas bort permanent 90 dagar efter hello modulen tas bort av en användare eller 90 dagar efter hello konto som innehåller hello modulen tas bort av en användare. |
| Runbooks |Tas bort permanent 90 dagar efter hello resurs tas bort av en användare eller 90 dagar efter hello kontot som innehåller hello resurs har tagits bort av en användare. |
| Jobb |Borttagna och tas bort permanent 90 dagar efter det sista som ska ändras. Detta kan vara när hello jobbet har slutförts, stoppas eller pausas. |
| Noden konfigurationer/MOF-filer |Gamla nodkonfiguration bort permanent 90 dagar efter att en ny konfiguration av noden genereras. |
| DSC-noder |Bort permanent 90 dagar efter hello nod har avregistrerats för Automation-konto med hjälp av Azure-portalen eller hello [Unregister-AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) cmdlet i Windows PowerShell. Noder tas bort 90 dagar efter hello-konto som innehåller hello noden tas bort av en användare även permanent. |
| Noden rapporter |Bort permanent 90 dagar efter att en ny rapport skapas för noden |

hello bevarandeprincip gäller tooall användare och för närvarande inte kan anpassas.

Om du behöver tooretain data under en längre tid kan vidarebefordra du dock runbook-jobbet loggar tooLog Analytics.  Mer information finns [vidarebefordra Azure Automation-jobbet data tooOMS logganalys](automation-manage-send-joblogs-log-analytics.md).   

## <a name="backing-up-azure-automation"></a>Säkerhetskopiera Azure Automation
När du tar bort ett automation-konto i Microsoft Azure raderas alla objekt i hello-konto inklusive runbooks, moduler, konfigurationer, inställningar, jobb och tillgångar. hello-objekt kan inte återställas efter hello kontot har tagits bort.  Du kan använda hello följande information toobackup hello innehållet i ditt automation-konto innan den tas bort. 

### <a name="runbooks"></a>Runbooks
Du kan exportera runbooks tooscript filerna med hjälp av hello Azure Management Portal eller hello [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) cmdlet i Windows PowerShell.  Dessa filer kan importeras till en annan automation-konto som beskrivs i [skapa eller importera en Runbook](https://msdn.microsoft.com/library/dn643637.aspx).

### <a name="integration-modules"></a>Integreringsmoduler
Du kan inte exportera integreringsmoduler från Azure Automation.  Du måste se till att de är tillgängliga utanför hello automation-konto.

### <a name="assets"></a>Tillgångar
Du kan inte exportera [tillgångar](https://msdn.microsoft.com/library/dn939988.aspx) från Azure Automation.  Använder hello Azure-hanteringsportalen, måste du Observera hello information om variabler, autentiseringsuppgifter, certifikat, anslutningar och scheman.  Därefter måste du manuellt skapa alla objekt som används av runbooks som importeras till en annan automation.

Du kan använda [Azure-cmdlets](https://msdn.microsoft.com/library/dn690262.aspx) tooretrieve information om okrypterad tillgångar och antingen spara dem för framtida referera eller skapa motsvarande tillgångar i en annan automation-kontot.

Du kan inte hämta hello värde för krypterade variabler eller hello lösenordsfältet autentiseringsuppgifter med hjälp av cmdlet: ar.  Om du inte känner till dessa värden, så du kan hämta dem från en runbook med hello [Get-automationvariable,](https://msdn.microsoft.com/library/dn940012.aspx) och [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) aktiviteter.

Du kan inte exportera certifikat från Azure Automation.  Du måste se till att det finns några certifikat utanför Azure.

### <a name="dsc-configurations"></a>DSC-konfigurationer
Du kan exportera konfigurationer tooscript filerna med hjälp av hello Azure Management Portal eller hello [Export AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) cmdlet i Windows PowerShell. De här konfigurationerna kan importeras och används i en annan automation-kontot.

## <a name="geo-replication-in-azure-automation"></a>GEO-replikering i Azure Automation
GEO-replikering, standard i Azure Automation-konton, säkerhetskopierar konto tooa olika geografiska dataområdet för redundans. Du kan välja en primär region när du konfigurerar ditt konto och en sekundär region tilldelas tooit automatiskt. hello sekundära data kopieras från hello primära regionen, uppdateras kontinuerligt händelse av dataförlust.  

hello följande tabell visar hello tillgängliga primära och sekundära region pairings.

| Primär | Sekundär |
| --- | --- |
| Södra centrala USA |Norra centrala USA |
| Östra USA 2 |Centrala USA |
| Västra Europa |Norra Europa |
| Sydostasien |Östasien |
| Östra Japan |Västra Japan |

Hello osannolika som en primär regiondata går förlorade, Microsoft försöker toorecover den. Om hello primära data inte kan återställas, geo-redundans utförs och hello berörda kunder ska meddelas om detta via sin prenumeration.

