---
title: "aaaIntegrating Data Lake Store med andra Azure-tjänster | Microsoft Docs"
description: "Förstå hur Data Lake Store integreras med andra Azure-tjänster"
documentationcenter: 
services: data-lake-store
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 48a5d1f4-3850-4c22-bbc4-6d1d394fba8a
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 839689f04623f8225884e7aa9c0b533a09323e80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-data-lake-store-with-other-azure-services"></a>Integrera Data Lake Store med andra Azure-tjänster
Azure Data Lake Store kan användas tillsammans med andra Azure-tjänster tooenable ett brett spektrum av scenarier. hello visar följande artikel hello-tjänster som Data Lake Store kan integreras med.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Använd Data Lake Store med Azure HDInsight
Du kan etablera ett [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) kluster som använder Data Lake Store som hello HDFS-kompatibla lagring. Den här versionen kan för Hadoop och Storm-kluster i Windows och Linux, du använda Data Lake Store enbart som ett ytterligare lagringsutrymme. Sådana kluster kan du fortfarande använda Azure Storage (WASB) som hello standardlagring. Dock gäller HBase-kluster i Windows och Linux, kan du använda Data Lake Store som hello standardlagring eller den ytterligare lagringsutrymme.

Anvisningar för hur tooprovision ett HDInsight-kluster med Data Lake Store, se:

* [Etablera ett HDInsight-kluster med Data Lake Store med hjälp av Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Tillhandahålla ett HDInsight-kluster med Data Lake Store som standardlagring med hjälp av Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Tillhandahålla ett HDInsight-kluster med Data Lake Store som ytterligare lagringsutrymme med hjälp av Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)

## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Använd Data Lake Store med Azure Data Lake Analytics
[Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) kan du toowork med Stordata i molnskala. Den resurser etableras dynamiskt och du kan utföra analyser på terabyte eller även exabyte av data som kan lagras i ett antal datakällor som stöds, ett av dem som Data Lake Store. Data Lake Analytics är speciellt optimerade toowork med Azure Data Lake Store - ger hello högsta prestanda, genomströmning och parallellisering för dina arbetsbelastningar med stordata.

Anvisningar för hur toouse Data Lake Analytics med Data Lake Store finns [Kom igång med Data Lake Analytics med hjälp av Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="use-data-lake-store-with-azure-data-factory"></a>Använd Data Lake Store med Azure Data Factory
Du kan använda [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) tooingest data från Azure-tabeller, Azure SQL Database, Azure SQL DataWarehouse, Azure Storage-Blobbar och lokala databaser. Som en förstklassigt allmänheten i hello Azure ekosystemet vara Azure Data Factory används tooorchestrate hello införandet av data från dessa källa tooAzure Data Lake Store.

Anvisningar för hur toouse Azure Data Factory med Data Lake Store finns [flytta data tooand från Data Lake Store med hjälp av Data Factory](../data-factory/data-factory-azure-datalake-connector.md).

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Kopiera data från Azure Storage-Blobbar till Data Lake Store
Azure Data Lake Store ger ett kommandoradsverktyg, AdlCopy som gör att du toocopy data från Azure Blob Storage till ett Data Lake Store-konto. Mer information finns i [kopiera data från Azure Storage BLOB tooData Datasjölager](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Kopiera data mellan Azure SQL Database och Data Lake Store
Du kan använda Apache Sqoop tooimport och exportera data mellan Azure SQL Database och Data Lake Store. Mer information finns i [kopiera data mellan Data Lake Store och Azure SQL database med Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

## <a name="use-data-lake-store-with-stream-analytics"></a>Använd Data Lake Store med Stream Analytics
Du kan använda Data Lake Store som ett hello matar ut toostore data som direktuppspelas med Azure Stream Analytics. Mer information finns i [strömma data från Azure Storage Blob till Data Lake Store med Azure Stream Analytics](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Använd Data Lake Store med Powerbi
Du kan använda Power BI tooimport data från ett Data Lake Store-konto tooanalyze och visualisera hello data. Mer information finns i [analysera data i Data Lake Store med hjälp av Power BI](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Använd Data Lake Store med Data Catalog
Du kan registrera data från Data Lake Store till hello Azure Data Catalog toomake hello data kan upptäckas hela hello organisation. Mer information finns i [registrera data från Data Lake Store i Azure Data Catalog](data-lake-store-with-data-catalog.md).

## <a name="use-data-lake-store-with-sql-server-integration-services-ssis"></a>Använd Data Lake Store med SQL Server Integration Services (SSIS)
Du kan använda Anslutningshanteraren för hello Azure Data Lake Store i SSIS tooconnect en SSIS-paket med Azure Data Lake Store. Mer information finns i [Använd Data Lake Store med SSIS](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager).

## <a name="use-data-lake-store-with-sql-data-warehouse"></a>Använd Data Lake Store med SQL Data Warehouse
Du kan använda PolyBase tooload data från Azure Data Lake Store till SQL Data Warehouse. Mer information finns i [Använd Data Lake Store med SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="see-also"></a>Se även
* [Översikt över Azure Data Lake Store](data-lake-store-overview.md)
* [Kom igång med Data Lake Store med hjälp av portalen](data-lake-store-get-started-portal.md)
* [Kom igång med Data Lake Store med hjälp av PowerShell](data-lake-store-get-started-powershell.md)  

