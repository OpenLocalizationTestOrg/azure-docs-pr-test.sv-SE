---
title: aaaStore och visa diagnostiska Data i Azure Storage | Microsoft Docs
description: "Hämta Azure diagnostikdata till Azure Storage och visa den"
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: 18e0780d-43e7-41e4-b8e9-f1fb9a36eb03
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: robb
ms.openlocfilehash: dd47a2ef6d6488c80c102c72b2ebf6ca6d2e473f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Lagra och visa diagnostiska data i Azure Storage
Diagnostikdata lagras inte permanent om du överför det toohello Microsoft Azure storage-emulatorn eller tooAzure lagring. En gång i lagring, den kan visas med en av flera tillgängliga verktyg.

## <a name="specify-a-storage-account"></a>Ange ett lagringskonto
Du kan ange hello storage-konto som du vill toouse i hello ServiceConfiguration.cscfg-filen. hello kontoinformation definieras som en anslutningssträng i en konfigurationsinställning. hello visar följande exempel hello standardanslutningssträngen som skapats för ett nytt Cloud Service-projekt i Visual Studio:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Du kan ändra den här tooprovide kontoinformationen i anslutningssträngen för Azure storage-konto.

Beroende på hello typ av diagnostiska data som samlas in, använder Azure-diagnostik hello Blob-tjänsten eller hello tabelltjänsten. hello följande tabell visar hello datakällor som sparas och format.

| Datakälla | Lagringsformat |
| --- | --- |
| Azure-loggar |Tabell |
| IIS 7.0-loggar |Blob |
| Azure Diagnostics infrastruktur loggar |Tabell |
| Kunde inte begära spårningsloggar |Blob |
| Windows-händelseloggar |Tabell |
| Prestandaräknare |Tabell |
| Krasch minnesdumpar |Blob |
| Anpassade fel |Blob |

## <a name="transfer-diagnostic-data"></a>Överför diagnostikdata
För SDK-2.5 och senare, kan det uppstå hello begäran tootransfer diagnostikdata via hello konfigurationsfilen. Du kan överföra diagnostikdata med schemalagda intervall som anges i hello konfiguration.

För SDK-2.4 och tidigare kan du begära tootransfer hello diagnostikdata via hello samt i konfigurationsfilen som programmässigt. hello programmässiga metod kan du också toodo på begäran överföringar.

> [!IMPORTANT]
> När du överför diagnostikdata tooan Azure storage-konto, medföra kostnader för hello lagringsresurser som använder din diagnostikdata.
> 
> 

## <a name="store-diagnostic-data"></a>Lagra diagnostikdata
Loggdata lagras i Blob eller tabell med hello följande namn:

**Tabeller**

* **WadLogsTable** - loggar som skrivs i kod med hello spårningslyssnaren.
* **WADDiagnosticInfrastructureLogsTable** -diagnostik ändringar av Övervakare och konfiguration.
* **WADDirectoriesTable** – kataloger som hello diagnostikövervakare övervakning.  Detta inkluderar IIS-loggar, IIS kunde inte loggar begäran och egna kataloger.  hello plats för hello blob-loggfilen anges i hello behållaren fältet och hello hello blob heter hello RelativePath fältet.  Hej AbsolutePath fältet anger hello plats och namn på hello som den såg ut hello virtuella Azure-datorn.
* **WADPerformanceCountersTable** – prestandaräknare.
* **WADWindowsEventLogsTable** – Windows-händelseloggarna.

**Blobbar**

* **bomullstuss-kontroll-container** – (endast för SDK-2.4 och tidigare) innehåller hello XML-konfigurationsfiler som styr hello Azure-diagnostik.
* **bomullstuss-iis-failedreqlogfiles** – innehåller information från IIS kunde inte begära loggar.
* **bomullstuss-iis-loggfiler** – innehåller information om IIS-loggar.
* **”anpassad”** – en anpassade container baserat på hur du konfigurerar kataloger som övervakas av hello diagnostikövervakare.  hello namn för den här blobbehållaren ska anges i WADDirectoriesTable.

## <a name="tools-tooview-diagnostic-data"></a>Verktyg tooview diagnostikdata
Flera verktyg är tillgängliga tooview hello data efter att den är överförda toostorage. Exempel:

* Server Explorer i Visual Studio - om du har installerat hello Azure Tools för Microsoft Visual Studio, du kan använda hello Azure Storage-nod i Server Explorer tooview skrivskyddade blob och tabellen data från Azure storage-konton. Du kan visa data från ditt konto för lokal lagring emulatorn och även från storage-konton du har skapat för Azure. Mer information finns i [webbsökning och hantera lagringsresurser med Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).
* [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en fristående app som du kan använda tooeasily fungerar med Azure Storage-data i Windows, OSX och Linux.
* [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) innehåller Azure Diagnostics Manager där du tooview, hämta och hantera hello diagnostikdata som samlas in av hello-program som körs på Azure.

## <a name="next-steps"></a>Nästa steg
[Spåra hello flödet i Cloud Services-program med Azure-diagnostik](cloud-services-dotnet-diagnostics-trace-flow.md)

