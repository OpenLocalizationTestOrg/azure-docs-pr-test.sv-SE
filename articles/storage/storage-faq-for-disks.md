---
title: "Vanliga frågor (FAQ) om Azure IaaS-VM-diskarna | Microsoft Docs"
description: "Vanliga frågor och svar om Azure IaaS-VM och premiumdiskar (hanterade och ohanterade)"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 31d0aa67b6ca58b75b432ae94f93ebcf6d730380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a>Vanliga frågor och svar om Azure IaaS-VM och hanterade och ohanterade premiumdiskar

Den här artikeln besvarar några vanliga frågor om Azure hanterade diskar och Azure Premium-lagring.

## <a name="managed-disks"></a>Managed Disks

**Vad är Azure hanterade diskar?**

Hanterade diskar är en funktion som förenklar Diskhantering för virtuella Azure IaaS-datorer genom att hantera lagringshantering konto för dig. Mer information finns i hello [översikt för hanterade diskar](storage-managed-disks-overview.md).

**Om jag skapa en standard hanterade diskar från en befintlig virtuell Hårddisk som är 80 GB, hur mycket som kostar mig?**

En standard hanterade diskar som skapats från en virtuell Hårddisk på 80 GB behandlas som hello nästa tillgängliga standard diskutrymme, vilket är en S10 disk. Du debiteras är bl.a toohello S10 disk priser. Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/storage).

**Finns det några transaktionskostnader för hanterade standarddiskar?**

Ja. Du är debiteras för varje transaktion. Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/storage).

**För en standard hanterade disk jag debiteras för hello verkliga storleken hos hello data på hello eller hello diskens hello etablerad kapacitet?**

Du är debiteras baserat på hello diskens hello etablerad kapacitet. Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/storage).

**Hur är prissättningen av hanterade premiumdiskar skiljer sig från ohanterade diskar?**

hello priser för premiumdiskar hanteras är hello samma som ohanterad premiumdiskar.

**Kan jag ändra hello lagringskontotypen (Standard eller Premium) av min hanterade diskar?**

Ja. Du kan ändra hello lagringskontotypen hanterade diskar med hjälp av hello Azure-portalen, PowerShell eller hello Azure CLI.

**Finns det ett sätt att jag kan kopiera eller exportera ett hanterade diskar tooa privata storage-konto?**

Ja. Du kan exportera dina hanterade diskar med hjälp av hello Azure-portalen, PowerShell eller hello Azure CLI.

**Kan jag använda en VHD-fil i ett Azure storage-konto toocreate hanterade diskar med en annan prenumeration?**

Nej.

**Kan jag använda en VHD-fil i ett Azure storage-konto toocreate hanterade diskar i en annan region?**

Nej.

**Finns det några begränsningar för skalan för kunder som använder hanterade diskar?**

Hanterade diskar eliminerar hello gränser som är kopplade till storage-konton. Hello antalet hanterade diskar per prenumeration är dock begränsad too2, 000 som standard. Du kan anropa stöd tooincrease numret.

**Kan jag göra en inkrementell ögonblicksbild av hanterade diskar?**

Nej. hello gör aktuella ögonblicksbild en fullständig kopia av hanterade diskar. Men planerar vi toosupport inkrementell ögonblicksbilder i hello framtida.

**Virtuella datorer i en tillgänglighetsuppsättning kan bestå av en kombination av hanterade och ohanterade diskar?**

Nej. hello virtuella datorer i en tillgänglighetsuppsättning måste använda alla hanterade diskar eller ohanterad diskarna. När du skapar en tillgänglighetsuppsättning, kan du välja vilken typ av diskar du vill använda toouse.

**Är standardalternativet för hanterade diskar hello hello Azure-portalen?**

För närvarande inte, men det blir hello standard i hello framtida.

**Kan jag skapa en tom disk som hanterade?**

Ja. Du kan skapa en tom disk. Hanterade diskar kan skapas oberoende av en virtuell dator, till exempel utan kopplar den tooa VM.

**Vad är hello stöds feldomänsantalet för en tillgänglighetsuppsättning som använder hanterade diskar?**

Beroende på hello region där hello tillgänglighetsuppsättningen som använder hanterade diskar finns, är hello stöds feldomänsantalet 2 eller 3.

**Hur är hello standard lagringskontot för diagnostik konfigurera?**

Du kan konfigurera ett konto för privat lagring för diagnostik för Virtuella datorer. I framtida hello, planerar vi tooswitch diagnostik tooManaged-diskar.

**Vilken typ av rollbaserad åtkomstkontroll support är tillgänglig för hanterade diskar?**

Hanterade diskar stöder tre viktiga standardroller:

* Ägare: Kan hantera allt, inklusive åtkomst
* Deltagare: Kan hantera allt utom åtkomst
* Läsare: Kan visa allt, men det går inte att göra ändringar

**Finns det ett sätt att jag kan kopiera eller exportera ett hanterade diskar tooa privata storage-konto?**

Du kan hämta en skrivskyddad delad åtkomstsignatur URI för hello hanterade disken och använda den toocopy hello innehållet tooa privat lagring konto eller lokal lagring.

**Kan jag skapa en kopia av min hanterade diskar?**

Kunder kan ta en ögonblicksbild av deras hanterade diskar och sedan använda hello ögonblicksbild toocreate en annan hanterade diskar.

**Ohanterad diskar stöds fortfarande?**

Ja. Vi stöder ohanterade och hanterade diskar. Vi rekommenderar att du använder hanterade diskar för nya arbetsbelastningar och migrera dina aktuella arbetsbelastningar toomanaged diskar.


**Om jag skapa en 128 GB disk och sedan öka hello storlek too130 GB jag debiteras för hello nästa diskstorlek (512 GB)?**

Ja.

**Kan jag skapa lokalt redundant lagring, geo-redundant lagring och zonredundant lagring hanteras diskar?**

Azure-hanterade diskar stöder för närvarande endast lokalt redundant lagring hanterade diskar.

**Kan jag krympa eller downsize min hanterade diskar?**

Nej. Den här funktionen stöds inte för närvarande. 

**Kan jag ändra hello namnegenskapen för datorn när en särskild (inte skapats med hjälp av hello systemförberedelseverktyget eller generaliserad) systemdisken är används tooprovision en virtuell dator?**

Nej. Du kan inte uppdatera hello namnegenskapen för datorn. hello ärver ny virtuell dator den från hello överordnade VM, som har använt toocreate hello operativsystemdisken. 

**Var hittar jag exempel Azure Resource Manager-mallar toocreate virtuella datorer med hanterade diskar?**
* [Lista över mallar med hjälp av hanterade diskar](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

## <a name="managed-disks-and-storage-service-encryption"></a>Hanterade diskar och Storage Service-kryptering 

**Är Azure Storage Service-kryptering aktiverat som standard när jag skapar hanterade diskar?**

Ja.

**Som hanterar hello krypteringsnycklar?**

Microsoft hanterar hello krypteringsnycklar.

**Kan jag inaktivera Storage Service-kryptering för min hanterade diskar?**

Nej.

**Är Lagringstjänstens kryptering endast tillgänglig i vissa områden?**

Nej. Den är tillgänglig i alla hello regioner där hanterade diskar är tillgänglig. Hanterade diskar är tillgänglig i alla offentliga regioner och Tyskland.

**Hur kan jag ta reda om hanterade disken krypteras?**

Du kan ta reda hello tid då en hanterad disk skapades från hello Azure-portalen, hello Azure CLI och PowerShell. Om hello är senare än den 9 juni 2017 krypteras disken. 

**Hur kan jag kryptera Mina befintliga diskar som skapades före 10 juni 2017?**

Från och med 10 juni 2017 krypteras automatiskt nya data skrivs tooexisting hanterade diskar. Vi också planerar tooencrypt befintliga data och hello kryptering sker asynkront i hello bakgrund. Om du måste nu krypterar befintliga data måste du skapa en kopia av disken. Nya diskar krypteras.

* [Kopiera hanterade diskar med hjälp av hello Azure CLI](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)
* [Kopiera hanterade diskar med hjälp av PowerShell](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)

**Är hanterad ögonblicksbilder och bilder som krypteras?**

Ja. Alla hanterade ögonblicksbilder och bilder som skapats efter den 9 juni 2017 krypteras automatiskt. 

**Kan jag konvertera virtuella datorer med ohanterad diskar som finns på lagringskonton som är eller fanns tidigare krypterade toomanaged diskar?**

Ja

**En exporterad VHD från en hanterad disk eller en ögonblicksbild även krypteras?**

Nej. Men om du exportera en VHD-tooan krypterade lagringskonto från en krypterad hanterade diskar eller ögonblicksbild och är krypterad. 

## <a name="premium-disks-managed-and-unmanaged"></a>Premiumdiskar: hanterade och ohanterade

**Om en virtuell dator använder en serie med storlek som har stöd för Premium-lagring, till exempel en DSv2 kan jag koppla premium- och datadiskar som standard?** 

Ja.

**Kan jag koppla premium- och standard diskar tooa storlek dataserie som inte stöder Premium-lagring, till exempel D, Dv2, G eller F serien?**

Nej. Du kan koppla bara standarddata diskar tooVMs som inte använder en serie med storlek som har stöd för Premium-lagring.

**Om jag skapa en disk för premium-data från en befintlig virtuell Hårddisk som är 80 GB, hur mycket som kostar?**

En disk för premium-data som skapas från en virtuell Hårddisk på 80 GB behandlas som hello nästa tillgängliga premium-diskstorleken, vilket är en P10 disk. Du debiteras är bl.a toohello P10 disk priser.

**Transaktionen kostnader toouse Premium-lagring finns?**

Det finns en fast kostnad för varje diskstorlek som hämtas etablerade med specifika gränser på IOPS och genomflöde. hello är övriga kostnader utgående bandbredd och ögonblicksbild kapacitet, om tillämpligt. Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/storage).

**Vad är hello gränser för IOPS och dataflöde som jag kan från hello diskcache?**

Hej kombinerade gränser för cache och lokala SSD för DS-serien är 4 000 IOPS per kärna och 33 MB per sekund per kärna. hello GS-serien ger 5 000 IOPS per kärna och 50 MB per sekund per kärna.

**Är hello lokala SSD stöds för en virtuell för hanterade diskar?**

hello är lokala SSD tillfälligt lagringsutrymme som ingår i en hanterad diskar i virtuell dator. Det finns inget extra kostnad för den här tillfällig lagring. Vi rekommenderar att du inte använder den här lokala SSD toostore dina programdata eftersom det inte finns kvar i Azure Blob storage.

**Finns det några konsekvenser för hello använda trim på premiumdiskar?**

Det finns ingen Nackdelen med toohello användning av TRIMNING på Azure-diskar på antingen premium eller standarddiskar.

## <a name="new-disk-sizes-managed-and-unmanaged"></a>Nya diskstorlekar: hanterade och ohanterade

**Vad är hello största diskstorleken kan användas för operativsystemet och datadiskarna?**

hello partitionstypen som Azure har stöd för en operativsystemdisk är hello master boot record (MBR). hello MBR-formatet stöder en diskstorlek in too2 TB. hello största storlek som Azure har stöd för en operativsystemdisk är 2 TB. Azure stöder upp too4 TB för datadiskar. 

**Vad är hello största blob sidstorlek som stöds?**

hello största blob sidstorlek som har stöd för Azure är 8 TB (8191 GB). Vi stöder inte sidblobbar som är större än 4 TB (4,095 GB) kopplade tooa VM som data eller operativsystemet diskar.

**Behöver jag toouse en ny version av Azure-verktyg toocreate, bifoga, ändra storlek och ladda upp diskar som är större än 1 TB?**

Du behöver inte tooupgrade din befintliga Azure-verktyg toocreate, koppla eller ändra storlek på diskar som är större än 1 TB. tooupload din VHD-filen från lokala direkt tooAzure som en sidblob eller ohanterad disk måste du toouse hello senaste verktyget anger:

|Azure-verktyg      | Versioner som stöds                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | Versionsnumret 4.1.0: juni 2017 släpper eller senare|
|Azure CLI v1     | Versionsnumret 0.10.13: släpp kan 2017 eller senare|
|AzCopy           | Versionsnumret 6.1.0: juni 2017 släpper eller senare|

hello-stöd för Azure CLI v2 och Azure Lagringsutforskaren kommer snart. 

**Stöds P4 och P6 diskstorlekar för ohanterade diskar eller sidblobbar?**

Nej. P4 (32 GB) och P6 diskstorlekar (64 GB) stöds endast för hanterade diskar. Stöd för ohanterade diskar och sidblobbar kommer snart.

**Om min befintliga premium hanteras disk mindre än 64 GB skapades innan hello liten disk har aktiverats (runt den 15 juni 2017), hur den faktureras?**

Befintliga små premiumdiskar som är mindre än 64 GB fortsätta toobe debiteras bl.a toohello P10 prisnivå. 

**Hur kan jag byta hello disk nivå i små premiumdiskar som är mindre än 64 GB från P10 tooP4 eller P6?**

Du kan ta en ögonblicksbild av små diskar och sedan skapa disken tooautomatically växel hello priser nivå tooP4 eller P6 baserat på hello etablerats storlek. 


## <a name="what-if-my-question-isnt-answered-here"></a>Vad gör jag om min fråga inte besvaras här?

Om din fråga inte finns med här kan för oss berätta och vi hjälper dig att hitta ett svar. Du kan ställa en fråga hello slutet av den här artikeln i hello kommentarer. tooengage med hello Azure Storage-teamet och andra gruppmedlemmar om den här artikeln använder hello MSDN [Azure Storage-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).

toorequest funktioner, skicka din begäran och idéer toohello [Azure Storage Feedbackforum](https://feedback.azure.com/forums/217298-storage).
