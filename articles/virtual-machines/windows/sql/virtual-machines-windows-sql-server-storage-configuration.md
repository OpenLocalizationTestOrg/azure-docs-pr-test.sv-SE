---
title: "aaaStorage konfigurationen för SQL Server-datorer | Microsoft Docs"
description: "Det här avsnittet beskrivs hur Azure konfigurerar lagring för virtuella SQL Server-datorer under etableringen (Resource Manager-modellen). Här beskrivs också hur du konfigurerar lagring för din befintliga SQL Server-datorer."
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 169fc765-3269-48fa-83f1-9fe3e4e40947
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: ninarn
ms.openlocfilehash: b50dbd698828780cfc044fa0966e8f4e2f3bb6c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storage-configuration-for-sql-server-vms"></a>Konfiguration för lagring för virtuella SQL Server-datorer
När du konfigurerar en avbildning av virtuell dator för SQL Server i Azure hjälper hello Portal tooautomate lagringskonfigurationen. Detta inkluderar kopplar lagring toohello VM, vilket gör den tillgänglig lagring tooSQL Server och konfigurera den toooptimize för dina specifika krav.

Det här avsnittet beskrivs hur Azure konfigurerar lagring för din SQL Server-datorer både under etableringen och för befintliga virtuella datorer. Den här konfigurationen baseras på hello [prestandarelaterade metodtips](virtual-machines-windows-sql-performance.md) för Azure virtuella datorer som kör SQL Server.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Krav
toouse hello automated konfigurationsinställningar för lagring, den virtuella datorn kräver hello följande egenskaper:

* Etablerade med en [bild av SQL Server-galleriet](virtual-machines-windows-sql-server-iaas-overview.md#option-1-create-a-sql-vm-with-per-minute-licensing).
* Använder hello [Resource Manager-distributionsmodellen](../../../azure-resource-manager/resource-manager-deployment-model.md).
* Använder [Premiumlagring](../../../storage/common/storage-premium-storage.md).

## <a name="new-vms"></a>Nya virtuella datorer
hello följande avsnitt beskrivs hur tooconfigure lagring för nya virtuella datorer för SQL Server.

### <a name="azure-portal"></a>Azure Portal
Etablera en virtuell Azure-dator med en SQL Server-galleriet bild kan du tooautomatically konfigurera hello lagring för den nya virtuella datorn. Du kan ange hello lagringsstorlek och prestandabegränsningarna belastningstyp. hello följande skärmbild visar hello lagring configuration bladet används under SQL VM etablering.

![Konfigurera SQL Server VM lagring under etableringen.](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Baserat på dina val utför Azure hello följande konfigurationsuppgifterna för lagring när du har skapat hello VM:

* Skapar och bifogar premium storage data diskar toohello virtuella datorn.
* Konfigurerar hello data diskar toobe tillgänglig tooSQL Server.
* Konfigurerar hello datadiskar till en lagringsplats för poolen baserat på hello angivna storlek och prestanda (IOPS och genomströmning) krav.
* Associerar hello lagringspoolen med en ny enhet på hello virtuella datorn.
* Optimerar nya enheten baserat på angivna belastningstyp (datalagring, överföringsprocesser eller Allmänt).

Mer information om hur Azure konfigurerar lagringsinställningarna finns hello [lagring konfigurationsavsnittet](#storage-configuration). En fullständig genomgång av hur toocreate SQL Server-VM i hello Azure-portalen, se [hello etablering kursen](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Resursen hantera mallar
Om du använder hello följande Resource Manager-mallar är kopplade två diskar för premium-data som standard med ingen konfigurationen för lagringspooler. Du kan dock anpassa dessa mallar toochange hello antalet premiumdiskar som är anslutna toohello virtuella datorn.

* [Skapa virtuell dator med automatisk säkerhetskopiering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
* [Skapa virtuell dator med inställningen automatisk uppdatering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
* [Skapa virtuell dator med AKV-integreringen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Befintliga virtuella datorer
För befintliga SQL Server virtuella datorer kan ändra du vissa Lagringsinställningar i hello Azure-portalen. Välj den virtuella datorn, gå toohello inställningar och väljer SQL Server-konfigurationsfilen. hello SQL Server-konfigurationsfilen bladet visar hello aktuella lagringskvoten på den virtuella datorn. Alla enheter som finns på den virtuella datorn visas i det här diagrammet. För varje enhet visar hello lagringsutrymme i fyra avsnitt:

* SQL-data
* SQL-logg
* Andra (icke-SQL-lagring)
* Tillgänglig

![Konfigurera lagring för befintliga SQLServer-dator](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

tooconfigure hello lagring tooadd en ny enhet eller utöka en befintlig enhet, klicka hello redigeringslänken ovan hello diagram.

hello konfigurationsalternativ som visas varierar beroende på om du har använt funktionen innan. När du använder för hello första gången måste ange du din lagringskraven för en ny enhet. Om du tidigare har använt den här funktionen toocreate en enhet kan du välja tooextend enhetens lagring.

### <a name="use-for-hello-first-time"></a>Använd för hello första gången
Om det är första gången du använder den här funktionen, kan du ange hello storlek och prestanda lagringsgränser för en ny enhet. Den här upplevelse är liknande toowhat visas vid etablering tid. hello största skillnaden är att du inte får toospecify hello belastningstyp. Den här begränsningen förhindrar störa eventuella befintliga SQL Server-konfigurationer på hello virtuella datorn.

![Konfigurera SQL Server Storage skjutreglage](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure skapar en ny enhet baserat på dina specifikationer. I det här scenariot utför Azure hello följande konfigurationsuppgifterna för lagring:

* Skapar och bifogar premium storage data diskar toohello virtuella datorn.
* Konfigurerar hello data diskar toobe tillgänglig tooSQL Server.
* Konfigurerar hello datadiskar till en lagringsplats för poolen baserat på hello angivna storlek och prestanda (IOPS och genomströmning) krav.
* Associerar hello lagringspoolen med en ny enhet på hello virtuella datorn.

Mer information om hur Azure konfigurerar lagringsinställningarna finns hello [lagring konfigurationsavsnittet](#storage-configuration).

### <a name="add-a-new-drive"></a>Lägg till en ny enhet
Om du redan har konfigurerat lagring på SQL Server-VM öppnar expanderande lagring två nya alternativ. hello första alternativet är tooadd en ny enhet, vilket kan öka hello prestandanivå på den virtuella datorn.

![Lägg till en ny enhet tooa SQL VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

När du lägger till hello enhet, måste du utföra vissa extra manuell konfiguration tooachieve hello prestandaökning.

### <a name="extend-hello-drive"></a>Utöka hello-enhet
hello är annat alternativ för att expandera lagring tooextend hello befintliga enheten. Det här alternativet ökar hello tillgängligt lagringsutrymme för enheten, men det ökar inte prestanda. Du kan inte ändra hello antalet kolumner när hello lagringspoolen har skapats med lagringspooler. hello antalet kolumner avgör hello antal parallella skrivningar, som kan vara stripe över hello datadiskar. Därför kan inte några datadiskar som lagts till öka prestanda. De kan endast ger mer lagringsutrymme för hello data skrivs. Den här begränsningen innebär också att när du utökar hello enhet hello antalet kolumner avgör hello minsta antalet diskar som du kan lägga till. Om du skapar en lagringspool med fyra datadiskar är hello antalet kolumner därför också fyra. När du utökar hello lagring, måste du lägga till minst fyra datadiskar.

![Utöka en enhet för en SQL-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Storage-konfiguration
Det här avsnittet innehåller en referens för hello lagring konfigurationsändringar som Azure utför automatiskt under SQL VM etablering eller konfigurationen i hello Azure-portalen.

* Om du har valt färre än två TBs lagring för den virtuella datorn, Azure inte att skapa en lagringspool.
* Om du har valt minst två TBs lagring för den virtuella datorn, konfigurerar en lagringspool i Azure. hello nästa avsnitt i det här avsnittet beskrivs hello hello konfigurationen för lagringspooler.
* Automatisk alltid lagringskonfiguration använder [premiumlagring](../../../storage/common/storage-premium-storage.md) P30 datadiskar. Därför det finns en 1:1-mappning mellan din valda antalet terabyte och hello antalet diskar kopplade tooyour VM.

Information om priser finns hello [Storage-priser](https://azure.microsoft.com/pricing/details/storage) sida på hello **disklagring** fliken.

### <a name="creation-of-hello-storage-pool"></a>Skapa hello lagringspoolen
Azure använder hello följande inställningar toocreate hello lagringspoolen på SQL Server-datorer.

| Inställning | Värde |
| --- | --- |
| Stripe-storlek |256 KB (datalagring;) 64 KB (transaktionell) |
| Diskstorlekar |1 TB |
| Cache |Läsa |
| Allokeringsstorlek |Storlek på 64 KB NTFS allokeringsenhet |
| Snabbmeddelanden filen initiering |Enabled |
| Låsa sidor i minnet |Enabled |
| Återställning |Enkel återställning (ingen återhämtning) |
| Antal kolumner |Antalet datadiskar<sup>1</sup> |
| TempDB-plats |Lagras på datadiskar<sup>2</sup> |

<sup>1</sup> när hello lagringspoolen har skapats kan du inte ändra hello antalet kolumner i hello lagringspoolen.

<sup>2</sup> inställningen gäller bara toohello första enheten som du skapar med hello lagring-konfigurationen.

## <a name="workload-optimization-settings"></a>Inställningar för optimering av arbetsbelastning
hello följande tabell beskrivs hello tre arbetsbelastning typen alternativen och deras motsvarande optimeringar:

| Arbetsbelastningstyp | Beskrivning | Optimeringar |
| --- | --- | --- |
| **Allmänt** |Standardinställning som stöder de flesta arbetsbelastningar |Ingen |
| **Transaktionell bearbetning** |Optimerar hello lagringen för traditionella OLTP-arbetsbelastningar |Spårningsflagga 1117<br/>Spårningsflagga 1118 |
| **Datalagring** |Optimerar hello lagringen för analys- och rapporteringsarbetsbelastningar |Spårningsflagga 610<br/>Spårningsflagga 1117 |

> [!NOTE]
> Du kan bara ange hello belastningstyp när du etablerar en virtuell SQL-dator genom att markera den i hello lagring konfigurationssteg.
>
>

## <a name="next-steps"></a>Nästa steg
För andra avsnitt relaterade toorunning SQL Server i virtuella Azure-datorer, se [SQL Server på Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
