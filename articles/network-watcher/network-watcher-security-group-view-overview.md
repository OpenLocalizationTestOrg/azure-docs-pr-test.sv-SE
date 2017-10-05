---
title: "Introduktion till säkerhetsvyn grupp i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över Nätverksbevakaren säkerhet visa kapaciteten"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 2c581a2d152a6d3f16de8f249e27a426aa9f844f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-network-security-group-view-in-azure-network-watcher"></a><span data-ttu-id="49625-103">Introduktion till vyn av grupp för nätverk i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="49625-103">Introduction to network security group view in Azure Network Watcher</span></span>

<span data-ttu-id="49625-104">Nätverkssäkerhetsgrupper associeras på en undernätverksnivå eller på en NIC-nivå.</span><span class="sxs-lookup"><span data-stu-id="49625-104">Network Security groups are associated at a subnet level or at a NIC level.</span></span> <span data-ttu-id="49625-105">Som är associerad till en undernätverksnivå, gäller alla VM-instanser i undernätet.</span><span class="sxs-lookup"><span data-stu-id="49625-105">When associated at a subnet level, it applies to all the VM instances in the subnet.</span></span> <span data-ttu-id="49625-106">Nätverkssäkerhetsgruppen vyn returnerar alla konfigurerade NSG: er och regler som är associerade på ett nätverkskort och undernät nivå för en virtuell dator som ger inblick i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="49625-106">Network Security Group view returns all the configured NSGs and rules that are associated at a NIC and subnet level for a virtual machine providing insight into the configuration.</span></span> <span data-ttu-id="49625-107">Dessutom returneras effektiva säkerhetsregler för varje nätverkskort i en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="49625-107">In addition, the effective security rules are returned for each of the NICs in a VM.</span></span> <span data-ttu-id="49625-108">Med hjälp av Nätverkssäkerhetsgruppen vyn du utvärdera en virtuell dator för säkerhetsrisker i nätverket, till exempel öppna portar.</span><span class="sxs-lookup"><span data-stu-id="49625-108">Using Network Security Group view, you can assess a VM for network vulnerabilities such as open ports.</span></span> <span data-ttu-id="49625-109">Du kan också verifiera om säkerhetsgrupp för nätverk fungerar som förväntat baserat på en [jämförelse mellan den konfigurerade och effektiva säkerhetsregler](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="49625-109">You can also validate if your Network Security Group is working as expected based on a [comparison between the configured and the effective security rules](network-watcher-nsg-auditing-powershell.md).</span></span>

<span data-ttu-id="49625-110">Det är ett mer utökade användningsfall i säkerhetsefterlevnaden och granskning.</span><span class="sxs-lookup"><span data-stu-id="49625-110">A more extended use case is in security compliance and auditing.</span></span> <span data-ttu-id="49625-111">Du kan definiera en normativ säkerhetsregler som en modell för styrning av säkerhet i din organisation.</span><span class="sxs-lookup"><span data-stu-id="49625-111">You can define a prescriptive set of security rules as a model for security governance in your organization.</span></span> <span data-ttu-id="49625-112">En periodisk efterlevnad audit kan implementeras på programmässiga sätt genom att jämföra normativ regler med de effektiva reglerna för var och en av de virtuella datorerna i nätverket.</span><span class="sxs-lookup"><span data-stu-id="49625-112">A periodic compliance audit can be implemented in a programmatic way by comparing the prescriptive rules with the effective rules for each of the VMs in your network.</span></span>

<span data-ttu-id="49625-113">I portalen regler är indelade efter gällande, undernät, nätverksgränssnittet och standard.</span><span class="sxs-lookup"><span data-stu-id="49625-113">In the portal rules are divided by Effective, Subnet, Network Interface, and Default.</span></span> <span data-ttu-id="49625-114">Detta ger en enkel vy i reglerna som gäller för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="49625-114">This provides a simple view into the rules applied to a virtual machine.</span></span> <span data-ttu-id="49625-115">En knapp för hämtning har angetts för enkelt hämta säkerhetsregler oavsett fliken till en CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="49625-115">A download button is provided to easily download all the security rules no matter the tab into a CSV file.</span></span>

![gruppvy för säkerhet][1]

<span data-ttu-id="49625-117">Regler kan väljas och ett nytt blad öppnas och visar Nätverkssäkerhetsgruppen och käll- och prefix.</span><span class="sxs-lookup"><span data-stu-id="49625-117">Rules can be selected and a new blade opens up to show the Network Security Group and source and destination prefixes.</span></span> <span data-ttu-id="49625-118">Du kan gå direkt till Nätverkssäkerhetsgruppen resursen från det här bladet.</span><span class="sxs-lookup"><span data-stu-id="49625-118">From this blade you can navigate directly to the Network Security Group resource.</span></span>

![nedbrytning][2]

### <a name="next-steps"></a><span data-ttu-id="49625-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49625-120">Next steps</span></span>

<span data-ttu-id="49625-121">Lär dig hur du granskningsinställningar säkerhetsgrupp för nätverk genom att besöka [gransknings-och Nätverkssäkerhetsgruppen inställningar med PowerShell](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="49625-121">Learn how to audit your Network Security Group settings by visiting [Audit Network Security Group settings with PowerShell](network-watcher-nsg-auditing-powershell.md)</span></span>

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









