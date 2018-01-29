---
title: "Skapa Azure-integrering körning i Azure Data Factory | Microsoft Docs"
description: "Lär dig mer om att skapa Azure-integrering körning i Azure Data Factory som används för att kopiera data och skicka transformeringen aktiviteter."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: spelluru
ms.openlocfilehash: b48e08233fb6457c180b280af1076838845fcc94
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/23/2018
---
# <a name="how-to-create-and-configure-azure-integration-runtime"></a>Hur du skapar och konfigurerar Azure Integration Runtime
Integration Runtime (IR) är beräkningsinfrastrukturen som används av Azure Data Factory för att tillhandahålla data integrationsmöjligheter mellan olika nätverksmiljöer. Läs mer om IR [integrering runtime](concepts-integration-runtime.md).

> [!NOTE]
> Den här artikeln gäller för version 2 av Data Factory, som för närvarande är en förhandsversion. Om du använder version 1 av Data Factory-tjänsten, som är allmänt tillgänglig (GA), se [Data Factory V1 dokumentationen](v1/data-factory-introduction.md).

Azure IR tillhandahåller en helt hanterad beräkning för att utföra internt data rörelser och sändning av data transformation aktiviteter för att beräkna tjänster som HDInsight. Det finns i Azure-miljön och stöder anslutning till resurser i offentliga nätverk med offentligt tillgängliga slutpunkter.

Det här dokumentet beskriver hur du kan skapa och konfigurera Azure Integration Runtime. 

## <a name="default-azure-ir"></a>Standard Azure IR
Som standard har varje datafabriken en Azure-IR i serverdelen som har stöd för åtgärder på molnet data lagras och beräkna tjänster i offentligt nätverk. Platsen för den Azure-IR är Lös automatiskt. Om **connectVia** egenskapen inte har angetts i den länkade tjänstdefinitionen angivna Azure-IR används. Behöver du bara skapa en Azure-IR explicit när du vill ange platsen för IR, eller om du vill gruppera praktiskt taget aktivitet körningar på olika IRs för hantering av ändamål. 

## <a name="create-azure-ir"></a>Skapa Azure IR
Integration körning kan skapas med hjälp av **Set AzureRmDataFactoryV2IntegrationRuntime** PowerShell-cmdlet. Om du vill skapa en Azure-IR ange namn, plats och typ för kommandot. Här är en Exempelkommando för att skapa en Azure-IR med platsen angetts till ”västra Europa”:

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName "SampleV2DataFactory1" -Name "MySampleAzureIR" -ResourceGroupName "ADFV2SampleRG" -Type Managed -Location "West Europe"
```  
För Azure IR typ måste anges till **hanteras**. Du behöver inte ange beräkning information eftersom den fullständigt hanterade Elastiskt i molnet. Ange beräkning information som nodstorlek och noden antal när du vill skapa Azure-SSIS IR. Mer information finns i [skapa och konfigurera Azure SSIS-IR](create-azure-ssis-integration-runtime.md).

Du kan konfigurera en befintlig Azure-IR om du vill ändra dess plats med hjälp av cmdlet Set-AzureRmDataFactoryV2IntegrationRuntime PowerShell. Mer information om platsen för en Azure-IR finns [introduktion till integration runtime](concepts-integration-runtime.md).

## <a name="use-azure-ir"></a>Använd Azure IR

När en Azure-IR har skapats kan använda du det i din länkade tjänstdefinitionen. Nedan är ett exempel på hur du kan referera till Azure Integration Runtime skapade ovan från en länkad tjänst för Azure-lagring:  

```json
{
    "name": "MyStorageLinkedService",
    "properties": {
      "type": "AzureStorage",
      "typeProperties": {
        "connectionString": {
          "value": "DefaultEndpointsProtocol=https;AccountName=myaccountname;AccountKey=...",
          "type": "SecureString"
        }
      },
      "connectVia": {
        "referenceName": "MySampleAzureIR",
        "type": "IntegrationRuntimeReference"
      }   
    }
}

```

## <a name="next-steps"></a>Nästa steg
Se följande artiklar om hur du skapar andra typer av integration körningar:

- [Skapa Integration Runtime med egen värd](create-self-hosted-integration-runtime.md)
- [Skapa Azure-SSIS-integrering runtime](create-azure-ssis-integration-runtime.md)
 
