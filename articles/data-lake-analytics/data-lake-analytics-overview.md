---
title: aaaOverview av Microsoft Azure Data Lake Analytics | Microsoft Docs
description: "Data Lake Analytics är en Beräkningstjänst för Stordata som låter dig använda data toodrive företaget med hjälp av kunskap från dina data i hello molnet, oavsett dess storlek eller om det är."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 1e1d443a-48a2-47fb-bc00-bf88274222de
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/23/2017
ms.author: saveenr
ms.openlocfilehash: 15bcd549c5aeb167da1338f253270ad57f8c5123
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Översikt över Microsoft Azure Data Lake Analytics
## <a name="what-is-azure-data-lake-analytics"></a>Vad är Azure Data Lake Analytics?
Azure Data Lake Analytics är en på begäran analytics-jobbet service toosimplify analyser av stordata. Du kan fokusera på att skriva, köra och hantera jobb, i stället för att hantera distribuerad infrastruktur. I stället för att distribuera, konfigurera och justera maskinvara, skriva frågor tootransform dina data och extrahera värdefulla insikter. hello analytics-tjänsten kan hantera jobb av alla skalor direkt genom att ange hello skalan för hur mycket kraft du behöver. Du betalar endast för jobbet när tjänsten körs vilket gör den kostnadseffektiv. hello analytics-tjänsten stöder Azure Active Directory så att du kan hantera åtkomst och roller, integrerade med ditt lokala identitetssystem. Den omfattar också U-SQL, ett språk som kombinerar hello fördelarna hos SQL med hello direkta kraften hos användarkod. U-SQL skalbara distribuerade körning aktiverar du tooefficiently analysera data i hello arkivet och mellan SQL-servrar i Azure, Azure SQL Database och Azure SQL Data Warehouse.

## <a name="key-capabilities"></a>De viktigaste funktionerna
* **Dynamisk skalning**
  
    Data Lake Analytics har konstruerats för molnskalning och prestanda.  Resurser etableras dynamiskt och du kan utföra analyser på terabyte eller även exabyte av data. När hello jobbet är slutfört rullas resurser ner automatiskt och du betalar bara för hello processorkraft används. När du ökar eller minskar hello storleken på data som lagras eller hello mängden beräkningsresurser används inte toorewrite kod. Du kan helt och hållet fokusera på affärslogiken och inte på hur du bearbetar och lagrar stora datamängder.
* **Utveckla snabbare, felsök och optimera smartare med hjälp av välbekanta verktyg**
  
    Data Lake Analytics har djupgående integrering med Visual Studio, så du kan använda välbekanta verktyg toorun, felsöka och finjustera din kod. Med visualiseringar av dina U-SQL-jobb kan du se hur koden körs i skala, så att du lätt kan identifiera prestandaflaskhalsar och optimera kostnader.
* **U-SQL: enkelt och bekant, kraftfullt och utökningsbart**
  
    Data Lake Analytics innehåller U-SQL, ett frågespråk som utökar hello bekanta, enkla, deklarativa kompetensen hos SQL med hello expressiva kraften i C#. hello U-SQL-språket bygger på hello samma distribuerade körning som används hello system för stordata i Microsoft. Miljontals SQL och .NET-utvecklare kan nu bearbeta och analysera data med hello kunskaper som de redan har.
* **Kan helt integreras med dina IT-investeringar**
  
    Data Lake Analytics kan använda dina befintliga IT-investeringar för identitet, hantering, säkerhet och datalagring. Den här metoden förenklar datastyrning och gör det enkelt tooextend dina aktuella program. Data Lake Analytics är integrerat med Active Directory, så att användare och behörigheter kan hanteras, och integrerad övervakning och granskning ingår.
* **Prisvärt och kostnadseffektivt**
  
    Data Lake Analytics är en kostnadseffektiv lösning för arbetsbelastningar av stordata som körs. Du betalar per projekt när data bearbetas. Det krävs ingen maskinvara, licenser eller tjänstspecifikt supportavtal. hello systemet skalas automatiskt uppåt eller nedåt som hello jobbet startas och är klar så att du aldrig betalar för mer än vad du behöver.
* **Fungerar med alla dina Azure-data**
  
    Data Lake Analytics är optimerad toowork med Azure Data Lake - ger hello högsta prestanda, genomströmning och parallellisering för dina arbetsbelastningar för stordata.  Data Lake Analytics kan även användas med Azure Blob Storage och Azure SQL-databaser.

## <a name="next-steps"></a>Nästa steg
 
  * Komma igång med Data Lake Analytics med hjälp av [Azure Portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI](data-lake-analytics-get-started-cli2.md)
  * Hantera Azure Data Lake Analytics med hjälp av [Azure Portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md) | [Azure .NET SDK](data-lake-analytics-manage-use-dotnet-sdk.md) | [Node.js](data-lake-analytics-manage-use-nodejs.md)
  * [Övervaka och felsök Azure Data Lake Analytics-jobb med hjälp av Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md) 
