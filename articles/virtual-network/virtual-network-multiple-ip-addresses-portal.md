---
title: "aaaMultiple IP-adresser för virtuella datorer i Azure - portalen | Microsoft Docs"
description: "Lär dig hur tooassign flera IP-adresser tooa virtuella datorn använder hello Azure-portalen | Resource Manager."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 3a8cae97-3bed-430d-91b3-274696d91e34
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2016
ms.author: annahar
ms.openlocfilehash: 34075766ac68c8de38c258a4d70e35881f28bb0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-hello-azure-portal"></a>Tilldela adresser toovirtual av flera IP-datorer som använder hello Azure-portalen

>[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]
>
Den här artikeln förklarar hur toocreate en virtuell dator (VM) via hello Azure Resource Manager distribution modellen med hello Azure-portalen. Flera IP-adresser kan inte tilldelas tooresources som skapats via hello klassiska distributionsmodellen. Mer om Azure distributionsmodeller läsa hello toolearn [förstår distributionsmodellerna](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Skapa en virtuell dator med flera IP-adresser

Om du vill toocreate en virtuell dator med flera IP-adresser eller en statisk privat IP-adress, måste du skapa den med hjälp av PowerShell eller hello Azure CLI. Klicka på hello PowerShell eller CLI alternativ hello överst i den här artikeln toolearn hur. Du kan skapa en virtuell dator med en dynamisk privat IP-adress och (valfritt) en offentlig IP-adress med hjälp av hello portal genom att följa stegen hello i hello [skapa en virtuell Windows-dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md) eller [skapa ett Linux VM](../virtual-machines/linux/quick-create-portal.md) artiklar. När du har skapat hello VM kan du ändra hello IP-adress från dynamisk toostatic och lägga till ytterligare IP-adresser med hjälp av hello portal genom att följa anvisningarna i hello [lägga till IP-adresser tooa VM](#add) i den här artikeln.

## <a name="add"></a>Lägg till IP-adresser tooa VM

Du kan lägga till privata och offentliga IP-adresser tooa NIC hello steg som följer. hello exemplen i följande avsnitt hello förutsätter att du redan har en virtuell dator med hello tre IP-konfigurationer som beskrivs i hello [scenariot](#Scenario) i den här artikeln, men det är inte nödvändigt att du gör.

### <a name="coreadd"></a>Grundläggande steg

1. Bläddra toohello Azure-portalen på https://portal.azure.com och logga in, om det behövs.
2. I hello-portalen klickar du på **fler tjänster** > typen *virtuella datorer* i hello filterfältet och klicka sedan på **virtuella datorer**.
3. I hello **virtuella datorer** bladet, klickar du på hello VM som du vill tooadd IP-adresser till. Klicka på **nätverksgränssnitt** i hello virtuella bladet som visas och välj sedan hello nätverket gränssnitt du tooadd hello IP-adresser till. Hello-exemplet som visas i hello följande bild, hello NIC med namnet *myNIC* från hello virtuella datorn med namnet *myVM* väljs:

    ![Nätverksgränssnitt](./media/virtual-network-multiple-ip-addresses-portal/figure1.png)

4. Klicka på hello bladet som visas för hello NIC som du har valt **IP-konfigurationer**.

Fullständig hello steg i hello avsnitt som följer, baserat på hello typ av IP-adress du vill tooadd.

### <a name="add-a-private-ip-address"></a>**Lägg till en privat IP-adress**

Slutför hello följande steg tooadd en ny privat IP-adress:

1. Fullständig hello stegen i hello [viktiga steg](#coreadd) i den här artikeln.
2. Klicka på **Lägg till**. I hello **lägga till IP-konfiguration** bladet som visas, skapa en IP-konfiguration med namnet *IPConfig 4* med *10.0.0.7* som en *statiska* privata IP-adress och klicka sedan på **OK**.

    > [!NOTE]
    > När du lägger till en statisk IP-adress, måste du ange en oanvända, giltig adress på hello undernät hello nätverkskortet är anslutet till. Om hello-adress som du väljer inte är tillgänglig hello portal visas ett X för hello IP-adress och du behöver tooselect en annan.

3. När du klickar på OK hello bladet stängs och du ser hello nya IP-konfiguration visas. Klicka på **OK** tooclose hello **lägga till IP-konfiguration** bladet.
4. Du kan klicka på **Lägg till** tooadd ytterligare IP-konfigurationer eller Stäng alla öppna blad toofinish att lägga till IP-adresser.
5. Lägg till hello privata IP-adresser toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln.

### <a name="add-a-public-ip-address"></a>Lägg till en offentlig IP-adress

En offentlig IP-adress har lagts till genom att associera en offentlig IP-adress resurs tooeither en ny IP-konfiguration eller en befintlig IP-konfiguration.

> [!NOTE]
> Offentliga IP-adresser har en låg kostnad. Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan. Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration. Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.
> 

### <a name="create-public-ip"></a>Skapa en offentlig IP-adressresurs

En offentlig IP-adress är en inställning för en offentlig IP-adressresurs. Om du har en offentlig IP-adressresurs som inte är för närvarande associerad tooan IP-konfiguration som du vill hoppa över hello följande tooassociate tooan IP-konfiguration och slutför hello stegen i ett av hello avsnitten som följer, som du behöver. Om du inte har en tillgänglig offentliga IP-adressresurs slutföra hello följande steg toocreate en:

1. Bläddra toohello Azure-portalen på https://portal.azure.com och logga in, om det behövs.
3. I hello-portalen klickar du på **ny** > **nätverk** > **offentliga IP-adressen**.
4. I hello **skapa offentlig IP-adress** bladet som visas, ange en **namn**, Välj en **IP-adresstilldelning** typ, en **prenumeration**, **Resursgruppen**, och en **plats**, klicka på **skapa**som visas i följande bild hello:

    ![Skapa en offentlig IP-adressresurs](./media/virtual-network-multiple-ip-addresses-portal/figure5.png)

5. Fullständig hello steg i en hello avsnitten som följer tooassociate hello offentliga IP-resurs tooan IP-adresskonfiguration.

#### <a name="associate-hello-public-ip-address-resource-tooa-new-ip-configuration"></a>Associera hello offentliga IP-adress resurs tooa nya IP-konfiguration

1. Fullständig hello stegen i hello [viktiga steg](#coreadd) i den här artikeln.
2. Klicka på **Lägg till**. I hello **lägga till IP-konfiguration** bladet som visas, skapa en IP-konfiguration med namnet *IPConfig 4*. Aktivera hello **offentliga IP-adressen** och välj en befintlig, tillgängliga offentliga IP-adressresurs från hello **Välj offentlig IP-adress** bladet som visas.

    När du har markerat hello offentliga IP-adressresurs, klickar du på **OK** och hello bladet stängs. Om du inte har en befintlig offentlig IP-adress, kan du skapa en genom att slutföra hello stegen i hello [skapa en offentlig IP-adressresurs](#create-public-ip) i den här artikeln. 

3. Granska hello nya IP-konfigurationen. Även om en privat IP-adress har tilldelats tilldelades en automatiskt toohello IP-konfiguration, eftersom alla IP-konfigurationer måste ha en privat IP-adress.
4. Du kan klicka på **Lägg till** tooadd ytterligare IP-konfigurationer eller Stäng alla öppna blad toofinish att lägga till IP-adresser.
5. Lägg till hello privata IP-adress toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln. Lägg inte till hello offentliga IP-adress toohello operativsystem.

#### <a name="associate-hello-public-ip-address-resource-tooan-existing-ip-configuration"></a>Associera hello offentliga IP-adress resurs tooan befintliga IP-konfiguration

1. Fullständig hello stegen i hello [viktiga steg](#coreadd) i den här artikeln.
2. Klicka på hello IP-konfiguration som du vill tooadd hello offentliga IP-adressresurs till.
3. I hello IPConfig bladet som visas, klickar du på **IP-adress**.
4. I hello **Välj offentlig IP-adress** bladet som visas, Välj en offentlig IP-adress.
5. Klicka på **spara** och hello blad stängs. Om du inte har en befintlig offentlig IP-adress, kan du skapa en genom att slutföra hello stegen i hello [skapa en offentlig IP-adressresurs](#create-public-ip) i den här artikeln.
3. Granska hello nya IP-konfigurationen.
4. Du kan klicka på **Lägg till** tooadd ytterligare IP-konfigurationer eller Stäng alla öppna blad toofinish att lägga till IP-adresser. Lägg inte till hello offentliga IP-adress toohello operativsystem.


[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
