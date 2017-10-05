---
title: "Distribuera virtuella Linux-datorer i befintliga nätverk med Azure-portalen | Microsoft Docs"
description: "Distribuera en Linux-VM till ett befintligt virtuellt Azure-nätverk som använder portalen."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 964c0fc41773b50a9fbe476df47460484c2ada66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-the-azure-portal"></a>Hur du distribuerar en virtuell Linux-dator i ett befintligt virtuellt nätverk med Azure med Azure-portalen

Den här artikeln visar hur du distribuerar en virtuell dator (VM) till ett befintligt virtuellt nätverk (VNet). Azure tillgångar som Vnet och nätverket säkerhetsgrupper bör vara statiska och resurser som distribueras sällan kvar längre. När ett virtuellt nätverk har distribuerats, kan den återanvändas av konstant redeployments utan någon negativ påverkar i infrastrukturen. Om du tänker ett VNet som en traditionell maskinvara nätverks-switch - du inte behöver konfigurera en helt ny maskinvara växel med varje distribution.  

Du kan fortsätta att distribuera nya servrar i det virtuella nätverket flera gånger med några eventuella ändringar som krävs under hela VNet med en korrekt konfigurerad VNet.

## <a name="create-the-resource-group"></a>Skapa en resursgrupp

Först skapa en resursgrupp för att organisera allt som du skapar i den här genomgången. Läs mer om Azure-resursgrupper [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)

![createResourceGroup](./media/deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-the-vnet"></a>Skapa VNet

Sedan skapa ett virtuellt nätverk för att starta de virtuella datorerna till. Det virtuella nätverket innehåller ett undernät och är kopplat till nätverkssäkerhetsgruppen med undernätet i ett senare steg.

![createVNet](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-to-the-subnet"></a>Lägg till ett virtuellt nätverkskort i undernätet

Virtuella nätverkskort (VNics) är viktigt när du ansluter dem till olika virtuella datorer. Den här metoden används för att hålla VNic som en statisk resurs medan de virtuella datorerna kan vara tillfälligt. Skapa ett virtuellt nätverkskort och associera det med undernät som skapats i föregående steg.

![createVNic](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-the-network-security-group"></a>Skapa nätverkssäkerhetsgruppen

Säkerhetsgrupper för Azure-nätverket är likvärdiga med en brandvägg på nätverksnivå. Läs mer på Azure nätverkssäkerhetsgrupper [vad är en Nätverkssäkerhetsgrupp](../../virtual-network/virtual-networks-nsg.md).

![createNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a>Lägg till regel för att tillåta en inkommande SSH

Den virtuella datorn behöver åtkomst från internet, så skapas en regel som tillåter inkommande port 22 trafik som skickas via nätverket till port 22 på den virtuella datorn.

![createInboundSSH](./media/deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-the-nsg-with-the-subnet"></a>Koppla NSG: N till undernätet

Associera nätverkssäkerhetsgruppen med VNet och undernät som skapats med undernätet. Nätverkssäkerhetsgrupper kan associeras med ett hela undernät eller ett enskilt virtuellt nätverkskort. Med brandväggen filtrerar trafik på undernätverksnivån, skyddas alla VNics och virtuella datorer i undernätet av nätverkssäkerhetsgruppen. Den andra metoden är nätverkssäkerhetsgruppen som är associerat med bara ett enda virtuellt nätverkskort och skyddar en VM.

![associateNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a>Distribuera den virtuella datorn till VNet och NSG

Med hjälp av Azure portal distribueras Linux VM till den befintliga Azure-resursgrupp, virtuella nätverk, undernät och virtuellt nätverkskort.

![createVM](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

Genom att använda portalen för att välja befintliga resurser kan instruera du Azure för att distribuera den virtuella datorn i det befintliga nätverket. När ett VNet och undernät har distribuerats, kan lämnas som statisk eller permanenta resurser i Azure-region.  

## <a name="next-steps"></a>Nästa steg

* [Skapa en specifik distribution med hjälp av en Azure Resource Manager-mall](../windows/cli-deploy-templates.md)
* [Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon](create-cli-complete.md)
* [Skapa en Linux-VM på Azure med hjälp av mallar](create-ssh-secured-vm-from-template.md)
