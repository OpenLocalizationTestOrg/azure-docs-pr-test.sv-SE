---
title: aaaAzure Data Lake Store mellan region migrering | Microsoft Docs
description: "Mer information om migrering mellan region för Azure Data Lake Store."
services: data-lake-store
documentationcenter: 
author: swums
manager: amitkul
editor: swums
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/27/2017
ms.author: stewu
ms.openlocfilehash: 561ac821c1bd555886035867678cb685997564eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-data-lake-store-across-regions"></a>Migrera Data Lake Store över regioner

Eftersom Azure Data Lake Store blir tillgängliga i nya regioner, kan du välja toodo en enstaka migrering tootake nytta av nya hello-regionen. Lär dig vilka tooconsider som du planerar och slutföra hello migreringen.

## <a name="prerequisites"></a>Krav

* **En Azure-prenumeration**. Mer information finns i [skapa din kostnadsfria Azure-konto idag](https://azure.microsoft.com/pricing/free-trial/).
* **Ett Data Lake Store-konto i två olika regioner**. Mer information finns i [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md).
* **Azure Data Factory**. Mer information finns i [introduktion tooAzure Data Factory](../data-factory/data-factory-introduction.md).


## <a name="migration-considerations"></a>Överväganden vid migrering

Först identifiera hello migreringsstrategi som passar bäst för ditt program som skriver, läser eller bearbetar data i Data Lake Store. När du väljer en strategi, Överväg programmets tillgänglighet och hello-avbrott som uppstår under migreringen. Den enklaste metoden kanske till exempel toouse hello ”lift och SKIFT” migrering modell. I den här metoden pausar hello program i din befintliga region medan kopieras alla dina data är toohello ny region. När hello kopieringen är klar du återuppta ditt program i hello ny region och tar bort hello gamla Data Lake Store-konto. Driftstopp under migreringen hello krävs.

tooreduce driftstopp, du kan starta direkt vill föra in nya data i hello ny region. När du har hello minsta mängd data som behövs, kör ditt program i hello nya region. Fortsätt toocopy äldre data från hello befintliga Data Lake Store-konto toohello ny Data Lake Store-konto i hello nya region i hello bakgrund. Med den här metoden kan du hello växeln toohello ny region med lite avbrottstid. Ta bort hello gamla Data Lake Store-konto när alla hello äldre data har kopierats.

Andra viktiga information tooconsider när du planerar migreringen är:

* **Datavolymen**. hello datavolymen (i GB hello antal filer och mappar och så vidare) påverkar hello tid och resurser som du behöver för hello migrering.

* **Data Lake Store-kontonamnet**. hello nya kontonamnet i hello nya region måste vara globalt unika. Hello namnet på ditt gamla Data Lake Store-konto i östra USA 2 kan till exempel vara contosoeastus2.azuredatalakestore.net. Ditt nya Data Lake Store-konto i Norra Europa contosonortheu.azuredatalakestore.net kan namnet.

* **Verktyg för**. Vi rekommenderar att du använder hello [Azure Data Factory-Kopieringsaktiviteten](../data-factory/data-factory-azure-datalake-connector.md) toocopy Data Lake Store-filer. Data Factory har stöd för flytt av data med hög prestanda och tillförlitlighet. Tänk på att Data Factory kopierar endast hello mapphierarkin och innehållet i hello-filer. Du behöver toomanually gäller alla åtkomstkontrollistor (ACL) som du använder i hello gamla konto toohello nytt konto. Mer information, inklusive prestandamål för bästa möjliga scenarier finns hello [prestandajustering guide och Kopieringsaktivitet prestanda](../data-factory/data-factory-copy-activity-performance.md). Om du vill kopiera snabbare data kan du behöva toouse ytterligare molnet Data Movement enheter. Vissa andra verktyg som AdlCopy, stöder inte kopiering av data mellan regioner.  

* **Kostnader för bandbredd**. [Kostnader för bandbredd](https://azure.microsoft.com/en-us/pricing/details/bandwidth/) gäller eftersom data överförs utanför en Azure-region.

* **ACL: er på dina data**. Skydda dina data i hello ny region genom att använda ACL: er toofiles och mappar. Mer information finns i [att skydda data som lagras i Azure Data Lake Store](data-lake-store-secure-data.md). Vi rekommenderar att du använder hello migrering tooupdate och justera dina ACL: er. Du kanske vill toouse liknande tooyour aktuella inställningar. Du kan visa hello ACL: er som används tooany filen med hjälp av hello Azure-portalen [PowerShell-cmdlets](/powershell/module/azurerm.datalakestore/get-azurermdatalakestoreitempermission), eller SDK: er.  

* **Platsen för Analystjänster**. För bästa prestanda bör din analytics tjänster, till exempel Azure Data Lake Analytics eller Azure HDInsight måste vara i hello samma region som dina data.  

## <a name="next-steps"></a>Nästa steg
* [Översikt över Azure Data Lake Store](data-lake-store-overview.md)
