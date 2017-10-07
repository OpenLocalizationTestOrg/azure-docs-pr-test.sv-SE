---
title: aaaHD-baserade Standard kostnadseffektiv lagring och Azure VM diskar | Microsoft Docs
description: Diskutera kostnadseffektiv standardlagring och ohanterade och hanterade Virtuella diskar.
services: storage
documentationcenter: 
author: yuemlu
manager: aungoo-msft
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: yuemlu
ms.openlocfilehash: c9162eaea50cdd43862378e62dcff9a3d762e092
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cost-effective-standard-storage-and-unmanaged-and-managed-azure-vm-disks"></a>Kostnadseffektiv standardlagring och ohanterade och hanterade Virtuella Azure-diskar

Azure standardlagring ger stöd för tillförlitlig, billig diskar för virtuella datorer som kör latens-okänslig arbetsbelastningar. Det stöder också blobbar, tabeller, köer och filer. Med standardlagring lagras hello data på hårddiskar (HDD). När du arbetar med virtuella datorer kan använda du standardlagring diskar för utveckling och testning scenarier och mindre viktiga arbetsbelastningar och premiumdiskar lagring för kritiska produktionsprogram. Standardlagring är tillgänglig i alla Azure-regioner. 

Den här artikeln fokuserar på hello användningen av standardlagring för Virtuella diskar. Mer information om hello använder lagring med blobbar, tabeller, köer och filer finns toohello [introduktion tooStorage](../storage-introduction.md).

## <a name="disk-types"></a>Disktyper

Det finns två sätt toocreate standarddiskar för virtuella Azure-datorer:

**Ohanterad diskar**: Detta är hello ursprungliga metod där du hanterar hello lagring konton som används toostore hello VHD-filer som motsvarar toohello Virtuella diskar. VHD-filer lagras som sidblobbar i storage-konton. Ohanterad diskar kan vara anslutna tooany Azure VM-storlek, inklusive hello virtuella datorer som i första hand använder Premiumlagring, till exempel hello DSv2 och GS-serien. Virtuella Azure-datorer stöder bifoga flera standarddiskar tillåter upp too256 TB lagringsutrymme per virtuell dator.

[**Azure-hanterade diskar**](../../virtual-machines/windows/managed-disks-overview.md): den här funktionen hanterar hello storage-konton som används för hello Virtuella diskar som du. Du anger hello typ (Premium eller Standard) och storlek på disk och Azure skapar och hanterar hello disk du. Du har inte tooworry om placera hello diskar över flera lagringskonton i ordning tooensure du hålla sig inom hello skalbarhetsbegränsningar för hello lagringskonton--Azure hanterar som du.

Även om båda typer av diskar är tillgängliga, bör du använda hanterade diskar tootake nytta av de många funktionerna.

Besök tooget igång med Azure standardlagring [Kom igång gratis](https://azure.microsoft.com/pricing/free-trial/). 

Mer information om hur toocreate en virtuell dator för hanterade diskar, finns en hello följande artiklar.

* [Skapa en virtuell dator med Resource Manager och PowerShell](/azure/virtual-machines/windows/quick-create-powershell.md)
* [Skapa en Linux VM som använder hello Azure CLI 2.0](../../virtual-machines/windows/quick-create-cli.md)

## <a name="standard-storage-features"></a>Standardlagring funktioner 

Låt oss ta en titt på några av funktionerna hello standardlagring. Mer information, se [introduktion tooAzure lagring](../storage-introduction.md).

**Standardlagring**: Azure standardlagring stöder Azure-diskar, Azure BLOB, Azure File storage, Azure-tabeller och köer i Azure. toouse standardlagring tjänster, startar med [skapa ett Azure Storage-konto](storage-create-storage-account.md#create-a-storage-account).

**Standard lagringsdiskar:** standardlagring diskar kan vara ansluten tooall virtuella Azure-datorer inklusive storlek-serien virtuella datorer som används med Premium-lagring, till exempel hello DSv2 och GS-serien. En standardlagring disk kan bara vara anslutna tooone VM. Men kan du koppla en eller flera av dessa diskar tooa VM, in toohello maximala antal har definierats för denna VM-storlek. I följande avsnitt på Standard skalbarhets- och Storage prestandamål hello, kommer vi beskriver hello specifikationer i detalj. 

**Standard sidblob**: Standard sidblobbar är används toohold beständiga diskar för virtuella datorer och kan också komma åt direkt via REST precis som andra typer av Azure-BLOB. [Sidblobbar](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) är en samling 512 byte-sidor som är optimerade för slumpmässiga Läs- och skrivåtgärder. 

**Storage-replikering:** i de flesta regioner data i ett standardlagringskonto kan vara lokalt replikerade eller georeplikerad i flera datacenter. hello fyra typer av replikering som är tillgängliga är lokalt Redundant lagring (LRS), Zonredundant lagring (ZRS), Geo-Redundant lagring (GRS) och Geo-Redundant lagring med läsbehörighet (RA-GRS). Hanterade diskar i standardlagring stöder för närvarande lokalt Redundant lagring (LRS) bara. Mer information finns [Lagringsreplikering](../storage-redundancy.md).

## <a name="scalability-and-performance-targets"></a>Mål för skalbarhet och prestanda

I det här avsnittet beskriver vi hello skalbarhets- och prestandamål som du behöver tooconsider när med standardlagring.

### <a name="account-limits--does-not-apply-toomanaged-disks"></a>Lagringskontogränser – gäller inte toomanaged diskar

| **Resurs** | **Standardgräns** |
|--------------|-------------------|
| TB per lagringskonto  | 500 TB |
| Max ingång<sup>1</sup> per lagringskonto (oss regioner) | 10 Gbit/s om GRS/ZRS aktiverad 20 Gbit/s för LRS |
| Maximalt antal utgående<sup>1</sup> per lagringskonto (oss regioner) | 20 Gbit/s om RA-GRS/GRS/ZRS aktiverad 30 Gbit/s för LRS |
| Max ingång<sup>1</sup> per lagringskonto (Europeiska och östasiatiska regioner) | 5 Gbit/s om GRS/ZRS aktiverad, 10 Gbit/s för LRS |
| Maximalt antal utgående<sup>1</sup> per lagringskonto (Europeiska och östasiatiska regioner) | 10 Gbit/s om RA-GRS/GRS/ZRS aktiverad 15 Gbit/s för LRS |
| Begära frekvens (förutsatt att 1 KB Objektstorlek) per lagringskonto | Konfigurera too20 000 IOPS, enheter per sekund eller meddelanden per sekund |

<sup>1</sup> ingång refererar tooall data (antal begäranden) som skickas tooa storage-konto. Utgående refererar tooall data (svar) tas emot från ett lagringskonto.

Mer information finns i [Azure Storage skalbarhets- och prestandamål](../storage-scalability-targets.md).

Om hello måste programmet överskrider hello skalbarhetsmål för ett enda lagringskonto, bygga program-toouse flera lagringskonton och partitionera data mellan dessa lagringskonton. Alternativt kan du kan Azure hanterade diskar och Azure ska hantera hello partitionering och placering av dina data för dig.

### <a name="standard-disks-limits"></a>Standarddiskar gränser

Till skillnad från Premiumdiskar etableras hello i/o-åtgärder per sekund (IOPS) och dataflöde (bandbredd) standarddiskar inte. hello prestanda för standarddiskar varierar beroende på hello VM toowhich hello disken kopplas inte toohello storleken på hello disk. Du kan förvänta dig tooachieve in toohello prestanda gränsen som anges i hello tabellen nedan.

**Standarddiskar gränser (hanterade och ohanterade)**

| **VM-nivå**            | **Grundnivån VM** | **Standardnivån VM** |
|------------------------|-------------------|----------------------|
| Max diskens storlek          | 4095 GB           | 4095 GB              |
| Maximalt antal 8 KB IOPS per disk | Konfigurera too300         | Konfigurera too500            |
| Max Bandwidth per disk | Konfigurera too60 MB/s     | Konfigurera too60 MB/s        |

Om din arbetsbelastning kräver stöd för diskar med hög prestanda, låg latens, bör du använda Premium-lagring. tooknow mer fördelarna med Premium-lagring finns [lagring med höga prestanda Premium och Azure VM diskar](../storage-premium-storage.md). 

## <a name="snapshots-and-copy-blob"></a>Ögonblicksbilder och kopiera blob

toohello lagringstjänsten hello VHD-filen är en sidblobb. Du kan ta ögonblicksbilder av sidblobar och kopiera dem tooanother plats, till exempel ett annat lagringskonto.

### <a name="unmanaged-disks"></a>Ohanterade diskar

Du kan skapa [inkrementell ögonblicksbilder](../../virtual-machines/windows/incremental-snapshots.md) för ohanterade standard diskar i hello samma sätt som du använder ögonblicksbilder med standardlagring. Vi rekommenderar att du skapar ögonblicksbilderna och kopierar sedan dessa ögonblicksbilder tooa geo-redundant standardlagringskonto om källdisken är i ett lokalt redundant lagringskonto. Mer information finns i [Azure lagringsalternativ för redundans](../storage-redundancy.md).

Om en disk ansluten tooa VM tillåts inte vissa API-åtgärder på hello diskar. Du kan exempelvis utföra en [kopiera Blob](/rest/api/storageservices/Copy-Blob) åtgärden på blobben så länge hello disken är ansluten tooa VM. I stället först skapa en ögonblicksbild av blobben med hjälp av hello [ögonblicksbild Blob](/rest/api/storageservices/Snapshot-Blob) REST API-metoden och utför sedan hello [kopiera Blob](/rest/api/storageservices/Copy-Blob) av hello ögonblicksbild toocopy hello ansluten disk. Alternativt kan du koppla bort disk hello och utföra alla nödvändiga åtgärder.

toomaintain geo-redundant kopior av dina ögonblicksbilder, du kan kopiera ögonblicksbilder från ett lokalt redundant lagring konto tooa geo-redundant standardlagringskonto med hjälp av AzCopy eller kopiera Blob. Mer information finns i [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md) och [kopiera Blob](/rest/api/storageservices/Copy-Blob).

Detaljerad information om hur du utför REST-åtgärder mot sidblobbar i standard storage-konton finns [Azure Storage Services REST API](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

### <a name="managed-disks"></a>Hanterade diskar

En ögonblicksbild för hanterade diskar är en skrivskyddad kopia av hello hanterade diskar som lagras som en standard hanterade diskar. Inkrementell ögonblicksbilder stöds inte för närvarande för hanterade diskar men stöds i framtida hello.

Om hanterade diskar är anslutna tooa VM, tillåts inte vissa API-åtgärder på hello diskar. Du kan till exempel generera en delad åtkomst (SAS)-signaturen tooperform en kopieringsåtgärd medan hello disken är anslutna tooa VM. I stället först skapa en ögonblicksbild av hello disken och sedan kopiera hello hello ögonblicksbilder. Du kan också koppla bort hello disken och sedan skapa en kopieringsåtgärd för delad åtkomst (SAS)-signaturen tooperform hello.

## <a name="pricing-and-billing"></a>Priser och fakturering

När du använder standardlagring hello följande för debitering:

* Standardlagring ohanterad diskar/datastorlek 
* Hanterade standarddiskar
* Standardlagring ögonblicksbilder
* Utgående dataöverföringar
* Transaktioner

**Ohanterad lagringsstorleken för data och disk:** för ohanterade diskar och andra data (blobbar, tabeller, köer och -filer), debiteras du bara för hello mängden utrymme som du använder. Om du har en virtuell dator vars sidblob har etablerats som 127 GB, men hello VM verkligen är till exempel bara använder 10 GB utrymme du debiteras för 10 GB ledigt utrymme. Vi stöder standardlagring in too8191 GB och ohanterade standarddiskar in too4095 GB. 

**Hanterade diskar:** hanterade diskar debiteras hello allokerade storleken. Om disken har etablerats som en disk på 10 GB och du bara använder 5 GB kan debiteras du fortfarande för hello etablera storlek på 10 GB.

**Ögonblicksbilder**: ögonblicksbilder av standarddiskar debiteras för hello ytterligare kapacitet som används av hello ögonblicksbilder. Mer information om ögonblicksbilder finns [skapa en ögonblicksbild av en Blob](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

**Utgående dataöverföringar**: [utgående dataöverföringar](https://azure.microsoft.com/pricing/details/data-transfers/) (data skickas från Azure-datacenter) debiteras för bandbreddsanvändning.

**Transaktionen**: Azure debiterar $0.0036 per 100 000 transaktioner för standardlagring. Transaktioner omfattar både läs- och skrivaktiviteter operations toostorage.

Detaljerad information om priser för standardlagring för finns virtuella datorer och hanterade diskar:

* [Azure Storage-priser](https://azure.microsoft.com/pricing/details/storage/)
* [Virtuella datorer priser](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Hanterade diskar priser](https://azure.microsoft.com/pricing/details/managed-disks)

## <a name="azure-backup-service-support"></a>Stöd för Azure Backup service 

Virtuella datorer med ohanterad diskar kan säkerhetskopieras med Azure Backup. [Mer information](../../backup/backup-azure-vms-first-look-arm.md).

Du kan också använda hello Azure Backup service med hanterade diskar toocreate en säkerhetskopiering med tidsbaserade säkerhetskopieringar, enkelt VM-återställning och säkerhetskopiering bevarandeprinciper. Du kan läsa mer om detta i [med hjälp av Azure Backup-tjänsten för virtuella datorer med hanterade diskar](../../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="next-steps"></a>Nästa steg

* [Introduktion tooAzure lagring](../storage-introduction.md)

* [Skapa ett lagringskonto](../storage-create-storage-account.md)

* [Översikt över Managed Disks](../../virtual-machines/windows/managed-disks-overview.md)

* [Skapa en virtuell dator med Resource Manager och PowerShell](/azure/virtual-machines/windows/quick-create-powershell.md)

* [Skapa en Linux VM som använder hello Azure CLI 2.0](../../virtual-machines/windows/quick-create-cli.md)
