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
# <a name="how-tooconnect-and-log-on-tooan-azure-virtual-machine-running-windows"></a>Hur tooconnect och logga in tooan Azure virtuella datorn kör Windows
Du kommer att använda hello **Anslut** knapp i hello Azure portal toostart en session för fjärrskrivbord (RDP) från en Windows-skrivbordet. Du ansluter toohello virtuell dator och du loggar in.

Om du försöker tooconnect tooa Windows virtuell dator från en Mac måste tooinstall som en RDP-klient för Mac [Microsoft Remote Desktop](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-toohello-virtual-machine"></a>Ansluta toohello virtuell dator
1. Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com/).
2. Hej hubbmenyn, klicka på **virtuella datorer**.
3. Välj hello virtuella hello-listan.
4. Klicka på hello bladet för den virtuella datorn hello **Anslut**.
   
    ![Skärmbild av hello Azure portal som visar hur tooconnect tooyour VM.](./media/connect-logon/connect.png)
   
   > [!TIP]
   > Om hello **Anslut** knappen hello-portalen är nedtonad och du inte är ansluten tooAzure via en [Express Route](../../expressroute/expressroute-introduction.md) eller [plats-till-plats VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) anslutning, behöver du toocreate och tilldela den virtuella datorn en offentlig IP-adress innan du kan använda RDP. Du kan läsa mer om [offentliga IP-adresser i Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 

## <a name="log-on-toohello-virtual-machine"></a>Logga in toohello virtuell dator
[!INCLUDE [virtual-machines-log-on-win-server](../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Nästa steg
Om du stöter på problem när du försöker tooconnect, se [felsöka fjärrskrivbordsanslutningar](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Den här artikeln beskriver hur du diagnostiserar och löser vanliga problem.

