---
title: Kontrollera aaaVerify trafik med Azure Network Watcher IP - Azure-portalen | Microsoft Docs
description: "Den här artikeln beskriver hur toocheck om trafik tooor från en virtuell dator tillåts eller nekas"
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
ms.openlocfilehash: abf639f36d32f3416dd927e66b635267b746e62f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="92d72-103">Kontrollera om trafik som tillåts eller nekas tooor från en virtuell dator med IP-flöda Kontrollera en komponent i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="92d72-103">Check if traffic is allowed or denied tooor from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="92d72-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="92d72-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="92d72-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="92d72-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="92d72-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="92d72-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="92d72-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="92d72-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="92d72-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="92d72-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="92d72-109">IP-flöde verifiera är en funktion i Nätverksbevakaren som gör att du tooverify om trafik tillåts tooor från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="92d72-109">IP flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="92d72-110">hello verifiering kan köras för inkommande eller utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="92d72-110">hello validation can be run for incoming or outgoing traffic.</span></span> <span data-ttu-id="92d72-111">Det här scenariot är användbart tooget aktuella tillstånd om en virtuell dator kan prata tooan extern resurs eller en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="92d72-111">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or another resource.</span></span> <span data-ttu-id="92d72-112">IP-flöde verifiera går att använda tooverify om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="92d72-112">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="92d72-113">En annan orsak till att använda IP flödet Kontrollera tooensure trafik som du vill blockerade blockeras korrekt av hello NSG.</span><span class="sxs-lookup"><span data-stu-id="92d72-113">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="92d72-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="92d72-114">Before you begin</span></span>

<span data-ttu-id="92d72-115">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren eller har en befintlig instans av Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="92d72-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="92d72-116">hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.</span><span class="sxs-lookup"><span data-stu-id="92d72-116">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="92d72-117">Scenario</span><span class="sxs-lookup"><span data-stu-id="92d72-117">Scenario</span></span>

<span data-ttu-id="92d72-118">Det här scenariot använder IP-flöda Kontrollera tooverify om en virtuell dator kan prata tooanother datorn via port 443.</span><span class="sxs-lookup"><span data-stu-id="92d72-118">This scenario uses IP Flow Verify tooverify if a virtual machine can talk tooanother machine over port 443.</span></span> <span data-ttu-id="92d72-119">Om hello trafik nekas returnerar hello säkerhetsregel som nekar trafiken.</span><span class="sxs-lookup"><span data-stu-id="92d72-119">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="92d72-120">toolearn mer om IP-flöda Kontrollera finns [flöda Kontrollera översikt över IP](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="92d72-120">toolearn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

### <a name="run-ip-flow-verify"></a><span data-ttu-id="92d72-121">Kontrollera kör IP-flöde</span><span class="sxs-lookup"><span data-stu-id="92d72-121">Run IP flow verify</span></span>

<span data-ttu-id="92d72-122">Navigera tooyour Nätverksbevakaren och klicka på **IP-flöde Kontrollera**.</span><span class="sxs-lookup"><span data-stu-id="92d72-122">Navigate tooyour Network Watcher and click **IP flow verify**.</span></span> <span data-ttu-id="92d72-123">Välj hello virtuell dator och gränssnitt som du vill tooverify trafik från.</span><span class="sxs-lookup"><span data-stu-id="92d72-123">Select hello virtual machine and network interface you want tooverify traffic from.</span></span> <span data-ttu-id="92d72-124">Ange ytterligare filtrering information och klickar på **Kontrollera**.</span><span class="sxs-lookup"><span data-stu-id="92d72-124">Enter any additional filtering information and click **Check**.</span></span>

<span data-ttu-id="92d72-125">När du klickar på **Kontrollera**, hello flödet baserat på hello kriterier som du angav är markerad.</span><span class="sxs-lookup"><span data-stu-id="92d72-125">Once you click **Check**, hello flow based on hello criteria you provided is checked.</span></span> <span data-ttu-id="92d72-126">hello resultatet är antingen **åtkomst tillåts** eller **åtkomst nekad**.</span><span class="sxs-lookup"><span data-stu-id="92d72-126">hello result is either **Access allowed** or **Access denied**.</span></span> <span data-ttu-id="92d72-127">Om åtkomst nekas hello Nätverkssäkerhetsgrupp (NSG) och säkerhetsregeln som blockerar trafik tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="92d72-127">If access is denied, hello Network Security Group (NSG) and security rule that block traffic is provided.</span></span> <span data-ttu-id="92d72-128">Om hello DOS-trafik är förväntat, lyckades hello regeln.</span><span class="sxs-lookup"><span data-stu-id="92d72-128">If hello denial of traffic is expected behavior, then hello rule was successful.</span></span>

> [!NOTE]
> <span data-ttu-id="92d72-129">IP-flöde Kontrollera kräver att hello Virtuella datorresursen allokeras.</span><span class="sxs-lookup"><span data-stu-id="92d72-129">IP flow verify requires that hello VM resource is allocated.</span></span>

<span data-ttu-id="92d72-130">Som du ser i följande bild hello tilläts hello utgående HTTPS-trafik.</span><span class="sxs-lookup"><span data-stu-id="92d72-130">As you can see from hello following image, hello outbound HTTPS traffic was allowed.</span></span>

![IP-flöde Kontrollera översikt][1]

<span data-ttu-id="92d72-132">Som det visas i följande bild hello trafik ändras tooinbound och hello inkommande port ändras too123.</span><span class="sxs-lookup"><span data-stu-id="92d72-132">As seen in hello following image, traffic is changed tooinbound and hello inbound port changed too123.</span></span> <span data-ttu-id="92d72-133">Trafik nekas nu har hello-meddelande ”åtkomst nekad” angetts tillsammans med hello grupp och säkerhet nätverkssäkerhetsregeln som nekar hello trafik.</span><span class="sxs-lookup"><span data-stu-id="92d72-133">Traffic is now denied, hello message "Access denied" is provided along with hello network security group and security rule that deny hello traffic.</span></span>

![IP-flöde resultat][2]

## <a name="next-steps"></a><span data-ttu-id="92d72-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="92d72-135">Next steps</span></span>

<span data-ttu-id="92d72-136">Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello network security grupp och säkerhet regler som har definierats.</span><span class="sxs-lookup"><span data-stu-id="92d72-136">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













