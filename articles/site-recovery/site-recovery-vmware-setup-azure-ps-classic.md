---
title: " Hantera en Server för Process som körs i Azure(Classic) | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset in återställning processen Server(Classic) i Azure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: eadcc0236c77c9ebbbc885c4a7ee81098f1f4e72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-process-server-running-in-azure-classic"></a>Hantera en Server för Process som körs i Azure (klassisk)
> [!div class="op_single_selector"]
> * [Klassiska Azure-portalen](./site-recovery-vmware-setup-azure-ps-classic.md)
> * [Resource Manager](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

Bör toodeploy Processerver i Azure under återställning efter fel, om det är fördröjningar mellan hello Azure Virtual Network och ditt lokala nätverk. Den här artikeln beskriver hur du kan skapa, konfigurera och hantera hello servrar körs i Azure.

> [!NOTE]
> Den här artikeln är toobe används om du använde klassisk som hello distributionsmodell för hello virtuella datorer under växling vid fel. Om du använde Resource Manager som hello modellen Följ hello distributionsstegen i [hur tooset upp och konfigurera en Processerver (Resource Manager)](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

## <a name="prerequisites"></a>Krav

[!INCLUDE [site-recovery-vmware-process-server-prereq](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>Distribuera en Processerver i Azure

1. Skapa en virtuell dator med hello i Azure Marketplace **Microsoft Azure Site Recovery Process Server V2** </br>
    ![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)
2. Se till att du väljer hello distributionsmodell som **klassiska** </br>
  ![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)
3. I guiden för hello Skapa virtuell dator > grundläggande inställningar, se till att du väljer hello prenumerationen och platsen toowhere du redundansväxlade hello virtuella datorer.</br>
  ![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)
4. Se till att hello den virtuella datorn är ansluten toohello Azure Virtual Network toowhich hello redundansväxlade virtuella datorn är ansluten.</br>
  ![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)
5. När hello Processervern virtuella datorn har etablerats måste toolog i och registrera den med hello konfigurationsservern.

> [!NOTE]
> toobe kan toouse denna Process-Server för återställning efter fel, behöver du tooregister med hello lokalt konfigurationsservern.

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a>Registrera hello (som körs i Azure) Processervern tooa konfigurationsservern (som körs lokalt)

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a>Uppgradera hello processen toolatest serverversionen.

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>Avregistrerar hello Processervern (som körs i Azure) från en Server för konfiguration (som körs lokalt)

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
