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
ms.openlocfilehash: 8dc19e1b37082c87d2990ad910d1af786f8b9280
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a><span data-ttu-id="a1d81-103">Garantera Stream Analytics-jobbet tillförlitlighet under uppdateringar av tjänsten</span><span class="sxs-lookup"><span data-stu-id="a1d81-103">Guarantee Stream Analytics job reliability during service updates</span></span>

<span data-ttu-id="a1d81-104">En del av en helt hanterad tjänst som är möjligheten att införa den nya tjänsten funktioner och förbättringar i en snabb takt.</span><span class="sxs-lookup"><span data-stu-id="a1d81-104">Part of being a fully managed service is the capability to introduce new service functionality and improvements at a rapid pace.</span></span> <span data-ttu-id="a1d81-105">Stream Analytics kan därför ha en tjänstuppdatering distribuera varje vecka (eller oftare).</span><span class="sxs-lookup"><span data-stu-id="a1d81-105">As a result, Stream Analytics can have a service update deploy on a weekly (or more frequent) basis.</span></span> <span data-ttu-id="a1d81-106">Oavsett hur mycket testning finns fortfarande en risk för att en befintlig, körs jobbet avbryts på grund av ett programfel.</span><span class="sxs-lookup"><span data-stu-id="a1d81-106">No matter how much testing is done there is still a risk that an existing, running job may break due to the introduction of a bug.</span></span> <span data-ttu-id="a1d81-107">För kunder som kör kritiska strömmande bearbetning jobb behöver dessa risker undvikas.</span><span class="sxs-lookup"><span data-stu-id="a1d81-107">For customers who run critical streaming processing jobs these risks need to be avoided.</span></span> <span data-ttu-id="a1d81-108">En funktion som kunder kan använda för att minska denna risk är Azures  **[parad region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  modell.</span><span class="sxs-lookup"><span data-stu-id="a1d81-108">A mechanism customers can use to reduce this risk is Azure’s **[paired region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** model.</span></span> 

## <a name="how-do-azure-paired-regions-address-this-concern"></a><span data-ttu-id="a1d81-109">Hur parad Azure-regioner adressera detta?</span><span class="sxs-lookup"><span data-stu-id="a1d81-109">How do Azure paired regions address this concern?</span></span>

<span data-ttu-id="a1d81-110">Stream Analytics garanterar jobb i parad regioner har uppdaterats i separata grupper.</span><span class="sxs-lookup"><span data-stu-id="a1d81-110">Stream Analytics guarantees jobs in paired regions are updated in separate batches.</span></span> <span data-ttu-id="a1d81-111">Det finns därför en tillräcklig lucka mellan uppdateringar att identifiera potentiella bryta buggar och åtgärda dem.</span><span class="sxs-lookup"><span data-stu-id="a1d81-111">As a result there is a sufficient time gap between the updates to identify potential breaking bugs and remediate them.</span></span>

<span data-ttu-id="a1d81-112">_Med undantag för centrala Indien_ (vars parad region, södra Indien och inte har Stream Analytics förekomst), distribution av en uppdatering till Stream Analytics inte skulle uppstå samtidigt i en parad regioner.</span><span class="sxs-lookup"><span data-stu-id="a1d81-112">_With the exception of Central India_ (whose paired region, South India, does not have Stream Analytics presence), the deployment of an update to Stream Analytics would not occur at the same time in a set of paired regions.</span></span> <span data-ttu-id="a1d81-113">Distribution i flera områden **i samma grupp** kan uppstå **samtidigt**.</span><span class="sxs-lookup"><span data-stu-id="a1d81-113">Deployments in multiple regions **in the same group** may occur **at the same time**.</span></span>

<span data-ttu-id="a1d81-114">Artikel om  **[tillgänglighet och parad regioner](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  har den senaste informationen som regioner har parats ihop.</span><span class="sxs-lookup"><span data-stu-id="a1d81-114">The article on **[availability and paired regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** has the most up-to-date information on which regions are paired.</span></span>

<span data-ttu-id="a1d81-115">Kunder bör distribuera identiska jobb till båda parad regioner.</span><span class="sxs-lookup"><span data-stu-id="a1d81-115">Customers are advised to deploy identical jobs to both paired regions.</span></span> <span data-ttu-id="a1d81-116">Förutom Stream Analytics interna övervakningsfunktionerna kunder bör också övervaka jobb som om **både** produktion jobb.</span><span class="sxs-lookup"><span data-stu-id="a1d81-116">In addition to Stream Analytics internal monitoring capabilities, customers are also advised to monitor the jobs as if **both** are production jobs.</span></span> <span data-ttu-id="a1d81-117">Om ett avbrott identifieras som ett resultat av tjänstuppdatering Stream Analytics eskalera på rätt sätt och över några underordnade konsumenter till Felfri jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="a1d81-117">If a break is identified to be a result of the Stream Analytics service update, escalate appropriately and fail over any downstream consumers to the healthy job output.</span></span> <span data-ttu-id="a1d81-118">Eskalering till stöd för kommer att förhindra att parad region påverkas av den nya distributionen och underhålla integriteten hos parad jobb.</span><span class="sxs-lookup"><span data-stu-id="a1d81-118">Escalation to support will prevent the paired region from being affected by the new deployment and maintain the integrity of the paired jobs.</span></span>
