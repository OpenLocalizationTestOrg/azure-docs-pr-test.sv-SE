---
title: "aaaAn Application Proxy-program tar för lång tooload | Microsoft Docs"
description: "Felsöka sidan belastningen prestandaproblem med hello Azure AD Application Proxy"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4c7a51f96840966a1d88933fa4e30f39479d8a5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="an-application-proxy-application-takes-too-long-tooload"></a><span data-ttu-id="82de6-103">Ett program med Application Proxy tar för lång tooload</span><span class="sxs-lookup"><span data-stu-id="82de6-103">An Application Proxy application takes too long tooload</span></span>

<span data-ttu-id="82de6-104">Den här artikeln hjälper dig toounderstand varför ett program för Azure AD Application Proxy kan ta en lång tid tooload och vad du kan göra tooresolve problemet.</span><span class="sxs-lookup"><span data-stu-id="82de6-104">This article help you toounderstand why an Azure AD Application Proxy application may take a long time tooload, and what you can do tooresolve this issue.</span></span>

## <a name="overview"></a><span data-ttu-id="82de6-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="82de6-105">Overview</span></span>
<span data-ttu-id="82de6-106">Om dina program fungerar, men du ser en lång fördröjning, kan det finnas några mindre justeringar i nätverkets topologi som du anser tooimprove hello hastighet.</span><span class="sxs-lookup"><span data-stu-id="82de6-106">If your applications are working but you see a long latency, there may be some minor tweaks in your network topology that you can consider tooimprove hello speed.</span></span> <span data-ttu-id="82de6-107">En utvärdering av olika topologier finns hello [nätverk överväganden dokumentet](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).</span><span class="sxs-lookup"><span data-stu-id="82de6-107">For an evaluation of different topologies, see hello [network considerations document](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).</span></span>

<span data-ttu-id="82de6-108">Om dessa överväganden inte hjälper kan har vi tyvärr inte för närvarande ytterligare rekommendationer för prestandajustering.</span><span class="sxs-lookup"><span data-stu-id="82de6-108">If those considerations don’t help, we unfortunately don’t have currently have further recommendations for performance tuning.</span></span> <span data-ttu-id="82de6-109">Eftersom hello Application Proxy-tjänsten utökar toomore datacenter som kan vara närmare tooyou, kanske du toosee bättre svarstid direkt.</span><span class="sxs-lookup"><span data-stu-id="82de6-109">As hello Application Proxy service expands toomore data centers that may be closer tooyou, you may start toosee improved latency directly.</span></span> <span data-ttu-id="82de6-110">toosee hello fullständig lista över Azure data datacenter, du kan se hello [svarstid testsida](http://www.azurespeed.com/Azure/Latency).</span><span class="sxs-lookup"><span data-stu-id="82de6-110">toosee hello full list of Azure data centers, you can see hello [latency test page](http://www.azurespeed.com/Azure/Latency).</span></span> 

<span data-ttu-id="82de6-111">hello datacenter med hello Application Proxy-tjänsten kan hittas med hello [anslutningsverktyget portar Test](https://aadap-portcheck.connectorporttest.msappproxy.net/).</span><span class="sxs-lookup"><span data-stu-id="82de6-111">hello data centers with hello Application Proxy service can be found with hello [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/).</span></span> 

## <a name="feedback-on-application-proxy-data-center-locations"></a><span data-ttu-id="82de6-112">Feedback om Application Proxy data center platser</span><span class="sxs-lookup"><span data-stu-id="82de6-112">Feedback on Application Proxy data center locations</span></span> 
<span data-ttu-id="82de6-113">Det kan finnas Azure-datacenter som ännu inte med Application Proxy men leder tooa bra svarstid förbättring av du.</span><span class="sxs-lookup"><span data-stu-id="82de6-113">There may be Azure data centers that don’t as yet include Application Proxy but would lead tooa great latency improvement for you.</span></span> <span data-ttu-id="82de6-114">hello Datacenter plats < aadapfeedback@microsoft.com > så att vi kan använda din feedback tooplan som vi expandera.</span><span class="sxs-lookup"><span data-stu-id="82de6-114">hello data center location at <aadapfeedback@microsoft.com> so we can use your feedback tooplan as we expand.</span></span>

<span data-ttu-id="82de6-115">Vi arbetar med vissa ytterligare funktioner som förbättrar hello svarstid för klienter som för närvarande finns långa fördröjningar och vara säker på att tooshare dokumentation när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="82de6-115">We are working on some additional capabilities that help improve hello latency for tenants that currently see long latencies, and be sure tooshare documentation once available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="82de6-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82de6-116">Next steps</span></span>
[<span data-ttu-id="82de6-117">Arbeta med befintliga lokala proxyservrar</span><span class="sxs-lookup"><span data-stu-id="82de6-117">Work with existing on-premises proxy servers</span></span>](application-proxy-working-with-proxy-servers.md)
