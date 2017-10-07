---
title: "aaaConfigure ett virtuellt nätverk i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur tooconfigure ett befintligt virtuellt nätverk och undernät, och Använd dem i en virtuell dator med Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: a11ce8315e3c540e44aeacc9c5ee3dde014d4621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Konfigurera ett virtuellt nätverk i Azure DevTest Labs
Enligt beskrivningen i artikeln hello [lägga till en virtuell dator med artefakter tooa lab](devtest-lab-add-vm-with-artifacts.md)när du skapar en virtuell dator i ett labb du kan ange ett konfigurerat virtuellt nätverk. Ett scenario för att göra detta är om du behöver tooaccess corpnet resurserna från dina virtuella datorer med hjälp av hello virtuella nätverk som har konfigurerats med ExpressRoute eller plats-till-plats-VPN. hello följande avsnitt visar hur tooadd ditt befintliga virtuella nätverk i ett labb inställningarna för virtuella nätverk så att den är tillgänglig toochoose när du skapar virtuella datorer.

## <a name="configure-a-virtual-network-for-a-lab-using-hello-azure-portal"></a>Konfigurera ett virtuellt nätverk för ett testlabb med hello Azure-portalen
hello följande steg beskriver hur du lägger till ett befintligt virtuellt nätverk (och undernät) tooa labb så att den kan användas när du skapar en virtuell dator i hello samma labbet. 

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
3. Välj önskad hello-labb hello listan övningar. 
4. På bladet hello lab väljer **Configuration**.
5. På hello lab **Configuration** bladet väljer **virtuella nätverken**.
6. På hello **virtuella nätverken** bladet visas en lista över virtuella nätverk som konfigurerats för hello aktuella labbet samt hello standard virtuella nätverk som har skapats för ditt labb. 
7. Välj **+ Lägg till**.
   
    ![Lägg till ett befintligt virtuellt nätverk tooyour labb](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. På hello **för virtuella nätverk** bladet väljer **[väljer virtuellt nätverk]**.
   
    ![Välj ett befintligt virtuellt nätverk](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. På hello **Välj virtuellt nätverk** bladet, Välj hello önskade virtuella nätverket. hello bladet visar alla hello virtuella nätverk som är under hello samma region i hello prenumeration som hello labbet.  
10. När du har valt ett virtuellt nätverk, returneras toohello **för virtuella nätverk** på hello undernät i hello lista längst hello hello-bladet.

    ![Lista över undernät](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    hello Lab undernät-bladet visas.

    ![Blad för labbet undernät](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. Ange en **labbundernätsnamn**.
12. tooallow ett undernät toobe som använts i labbet skapa en virtuell dator, Välj **användning i virtuell dator skapas**.
13. tooenable en [delad offentlig IP-adress](devtest-lab-shared-ip.md)väljer **aktivera delad offentlig IP**.
14. tooallow offentliga IP-adresser i ett undernät, Välj **kan skapa en offentlig IP-**.
15. I hello **maximalt virtuella datorer per användare** anger hello maximala virtuella datorer per användare för varje undernät. Lämna fältet tomt om du vill att ett obegränsat antal virtuella datorer.
16. Välj **OK** tooclose hello Lab undernät-bladet.
17. Välj **spara** tooclose hello virtuella nätverk-bladet.
18. Hello virtuellt nätverk är konfigurerat, kan du välja den när du skapar en virtuell dator. 
    toosee hur toocreate en virtuell dator och ange ett virtuellt nätverk finns i artikeln toohello [lägga till en virtuell dator med artefakter tooa lab](devtest-lab-add-vm-with-artifacts.md). 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nästa steg
När du har lagt till hello önskade virtuella nätverket tooyour lab, hello nästa steg är för[lägga till ett labb för VM-tooyour](devtest-lab-add-vm-with-artifacts.md).

