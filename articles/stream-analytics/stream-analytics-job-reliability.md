---
title: "Undvika avbrott i tjänsten med Azure Stream Analytics-jobb | Microsoft Docs"
description: "Riktlinjer för att göra Stream Analytics jobb uppgraderingen flexibel."
services: stream-analytics
documentationCenter: 
authors: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 3ac65c93ecb47e93e963dd9869a7af70f73b19c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a><span data-ttu-id="9b11c-103">Garantera Stream Analytics-jobbet tillförlitlighet under uppdateringar av tjänsten</span><span class="sxs-lookup"><span data-stu-id="9b11c-103">Guarantee Stream Analytics job reliability during service updates</span></span>

<span data-ttu-id="9b11c-104">En del av en helt hanterad tjänst som är hello kapaciteten toointroduce nya service funktioner och förbättringar i en snabb takt.</span><span class="sxs-lookup"><span data-stu-id="9b11c-104">Part of being a fully managed service is hello capability toointroduce new service functionality and improvements at a rapid pace.</span></span> <span data-ttu-id="9b11c-105">Stream Analytics kan därför ha en tjänstuppdatering distribuera varje vecka (eller oftare).</span><span class="sxs-lookup"><span data-stu-id="9b11c-105">As a result, Stream Analytics can have a service update deploy on a weekly (or more frequent) basis.</span></span> <span data-ttu-id="9b11c-106">Oavsett hur mycket testning finns fortfarande en risk för att en befintlig, körs jobbet avbryts på grund av toohello införandet av ett programfel.</span><span class="sxs-lookup"><span data-stu-id="9b11c-106">No matter how much testing is done there is still a risk that an existing, running job may break due toohello introduction of a bug.</span></span> <span data-ttu-id="9b11c-107">För kunder som kör kritiska strömmande bearbetning jobb måste riskerna toobe undvikas.</span><span class="sxs-lookup"><span data-stu-id="9b11c-107">For customers who run critical streaming processing jobs these risks need toobe avoided.</span></span> <span data-ttu-id="9b11c-108">En mekanism för kunder kan använda tooreduce risken är Azures  **[parad region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  modell.</span><span class="sxs-lookup"><span data-stu-id="9b11c-108">A mechanism customers can use tooreduce this risk is Azure’s **[paired region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** model.</span></span> 

## <a name="how-do-azure-paired-regions-address-this-concern"></a><span data-ttu-id="9b11c-109">Hur parad Azure-regioner adressera detta?</span><span class="sxs-lookup"><span data-stu-id="9b11c-109">How do Azure paired regions address this concern?</span></span>

<span data-ttu-id="9b11c-110">Stream Analytics garanterar jobb i parad regioner har uppdaterats i separata grupper.</span><span class="sxs-lookup"><span data-stu-id="9b11c-110">Stream Analytics guarantees jobs in paired regions are updated in separate batches.</span></span> <span data-ttu-id="9b11c-111">Det finns därför en tillräcklig lucka mellan hello uppdateringar tooidentify potentiella bryta buggar och åtgärda dem.</span><span class="sxs-lookup"><span data-stu-id="9b11c-111">As a result there is a sufficient time gap between hello updates tooidentify potential breaking bugs and remediate them.</span></span>

<span data-ttu-id="9b11c-112">_Med undantag för hello av centrala Indien_ (vars parad region, södra Indien och inte har Stream Analytics förekomst), hello distribution av en uppdatering tooStream Analytics inte skulle inträffar vid hello samma tid i en parad regioner.</span><span class="sxs-lookup"><span data-stu-id="9b11c-112">_With hello exception of Central India_ (whose paired region, South India, does not have Stream Analytics presence), hello deployment of an update tooStream Analytics would not occur at hello same time in a set of paired regions.</span></span> <span data-ttu-id="9b11c-113">Distribution i flera områden **i hello samma grupp** kan uppstå **på hello samtidigt**.</span><span class="sxs-lookup"><span data-stu-id="9b11c-113">Deployments in multiple regions **in hello same group** may occur **at hello same time**.</span></span>

<span data-ttu-id="9b11c-114">hello artikel på  **[tillgänglighet och parad regioner](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  har hello allra senaste informationen som regioner har parats ihop.</span><span class="sxs-lookup"><span data-stu-id="9b11c-114">hello article on **[availability and paired regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** has hello most up-to-date information on which regions are paired.</span></span>

<span data-ttu-id="9b11c-115">Kunder är uppmanade toodeploy identiska jobb tooboth länkas regioner.</span><span class="sxs-lookup"><span data-stu-id="9b11c-115">Customers are advised toodeploy identical jobs tooboth paired regions.</span></span> <span data-ttu-id="9b11c-116">Dessutom tooStream Analytics interna övervakningsfunktionerna kunder är också bäst toomonitor hello jobb som om **både** produktion jobb.</span><span class="sxs-lookup"><span data-stu-id="9b11c-116">In addition tooStream Analytics internal monitoring capabilities, customers are also advised toomonitor hello jobs as if **both** are production jobs.</span></span> <span data-ttu-id="9b11c-117">Om ett avbrott identifierade toobe ett resultat av hello Stream Analytics tjänstuppdatering eskalera på rätt sätt och över alla underordnade konsumenter toohello felfri jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="9b11c-117">If a break is identified toobe a result of hello Stream Analytics service update, escalate appropriately and fail over any downstream consumers toohello healthy job output.</span></span> <span data-ttu-id="9b11c-118">Eskalering toosupport kommer att förhindra att hello parad region påverkas av hello ny distribution och underhålla hello integriteten hos hello länkas jobb.</span><span class="sxs-lookup"><span data-stu-id="9b11c-118">Escalation toosupport will prevent hello paired region from being affected by hello new deployment and maintain hello integrity of hello paired jobs.</span></span>
