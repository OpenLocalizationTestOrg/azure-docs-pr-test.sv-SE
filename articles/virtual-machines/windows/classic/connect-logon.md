---
title: "Logga in på en klassisk Azure-VM | Microsoft Docs"
description: "Använda Azure portal för att logga in på en Windows-dator som skapats med den klassiska distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 43d54de7e875de9212c23c49ad0539bf2272a312
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-portal"></a><span data-ttu-id="23e91-103">Logga in på en virtuell Windows-dator med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="23e91-103">Log on to a Windows virtual machine using the Azure portal</span></span>
<span data-ttu-id="23e91-104">I Azure portal ska du använda den **Anslut** för att starta en fjärrskrivbordssession och logga in på en virtuell Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="23e91-104">In the Azure portal, you use the **Connect** button to start a Remote Desktop session and log on to a Windows VM.</span></span>

<span data-ttu-id="23e91-105">Vill du ansluta till en Linux-VM?</span><span class="sxs-lookup"><span data-stu-id="23e91-105">Do you want to connect to a Linux VM?</span></span> <span data-ttu-id="23e91-106">Se [så att logga in på en virtuell dator som kör Linux](../../linux/mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="23e91-106">See [How to log on to a virtual machine running Linux](../../linux/mac-create-ssh-keys.md).</span></span>

<!--
Deleting, but not 100% sure
Learn how to [perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> <span data-ttu-id="23e91-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="23e91-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="23e91-108">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="23e91-108">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="23e91-109">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="23e91-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="23e91-110">Information om hur du loggar in på en virtuell dator med hjälp av Resource Manager-modellen finns [här](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="23e91-110">For information about how to log on to a VM using the Resource Manager model, see [here](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="connect-to-the-virtual-machine"></a><span data-ttu-id="23e91-111">Ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="23e91-111">Connect to the virtual machine</span></span>
1. <span data-ttu-id="23e91-112">Logga in på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="23e91-112">Sign in to the Azure portal.</span></span>
2. <span data-ttu-id="23e91-113">Klicka på den virtuella dator som du vill komma åt.</span><span class="sxs-lookup"><span data-stu-id="23e91-113">Click on the virtual machine that you want to access.</span></span> <span data-ttu-id="23e91-114">Namnet visas i den **alla resurser** fönstret.</span><span class="sxs-lookup"><span data-stu-id="23e91-114">The name is listed in the **All resources** pane.</span></span>

    ![Virtuella-machine-platser](./media/connect-logon/azureportaldashboard.png)

3. <span data-ttu-id="23e91-116">Klicka på **Anslut** i kommandofältet på instrumentpanelen för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="23e91-116">Click **Connect** on the command bar atop the virtual machine dashboard.</span></span>

    ![Ansluta ikonen för den virtuella datorn](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If the **Connect** button isn't available, see the troubleshooting tips at the end of this article.
>
>
-->

## <a name="log-on-to-the-virtual-machine"></a><span data-ttu-id="23e91-118">Logga in på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="23e91-118">Log on to the virtual machine</span></span>
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a><span data-ttu-id="23e91-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23e91-119">Next steps</span></span>
* <span data-ttu-id="23e91-120">Om den **Anslut** är inaktiverad eller du har andra problem med anslutningen till fjärrskrivbordet, pröva att återställa konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="23e91-120">If the **Connect** button is inactive or you are having other problems with the Remote Desktop connection, try resetting the configuration.</span></span> <span data-ttu-id="23e91-121">Klicka på **Återställ fjärråtkomst** från instrumentpanelen för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="23e91-121">click **Reset remote access** from the virtual machine dashboard.</span></span>

    ![Återställ fjärråtkomst](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* <span data-ttu-id="23e91-123">För problem med ditt lösenord, pröva att återställa den.</span><span class="sxs-lookup"><span data-stu-id="23e91-123">For problems with your password, try resetting it.</span></span> <span data-ttu-id="23e91-124">Klicka på **Återställ lösenord** längs vänster kant för den virtuella datorn instrumentpanelen under **stöd + felsökning**.</span><span class="sxs-lookup"><span data-stu-id="23e91-124">Click **Reset password** along the left edge of virtual machine dashboard, under **Support + Troubleshooting**.</span></span>

    ![Återställ lösenord](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

<span data-ttu-id="23e91-126">Om dessa tips fungerar inte eller inte är vad du behöver kan du se [felsökning av anslutning till fjärrskrivbord till en Windows-baserad Azure-dator](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="23e91-126">If those tips don't work or aren't what you need, see [Troubleshoot Remote Desktop connections to a Windows-based Azure Virtual Machine](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="23e91-127">Den här artikeln beskriver hur du diagnostiserar och löser vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="23e91-127">This article walks you through diagnosing and resolving common problems.</span></span>
