---
title: "aaaAzure SDK för .NET 2.9 viktig information"
description: "Azure SDK för .NET 2.9 viktig information"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 96df2b80224190cc2093e6bf350eaec224ac2e98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-29-release-notes"></a>Azure SDK för .NET 2.9 viktig information

Det här avsnittet innehåller viktig information för version 2.9 och 2.9.6 av Azure SDK för .NET.

##<a name="azure-sdk-for-net-296-release-summary"></a>Azure SDK för .NET 2.9.6 släpper sammanfattning

Utgivningsdatum: 11/16/2016
 
Inga senaste ändringar toohello Azure SDK 2.9 har lagts till i den här versionen. Det finns också några uppgraderingsprocessen behövs tooleverage detta SDK med befintliga Cloud Service-projekt.

### <a name="visual-studio-2017-release-candidate"></a>Visual Studio 2017 Release Candidate

- I Visual Studio 2017 RC är den här versionen av hello Azure SDK för .NET inbyggt i hello Azure arbetsbelastning. Alla hello verktyg du behöver toodo Azure-utveckling ska ingå i Visual Studio 2017 RC framöver. Visual Studio 2015 och Visual Studio 2013 är hello SDK fortfarande tillgänglig för via WebPI. Vi kommer att upphöra Azure SDK för .NET-versioner för Visual Studio 2013 när Visual Studio 2017 släpper som en slutlig produkt. Följ den här länken toodownload Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/

### <a name="azure-diagnostics"></a>Azure Diagnostics

- Ändrade hello beteende tooonly lagra en partiell anslutningssträng med hello nyckel ersättas med en token för anslutningssträngen för lagring av molntjänster diagnostik. hello faktiska lagringsnyckel lagras nu på hello mapp så att dess åtkomst kan kontrolleras. Visual Studio läser hello lagringsnyckel från mapp för lokala felsökning och publiceringsprocessen. 
- I svaret toohello ändra som beskrivs ovan, team Visual Studio Online förbättrad hello Azure Cloud Services Distributionsmall för aktiviteten så att användarna kan ange hello lagringsnyckel för att ställa in diagnostik tillägget när du publicerar tooAzure i kontinuerlig Integration och distribution.
- Vi har gjort det möjliga toostore säker anslutningssträngen och tokenisering för Azure Diagnostics (BOMULLSTUSS), toohelp du lösa problem med konfigurationen över environements.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016 virtuella datorer

- Visual Studio stöder nu distribuera molntjänster tooOS familj 5 (Windows Server 2016) virtuella datorer. Du kan ändra dina inställningar tootarget för befintliga molntjänster hello nya OS-familjen. När du skapar nya molntjänster, om du väljer toocreate hello-tjänsten med hjälp av .net 4.6 eller högre, kommer den som standard hello service toouse OS-familjen 5.  Mer information kan du granska hello [gäst-OS-familjen stöder tabellen](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).

#### <a name="known-issues"></a>Kända problem

- Azure .NET SDK 2.9.6 introducerades en begränsning som blockerar distributionen av projekt med hjälp av stöds inte .NET Framework (till exempel .NET 4.6) tooany OS-familjen < 5. En åtgärdsmetod [här](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).

 
### <a name="azure-in-role-cache"></a>Azure i Rollinstanser 

- Stöd för cachelagring i Rollinstanser för Azure ends i 30 November 2016. Mer information klickar du på [här](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).

### <a name="azure-resource-manager-templates-for-azure-stack"></a>Azure Resource Manager-mallar för Azure-Stack

- Vi har introducerat Azure Stack som mål för en distribution för din Azure Resource Manager-mallar.


## <a name="azure-sdk-for-net-29-summary"></a>Azure SDK för .NET 2.9 sammanfattning

## <a name="overview"></a>Översikt
Det här dokumentet innehåller hello viktig information för hello Azure SDK för .NET 2.9 versionen. 

Detaljerad information om uppdateringarna i den här versionen finns hello [Azure SDK 2.9 meddelande efter](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Förhandsgranska Azure SDK 2.9 för Visual Studio 2015 Update 2 och Visual Studio ”15”
Den här uppdateringen innehåller följande felkorrigeringar hello:

* Utfärda relaterade tooREST API klienten Generation i i vilka hello-strängen ”okänd typ” visas som hello hello kod gen mappens namn och/eller hello namnet på hello namnområde släppa i hello genererade koden.
* Utfärda relaterade tooScheduled WebJobs i vilka hello autentiseringsinformation misslyckas toobe skickades toohello Scheduler-etableringsprocessen.

Den här uppdateringen innehåller följande nya funktion hello:

* Stöd för sekundära Apptjänster hello ”tjänster”-fliken i hello etablering dialogrutan för Apptjänst. 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure Data Lake-verktyg för Visual Studio 2015 Update 2
Den här uppdateringar omfattar hello följande:

* **Azure Data Lake-verktyg** för Visual Studio nu sammanfogas hello Azure SDK för .NET-versionen. hello-verktyget installeras automatiskt när du installerar Azure SDK. 
  
    hello verktyget uppdateras ofta, gå [här](http://aka.ms/datalaketool) tooget hello uppdateringar.
* **Server Explorer** nu kan du alla tooview och skapa entiteter vissa U-SQL-metadata. Mer information finns i [detta](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blogg.

## <a name="hdinsight-tools"></a>HDInsight-verktyg
**HDInsight Tools** för Visual Studio nu stöder HDInsight version 3.3, inklusive visar Tez-diagram och andra språk åtgärdas.

## <a name="azure-resource-manager"></a>Azure Resource Manager
Den här versionen lägger till [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) stöd för Resource Manager-mallar.

## <a name="see-also"></a>Se även
[Azure SDK 2.9 meddelande post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

