---
title: "aaaMove Data tooor från Azure Blob Storage med hjälp av SSIS-anslutningar | Microsoft Docs"
description: "Flytta Data tooor från Azure Blob Storage med hjälp av SSIS-anslutningar."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 96a1b5fb-34d1-4b9b-8d99-2bb8289e0398
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 15068af1c69f11e74e109ee5ae2b9f1a674ea388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooor-from-azure-blob-storage-using-ssis-connectors"></a>Flytta data tooor från Azure Blob Storage med hjälp av SSIS-anslutningar
Hej [Funktionspaketet för SQL Server Integration Services för Azure](https://msdn.microsoft.com/library/mt146770.aspx) innehåller komponenter tooconnect tooAzure, överföra data mellan Azure och lokala datakällor och bearbetning av data lagras i Azure.

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

När kunder har flyttat lokala data i hello moln kan de komma åt den från alla Azure-tjänsten tooleverage hello full effekt hello uppsättning tekniker för Azure. Den kan användas, till exempel i Azure Machine Learning eller på ett HDInsight-kluster.

Detta är vanligtvis vara hello första steget för hello [SQL](machine-learning-data-science-process-sql-walkthrough.md) och [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) genomgång.

En beskrivning av kanoniska scenarier som använder SSIS tooaccomplish företag måste gemensamma i hybrid data integrationsscenarier, finns [göra mer med SQL Server Integration Services Feature Pack för Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blogg.

> [!NOTE]
> Fullständig presentation tooAzure blob storage finns för[grunderna i Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) och för[Azure Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Krav
tooperform hello uppgifter som beskrivs i den här artikeln måste du ha en Azure-prenumeration och skapa ett Azure storage-konto. Du måste veta din Azure storage-konto och nyckel tooupload eller hämta data.

* tooset upp en **Azure-prenumeration**, se [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).
* Anvisningar om hur du skapar en **lagringskonto** och för att hämta konto och viktig information, se [om Azure storage-konton](../storage/common/storage-create-storage-account.md).

toouse hello **SSIS kopplingar**, måste du hämta:

* **SQL Server 2014 eller Standard 2016 (eller senare)**: Installera omfattar SQL Server Integration Services.
* **Microsoft SQL Server 2014 eller 2016 Integration Services Feature Pack för Azure**: dessa kan laddas ner, respektive från hello [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) och [SQL Server 2016-Integration Tjänster](https://www.microsoft.com/download/details.aspx?id=49492) sidor.

> [!NOTE]
> SSIS installeras med SQL Server, men det ingår inte i hello Express-version. Information om vilka program som ingår i olika utgåvor av SQL Server finns [SQL Server-versioner](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)
> 
> 

Utbildningsmaterial på SSIS finns [händerna på utbildning för SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Mer information om hur tooget upp och körs med SISS toobuild enkel extrahering, transformering och laddning (ETL) paket finns i [SSIS-Självstudier: skapa ett enkelt ETL-paket](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Hämta NYC Taxi dataset
hello exempel som beskrivs här använder en offentligt tillgängliga datauppsättningen--hello [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset. hello dataset består av om 173 miljoner taxi bilar i NYC hello år 2013. Det finns två typer av data: resa information data och avgiften data. Det finns en fil för varje månad, har vi 24 filer i alla som är cirka 2GB okomprimerade.

## <a name="upload-data-tooazure-blob-storage"></a>Ladda upp data tooAzure blob-lagring
toomove data med hjälp av hello SSIS Funktionspaket för från lokala tooAzure blob storage, vi använder en instans av hello [ **Azure Blob överför uppgiften**](https://msdn.microsoft.com/library/mt146776.aspx), visas här:

![Konfigurera-data-vetenskap-vm](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

hello-parametrar som hello aktiviteten använder beskrivs:

| Fält | Beskrivning |
| --- | --- |
| **AzureStorageConnection** |Anger en befintlig Azure Storage Connection Manager eller skapar en ny som refererar tooan Azure storage-konto som pekar toowhere hello blob-filerna finns. |
| **BlobContainer** |Anger hello blob-behållaren som håller hello överföra filer som blobar hello namn. |
| **BlobDirectory** |Anger hello blob katalog där hello överförda filen lagras som en blockblobb. hello blob-katalogen är en virtuell hierarkisk struktur. Om det redan finns hello blob it ia ersättas. |
| **LocalDirectory** |Anger hello lokal katalog som innehåller hello filer toobe har överförts. |
| **Filnamn** |Anger ett namn filter tooselect filer med hello angivna namnmönster. Till exempel MySheet\*.xls\* innehåller filer som MySheet001.xls och MySheetABC.xlsx |
| **TimeRangeFrom/TimeRangeTo** |Anger ett tidsfilter för intervallet. Filer som modifierats efter *TimeRangeFrom* och innan *TimeRangeTo* ingår. |

> [!NOTE]
> Hej **AzureStorageConnection** autentiseringsuppgifter måste rätt toobe och hello **BlobContainer** måste finnas innan hello överföring görs.
> 
> 

## <a name="download-data-from-azure-blob-storage"></a>Hämta data från Azure blob storage
toodownload data från Azure blob storage tooon lokal lagring med SSIS, använda en instans av hello [Azure Blob överför uppgiften](https://msdn.microsoft.com/library/mt146779.aspx).

## <a name="more-advanced-ssis-azure-scenarios"></a>Mer avancerade SSIS-Azure-scenarier
Funktionspaket för hello SSIS möjliggör mer komplexa flöden toobe hanteras av paketering uppgifter tillsammans. Hello blob-data kan till exempel feed direkt till ett HDInsight-kluster, vars utdata gick att hämta tillbaka tooa blob och tooon lokal lagring. SSIS kan köra Hive och Pig-jobb på ett HDInsight-kluster med hjälp av ytterligare SSIS-anslutningar:

* en Hive-skript i ett Azure HDInsight-kluster med SSIS, Använd toorun [Azure HDInsight Hive uppgiften](https://msdn.microsoft.com/library/mt146771.aspx).
* Pig-skriptet på en Azure HDInsight-kluster med SSIS, Använd toorun [Azure HDInsight Pig aktiviteten](https://msdn.microsoft.com/library/mt146781.aspx).

