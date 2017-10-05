---
title: Ansluta till en virtuell Windows Server-dator | Microsoft Docs
description: "Lär dig hur du ansluter till och loggar in på en virtuell Windows-dator med hjälp av Azure Portal och Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: 88431377a36d5bc36220c630f0c8d4a46ab4a434
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a><span data-ttu-id="8481a-103">Ansluta till och logga in på en virtuell Azure-dator som kör Windows</span><span class="sxs-lookup"><span data-stu-id="8481a-103">How to connect and log on to an Azure virtual machine running Windows</span></span>
<span data-ttu-id="8481a-104">Du använder knappen **Anslut** på Azure Portal för att starta en fjärrskrivbordssession (RDP) från ett Windows-skrivbord.</span><span class="sxs-lookup"><span data-stu-id="8481a-104">You'll use the **Connect** button in the Azure portal to start a Remote Desktop (RDP) session from a Windows desktop.</span></span> <span data-ttu-id="8481a-105">Du börjar med att ansluta till den virtuella datorn och loggar sedan in.</span><span class="sxs-lookup"><span data-stu-id="8481a-105">First you connect to the virtual machine, then you log on.</span></span>

<span data-ttu-id="8481a-106">Om du försöker ansluta till en virtuell Windows-dator från en Mac måste du installera en RDP-klient för Mac som [Microsoft Remote Desktop](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).</span><span class="sxs-lookup"><span data-stu-id="8481a-106">If you are trying to connect to a Windows VM from a Mac, you need to install an RDP client for Mac like [Microsoft Remote Desktop](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).</span></span>

## <a name="connect-to-the-virtual-machine"></a><span data-ttu-id="8481a-107">Ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="8481a-107">Connect to the virtual machine</span></span>
1. <span data-ttu-id="8481a-108">Om du inte redan gjort det loggar du in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8481a-108">If you haven't already done so, sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="8481a-109">Klicka på **Virtual Machines** på navmenyn.</span><span class="sxs-lookup"><span data-stu-id="8481a-109">On the Hub menu, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="8481a-110">Välj den virtuella datorn i listan.</span><span class="sxs-lookup"><span data-stu-id="8481a-110">Select the virtual machine from the list.</span></span>
4. <span data-ttu-id="8481a-111">Klicka på **Anslut** i bladet för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8481a-111">On the blade for the virtual machine, click **Connect**.</span></span>
   
    ![Skärmbild av Azure Portal som visar hur du ansluter till den virtuella datorn.](./media/connect-logon/connect.png)
   
   > [!TIP]
   > <span data-ttu-id="8481a-113">Om knappen **Anslut** på portalen är nedtonad och du inte är ansluten till Azure via [Express Route](../../expressroute/expressroute-introduction.md) eller en [VPN-anslutning för plats till plats](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) måste du skapa och tilldela den virtuella datorn en offentlig IP-adress innan du kan använda RDP.</span><span class="sxs-lookup"><span data-stu-id="8481a-113">If the **Connect** button in the portal is greyed out and you are not connected to Azure via an [Express Route](../../expressroute/expressroute-introduction.md) or [Site-to-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) connection, you need to create and assign your VM a public IP address before you can use RDP.</span></span> <span data-ttu-id="8481a-114">Du kan läsa mer om [offentliga IP-adresser i Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="8481a-114">You can read more about [public IP addresses in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span></span>
   > 
   > 

## <a name="log-on-to-the-virtual-machine"></a><span data-ttu-id="8481a-115">Logga in på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="8481a-115">Log on to the virtual machine</span></span>
[!INCLUDE [virtual-machines-log-on-win-server](../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a><span data-ttu-id="8481a-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8481a-116">Next steps</span></span>
<span data-ttu-id="8481a-117">Om du får problem när du försöker ansluta läser du [Felsöka anslutningar till fjärrskrivbord](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8481a-117">If you run into trouble when you try to connect, see [Troubleshoot Remote Desktop connections](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="8481a-118">Den här artikeln beskriver hur du diagnostiserar och löser vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="8481a-118">This article walks you through diagnosing and resolving common problems.</span></span>

