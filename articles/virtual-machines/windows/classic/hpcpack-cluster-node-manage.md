---
title: aaaManage HPC Pack klustret datornoder | Microsoft Docs
description: "Lär dig mer om PowerShell-skript, verktyg tooadd, ta bort, starta och stoppa HPC Pack 2012 R2-kluster compute-noder i Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 4193f03b-94e9-4704-a7ad-379abde063a9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 5ac1142cc5da984020779434fbb7cba5ad7c14bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Hantera hello antal och tillgängligheten för compute-noder i ett HPC Pack kluster i Azure
Om du har skapat ett HPC Pack 2012 R2-kluster i virtuella Azure-datorer, kanske du vill sätt tooeasily lägga till, ta bort, starta (tillhandahålla) eller stoppa (avetablering) vissa compute-nod virtuella datorer i klustret. toodo dessa uppgifter köra Azure PowerShell-skript som är installerade på hello huvudnod VM. Dessa skript som hjälper dig styra hello antal och tillgängligheten för din HPC Pack klusterresurser så du kan styra kostnader.

> [!IMPORTANT] 
> Den här artikeln gäller bara tooHPC Pack 2012 R2-kluster i Azure som skapats med hjälp av hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.
> Dessutom är hello PowerShell-skript som beskrivs i den här artikeln inte tillgängliga i HPC Pack 2016.

## <a name="prerequisites"></a>Krav
* **HPC Pack 2012 R2-kluster i Azure Virtual Machines**: skapa ett HPC Pack 2012 R2-kluster i hello klassiska distributionsmodellen. Exempelvis kan du automatisera hello distribution med hjälp av hello HPC Pack 2012 R2 VM-avbildning i hello Azure Marketplace och ett Azure PowerShell-skript. Information och krav finns i [skapa ett HPC-kluster med hello HPC Pack IaaS distributionsskriptet](hpcpack-cluster-powershell-script.md).
  
    Efter distributionen kan hitta hello nod hanteringsskript i hello % CCP\_hem % bin-mappen i hello huvudnod. Kör alla hello skript som administratör.
* **Azure publicera certifikat för filen eller hantering av inställningar**: du behöver toodo något av följande hello i hello huvudnod:
  
  * **Importera hello Azure inställningsfilen för publicering**. toodo detta, kör hello följande Azure PowerShell-cmdlets i hello huvudnod:
    
    ```PowerShell
    Get-AzurePublishSettingsFile
    
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
  * **Konfigurera hello Azure-hanteringscertifikatet på hello huvudnod**. Om du har hello .cer-fil, importera det i certifikatarkivet för hello CurrentUser\My och kör sedan hello följande Azure PowerShell-cmdleten för Azure-miljön (AzureCloud eller AzureChinaCloud):
    
    ```PowerShell
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Lägg till Beräkningsnoden virtuella datorer
Lägg till compute-noder med hello **Lägg till HpcIaaSNode.ps1** skript.

### <a name="syntax"></a>Syntax
```PowerShell
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parametrar
* **ServiceName**: namnet på hello molntjänst som nya compute-nod virtuella datorer läggs till.
* **Avbildning**: Azure VM avbildningens namn, som kan hämtas via hello klassiska Azure-portalen eller Azure PowerShell-cmdleten **Get-AzureVMImage**. hello avbildningen måste uppfylla följande krav hello:
  
  1. Ett Windows-operativsystem installeras.
  2. HPC Pack måste vara installerad i hello beräkningsrollen nod.
  3. hello bilden måste vara en privat avbildning i hello användarkategorin, inte en offentlig Azure VM-avbildning.
* **Antal**: antal compute-nod VMs toobe lagts till.
* **InstanceSize**: storleken på hello compute-nod virtuella datorer.
* **DomainUserName**: användaren domännamn som är används toojoin hello ny VMs toohello domän.
* **DomainUserPassword**: lösenordet för hello domänanvändare.
* **NodeNameSeries** (valfritt): namnge mönster för hello compute-noder. hello formatet måste vara &lt; *rot\_namn*&gt;&lt;*starta\_nummer*&gt;%. Till exempel beräkna MyCN % 10%: en serie hello nodnamn från MyCN11. Om inget anges konfigurerats hello skriptet använder hello nod namngivning serien i hello HPC-kluster.

### <a name="example"></a>Exempel
hello följande exempel lägger till 20 storlek stora beräkningsnod virtuella datorer i hello Molntjänsten *hpcservice1*, baserat på hello VM-avbildning *hpccnimage1*.

```PowerShell
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Ta bort beräkningsnod virtuella datorer
Ta bort compute-noder med hello **ta bort HpcIaaSNode.ps1** skript.

### <a name="syntax"></a>Syntax
```PowerShell
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parametrar
* **Namnet**: namn på klustret noder toobe tas bort. Jokertecken stöds. hello parameternamn är namn. Du kan inte ange båda hello **namn** och **nod** parametrar.
* **Noden**: hello HpcNode objekt för hello noder toobe tas bort, som kan hämtas via hello HPC PowerShell-cmdleten [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). hello parameternamn är noden. Du kan inte ange båda hello **namn** och **nod** parametrar.
* **DeleteVHD** (valfritt): Ange toodelete hello associerade diskar för hello virtuella datorer som har tagits bort.
* **Framtvinga** (valfritt): Ange tooforce HPC noder offline innan du tar bort dem.
* **Bekräfta** (valfritt): fråga efter bekräftelse innan hello kommando körs.
* **WhatIf**: Ange toodescribe vad som skulle hända om du körde kommandot hello utan verkligen körs hello-kommando.

### <a name="example"></a>Exempel
hello följande exempel tvingar offline hello noder med namn som börjar *HPCNode-CN -* och de tar bort hello noder och deras associerade diskar.

```PowerShell
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Starta beräkningsnod virtuella datorer
Starta compute-noder med hello **Start HpcIaaSNode.ps1** skript.

### <a name="syntax"></a>Syntax
```PowerShell
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parametrar
* **Namnet**: namnen på hello klustret noder toobe igång. Jokertecken stöds. hello parameternamn är namn. Du kan inte ange båda hello **namn** och **nod** parametrar.
* **Noden**-hello HpcNode objekt för hello noder toobe igång, som kan hämtas via hello HPC PowerShell-cmdleten [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). hello parameternamn är noden. Du kan inte ange båda hello **namn** och **nod** parametrar.

### <a name="example"></a>Exempel
hello följande exempel startar noder med namn som börjar *HPCNode-CN -*.

```PowerShell
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Stoppa beräkningsnod virtuella datorer
Stoppa compute-noder med hello **stoppa HpcIaaSNode.ps1** skript.

### <a name="syntax"></a>Syntax
```PowerShell
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parametrar
* **Namnet**-namnen på hello klustret noder toobe stoppades. Jokertecken stöds. hello parameternamn är namn. Du kan inte ange båda hello **namn** och **nod** parametrar.
* **Noden**: hello HpcNode objekt för hello noder toobe stoppas, som kan hämtas via hello HPC PowerShell-cmdleten [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). hello parameternamn är noden. Du kan inte ange båda hello **namn** och **nod** parametrar.
* **Framtvinga** (valfritt): Ange tooforce HPC noder offline innan du stoppar dem.

### <a name="example"></a>Exempel
hello följande exempel tvingar offline noder med namn som börjar *HPCNode-CN -* och sedan stoppas hello noder.

```PowerShell
Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force
```

## <a name="next-steps"></a>Nästa steg
* tooautomatically växa eller krympa hello klusternoder enligt hello nuvarande arbetsbelastningen för jobb och aktiviteter på hello kluster, se [automatiskt växa eller krympa hello HPC Pack klusterresurser i Azure bl.a toohello klusterarbetsbelastning](hpcpack-cluster-node-autogrowshrink.md).

