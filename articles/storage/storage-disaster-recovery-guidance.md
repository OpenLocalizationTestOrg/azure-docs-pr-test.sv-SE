---
title: "aaaWhat toodo hello Azure Storage avbrott för händelsen | Microsoft Docs"
description: "Vilka toodo i ett Azure Storage-avbrott hello-händelse"
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 1/19/2017
ms.author: robinsh
ms.openlocfilehash: ee7eb71311c6e453dc078ec3566267ee0c2f444a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-toodo-if-an-azure-storage-outage-occurs"></a>Vilka toodo om ett Azure Storage-avbrott inträffar
Vi arbetar hårda toomake till våra tjänster alltid är tillgängliga på Microsoft. Ibland tvingar utöver våra styr hur oss på ett sätt som kan leda till oplanerade driftstopp i en eller flera regioner. toohelp som du hanterar dessa sällsynta förekomster vi ger hello följande övergripande riktlinjer för Azure Storage-tjänster.

## <a name="how-tooprepare"></a>Hur tooprepare
Det är viktigt för varje kund tooprepare sina egna plan för katastrofåterställning. hello arbete toorecover från ett avbrott för lagring innebär vanligtvis både operations personal och automatiserade procedurerna i ordning tooreactivate ditt program med fungerande tillstånd. Se toohello dokumentation för Azure nedan toobuild en egen plan för katastrofåterställning:

* [Haveriberedskap och hög tillgänglighet för Azure-program](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Azure-återhämtning, tekniska riktlinjer](../resiliency/resiliency-technical-guidance.md)
* [Azure Site Recovery-tjänsten](https://azure.microsoft.com/services/site-recovery/)
* [Azure Storage-replikering](storage-redundancy.md)
* [Azure Backup-tjänsten](https://azure.microsoft.com/services/backup/)

## <a name="how-toodetect"></a>Hur toodetect
hello rekommenderas sätt toodetermine hello Azure-tjänstestatus är toosubscribe toohello [Azure Hälsoinstrumentpanelen](https://azure.microsoft.com/status/).

## <a name="what-toodo-if-a-storage-outage-occurs"></a>Vilka toodo om ett lagringsutrymme strömavbrott uppstår
Om en eller flera lagringstjänster är otillgängliga just nu på en eller flera regioner, finns det två alternativ för att du tooconsider. Om du vill ha direktåtkomst tooyour data Överväg alternativ 2.

### <a name="option-1-wait-for-recovery"></a>Alternativ 1: Vänta tills återställningen
I så fall krävs ingen åtgärd. Vi arbetar ordentligt toorestore hello Azure-tjänstetillgänglighet. Du kan övervaka hello Tjänststatus på hello [Azure Hälsoinstrumentpanelen](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>Alternativ 2: Kopiera data från sekundär
Om du väljer [geo-redundant lagring med läsbehörighet (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (rekommenderas) för dina lagringskonton, du har läsbehörighet tooyour data från hello sekundär region. Du kan använda verktyg som [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), och hello [Azure Data Movement library](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) toocopy data från hello sekundär region till ett annat lagringskonto i en unimpacted region, och peka sedan program toothat lagringen konto för både läsa och skriva tillgänglighet.

## <a name="what-tooexpect-if-a-storage-failover-occurs"></a>Vilka tooexpect om det uppstår redundans lagring
Om du väljer [Geo-redundant lagring (GRS)](storage-redundancy.md#geo-redundant-storage) eller [geo-redundant lagring med läsbehörighet (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (rekommenderas), Azure Storage kommer att hålla dina data beständig i två regioner (primär eller sekundär). I båda regioner underhåller Azure Storage ständigt flera kopior av dina data.

När en regional katastrof påverkar din primära region, kommer vi först försök toorestore hello tjänsten i den regionen. Beroende av hello art hello katastrofåterställning och dess påverkan, i vissa sällsynta fall vi får inte vara kan toorestore hello primär region. Vi kommer då att utföra en geo-redundans. hello mellan region-replikering är en asynkron åtgärd som kan medföra en fördröjning, så det är möjligt att ändringar som ännu inte har replikerats toohello sekundär region kan gå förlorade. Du kan fråga hello [”tidpunkt för senaste synkronisering” för ditt lagringskonto](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) tooget information om hello replikeringsstatus.

Ett antal punkter för hello lagring geo-redundans upplevelse:

* Lagring geo-redundans endast utlöses av hello Azure Storage team – kund åtgärd ingen krävs.
* Befintliga lagringstjänsten slutpunkter för blobbar, tabeller, köer och filer förblir hello samma efter hello redundans; hello DNS-posten måste toobe uppdateras tooswitch från hello primär region toohello sekundär region.
* Före och under hello geo-redundans du har inte skrivbehörighet tooyour storage-konto på grund av toohello effekten av hello katastrofåterställning men du fortfarande kan läsa från hello sekundär om ditt lagringskonto har konfigurerats som RA-GRS.
* Läs- och skrivbehörighet tooyour storage-konto kommer att återupptas när hello geo-redundans har slutförts och hello DNS ändringar sprids, Detta pekar toowhat används toobe sekundära slutpunkten. 
* Observera att du har skrivbehörighet om du har GRS eller RA-GRS som konfigurerats för hello storage-konto. 
* Du kan fråga [”Geo redundans senast” för ditt lagringskonto](https://msdn.microsoft.com/library/azure/ee460802.aspx) tooget mer information.
* Efter hello växling, storage-konto kommer att fungera helt och hållet, men status är ”degraderad”, som faktiskt finns i en fristående region med ingen geo-replikering möjligt. toomitigate detta risk, vi återställer hello ursprungliga primär region och sedan gör en geo-återställning toorestore hello ursprungliga tillstånd. Om hello ursprungliga primära region är ett oåterkalleligt allokerar vi en annan sekundär region.
  Mer information om hello infrastrukturen i Azure Storage geo-replikering finns toohello artikel på hello Storage-teamets blogg om [alternativ för redundans och RA-GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

## <a name="best-practices-for-protecting-your-data"></a>Metodtips för att skydda dina data
Det finns några rekommenderade metoder tooback in storage-data regelbundet.

* Virtuella diskar – Använd hello [Azure Backup-tjänsten](https://azure.microsoft.com/services/backup/) tooback in hello Virtuella diskar som används av Azure virtuella datorer.
* Block-blobbar – skapa en [ögonblicksbild](https://msdn.microsoft.com/library/azure/hh488361.aspx) för var och en blockblobb eller kopiera hello blobbar tooanother storage-konto i en annan region med [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), eller hello [ Azure Data Movement library](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* – Använder [AzCopy](storage-use-azcopy.md) tooexport hello tabelldata i ett annat lagringskonto i en annan region.
* Filer – använda [AzCopy](storage-use-azcopy.md) eller [Azure PowerShell](storage-powershell-guide-full.md) toocopy filer tooanother lagringen konto i en annan region.

Information om hur du skapar program som dra full nytta av funktionen hello RA-GRS checka ut [skapar hög tillgängliga program använder RA-GRS-lagring](storage-designing-ha-apps-with-ragrs.md)

