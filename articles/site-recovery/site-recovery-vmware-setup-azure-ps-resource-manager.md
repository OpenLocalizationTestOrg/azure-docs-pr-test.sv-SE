---
title: " Hantera en processerver som körs i Azure (Resource Manager) | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset upp en återställning efter fel bearbeta server (Resource Manager) i Azure."
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
ms.openlocfilehash: 70426b96095cc42befff6c4114fb56536284a667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-process-server-running-in-azure-resource-manager"></a>Hantera en processerver som körs i Azure (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](./site-recovery-vmware-setup-azure-ps-resource-manager.md)
> * [Klassisk](./site-recovery-vmware-setup-azure-ps-classic.md)

Bör toodeploy processerver i Azure under återställning efter fel, om det är fördröjningar mellan hello Azure Virtual Network och ditt lokala nätverk. Den här artikeln beskriver hur du kan skapa, konfigurera och hantera hello servrar körs i Azure.

> [!NOTE]
> Den här artikeln är toobe används om du har använt **Resource Manager** som hello distributionsmodell för hello virtuella datorer under växling vid fel. Om du använde **klassiska** som hello distributionsmodell åtgärderna hello i [hur tooset upp och konfigurera en processerver (klassisk)](./site-recovery-vmware-setup-azure-ps-classic.md)

## <a name="prerequisites"></a>Krav

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>Distribuera en processerver i Azure
1. I hello valv > **Site Recovery-infrastruktur** (under hello ”hantera” rubrik) > **Konfigurationsservrar** (under ”för VMware och fysiska datorer” rubrik), väljer du hello konfigurationsservern.
2. Hello konfigurationsservern information på sidan som öppnas klickar du på ”+ bearbeta server”

  ![Lägg till Galleri för process-server](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps.png)

3.  På hello **Lägg till processerver** sidan Välj hello följande värden

  ![Lägg till galleriobjektet för processervern](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-1.png)
|**Fältnamn**|**Värde**|
|-|-|
|Välj var du vill toodeploy processervern|Välj hello värde **distribuera en processerver i Azure** |
|Prenumeration|Välj hello Azure-prenumeration där du redundansväxlade hello virtuella datorer|
|Resursgrupp|Du kan skapa en resursgrupp toodeploy processen servern eller välj toodeploy hello processervern i en befintlig resursgrupp|
|Plats|Välj hello Azure-Datacenter till vilka hello virtuella datorer där redundansväxlats till|
|Azure-nätverk|Välj hello Azure virtuella Network(VNet) som hello virtuella datorer där redundansväxlats till. Om du redundansväxlade virtuella datorer i flera virtuella Azure-nätverk måste en processerver distribueras per virtuellt nätverk|

4. Fyll i hello resten av hello egenskaper för hello processervern

  ![Lägg till processerver sammanfattning](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-2.png)
|**Fältnamn**|**Värde**|
|-|-|
|Servernamn|Visningsnamnet & värdnamnet för den virtuella datorn på processen|
| Användarnamn|Ett användarnamn som är administratör på den virtuella datorn|
|Lagringskonto|Namnet på hello Storage-konto där hello virtuella datorns virtuella disken placeras|
|Undernät|hello undernät för hello Azure VNet toowhich hello virtuella datorn är ansluten|
| IP-adress|IP-adress som du vill att hello processen server tooassume när den startar|
5. Klicka på hello OK-knapp toostart hello processen server virtuell dator distribueras.

> [!NOTE]
> toobe kan toouse denna processerver för återställning efter fel, behöver du tooregister med hello lokalt konfigurationsservern.

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a>Registrera hello processen server (som körs i Azure) tooa konfigurationsservern (som körs lokalt)

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a>Uppgradera hello toolatest processerverversionen.

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>Avregistrera hello processervern (som körs i Azure) från en Server för konfiguration (som körs lokalt)

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
