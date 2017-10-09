---
title: "aaaPoint ett företags Internet domän tooa Traffic Manager-domännamn | Microsoft Docs"
description: "Den här artikeln hjälper dig peka företagets namn tooa Traffic Manager-domän domännamn."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 29822946-2d45-4434-ba47-fc180a445cc3
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
ms.openlocfilehash: 84c428f60a1dc70452bf957d98a68c95e0b51715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="point-a-company-internet-domain-tooan-azure-traffic-manager-domain"></a><span data-ttu-id="8bec1-103">Peka företagets domän tooan Azure Traffic Manager Internetdomän</span><span class="sxs-lookup"><span data-stu-id="8bec1-103">Point a company Internet domain tooan Azure Traffic Manager domain</span></span>

<span data-ttu-id="8bec1-104">När du skapar en Traffic Manager-profil, tilldelar Azure automatiskt ett DNS-namn för profilen.</span><span class="sxs-lookup"><span data-stu-id="8bec1-104">When you create a Traffic Manager profile, Azure automatically assigns a DNS name for that profile.</span></span> <span data-ttu-id="8bec1-105">toouse ett namn från DNS-zon skapa en CNAME DNS-post som mappar toohello domännamnet för Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="8bec1-105">toouse a name from your DNS zone, create a CNAME DNS record that maps toohello domain name of your Traffic Manager profile.</span></span> <span data-ttu-id="8bec1-106">Du hittar hello Traffic Manager-domännamnet i hello **allmänna** avsnitt på sidan för konfiguration av hello i hello Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="8bec1-106">You can find hello Traffic Manager domain name in hello **General** section on hello Configuration page of hello Traffic Manager profile.</span></span>

<span data-ttu-id="8bec1-107">Till exempel toopoint namnet www.contoso.com toohello contoso.trafficmanager.net för Traffic Manager-DNS-namn, du skulle skapa hello följande DNS-resursposten:</span><span class="sxs-lookup"><span data-stu-id="8bec1-107">For example, toopoint name www.contoso.com toohello Traffic Manager DNS name contoso.trafficmanager.net, you would create hello following DNS resource record:</span></span>

    www.contoso.com IN CNAME contoso.trafficmanager.net

<span data-ttu-id="8bec1-108">Alla trafikförfrågningar för*www.contoso.com* hämta dirigeras för*contoso.trafficmanager.net*.</span><span class="sxs-lookup"><span data-stu-id="8bec1-108">All traffic requests too*www.contoso.com* get directed too*contoso.trafficmanager.net*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8bec1-109">Du kan inte peka en andranivådomän, t.ex *contoso.com*, toohello Traffic Manager-domän.</span><span class="sxs-lookup"><span data-stu-id="8bec1-109">You cannot point a second-level domain, such as *contoso.com*, toohello Traffic Manager domain.</span></span> <span data-ttu-id="8bec1-110">DNS-protokollstandarder tillåter inte CNAME-poster för andra nivåns domännamn.</span><span class="sxs-lookup"><span data-stu-id="8bec1-110">DNS protocol standards do not allow CNAME records for second-level domain names.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8bec1-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8bec1-111">Next steps</span></span>

* [<span data-ttu-id="8bec1-112">Routningsmetoder för Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="8bec1-112">Traffic Manager routing methods</span></span>](traffic-manager-routing-methods.md)
* [<span data-ttu-id="8bec1-113">Traffic Manager – Inaktivera, aktiver eller ta bort en profil</span><span class="sxs-lookup"><span data-stu-id="8bec1-113">Traffic Manager - Disable, enable or delete a profile</span></span>](disable-enable-or-delete-a-profile.md)
* [<span data-ttu-id="8bec1-114">Traffic Manager – Inaktivera eller aktivera en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="8bec1-114">Traffic Manager - Disable or enable an endpoint</span></span>](disable-or-enable-an-endpoint.md)
