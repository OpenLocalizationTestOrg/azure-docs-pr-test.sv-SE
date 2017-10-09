---
title: "aaaOverview för Azure Batch för utvecklare | Microsoft Docs"
description: "Lär dig hello funktionerna i hello Batch-tjänsten och dess API: er från en utveckling synvinkel."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 416b95f8-2d7b-4111-8012-679b0f60d204
ms.service: batch
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3da9d82572b28d05d1923166bd08c26725ca3316
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-large-scale-parallel-compute-solutions-with-batch"></a>Utveckla storskaliga parallella beräkningslösningar med Batch

I den här översikten hello kärnkomponenterna i hello Azure Batch-tjänsten, diskuterar vi hello primära tjänstens funktioner och resurser som Batch-utvecklare kan använda toobuild storskaliga parallell compute-lösningar.

Om du utvecklar beräkningar programmet eller tjänsten som utfärdar direkt [REST API] [ batch_rest_api] anrop eller om du använder någon av hello [Batch SDK](batch-apis-tools.md#azure-accounts-for-batch-development), ska du använda många av hello resurser och funktioner som beskrivs i den här artikeln.

> [!TIP]
> En högre nivå introduktion toohello Batch-tjänsten, se [grunderna i Azure Batch](batch-technical-overview.md).
>
>

## <a name="batch-service-workflow"></a>Batch-tjänstens arbetsflöde
hello är följande övergripande arbetsflöde typiska för nästan alla program och tjänster som använder hello Batch-tjänsten för bearbetning av parallell arbetsbelastningar:

1. Överför hello **datafiler** som du vill tooprocess tooan [Azure Storage] [ azure_storage] konto. Batch innehåller inbyggt stöd för att komma åt Azure Blob storage och aktiviteterna kan hämta dessa filer för[datornoder](#compute-node) när hello aktiviteter körs.
2. Överför hello **programfiler** som dina aktiviteter körs. Dessa filer kan vara binärfiler eller skript och deras beroenden och utförs av hello uppgifter i dina jobb. Aktiviteterna kan hämta dessa filer från ditt lagringskonto eller använda hello [programpaket](#application-packages) funktion i Batch för hantering av program och distribution.
3. Skapa en [pool](#pool) med beräkningsnoder. När du skapar en pool kan ange du hello antal compute-noder för hello pool, storlek och hello operativsystem. När varje aktivitet i ditt jobb körs, tilldelas tooexecute på en av noderna hello i din pool.
4. Skapa ett [jobb](#job). Ett jobb hanterar en samling aktiviteter. Du kan associera varje jobb tooa specifika pool där det jobbet aktiviteter körs.
5. Lägg till [uppgifter](#task) toohello jobb. Varje aktivitet körs hello program eller skript som du har överfört tooprocess hello datafiler den hämtas från ditt lagringskonto. Det kan överföra dess utdata tooAzure lagring som varje aktivitet har slutförts.
6. Övervaka jobbförloppet och hämta hello uppgiftens utdata från Azure Storage.

hello följande avsnitt beskrivs dessa och hello andra resurser Batch som gör att ditt distribuerade beräkningar scenario.

> [!NOTE]
> Du behöver en [Batchkontot](#account) toouse hello Batch-tjänsten. De flesta Batch-lösningar använder ett [Azure Storage][azure_storage]-konto för fillagring och filhämtning. Batch stöder för närvarande endast hello **allmänna** lagringskontotypen, enligt beskrivningen i steg 5 i [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) i [om Azure storage-konton](../storage/common/storage-create-storage-account.md).
>
>

## <a name="batch-service-resources"></a>Resurser i Batch-tjänsten
Några av följande resurser - konton, hello compute-noder, pooler, jobb och uppgifter – krävs av alla lösningar som använder hello Batch-tjänsten. Andra, till exempel jobbscheman och programpaket, är användbara men valfria funktioner.

* [Konto](#account)
* [Beräkningsnod](#compute-node)
* [Pool](#pool)
* [Jobb](#job)

  * [Jobbscheman](#scheduled-jobs)
* [Aktivitet](#task)

  * [Startaktivitet](#start-task)
  * [Job Manager-aktivitet](#job-manager-task)
  * [Jobbförberedelse- och jobbpubliceringsaktiviteter](#job-preparation-and-release-tasks)
  * [Aktivitet med flera instanser (MPI)](#multi-instance-tasks)
  * [Aktivitetsberoenden](#task-dependencies)
* [Programpaket](#application-packages)

## <a name="account"></a>Konto
Batch-kontot är ett unikt identifierade entiteten inom hello Batch-tjänsten. All bearbetning är associerad med ett Batch-konto.

Du kan skapa ett Azure Batch-kontot med hello [Azure-portalen](batch-account-create-portal.md) eller programmässigt, såsom med hello [Batch Management .NET-bibliotek](batch-management-dotnet.md). När du skapar hello konto, kan du associera ett Azure storage-konto.

### <a name="pool-allocation-mode"></a>Poolallokeringsläge

När du skapar ett Batch-konto kan du ange hur [pooler](#pool) med beräkningsnoder ska allokeras. Du kan välja tooallocate pooler för compute-noder i en prenumeration som hanteras av Azure Batch eller kan du tilldela dem i din egen prenumeration. Hej *pool allokering läge* bestämmer egenskapen för hello konto där pooler är allokerade. 

toodecide som poolens fördelning läge toouse Tänk som passar bäst för ditt scenario:

* **Batch-tjänsten**: Batch-tjänsten är hello poolen allokering standardläget, pooler allokeras hello bakgrunden i Azure-hanterade prenumerationer. Tänk på dessa huvudpunkter om hello Batch-tjänsten poolen allokering läge:

    - hello Batch-tjänsten poolen allokering läge stöder både molntjänster och virtuella pooler.
    - hello Batch-tjänsten poolen allokering läge stöder både delad nyckelautentisering eller [Azure Active Directory-autentisering](batch-aad-auth.md) (Azure AD). 
    - Du kan använda antingen dedikerade eller låg prioritet compute-noder i pooler allokerade med hello Batch-tjänsten poolen allokering läge.
    - Använd inte hello Batch-tjänsten poolen allokering läge om du planerar toocreate virtuella Azure-datorn pooler från anpassade VM-avbildningar, eller om du planerar toouse ett virtuellt nätverk. Skapa ditt konto med hello användarens prenumeration poolen allokering läge i stället.
    - Virtuella pooler etablerad på ett konto som har skapats med hello Batch-tjänsten poolen allokering läge måste skapas från [Azure virtuella datorer Marketplace] [ vm_marketplace] bilder.

* **Användarens prenumeration**: med hello användarens prenumeration poolen allokering läge tilldelas Batch-pooler i hello Azure-prenumeration där hello kontot skapas. Tänk på dessa huvudpunkter om hello användarens prenumeration poolen allokering läge:
     
    - hello användarens prenumeration poolen allokering läge stöder bara virtuella pooler. Den stöder inte Cloud Services-pooler.
    - toocreate virtuella pooler från anpassade VM-avbildningar eller toouse ett virtuellt nätverk med pooler med virtuella datorer, måste du använda hello användarens prenumeration poolen allokering läge.  
    - Du måste använda [Azure Active Directory-autentisering](batch-aad-auth.md) med pooler som har allokerats i hello användaren prenumerationen. 
    - Du måste ställa in en Azure key vault för Batch-kontot om hello poolen allokering läget är inställt tooUser prenumeration. 
    - Du kan använda endast dedikerade compute-noder i pooler i ett konto som har skapats med hello användarens prenumeration poolen allokering läge. Noder med låg prioritet stöds inte.
    - Virtuella pooler etablerad på ett konto med hello användarens prenumeration poolen allokering läge kan skapas från [Azure virtuella datorer Marketplace] [ vm_marketplace] bilder eller från anpassade bilder som du Ange.

hello följande tabell jämförs hello Batch-tjänsten och användarens prenumeration poolen allokering lägen.

| **Poolallokeringsläge**                 | **Batch-tjänst**                                                                                       | **Användarprenumeration**                                                              |
|-------------------------------------------|---------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| **Pooler allokeras i**               | En Azure-hanterad prenumeration                                                                           | hello användarens prenumeration i vilka hello Batch-kontot har skapats                        |
| **Konfigurationer som stöds**             | <ul><li>Konfiguration av molntjänst</li><li>Konfiguration av virtuell dator (Linux och Windows)</li></ul> | <ul><li>Konfiguration av virtuell dator (Linux och Windows)</li></ul>                |
| **Virtuella datoravbildningar som stöds**                  | <ul><li>Azure Marketplace-avbildningar</li></ul>                                                              | <ul><li>Azure Marketplace-avbildningar</li><li>Anpassade avbildningar</li></ul>                   |
| **Beräkningsnodtyper som stöds**         | <ul><li>Dedikerade noder</li><li>Lågprioriterade virtuella noder</li></ul>                                            | <ul><li>Dedikerade noder</li></ul>                                                  |
| **Autentisering som stöds**             | <ul><li>Delad nyckel</li><li>Azure AD</li></ul>                                                           | <ul><li>Azure AD</li></ul>                                                         |
| **Azure Key Vault krävs**             | Nej                                                                                                      | Ja                                                                                |
| **Kärnkvot**                           | Bestäms av Batch-kärnkvoten                                                                          | Bestäms av prenumerationens kärnkvot                                              |
| **Stöd för Azure Virtual Network (Vnet)** | Pooler som har skapats med hello molnet tjänstkonfiguration                                                      | Pooler som har skapats med hello konfiguration av virtuell dator                               |
| **Vnet-distributionsmodell som stöds**      | Vnet som skapats med klassisk distributionsmodell                                                             | Vnet som skapats med hello klassiska distributionsmodellen eller Azure Resource Manager |

## <a name="azure-storage-account"></a>Azure-lagringskonto

De flesta Batch-lösningar använder Azure Storage för lagring av resursfiler och utdatafiler.  

Batch stöder för närvarande endast hello allmänna lagringskontotypen, enligt beskrivningen i steg 5 i [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) i [om Azure storage-konton](../storage/common/storage-create-storage-account.md). Batch-aktiviteterna (inklusive standardaktiviteter, startaktiviteter, jobbförberedelse- och jobbpubliceringsaktiviteter) måste definiera resursfiler som finns i allmänna lagringskonton.


## <a name="compute-node"></a>Beräkningsnod
En beräkningsnod är en virtuell Azure-dator (VM) eller en molntjänst för virtuell dator som är dedikerad tooprocessing en del av programmets arbetsbelastning. hello storleken på en nod avgör hello antalet CPU-kärnor, minneskapacitet och lokala system filstorlek som har allokerats toohello nod. Du kan skapa pooler för Windows- eller Linux-noder med hjälp av Azure Cloud Services, bilder från hello [Azure virtuella datorer Marketplace][vm_marketplace], eller anpassade avbildningar som du förbereder. Se följande hello [Pool](#pool) för mer information om dessa alternativ.

Noder kan köra alla körbara filer eller skript som stöds av hello operativsystemmiljön hello noden. inklusive \*.exe-, \*.cmd-, \*.bat- och PowerShell-skript för Windows och binär-, shell- och Python-skript för Linux.

Alla beräkningsnoder i Batch innehåller också:

* En fördefinierad [mappstruktur](#files-and-directories) och associerade [miljövariabler](#environment-settings-for-tasks) som är tillgängliga som referens för aktiviteterna.
* **Brandväggen** inställningar som är konfigurerat toocontrol åtkomst.
* [Fjärråtkomst](#connecting-to-compute-nodes) tooboth (Remote Desktop Protocol (RDP)) för Windows och Linux (Secure Shell (SSH))-noder.

## <a name="pool"></a>Pool
En pool är en samling noder som ditt program körs på. hello pool kan skapas manuellt av dig eller automatiskt av hello Batch-tjänsten när du anger hello arbete toobe är klar. Du kan skapa och hantera en pool som uppfyller hello resurskraven för programmet. En pool kan endast användas av hello Batch-kontot som du skapade. Ett Batch-konto kan ha mer än en pool.

Azure Batch-pooler bygga på hello core Azure compute-plattformen. De ger storskaliga allokering, programinstallation, Datadistribution, övervakning av hälsotillstånd och flexibel justering av hello antal datornoderna inom en pool ([skalning](#scaling-compute-resources)).

Varje nod läggs tooa pool tilldelas ett unikt namn och IP-adress. När en nod tas bort från en pool, ändringar som görs toohello operativsystem eller filer går förlorade och namn och IP-adress släpps för framtida användning. När en nod lämnar poolen är dess livslängd över.

När du skapar en pool, kan du ange hello följande attribut. Vissa inställningar varierar beroende på hello poolen allokering läge hello Batch [konto](#account):

- Beräkningsnodens operativsystem och version
- Typ av beräkningsnod och antal målnoder
- Storleken på hello compute-noder
- Skalningsprincip
- Schemaläggningsprincip för aktiviteter
- Kommunikationsstatus för beräkningsnoder
- Startaktiviteter för beräkningsnoder
- Programpaket
- Nätverkskonfiguration

Dessa inställningar beskrivs mer detaljerat i följande avsnitt hello.

> [!IMPORTANT]
> Batch-konton som skapats med hello Batch-tjänsten poolen allokering läge har en standardkvot som begränsar hello antal kärnor i en Batch-kontot. hello antal kärnor motsvarar toohello antalet compute-noder. Du kan hitta hello standard kvoterna och anvisningar om hur för[öka kvoten för en](batch-quota-limit.md#increase-a-quota) i [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md). Om din pool inte är uppnå dess mål antalet noder, kanske hello kärnkvot hello orsak.
>
>Batch-konton som skapats med hello användarens prenumeration poolen allokering läge Följ inte hello Batch-tjänsten kvoter. I stället de delar i hello kärnkvot för hello angivna prenumerationen. Mer information finns i [Virtual Machines limits](../azure-subscription-service-limits.md#virtual-machines-limits) (Gränser för virtuella datorer) i [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md) (Prenumerations- och tjänstgränser, kvoter och begränsningar i Azure).
>
>

### <a name="compute-node-operating-system-and-version"></a>Beräkningsnodens operativsystem och version

När du skapar en Batch-pool kan ange du hello Azure virtuella datorkonfigurationen och hello typ av operativsystem som du vill använda toorun på varje beräkningsnod i poolen hello. hello två typer av konfigurationer som är tillgängliga i Batch är:

- Hej **konfiguration av virtuell dator**, vilket innebär att hello poolen består av virtuella Azure-datorer. Dessa virtuella datorer kan skapas från Linux- eller Windows-avbildningar. 

    När du skapa en pool som baseras på hello konfiguration av virtuell dator måste du ange inte bara hello storlek hello noder och hello källa för hello bilder används toocreate dem, utan även hello **virtuella bildreferens** och hello Batch **nod agent SKU** toobe har installerats på hello noder. Mer information om hur du anger dessa poolegenskaper finns i [Etablera Linux-beräkningsnoder i Azure Batch-pooler](batch-linux-nodes.md).

- Hej **Cloud Services-konfiguration**, vilket innebär att hello poolen består av Azure Cloud Services noder. Cloud Services tillhandahåller *endast* Windows-beräkningsnoder.

    Tillgängliga operativsystem för pooler Cloud Services-konfiguration visas i hello [Azure gäst-OS-versioner och SDK-kompatibilitetsmatris](../cloud-services/cloud-services-guestos-update-matrix.md). När du skapar en pool som innehåller Cloud Services noder måste toospecify hello nodstorlek och dess *OS-familjen*. Molntjänster är distribuerade tooAzure snabbare än virtuella datorer som kör Windows. Om du vill ha pooler med Windows-beräkningsnoder får du bättre distributionstid med Cloud Services.

    * Hej *OS-familjen* avgör också vilka versioner av .NET installeras med hello OS.
    * Som med arbetsroller i Cloud Services, kan du ange en *OS-Version* (Mer information om arbetsroller finns hello [berätta om molntjänster](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) avsnitt i hello [molntjänster Översikt över](../cloud-services/cloud-services-choose-me.md)).
    * Som med arbetsroller, rekommenderar vi att du anger `*` för hello *OS-Version* så att hello noder uppgraderas automatiskt och det finns inga arbetet som krävs för toocater toonewly publicerat versioner. hello primära användningsfall för att välja en viss OS-version är tooensure programkompatibilitet, vilket gör att bakåtkompatibilitet tester toobe utförs innan hello version toobe uppdateras. När valideringen, hello *OS-Version* för hello pool kan uppdateras och hello ny operativsystemsavbildning kan installeras--alla aktiviteter som körs är avbruten och placerats i kö.

När du skapar en pool måste tooselect hello lämpliga **nodeAgentSkuId**, beroende på hello OS av hello basavbildning för den virtuella Hårddisken. Du kan få en mappning av tillgängliga noden agent SKU-ID: n tootheir OS bildreferenser anropa hello [listan stöds nod Agent SKU: er](https://docs.microsoft.com/rest/api/batchservice/list-supported-node-agent-skus) igen.

Se hello [konto](#account) för information om inställningen hello poolen allokering läge när du skapar ett Batch-konto.

#### <a name="custom-images-for-virtual-machine-pools"></a>Anpassade avbildningar för virtuell datorpooler

toouse skapa en anpassad avbildning tooprovision virtuella pooler Batch-kontot med hello användarens prenumeration poolen allokering läge. Med det här läget allokeras Batch pooler till hello prenumeration där hello kontot finns. Se hello [konto](#account) för information om inställningen hello poolen allokering läge när du skapar ett Batch-konto.

toouse en anpassad avbildning, behöver du tooprepare hello avbildning genom att generalisera den. Information om hur du förbereder anpassade Linux-avbildningar från virtuella Azure-datorer finns i [avbilda en toouse virtuella Azure Linux-datorn som en mall](../virtual-machines/linux/capture-image-nodejs.md). Information om förberedelser som anpassade Windows-avbildningar från virtuella Azure-datorer finns i [Create custom VM images with Azure PowerShell](../virtual-machines/windows/tutorial-custom-images.md) (Skapa anpassade avbildningar av en virtuell dator med Azure PowerShell). 

> [!IMPORTANT]
> När du förbereder den anpassade avbildningen, Kom ihåg hello följande:
> - Se till att hello OS basavbildning du använder tooprovision Batch-pooler har inte har någon förinstallerat Azure tillägg, till exempel hello tillägget för anpassat skript. Om hello bilden innehåller ett tillägg för förinstallerade, stöta Azure på problem som distribuerar hello VM.
> - Se till att hello OS basavbildning du tillhandahålla använder hello temp standardenheten som hello Batch nod agent för närvarande förväntar temp hello-standardenheten.
>
>

toocreate poolen konfiguration av virtuell dator med hjälp av en anpassad avbildning, behöver du en eller flera Azure standardlagring konton toostore din anpassade VHD-avbildningar. Anpassade avbildningar lagras som blobar. tooreference dina anpassade bilder när du skapar en adresspool ange hello URI: er för hello anpassad avbildning VHD-blobbar för hello [osDisk](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_osdisk) -egenskapen för hello [virtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_vmconf) egenskapen.

Kontrollera att dina lagringskonton uppfyller hello följande kriterier:   

- hello storage-konton som innehåller hello anpassad avbildning VHD-blobbar måste toobe i hello samma prenumeration som hello Batch-kontot (hello användaren prenumeration).
- hello angetts storage-konton måste toobe i hello samma region som hello Batch-kontot.
- Endast allmänna standardlagringskonton stöds för närvarande. Azure Premium-lagring stöds i framtida hello.
- Du kan ange ett lagringskonto med flera blobar för anpassade VHD-avbildningar eller flera lagringskonton som vart och ett har en enskild blob. Vi rekommenderar att du toouse flera lagringskonton tooget bättre prestanda.
- En unik anpassad avbildning VHD-blobben stöder too40 Linux VM-instanser eller 20 Windows VM-instanser. Du behöver toocreate kopior av hello VHD blob toocreate pooler med flera virtuella datorer. Till exempel en pool med 200 virtuella Windows-datorer måste 10 unika VHD-blobbar angetts för hello **osDisk** egenskapen.

toocreate poolen från en anpassad avbildning med hjälp av hello Azure-portalen:

1. Navigera tooyour Batch-kontot i hello Azure-portalen.
2. På hello **inställningar** bladet, Välj hello **pooler** menyalternativet.
3. På hello **pooler** bladet, Välj hello **Lägg till** kommandot; hello **Lägg till pool** bladet visas.
4. Välj **anpassad bild (Linux/Windows)** från hello **bildtypen** listrutan. hello portalen visar hello **anpassad bild** väljare. Välj en eller flera virtuella hårddiskar från hello samma behållare och klicka på hello **Välj** knappen. 
    Stöd för flera virtuella hårddiskar från olika lagringskonton och olika behållare läggs i hello framtida.
5. Välj rätt hello **Publisher-erbjudande-Sku** för dina anpassade virtuella hårddiskar, väljer du önskad hello **cachelagring** läge och Fyll i alla hello andra parametrar för hello poolen.
6. toocheck om poolen är baserad på en anpassad avbildning finns hello **operativsystemet** egenskap i sammanfattningen av hello resurs i hello **Pool** bladet. hello-värdet för den här egenskapen ska **anpassad VM-avbildning**.
7. Alla anpassade virtuella hårddiskar som är associerade med en pool visas på hello poolen **egenskaper** bladet.

### <a name="compute-node-type-and-target-number-of-nodes"></a>Typ av beräkningsnod och antal målnoder

När du skapar en pool, kan du ange vilka typer av compute-noder och du vill hello målnumret för varje. hello två typer av beräkningsnoder är:

- **Dedikerade beräkningsnoder.** Dedikerade beräkningsnoder är reserverade för dina arbetsbelastningar. De är dyrare än låg prioritet noder, men de är garanterat toonever avbrytas.

- **Beräkningsnoder med låg prioritet.** Låg prioritet noder utnyttja överflödiga kapacitet i Azure toorun Batch-arbetsbelastningar. Noder med låg prioritet är billigare per timme än dedikerade noder och gör att du kan använda arbetsbelastningar som kräver mycket beräkningskraft. Mer information finns i [Use low-priority VMs with Batch](batch-low-pri-vms.md) (Använda virtuella datorer med låg prioritet med Batch).

    Beräkningsnoder med låg prioritet kan avbrytas om det inte finns tillräckligt med överkapacitet. Om en nod frånkopplas när du kör uppgifter, placerats i kö hello uppgifter och kör igen när en beräkningsnod blir tillgänglig igen. Låg prioritet noder är ett bra alternativ för arbetsbelastningar där hello tid för slutförande av jobbet är flexibel och hello arbete distribueras över flera noder. Innan du väljer toouse låg prioritet noder för ditt scenario, se till att allt arbete förlorade på grund av toopreemption kommer att vara minimal och enkelt toorecreate.

    Låg prioritet compute-noderna är bara tillgängligt för Batch-konton som skapats med hello poolen allokering inställd för**Batch-tjänsten**.

Du kan ha både låg prioritet och dedikerad compute-noder i hello samma pool. Varje typ av noden &mdash; låg prioritet och dedikerad &mdash; har sin egen mål-inställning som du kan ange hello önskad antalet noder. 
    
hello antalet datornoderna är refererad tooas en *mål* eftersom din pool inte kan i vissa situationer kan nå hello önskad antalet noder. Till exempel en pool kanske inte får hello mål om den når hello [kärnkvot](batch-quota-limit.md) för ditt konto först. Eller hello poolen kanske inte får hello mål om du har använt en automatisk skalning formeln toohello pool som begränsar hello maximalt antal noder.

Information om priser för beräkningsnoder med låg prioritet och dedikerade beräkningsnoder finns i [Batch-priser](https://azure.microsoft.com/pricing/details/batch/).

### <a name="size-of-hello-compute-nodes"></a>Storleken på hello compute-noder

Information om storleken på beräkningsnoder med **Cloud Services-konfiguration** finns i [Storlekar för Cloud Services](../cloud-services/cloud-services-sizes-specs.md). Batch stöder alla Cloud Services-storlekar utom `ExtraSmall`, `STANDARD_A1_V2` och `STANDARD_A2_V2`.

Information om storleken på beräkningsnoder med **VM-konfiguration** finns i [Storlekar för virtuella datorer i Azure](../virtual-machines/linux/sizes.md) (Linux) och [Storlekar för virtuella datorer i Azure](../virtual-machines/windows/sizes.md) (Windows). Batch har stöd för alla storlekar för virtuella datorer i Azure utom `STANDARD_A0` och de med Premium Storage (`STANDARD_GS`, `STANDARD_DS`- och `STANDARD_DSV2`-serien).

När du väljer en storlek för compute-nod, Överväg hello egenskaper och krav för hello-program som du kör på hello noder. Aspekter som om programmet hello är flertrådade och hur mycket minne som den förbrukar hjälper dig att bestämma hello mest lämplig och kostnadseffektiv nodstorlek. Det är typiska tooselect en nodstorlek förutsatt att en aktivitet ska köras på en nod i taget. Det är dock möjligt toohave flera uppgifter (och därför flera instanser av programmet) [körs parallellt](batch-parallel-node-tasks.md) på datornoder under jobbkörningen. I det här fallet är det vanliga toochoose större nod storlek tooaccommodate hello ökade behov av parallella aktivitetskörningen. Se [Schemaläggningsprincip](#task-scheduling-policy) för mer information.

Alla hello noder i en pool är hello samma storlek. Om du planerar toorun program med olika systemkrav och/eller läsa in nivåer, rekommenderar vi att du använder separata pooler.

### <a name="scaling-policy"></a>Skalningsprincip

För dynamiska arbetsbelastningar, som du kan skriva och använda en [automatisk skalning formeln](#scaling-compute-resources) tooa pool. hello Batch-tjänsten utvärderar formeln regelbundet och justerar hello antalet noder i hello poolen baserat på olika pool, jobb och uppgiftsparametrar som du kan ange.

### <a name="task-scheduling-policy"></a>Schemaläggningsprincip för aktiviteter

Hej [högsta antal uppgifter per nod](batch-parallel-node-tasks.md) konfigurationsalternativet anger hello högsta antalet aktiviteter som körs parallellt på varje beräkningsnod i poolen hello.

hello standardkonfigurationen anger att en aktivitet i taget körs på en nod, men det finns scenarier där det är bra toohave två eller fler uppgifter som körs samtidigt på en nod. Se hello [Exempelscenario](batch-parallel-node-tasks.md#example-scenario) i hello [samtidiga nod uppgifter](batch-parallel-node-tasks.md) artikel toosee hur du kan dra nytta av flera uppgifter per nod.

Du kan också ange en *fyller typen* som anger om Batch sprids hello uppgifter jämnt över alla noder i en pool eller hanteringspaket varje nod med hello maximalt antal uppgifter innan du tilldelar aktiviteter tooanother nod.

### <a name="communication-status-for-compute-nodes"></a>Kommunikationsstatus för beräkningsnoder

I de flesta fall uppgifter oberoende och behöver inte toocommunicate med varandra. Det kan dock finnas program där aktiviteterna måste kommunicera (till exempel i [MPI-scenarier](batch-mpi.md)).

Du kan konfigurera en pool tooallow **dessa kommunikation**, så att noder i en pool kan kommunicera vid körning. När kommunikation mellan noder är aktiverat kan noderna i pooler med Cloud Services-konfiguration kommunicera med varandra på portar som är större än 1100, och pooler med VM-konfiguration begränsar inte trafiken på någon port.

Observera att aktivera dessa kommunikation också påverkar hello placeringen av hello noder i kluster och kan begränsa hello maximala antalet noder i en pool på grund av begränsningar i distributionen. Om ditt program inte kräver kommunikation mellan noder hello Batch-tjänsten kan tilldela ett potentiellt stora antal noder toohello pool från många olika kluster och Datacenter tooenable ökat parallell bearbetning.

### <a name="start-tasks-for-compute-nodes"></a>Startaktiviteter för beräkningsnoder

hello valfria *aktiviteten starta* körs på varje nod som noden ansluter hello poolen och varje gång en nod startas om eller avbildade. hello starta aktiviteten är särskilt användbar för att förbereda compute-noder för hello körningen av uppgifter, t.ex. installation hello-program som aktiviteterna körs på hello compute-noder.

### <a name="application-packages"></a>Programpaket

Du kan ange [programpaket](#application-packages) toodeploy toohello compute-noder i hello pool. Programpaket ger förenklad distribution och versionshantering av hello-program som körs i dina uppgifter. Programpaket som du anger för en pool installeras på varje nod som ansluter till den poolen, samt varje gång en nod startas om och varje gång nodens avbildning återställs.

> [!NOTE]
> Programpaket kan användas för alla Batch-pooler som skapats efter 5 juli 2017. De stöds i Batch-adresspooler skapa mellan 10 mars 2016 och 5 juli 2017 om hello poolen har skapats med en tjänst i molnet. Batch-pooler som skapats tidigare too10 mars 2016 stöder inte programpaket. Mer information om hur du använder programmet paket toodeploy program tooyour Batch noderna, finns [distribuera program toocompute noder med Batch-programpaket](batch-application-packages.md).
>
>

### <a name="network-configuration"></a>Nätverkskonfiguration

Du kan ange hello undernätet för ett Azure [virtuella nätverk (VNet)](../virtual-network/virtual-networks-overview.md) i vilka hello poolen beräkning noder ska skapas. Se hello [Pool nätverkskonfigurationen](#pool-network-configuration) avsnitt för mer information.


## <a name="job"></a>Jobb
Ett jobb är en samling aktiviteter. Den hanterar hur beräkningar utförs av sina uppgifter på hello beräkningsnoder i poolen.

* hello jobb anger hello **pool** i vilka hello arbete är toobe kör. Du kan skapa en ny pool för varje jobb eller använda en pool för många jobb. Du kan skapa en pool för varje jobb som associeras med ett jobbschema eller för alla jobb som associeras med ett jobbschema.
* Du kan ange en valfri **jobbprioritet**. När ett jobb skickas med högre prioritet än jobb som pågår för tillfället infogas hello uppgifter för hello högre prioritet jobbet i hello kö i uppgifter för hello lägre prioritet jobb. Aktiviteter med lägre prioritet som redan körs avbryts inte.
* Du kan använda jobbet **begränsningar** toospecify vissa gränser för dina jobb:

    Du kan ange en **maximala wallclock tid**, så att om ett jobb körs längre än hello maximala wallclock tiden som har angetts, hello jobb och alla dess uppgifter avslutas.

    Batch kan identifiera och försöka köra aktiviteter som misslyckats igen. Du kan ange hello **maximalt antal återförsök för aktiviteten** som en begränsning, inklusive om en aktivitet är *alltid* eller *aldrig* igen. Du försöker en aktivitet innebär det hello har placerats i kö toobe kör igen.
* Klientprogrammet kan lägga till uppgifter tooa jobb eller ange en [manager projektaktivitet](#job-manager-task). En projektaktivitet manager innehåller hello information som är nödvändiga toocreate hello krävs uppgifter för ett jobb, med hello manager projektaktivitet som körs på en av hello compute-noder i hello pool. hello manager projektaktivitet hanteras specifikt efter Batch--den är i kö så snart hello jobbet har skapats och startas om den inte. En projektaktivitet manager är *krävs* för jobb som har skapats med en [jobbschemat](#scheduled-jobs) eftersom den är hello endast sätt toodefine hello aktiviteter innan hello jobbet instansieras.
* Som standard förblir jobb hello aktiv när alla inom hello-jobbet är klart. Du kan ändra det här problemet så att hello jobb automatiskt avslutas när alla aktiviteter i hello-jobbet har slutförts. Ange hello jobbet **onAllTasksComplete** egenskap ([OnAllTasksComplete] [ net_onalltaskscomplete] i Batch .NET) för*terminatejob* tooautomatically Avsluta hello jobbet när alla aktiviteter har slutförts hello status.

    Observera att hello Batch-tjänsten tar hänsyn till ett jobb med *inga* uppgifter toohave alla aktiviteter har slutförts. Därför används det här alternativet oftast med en [Job Manager-aktivitet](#job-manager-task). Om du vill toouse automatisk jobbet avslutas utan en jobbhanteraren, bör du först ställa ett nytt jobb **onAllTasksComplete** egenskapen för*noaction*, ställa in den också*terminatejob* förrän du har lagt till uppgifter toohello jobb.

### <a name="job-priority"></a>Jobbprioritet
Du kan tilldela en prioritet toojobs som du skapar i batchen. hello Batch-tjänsten används hello prioritetsvärdet hello jobbet toodetermine hello ordningen för schemaläggning av jobb i ett konto (detta är inte förväxlas med toobe en [schemalagt jobb](#scheduled-jobs)). hello prioritet värdena variera från-1000 too1000, som hello lägst prioritet och 1000 som hello med-1000 i högsta. tooupdate hello prioriteten för ett jobb, anrop hello [uppdatera hello egenskaper för ett jobb] [ rest_update_job] åtgärden (Batch REST) eller ändra hello [CloudJob.Priority] [ net_cloudjob_priority] egenskapen (Batch .NET).

Inom hello samma konto högre prioritet jobb har schemaläggning företräde över jobb med lägre prioritet. Ett jobb med ett högre prioritetsvärde i ett konto har inte schemaläggningsprioritet över ett annat jobb med ett lägre prioritetsvärde i ett annat konto.

Schemaläggningen av jobb mellan pooler är oberoende av varandra. Mellan olika pooler är det inte säkert att ett jobb med högre prioritet schemaläggs först om dess associerade pool har ont om inaktiva noder. I hello samma pool jobb med hello samma prioritetsnivå har ett lika chans att schemalagda.

### <a name="scheduled-jobs"></a>Schemalagda jobb
[Jobbet scheman] [ rest_job_schedules] aktivera toocreate återkommande jobb i hello Batch-tjänsten. Ett Jobbschema anger när toorun jobb och innefattar hello specifikationer för hello jobb toobe kör. Du kan ange hur länge hello varaktighet hello schedule-- och hello schema är aktivt--när och hur ofta jobb skapas under hello schemalagd tid.

## <a name="task"></a>Aktivitet
En aktivitet är en beräkningsenhet som associeras med ett jobb. Den körs på en nod. Aktiviteter tilldelas tooa nod för körning eller köas förrän en nod blir ledig. Placera helt enkelt körs en aktiviteten en eller flera program eller skript på en beräkning nod tooperform hello arbete du behöver.

När du skapar en aktivitet kan du ange:

* Hej **kommandoraden** för hello-aktivitet. Detta är hello-kommandoraden som kör ditt program eller skript på hello beräkningsnod.

    Det är viktigt toonote som hello kommandoraden faktiskt inte körs under ett gränssnitt. Därför kan det internt dra nytta av shell-funktioner som [miljövariabeln](#environment-settings-for-tasks) expandering (Detta omfattar hello `PATH`). tootake nytta av dessa funktioner, måste du anropa hello shell hello kommandoraden – till exempel genom att starta `cmd.exe` på Windows-noder eller `/bin/sh` på Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Om dina aktiviteter behöver toorun ett program eller skript som inte är i hello nodens `PATH` eller referera miljövariabler, anropa hello shell explicit i hello kommandorad för uppgift.
* **Resursfiler** som innehåller hello data toobe bearbetas. Dessa filer är automatiskt kopierade toohello noden från Blob storage i ett allmänt Azure Storage-konto innan hello aktivitetens kommandoraden körs. Mer information finns i avsnitt hello [startuppgift](#start-task) och [filer och kataloger](#files-and-directories).
* Hej **miljövariabler** som krävs av ditt program. Mer information finns i hello [miljöinställningar för uppgifter](#environment-settings-for-tasks) avsnitt.
* Hej **begränsningar** under vilka hello aktiviteten ska köras. Begränsningar innehåller till exempel hello maximal tid hello aktiviteten tillåts toorun hello maximalt antal gånger som en misslyckad uppgift ska göras och hello maximal tid som filer i arbetskatalogen hello aktiviteten finns kvar.
* **Programpaket** toodeploy toohello beräkningsnod på vilka hello aktiviteten är schemalagda toorun. [Programpaket](#application-packages) förenklad distribution och versionshantering av hello-program som körs i dina uppgifter. Aktivitetsnivå programpaket är särskilt användbar i miljöer med delad pool, där olika jobben körs på en pool och hello poolen tas inte bort när ett jobb har slutförts. Om jobbet har färre uppgifter än noder i hello pool, kan aktivitet programpaket minimera dataöverföring eftersom programmet är distribuerade endast toohello noder som kör aktiviteter.

Dessutom tootasks som du definierar tooperform beräkningar på en nod, hello följa särskilda aktiviteter också tillhandahålls av hello Batch-tjänsten:

* [Startaktivitet](#start-task)
* [Job Manager-aktivitet](#job-manager-task)
* [Jobbförberedelse- och jobbpubliceringsaktiviteter](#job-preparation-and-release-tasks)
* [Aktiviteter med flera instanser (MPI)](#multi-instance-tasks)
* [Aktivitetsberoenden](#task-dependencies)

### <a name="start-task"></a>Startaktivitet
Genom att associera en **aktiviteten starta** med en pool kan du förbereda hello operativa miljö av dess noder. Du kan exempelvis utföra åtgärder som installerar hello-program som körs i dina aktiviteter eller starta bakgrundsprocesser. hello starta aktiviteten körs varje gång en nod startas för så länge den är i hello pool – inklusive när hello nod först läggs till toohello pool och när den startas om eller avbildade.

En primär fördelen hello startuppgift är att den innehåller alla hello information som är nödvändiga tooconfigure en beräkningsnod och installera hello-program som krävs för körning av aktiviteten. Därför är öka hello antalet noder i en pool så enkelt som att ange hello nya målantalet noder. hello startuppgift ger hello Batch hello tjänstinformation som är nödvändiga tooconfigure hello nya noder och förbereda dem för att acceptera uppgifter.

Som med Azure Batch aktiviteter, kan du ange en lista över **resursfiler** i [Azure Storage][azure_storage], dessutom tooa **kommandoraden** toobe köra. hello Batch-tjänsten först kopierar hello resurs filer toohello noden från Azure Storage och kör sedan hello-kommandoraden. För en pool startuppgift innehåller vanligtvis hello fillistan hello uppgiften programmet och dess beroenden.

Hello startuppgift kan också omfatta referens data toobe används av alla aktiviteter som körs på hello compute-nod. Till exempel en startuppgift kommandoraden kan utföra en `robocopy` åtgärden toocopy programfiler (som har angetts som resursfiler och hämtas toohello nod) från hello starta aktivitetens [arbetskatalogen](#files-and-directories) toohello [delade mappen](#files-and-directories), och kör sedan en MSI eller `setup.exe`.

Det är vanligtvis önskvärt för hello Batch-tjänsten toowait för hello starta uppgiften toocomplete innan hello nod redo toobe tilldelade aktiviteter, men du kan konfigurera detta.

Om en start misslyckas på en beräkningsnod, sedan hello hello nod är uppdaterade tooreflect hello fel och hello noden har tilldelats inte några uppgifter. Startuppgiften kan misslyckas om det uppstår problem kopiera dess resursfiler från lagring, eller om hello process som körs av dess kommandoraden returnerar slutkoden som inte är noll.

Om du lägger till eller uppdatera hello startuppgift för en befintlig adresspool måste du starta om compute-noder för hello starta uppgiften toobe tillämpas toohello noder.

>[!NOTE]
> hello total storlek på Startuppgiften måste vara mindre än eller lika med too32768 tecken, inklusive resursfiler och miljövariabler. tooensure att Startuppgiften uppfyller detta krav, du kan använda något av två sätt:
>
> 1. Du kan använda programmet paket toodistribute program eller data över varje nod i Batch-pool. Mer information om programpaket finns [distribuera program toocompute noder med Batch-programpaket](batch-application-packages.md).
> 2. Du kan manuellt skapa ett zippat arkiv som innehåller dina programfiler. Ladda upp den komprimerade Arkiv tooAzure lagring som en blob. Ange hello zippade Arkiv som en resurs för start-aktivitet. Packa upp hello Arkiv från kommandoraden hello innan du kör hello kommandorad för startuppgift. 
>
>    Du kan använda hello arkivering verktyg som helst toounzip hello Arkiv. Behöver du att du använder toounzip hello Arkiv som en resurs för hello startuppgift tooinclude hello-verktyget.
>
>

### <a name="job-manager-task"></a>Job Manager-aktivitet
Normalt använder du en **manager projektaktivitet** toocontrol och/eller övervaka jobbet körning – till exempel toocreate och skicka hello uppgifter för ett jobb, fastställa ytterligare aktiviteter toorun och avgöra när arbetet är klar. Men är en projektaktivitet manager inte begränsade toothese aktiviteter. Det är en helt fledged aktivitet som kan utföra alla åtgärder som krävs för hello jobb. En projektaktivitet manager kan till exempel en fil som har angetts som en parameter, analysera hello innehållet i filen och skicka ytterligare aktiviteter baserat på innehållet.

En Job Manager-aktivitet startar innan alla andra aktiviteter. Det innehåller hello följande funktioner:

* Det skickas automatiskt som en aktivitet av hello Batch-tjänsten när hello jobb skapas.
* Den är schemalagd tooexecute innan hello andra aktiviteter i ett projekt.
* Dess associerade nod är hello senaste toobe tas bort från en pool när hello poolen är att downsized.
* Upphör att gälla kan vara bundet toohello avslutning av alla aktiviteter i hello-jobbet.
* En projektaktivitet manager ges hello högsta prioritet när det behövs toobe startas om. Om en inaktiv nod inte är tillgänglig hello Batch-tjänsten kan säga upp en hello andra aktiviteter som körs i hello poolen toomake utrymme för hello job manager uppgiften toorun.
* En projektaktivitet manager i ett jobb har inte prioritet över hello uppgifter i andra jobb. Mellan jobb gäller endast prioriteringar på jobbnivå.

### <a name="job-preparation-and-release-tasks"></a>Jobbförberedelse- och jobbpubliceringsaktiviteter
Batch innehåller jobbförberedelseaktiviteter för konfiguration av jobb före körningen. Jobbpubliceringsaktiviteter är avsedda för underhåll och rensning när jobbet har körts.

* **Förberedelse av projektaktivitet**: en aktivitet för förberedelse av jobbet körs på alla compute-noder som är schemalagda toorun uppgifter, innan någon av hello andra jobbuppgifter körs. Du kan använda en förberedelse uppgiften toocopy jobbdata som delas av alla aktiviteter, men är unika toohello jobb, t.ex.
* **Versionen för projektaktivitet**: när ett jobb har slutförts, en projektaktivitet versionen körs på varje nod i hello-pool som körts minst en aktivitet. Du kan använda en jobbet viktig uppgift toodelete data som kopieras av hello projektaktivitet förberedelse eller toocompress och överför loggen diagnostikdata, till exempel.

Både jobb förberedelse och versionen uppgifter gör att du toospecify en kommandorad toorun när hello aktivitet anropas. De stöder funktioner som filhämtning, upphöjd körning, anpassade miljövariabler, högsta körningstid, antal omförsök och kvarhållningstid för filer.

Mer information om jobbförberedelse- och jobbpubliceringsaktiviteter finns i [Köra jobbförberedelse- och jobbpubliceringsaktiviteter på Azure Batch-beräkningsnoder](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Aktivitet med flera instanser
En [flera instanser uppgiften](batch-mpi.md) är en aktivitet som är konfigurerade toorun på mer än en beräkningsnod samtidigt. Du kan aktivera hög prestanda med flera instanser aktiviteter datorer scenarier som kräver en grupp av compute-noder som är allokerade tillsammans tooprocess en enda arbetsbelastning (t.ex. Message Passing Interface (MPI)).

Detaljerad information om hur du kör MPI-jobb i Batch med hjälp av hello Batch .NET-biblioteket, kolla [Använd flera instanser av åtgärder toorun Message Passing Interface (MPI) program i Azure Batch](batch-mpi.md).

### <a name="task-dependencies"></a>Aktivitetsberoenden
[Uppgift beroenden](batch-task-dependencies.md)som hello namnet antyder, kan du toospecify som en aktivitet är beroende av hello andra aktiviteter innan körningen har slutförts. Den här funktionen ger stöd för situationer där ”underordnade” går åt hello utdata för en ”överordnad” aktivitet-- eller när en överordnad aktivitet utför vissa initiering som krävs av en underordnad aktivitet. toouse den här funktionen måste du först aktivera aktivitet beroenden i Batch-jobbet. Sedan för varje aktivitet som är beroende av en annan (eller många andra) kan du ange hello uppgifter som är den aktiviteten som är beroende av.

Du kan konfigurera scenarier som hello nedan med uppgiften beroenden:

* *aktivitetB* är beroende av *aktivitetA* (*aktivitetB* kan inte köra förrän *aktivitetA* har slutförts).
* *aktivitetC* är beroende av både *aktivitetA* och *aktivitetB*.
* *aktivitetD* är beroende av en serie aktiviteter, till exempel aktivitet *1* till och med *10*, innan den kan köras.

Checka ut [uppgift beroenden i Azure Batch](batch-task-dependencies.md) och hello [TaskDependencies] [ github_sample_taskdeps] kodexempel i hello [azure-batch-samples] [ github_samples] GitHub-lagringsplatsen mer detaljerad information om den här funktionen.

## <a name="environment-settings-for-tasks"></a>Miljöinställningar för aktiviteter
Varje aktivitet som körs av hello Batch-tjänsten har åtkomst tooenvironment variabler som anges på compute-noder. Detta inkluderar miljövariabler som definieras av hello Batch-tjänsten ([tjänst definierade][msdn_env_vars]) och anpassade miljövariabler som du kan definiera för dina uppgifter. hello program och skript som kör aktiviteterna har miljövariabler för åtkomst toothese under körning.

Du kan ange anpassade miljövariabler på hello aktivitets- eller nivå genom att fylla hello *miljöinställningar* -egenskapen för dessa enheter. Se exempelvis hello [lägga till ett jobb för aktiviteten tooa] [ rest_add_task] åtgärden (Batch REST API) eller hello [CloudTask.EnvironmentSettings] [ net_cloudtask_env] och [ CloudJob.CommonEnvironmentSettings] [ net_job_env] egenskaper i Batch .NET.

Klientprogrammet eller tjänsten kan erhålla en uppgift miljövariabler, både service definierade och anpassade, med hjälp av hello [hämta information om en aktivitet] [ rest_get_task_info] åtgärden (Batch REST) eller genom att öppna Hej [CloudTask.EnvironmentSettings] [ net_cloudtask_env] egenskapen (Batch .NET). Processer som körs på en beräkningsnod kan komma åt dessa och andra miljövariabler på hello nod, till exempel med hjälp av hello bekant `%VARIABLE_NAME%` (Windows) eller `$VARIABLE_NAME` (Linux) syntax.

Du hittar en fullständig lista över alla definierade miljövariabler i [Beräkna nodmiljövariabler][msdn_env_vars].

## <a name="files-and-directories"></a>Filer och kataloger
Varje aktivitet har en *arbetskatalog* där den skapar inga eller flera filer och kataloger. Den här arbetskatalogen kan användas för att lagra hello-program som körs av hello aktivitet, hello data som den behandlar och hello utgående hello-bearbetning utförs. Alla filer och kataloger i en aktivitet ägs av hello uppgiften användare.

hello Batch-tjänsten Exponerar en del av hello filsystemet på en nod som hello *rotkatalog*. Uppgifter kan komma åt hello rotkatalogen genom att referera hello `AZ_BATCH_NODE_ROOT_DIR` miljövariabeln. Mer information om hur du använder miljövariabler finns i [Miljöinställningar för aktiviteter](#environment-settings-for-tasks).

hello rotkatalogen innehåller följande katalogstruktur hello:

![Katalogstrukturen på beräkningsnoder][1]

* **delad**: den här katalogen ger läs-/ skrivbehörighet för*alla* aktiviteter som körs på en nod. Alla aktiviteter som körs på hello kan skapa, läsa, uppdatera och ta bort filer i den här katalogen. Uppgifter kan komma åt den här katalogen genom att referera hello `AZ_BATCH_NODE_SHARED_DIR` miljövariabeln.
* **startup**: Den här katalogen används av en startaktivitet som dess arbetskatalog. Alla hello-filer som är hämtade toohello noden hello startuppgift lagras här. hello startuppgift kan skapa, läsa, uppdatera och ta bort filer under den här katalogen. Uppgifter kan komma åt den här katalogen genom att referera hello `AZ_BATCH_NODE_STARTUP_DIR` miljövariabeln.
* **Uppgifter**: en katalog skapas för varje aktivitet som körs på hello-nod. Den kan nås genom att referera hello `AZ_BATCH_TASK_DIR` miljövariabeln.

    I varje uppgift katalog hello Batch-tjänsten skapar en arbetskatalog (`wd`) vars unik sökväg har angetts av hello `AZ_BATCH_TASK_WORKING_DIR` miljövariabeln. Den här katalogen innehåller läsning och skrivning åtkomst toohello aktivitet. hello aktivitet kan skapa, läsa, uppdatera och ta bort filer under den här katalogen. Den här katalogen finns kvar baserat på hello *RetentionTime* villkor som har angetts för hello-aktivitet.

    `stdout.txt`och `stderr.txt`: de här filerna skrivs toohello aktivitetsmapp under hello körning av hello uppgift.

> [!IMPORTANT]
> När en nod tas bort från hello poolen, *alla* av hello filer som lagras på hello noden tas bort.
>
>

## <a name="application-packages"></a>Programpaket
Hej [programpaket](batch-application-packages.md) funktionen tillhandahåller enkel hantering och distribution av program toohello compute-noder i din pooler. Du kan ladda upp och hantera flera versioner av hello program körs med dina aktiviteter, inklusive deras binärfiler och stödfiler. Sedan kan du automatiskt distribuera en eller flera av dessa program toohello compute-noder i din pool.

Du kan ange programpaket på hello poolen och aktivitet. När du anger poolen programpaket är hello program distribuerade tooevery nod i hello pool. När du anger aktivitet programpaket hello program är distribuerade endast toonodes som är schemalagda toorun minst en av hello jobbets uppgifter, precis före hello aktivitetens kommandoraden körs.

Batch handtag hello information om att arbeta med Azure Storage toostore ditt programpaket och distribuerar dem toocompute noder, så kan både din kod och hanteringen förenklas.

toofind checka ut mer information om hello tillämpningsfunktion paketet [distribuera program toocompute noder med Batch-programpaket](batch-application-packages.md).

> [!NOTE]
> Om du lägger till poolen programmet paket tooan *befintliga* poolen, måste du starta om dess compute-noder för programmet hello paket toobe distribueras toohello noder.
>
>

## <a name="pool-and-compute-node-lifetime"></a>Livslängd för pooler och beräkningsnoder
När du utformar din lösning för Azure Batch har toomake ett beslut om hur och när pooler skapas och hur länge compute-noder i dessa programpooler hålls tillgängliga.

För en end i hello spektrumet kan du skapa en pool för varje projekt som du skicka och ta bort hello pool så fort tillhörande uppgifter körning. Detta maximerar användning eftersom hello noderna tilldelas enbart vid behov, och Stäng när de är inaktiv. Det innebär att hello jobbet måste vänta tills hello noder toobe allokerade, är det viktigt toonote som aktiviteter som är schemalagda för körning som noder finns individuellt, allokerat, och hello har starta aktiviteten slutförts. Batch har *inte* vänta tills alla noder i en pool som är tillgängliga innan du tilldelar aktiviteter toohello noder. vilket säkerställer maximal användning av alla tillgängliga noder.

Vid hello andra änden av hello spektrum om med jobb startar omedelbart hello högst prioritet, du kan skapa en pool i förväg och tillgängliggöra noder innan jobb skickas. I det här scenariot uppgifter kan starta direkt, men noder kan sitta inaktiv väntan på dem toobe som tilldelats.

En kombinerad metod, som normalt används för att hantera en varierande men kontinuerlig belastning. Du kan ha en pool för att flera jobb skickas till, men kan skala hello antalet noder uppåt eller nedåt enligt toohello arbetsbelastningen (se [skalning beräkningsresurser](#scaling-compute-resources) i hello efter avsnittet). Detta kan göras reaktivt baserat på den aktuella belastningen eller proaktivt om det går att förutse belastningen.

## <a name="virtual-network-vnet-and-firewall-configuration"></a>Konfiguration av virtuella nätverk (VNet) och brandväggar 

När du etablerar en pool av beräkningsnoder i Azure Batch du kan associera hello-pool med ett undernät för ett Azure [virtuella nätverk (VNet)](../virtual-network/virtual-networks-overview.md). toolearn mer information om hur du skapar ett VNet med undernät, se [skapa ett Azure-nätverk med undernät](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). 

 * hello virtuella nätverk som är associerade med en pool måste vara:

   * I samma Azure hello **region** som hello Azure Batch-kontot.
   * Hej i samma **prenumeration** som hello Azure Batch-kontot.

* Hej beror VNet som stöds på hur pooler allokeras till hello Batch-kontot:

    - Om hello poolen allokering läge för Batch-kontot är inställt tooBatch Service och du kan tilldela ett VNet enbart toopools som skapats med hello **Services Molnkonfigurationen**. Dessutom anges hello VNet måste skapas med hello klassiska distributionsmodellen. Vnet som skapats med hello Azure Resource Manager-distributionsmodellen stöds inte.
 
    - Om hello poolen allokering läge för Batch-kontot är inställt tooUser prenumeration och du kan tilldela ett VNet enbart toopools som skapats med hello **konfiguration av virtuell dator**. Pooler som har skapats med hello **moln tjänstkonfiguration** stöds inte. hello associerade virtuella nätverk kan skapas med hello Azure Resource Manager-distributionsmodellen eller hello klassiska distributionsmodellen.

    En tabell som sammanfattar VNet support enligt toopool allokering läge finns hello [Pool allokering läge](#pool-allocation-mode) avsnitt.

* Om hello poolen allokering läge för Batch-kontot har angetts tooBatch Service, måste du ange behörigheter för hello Batch service principal tooaccess hello virtuella nätverk. Hej VNet måste tilldela hello [Classic Virtual Machine Contributor Role-Based åtkomstkontroll (RBAC)](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/#classic-virtual-machine-contributor) rollen toohello Batch tjänstens huvudnamn. Om du hello RBAC roll har angetts returnerar hello Batch-tjänsten 400 (felaktig begäran). tooadd hello roll i hello Azure-portalen:

    1. Välj hello **VNet**, sedan **åtkomstkontroll (IAM)** > **roller** > **Virtual Machine-deltagare**  >  **Lägga till**.
    2. På hello **lägga till behörigheter** bladet, Välj hello **Virtual Machine-deltagare** roll.
    3. På hello **lägga till behörigheter** bladet, söka efter hello Batch-API. Sök efter var och en av dessa strängar i tur och ordning tills du hittar hello-API:
        1. **MicrosoftAzureBatch**.
        2. **Microsoft Azure Batch**. Nyare Azure AD-klientorganisationer kan använda det här namnet.
        3. **ddbf3205-c6bd-46ae-8127-60eb93363864** är hello-ID för hello Batch-API. 
    3. Välj hello Batch-API-tjänstens huvudnamn. 
    4. Klicka på **Spara**.

        ![Tilldela VM deltagare rollen tooBatch tjänstens huvudnamn](./media/batch-api-basics/iam-add-role.png)


* hello angivna undernätet måste ha tillräckligt med ledigt **IP-adresser** tooaccommodate hello Totalt antal målnoder; hello som är summan av hello `targetDedicatedNodes` och `targetLowPriorityNodes` egenskaper för hello-adresspool. Om hello undernät inte har tillräckligt med lediga IP-adresser, allokerar hello beräkningsnoder i poolen hello delvis hello Batch-tjänsten och returnerar ett fel storlek.

* hello angetts undernätet måste tillåta kommunikation från hello Batch-tjänsten toobe kan tooschedule uppgifter på hello compute-noder. Om kommunikation toohello datornoder nekas av en **Nätverkssäkerhetsgrupp (NSG)** som är associerade med hello VNet, och sedan hello Batch-tjänsten anger hello tillståndet för hello compute-noder för**oanvändbara**.

* Om hello anges virtuella nätverk har associerade **Nätverkssäkerhetsgrupper (NSG: er)** och/eller en **brandväggen**, och sedan några reserverade system portar måste vara aktiverat för inkommande kommunikation:

- För pooler som har skapats med en konfiguration av virtuell dator aktiverar du portarna 29876 och 29877, samt port 22 för Linux och port 3389 för Windows. 
- För pooler som har skapats med en molntjänstkonfiguration aktiverar du portarna 10100, 20100 och 30100. 
- Aktivera utgående anslutningar tooAzure lagring på port 443. Kontrollera också att din Azure Storage-slutpunkt kan matchas av eventuella anpassade DNS-servrar som betjänar ditt VNET. Mer specifikt URL: en hello formuläret `<account>.table.core.windows.net` ska kunna matchas.

    hello följande tabell beskrivs hello ingående portar som du behöver tooenable för programpooler som du skapade med hello konfiguration av virtuell dator:

    |    Målportar    |    Källans IP-adress      |    Lägger Batch till nätverkssäkerhetsgrupper?    |    Krävs för VM toobe användas?    |    Åtgärd från användaren   |
    |---------------------------|---------------------------|----------------------------|-------------------------------------|-----------------------|
    |    <ul><li>För pooler som har skapats med hello konfiguration av virtuell dator: 29876, 29877</li><li>För pooler som har skapats med hello molnet tjänstkonfiguration: 10100, 20100, 30100</li></ul>         |    Endast IP-adresser för Batch-tjänstrollen |    Ja. Batch lägger till NSG: er på nätverket gränssnitt (NIC) som är anslutna tooVMs hello nivå. Dessa nätverkssäkerhetsgrupper tillåter endast trafik från Batch-tjänstrollens IP-adresser. Även om du öppnar portarna för hello hela webbplatsen hämta hello trafik blockeras vid hello NIC. |    Ja  |  Du behöver inte toospecify en NSG eftersom Batch tillåts endast Batch IP-adresser. <br /><br /> Men om du anger en NSG måste du se till att dessa portar är öppna för inkommande trafik. <br /><br /> Om du anger * som hello källans IP-adress i din NSG läggs Batch ändå NSG: er på nätverkskortet som sitter tooVMs hello nivå. |
    |    3389, 22               |    Användaren maskiner som används för felsökning, så att du kan fjärransluta till hello VM.    |    Nej                                    |    Nej                     |    Lägg till NSG: er om du vill toopermit fjärråtkomst (RDP/SSH) toohello VM.   |                 

    hello i den följande tabellen beskrivs hello utgående port som du behöver tooenable toopermit åtkomst tooAzure lagring:

    |    Utgående portar    |    Mål    |    Lägger Batch till nätverkssäkerhetsgrupper?    |    Krävs för VM toobe användas?    |    Åtgärd från användaren    |
    |------------------------|-------------------|----------------------------|-------------------------------------|------------------------|
    |    443    |    Azure Storage    |    Nej    |    Ja    |    Om du lägger till alla NSG: er, se till att den här porten är öppen toooutbound trafik.    |


## <a name="scaling-compute-resources"></a>Skala beräkningsresurser
Med [autoskalning](batch-automatic-scaling.md), du kan ha hello Batch-tjänsten dynamiskt justera hello antalet beräkningsnoder i poolen enligt toohello aktuella arbetsbelastning och Resursanvändning på beräknings-scenario. Detta gör att du toolower hello totala kostnaden för att köra ditt program med hjälp av endast hello resurser du behöver och släppa dem som du inte behöver.

Du aktiverar automatisk skalning genom att skriva en [formel för automatisk skalning](batch-automatic-scaling.md#automatic-scaling-formulas) och associera formeln med en pool. hello Batch-tjänsten använder hello formeln toodetermine hello mål antalet noder i hello pool för hello nästa skalning intervall (intervall som du kan konfigurera). Du kan ange hello inställningar för automatisk skalning för en pool när du skapar den eller aktivera skalning på poolen senare. Du kan också uppdatera hello skalning på poolen skalning är aktiverad.

Exempelvis kanske kräver ett jobb att du skickar ett mycket stort antal aktiviteter toobe utförs. Du kan tilldela en skalning formeln toohello pool som justerar hello antalet noder i hello poolen baserat på hello aktuellt antal köade aktiviteter och hello slutförande hello uppgifter i jobbet hello. hello Batch-tjänsten utvärderar hello formel med jämna mellanrum och ändrar hello poolen, baserat på arbetsbelastning och andra formeln inställningar. hello-tjänsten lägger till noder efter behov när det finns ett stort antal köade aktiviteter och tar bort noder när det finns inga uppgifter i kö eller inte körs.

En skalning formel kan baseras på hello följande mått:

* **Tidsmått** baseras på statistik som samlas in var femte minut i hello angivna antalet timmar.
* **Resursmått** baseras på processoranvändning, bandbreddsanvändning, minnesanvändning och antalet noder.
* **Aktivitetsmått** baseras på aktivitetens tillstånd, t.ex. *Aktiv* (köad), *Körs* eller *Slutförd*.

När automatisk skalning minskar hello antalet beräkningsnoder i poolen, måste du bestämma hur toohandle aktiviteter som körs när hello hello minska igen. tooaccommodate, batchen innehåller en *nod flyttningen alternativet* som ska inkluderas i formler. Du kan till exempel ange att aktiviteter som körs stoppas omedelbart, stoppas omedelbart och sedan placerats i kö för körning på en annan nod, eller tillåtna toofinish innan hello noden tas bort från hello poolen.

Mer information om automatisk skalning av program finns i [Skala beräkningsnoder automatiskt i en Azure Batch-pool](batch-automatic-scaling.md).

> [!TIP]
> toomaximize beräkna resursutnyttjande, ange hello mål antalet noder toozero hello slutet av ett jobb, men tillåter toofinish för pågående aktiviteter.
>
>

## <a name="security-with-certificates"></a>Säkerhet med certifikat
Vanligtvis behöver du toouse certifikat när du kryptera eller dekryptera känsliga data för uppgifter som hello nyckel för en [Azure Storage-konto][azure_storage]. toosupport kan du installera certifikat på noder. Krypterade hemligheter släpps tootasks via kommandoradsparametrar eller inbäddat i någon av hello uppgiften resurser och hello installerade certifikat kan vara används toodecrypt dem.

Du använder hello [Lägg till certifikat] [ rest_add_cert] åtgärden (Batch REST) eller [CertificateOperations.CreateCertificate] [ net_create_cert] metod (Batch .NET) tooadd certifikat-tooa Batch-kontot. Därefter kan du associera hello certifikat med en ny eller befintlig pool. När ett certifikat är kopplad till en pool, installerar hello Batch-tjänsten hello certifikat på varje nod i hello pool. hello Batch-tjänsten installeras hello tillämpliga certifikat när hello nod startas, innan du startar alla uppgifter (inklusive hello start och aktiviteten job manager).

Om du lägger till certifikat tooan *befintliga* poolen, måste du starta om dess compute-noder för hello certifikat toobe tillämpas toohello noder.

## <a name="error-handling"></a>Felhantering
Du kan hitta den nödvändiga toohandle både aktivitets- och fel i Batch-lösning.

### <a name="task-failure-handling"></a>Hantering av aktivitetsfel
Aktivitetsfel kan delas in i följande kategorier:

* **Förbearbetningsfel**

    Om en aktivitet inte toostart, ställs inför bearbetnings fel för hello aktivitet.  

    Förbearbetning fel kan inträffa om hello aktivitetens resursfiler har flyttat, hello Storage-konto är inte tillgänglig längre eller ett annat problem uppstod när att förhindrade hello lyckade kopiering av filer toohello nod.

* **Filöverföringsfel**

    Om överföringen av filer som har angetts för en aktivitet misslyckas av någon anledning, ange ett filöverföringsfel för hello aktivitet.

    Överför fel kan inträffa om hello SAS som angavs för åtkomst till Azure Storage är ogiltig eller inte har skrivbehörighet, om hello storage-konto är inte längre tillgänglig, eller om ett annat problem uppstår som förhindrade hello lyckade kopiering av filer från filen hello-nod.    

* **Programfel**

    hello-processen som anges via kommandoraden hello aktivitet kan också misslyckas. hello processen bedöms toohave misslyckades när slutkoden noll returneras av hello process som körs av hello aktivitet (se *aktivitet slutkoder* i nästa avsnitt om hello).

    Programfel, kan du konfigurera Batch tooautomatically försök hello uppgiften tooa angivet antal gånger.

* **Begränsningsfel**

    Du kan ange en begränsning som anger hello maximal körning varaktighet för ett jobb eller en uppgift, hello *maxWallClockTime*. Detta kan vara användbart för avslutar aktiviteter som inte tooprogress.

    När hello längsta tid har överskridits hello är markerad som *slutförts*, men hello slutkoden har angetts för`0xC000013A` och hello *schedulingError* fält har markerats som `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Felsöka programfel
* `stderr` och `stdout`

    Ett program kan ge diagnostikdata som du kan använda tootroubleshoot problem under körningen. Som anges i hello tidigare avsnittet [filer och kataloger](#files-and-directories), hello Batch-tjänsten skriver standardutdata och standardfel utdata för`stdout.txt` och `stderr.txt` filer i katalogen för hello aktivitet på hello beräkningsnod. Du kan använda hello Azure-portalen eller någon av hello Batch SDK toodownload dessa filer. Du kan till exempel hämta dessa och andra filer för felsökning med hjälp av [ComputeNode.GetNodeFile] [ net_getfile_node] och [CloudTask.GetNodeFile] [ net_getfile_task] i hello Batch .NET-biblioteket.

* **Slutkoder för aktiviteter**

    Som tidigare nämnts är har en aktivitet markerats som misslyckades på grund av hello Batch-tjänsten om hello process som körs av hello uppgiften returnerar slutkoden som inte är noll. När en uppgift utförs en process, Batch fyller hello aktiviteten avsluta kodegenskapen med hello *returkod hello processens*. Det är viktigt toonote som en aktivitet slutkoden är **inte** bestäms av hello Batch-tjänsten. En uppgift slutkoden bestäms av hello-processen eller hello operativsystemet på vilken hello process som körs.

### <a name="accounting-for-task-failures-or-interruptions"></a>Ta med aktivitetsfel eller avbrott i beräkningen
Aktiviteter kan ibland misslyckas eller avbrytas. hello själva uppgiften programmet misslyckas, hello noden vilka hello uppgiften körs kan startas eller hello nod kan tas bort från poolen hello under en åtgärd för storleksändring om hello poolens flyttningen principen är uppsättningen tooremove noder omedelbart utan att vänta på uppgifter toofinish. I samtliga fall hello aktiviteten kan vara automatiskt placerats i kö av Batch för körning på en annan nod.

Det är också möjligt för ett tillfälligt fel toocause toohang en aktivitet eller ta för lång tooexecute. Du kan ange hello maximal körning intervall för en aktivitet. Om hello maximal körning intervall har överskridits avbryter hello uppgiften programmet hello Batch-tjänsten.

### <a name="connecting-toocompute-nodes"></a>Ansluta toocompute noder
Du kan utföra ytterligare felsökning och felsökning genom att logga in tooa beräkningsnod via fjärranslutning. Du kan använda hello Azure portal toodownload en Remote Desktop Protocol (RDP)-fil för Windows-noder och hämta SSH (Secure Shell) anslutningsinformationen för Linux-noder. Du kan också göra detta med hjälp av hello Batch-API: er – till exempel med [Batch .NET] [ net_rdpfile] eller [Batch Python](batch-linux-nodes.md#connect-to-linux-nodes-using-ssh).

> [!IMPORTANT]
> tooconnect tooa nod via RDP eller SSH, du måste först skapa en användare på hello-nod. toodo, du kan använda hello Azure-portalen [lägger till en användarens konto tooa nod] [ rest_create_user] med hello Batch REST API kan anropa hello [ComputeNode.CreateComputeNodeUser] [ net_create_user] metod i Batch .NET eller anropet hello [add_user] [ py_add_user] metod i hello Batch Python-modulen.
>
>

### <a name="troubleshooting-problematic-compute-nodes"></a>Felsöka problematiska beräkningsnoder
I situationer där vissa av dina uppgifter misslyckas granska Batch klientprogrammet eller tjänsten hello metadata för hello misslyckades uppgifter tooidentify problem nod. Varje nod i en pool ges ett unikt ID och hello nod som körs på en aktivitet som ingår i hello uppgiften metadata. När du har identifierat en nod som har problem kan du utföra flera åtgärder:

* **Starta om noden hello** ([REST][rest_reboot] | [.NET][net_reboot])

    Starta om hello nod kan ibland Rensa Latenta problem som låsta eller krasch processer. Observera att om din pool använder Startuppgiften eller jobbet använder en projektaktivitet förberedelse, de utförs när hello nod startas om.
* **Återavbilda hello nod** ([REST][rest_reimage] | [.NET][net_reimage])

    Detta installerar hello operativsystemet på hello-nod. Starta aktiviteter och jobb förberedande uppgifter är på nytt efter hello nod har varit avbildade som startas om en nod.
* **Ta bort hello nod hello** ([REST][rest_remove] | [.NET][net_remove])

    Ibland är det nödvändigt toocompletely ta bort hello noden från hello poolen.
* **Inaktivera schemaläggning på hello nod** ([REST][rest_offline] | [.NET][net_offline])

    Detta tar effektivt hello noden offline så att inga ytterligare aktiviteter tilldelas tooit, men tillåter hello nod tooremain körs och i hello pool. Detta gör du tooperform ytterligare undersökning hello orsak hello fel utan att förlora data hello om aktiviteten, och utan hello nod orsakar misslyckade ytterligare aktiviteter. Du kan till exempel inaktivera schemaläggning på hello nod sedan [fjärransluta](#connecting-to-compute-nodes) tooexamine hello nodens händelseloggar eller utföra andra åtgärder för felsökning. När du är klar undersökningen kan du sedan kan ta hello nod online igen genom att aktivera schemaläggning ([REST][rest_online] | [.NET] [ net_online]), eller utföra en hello andra åtgärder som nämnts tidigare.

> [!IMPORTANT]
> Med varje åtgärd som beskrivs i det här avsnittet--omstart är avbildningsåterställning, ta bort och inaktivera schemaläggning – du kan toospecify hur aktiviteter som körs på hello noden hanteras när du utför hello-åtgärd. Till exempel när du inaktiverar schemaläggning på en nod med hjälp av hello Batch .NET-klientbibliotek, kan du ange en [DisableComputeNodeSchedulingOption] [ net_offline_option] enum värdet toospecify om för**Avsluta** köra aktiviteter, **Requeue** dem för schemaläggning på andra noder eller låt pågående aktiviteter toocomplete innan du utför åtgärden hello (**TaskCompletion**).
>
>

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om hello [Batch-API: er och verktyg](batch-apis-tools.md) tillgängliga för att skapa Batch-lösningar.
* Gå igenom en Batch exempelprogrammet stegvisa i [komma igång med hello Azure Batch-biblioteket för .NET](batch-dotnet-get-started.md). Det finns också en [Python-versionen](batch-python-tutorial.md) hello kursen som kör en arbetsbelastning på Linux compute-noder.
* Hämta och skapa hello [Batch Explorer] [ github_batchexplorer] exempelprojektet för användning när du utvecklar dina Batch-lösningar. Med hello Batch Explorer kan utföra du följande hello och mycket mer:

  * Övervaka och hantera pooler, jobb och aktiviteter i ditt Batch-konto
  * Ladda ned `stdout.txt`, `stderr.txt` och andra filer från noder
  * Skapa användare på noder och ladda ned RDP-filer för fjärrinloggning
* Lär dig hur för[skapa pooler med Linux datornoderna](batch-linux-nodes.md).
* Besök hello [Azure Batch-forum] [ batch_forum] på MSDN. hello forum är en bra placera tooask frågor, oavsett om det bara lära sig eller en expert med Batch.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
