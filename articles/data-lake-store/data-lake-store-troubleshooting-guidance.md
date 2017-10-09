---
title: "aaaFrequently frågor för Azure Data Lake Store | Microsoft Docs"
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
ms.openlocfilehash: eeabdeef18f3b72901bd1a14283f85dd9c0ead44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-azure-data-lake-store"></a>Vanliga frågor för Azure Data Lake Store
I den här artikeln får du kunskaper om vanliga frågor och svar relaterade tooAzure Data Lake Store.

## <a name="how-can-i-further-protect-my-data-from-region-wide-disasters-or-accidental-deletions"></a>Hur kan jag ytterligare skydda data mot regionomfattande katastrofer eller oavsiktliga borttagningar?
hello data i ditt Azure Data Lake Store-konto är flexibel tootransient maskinvarufel inom en region via automatisk repliker. Detta säkerställer hållbarhet och hög tillgänglighet, möte hello Azure Data Lake Store SLA. Här är några riktlinjer om hur toofurther skydda dina data från sällsynta region hela avbrott eller oavsiktliga borttagningar.

### <a name="disaster-recovery-guidance"></a>Vägledning om haveriberedskap
Det är viktigt för varje kund tooprepare sina egna plan för katastrofåterställning. Se toohello dokumentation för Azure nedan toobuild din plan för katastrofåterställning. Här finns resurser som hjälper dig att skapa ett eget schema.

* [Haveriberedskap och hög tillgänglighet för Azure-program](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Azure-återhämtning, tekniska riktlinjer](../resiliency/resiliency-technical-guidance.md)

#### <a name="best-practices"></a>Bästa praxis
Vi rekommenderar att du kopierar dina kritiska data tooanother Data Lake Store-konto i en annan region med en frekvens som justerad toohello behoven för din plan för katastrofåterställning. Det finns en mängd olika metoder toocopy data inklusive [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) eller [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md). Azure Data Factory är användbart för att skapa och distribuera pipelines för datarörlighet regelbundet.

Om ett regionalt strömavbrott kan sedan komma åt dina data i hello region där hello data har kopierats. Du kan övervaka hello [Azure Hälsoinstrumentpanelen](https://azure.microsoft.com/status/) toodetermine hello Azure Tjänststatus över hello jordglob.

### <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Skadade data eller vägledning för återställning av oavsiktligt borttagna data
Azure Data Lake Store skyddar data med hjälp av automatiska repliker. Detta utgör dock inte ett skydd mot att ditt program (eller utvecklare/användare) skadar eller oavsiktligen raderar data.

#### <a name="best-practices"></a>Bästa praxis
tooprevent oavsiktlig borttagning, rekommenderar vi att du först ställa in hello rätt åtkomstprinciper för ditt Data Lake Store-konto.  Detta inkluderar tillämpa [Azure-resurslås](../azure-resource-manager/resource-group-lock-resources.md) toolock ned viktiga resurser samt använda kontot och filen nivån behörighet med hello tillgängliga [Data Lake Store-säkerhetsfunktioner](data-lake-store-security-overview.md). Vi rekommenderar dessutom att du skapar regelbundna kopior av dina viktigaste data med [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) eller [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) i ett annat Data Lake Store-konto eller Azure-prenumeration.  Detta kan vara används toorecover från en data skadas eller tas bort incident. Azure Data Factory är användbart för att skapa och distribuera pipelines för datarörlighet regelbundet.

Organisationer kan också aktivera [diagnostikloggning](data-lake-store-diagnostic-logs.md) för sina Azure Data Lake Store-konto toocollect data granskningsspår från filåtkomstförsök som ger information om vem som kan ha tagits bort eller uppdaterats en fil.

## <a name="next-steps"></a>Nästa steg
* [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)

