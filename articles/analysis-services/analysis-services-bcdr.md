---
title: "aaaAzure hög tillgänglighet för Analysis Services | Microsoft Docs"
description: "Säkerställa hög tillgänglighet för Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 6e09536c5bd7dc7f88f9b662e1a9f42d2b8c969b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analysis-services-high-availability"></a><span data-ttu-id="77ce5-103">Hög tillgänglighet för Analysis Services</span><span class="sxs-lookup"><span data-stu-id="77ce5-103">Analysis Services high availability</span></span>
<span data-ttu-id="77ce5-104">Den här artikeln beskriver garantera hög tillgänglighet för Azure Analysis Services-servrar.</span><span class="sxs-lookup"><span data-stu-id="77ce5-104">This article describes assuring high availability for Azure Analysis Services servers.</span></span> 


## <a name="assuring-high-availability-during-a-service-disruption"></a><span data-ttu-id="77ce5-105">Säkerställa hög tillgänglighet vid ett avbrott i tjänsten</span><span class="sxs-lookup"><span data-stu-id="77ce5-105">Assuring high availability during a service disruption</span></span>
<span data-ttu-id="77ce5-106">När det är sällsynt, kan ett Azure-Datacenter ha ett avbrott.</span><span class="sxs-lookup"><span data-stu-id="77ce5-106">While rare, an Azure data center can have an outage.</span></span> <span data-ttu-id="77ce5-107">När ett avbrott inträffar gör en business-avbrott som kan senaste några minuter eller kan senaste timmar.</span><span class="sxs-lookup"><span data-stu-id="77ce5-107">When an outage occurs, it causes a business disruption that might last a few minutes or might last for hours.</span></span> <span data-ttu-id="77ce5-108">Hög tillgänglighet uppnås ofta med server redundans.</span><span class="sxs-lookup"><span data-stu-id="77ce5-108">High availability is most often achieved with server redundancy.</span></span> <span data-ttu-id="77ce5-109">Du kan uppnå redundans med Azure Analysis Services genom att skapa ytterligare sekundära servrar i en eller flera regioner.</span><span class="sxs-lookup"><span data-stu-id="77ce5-109">With Azure Analysis Services, you can achieve redundancy by creating additional, secondary servers in one or more regions.</span></span> <span data-ttu-id="77ce5-110">När du skapar redundanta servrar, tooassure hello data och metadata på dessa servrar är synkroniserad med hello-server i en region som är offline, kan du:</span><span class="sxs-lookup"><span data-stu-id="77ce5-110">When creating redundant servers, tooassure hello data and metadata on those servers is in-sync with hello server in a region that has gone offline, you can:</span></span>

* <span data-ttu-id="77ce5-111">Distribuera modeller tooredundant servrar i andra regioner.</span><span class="sxs-lookup"><span data-stu-id="77ce5-111">Deploy models tooredundant servers in other regions.</span></span> <span data-ttu-id="77ce5-112">Den här metoden kräver databearbetning på både primära servern och redundanta servrar i parallellt, så alla servrar är synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="77ce5-112">This method requires processing data on both your primary server and redundant servers in-parallel, assuring all servers are in-sync.</span></span>

* <span data-ttu-id="77ce5-113">Säkerhetskopiera databaser från den primära servern och återställning på redundanta servrar.</span><span class="sxs-lookup"><span data-stu-id="77ce5-113">Back up databases from your primary server and restore on redundant servers.</span></span> <span data-ttu-id="77ce5-114">Du kan till exempel automatisera säkerhetskopieringar tooAzure lagring och återställa tooother redundanta servrar i andra regioner.</span><span class="sxs-lookup"><span data-stu-id="77ce5-114">For example, you can automate nightly backups tooAzure storage, and restore tooother redundant servers in other regions.</span></span> 

<span data-ttu-id="77ce5-115">Om den primära servern skulle få ett avbrott i båda fallen måste du ändra hello anslutningssträngar i rapportserver klienter tooconnect toohello i en annan regionala datacenter.</span><span class="sxs-lookup"><span data-stu-id="77ce5-115">In either case, if your primary server experiences an outage, you must change hello connection strings in reporting clients tooconnect toohello server in a different regional datacenter.</span></span> <span data-ttu-id="77ce5-116">Den här ändringen ska betraktas som en sista utväg och endast om ett oåterkalleligt regionala data center avbrott inträffar.</span><span class="sxs-lookup"><span data-stu-id="77ce5-116">This change should be considered a last resort and only if a catastrophic regional data center outage occurs.</span></span> <span data-ttu-id="77ce5-117">Det är troligt att en data center avbrott som värd för den primära servern skulle komma online igen innan du kan uppdatera anslutningar på alla klienter.</span><span class="sxs-lookup"><span data-stu-id="77ce5-117">It's more likely a data center outage hosting your primary server would come back online before you could update connections on all clients.</span></span> 



## <a name="related-information"></a><span data-ttu-id="77ce5-118">Relaterad information</span><span class="sxs-lookup"><span data-stu-id="77ce5-118">Related information</span></span>
<span data-ttu-id="77ce5-119">[Säkerhetskopiering och återställning](analysis-services-backup.md) </span><span class="sxs-lookup"><span data-stu-id="77ce5-119">[Backup and restore](analysis-services-backup.md) </span></span>  
[<span data-ttu-id="77ce5-120">Hantera Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="77ce5-120">Manage Azure Analysis Services</span></span>](analysis-services-manage.md) 

