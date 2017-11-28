---
title: "aaaCreate en Internetriktade belastningsutjämnare - Azure PowerShell klassiska | Microsoft Docs"
description: "Lär dig hur toocreate Internet-riktade belastningsutjämnare i klassiskt läge med hjälp av PowerShell"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 73e8bfa4-8086-4ef0-9e35-9e00b24be319
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 76d9a712a0acda223fc86b80be9c35c0ed9f3a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a><span data-ttu-id="928f8-103">Komma igång med att skapa en Internetuppkopplad belastningsutjämnare (klassisk) i PowerShell</span><span class="sxs-lookup"><span data-stu-id="928f8-103">Get started creating an Internet facing load balancer (classic) in PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="928f8-104">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="928f8-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="928f8-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="928f8-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="928f8-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="928f8-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="928f8-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="928f8-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="928f8-108">Innan du börjar arbeta med Azure-resurser, är det viktigt toounderstand att Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="928f8-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="928f8-109">Se till att du förstår [distributionsmodeller och verktyg](../azure-classic-rm.md) innan du börjar arbeta med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="928f8-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="928f8-110">Du kan visa hello dokumentationen för olika verktyg genom att klicka på flikarna hello hello överst i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="928f8-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="928f8-111">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="928f8-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="928f8-112">Du kan också [Lär dig hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="928f8-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a><span data-ttu-id="928f8-113">Konfigurera en belastningsutjämnare med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="928f8-113">Set up load balancer using PowerShell</span></span>

<span data-ttu-id="928f8-114">tooset in belastningsutjämning med hjälp av powershell så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="928f8-114">tooset up a load balancer using powershell, follow hello steps below:</span></span>

1. <span data-ttu-id="928f8-115">Om du aldrig har använt Azure PowerShell, se [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) och följer instruktionerna för hello alla hello sätt toohello avslutas toosign till Azure och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="928f8-115">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="928f8-116">När du har skapat en virtuell dator kan du använda PowerShell-cmdlets tooadd en load balancer tooa virtuell dator i hello samma molntjänst.</span><span class="sxs-lookup"><span data-stu-id="928f8-116">After creating a virtual machine, you can use PowerShell cmdlets tooadd a load balancer tooa virtual machine within hello same cloud service.</span></span>

<span data-ttu-id="928f8-117">I hello följande exempel lägger du till en belastningen belastningsutjämnaren uppsättning kallas ”webbservergrupp” toocloud service ”mytestcloud” (eller myctestcloud.cloudapp.net), lägger till hello slutpunkter för hello läsa in belastningsutjämning toovirtual datorer kallas ”web1” och ”web2”.</span><span class="sxs-lookup"><span data-stu-id="928f8-117">In hello following example you will add a load balancer set called "webfarm" toocloud service "mytestcloud" (or myctestcloud.cloudapp.net) , adding hello endpoints for hello load balancer toovirtual machines named "web1" and "web2".</span></span> <span data-ttu-id="928f8-118">hello belastningsutjämnaren tar emot trafik på port 80 och belastningsutjämning mellan hello virtuella datorer som definieras av hello lokal slutpunkt (i det här fallet port 80) med TCP.</span><span class="sxs-lookup"><span data-stu-id="928f8-118">hello load balancer receives network traffic on port 80 and load balances between hello virtual machines defined by hello local endpoint (in this case port 80) using TCP.</span></span>

### <a name="step-1"></a><span data-ttu-id="928f8-119">Steg 1</span><span class="sxs-lookup"><span data-stu-id="928f8-119">Step 1</span></span>

<span data-ttu-id="928f8-120">Skapa en belastningsutjämnad slutpunkt för web1 ”hello första VM”</span><span class="sxs-lookup"><span data-stu-id="928f8-120">Create a load balanced endpoint for hello first VM "web1"</span></span>

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

### <a name="step-2"></a><span data-ttu-id="928f8-121">Steg 2</span><span class="sxs-lookup"><span data-stu-id="928f8-121">Step 2</span></span>

<span data-ttu-id="928f8-122">Skapa en annan slutpunkt för hello andra VM ”web2” med hjälp av hello samma läsa in belastningsutjämning namn</span><span class="sxs-lookup"><span data-stu-id="928f8-122">Create another endpoint for hello second VM  "web2" using hello same load balancer set name</span></span>

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a><span data-ttu-id="928f8-123">Ta bort en virtuell dator från en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="928f8-123">Remove a virtual machine from a load balancer</span></span>

<span data-ttu-id="928f8-124">Du kan använda Remove-AzureEndpoint tooremove virtuell datorslutpunkt från hello belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="928f8-124">You can use Remove-AzureEndpoint tooremove a virtual machine endpoint from hello load balancer</span></span>

```powershell
Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin | Update-AzureVM
```

## <a name="next-steps"></a><span data-ttu-id="928f8-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="928f8-125">Next steps</span></span>

<span data-ttu-id="928f8-126">Du kan också läsa mer om hur du [kommer igång med att skapa en intern belastningsutjämnare](load-balancer-get-started-ilb-classic-ps.md) och hur du konfigurerar typen av [distributionsläge](load-balancer-distribution-mode.md) för ett specifikt trafikbeteende i ett belastningsutjämnat nätverk.</span><span class="sxs-lookup"><span data-stu-id="928f8-126">You can also [get started creating an internal load balancer](load-balancer-get-started-ilb-classic-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for a specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="928f8-127">Om ditt program måste tookeep anslutningar alive för servrar bakom en belastningsutjämnare, du veta mer om [inaktiv TCP timeout-inställningar för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="928f8-127">If your application needs tookeep connections alive for servers behind a load balancer, you can understand more about [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="928f8-128">Det hjälper toolearn om inaktiv anslutning beteende när du använder Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="928f8-128">It will help toolearn about idle connection behavior when you are using Azure Load Balancer.</span></span>
