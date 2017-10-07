---
title: "aaaConfigure privata IP-adresser för virtuella datorer - Azure-portalen | Microsoft Docs"
description: "Lär dig hur tooconfigure privata IP-adresser för virtuella datorer med hello Azure-portalen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 474161303cdf8cb98e16ffd7cef6b74debdbc49a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-portal"></a>Konfigurera privat IP-adresser för en virtuell dator med hello Azure-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)
> * [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
> * [Azure CLI](virtual-networks-static-private-ip-arm-cli.md)
> * [Azure-portalen (klassisk)](virtual-networks-static-private-ip-classic-pportal.md)
> * [PowerShell (klassisk)](virtual-networks-static-private-ip-classic-ps.md)
> * [Azure CLI (klassisk)](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Den här artikeln beskriver hello Resource Manager-modellen. Du kan också [hantera statisk privat IP-adress i hello klassiska distributionsmodellen](virtual-networks-static-private-ip-classic-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

hello exempel stegen nedan förväntar sig en enkel miljö som redan har skapats. Om du vill toorun hello steg så som de visas i det här dokumentet, först skapa hello testmiljö som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-arm-pportal.md).

## <a name="how-toocreate-a-vm-for-testing-static-private-ip-addresses"></a>Hur toocreate en virtuell dator för att testa statisk privat IP-adresser
Du kan inte ange en statisk privat IP-adress under hello skapandet av en virtuell dator i läget för hello Resource Manager distribution med hello Azure-portalen. Du måste först skapa hello VM är textmarkering dess privata IP-toobe statisk.

toocreate en virtuell dator med namnet *DNS01* i hello *klientdel* undernätet i ett VNet med namnet *TestVNet*, följ hello stegen nedan.

1. Från en webbläsare, navigerar du toohttp://portal.azure.com och, om det behövs, logga in med ditt Azure-konto.
2. Klicka på **ny** > **Compute** > **Windows Server 2012 R2 Datacenter**, Lägg märke till att hello **Välj en distributionsmodell** redan lista visas **Resource Manager**, och klicka sedan på **skapa**enligt hello bilden nedan.
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. I hello **grunderna** bladet anger hello namnet på hello VM toobe skapade (*DNS01* i vårt exempel), hello lokalt administratörskonto och lösenord, som visas i hello bilden nedan.
   
    ![Bladet Grundläggande inställningar](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. Se till att hello **plats** valts är *centrala USA*, klicka sedan på **Välj befintlig** under **resursgruppen**, klicka på **Resursgruppen** igen och klicka sedan på *TestRG*, och klicka sedan på **OK**.
   
    ![Bladet Grundläggande inställningar](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. I hello **välja en storlek** bladet väljer **A1 Standard**, och klicka sedan på **Välj**.
   
    ![Välj ett blad storlek](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. I den **inställningar** bladet, se till att hello följande egenskaper har angetts med hello värdena nedan och klicka sedan på **OK**.
   
    -**Lagringskontot**: *vnetstorage*
   
   * **Nätverket**: *TestVNet*
   * **Undernät**: *klientdel*
     
     ![Välj ett blad storlek](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. I hello **sammanfattning** bladet, klickar du på **OK**. Meddelande hello panelen nedan visas i instrumentpanelen.
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a>Hur tooretrieve statiska privata IP-information för en virtuell dator
tooview hello statisk privat IP-adressinformation för hello skapas den virtuella datorn med hello ovanstående steg, köra hello stegen nedan.

1. Hello Azure Azure-portalen klickar du på **Bläddra bland alla** > **virtuella datorer** > **DNS01** > **alla inställningar för** > **nätverksgränssnitt** och klicka sedan på hello endast nätverksgränssnitt som anges.
   
    ![Distribuera Virtuella sida vid sida](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. I hello **nätverksgränssnittet** bladet, klickar du på **alla inställningar** > **IP-adresser** och meddelande hello **tilldelning** och **IP-adress** värden.
   
    ![Distribuera Virtuella sida vid sida](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a>Hur tooadd en statisk privat IP-adress tooan befintliga VM
tooadd en statisk privat IP-adress toohello VM som skapats med hello anvisningarna ovan, så hello nedan:

1. Från hello **IP-adresser** bladet som visas ovan, klickar du på **statiska** under **tilldelning**.
2. Typen *192.168.1.101* för **IP-adress**, och klicka sedan på **spara**.
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> Om när du klickar på **spara** du märker att hello tilldelning fortfarande är angiven för**dynamisk**, innebär det att hello IP-adress som du har angett används redan. Prova en annan IP-adress.
> 
> 

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a>Hur tooremove en statisk privat IP-adress från en virtuell dator
tooremove hello statisk privat IP-adress från hello VM skapade ovan, Slutför hello följande steg:

Från hello **IP-adresser** bladet som visas ovan, klickar du på **dynamiska** under **tilldelning**, och klicka sedan på **spara**.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.
* Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.
* Kontakta hello [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).

