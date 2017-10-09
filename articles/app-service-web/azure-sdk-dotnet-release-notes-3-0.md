---
title: "aaaAzure SDK för .NET 3.0 viktig information | Microsoft Docs"
description: "Azure SDK för .NET 3.0 viktig information"
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
ms.date: 03/07/2017
ms.author: juliako
ms.openlocfilehash: 8970b4c9b64de40dc29a33d69006a00ae5e38a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-30-release-notes"></a>Azure SDK för .NET 3.0 viktig information

Det här avsnittet innehåller viktig information för version 3.0 av hello Azure SDK för .NET.

##<a name="azure-sdk-for-net-30-release-summary"></a>Azure SDK för .NET 3.0-utgåvan sammanfattning

Utgivningsdatum: 03/07/2017
 
Inga senaste ändringar toohello Azure SDK 3.0 har lagts till i den här versionen. Det finns också några uppgraderingsprocessen behövs tooleverage detta SDK med befintliga Cloud Service-projekt. tooallow användning av hello Azure SDK 3.0 utan uppgraderingen, Azure SDK 3.0 installerar toohello samma kataloger som Azure SDK 2.9. De flesta hello komponenter att ändra inte hello huvudversion från 2.9 men i stället uppdatera hello build-nummer.

## <a name="visual-studio-2017-rtw"></a>Visual Studio 2017 RTW

- Den här versionen av hello Azure SDK för .NET är inbyggd i toohello Azure arbetsbelastning i Visual Studio 2017. Alla hello verktyg du behöver toodo Azure-utveckling ska ingå i Visual Studio 2017 framöver. Hello SDK blir fortfarande tillgängliga via WebPI för Visual Studio 2015. Vi har avbrutit Azure SDK för .NET-versioner för Visual Studio 2013 nu när Visual Studio 2017 har släppts.

### <a name="azure-diagnostics"></a>Azure Diagnostics

- Ändrade hello beteende tooonly lagra en partiell anslutningssträng med hello nyckel ersättas med en token för anslutningssträngen för lagring av molntjänster diagnostik. hello faktiska lagringsnyckel lagras nu på hello mapp så att dess åtkomst kan kontrolleras. Visual Studio läser hello lagringsnyckel från mapp för lokala felsökning och publiceringsprocessen. 
- I svaret toohello ändra som beskrivs ovan, team Visual Studio Online förbättrad hello Azure Cloud Services Distributionsmall för aktiviteten så att användarna kan ange hello lagringsnyckel för att ställa in diagnostik tillägget när du publicerar tooAzure i kontinuerlig Integration och distribution.
- Vi har gjort det möjliga toostore säker anslutningssträngen och tokenisering för Azure Diagnostics (BOMULLSTUSS), toohelp du lösa problem med konfigurationen över environements.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016 virtuella datorer

- Visual Studio stöder nu distribuera molntjänster tooOS familj 5 (Windows Server 2016) virtuella datorer. Du kan ändra dina inställningar tootarget för befintliga molntjänster hello nya OS-familjen. När du skapar nya molntjänster, om du väljer toocreate hello-tjänsten med hjälp av .net 4.6 eller högre, kommer den som standard hello service toouse OS-familjen 5.  Mer information kan du granska hello [gäst-OS-familjen stöder tabellen](../cloud-services/cloud-services-guestos-update-matrix.md).

### <a name="known-issues"></a>Kända problem

- Azure .NET SDK 3.0 introducerade ett problem när du tar bort Visual Studio-2017 i en konfiguration för sida vid sida med Visual Studio 2015.  Om du har hello Azure SDK för Visual Studio 2015 hello Microsoft Azure Storage-emulatorn och Microsoft Azure Compute Emulator kommer tas bort om du avinstallerar Visual Studio 2017.  Detta genererar ett fel när du skapar och felsökning nya Cloud services-projekt i Visual Studio 2015. I ordning toofix det här problemet genom att installera om hello Azure SDK från hello installationsprogram för webbplattform.  hello problemet löses i en framtida uppdatering för Visual Studio-2017.  .

 
### <a name="azure-in-role-cache"></a>Azure i Rollinstanser 

- Stöd för cachelagring i Rollinstanser för Azure upphörde 30 November 2016. Mer information klickar du på [här](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).




