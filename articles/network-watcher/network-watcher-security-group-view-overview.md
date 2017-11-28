---
title: "aaaIntroduction toosecurity gruppvyn i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Nätverksbevakaren säkerhet visa kapaciteten"
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
ms.openlocfilehash: c2f6dbbffd0aedbb9db4b69d1758f2e66dd7abb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toonetwork-security-group-view-in-azure-network-watcher"></a><span data-ttu-id="3c82b-103">Vyn av grupp om introduktion toonetwork i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="3c82b-103">Introduction toonetwork security group view in Azure Network Watcher</span></span>

<span data-ttu-id="3c82b-104">Nätverkssäkerhetsgrupper associeras på en undernätverksnivå eller på en NIC-nivå.</span><span class="sxs-lookup"><span data-stu-id="3c82b-104">Network Security groups are associated at a subnet level or at a NIC level.</span></span> <span data-ttu-id="3c82b-105">När associerade på en undernätverksnivå, tillämpas tooall hello VM-instanser i hello undernät.</span><span class="sxs-lookup"><span data-stu-id="3c82b-105">When associated at a subnet level, it applies tooall hello VM instances in hello subnet.</span></span> <span data-ttu-id="3c82b-106">Nätverkssäkerhetsgruppen vyn returnerar alla hello konfigurerats NSG: er och regler som är associerade på ett nätverkskort och undernät nivå för en virtuell dator som ger inblick i hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3c82b-106">Network Security Group view returns all hello configured NSGs and rules that are associated at a NIC and subnet level for a virtual machine providing insight into hello configuration.</span></span> <span data-ttu-id="3c82b-107">Dessutom returneras hello effektiva säkerhetsregler för varje hello nätverkskort i en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3c82b-107">In addition, hello effective security rules are returned for each of hello NICs in a VM.</span></span> <span data-ttu-id="3c82b-108">Med hjälp av Nätverkssäkerhetsgruppen vyn du utvärdera en virtuell dator för säkerhetsrisker i nätverket, till exempel öppna portar.</span><span class="sxs-lookup"><span data-stu-id="3c82b-108">Using Network Security Group view, you can assess a VM for network vulnerabilities such as open ports.</span></span> <span data-ttu-id="3c82b-109">Du kan också verifiera om säkerhetsgrupp för nätverk fungerar som förväntat baserat på en [jämförelse mellan hello konfigurerad och hello effektiva säkerhetsregler](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="3c82b-109">You can also validate if your Network Security Group is working as expected based on a [comparison between hello configured and hello effective security rules](network-watcher-nsg-auditing-powershell.md).</span></span>

<span data-ttu-id="3c82b-110">Det är ett mer utökade användningsfall i säkerhetsefterlevnaden och granskning.</span><span class="sxs-lookup"><span data-stu-id="3c82b-110">A more extended use case is in security compliance and auditing.</span></span> <span data-ttu-id="3c82b-111">Du kan definiera en normativ säkerhetsregler som en modell för styrning av säkerhet i din organisation.</span><span class="sxs-lookup"><span data-stu-id="3c82b-111">You can define a prescriptive set of security rules as a model for security governance in your organization.</span></span> <span data-ttu-id="3c82b-112">En periodisk efterlevnad audit kan implementeras på programmässiga sätt genom att jämföra hello normativ regler med hello effektiva regler för varje hello virtuella datorer i nätverket.</span><span class="sxs-lookup"><span data-stu-id="3c82b-112">A periodic compliance audit can be implemented in a programmatic way by comparing hello prescriptive rules with hello effective rules for each of hello VMs in your network.</span></span>

<span data-ttu-id="3c82b-113">I hello delas portal regler som gäller, undernät, nätverksgränssnittet och standard.</span><span class="sxs-lookup"><span data-stu-id="3c82b-113">In hello portal rules are divided by Effective, Subnet, Network Interface, and Default.</span></span> <span data-ttu-id="3c82b-114">Detta ger en enkel vy till hello reglerna tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3c82b-114">This provides a simple view into hello rules applied tooa virtual machine.</span></span> <span data-ttu-id="3c82b-115">En knapp för hämtning tillhandahålls tooeasily hämta alla hello säkerhetsregler oavsett hello fliken till en CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="3c82b-115">A download button is provided tooeasily download all hello security rules no matter hello tab into a CSV file.</span></span>

![gruppvy för säkerhet][1]

<span data-ttu-id="3c82b-117">Regler kan väljas och ett nytt blad öppnas tooshow hello Nätverkssäkerhetsgruppen och käll- och prefix.</span><span class="sxs-lookup"><span data-stu-id="3c82b-117">Rules can be selected and a new blade opens up tooshow hello Network Security Group and source and destination prefixes.</span></span> <span data-ttu-id="3c82b-118">Från det här bladet kan du gå direkt toohello Nätverkssäkerhetsgruppen resurs.</span><span class="sxs-lookup"><span data-stu-id="3c82b-118">From this blade you can navigate directly toohello Network Security Group resource.</span></span>

![nedbrytning][2]

### <a name="next-steps"></a><span data-ttu-id="3c82b-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c82b-120">Next steps</span></span>

<span data-ttu-id="3c82b-121">Lär dig hur tooaudit nätverkssäkerheten grupp inställningar genom att besöka [Audit Nätverkssäkerhetsgruppen inställningar med PowerShell](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="3c82b-121">Learn how tooaudit your Network Security Group settings by visiting [Audit Network Security Group settings with PowerShell](network-watcher-nsg-auditing-powershell.md)</span></span>

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









