---
title: "aaaConfigure privata IP-adresser för virtuella datorer (klassisk) - Azure-portalen | Microsoft Docs"
description: "Lär dig hur tooconfigure privata IP-adresser för virtuella datorer (klassisk) med hello Azure-portalen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: df4bfa6768fc9e66db89785b633ffdb0274dbc46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-portal"></a>Konfigurera privat IP-adresser för en virtuell dator (klassisk) med hello Azure-portalen

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Den här artikeln beskriver hello klassiska distributionsmodellen. Du kan också [hantera en statisk privat IP-adress i hello Resource Manager-distributionsmodellen](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

hello exempel stegen nedan förväntar sig en enkel miljö som redan har skapats. Om du vill toorun hello steg så som de visas i det här dokumentet, först skapa hello testmiljö som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a>Hur toospecify en statisk privat IP-adress när du skapar en virtuell dator
toocreate en virtuell dator med namnet *DNS01* i hello *klientdel* undernätet i ett VNet med namnet *TestVNet* med en statisk privat IP-adress för *192.168.1.101*, Följ hello stegen nedan:

1. Från en webbläsare, navigerar du toohttp://portal.azure.com och, om det behövs, logga in med ditt Azure-konto.
2. Klicka på **ny** > **Compute** > **Windows Server 2012 R2 Datacenter**, Lägg märke till att hello **Välj en distributionsmodell** redan lista visas **klassiska**, och klicka sedan på **skapa**.
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. I hello **skapa VM** bladet anger hello namnet på hello VM toobe skapade (*DNS01* i vårt exempel), hello lokalt administratörskonto och lösenord.
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Klicka på **valfri konfiguration** > **nätverk** > **virtuellt nätverk**, och klicka sedan på **TestVNet**. Om **TestVNet** är inte tillgänglig, kontrollera att du använder hello *centrala USA* plats och har skapat hello testmiljön som beskrivs hello början av den här artikeln.
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. I hello **nätverk** bladet, se till att hello undernät markerade är *klientdel*, klicka på **IP-adresser**under **tilldelningavIP-adress** klickar du på **statiska**, och ange sedan *192.168.1.101* för **IP-adress** som visas nedan.
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Klicka på **OK** i hello **IP-adresser** bladet Klicka **OK** i hello **nätverk** bladet och klickar på **OK**i hello **valfria config** bladet.
7. I hello **skapa VM** bladet, klickar du på **skapa**. Meddelande hello panelen nedan visas i instrumentpanelen.
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a>Hur tooretrieve statiska privata IP-information för en virtuell dator
tooview hello statisk privat IP-adressinformation för hello skapas den virtuella datorn med hello ovanstående steg, köra hello stegen nedan.

1. Hello Azure Azure-portalen klickar du på **Bläddra bland alla** > **virtuella datorer (klassisk)** > **DNS01**  >   **Alla inställningar** > **IP-adresser** och notera hello tilldelning av IP-adresser och IP-adress som du ser nedan.
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a>Hur tooremove en statisk privat IP-adress från en virtuell dator
tooremove hello statisk privat IP-adress från hello VM skapade ovan, följ hello stegen nedan.

1. Från hello **IP-adresser** bladet som visas ovan, klickar du på **dynamiska** toohello höger i **IP-adresstilldelning**, klicka på **spara**, och sedan Klicka på **Ja**.
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a>Hur tooadd en statisk privat IP-adress tooan befintliga VM
tooadd en statisk privat IP-adress toohello VM som skapats med hello anvisningarna ovan, så hello nedan:

1. Från hello **IP-adresser** bladet som visas ovan, klickar du på **statiska** toohello höger i **IP-adresstilldelning**.
2. Typen *192.168.1.101* för **IP-adress**, klicka på **spara**, och klicka sedan på **Ja**.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.
* Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.
* Kontakta hello [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).

