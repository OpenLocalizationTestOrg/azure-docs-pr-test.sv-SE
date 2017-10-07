---
title: "aaaOMS tillägg för Azure virtuell dator för Linux | Microsoft Docs"
description: "Distribuera hello OMS-agent på Linux-dator som använder ett tillägg för virtuell dator."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: 0fc8003d1fae6c043eef18ae78d12f9304b70832
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="oms-virtual-machine-extension-for-linux"></a>OMS tillägg för virtuell dator för Linux

## <a name="overview"></a>Översikt

Operations Management Suite (OMS) ger funktioner för övervakning, aviseringar, och reparationen i molnet och lokala resurser. hello tillägg för virtuell dator i OMS-Agent för Linux publiceras och stöds av Microsoft. hello-tillägget hello OMS-agent installeras på virtuella Azure-datorer och registrerar virtuella datorer i en befintlig OMS-arbetsyta. Det här dokumentet information hello stöds plattformar, konfigurationer och distributionsalternativ för hello OMS-tillägg för virtuell dator för Linux.

## <a name="prerequisites"></a>Krav

### <a name="operating-system"></a>Operativsystem

hello tillägget OMS-Agent kan köras mot dessa Linux-distributioner.

| Distribution | Version |
|---|---|
| CentOS Linux | 5, 6 och 7 |
| Oracle Linux | 5, 6 och 7 |
| Red Hat Enterprise Linux Server | 5, 6 och 7 |
| Debian GNU/Linux | 6, 7 och 8 |
| Ubuntu | 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS |
| SUSE Linux Enterprise Server | 11 och 12 |

### <a name="internet-connectivity"></a>Internetanslutning

hello tillägget OMS-Agent för Linux kräver att hello mål den virtuella datorn är ansluten toohello internet. 

## <a name="extension-schema"></a>Tilläggsschema

hello visar följande JSON hello schemat för hello tillägget OMS-Agent. hello tillägget kräver hello-ID och arbetsytan arbetsytenyckel från hello mål OMS-arbetsytan; Dessa värden finns i hello OMS-portalen. Eftersom hello arbetsytenyckel ska behandlas som känsliga data, bör det lagras i en Inställningskonfiguration för skyddade. Azure VM tillägget skyddade inställningsdata krypteras och dekrypteras endast på hello virtuella måldatorn. Observera att **workspaceId** och **workspaceKey** är skiftlägeskänsliga.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a>Egenskapsvärden

| Namn | Värdet / exempel |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Publisher | Microsoft.EnterpriseCloud.Monitoring |
| typ | OmsAgentForLinux |
| typeHandlerVersion | 1.4 |
| workspaceId (t.ex.) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (t.ex.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ == |


## <a name="template-deployment"></a>Malldistribution

Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar. Mallar är perfekt när du distribuerar en eller flera virtuella datorer som kräver post distributionskonfiguration, till exempel onboarding tooOMS. En Resource Manager-mall som innehåller hello OMS-agenten VM-tillägget kan hittas på hello [Azure Quick Start-galleriet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

hello JSON-konfiguration för ett tillägg för virtuell dator kan kapslas i hello virtuell datorresurs eller placeras på hello rot- eller översta nivån i en Resource Manager JSON-mall. hello placeringen av hello JSON configuration påverkar hello värdet hello resursens namn och typ. Mer information finns i [ange namn och typ för underordnade resurser](../../azure-resource-manager/resource-manager-template-child-resource.md). 

hello förutsätter följande exempel hello OMS-tillägget är kapslad i hello virtuella datorresurser. När för många kapslade hello tillägget resurs, hello JSON placeras i hello `"resources": []` objekt av hello virtuell dator.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

När du monterar hello tillägget JSON hello roten i hello mallen hello resursnamnet innehåller en referens toohello överordnad virtuell dator och hello typen visar hello kapslade konfiguration.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a>Azure CLI-distribution

hello Azure CLI kan vara används toodeploy hello OMS-agenten VM-tillägget tooan befintlig virtuell dator. Ersätt hello OMS-nyckel och OMS-ID med de från din OMS-arbetsyta. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Felsöka och stöd

### <a name="troubleshoot"></a>Felsöka

Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure CLI. toosee hello distributionsstatusen för tillägg för en viss virtuell dator, kör följande kommando använder hello hello Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Tillägget körning utdata är loggade toohello följande:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Felkoder och deras innebörd

| Felkod | Betydelse | Möjlig åtgärd |
| :---: | --- | --- |
| 10 | Virtuella datorn är redan anslutet tooan OMS-arbetsyta | tooconnect hello VM toohello arbetsytan som angetts i hello tillägget schemat, ange stopOnMultipleConnections toofalse i inställningar för offentliga eller ta bort den här egenskapen. Den här virtuella datorn hämtar debiteras när för varje arbetsyta som den är ansluten till. |
| 11 | Ogiltig config tillhandahålls toohello tillägg | Följ hello föregående exempel tooset alla egenskapsvärden som krävs för distributionen. |
| 12 | Hej dpkg Pakethanteraren är låst | Kontrollera att alla dpkg uppdateringsåtgärder på hello datorn har slutförts och försök igen. |
| 20 | Aktivera kallas för tidigt | [Uppdatera hello Azure Linux-agenten](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) toohello senaste tillgängliga versionen. |
| 51 | Det här tillägget stöds inte på hello VM-operativsystemet | |
| 55 | Det går inte att ansluta toohello Microsoft Operations Management Suite-tjänsten | Kontrollera att hello systemet antingen har Internetåtkomst eller att en giltig HTTP-proxy har tillhandahållits. Kontrollera också hello är korrekt hello arbetsyte-ID. |

Mer information om felsökning finns på hello [OMS-Agent för Linux felsökningsguide](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).

### <a name="support"></a>Support

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support. Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).
