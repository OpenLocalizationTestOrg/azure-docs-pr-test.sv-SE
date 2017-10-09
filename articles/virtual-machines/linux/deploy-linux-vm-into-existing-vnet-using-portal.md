---
title: "aaaDeploy virtuella Linux-datorer i befintliga nätverk med Azure-portalen | Microsoft Docs"
description: "Distribuera en Linux-VM till ett befintligt virtuellt Azure-nätverk som använder hello-portalen."
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
ms.openlocfilehash: 51272b8cdb1dc7f3fe768d2ebbf4ebe0d754cf19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-portal"></a>Hur toodeploy en Linux-dator i ett befintligt virtuellt nätverk med Azure med hello Azure-portalen

Den här artikeln beskrivs hur du toodeploy en virtuell dator (VM) till ett befintligt virtuellt nätverk (VNet). Azure tillgångar som Vnet och nätverket säkerhetsgrupper bör vara statiska och resurser som distribueras sällan kvar längre. När ett virtuellt nätverk har distribuerats, kan den återanvändas av konstant redeployments utan någon negativ påverkar toohello infrastruktur. Om du tänker ett VNet som en traditionell maskinvara nätverks-switch - du behöver tooconfigure en helt ny maskinvara växel med varje distribution.  

Med en korrekt konfigurerad VNet, kan du fortsätta toodeploy nya servrar i det virtuella nätverket flera gånger med några eventuella ändringar som krävs under hello hello virtuella nätverk.

## <a name="create-hello-resource-group"></a>Skapa hello resursgrupp

Först skapar du en resurs grupp tooorganize allt som du skapar i den här genomgången. Läs mer om Azure-resursgrupper [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)

![createResourceGroup](./media/deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-hello-vnet"></a>Skapa hello VNet

Skapa sedan en VNet toolaunch hello virtuella datorer i. Hej VNet innehåller ett undernät och associeras med hello nätverkssäkerhetsgrupp med det här undernätet i ett senare steg.

![createVNet](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-toohello-subnet"></a>Lägg till ett virtuellt nätverkskort toohello undernät

Virtuella nätverkskort (VNics) är viktigt när du ansluter toodifferent virtuella datorer. Den här metoden används för att hålla hello VNic som en statisk resurs hello virtuella datorer kan vara tillfälliga. Skapa ett virtuellt nätverkskort och koppla den till hello undernät som skapats i hello föregående steg.

![createVNic](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-hello-network-security-group"></a>Skapa hello nätverkssäkerhetsgrupp

Säkerhetsgrupper för Azure-nätverk är likvärdiga tooa brandväggen på hello nätverksnivån. Läs mer på Azure nätverkssäkerhetsgrupper [vad är en Nätverkssäkerhetsgrupp](../../virtual-network/virtual-networks-nsg.md).

![createNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a>Lägg till regel för att tillåta en inkommande SSH

hello VM behöver åtkomst från hello internet, så att en regel som tillåter inkommande port 22 trafik toobe har passerat hello nätverket tooport 22 på hello virtuell dator skapas.

![createInboundSSH](./media/deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-hello-nsg-with-hello-subnet"></a>Koppla hello NSG till hello undernät

Associera hello nätverkssäkerhetsgrupp med hello VNet och hello undernät som skapats med hello undernät. Nätverkssäkerhetsgrupper kan associeras med ett hela undernät eller ett enskilt virtuellt nätverkskort. Med hello brandvägg filtrera trafik på nivån för hello undernät, skyddas alla VNics och hello virtuella datorer i undernätet hello av hello nätverkssäkerhetsgruppen. hello är andra metoden hello network security group som är associerad med bara ett enda virtuellt nätverkskort och skyddar en VM.

![associateNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a>Distribuera hello VM till hello VNet och NSG

Hello Azure-portalen är hello Linux VM att använda, distribuerade toohello befintliga Azure-resursgrupp, virtuella nätverk, undernät och virtuellt nätverkskort.

![createVM](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

Genom att använda befintliga resurser för hello portal toochoose kan instruera Azure toodeploy hello VM i hello befintliga nätverk. När ett VNet och undernät har distribuerats, kan lämnas som statisk eller permanenta resurser i Azure-region.  

## <a name="next-steps"></a>Nästa steg

* [Använda en viss distribution för toocreate en Azure Resource Manager-mall](../windows/cli-deploy-templates.md)
* [Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon](create-cli-complete.md)
* [Skapa en Linux-VM på Azure med hjälp av mallar](create-ssh-secured-vm-from-template.md)
