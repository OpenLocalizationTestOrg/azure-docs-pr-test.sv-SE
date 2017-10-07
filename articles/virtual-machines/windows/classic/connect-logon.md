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
# <a name="log-on-tooa-windows-virtual-machine-using-hello-azure-portal"></a>Logga in tooa Windows virtuell dator med hello Azure-portalen
I hello Azure-portalen, använder du hello **Anslut** knappen toostart en fjärrskrivbordssession och logga in tooa Windows VM.

Vill du tooconnect tooa Linux VM? Se [hur toolog på tooa virtuell dator som kör Linux](../../linux/mac-create-ssh-keys.md).

<!--
Deleting, but not 100% sure
Learn how too[perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Mer information om hur toolog tooa VM använda hello Resource Manager objektmodellen, se [här](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="connect-toohello-virtual-machine"></a>Ansluta toohello virtuell dator
1. Logga in toohello Azure-portalen.
2. Klicka på hello virtuell dator som du vill tooaccess. hello namnet visas i hello **alla resurser** fönstret.

    ![Virtuella-machine-platser](./media/connect-logon/azureportaldashboard.png)

3. Klicka på **Anslut** i hello kommandofält ovanpå hello virtuella instrumentpanelen.

    ![Ansluta ikonen för hello virtuell dator](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If hello **Connect** button isn't available, see hello troubleshooting tips at hello end of this article.
>
>
-->

## <a name="log-on-toohello-virtual-machine"></a>Logga in toohello virtuell dator
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Nästa steg
* Om hello **Anslut** är inaktiverad eller du har andra problem med anslutning till fjärrskrivbord hello, försök återställer hello-konfigurationen. Klicka på **Återställ fjärråtkomst** hello virtuella instrumentpanel.

    ![Återställ fjärråtkomst](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* För problem med ditt lösenord, pröva att återställa den. Klicka på **Återställ lösenord** längs hello vänster kant instrumentpanelen för virtuell dator under **stöd + felsökning**.

    ![Återställ lösenord](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

Om dessa tips fungerar inte eller inte är vad du behöver kan du se [Felsöka fjärrskrivbord anslutningar tooa Windows-baserade virtuella Azure-datorn](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Den här artikeln beskriver hur du diagnostiserar och löser vanliga problem.
