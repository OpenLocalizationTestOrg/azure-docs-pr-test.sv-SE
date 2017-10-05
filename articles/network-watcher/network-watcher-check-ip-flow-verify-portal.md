---
title: "Kontrollera trafik med Azure Network Watcher IP flödet verifiera - Azure-portalen | Microsoft Docs"
description: "Den här artikeln beskrivs hur du kontrollerar om trafik till eller från en virtuell dator tillåts eller nekas"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e0e3e9a8-70eb-409a-a744-0ce9deb27148
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 7db29c186cf6e6f3b40a680ab76f1d2763f806ba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="80b77-103">Kontrollera om trafik tillåts eller nekas till eller från en virtuell dator med IP-flöda Kontrollera en komponent i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="80b77-103">Check if traffic is allowed or denied to or from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="80b77-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="80b77-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="80b77-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="80b77-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="80b77-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="80b77-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="80b77-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="80b77-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="80b77-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="80b77-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="80b77-109">IP-flöde verifiera är en funktion i Nätverksbevakaren som hjälper dig att kontrollera om tillåts trafik till eller från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="80b77-109">IP flow verify is a feature of Network Watcher that allows you to verify if traffic is allowed to or from a virtual machine.</span></span> <span data-ttu-id="80b77-110">Verifieringen kan köras för inkommande eller utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="80b77-110">The validation can be run for incoming or outgoing traffic.</span></span> <span data-ttu-id="80b77-111">Det här scenariot är användbart för att hämta aktuella tillstånd om en virtuell dator kan kommunicera med en extern resurs eller en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="80b77-111">This scenario is useful to get a current state of whether a virtual machine can talk to an external resource or another resource.</span></span> <span data-ttu-id="80b77-112">IP-flöde Kontrollera kan användas för att kontrollera om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="80b77-112">IP flow verify can be used to verify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="80b77-113">En annan orsak till att använda IP flödet Kontrollera är att kontrollera att trafik som du vill blockerade blockeras korrekt av NSG: N.</span><span class="sxs-lookup"><span data-stu-id="80b77-113">Another reason for using IP flow verify is to ensure traffic that you want blocked is being blocked properly by the NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="80b77-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="80b77-114">Before you begin</span></span>

<span data-ttu-id="80b77-115">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren eller har en befintlig instans av Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="80b77-115">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="80b77-116">Det här scenariot förutsätter att det finns en resursgrupp med en giltig virtuell dator som ska användas.</span><span class="sxs-lookup"><span data-stu-id="80b77-116">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="80b77-117">Scenario</span><span class="sxs-lookup"><span data-stu-id="80b77-117">Scenario</span></span>

<span data-ttu-id="80b77-118">Det här scenariot använder IP-flöda Kontrollera för att verifiera om en virtuell dator kan du kontakta en annan dator via port 443.</span><span class="sxs-lookup"><span data-stu-id="80b77-118">This scenario uses IP Flow Verify to verify if a virtual machine can talk to another machine over port 443.</span></span> <span data-ttu-id="80b77-119">Om trafiken nekas returnerar säkerhetsregeln som nekar trafiken.</span><span class="sxs-lookup"><span data-stu-id="80b77-119">If the traffic is denied, it returns the security rule that is denying that traffic.</span></span> <span data-ttu-id="80b77-120">Läs mer om IP-flöda Kontrollera [flöda Kontrollera översikt över IP](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="80b77-120">To learn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

### <a name="run-ip-flow-verify"></a><span data-ttu-id="80b77-121">Kontrollera kör IP-flöde</span><span class="sxs-lookup"><span data-stu-id="80b77-121">Run IP flow verify</span></span>

<span data-ttu-id="80b77-122">Navigera till din Nätverksbevakaren och på **IP-flöde Kontrollera**.</span><span class="sxs-lookup"><span data-stu-id="80b77-122">Navigate to your Network Watcher and click **IP flow verify**.</span></span> <span data-ttu-id="80b77-123">Välj den virtuella datorn och nätverksgränssnittet som du vill kontrollera trafik från.</span><span class="sxs-lookup"><span data-stu-id="80b77-123">Select the virtual machine and network interface you want to verify traffic from.</span></span> <span data-ttu-id="80b77-124">Ange ytterligare filtrering information och klickar på **Kontrollera**.</span><span class="sxs-lookup"><span data-stu-id="80b77-124">Enter any additional filtering information and click **Check**.</span></span>

<span data-ttu-id="80b77-125">När du klickar på **Kontrollera**, flödet baserat på kriterier som du angav är markerad.</span><span class="sxs-lookup"><span data-stu-id="80b77-125">Once you click **Check**, the flow based on the criteria you provided is checked.</span></span> <span data-ttu-id="80b77-126">Resultatet är antingen **åtkomst tillåts** eller **åtkomst nekad**.</span><span class="sxs-lookup"><span data-stu-id="80b77-126">The result is either **Access allowed** or **Access denied**.</span></span> <span data-ttu-id="80b77-127">Om åtkomst nekas tillhandahålls Nätverkssäkerhetsgrupp (NSG) och säkerhet regel som blockerar trafik.</span><span class="sxs-lookup"><span data-stu-id="80b77-127">If access is denied, the Network Security Group (NSG) and security rule that block traffic is provided.</span></span> <span data-ttu-id="80b77-128">Om DOS-trafik är förväntat, lyckades regeln.</span><span class="sxs-lookup"><span data-stu-id="80b77-128">If the denial of traffic is expected behavior, then the rule was successful.</span></span>

> [!NOTE]
> <span data-ttu-id="80b77-129">IP-flöde Kontrollera kräver att den Virtuella datorresursen allokeras.</span><span class="sxs-lookup"><span data-stu-id="80b77-129">IP flow verify requires that the VM resource is allocated.</span></span>

<span data-ttu-id="80b77-130">Som du ser i följande bild, tilläts utgående HTTPS-trafiken.</span><span class="sxs-lookup"><span data-stu-id="80b77-130">As you can see from the following image, the outbound HTTPS traffic was allowed.</span></span>

![IP-flöde Kontrollera översikt][1]

<span data-ttu-id="80b77-132">Som det visas i följande bild, trafik ändras till inkommande och den inkommande porten ändras till 123.</span><span class="sxs-lookup"><span data-stu-id="80b77-132">As seen in the following image, traffic is changed to inbound and the inbound port changed to 123.</span></span> <span data-ttu-id="80b77-133">Trafik nu nekas meddelandet ”åtkomst nekad” har angetts tillsammans med gruppen och säkerhet nätverkssäkerhetsregeln som nekar trafik.</span><span class="sxs-lookup"><span data-stu-id="80b77-133">Traffic is now denied, the message "Access denied" is provided along with the network security group and security rule that deny the traffic.</span></span>

![IP-flöde resultat][2]

## <a name="next-steps"></a><span data-ttu-id="80b77-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="80b77-135">Next steps</span></span>

<span data-ttu-id="80b77-136">Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) att spåra de grupp och säkerhet Nätverkssäkerhetsregler som har definierats.</span><span class="sxs-lookup"><span data-stu-id="80b77-136">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that are defined.</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













