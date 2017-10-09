---
title: "aaaVirtual datorn tillägg och funktioner för Linux | Microsoft Docs"
description: "Lär dig vilka tillägg som är tillgängliga för virtuella Azure-datorer, grupperade efter vad de tillhandahålla och förbättra."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: e0d2ce794c76815ccc6743e8788ee5d9d931e9a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Tillägg för virtuell dator och funktioner för Linux

Tillägg för virtuell dator i Azure är små program som innehåller efter distributionen konfiguration och automatisering av uppgifter på virtuella Azure-datorer. Om en virtuell dator kräver installation av programvara, anti-virusskydd eller Docker-konfiguration, VM-tillägget kan exempelvis vara används toocomplete dessa uppgifter. Azure VM-tillägg kan köras med hello Azure CLI, PowerShell, Azure Resource Manager-mallar och hello Azure-portalen. Tillägg kan levereras tillsammans med en ny distribution av virtuella datorer eller köra mot alla befintliga system.

Det här dokumentet innehåller en översikt över krav för att använda Azure VM-tillägg och vägledning om hur toodetect, hantera och ta bort VM-tillägg på VM-tillägg. Det här dokumentet innehåller allmänna informationen eftersom många VM-tillägg är tillgängligt, var och en med en potentiellt unik konfiguration. Tillägget-specifik information finns i varje dokument specifika toohello enskilda tillägg.

## <a name="use-cases-and-samples"></a>Användningsfall och exempel

Det finns flera olika Virtuella Azure-tillägg, var och en med en specifik användningsfall. Några exempel är:

- Använd PowerShell önskade konfigurationer tooa virtuella datorn med hjälp av hello DSC-tillägg för Linux. Mer information finns i [tillägg för konfiguration av Azure önskade tillstånd](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- Konfigurera övervakning av en virtuell dator med hello Microsoft Monitoring Agent VM-tillägget. Mer information finns i [hur toomonitor en Linux VM](tutorial-monitoring.md).
- Konfigurera övervakning av Azure-infrastrukturen med hello Datadog tillägg. Mer information finns i hello [Datadog blogg](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- Konfigurera en Docker-värd på en virtuell Azure-dator med hjälp av hello Docker VM-tillägget. Mer information finns i [Docker VM-tillägget](dockerextension.md).

Dessutom tooprocess-specifika tillägg, ett anpassat skript tillägg är tillgängligt för både Windows och Linux virtuella datorer. hello tillägget för anpassat skript för Linux kan alla Bash-skript toobe som körs på en virtuell dator. Anpassade skript är användbara för att utforma Azure-distributioner som kräver konfiguration efter vilken interna Azure verktygsuppsättning kan ge. Mer information finns i [tillägget för anpassat skript för Linux Virtuella](extensions-customscript.md).


## <a name="prerequisites"></a>Krav

Varje tillägg för virtuell dator kan ha en egen uppsättning krav. Hej Docker VM-tillägget har till exempel en förutsättning för en Linux-fördelning som stöds. Krav av enskilda tillägg beskrivs i dokumentationen för hello-tillägg-specifika.

### <a name="azure-vm-agent"></a>Virtuell Azure-datoragent

hello Azure VM-agenten hanterar samverkan mellan en virtuell Azure-dator och hello Azure-infrastrukturkontrollanten. hello VM-agenten är ansvarig för många funktioner för att distribuera och hantera virtuella Azure-datorer, inklusive kör VM-tillägg. hello Azure VM-agenten är förinstallerat på Azure Marketplace-bilder och kan installeras manuellt på operativsystem som stöds.

Information om operativsystem som stöds och installationsanvisningar finns [virtuella Azure-datorn agent](../windows/classic/agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Identifiera VM-tillägg

Det finns många olika VM-tillägg för användning med virtuella Azure-datorer. toosee en fullständig lista, kör hello följande kommando med hello Azure CLI, ersätta hello Exempelplats med hello plats.

```azurecli
az vm extension image list --location westus -o table
```

## <a name="run-vm-extensions"></a>Kör VM-tillägg

Tillägg för virtuell Azure-dator kan köras på befintliga virtuella datorer som är användbara när du behöver toomake konfigurationsändringar eller återställa anslutningen på en redan distribuerad virtuell dator. VM-tillägg kan också tillsammans med Azure Resource Manager mall-distributioner. Genom att använda tillägg med Resource Manager-mallar, virtuella Azure-datorer distribuerats och konfigurerats utan åtgärder från efter distributionen.

hello följande metoder kan vara används toorun filnamnstillägget mot en befintlig virtuell dator.

### <a name="azure-cli"></a>Azure CLI

Tillägg för virtuell Azure-dator kan köras mot en befintlig virtuell dator med hjälp av hello `az vm extension set` kommando. Det här exemplet körs hello tillägget för anpassat skript mot en virtuell dator.

```azurecli
az vm extension set `
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

hello skriptet ger utdata liknande toohello följande text:

```azurecli
info:    Executing command vm extension set
+ Looking up hello VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Azure Portal

VM-tillägg kan vara tillämpade tooan befintliga virtuella datorn via hello Azure-portalen. toodo Välj därför hello virtuell dator, Välj **tillägg**, och klicka på **Lägg till**. Välj hello tillägg du vill använda hello listan över tillgängliga tillägg och följ instruktionerna hello i hello guiden.

hello visar följande bild hello installation av hello Linux anpassade skripttillägg från hello Azure-portalen.

![Installera tillägget för anpassat skript](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-mallar

VM-tillägg kan vara tillagda tooan Azure Resource Manager-mall och utförs med hello distribution av hello mallen. När du distribuerar ett tillägg med en mall kan skapa du helt konfigurerade Azure-distributioner. Till exempel hello följande JSON hämtas från en Resource Manager-mall. hello mallen distribuerar en uppsättning belastningsutjämnade virtuella datorer och en Azure SQL database och installerar sedan en .NET Core-programmet på varje virtuell dator. hello VM-tillägget hand tar om hello Programvaruinstallation.

Mer information finns i hello fullständig [Resource Manager-mall](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

Mer information finns i [redigera Azure Resource Manager-mallar](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).

## <a name="secure-vm-extension-data"></a>Säkra data för VM-tillägg

När du kör en VM-tillägget, kan det vara nödvändigt tooinclude känslig information, till exempel autentiseringsuppgifter, lagringskontonamn och åtkomst för lagringskontonycklar. Många VM-tillägg innehåller en skyddad konfiguration som krypterar data och dekrypterar dem bara inuti hello virtuella måldatorn. Varje tillägg innehåller en specifik skyddade konfigurationsschema och varje beskrivs i dokumentationen för specifika tillägg.

hello som följande exempel visar en instans av hello tillägget för anpassat skript för Linux. Observera att hello kommandot tooexecute innehåller en uppsättning autentiseringsuppgifter. I det här exemplet kommer hello kommandot tooexecute inte krypteras.


```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Flytta hello **kommandot tooexecute** egenskapen toohello **skyddade** konfigurationen skyddar hello körning sträng.

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

## <a name="troubleshoot-vm-extensions"></a>Felsökning av VM-tillägg

Varje VM-tillägg kan ha felsökning steg specifika toohello tillägg. Till exempel när du använder hello tillägget för anpassat skript finns körning skriptdetaljer lokalt på hello virtuell dator där hello tillägget kördes. Tillägget-specifika felsökning beskrivs i dokumentationen för tillägget-specifika.

hello följande felsökningssteg gäller tooall tillägg för virtuell dator.

### <a name="view-extension-status"></a>Visa status för tillägg

När ett tillägg för virtuell dator har körts mot en virtuell dator, kan du använda hello följande Azure CLI kommandot tooreturn tillståndets status. Ersätt exempel parameternamn med egna värden.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

hello utdata ser ut så hello följande text:

```azurecli
AutoUpgradeMinorVersion    Location    Name          ProvisioningState    Publisher                   ResourceGroup      TypeHandlerVersion  VirtualMachineExtensionType
-------------------------  ----------  ------------  -------------------  --------------------------  ---------------  --------------------  -----------------------------
True                       westus      customScript  Succeeded            Microsoft.Azure.Extensions  exttest                             2  customScript
```

Tillståndets status är körningen kan också finnas i hello Azure-portalen. tooview hello status för ett tillägg markerar hello virtuell dator, Välj **tillägg**, och välj hello önskade tillägg.

### <a name="rerun-a-vm-extension"></a>Kör en VM-tillägget

Det kan finnas fall där tillägget för virtuell dator måste toobe kör igen. Du kan köra ett tillägg genom att ta bort den och sedan köra hello-tillägget med en körning metod du föredrar. tooremove ett tillägg, kör följande kommando med hello Azure CLI hello. Ersätt exempel parameternamn med egna värden.

```azurecli
az vm extension delete --name customScript --resource-group myResourceGroup --vm-name myVM
```

Du kan ta bort ett tillägg genom att använda hello följa stegen i hello Azure-portalen:

1. Välj en virtuell dator.
2. Välj **tillägg**.
3. Välj önskad hello tillägg.
4. Välj **avinstallera**.

## <a name="common-vm-extension-reference"></a>Benämningen för VM-tillägg
| Tilläggsnamn | Beskrivning | Mer information |
| --- | --- | --- |
| Tillägget för anpassat skript för Linux |Kör skript mot en virtuell Azure-dator |[Tillägget för anpassat skript för Linux](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Docker-tillägg |Installera hello Docker daemon toosupport Docker fjärrkommandon. |[Docker VM-tillägg](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| VM Access-tillägg |Återfå åtkomst tooan virtuella Azure-datorn |[VM Access-tillägg](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Azure Diagnostics-tillägget |Hantera Azure-diagnostik |[Azure Diagnostics-tillägget](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure VM Access-tillägg |Hantera användare och autentiseringsuppgifter |[Tillägget för virtuell dator åtkomst för Linux](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
