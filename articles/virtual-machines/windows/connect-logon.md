---
title: aaaConnect tooa Windows Server VM | Microsoft Docs
description: "Lär dig hur tooconnect och logga in tooa Windows VM med hello Azure-portalen och hello Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/01/2017
ms.author: cynthn
ms.openlocfilehash: dc397f435ef165ae5af09f1d037ad3d520bb7ac3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconnect-and-log-on-tooan-azure-virtual-machine-running-windows"></a><span data-ttu-id="e6558-103">Hur tooconnect och logga in tooan Azure virtuella datorn kör Windows</span><span class="sxs-lookup"><span data-stu-id="e6558-103">How tooconnect and log on tooan Azure virtual machine running Windows</span></span>
<span data-ttu-id="e6558-104">Du kommer att använda hello **Anslut** knapp i hello Azure portal toostart en session för fjärrskrivbord (RDP) från en Windows-skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="e6558-104">You'll use hello **Connect** button in hello Azure portal toostart a Remote Desktop (RDP) session from a Windows desktop.</span></span> <span data-ttu-id="e6558-105">Du ansluter toohello virtuell dator och du loggar in.</span><span class="sxs-lookup"><span data-stu-id="e6558-105">First you connect toohello virtual machine, then you log on.</span></span>

<span data-ttu-id="e6558-106">Om du försöker tooconnect tooa Windows virtuell dator från en Mac måste tooinstall som en RDP-klient för Mac [Microsoft Remote Desktop](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).</span><span class="sxs-lookup"><span data-stu-id="e6558-106">If you are trying tooconnect tooa Windows VM from a Mac, you need tooinstall an RDP client for Mac like [Microsoft Remote Desktop](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).</span></span>

## <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="e6558-107">Ansluta toohello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e6558-107">Connect toohello virtual machine</span></span>
1. <span data-ttu-id="e6558-108">Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e6558-108">If you haven't already done so, sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e6558-109">Hej hubbmenyn, klicka på **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="e6558-109">On hello Hub menu, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="e6558-110">Välj hello virtuella hello-listan.</span><span class="sxs-lookup"><span data-stu-id="e6558-110">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="e6558-111">Klicka på hello bladet för den virtuella datorn hello **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="e6558-111">On hello blade for hello virtual machine, click **Connect**.</span></span>
   
    ![Skärmbild av hello Azure portal som visar hur tooconnect tooyour VM.](./media/connect-logon/connect.png)
   
   > [!TIP]
   > <span data-ttu-id="e6558-113">Om hello **Anslut** knappen hello-portalen är nedtonad och du inte är ansluten tooAzure via en [Express Route](../../expressroute/expressroute-introduction.md) eller [plats-till-plats VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) anslutning, behöver du toocreate och tilldela den virtuella datorn en offentlig IP-adress innan du kan använda RDP.</span><span class="sxs-lookup"><span data-stu-id="e6558-113">If hello **Connect** button in hello portal is greyed out and you are not connected tooAzure via an [Express Route](../../expressroute/expressroute-introduction.md) or [Site-to-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) connection, you need toocreate and assign your VM a public IP address before you can use RDP.</span></span> <span data-ttu-id="e6558-114">Du kan läsa mer om [offentliga IP-adresser i Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="e6558-114">You can read more about [public IP addresses in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span></span>
   > 
   > 

## <a name="log-on-toohello-virtual-machine"></a><span data-ttu-id="e6558-115">Logga in toohello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e6558-115">Log on toohello virtual machine</span></span>
[!INCLUDE [virtual-machines-log-on-win-server](../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a><span data-ttu-id="e6558-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6558-116">Next steps</span></span>
<span data-ttu-id="e6558-117">Om du stöter på problem när du försöker tooconnect, se [felsöka fjärrskrivbordsanslutningar](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e6558-117">If you run into trouble when you try tooconnect, see [Troubleshoot Remote Desktop connections](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="e6558-118">Den här artikeln beskriver hur du diagnostiserar och löser vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="e6558-118">This article walks you through diagnosing and resolving common problems.</span></span>

