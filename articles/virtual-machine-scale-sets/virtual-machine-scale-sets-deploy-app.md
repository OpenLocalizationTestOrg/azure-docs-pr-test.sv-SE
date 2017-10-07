---
title: "Anger aaaDeploy en app på den virtuella datorn"
description: "Använd tillägg toodepoy en app på Azure Virtual Machine-Skalningsuppsättningar."
services: virtual-machine-scale-sets
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.openlocfilehash: 5f3988b9511d80370a8be1fc042c21fee212506e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Distribuera programmet på virtuella datorer

Den här artikeln beskrivs olika sätt att hur tooinstall software hello hello skala in etableras.

Du kanske vill tooreview hello [skala ange översikt över Design](virtual-machine-scale-sets-design-overview.md) artikel som beskriver vissa av hello begränsningar har införts av virtuella datorer.

## <a name="capture-and-reuse-an-image"></a>Fånga och återanvända en avbildning

Du kan använda en virtuell dator som du har i Azure tooprepare en bas-avbildning för din skala angett. Den här processen skapar hanterade diskar i ditt lagringskonto som du kan referera till som hello basavbildning för din skaluppsättning. 

Hej gör du följande steg:

1. Skapa en virtuell Azure-dator
   * [Linux][linux-vm-create]
   * [Windows][windows-vm-create]

2. Fjärråtkomst till hello virtuell dator och anpassa hello system tooyour önskemål.

   Om du vill kan installera du programmet nu. Dock vet att genom att installera programmet nu, kan du uppgradera ditt program som är mer komplicerad eftersom du kan behöva tooremove den första. I stället kan du använda det här steget tooinstall allt ditt program kan behöva, som en specifik funktion för körning eller operativsystem.

3. Följ hello ”skapar du en dator” självstudier för antingen [Linux] [ linux-vm-capture] eller [Windows][windows-vm-capture].

4. Skapa en [Skaluppsättning för virtuell dator] [ vmss-create] med hello bild-URI som du har hämtat i hello föregående steg.

Mer information om diskar finns [översikt för hanterade diskar](../virtual-machines/windows/managed-disks-overview.md) och [Använd kopplade Datadiskar](virtual-machine-scale-sets-attached-disks.md).

## <a name="install-when-hello-scale-set-is-provisioned"></a>Installera när hello skaluppsättning har etablerats

Tillägg för virtuell dator kan vara tillämpas tooa virtuella datorns skaluppsättning. Du kan anpassa hello virtuella datorer i en skala som angetts som en hel grupp med ett tillägg för virtuell dator. Mer information om tillägg finns [virtuella Datortillägg](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Det finns tre huvudsakliga tillägg som du kan använda, beroende på om ditt operativsystem är Linux- eller Windows-baserade.

### <a name="windows"></a>Windows

För en Windows-baserade operativsystem, använder du antingen hello **anpassat skript v1.8** tillägg eller hello **PowerShell DSC** tillägg.

#### <a name="custom-script"></a>Anpassat skript

hello tillägget för anpassat skript körs ett skript på varje virtuell datorinstans i hello skaluppsättning. Anger vilka filer som är hämtade toohello virtuella datorn och sedan vilken kommandot Kör en .config-fil eller en variabel. Du kan använda den här toorun ett installationsprogram, ett skript, en kommandofil, körbara filer till exempel.

PowerShell använder en hash-tabell för hello inställningar. Det här exemplet konfigureras hello anpassat skript tillägget toorun ett PowerShell.skript som installerar IIS.

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Använd hello `-ProtectedSetting` växla för alla inställningar som kan innehålla känslig information.

---------


Azure CLI använder en json-fil för hello inställningar. Det här exemplet konfigureras hello anpassat skript tillägget toorun ett PowerShell.skript som installerar IIS. Spara hello följande json-fil som _settings.json_.

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1"
  ],
  "commandToExecute": "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> \"%TEMP%\StartupLog.txt\" 2>&1"
}
```

Kör sedan kommandot Azure CLI.

```azurecli
az vmss extension set --publisher Microsoft.Compute --version 1.8 --name CustomScriptExtension --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Använd hello `--protected-settings` växla för alla inställningar som kan innehålla känslig information.

### <a name="powershell-dsc"></a>PowerShell DSC

Du kan använda PowerShell DSC toocustomize hello scale set vm-instanser. Hej **DSC** tillägg som publicerats av **Microsoft.Powershell** distribuerar och kör hello tillhandahålls DSC-konfigurationen på varje virtuell dator-instans. En .config-fil eller en variabel visar hello tillägget där *.zip* paketet är och vilka _skript-funktionen_ kombination toorun.

PowerShell använder en hash-tabell för hello inställningar. Det här exemplet används ett DSC-paket som installerar IIS.

```powershell
# Setup extension configuration hashtable variable
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Använd hello `-ProtectedSetting` växla för alla inställningar som kan innehålla känslig information.

-----------

Azure CLI använder en json för hello inställningar. Det här exemplet används ett DSC-paket som installerar IIS. Spara hello följande json-fil som _settings.json_.

```json
{
  "wmfVersion": "latest",
  "configuration": {
    "url": "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip",
    "script": "configure-http.ps1",
    "function": "WebsiteTest"
  }
}
```

Kör sedan kommandot Azure CLI.

```azurecli
az vmss extension set --publisher Microsoft.Powershell --version 2.24 --name DSC --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Använd hello `--protected-settings` växla för alla inställningar som kan innehålla känslig information.

### <a name="linux"></a>Linux

Linux kan använda antingen hello **anpassat skript v2.0** tillägg eller Använd **moln init** under skapandet.

Anpassat skript är en enkel tillägg som laddar ned filer toohello virtuella instanser och kör ett kommando.

#### <a name="custom-script"></a>Anpassat skript

Spara hello följande json-fil som _settings.json_.

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

Använd hello Azure CLI tooadd det här tillägget tooan befintliga virtuella datorn. Varje virtuell dator i hello skala in automatiskt körs hello tillägg.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Använd hello `--protected-settings` växla för alla inställningar som kan innehålla känslig information.

#### <a name="cloud-init"></a>Molnet initiering

Molnet Init används när hello scale set skapas. Skapa först en lokal fil med namnet _moln init.txt_ och Lägg till din konfiguration tooit. Se exempelvis [denna gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)

Använd hello Azure CLI toocreate en skala har angetts. Hej `--custom-data` fältet accepterar hello filnamnet för ett moln init-skript.

```azurecli
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image Canonical:UbuntuServer:14.04.4-LTS:latest \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

## <a name="how-do-i-manage-application-updates"></a>Hur hanterar programuppdateringar?

Om du har distribuerat ditt program via ett tillägg alter hello resurstilläggsdefinitionen på något sätt. Den här ändringen gör hello tillägget toobe omdistribueras tooall virtuella instanser. Något **måste** ändras om hello filnamnstillägg, till exempel byta namn på en refererade filen, annars Azure har inte se som hello tillägg har ändrats.

Om du inbyggd hello program i din egen avbildning av operativsystemet kan du använda en pipeline för automatisk distribution för programuppdateringar. Utforma din arkitektur toofacilitate snabb växling av en stegvis skala i produktion. Ett bra exempel på den här metoden är hello [Azure Spinnaker drivrutinen arbete](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

[Packare](https://www.packer.io/) och [Terraform](https://www.terraform.io/) stöd för Azure Resource Manager, så att du kan definiera dina bilder ”som kod” och skapar dem i Azure, sedan använda hello VHD i en skaluppsättning. Dock skulle göra så bli problematisk för marketplace-bilder, där tillägg/anpassade skript bli viktigare eftersom du inte direkt ändra bits från marketplace.

## <a name="what-happens-when-a-scale-set-scales-out"></a>Vad händer när en skaluppsättning för skalor ut?
Hello programmet installeras automatiskt när du lägger till en eller flera virtuella datorer tooa skaluppsättning. För exempel om hello skalan har tillägg som definierats kan köras de på en ny virtuell dator när den skapas. Om hello skaluppsättning baseras på en anpassad avbildning är en kopia av anpassade hello Källavbildningen eventuella nya virtuella datorer. Om hello skala uppsättning virtuella datorer är värdar för behållaren, kanske du har Start kod tooload hello behållare i tillägget för anpassat skript. Eller ett tillägg kan installera en agent som registreras med en orchestrator för klustret, till exempel Azure Container Service.


## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Hur du lanserar en OS-uppdatering update domäner?
Anta att du vill tooupdate OS-avbildningen samtidigt som hello virtuella skaluppsättningen körs. PowerShell och hello Azure CLI kan uppdatera hello avbildningar av virtuella datorer, en virtuell dator i taget. Hej [uppgradera en Virtual Machine Scale Set](./virtual-machine-scale-sets-upgrade-scale-set.md) artikeln innehåller även ytterligare information om vilka alternativ som är tillgängliga tooperform uppgraderar du ett operativsystem i en skaluppsättning för virtuell dator.

## <a name="next-steps"></a>Nästa steg

* [Använd PowerShell toomanage din skaluppsättning.](virtual-machine-scale-sets-windows-manage.md)
* [Skapa en mall för skalan.](virtual-machine-scale-sets-mvss-start.md)


[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md

