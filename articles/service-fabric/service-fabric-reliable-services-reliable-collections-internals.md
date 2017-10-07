---
title: "aaaAzure Service Fabric tillförlitliga tillstånd Manager och tillförlitlig samling internals | Microsoft Docs"
description: "Ingående om tillförlitliga samling begrepp och design i Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: 651bfb52785a2475e4840cd471e87220d1936391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a><span data-ttu-id="34c2f-103">Azure Service Fabric tillförlitlig Tillståndshanterare och tillförlitlig samling internals</span><span class="sxs-lookup"><span data-stu-id="34c2f-103">Azure Service Fabric Reliable State Manager and Reliable Collection internals</span></span>
<span data-ttu-id="34c2f-104">Det här dokumentet går på djupet i tillförlitliga Tillståndshanterare och tillförlitlig samlingar toosee hur kärnkomponenter fungerar hello bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="34c2f-104">This document delves inside Reliable State Manager and Reliable Collections toosee how core components work behind hello scenes.</span></span>

> [!NOTE]
> <span data-ttu-id="34c2f-105">Det här dokumentet är arbetet pågår.</span><span class="sxs-lookup"><span data-stu-id="34c2f-105">This document is work in-progress.</span></span> <span data-ttu-id="34c2f-106">Lägga till kommentarer toothis artikel tootell oss vilka avsnitt som du vill att toolearn mer om.</span><span class="sxs-lookup"><span data-stu-id="34c2f-106">Add comments toothis article tootell us what topic you would like toolearn more about.</span></span>
>

##  <a name="local-persistence-model-log-and-checkpoint"></a><span data-ttu-id="34c2f-107">Lokala beständiga modell: loggen och kontrollpunkt</span><span class="sxs-lookup"><span data-stu-id="34c2f-107">Local persistence model: log and checkpoint</span></span>
<span data-ttu-id="34c2f-108">hello tillförlitliga Tillståndshanterare och tillförlitlig samlingar följer en beständiga modell som kallas loggen och kontrollpunkt.</span><span class="sxs-lookup"><span data-stu-id="34c2f-108">hello Reliable State Manager and Reliable Collections follow a persistence model that is called Log and Checkpoint.</span></span>
<span data-ttu-id="34c2f-109">I den här modellen är varje tillståndsändring inloggad disk först och sedan tillämpas i minnet.</span><span class="sxs-lookup"><span data-stu-id="34c2f-109">In this model, each state change is logged on disk first and then applied in memory.</span></span>
<span data-ttu-id="34c2f-110">hello fullständig tillstånd själva sparas bara ibland (kallas även</span><span class="sxs-lookup"><span data-stu-id="34c2f-110">hello complete state itself is persisted only occasionally (a.k.a.</span></span> <span data-ttu-id="34c2f-111">Kontrollpunkten).</span><span class="sxs-lookup"><span data-stu-id="34c2f-111">Checkpoint).</span></span>
<span data-ttu-id="34c2f-112">hello fördelen är att går blev sekventiella Lägg endast skrivåtgärder på disken för bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="34c2f-112">hello benefit is that deltas are turned into sequential append-only writes on disk for improved performance.</span></span>

<span data-ttu-id="34c2f-113">toobetter förstå hello loggen och kontrollpunkt modell, låt oss först titta på hello oändlig disk scenario.</span><span class="sxs-lookup"><span data-stu-id="34c2f-113">toobetter understand hello Log and Checkpoint model, let’s first look at hello infinite disk scenario.</span></span>
<span data-ttu-id="34c2f-114">hello tillförlitliga Tillståndshanterare loggar varje åtgärd innan de replikeras.</span><span class="sxs-lookup"><span data-stu-id="34c2f-114">hello Reliable State Manager logs every operation before it is replicated.</span></span>
<span data-ttu-id="34c2f-115">Loggning kan hello tillförlitliga samlingar tooapply hello åtgärden endast i minnet.</span><span class="sxs-lookup"><span data-stu-id="34c2f-115">Logging allows hello Reliable Collections tooapply hello operation only in memory.</span></span>
<span data-ttu-id="34c2f-116">Eftersom loggar sparas även när hello replik misslyckas och måste startas om toobe har hello tillförlitliga Tillståndshanterare tillräckligt med information i dess loggen tooreplay alla hello åtgärder hello replik har förlorat.</span><span class="sxs-lookup"><span data-stu-id="34c2f-116">Since logs are persisted, even when hello replica fails and needs toobe restarted, hello Reliable State Manager has enough information in its log tooreplay all hello operations hello replica has lost.</span></span>
<span data-ttu-id="34c2f-117">Hello disk är oändlig loggposter behöver aldrig toobe tas bort och hello tillförlitliga samlingen måste toomanage endast hello InMemory-läge.</span><span class="sxs-lookup"><span data-stu-id="34c2f-117">As hello disk is infinite, log records never need toobe removed and hello Reliable Collection needs toomanage only hello in-memory state.</span></span>

<span data-ttu-id="34c2f-118">Nu ska vi titta på hello ändligt disk scenario.</span><span class="sxs-lookup"><span data-stu-id="34c2f-118">Now let’s look at hello finite disk scenario.</span></span>
<span data-ttu-id="34c2f-119">Eftersom loggposter ackumuleras körs hello tillförlitliga Tillståndshanterare slut på diskutrymme.</span><span class="sxs-lookup"><span data-stu-id="34c2f-119">As log records accumulate, hello Reliable State Manager will run out of disk space.</span></span>
<span data-ttu-id="34c2f-120">Innan i så fall måste hello tillförlitliga Tillståndshanterare tootruncate dess toomake utrymme i loggen för hello senare poster.</span><span class="sxs-lookup"><span data-stu-id="34c2f-120">Before that happens, hello Reliable State Manager needs tootruncate its log toomake room for hello newer records.</span></span>
<span data-ttu-id="34c2f-121">Tillstånd för tillförlitlig Manager begäranden hello tillförlitliga samlingar toocheckpoint toodisk sina InMemory-läge.</span><span class="sxs-lookup"><span data-stu-id="34c2f-121">Reliable State Manager requests hello Reliable Collections toocheckpoint their in-memory state toodisk.</span></span>
<span data-ttu-id="34c2f-122">Nu skulle hello tillförlitliga samlingar' sparas tillståndet i minnet.</span><span class="sxs-lookup"><span data-stu-id="34c2f-122">At this point, hello Reliable Collections' would persist its in-memory state.</span></span>
<span data-ttu-id="34c2f-123">När hello tillförlitliga samlingar har slutfört sin kontrollpunkter trunkera hello tillförlitliga Tillståndshanterare hello loggen toofree utrymme på disken.</span><span class="sxs-lookup"><span data-stu-id="34c2f-123">Once hello Reliable Collections complete their checkpoints, hello Reliable State Manager can truncate hello log toofree up disk space.</span></span>
<span data-ttu-id="34c2f-124">När hello replik måste toobe startas om, tillförlitlig samlingar återställa sina kontrollpunkt tillstånd och hello tillförlitliga Tillståndshanterare återställer och spelar upp alla hello tillståndsändringar som gjorts sedan hello senaste kontrollpunkten.</span><span class="sxs-lookup"><span data-stu-id="34c2f-124">When hello replica needs toobe restarted, Reliable Collections recover their checkpointed state, and hello Reliable State Manager recovers and plays back all hello state changes that occurred since hello last checkpoint.</span></span>

<span data-ttu-id="34c2f-125">Ett annat värde lägga till kontrollpunkter är det förbättrar återställningstiden i vanliga scenarier.</span><span class="sxs-lookup"><span data-stu-id="34c2f-125">Another value add of checkpointing is that it improves recovery times in common scenarios.</span></span> <span data-ttu-id="34c2f-126">Loggen innehåller alla åtgärder som inträffat sedan hello senaste kontrollpunkten.</span><span class="sxs-lookup"><span data-stu-id="34c2f-126">Log contains all operations that have happened since hello last checkpoint.</span></span>
<span data-ttu-id="34c2f-127">Så kan den innehålla flera versioner av ett objekt som flera värden för en viss rad i tillförlitliga ordlistan.</span><span class="sxs-lookup"><span data-stu-id="34c2f-127">So it may include multiple versions of an item like multiple values for a given row in Reliable Dictionary.</span></span>
<span data-ttu-id="34c2f-128">En tillförlitlig samling kontrollpunkter hello däremot bara senaste versionen av varje värde för en nyckel.</span><span class="sxs-lookup"><span data-stu-id="34c2f-128">In contrast, a Reliable Collection checkpoints only hello latest version of each value for a key.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34c2f-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34c2f-129">Next steps</span></span>
* [<span data-ttu-id="34c2f-130">Transaktioner och lås</span><span class="sxs-lookup"><span data-stu-id="34c2f-130">Transactions and Locks</span></span>](service-fabric-reliable-services-reliable-collections-transactions-locks.md)

