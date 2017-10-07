---
title: aaaCreate ett HPC Pack huvudnod i en Azure VM | Microsoft Docs
description: "Lär dig hur modellen toouse hello Azure-portalen och hello Resource Manager distribution toocreate en Microsoft HPC Pack 2012 R2 huvudnod i en Azure VM."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: e6a13eaf-9124-47b4-8d75-2bc4672b8f21
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 3ddefb74b053a48a15f1ba1ca8edbc0192da51a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hello-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Skapa hello huvudnod i HPC Pack-kluster i en virtuell Azure-dator med en Marketplace-avbildning
Använd en [Microsoft HPC Pack 2012 R2 avbildning av virtuell dator](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) från hello Azure Marketplace och hello Azure portal toocreate hello huvudnod i HPC-kluster. HPC Pack VM avbildningen baseras på Windows Server 2012 R2 Datacenter med HPC Pack 2012 R2 uppdatering 3 förinstallerat. Använd den här huvudnod för ett bevis på koncept distributionen av HPC Pack i Azure. Du kan sedan lägga till compute-noder toohello toorun HPC arbetsbelastningar i ett kluster.

> [!TIP]
> toodeploy ett komplett HPC Pack 2012 R2-kluster i Azure som innehåller hello huvudnod och datornoder, rekommenderar vi att du använder en automatiserad metod. Alternativen är hello [HPC Pack IaaS distributionsskriptet](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) och Resource Manager-mallar, till exempel hello [HPC Pack kluster för Windows-arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/). Resource Manager-mallar finns även [Microsoft HPC Pack 2016 kluster](https://github.com/MsHpcPack/HPCPack2016/tree/master/newcluster-templates). 
> 
> 

## <a name="planning-considerations"></a>Planera överväganden
I följande bild hello visas kan du distribuera hello HPC Pack huvudnod i en Active Directory-domän i Azure-nätverk.

![Huvudnod i HPC Pack][headnode]

* **Active Directory-domän**: hello HPC Pack 2012 R2 huvudnod måste vara ansluten tooan Active Directory-domän i Azure innan du börjar hello HPC-tjänster på hello VM. Du kan höja hello VM som du skapar för hello huvudnod som en domänkontrollant innan du startar hello HPC-tjänster som visas i den här artikeln för ett bevis på koncept distribution. Ett annat alternativ är toodeploy en separat domänkontrollant och skog i Azure toowhich du ansluta hello huvudnod VM.

* **Distributionsmodell**: för de flesta nya distributioner Microsoft rekommenderar att du använder hello Resource Manager-modellen. Den här artikeln förutsätter att du använder denna distributionsmodell.

* **Virtuella Azure-nätverket**: när du använder hello Resource Manager distribution modellen toodeploy hello huvudnod kan du ange eller skapa ett Azure-nätverk. Du kan använda hello virtuellt nätverk om du behöver toojoin hello huvudnod tooan Active Directory-domän. Du måste den också senare tooadd beräkningsnod VMs toohello klustret.

## <a name="steps-toocreate-hello-head-node"></a>Steg toocreate hello huvudnod
Nedan följer anvisningar toouse hello Azure portal toocreate en Azure VM för hello HPC Pack huvudnod med hjälp av hello Resource Manager-modellen. 

1. Om du vill toocreate en ny Active Directory-skog i Azure med separata domänkontrollant virtuella datorer kan ett alternativ är toouse en [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc). För en enkel bevis på koncept distribution har finjustering tooomit det här steget och konfigurera hello huvudnod VM sig själv som en domänkontrollant. Det här alternativet beskrivs senare i den här artikeln.
2. På hello [HPC Pack 2012 R2 på Windows Server 2012 R2 sidan](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) i hello Azure Marketplace, klickar du på **Skapa virtuell dator**. 
3. I hello-portalen på hello **HPC Pack 2012 R2 på Windows Server 2012 R2** sidan, Välj hello **Resource Manager** distributionsmodell och klicka sedan på **skapa**.
   
    ![HPC Pack bild][marketplace]
4. Använd hello portal tooconfigure hello inställningar och skapa hello VM. Om du är ny tooAzure följer hello kursen [skapa en virtuell Windows-dator i hello Azure-portalen](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). För ett bevis på koncept distribution kan oftast acceptera hello standard eller rekommenderade inställningar.
   
   > [!NOTE]
   > Om du vill toojoin hello huvudnod tooan befintliga Active Directory-domän i Azure, kontrollera att du anger hello virtuellt nätverk för domänen när du skapar hello VM.
   > 
   > 
5. När du skapar hello VM och hello virtuell dator körs, [ansluta toohello VM](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) av fjärrskrivbord. 
6. Ansluta till hello VM tooan Active Directory-domänens skog genom att välja något av följande alternativ för hello:
   
   * Om du har skapat hello VM i Azure-nätverk med en befintlig domänskog att ansluta hello VM toohello skog med hjälp av Serverhanteraren eller Windows PowerShell standardverktyg. Starta sedan om.
   * Om du har skapat hello VM i ett nytt virtuellt nätverk (utan en befintlig domänskog), befordra du hello VM som en domänkontrollant. Använd standard steg tooinstall och konfigurera hello Active Directory Domain Services-rollen i hello huvudnod. Detaljerade anvisningar finns i [installera en ny Windows Server 2012 Active Directory-skog](https://technet.microsoft.com/library/jj574166.aspx).
7. Efter hello VM körs och är kopplade tooan Active Directory-skog, starta hello HPC Pack tjänster på följande sätt:
   
    a. Ansluta toohello huvudnod virtuella datorn med ett domänkonto som är medlem i hello lokala gruppen Administratörer. Till exempel använda hello administratörskontot som du ställer in när du skapade hello huvudnod VM.
   
    b. Starta Windows PowerShell som administratör för en standardkonfiguration av huvudnod och skriver hello följande:
   
    ```PowerShell
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```
   
    Det kan ta flera minuter för hello HPC Pack services toostart.
   
    Ytterligare huvudnod konfigurationsalternativ, Skriv `get-help HPCHNPrepare.ps1`.

## <a name="next-steps"></a>Nästa steg
* Nu kan du arbeta med hello huvudnod i HPC Pack-kluster. Till exempel starta HPC Cluster Manager och fullständig hello [distribution uppgiftslista](https://technet.microsoft.com/library/jj884141.aspx).
* Om du vill tooincrease hello klustret beräkning kapacitet på begäran, lägger du till [Azure burst noder](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) i en molntjänst. 
* Försök att köra en test-arbetsbelastning på hello klustret. Ett exempel finns hello HPC Pack [Kom igång med](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/hpcpack-cluster-headnode/marketplace.png
