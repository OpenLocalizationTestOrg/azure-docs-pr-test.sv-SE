---
title: "aaaData källor som stöds i Azure Analysis Services | Microsoft Docs"
description: "Beskriver de datakällor som stöds för datamodeller i Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 2902d7d3c3bcf086419822fa826193bd247bde61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Datakällor som stöds i Azure Analysis Services
Azure Analysis Services-servrar som stöder anslutning toodata källor i hello molnet och lokalt i din organisation. Ytterligare datakällor läggs alla hello tid. Kom tillbaka ofta. 

hello följande datakällor stöds:

| Molnet  |
|---|
| Azure Blob Storage *  |
| Azure SQL Database  |
| Azure Data Warehouse |


| Lokal  |   |   |   |
|---|---|---|---|
| Access-databas  | Mappen * | Oracle-databas  | Teradata-databas |
| Active Directory *  | JSON-dokumentet *  | Postgre SQL-databasen *  |XML-tabellen * |
| Analysis Services  | Rader från binary *  | SAP HANA *  |
| Analytics Platform System  | MySQL-databas  | SAP Business Warehouse *  | |
| Dynamics CRM *  | OData-Feed *  | SharePoint *  |
| Excel-arbetsbok  | ODBC-fråga  | SQL Database  |
| Exchange *  | OLE DB  | Sybase-databas  |

\*1400 tabellmodeller endast. 

> [!IMPORTANT]
> Ansluta tooon lokala data källor kräver en [lokala datagateway](analysis-services-gateway.md) installeras på en dator i din miljö.

## <a name="data-providers"></a>Dataleverantörer

Datamodeller i Azure Analysis Services kan kräva olika dataleverantörer när du ansluter toocertain datakällor. I vissa fall kan tabellmodeller ansluta toodata källor med hjälp av inbyggda providrar, till exempel SQL Server Native Client (SQLNCLI11) returnerar ett fel.

För datamodeller som ansluter tooa molndata källa, till exempel Azure SQL Database, om du använder inbyggda providers än SQLOLEDB kanske du ser felmeddelandet: **”hello-providern 'SQLNCLI11.1' inte är registrerad”.** Om du har en DirectQuery modell anslutande tooon lokala datakällor, om du använder inbyggda providrar kan du läsa felmeddelande: **”Error OLE DB raden skapades. Felaktig syntax nära 'LIMIT' ”**.

hello följande datasource providers stöds för i minnet eller DirectQuery datamodeller när anslutande toodata datakällor i hello molnet eller lokalt:

### <a name="cloud"></a>Molnet
| **DataSource** | **Minnesintern** | **DirectQuery** |
|  --- | --- | --- |
| Azure SQL Data Warehouse |.NET framework dataprovider för SQLServer |.NET framework dataprovider för SQLServer |
| Azure SQL Database |.NET framework dataprovider för SQLServer |.NET framework dataprovider för SQLServer | |

### <a name="on-premises-via-gateway"></a>Lokalt (via Gateway)
|**DataSource** | **Minnesintern** | **DirectQuery** |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0 |.NET framework dataprovider för SQLServer |
| SQL Server |Microsoft OLE DB Provider för SQLServer |.NET framework dataprovider för SQLServer | |
| SQL Server |.NET framework dataprovider för SQLServer |.NET framework dataprovider för SQLServer | |
| Oracle |Microsoft OLE DB Provider för Oracle |Oracle dataprovider för .NET | |
| Oracle |Oracle dataprovider för .NET |Oracle dataprovider för .NET | |
| Teradata |OLE DB Provider för Teradata |Teradata-dataprovidern för .NET | |
| Teradata |Teradata-dataprovidern för .NET |Teradata-dataprovidern för .NET | |
| Analytics Platform System |.NET framework dataprovider för SQLServer |.NET framework dataprovider för SQLServer | |

> [!NOTE]
> Kontrollera 64-bitars providrar är installerade när du använder lokala gateway.
> 
> 

När du migrerar en lokal SQL Server Analysis Services tabellmodell tooAzure Analysis Services, kan det vara nödvändigt toochange hello-providern.

**toospecify en datasource-provider**

1. I SSDT > **Tabular modellen Explorer** > **datakällor**, högerklicka på anslutning till en datakälla och klicka sedan på **redigera datakällan**.
2. I **Redigera anslutning**, klickar du på **Avancerat** tooopen hello avancerade egenskapsfönster.
3. I **avancerade egenskaper för** > **Providers**, och sedan väljer hello lämplig provider.

## <a name="impersonation"></a>Personifiering
I vissa fall kanske den nödvändiga toospecify en annan personifieringskontot. Personifieringskontot kan anges i SSDT eller SSMS.

För lokala datakällor:

* Om du använder SQL-autentisering vara personifiering tjänstkontot.
* Ange Windows användarlösenord om du använder Windows-autentisering. Windows-autentisering med en specifik personifieringskontot stöds endast för datamodeller i minnet för SQL Server.

För molnet datakällor:

* Om du använder SQL-autentisering vara personifiering tjänstkontot.

## <a name="next-steps"></a>Nästa steg
Om du har lokala datakällor kan vara säker på att tooinstall hello [lokala gateway](analysis-services-gateway.md).   
toolearn mer information om hur du hanterar servern i SSDT eller SSMS, se [hantera servern](analysis-services-manage.md).

