---
title: "Vanliga frågor för Azure Data Lake Store | Microsoft-Docs"
description: "Riktlinjer för felsökning eller åtgärder med Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf7fd555-3e30-43ce-b28c-c3ad0a241fdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 44aaec9dc145b47b809a3ad4bc244eb9dfead24c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="frequently-asked-questions-for-azure-data-lake-store"></a>Vanliga frågor för Azure Data Lake Store
Läs mer om vanliga frågor om Azure Data Lake Store i den här artikeln.

## <a name="how-can-i-further-protect-my-data-from-region-wide-disasters-or-accidental-deletions"></a>Hur kan jag ytterligare skydda data mot regionomfattande katastrofer eller oavsiktliga borttagningar?
Data i ditt Azure Data Lake Store-konto är motståndskraftiga mot tillfälliga maskinvarufel inom en region, tack vare automatisk replikering. Detta säkerställer hållbarheten och hög tillgänglighet, vilket uppfyller SLA för Azure Data Lake Store. Detta avsnitt beskriver hur du kan skydda dina data ytterligare mot sällsynta driftavbrott som berör hela regioner eller oavsiktlig borttagning.

### <a name="disaster-recovery-guidance"></a>Vägledning om haveriberedskap
Det är viktigt för varje kund att förbereda en plan för haveriberedskap. I Azure-dokumentationen nedan finns information om hur du skapar din haveriberedskapsplan. Här finns resurser som hjälper dig att skapa ett eget schema.

* [Haveriberedskap och hög tillgänglighet för Azure-program](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Azure-återhämtning, tekniska riktlinjer](../resiliency/resiliency-technical-guidance.md)

#### <a name="best-practices"></a>Bästa praxis
Vi rekommenderar att du kopierar dina viktigaste data till ett annat Data Lake Store-konto i en annan region med en frekvens som är anpassad till din haverberedskapsplan. Det finns en mängd olika metoder för att kopiera data, inklusive [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) eller [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md). Azure Data Factory är användbart för att skapa och distribuera pipelines för datarörlighet regelbundet.

Om ett nationellt strömavbrott uppstår kan du sedan hämta dina data från regionen där de kopierades. Du kan övervaka [Hälsoinstrumentpanelen för Azure](https://azure.microsoft.com/status/) för att bestämma statusen för Azure-tjänsten i hela världen.

### <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Skadade data eller vägledning för återställning av oavsiktligt borttagna data
Azure Data Lake Store skyddar data med hjälp av automatiska repliker. Detta utgör dock inte ett skydd mot att ditt program (eller utvecklare/användare) skadar eller oavsiktligen raderar data.

#### <a name="best-practices"></a>Bästa praxis
För att förhindra oavsiktlig borttagning rekommenderar vi att du först ställer in rätt åtkomstprinciper för ditt Data Lake Store-konto.  Det innebär att tillämpa [lås för Azure-resurser](../azure-resource-manager/resource-group-lock-resources.md) för att låsa viktiga resurser samt tillämpa åtkomstkontroll på konto- och filnivå med de tillgängliga [säkerhetsfunktionerna i Data Lake Store](data-lake-store-security-overview.md). Vi rekommenderar dessutom att du skapar regelbundna kopior av dina viktigaste data med [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) eller [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) i ett annat Data Lake Store-konto eller Azure-prenumeration.  Detta kan användas för att återställa från skadade data eller en borttagning. Azure Data Factory är användbart för att skapa och distribuera pipelines för datarörlighet regelbundet.

Organisationer kan även aktivera [Diagnostisk loggning](data-lake-store-diagnostic-logs.md) för sitt konto för att samla in granskningshistorik för dataåtkomst till Azure Data Lake Store som ger information om vem som har tagit bort eller uppdaterat en fil.

## <a name="next-steps"></a>Nästa steg
* [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)

