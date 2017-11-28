---
title: "aaaLog på tooa klassiska Azure-VM | Microsoft Docs"
description: "Använd hello Azure portal toolog på tooa Windows virtuell dator som skapats med hello klassiska distributionsmodellen."
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
ms.openlocfilehash: 2e32b7036c2538e73b46580e0f5f8f4979e8a685
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-on-tooa-windows-virtual-machine-using-hello-azure-portal"></a><span data-ttu-id="48bd7-103">Logga in tooa Windows virtuell dator med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="48bd7-103">Log on tooa Windows virtual machine using hello Azure portal</span></span>
<span data-ttu-id="48bd7-104">I hello Azure-portalen, använder du hello **Anslut** knappen toostart en fjärrskrivbordssession och logga in tooa Windows VM.</span><span class="sxs-lookup"><span data-stu-id="48bd7-104">In hello Azure portal, you use hello **Connect** button toostart a Remote Desktop session and log on tooa Windows VM.</span></span>

<span data-ttu-id="48bd7-105">Vill du tooconnect tooa Linux VM?</span><span class="sxs-lookup"><span data-stu-id="48bd7-105">Do you want tooconnect tooa Linux VM?</span></span> <span data-ttu-id="48bd7-106">Se [hur toolog på tooa virtuell dator som kör Linux](../../linux/mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="48bd7-106">See [How toolog on tooa virtual machine running Linux](../../linux/mac-create-ssh-keys.md).</span></span>

<!--
Deleting, but not 100% sure
Learn how too[perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> <span data-ttu-id="48bd7-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="48bd7-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="48bd7-108">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="48bd7-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="48bd7-109">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="48bd7-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="48bd7-110">Mer information om hur toolog tooa VM använda hello Resource Manager objektmodellen, se [här](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="48bd7-110">For information about how toolog on tooa VM using hello Resource Manager model, see [here](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="48bd7-111">Ansluta toohello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="48bd7-111">Connect toohello virtual machine</span></span>
1. <span data-ttu-id="48bd7-112">Logga in toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="48bd7-112">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="48bd7-113">Klicka på hello virtuell dator som du vill tooaccess.</span><span class="sxs-lookup"><span data-stu-id="48bd7-113">Click on hello virtual machine that you want tooaccess.</span></span> <span data-ttu-id="48bd7-114">hello namnet visas i hello **alla resurser** fönstret.</span><span class="sxs-lookup"><span data-stu-id="48bd7-114">hello name is listed in hello **All resources** pane.</span></span>

    ![Virtuella-machine-platser](./media/connect-logon/azureportaldashboard.png)

3. <span data-ttu-id="48bd7-116">Klicka på **Anslut** i hello kommandofält ovanpå hello virtuella instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="48bd7-116">Click **Connect** on hello command bar atop hello virtual machine dashboard.</span></span>

    ![Ansluta ikonen för hello virtuell dator](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If hello **Connect** button isn't available, see hello troubleshooting tips at hello end of this article.
>
>
-->

## <a name="log-on-toohello-virtual-machine"></a><span data-ttu-id="48bd7-118">Logga in toohello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="48bd7-118">Log on toohello virtual machine</span></span>
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a><span data-ttu-id="48bd7-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48bd7-119">Next steps</span></span>
* <span data-ttu-id="48bd7-120">Om hello **Anslut** är inaktiverad eller du har andra problem med anslutning till fjärrskrivbord hello, försök återställer hello-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="48bd7-120">If hello **Connect** button is inactive or you are having other problems with hello Remote Desktop connection, try resetting hello configuration.</span></span> <span data-ttu-id="48bd7-121">Klicka på **Återställ fjärråtkomst** hello virtuella instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="48bd7-121">click **Reset remote access** from hello virtual machine dashboard.</span></span>

    ![Återställ fjärråtkomst](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* <span data-ttu-id="48bd7-123">För problem med ditt lösenord, pröva att återställa den.</span><span class="sxs-lookup"><span data-stu-id="48bd7-123">For problems with your password, try resetting it.</span></span> <span data-ttu-id="48bd7-124">Klicka på **Återställ lösenord** längs hello vänster kant instrumentpanelen för virtuell dator under **stöd + felsökning**.</span><span class="sxs-lookup"><span data-stu-id="48bd7-124">Click **Reset password** along hello left edge of virtual machine dashboard, under **Support + Troubleshooting**.</span></span>

    ![Återställ lösenord](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

<span data-ttu-id="48bd7-126">Om dessa tips fungerar inte eller inte är vad du behöver kan du se [Felsöka fjärrskrivbord anslutningar tooa Windows-baserade virtuella Azure-datorn](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="48bd7-126">If those tips don't work or aren't what you need, see [Troubleshoot Remote Desktop connections tooa Windows-based Azure Virtual Machine](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="48bd7-127">Den här artikeln beskriver hur du diagnostiserar och löser vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="48bd7-127">This article walks you through diagnosing and resolving common problems.</span></span>
