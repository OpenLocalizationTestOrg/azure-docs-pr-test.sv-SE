---
title: aaaMigrating VMs tooAzure Premium-lagring | Microsoft Docs
description: "Migrera dina befintliga virtuella datorer tooAzure Premium-lagring. Premium-lagring ger stöd för I/O-intensiva arbetsbelastningar som körs på Azure Virtual Machines diskar med hög prestanda, låg latens."
services: storage
documentationcenter: na
author: yuemlu
manager: tadb
editor: tysonn
ms.assetid: 272250b3-fd4e-41d2-8e34-fd8cc341ec87
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: yuemlu
ms.openlocfilehash: cd812bdbe39f43fe053a998d96788045d5a43258
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-premium-storage-unmanaged-disks"></a>Migrera tooAzure Premium-lagring (ohanterade diskar)

> [!NOTE]
> Den här artikeln beskrivs hur toomigrate en virtuell dator som använder ohanterade standarddiskar tooa virtuell dator som använder ohanterad premiumdiskar. Vi rekommenderar att du använder Azure hanterade diskar för nya virtuella datorer och att du konverterar tidigare ohanterade diskar toomanaged diskarna. Hanterade diskar-referensen hello underliggande storage-konton så du behöver. Mer information, se vår [översikt för hanterade diskar](storage-managed-disks-overview.md).
>

Azure Premium Storage ger stöd för virtuella datorer som körs I/O-intensiva arbetsbelastningar diskar med hög prestanda, låg latens. Du kan dra nytta av hello hastighet och prestanda för dessa diskar genom att migrera programmets Virtuella diskar tooAzure Premium-lagring.

hello syftet med den här guiden är toohelp nya användare på Azure Premium Storage bättre förbereda toomake en smidig övergång från deras aktuella system tooPremium lagring. hello guiden löser tre hello viktiga komponenter i den här processen:

* [Planera för hello migrering tooPremium lagring](#plan-the-migration-to-premium-storage)
* [Förbereda och kopiera virtuella hårddiskar (VHD) tooPremium lagring](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [Skapa Azure virtuell dator med Premium-lagring](#create-azure-virtual-machine-using-premium-storage)

Du kan migrera virtuella datorer från andra plattformar tooAzure Premium-lagring, eller så kan du migrera befintliga virtuella Azure-datorer från standardlagring tooPremium lagring. Den här guiden beskriver steg för båda två scenarier. Åtgärderna hello hello relevanta avsnitt beroende på ditt scenario.

> [!NOTE]
> Du hittar en översikt över funktioner och priser för Premium-lagring i Premium Storage: [högpresterande lagring för arbetsbelastningar på virtuella Azure](storage-premium-storage.md). Vi rekommenderar att du migrerar alla virtuella diskar som kräver hög IOPS tooAzure Premium-lagring för hello bästa prestanda för ditt program. Om disken inte kräver hög IOPS, kan du begränsa kostnader genom att upprätthålla i standardlagring som lagrar data för virtuell disk på hårddiskar (HDD) i stället för SSD-enheter.
>

Slutför hello migreringsprocessen i sin helhet kan kräva ytterligare åtgärder både före och efter hello stegen i den här guiden. Exempel inkluderar konfigurering av virtuella nätverk eller slutpunkter eller ändrar koden i hello själva programmet som kan kräva vissa avbrott i ditt program. Dessa åtgärder är unika tooeach program och du bör utföra dem tillsammans med hello stegen i den här guiden toomake hello fullständig övergång tooPremium lagring som sömlös som möjligt.

## <a name="plan-the-migration-to-premium-storage"></a>Planera för hello migrering tooPremium lagring
Det här avsnittet säkerställer att du är klar toofollow hello migreringssteg i den här artikeln och hjälper dig att toomake hello bästa beslut på VM- och diskresurser typer.

### <a name="prerequisites"></a>Krav
* Du behöver en Azure-prenumeration. Om du inte har någon, kan du skapa en månad [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/) prenumeration eller besök [priser för Azure](https://azure.microsoft.com/pricing/) fler alternativ.
* tooexecute PowerShell-cmdlets måste hello Microsoft Azure PowerShell-modulen. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för hello installera installationsplatser och installationsanvisningar.
* När du planerar toouse Azure virtuella datorer som körs på Premium-lagring måste toouse hello Premium-lagring kan virtuella datorer. Du kan använda Standard- och Premium-lagring diskar med Premium-lagring kan virtuella datorer. Premium lagringsdiskar kan användas med flera VM-typer i hello framtida. Läs mer på alla tillgängliga Virtuella Azure-disktyper och storlekar, [storlekar för virtuella datorer](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och [storlekar för molntjänster](../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Överväganden
En Azure VM stöder bifoga flera Premium-lagring diskar så att dina program kan ha upp too256 TB lagringsutrymme per virtuell dator. Med Premium-lagring kan dina program uppnå (i/o-åtgärder per sekund) för 80 000 IOPS per virtuell dator och 2000 MB per andra diskgenomflödet per virtuell dator med mycket låg latens för läsåtgärder. Har du alternativ på en mängd olika virtuella datorer och diskar. Det här avsnittet är toohelp toofind ett alternativ som bäst passar din arbetsbelastning.

#### <a name="vm-sizes"></a>VM-storlekar
hello Azure VM storlek specifikationer listas i [storlekar för virtuella datorer](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Granska hello prestandaegenskaper för virtuella datorer som fungerar med Premium-lagring och välj hello lämpligaste VM-storlek som bäst passar din arbetsbelastning. Kontrollera att det finns tillräckligt med bandbredd på VM toodrive hello disk trafiken.

#### <a name="disk-sizes"></a>Diskstorlekar
Det finns fem typer av diskar som kan användas med den virtuella datorn och var och en har särskilda IOPs och genomströmning gränser. Ta hänsyn till dessa gränser när välja hello disktyp för den virtuella datorn baserat på hello behoven för ditt program vad gäller kapacitet, prestanda, skalbarhet och belastning läses in.

| Premium diskar typ  | P10   | P20   | P30            | P40            | P50            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| Diskstorlek           | 128 GB| 512 GB| 1 024 GB (1 TB) | 2 048 GB (2 TB) | 4095 GB (4 TB) | 
| IOPS per disk       | 500   | 2 300  | 5000           | 7500           | 7500           | 
| Dataflöde per disk | 100 MB per sekund | 150 MB per sekund | 200 MB per sekund | 250 MB per sekund | 250 MB per sekund |

Beroende på din arbetsbelastning avgör du om ytterligare hårddiskar krävs för den virtuella datorn. Du kan bifoga flera beständiga data diskar tooyour VM. Om det behövs kan stripe du över hello diskar tooincrease hello kapacitet och prestanda för hello volym. (Se vad som är Disk Striping [här](storage-premium-storage-performance.md#disk-striping).) Om du stripe-Premium-lagring datadiskar med hjälp av [lagringsutrymmen][4], bör du konfigurera den med en kolumn för varje disk som används. Annars vara hello övergripande prestanda för hello stripe-volym lägre än väntat på grund av toouneven fördelning av trafik över hello diskar. För virtuella Linux-datorer kan du använda hello *mdadm* verktyget tooachieve hello samma. Se artikeln [konfigurera programvara RAID på Linux](../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mer information.

#### <a name="storage-account-scalability-targets"></a>Skalbarhetsmål för lagringskontot
Premium-lagringskonton har hello följande skalbarhetsmål i tillägg toohello [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md). Om kraven för application överskrider hello skalbarhetsmål för ett enda storage-konto, och skapa ditt program toouse flera lagringskonton och partitionera data mellan dessa lagringskonton.

| Total kapacitet | Totala bandbredden för ett lokalt Redundant Lagringskonto |
|:--- |:--- |
| Disken kapacitet: 35TB<br />Ögonblicksbild kapacitet: 10 TB |Konfigurera too50 Gigabit per sekund för inkommande och utgående |

För mer information om specifikationer för Premium-lagring som Hej, kolla [skalbarhets- och prestandamål när du använder Premiumlagring](storage-premium-storage.md#scalability-and-performance-targets).

#### <a name="disk-caching-policy"></a>Princip för cachelagring av disk
Princip för cachelagring av disk är som standard *skrivskyddad* för alla hello Premium datadiskar och *Read-Write* för hello Premium operativsystemdisken kopplade toohello VM. Den här konfigurationen rekommenderas tooachieve hello optimala prestanda för ditt program IOs. Inaktivera cachelagring på disk så att du kan få bättre prestanda för programmet för skrivintensiv eller lässkyddad datadiskar (till exempel loggfiler för SQL Server). inställningar för cachelagring av hello för befintliga datadiskar kan uppdateras med hjälp av [Azure-portalen](https://portal.azure.com) eller hello *- HostCaching* parametern för hello *Set AzureDataDisk* cmdlet.

#### <a name="location"></a>Plats
Välj en plats där Azure Premium-lagring är tillgängliga. Se [Azure-tjänster efter Region](https://azure.microsoft.com/regions/#services) uppdaterad information om tillgängliga platser. Virtuella datorer finns i hello samma region som hello Storage-konto som lagrar hello diskar för hello VM ger mycket bättre prestanda än om de finns i olika områden.

#### <a name="other-azure-vm-configuration-settings"></a>Andra Virtuella Azure-konfigurationsinställningar
När du skapar en Azure VM, blir du ombedd tooconfigure vissa VM-inställningar. Kom ihåg att några inställningar som är fasta under hello livstid hello VM, medan du kan ändra eller lägga till andra senare. Granska dessa Virtuella Azure-konfigurationsinställningar och se till att de är korrekt konfigurerad toomatch dina behov av arbetsbelastning.

### <a name="optimization"></a>Optimering
[Azure Premium Storage: Utforma för högprestanda](storage-premium-storage-performance.md) innehåller riktlinjer för att skapa program med höga prestanda med Azure Premium-lagring. Du kan följa hello riktlinjer kombineras med prestanda bästa praxis tillämpliga tootechnologies används av ditt program.

## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Förbereda och kopiera virtuella hårddiskar (VHD) tooPremium lagring
hello efter avsnittet innehåller riktlinjer för att förbereda virtuella hårddiskar från den virtuella datorn och kopiera virtuella hårddiskar tooAzure lagring.

* [Scenario 1: ”jag migrera befintliga virtuella Azure-datorer tooAzure Premium-lagring”.](#scenario1)
* [Scenario 2: ”jag migrera virtuella datorer från andra plattformar tooAzure Premium-lagring”.](#scenario2)

### <a name="prerequisites"></a>Krav
tooprepare hello virtuella hårddiskar för migrering, behöver du:

* En Azure-prenumeration, ett lagringskonto och en behållare i lagringsenheterna kontot toowhich kan du kopiera den virtuella Hårddisken. Observera att hello destinationslagringskontot kan vara ett Standard eller Premium-lagring konto utifrån dina behov.
* En verktyget toogeneralize hello VHD om du planerar toocreate flera VM-instanser från den. Till exempel sysprep för Windows eller Radnr i virtuell sysprep för Ubuntu.
* En verktyget tooupload hello VHD-filen toohello Storage-konto. Se [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md) eller Använd en [Azure Lagringsutforskaren](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Den här guiden beskriver kopiera den virtuella Hårddisken med hello AzCopy-verktyget.

> [!NOTE]
> Om du väljer synkron kopiera alternativet med AzCopy, för optimala prestanda, kopiera den virtuella Hårddisken genom att köra något av dessa verktyg från en Azure VM i hello samma region som hello mål-lagringskontot. Om du kopierar en VHD från en Azure-dator i en annan region, gå din långsammare.
>
> Överväg att kopiera stora mängder data över begränsad bandbredd, [med hello Azure Import/Export service tootransfer data tooBlob lagring](storage-import-export-service.md); detta kan du tootransfer dina data av leverans hårddisk enheter tooan Azure Datacenter. Du kan använda hello Azure Import/Export service toocopy data tooa standardlagringskonto endast. När hello data finns i ditt lagringskonto som standard, kan du använda antingen hello [kopiera Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) eller AzCopy tootransfer hello data tooyour premium storage-konto.
>
> Observera att Microsoft Azure bara stöder fast storlek VHD-filer. VHDX-filer eller dynamiska virtuella hårddiskar stöds inte. Om du har en dynamisk virtuell Hårddisk kan du konvertera den toofixed storlek med hello [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet.
>
>

### <a name="scenario1"></a>Scenario 1: ”jag migrera befintliga virtuella Azure-datorer tooAzure Premium-lagring”.
Om du migrerar befintliga virtuella Azure-datorer stoppa hello VM, Förbered virtuella hårddiskar per hello typ av virtuell Hårddisk som du vill och sedan kopiera hello VHD med AzCopy eller PowerShell.

hello VM måste toobe helt ned toomigrate ett rent tillstånd. Det är inte ett driftstopp förrän hello migreringen är klar.

#### <a name="step-1-prepare-vhds-for-migration"></a>Steg 1. Förbereda virtuella hårddiskar för migrering
Om du migrerar befintliga virtuella datorer i Azure tooPremium lagring kan vara den virtuella Hårddisken:

* En generaliserad operativsystemsavbildning
* En unik operativsystemdisk
* En datadisk

Nedan går vi igenom dessa 3 scenarier för att förbereda den virtuella Hårddisken.

##### <a name="use-a-generalized-operating-system-vhd-toocreate-multiple-vm-instances"></a>Använd en generaliserad virtuell Hårddisk för operativsystemet toocreate flera VM-instanser
Om du överför en virtuell Hårddisk som ska använda toocreate flera generiska Azure VM-instanser måste du först generalisera VHD med sysprep-verktyget. Detta gäller tooa VHD som är lokalt eller i hello molnet. Sysprep tar bort alla datorspecifik information hello VHD.

> [!IMPORTANT]
> Ta en ögonblicksbild eller säkerhetskopiera den virtuella datorn innan generalisera den. Kör sysprep avbryts och frigöra hello VM-instans. Följ stegen nedan toosysprep en Windows OS-VHD. Observera att köra hello Sysprep kommando kräver att du tooshut av hello virtuella datorn. Mer information om Sysprep finns [Sysprep översikt](http://technet.microsoft.com/library/hh825209.aspx) eller [Teknisk referens för Sysprep](http://technet.microsoft.com/library/cc766049.aspx).
>
>

1. Öppna ett kommandotolksfönster som administratör.
2. Ange följande kommando tooopen Sysprep hello:

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. Välj ange System Out of Box Experience (OOBE), Välj hello Generalize kryssrutan Markera i hello systemförberedelseverktyget, **avstängning**, och klicka sedan på **OK**som visas i hello bilden nedan. Sysprep generalize hello operativsystem och Stäng hello system.

    ![][1]

Använd Radnr i virtuell sysprep tooachieve för en Ubuntu VM hello samma. Se [Radnr i virtuell sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) för mer information. Se även några av hello öppen källkod [etablering av Linux-servrar programvara](http://www.cyberciti.biz/tips/server-provisioning-software.html) för andra Linux-operativsystem.

##### <a name="use-a-unique-operating-system-vhd-toocreate-a-single-vm-instance"></a>Använda en unik operativsystemet VHD toocreate en enda VM-instans
Om du har ett program som körs på hello VM som kräver hello datorn specifika data kan du inte generalisera hello VHD. En ej generaliserad virtuell Hårddisk kan vara används toocreate ett unikt Virtuella Azure-instans. Till exempel om du har en domänkontrollant på den virtuella Hårddisken kan gör köra sysprep det ineffektiv som en domänkontrollant. Granska hello-program som körs på din virtuella datorn och hello effekten av att köra sysprep på dem. innan generalisera hello VHD.

##### <a name="register-data-disk-vhd"></a>Registrera datadisk VHD
Om du har migrerat datadiskar i Azure toobe, måste du se att hello VMs som använder dessa data diskar är avstängd.

Följ hello stegen som beskrivs nedan toocopy VHD tooAzure Premium-lagring och registrera den som en etablerade datadisk.

#### <a name="step-2-create-hello-destination-for-your-vhd"></a>Steg 2. Skapa hello mål för den virtuella Hårddisken
Skapa ett lagringskonto för att underhålla de virtuella hårddiskarna. Överväg följande punkter när du planerar var hello toostore de virtuella hårddiskarna:

* hello mål Premium storage-konto.
* hello lagringskontoplatsen måste vara samma som Premium-lagring kan virtuella Azure-datorer skapas i hello sista steget. Du kan kopiera tooa nytt lagringskonto eller plan toouse hello samma lagringskonto baserat på dina behov.
* Kopiera och spara hello lagringskontonyckel av hello destinationslagringskontot för hello nästa steg.

Du kan välja tookeep vissa datadiskar i ett standardlagringskonto (till exempel diskar med kyligare lagring) för datadiskar, men rekommenderar vi du flytta alla data för produktion arbetsbelastning toouse premium-lagring.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Steg 3. Kopiera VHD med AzCopy eller PowerShell
Du behöver toofind din sökvägen och lagring konto viktiga tooprocess något av dessa två alternativ. Behållaren sökvägen och lagringsutrymmet kontonyckel finns i **Azure Portal** > **lagring**. hello behållarens Webbadress kommer att som ”https://myaccount.blob.core.windows.net/mycontainer/”.

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Alternativ 1: Kopiera en virtuell Hårddisk med AzCopy (asynkron kopia)
Med AzCopy kan överföra du enkelt hello VHD över hello Internet. Detta kan ta tid beroende på hello stor hello virtuella hårddiskar. Kom ihåg ingång-/ utgång lagringskontogränser för toocheck hello när du använder det här alternativet. Se [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md) mer information.

1. Hämta och installera AzCopy härifrån: [senaste versionen av AzCopy](http://aka.ms/downloadazcopy)
2. Öppna Azure PowerShell och gå toohello mappen där AzCopy är installerat.
3. Använd hello följande kommando toocopy hello VHD-filen från ”källa” för ”mål”.

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Exempel:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    Nedan följer beskrivningar av hello parametrar som används i hello AzCopy-kommandot:

   * **/ Källa:  *&lt;källa&gt;:***  platsen för hello mapp eller lagring behållarens Webbadress som innehåller hello VHD.
   * **/ SourceKey:  *&lt;källa kontonyckel&gt;:***  lagringskontonyckel av hello källa storage-konto.
   * **/ Dest:  *&lt;mål&gt;:***  lagring behållaren URL toocopy hello VHD till.
   * **/ DestKey:  *&lt;dest kontonyckel&gt;:***  lagringskontonyckel för hello mål-lagringskontot.
   * **/ Mönster:  *&lt;filnamn&gt;:***  ange hello filnamn hello VHD toocopy.

Mer information om hur du använder AzCopy verktyget, se [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Alternativ 2: Kopiera en VHD med PowerShell (Synchronized kopia)
Du kan också kopiera hello VHD-fil med hello PowerShell-cmdleten Start-AzureStorageBlobCopy. Använd följande kommando på Azure PowerShell toocopy VHD hello. Ersätt hello värden i <> med motsvarande värden från din käll- och storage-konto. Det här kommandot måste du ha en behållare som kallas virtuella hårddiskar i din destinationslagringskontot toouse. Om hello-behållaren inte finns skapa en innan du kör hello-kommando.

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

Exempel:

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <a name="scenario2"></a>Scenario 2: ”jag migrera virtuella datorer från andra plattformar tooAzure Premium-lagring”.
Om du migrerar VHD från icke - Azure Molnlagring tooAzure, måste du först exportera hello VHD tooa lokal katalog. Har hello fullständig källsökvägen för hello lokal katalog där VHD lagras praktiska, och sedan använder AzCopy tooupload den tooAzure lagring.

#### <a name="step-1-export-vhd-tooa-local-directory"></a>Steg 1. Exportera VHD tooa lokal katalog
##### <a name="copy-a-vhd-from-aws"></a>Kopiera en VHD från AWS
1. Om du använder AWS exportera hello EC2 instans tooa VHD i en Amazon S3-bucket. Följ hello stegen som beskrivs i hello Amazon-dokumentationen för export av instanser för Amazon EC2 tooinstall hello Amazon EC2 kommandoradsgränssnittet (CLI) verktyget och kör hello skapa-instans-export-kommandot aktivitet tooexport hello EC2 instans tooa VHD-filen. Vara säker på att toouse **VHD** för hello DISK &#95; BILDEN & #95. FORMATET variabeln när du kör hello **skapa-instans-export-aktivitet** kommando. hello sparas exporterade VHD-filen i hello Amazon S3-bucket som du anger under den här processen.

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. Hämta hello VHD-filen från hello S3-bucket. Välj hello VHD-filen, sedan **åtgärder** > **hämta**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Kopiera en VHD från andra Azure-moln
Om du migrerar VHD från icke - Azure Molnlagring tooAzure, måste du först exportera hello VHD tooa lokal katalog. Kopiera hello fullständig källsökvägen för hello lokal katalog där VHD lagras.

##### <a name="copy-a-vhd-from-on-premises"></a>Kopiera en VHD från lokala
Om du migrerar VHD från en lokal miljö, behöver hello fullständig sökväg där VHD lagras. hello källsökväg kan vara en server eller en filresurs.

#### <a name="step-2-create-hello-destination-for-your-vhd"></a>Steg 2. Skapa hello mål för den virtuella Hårddisken
Skapa ett lagringskonto för att underhålla de virtuella hårddiskarna. Överväg följande punkter när du planerar var hello toostore de virtuella hårddiskarna:

* hello mål-lagringskontot kan vara standard eller premium storage beroende på din programkrav.
* Hej lagringskontots region måste vara samma som Premium-lagring kan virtuella Azure-datorer skapas i hello sista steget. Du kan kopiera tooa nytt lagringskonto eller plan toouse hello samma lagringskonto baserat på dina behov.
* Kopiera och spara hello lagringskontonyckel av hello destinationslagringskontot för hello nästa steg.

Vi rekommenderar starkt att du flyttar alla data för produktion arbetsbelastning toouse premium-lagring.

#### <a name="step-3-upload-hello-vhd-tooazure-storage"></a>Steg 3. Överför hello VHD tooAzure lagring
Nu när du har den virtuella Hårddisken i hello lokal katalog, kan du använda AzCopy eller AzurePowerShell tooupload hello VHD-filen tooAzure lagring. Båda alternativen finns här:

##### <a name="option-1-using-azure-powershell-add-azurevhd-tooupload-hello-vhd-file"></a>Alternativ 1: Använda Azure PowerShell Add-AzureVhd tooupload hello VHD-filen

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

Ett exempel <Uri> kanske ***”https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd”***. Ett exempel <FileInfo> kanske ***”C:\path\to\upload.vhd”***.

##### <a name="option-2-using-azcopy-tooupload-hello-vhd-file"></a>Alternativ 2: Använda AzCopy tooupload hello VHD-filen
Med AzCopy kan överföra du enkelt hello VHD över hello Internet. Detta kan ta tid beroende på hello stor hello virtuella hårddiskar. Kom ihåg ingång-/ utgång lagringskontogränser för toocheck hello när du använder det här alternativet. Se [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md) mer information.

1. Hämta och installera AzCopy härifrån: [senaste versionen av AzCopy](http://aka.ms/downloadazcopy)
2. Öppna Azure PowerShell och gå toohello mappen där AzCopy är installerat.
3. Använd hello följande kommando toocopy hello VHD-filen från ”källa” för ”mål”.

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Exempel:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    Nedan följer beskrivningar av hello parametrar som används i hello AzCopy-kommandot:

   * **/ Källa:  *&lt;källa&gt;:***  platsen för hello mapp eller lagring behållarens Webbadress som innehåller hello VHD.
   * **/ SourceKey:  *&lt;källa kontonyckel&gt;:***  lagringskontonyckel av hello källa storage-konto.
   * **/ Dest:  *&lt;mål&gt;:***  lagring behållaren URL toocopy hello VHD till.
   * **/ DestKey:  *&lt;dest kontonyckel&gt;:***  lagringskontonyckel för hello mål-lagringskontot.
   * **/ BlobType: sidan:** anger att hello-målet är en sidblobb.
   * **/ Mönster:  *&lt;filnamn&gt;:***  ange hello filnamn hello VHD toocopy.

Mer information om hur du använder AzCopy verktyget, se [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Andra alternativ för att överföra en virtuell Hårddisk
Du kan också ladda upp en VHD tooyour storage-konto med en hello följande sätt:

* [Azure Storage kopiera Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [Azure Storage Explorer överför Blobbar](https://azurestorageexplorer.codeplex.com/)
* [Storage Import/Export Service REST API-referens](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> Du rekommenderas att använda Import/Export Service om överföring tid är längre än 7 dagar. Du kan använda [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) tooestimate hello tiden från enhet för storlek och överföring av data.
>
> Import/Export kan vara används tooa toocopy standardlagringskonto. Du behöver toocopy från standardlagring toopremium storage-konto med ett verktyg som AzCopy.
>
>

## <a name="create-azure-virtual-machine-using-premium-storage"></a>Skapa virtuella Azure-datorer med Premium-lagring
När hello VHD är överförda eller kopierade toohello önskad lagringskonto, följ hello instruktionerna i det här avsnittet tooregister hello VHD som en operativsystemsavbildning eller OS-disken beroende på ditt scenario och sedan skapa en VM-instans från den. Hej datadisk VHD kan vara anslutna toohello VM när den har skapats.
En migrering exempelskript tillhandahålls hello slutet av det här avsnittet. Det här enkla skriptet matchar inte alla scenarier. Du kan behöva tooupdate hello skriptet toomatch med din situation. toosee om det här skriptet gäller tooyour scenariot nedan finns [ett exempelskript för migrering](#a-sample-migration-script).

### <a name="checklist"></a>Checklista
1. Vänta tills alla hello VHD-diskar kopiera har slutförts.
2. Kontrollera att Premium-lagring finns i hello region du migrerar till.
3. Bestäm hello ny virtuell dator-serie du kommer att använda. Det bör vara en Premium-lagring som är kompatibla och hello storlek bör vara beroende på hello tillgänglighet i hello region och beroende på dina behov.
4. Bestäm hello exakt VM-storlek du ska använda. VM-storlek måste toobe tillräckligt stor toosupport hello antalet datadiskar som du har. T.ex. Om du har 4 datadiskar har hello VM 2 eller flera kärnor. Du kan också processorkraft, minne och nätverksbandbredd måste.
5. Skapa ett Premium Storage-konto i hello målregionen. Det här är hello-konto som ska användas för hello ny virtuell dator.
6. Ha hello aktuella VM information praktisk, inklusive hello lista över diskar och motsvarande VHD-blobbar.

Förbered ditt program för driftstopp. toodo en ren migrering, har du toostop all hello bearbetning i hello nuvarande systemet. Först då kan du få tooconsistent tillstånd som du kan migrera nya toohello-plattformen. Varaktighet för stilleståndstid beror på hello mängden data i hello diskar toomigrate.

> [!NOTE]
> Om du skapar en Azure Resource Manager-VM från en särskild VHD-Disk, se för[mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) för distribution av Resource Manager VM med hjälp av den befintliga disken.
>
>

### <a name="register-your-vhd"></a>Registrera den virtuella Hårddisken
toocreate en virtuell dator från OS VHD- eller tooattach tooa en data-disk ny virtuell dator, måste du först registrera den. Följ instruktionerna nedan scenario för den virtuella Hårddisken.

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a>Generaliserad virtuell Hårddisk för operativsystemet toocreate flera Virtuella Azure-instanser
När generaliserad OS-avbildningen VHD har överförts toohello storage-konto, kan du registrera den som en **Azure VM-avbildning** så att du kan skapa en eller flera VM-instanser av den. Använd följande PowerShell-cmdlets tooregister hello den virtuella Hårddisken som en Azure VM OS-bild. Ange URL för hello fullständig behållare där VHD kopierades till.

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

Kopiera och spara hello namnet på den här nya Azure VM-avbildning. Hello-exemplet ovan, är det *OSImageName*.

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a>Unik operativsystemet VHD toocreate en Azure VM-instans
När hello unika OS VHD är överförda toohello lagring konto måste du registrera den som en **Azure OS-disken** så att du kan skapa en VM-instans av den. Använd dessa PowerShell-cmdlets tooregister den virtuella Hårddisken som en Azure OS-Disk. Ange URL för hello fullständig behållare där VHD kopierades till.

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

Kopiera och spara hello namnet på den här nya Azure OS-disken. Hello-exemplet ovan, är det *OSDisk*.

#### <a name="data-disk-vhd-toobe-attached-toonew-azure-vm-instances"></a>Data disk VHD toobe kopplade toonew Azure VM-instanser
När hello datadisk VHD har överförts toostorage konto, registrera den som en Azure-datadisk så att det kan vara anslutna tooyour nya DS-serien, DSv2-serien eller GS-serien Azure VM-instans.

Använd följande PowerShell-cmdlets tooregister den virtuella Hårddisken som en Azure-datadisk. Ange URL för hello fullständig behållare där VHD kopierades till.

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

Kopiera och spara hello namnet på den här nya Azure-datadisk. Hello-exemplet ovan, är det *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Skapa en Premium-lagring kan virtuell dator
En gång hello OS-avbildningen eller OS-disken registreras, skapa en ny DS-serien, DSv2-serien eller GS-serien VM. Du kommer att använda hello operativsystemavbildning eller operativsystemet diskens namn som du har registrerat. Välj hello VM typen hello Premium lagringsnivå. I exemplet nedan använder hello *Standard_DS2* VM-storlek.

> [!NOTE]
> Uppdatera hello disk storlek toomake att den matchar din kapacitet och prestandakrav och hello Azure ledigt storlekar.
>
>

Följ hello steg för steg PowerShell-cmdlet nedan toocreate hello ny virtuell dator. Ange först hello gemensamma parametrar:

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

Först skapa en molnbaserad tjänst där du ska vara värd för din nya virtuella datorer.

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

Skapa sedan hello Azure VM-instans från hello OS-avbildningen eller OS-Disk som du har registrerat beroende på ditt scenario.

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a>Generaliserad virtuell Hårddisk för operativsystemet toocreate flera Virtuella Azure-instanser
Skapa hello en eller flera nya DS-serien Azure VM-instanser som använder hello **Azure OS-avbildningen** som har registrerats. Ange operativsystemsavbildning namnet hello VM-konfiguration när du skapar ny virtuell dator som visas nedan.

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a>Unik operativsystemet VHD toocreate en Azure VM-instans
Skapa en ny DS-serien Virtuella Azure-instans med hello **Azure OS-disken** som har registrerats. Ange den här OS-disknamnet hello VM-konfiguration när hello skapar ny virtuell dator som visas nedan.

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

Ange andra Virtuella Azure-information, till exempel en molntjänst, region, storage-konto, tillgänglighetsuppsättning och cachelagringsprincipen. Observera att hello VM-instans måste samordnas med associerade operativsystem eller hårddiskar, så hello valt cloud service, region och lagring konto måste vara i hello samma plats som hello underliggande virtuella hårddiskar för dessa diskar.

### <a name="attach-data-disk"></a>Anslut en datadisk
Slutligen, om du har registrerat data disk virtuella hårddiskar, bifoga dem toohello nya Premium-lagring kan Azure VM.

Använd följande PowerShell-cmdlet tooattach data disk toohello ny virtuell dator och ange hello princip för cachelagring. I exemplet nedan hello cachelagring princip har angetts för*ReadOnly*.

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> Det kan finnas ytterligare steg krävs toosupport ditt program som inte omfattas av den här guiden.
>
>

### <a name="checking-and-plan-backup"></a>Kontrollera och planera säkerhetskopiering
En gång hello nya virtuella datorn är igång och körs, åtkomst till den med hjälp av hello samma inloggnings-id och lösenord som hello ursprungliga virtuella datorn och kontrollera att allt fungerar som förväntat. Alla hello-inställningar, inklusive hello stripe-volymer, finnas i hello ny virtuell dator.

hello sista steget är tooplan schema för säkerhetskopiering och underhåll för hello ny virtuell dator baserat på hello programbehov.

### <a name="a-sample-migration-script"></a>Ett exempelskript för migrering
Om du har flera virtuella datorer toomigrate vara automation via PowerShell-skript användbara. Följande är ett exempelskript som automatiserar hello migreringen av en virtuell dator. Observera som skriptet nedan är bara ett exempel och det finns några antaganden om hello aktuella Virtuella diskar. Du kan behöva tooupdate hello skriptet toomatch med din situation.

hello antaganden är:

* Du skapar klassiska virtuella Azure-datorer.
* Källan OS diskar och källa Datadiskar finns i samma lagringskonto och samma behållare. Om det inte i OS-diskar och Datadiskar hello samma placera, du kan använda AzCopy eller Azure PowerShell toocopy virtuella hårddiskar över storage-konton och behållare. Se toohello föregående steg: [kopiera VHD med AzCopy eller PowerShell](#copy-vhd-with-azcopy-or-powershell). Redigera det här skriptet toomeet ditt scenario är ett annat alternativ, men vi rekommenderar att du använder AzCopy eller PowerShell eftersom det är enklare och snabbare.

Hej automatiseringsskriptet finns nedan. Ersätt texten med din information och uppdaterar hello skriptet toomatch med din situation.

> [!NOTE]
> Med hjälp av hello befintliga skript bevaras inte hello nätverkskonfigurationen för ditt Virtuella källdatorn. Du behöver toore-config hello nätverksinställningar på de migrerade virtuella datorerna.
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE tooshow how toomigrate a VM from a standard storage account tooa premium storage account. You can customize it according tooyour specific requirements.

    .Description
    hello script will copy hello vhds (page blobs) of hello source VM toohello new storage account.
    And then it will create a new VM from these copied vhds based on hello inputs that you specified for hello new VM.
    You can modify hello script toosatisfy your specific requirement, but please be aware of hello items specified
    in hello Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED toohello IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. hello ENTIRE RISK OF USE, INABILITY tooUSE, OR
    RESULTS FROM hello USE OF THIS CODE REMAINS WITH hello USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    toofind more information about how tooset up Azure PowerShell, refer toohello following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # hello cloud service name of hello VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # hello VM name toocopy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # hello destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # hello destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # hello destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # hello destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # hello location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not toocopy hello os disk, hello default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently tooreport hello copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # hello name suffix tooadd toonew created disks tooavoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import hello Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check hello Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer toothis article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ tooconnect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if hello VM is shut down
    #  Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - hello source VM doesn't exist in hello current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation. Azure does not support live migration at this time. If you'd like toocreate a VM from a generalized image, sys-prep hello Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish toostop $SourceVMName now? Input 'N' if you want tooshut down hello VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until hello VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName tooshut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting hello sourve vm tooa configuration file, you can restore hello original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration too$vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy hello vhds of hello source vm
    #  You can choose toocopy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly toobe $false. hello default is toocopy only data disk vhds
    #  and hello new VM will boot from hello original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering hello data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in hello destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need toocopy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting hello source VM so that dest VM can boot
        # from hello same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose toocopy data disks only. Moving VM requires removing hello original VM (hello disks and backing vhd files will NOT be deleted) so that hello new VM can boot from hello same vhd. This is an irreversible action. Do you wish tooproceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing hello original VM (hello vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill hello OS disk is released by source VM. This may take up tooseveral minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy hello os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update hello media link toopoint toohello target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy toocomplete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  hello new VM can be created from hello copied disks or hello original os disk.
    #  You can ddd your own logic here toosatisfy your specific requirements of hello vm.
    #######################################################################

    # Create a VM from hello existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached hello copied data disks toohello new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of hello source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want tooadd more custimization toohello new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <a name="optimization"></a>Optimering
Din aktuella VM-konfiguration kan anpassas specifikt toowork bra med standarddiskar. Till exempel tooincrease hello prestanda med hjälp av många diskar i en stripe-volym. Till exempel i stället för att använda 4 diskar separat på Premium-lagring, du kan toooptimize hello kostnader genom att ha en enda disk. Optimeringar som den här behovet toobe hanteras på ett fall till fall och kräver anpassade åtgärder efter migreringen hello. Observera också att den här processen och inte fungerar för databaser och program som beror på hello disklayouten som definierats i hello-installationen.

##### <a name="preparation"></a>Förberedelse
1. Fullständig hello enkla migrering, enligt beskrivningen i hello tidigare avsnitt. Optimeringar kommer att utföras på hello ny virtuell dator efter hello migreringen.
2. Definiera hello nya diskstorlekar krävs för att konfigurera hello optimerad.
3. Fastställa mappningen av hello aktuella diskar/volymer toohello nya disk specifikationer.

##### <a name="execution-steps"></a>Utförande
1. Skapa hello nya diskar med hello rätt storlek på hello Premium Storage VM.
2. Inloggningen toohello virtuella datorn och kopiera hello data från hello aktuella volymen toohello ny disk som mappar toothat volym. Gör detta för alla aktuella hello-volymer som behöver toomap tooa ny disk.
3. Sedan ändra hello programmet inställningar tooswitch toohello nya diskar och koppla från hello gamla volymer.

Justera hello program för bättre diskprestanda finns för[optimera programprestanda](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Migrering av programmet
Databaser och andra komplexa program kan kräva särskilda åtgärder som definierats av hello programmets provider för hello migrering. Se dokumentationen toorespective. T.ex. vanligtvis databaser kan migreras med säkerhetskopiering och återställning.

## <a name="next-steps"></a>Nästa steg
Se hello följande resurser för specifika scenarier för att migrera virtuella datorer:

* [Migrera Azure virtuella datorer mellan Storage-konton](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Skapa och ladda upp en Windows Server VHD-tooAzure.](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Migrering av virtuella datorer från Amazon AWS tooMicrosoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Se även hello följande resurser toolearn mer om Azure Storage- och virtuella datorer i Azure:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Virtuella Azure-datorer](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Premium Storage: Lagring med höga prestanda för Azure Virtual Machines-arbetsbelastningar](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
