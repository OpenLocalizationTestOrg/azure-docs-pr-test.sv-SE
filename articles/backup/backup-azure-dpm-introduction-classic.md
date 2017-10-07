---
title: aaaBack in DPM-arbetsbelastningar tooAzure klassiska portal | Microsoft Docs
description: "En introduktion toobacking in DPM-servrar med hello Azure Backup-tjänsten"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
keywords: "System Center Data Protection Manager data protection manager, dpm-säkerhetskopiering"
ms.assetid: 8f23972b-d167-4231-b331-e198db3b18b4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;giridham;markgal
ms.openlocfilehash: f408957db69d45f745d5e89bd97030a341405b72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-tooazure-with-dpm"></a>Förbereda tooback in tooAzure arbetsbelastningar med DPM
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Azure Backup-Server (klassisk)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klassisk)](backup-azure-dpm-introduction-classic.md)
>
>

Den här artikeln innehåller en introduktion toousing Microsoft Azure Backup tooprotect System Center Data Protection Manager (DPM) servrar och arbetsbelastningar. Genom att läsa det, måste du förstå:

* Hur fungerar Azure DPM server-säkerhetskopiering
* hello krav tooachieve en smidig säkerhetskopiering upplevelse
* Hej vanliga fel som inträffar och hur toodeal med dem.
* Scenarier som stöds

System Center DPM säkerhetskopierar filer och programdata. Data som säkerhetskopieras tooDPM kan lagras på band på disk, eller säkerhetskopierade tooAzure med Microsoft Azure Backup. DPM samverkar med Azure Backup på följande sätt:

* **DPM distribueras som en fysisk server eller lokal virtuell dator** – om DPM distribueras som en fysisk server eller en lokal Hyper-V virtuell dator som du kan säkerhetskopiera upp data tooan Azure Backup valvet dessutom toodisk och säkerhetskopiering av band.
* **DPM distribueras som en virtuell Azure-dator** – från System Center 2012 R2 med uppdatering 3 kan DPM distribueras som en virtuell Azure-dator. Om DPM distribueras som en virtuell Azure-dator kan du säkerhetskopiera data tooAzure diskar kopplade toohello DPM Azure-datorn eller du omfördela datalagring hello genom att säkerhetskopiera tooan Azure Backup-valvet.

## <a name="why-backup-your-dpm-servers"></a>Varför säkerhetskopiera DPM-servrar?
hello fördelarna med att använda Azure Backup för att säkerhetskopiera DPM-servrar är:

* Du kan använda Azure-säkerhetskopiering som ett alternativt toolong sikt distribution tootape för lokal DPM-distribution.
* För DPM-distributioner i Azure kan Azure Backup du toooffload storage från hello Azure-disken så att du tooscale upp genom att lagra äldre data i Azure Backup och nya data på disken.

## <a name="how-does-dpm-server-backup-work"></a>Hur fungerar säkerhetskopiering med DPM-servern?
tooback av en virtuell dator, första en tidpunkt i ögonblicksbild av hello data krävs. hello Azure Backup-tjänsten startar hello säkerhetskopieringsjobb på hello schemalagd tid och utlösare hello reservanknytning tootake en ögonblicksbild. Hej reservanknytning koordinerar hello-gäst VSS tjänsten tooachieve konsekvens och anropar hello blob ögonblicksbild API hello Azure Storage-tjänst när konsekvenskontrollen har uppnåtts. Detta är gjort tooget en programkonsekvent ögonblicksbild av hello diskar för hello virtuell dator utan att behöva tooshut ner den.

Efter hello ögonblicksbild fattats överförs hello data med hello Azure Backup service toohello säkerhetskopieringsvalvet. hello tjänsten hand tar om identifiering och överföra endast hello-block som har ändrats från hello senaste säkerhetskopia gör hello säkerhetskopieringar lagrings- och effektivt. När hello dataöverföring har slutförts hello ögonblicksbild tas bort och skapa en återställningspunkt. Den här återställningspunkten kan ses i hello klassiska Azure-portalen.

> [!NOTE]
> Endast filkonsekventa säkerhetskopiering är möjligt för Linux virtuella datorer.
>
>

## <a name="prerequisites"></a>Krav
Förbered Azure Backup tooback in DPM-data på följande sätt:

1. **Skapa ett säkerhetskopieringsvalv**. Om du inte har skapat ett säkerhetskopieringsvalv i din prenumeration, se hello Azure portal versionen av den här artikeln - [förbereda tooback in tooAzure arbetsbelastningar med DPM](backup-azure-dpm-introduction.md).

  > [!IMPORTANT]
  > Från mars 2017 kan använda du inte längre hello klassiska portal toocreate säkerhetskopieringsvalv.
  > Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017. **Från den 1 november 2017**:
  >- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
  >- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
  >

2. **Ladda ned valvautentiseringsuppgifter** – i Azure Backup överför hello hanteringscertifikat som du skapade toohello valvet.
3. **Installera hello Azure Backup-agenten och registrera hello server** – från Azure Backup, installera hello-agenten på varje DPM-servern och registrera hello DPM-servern i hello säkerhetskopieringsvalvet.

[!INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[!INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]

## <a name="requirements-and-limitations"></a>Krav (och begränsningar)
* DPM kan köras som en fysisk server eller en virtuell dator för Hyper-V som är installerade på System Center 2012 SP1 eller System Center 2012 R2. Det kan också köras som en Azure-dator som körs på System Center 2012 R2 med minst DPM 2012 R2 Samlad uppdatering 3 eller en Windows-dator i VMWare som körs på System Center 2012 R2 med minst Samlad uppdatering 5.
* Om du kör DPM med System Center 2012 SP1 måste installera du Samlad uppdatering 2 för System Center Data Protection Manager SP1. Detta krävs innan du kan installera hello Azure Backup-agenten.
* hello DPM-servern måste ha Windows PowerShell och .net Framework 4.5 installerat.
* DPM kan säkerhetskopiera de flesta arbetsbelastningar tooAzure säkerhetskopiering. Hello Azure Backup för en fullständig lista över vad har stöd för finns stöd för objekten nedan.
* Data som lagras i Azure Backup kan inte återställas med alternativet för hello ”kopiera tootape”.
* Du behöver ett Azure-konto med hello Azure Backup-funktionen aktiverad. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Läs mer om [priser för Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
* Med Azure Backup kräver hello Azure Backup-agenten toobe installerade på hello-servrar som du vill att tooback. Alla servrar måste ha minst 10% av hello storlek hello data som säkerhetskopieras, tillgänglig som lokala ledigt utrymme. Till exempel kräver säkerhetskopiera 100 GB data minst 10 GB ledigt utrymme på hello virtuellt plats. Hello minsta är 10%, rekommenderas 15% ledigt lokal lagring utrymme toobe används för hello cacheplatsen.
* Data lagras i hello Azure valvet lagring. Det finns ingen gräns toohello mängden data som du kan säkerhetskopiera tooan Azure Backup-valv men hello storleken för en datakälla (till exempel en virtuell dator eller en databas) får inte överskrida 54,400 GB.

Dessa filtyper stöds för säkerhetskopiering av tooAzure:

* Krypterade (endast fullständiga säkerhetskopior)
* Komprimerade (inkrementella säkerhetskopior stöds)
* Sparse-filer (inkrementella säkerhetskopior stöds)
* Komprimerade och sparse-filer (hanteras som sparse-filer)

Och de stöds inte:

* Servrar i skiftlägeskänsliga filsystem stöds inte.
* Hårda länkar (ignoreras)
* Referenspunkt (ignoreras)
* Krypterade och komprimerade (ignoreras)
* Krypterade och utspridda (ignoreras)
* Komprimerad ström
* Utspridd ström

> [!NOTE]
> Från i System Center 2012 DPM med SP1 och senare, kan du säkerhetskopiera upp arbetsbelastningar skyddade av DPM tooAzure med hjälp av Microsoft Azure Backup.
>
>
