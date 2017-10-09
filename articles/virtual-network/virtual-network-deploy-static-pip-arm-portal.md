---
title: aaaCreate en virtuell dator med en statisk offentlig IP-adress - Azure-portalen | Microsoft Docs
description: "Lär dig hur toocreate en virtuell dator med en statisk offentlig IP-adress med hjälp av hello Azure-portalen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f74d2132785f06148757409ee0a44b98d1e4b98e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-portal"></a>Skapa en virtuell dator med en statisk offentlig IP-adress med hjälp av hello Azure-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Mall](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klassisk)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md). Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Skapa en virtuell dator med en statisk offentlig IP-adress

toocreate en virtuell dator med en statisk offentlig IP-adressen i hello Azure-portalen fullständig hello följande steg:

1. Från en webbläsare, navigerar du toohello [Azure-portalen](https://portal.azure.com) och vid behov, logga in med ditt Azure-konto.
2. Klicka på hello övre vänstra hörnet av hello portal **ny**>>**Compute**>**Windows Server 2012 R2 Datacenter**.
3. I hello **Välj en distributionsmodell** väljer **Resource Manager** och på **skapa**.
4. I hello **grunderna** bladet ange hello VM information som visas nedan och klickar sedan på **OK**.
   
    ![Azure portal – grunderna](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. I hello **välja en storlek** bladet, klickar du på **A1 Standard** enligt nedan, och klicka sedan på **Välj**.
   
    ![Azure portal – Välj en storlek](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. I hello **inställningar** bladet, klickar du på **offentliga IP-adressen**, sedan i hello **skapa offentlig IP-adress** bladet under **tilldelning**, klickar du på **Statiska** enligt nedan. Och klicka sedan på **OK**.
   
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. I hello **inställningar** bladet, klickar du på **OK**.
8. Granska hello **sammanfattning** bladet som visas nedan och klickar sedan på **OK**.
   
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Observera hello ny panel på instrumentpanelen.
   
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. När du har skapat hello VM hello **inställningar** bladet visas enligt nedan
    
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

