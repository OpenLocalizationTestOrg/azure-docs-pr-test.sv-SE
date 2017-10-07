---
title: "aaaVerify trafik med Azure Network Watcher IP flöda Kontrollera - Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur toocheck om trafik tooor från en virtuell dator tillåts eller nekas med Azure CLI"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 92b857ed-c834-4c1b-8ee9-538e7ae7391d
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 128a00b4296994551e7e17838a51e6d9de180e21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Kontrollera om trafik som tillåts eller nekas tooor från en virtuell dator med IP-flöda Kontrollera en komponent i Azure Nätverksbevakaren

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST-API](network-watcher-check-ip-flow-verify-rest.md)


IP-flöda verifiera är en funktion i Nätverksbevakaren som gör att du tooverify om trafik tillåts tooor från en virtuell dator. Det här scenariot är användbart tooget aktuella tillstånd om en virtuell dator kan prata tooan extern resurs eller backend. IP-flöde verifiera går att använda tooverify om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler. En annan orsak till att använda IP flödet Kontrollera tooensure trafik som du vill blockerade blockeras korrekt av hello NSG.

Den här artikeln använder våra nästa generations CLI för hello management resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.

tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Innan du börjar

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren eller har en befintlig instans av Nätverksbevakaren. hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.

## <a name="scenario"></a>Scenario

Det här scenariot använder IP-flöda Kontrollera tooverify om en virtuell dator kan prata tooa kända Bing IP-adress. Om hello trafik nekas returnerar hello säkerhetsregel som nekar trafiken. toolearn mer om IP-flöda Kontrollera finns [flöda Kontrollera översikt över IP](network-watcher-ip-flow-verify-overview.md)

## <a name="get-a-vm"></a>Hämta en virtuell dator

IP-flöde verifiera tester trafik tooor från en IP-adress på en virtuell dator tooor från en extern enhet. Ett Id för en virtuell dator krävs för hello cmdlet. Om du redan vet hello-ID för hello virtuella toouse kan du hoppa över det här steget.

```azurecli
az vm show --resource-group MyResourceGroup5431 --name MyVM-Web
```

## <a name="get-hello-nics"></a>Hämta hello nätverkskort

hello IP-adress till ett nätverkskort på den virtuella datorn hello krävs i det här exemplet vi hämta hello nätverkskort på en virtuell dator. Om du redan vet hello IP-adress som du vill tootest på hello virtuell dator kan du hoppa över det här steget.

```azurecli
az network nic show --resource-group MyResourceGroup5431 --name MyNic-Web
```

## <a name="run-ip-flow-verify"></a>Kontrollera kör IP-flöde

Nu när vi har hello information behövs toorun hello cmdlet vi kör hello `az network watcher test-ip-flow` cmdlet tootest hello trafik. I det här exemplet använder vi hello första IP-adressen på hello första nätverkskortet.

```azurecli
az network watcher test-ip-flow --resource-group resourceGroupName --direction directionInboundorOutbound --protocol protocolTCPorUDP --local ipAddressandPort --remote ipAddressandPort --vm vmNameorID --nic nicNameorID
```

> [!NOTE]
> IP-flöda Kontrollera kräver att hello Virtuella datorresursen fördelas toorun.

## <a name="review-results"></a>Granska resultaten

När du har kört `az network watcher test-ip-flow` hello resultaten returneras, hello följande exempel är hello resultaten som returnerades från hello föregående steg.

```azurecli
{
    "access": "Allow",
    "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

## <a name="next-steps"></a>Nästa steg

Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello network security grupp och säkerhet regler som har definierats.

Läs tooaudit NSG-inställningarna genom att besöka [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md).

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
