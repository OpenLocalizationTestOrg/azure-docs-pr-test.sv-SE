---
title: "aaaRun Azure Batch arbetsbelastningar på kostnadseffektiv låg prioritet för virtuella datorer (förhandsversion) | Microsoft Docs"
description: "Lär dig hur tooprovision låg prioritet VMs tooreduce hello kostnader för Azure Batch-arbetsbelastningar."
services: batch
author: mscurrell
manager: timlt
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 07/21/2017
ms.author: markscu
ms.openlocfilehash: 91a5e89a819d05583e6b146932d925e217b4be4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-low-priority-vms-with-batch-preview"></a>Använd låg prioritet virtuella datorer med Batch (förhandsgranskning)

Azure Batch har låg priorty virtuella maskiner (VMs) tooreduce hello kostnaden för Batch-arbetsbelastningar. VM med låg prioritet möjliggör nya typer av Batch-arbetsbelastningar genom att tillhandahålla mycket datorkraft som också är ekonomiskt.

Låg prioritet VMs utnyttja överflödiga kapacitet i Azure. När du anger låg prioritet virtuella datorer i din pooler kan Azure Batch automatiskt använda den här överskott när det är tillgängligt.

hello förhållandet för med låg prioritet virtuella datorer är dessa virtuella datorer kan avbrytas om någon överskott kapacitet finns i Azure. Därför är låg prioritet mest lämpliga för vissa typer av arbetsbelastningar. Använd VM med låg prioritet för batch- och asynkron bearbetning arbetsbelastningar där hello tid för slutförande av jobbet är flexibel och hello arbete distribueras till många virtuella datorer.

Låg prioritet är betydligt mindre dyrbart än dedikerade virtuella datorer. Information om priser, se [Batch priser](https://azure.microsoft.com/pricing/details/batch/).

En ytterligare beskrivning av VM med låg prioritet finns hello blogginlägget: [Batch computing på en bråkdel av hello pris](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).

> [!IMPORTANT]
> Låg prioritet VM finns för närvarande i förhandsvisning och är bara tillgängliga för arbetsbelastningar som körs i en Batch. 
>
>

## <a name="use-cases-for-low-priority-vms"></a>Användningsfall för VM med låg prioritet

Hello egenskaper med låg prioritet virtuella datorer, vilka arbetsbelastningar kan och inte använda dem? I allmänhet är batch bearbetningsbelastningar passa, eftersom jobben är uppdelad i många parallella aktiviteter eller det finns många jobb som skalats ut och distribueras över flera virtuella datorer.

-   toomaximize användning av överskott kapacitet i Azure, lämplig jobb kan skala ut.

-   Ibland virtuella datorer kanske inte tillgänglig eller ska avbrytas, vilket leder till minskad kapacitet för jobb och leda tootask avbrott och repriser. Jobb måste därför vara flexibel hello tidpunkt som de kan ta toorun.

-   Jobb med längre uppgifter kan påverkas mer om avbryts. Om tidskrävande uppgifter implementera kontrollpunkter toosave förlopp som de körs, skulle sedan effekten av avbrott vara betydligt lägre. Uppgifter med kortare körningstider tenderar toowork bäst med låg prioritet virtuella datorer som hello effekten av avbrott är mindre.

-   Långvariga MPI-jobb som använder flera virtuella datorer inte är bra toouse kommer låg prioritet virtuella datorer som en frånkopplas VM troligen lead toohello hela jobbet med toobe kör igen.

Några exempel på batchbearbetning använder fall utmärkt toouse låg prioritet är:

-   **Utveckling och testning**: I synnerhet om storskaliga lösningar utvecklas betydande besparingar kan genomföras. Kan dra nytta av alla typer av testning, men stora belastningen testning och regression testning är bra använder.

-   **Kompletta kapacitet på begäran**: VM med låg prioritet kan användas för att komplettera regular dedikerade virtuella datorer – när det är tillgängligt, jobb kan skalas och därför slutföra snabbare för lägre kostnad; när inte tillgänglig, hello baslinje för dedikerade virtuella datorer är tillgängliga.

-   **Flexibla jobbet körningstid**: om det finns flexibilitet i hello tid jobb har toocomplete, och sedan potentiella fall kapaciteten som kan tillåtas, men med hello lägga till jobb med låg prioritet virtuella datorer som ofta körs snabbare och för en lägre kostnad.

Batch-pooler kan vara konfigurerade toouse låg prioritet virtuella datorer på flera sätt, beroende på hello flexibilitet i jobbet körningstid:

-   VM med låg prioritet kan endast användas i en pool och Batch ska bara återställa alla preempted kapacitet när det är tillgängligt. Detta är hello billigaste sätt tooexecute jobb som används för virtuella datorer bara låg prioritet.

-   VM med låg prioritet kan användas tillsammans med en fast baslinje för dedikerade virtuella datorer. hello garanterar fast antal dedikerade virtuella datorer det alltid vissa kapacitet tookeep ett jobb som fortskrider.

-   Det kan vara dynamiska blandning av dedikerade och låg prioritet virtuella datorer, så att billigare låg prioritet virtuella datorer används endast när det är tillgängligt, men hello fullständig prisvärda dedikerade virtuella datorer skalas vid behov, tookeep en minimal mängd kapacitet tillgängliga tookeep hello jobb fortskrider.

## <a name="batch-support-for-low-priority-vms"></a>Batch-stöd för virtuella datorer låg prioritet

Azure Batch tillhandahåller flera funktioner som gör det enkelt tooconsume och dra nytta av låg prioritet virtuella datorer:

-   Batch-pooler kan innehålla både dedikerade virtuella datorer och låg prioritet virtuella datorer. hello-numret för varje typ av VM kan anges när en programpool skapas eller ändras när som helst för en befintlig pool med hjälp av åtgärden för hello explicit ändra storlek eller Autoskala. Jobb- och skicka kan ändras och behöver inte bry sig med hello VM typer i hello pool. Det är också möjligt toohave poolen helt använder låg prioritet VMs toorun jobb som billigt som möjligt, men rotationsrutan upp dedikerade virtuella datorer om hello kapacitet sjunker under en minsta tröskelvärdet för att hålla jobb som körs.

-   Batch-pooler söka automatiskt toohello mål antal VM med låg prioritet. Om virtuella datorer som frånkopplas försöker Batch tooreplace hello förlorade kapacitet och returnera toohello mål.

-   I hello fallet uppgifter störs Batch identifierar och automatiskt meddelanden uppgifter toobe kör igen.

-   Låg prioritet virtuella datorer har ett kärnkvot som skiljer sig från dedikerade virtuella datorer. 
    hello citattecken för virtuella datorer låg prioritet är högre än dedikerade virtuella datorer, eftersom VM med låg prioritet billigare. Se [Batch-tjänsten kvoter och gränser](batch-quota-limit.md#resource-quotas) för mer information.    

> [!NOTE]
> Låg prioritet virtuella datorer stöds inte för närvarande för Batch-konton där hello poolen allokering läge har angetts för[användarens prenumeration](batch-account-create-portal.md#user-subscription-mode).
>
>

## <a name="create-and-update-pools"></a>Skapa och uppdatera pooler

En Batch-pool kan innehålla både dedikerade och låg prioritet virtuella datorer (även hänvisade tooas compute-noder). Du kan ange hello mål antalet compute-noder för virtuella datorer både dedikerade och låg prioritet. hello mål antalet noder anger hello många virtuella datorer som du vill toohave i hello pool.

Till exempel dedikerade toocreate en pool med hjälp av Azure-molntjänst virtuella datorer med ett mål för 5 virtuella datorer och 20 låg prioritet för virtuella datorer:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

toocreate en pool med virtuella Azure-datorer (i det här fallet virtuella Linux-datorer) med ett mål för 5 dedikerade virtuella datorer och 20 låg prioritet för virtuella datorer:

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04.0-LTS",
    version: "latest");

// Create hello pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard\_D2\_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

Du kan få hello aktuellt antal noder för virtuella datorer både dedikerade och låg prioritet:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

Poolen noder har en egenskap tooindicate om hello noden är en virtuell dator för dedikerade eller låg prioritet:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

När en eller flera noder i en pool frånkopplas, en lista över noderna på poolen fortfarande returneras dessa noder, hello aktuellt antal noder med låg prioritet förblir oförändrat, men de noderna har tillståndet anger toothe **tillfälligt**tillstånd. Batch försöker toofind ersättning virtuella datorer och, om det lyckas, hello noder går igenom **skapa** och sedan **Start** tillstånd innan den blir tillgänglig för körning av aktiviteten, precis som nya noder.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Skala en pool som innehåller VM med låg prioritet

Som med pooler som endast består av dedikerade virtuella datorer, är det möjligt tooscale en pool med låg prioritet virtuella datorer genom att anropa metoden för storleksändring av hello eller genom att använda automatisk skalning.

hello åtgärden Ändra storlek för poolen tar en valfri andra parameter som uppdaterar värdet för **targetLowPriorityNodes**:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

hello poolen Autoskala formeln stöder låg prioritet virtuella datorer på följande sätt:

-   Du kan hämta eller ange hello värde för hello tjänst definierade variabel **$TargetLowPriorityNodes**.

-   Du kan hämta hello värdet för hello tjänst definierade variabel **$CurrentLowPriorityNodes**.

-   Du kan hämta hello värdet för hello tjänst definierade variabel **$PreemptedNodeCount**. 
    Den här variabeln returnerar hello antalet noder i hello frånkopplas tillstånd och gör att du kan skala upp eller ned hello antal dedikerade noder, beroende på hello antalet frånkopplas noder som inte är tillgängliga.

## <a name="jobs-and-tasks"></a>Jobb och uppgifter

Jobb och uppgifter som kräver mycket lite stöd för låg prioritet noder. hello endast stöder är följande:

-   Hej JobManagerTask-egenskapen för ett jobb har en ny egenskap **AllowLowPriorityNode**. 
    När den här egenskapen är true kan du schemalägga hello projektaktivitet manager på en dedikerad eller låg prioritet nod. Om den här egenskapen är false kommer hello projektaktivitet manager att schemalagda tooa dedikerade nod.

-   En [miljövariabeln](batch-compute-node-environment-variables.md) är tillgängliga tooa aktivitet program så att den kan avgöra om den körs på en nod med låg prioritet eller dedikerade. hello miljövariabel är AZ_BATCH_NODE_IS_DEDICATED.

## <a name="handling-preemption"></a>Hantera avstängning

Virtuella datorer kan ibland avbrytas; När detta inträffar hello Batch följande:

-   hello frånkopplas virtuella datorer har det tillståndet uppdateras**tillfälligt**.
-   Om uppgifter som kördes på frånkopplas hello nod virtuella datorer, och sedan uppgifterna begärdes och kör igen.
-   hello VM raderas effektivt, inledande tooany data som lagras lokalt på hello VM förlorad.
-   hello poolen försöker kontinuerligt tooreach hello mål antalet låg prioritet noder som är tillgängliga. När ersättningskapacitet, noderna hålla sina ID: n, men är initieras igen, gå igenom **skapa** och **Start** tillstånd innan de är tillgängliga för schemaläggning.
-   Avstängningen antal är tillgängliga som ett mått i hello Azure-portalen.

## <a name="metrics"></a>Mått

Nya är tillgängliga i hello [Azure-portalen](https://portal.azure.com) för låg prioritet noder. Dessa är:

- Antalet noder med låg prioritet
- Antal kärnor med låg prioritet 
- Antalet frånkopplas noder

tooview mått i hello Azure-portalen:

1. Navigera tooyour Batch-kontot i hello-portalen och visa hello inställningar för Batch-kontot.
2. Välj **mått** från hello **övervakning** avsnitt.
3. Välj hello mått som du önskar hello **tillgängliga mått** lista.

![Mätvärden för noder med låg prioritet](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Nästa steg

* Läs hello [Batch funktionsöversikt för utvecklare](batch-api-basics.md), viktig information för alla förbereder toouse Batch. hello artikeln innehåller mer detaljerad information om Batch-tjänsten resurser som pooler, noder, jobb och uppgifter och hello många API-funktioner som du kan använda när du skapar Batch-program.
* Lär dig mer om hello [Batch-API: er och verktyg](batch-apis-tools.md) tillgängliga för att skapa Batch-lösningar.
