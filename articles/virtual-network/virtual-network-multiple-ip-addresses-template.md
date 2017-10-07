---
title: "aaaMultiple IP-adresser för virtuella datorer i Azure - mall | Microsoft Docs"
description: "Lär dig hur tooassign flera IP-adresser tooa virtuell dator med en Azure Resource Manager-mall."
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/08/2016
ms.author: jdial
ms.openlocfilehash: e7660257b2d5c7da4b8b86771abe51a2c5012fa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-an-azure-resource-manager-template"></a>Tilldela flera IP-adresser toovirtual datorer med en Azure Resource Manager-mall

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Den här artikeln förklarar hur toocreate en virtuell dator (VM) via hello Azure Resource Manager distribution modellen med hjälp av en Resource Manager-mall. Flera offentliga och privata IP-adresser kan inte tilldelas toohello samma NIC när du distribuerar en virtuell dator via hello klassiska distributionsmodellen. Mer om Azure distributionsmodeller läsa hello toolearn [förstår distributionsmodellerna](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name="template-description"></a>Beskrivning av mall

Distribuera en mall kan du tooquickly och skapa konsekvent Azure-resurser med olika konfigurationsvärden. Läs hello [genomgång av Resource Manager-mall](../azure-resource-manager/resource-manager-template-walkthrough.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel om du inte är bekant med Azure Resource Manager-mallar. Hej [distribuera en virtuell dator med flera IP-adresser](https://azure.microsoft.com/resources/templates/101-vm-multiple-ipconfig) mall används i den här artikeln.

<a name="resources"></a>Distribuera skapas hello hello följande resurser:

|Resurs|Namn|Beskrivning|
|---|---|---|
|Nätverksgränssnitt|*myNic1*|hello tre IP-konfigurationer som beskrivs i scenariot hello i den här artikeln skapas och tilldelas toothis nätverkskort.|
|Offentliga IP-adressresurs|2 skapas: *myPublicIP* och *myPublicIP2*|Dessa resurser tilldelas statiska offentliga IP-adresser och tilldelas toohello *IPConfig-1* och *IPConfig-2* IP-konfigurationer som beskrivs i hello scenario.|
|Virtuell dator|*myVM1*|Standard DS3 VM.|
|Virtuellt nätverk|*myVNet1*|Ett virtuellt nätverk med ett undernät med namnet *mySubnet*.|
|Lagringskonto|Unik toohello distribution|Ett lagringskonto.|

<a name="parameters"></a>När du distribuerar hello mall måste du ange värden för hello följande parametrar:

|Namn|Beskrivning|
|---|---|
|adminUsername|Administratörsanvändarnamnet. hello användarnamnet måste vara kompatibel med [användarnamn för Azure krav](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|
|adminPassword|Administratörslösenordet lösenord hello måste vara kompatibel med [Azure lösenordskrav](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
|dnsLabelPrefix|DNS-namnet för PublicIPAddressName1. hello DNS-namnet ska matcha tooone hello offentliga IP-adresser som tilldelats toohello VM. hello namn måste vara unika inom hello Azure region (plats) som du skapar hello VM i.|
|dnsLabelPrefix1|DNS-namnet för PublicIPAddressName2. hello DNS-namnet ska matcha tooone hello offentliga IP-adresser som tilldelats toohello VM. hello namn måste vara unika inom hello Azure region (plats) som du skapar hello VM i.|
|OSVersion|hello Windows-/ Linux-version för hello VM. hello operativsystem är en fullständigt uppdaterad bild av hello angivna valda Windows-/ Linux-versionen.|
|imagePublisher|hello Windows-/ Linux avbildningens utgivare för hello valt VM.|
|imageOffer|hello Windows-/ Linux-avbildning för hello valt VM.|

Var och en av hello resurser har distribuerats av hello mallen har konfigurerats med flera standardinställningar. Du kan visa dessa inställningar via antingen hello följande metoder:

- **Visa hello mall på GitHub:** om du är bekant med mallar kan du visa hello inställningarna i hello [mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json).
- **Visa hello inställningar när du har distribuerat:** om du inte är bekant med mallar kan du distribuera hello mallen med hjälp av stegen i något av följande avsnitt hello och sedan visar hello inställningar efter distributionen.

Du kan använda hello Azure-portalen, PowerShell eller hello Azure-kommandoradsgränssnittet (CLI) toodeploy hello mallen. Alla metoderna ge hello samma resultat. toodeploy hello mall klar hello stegen i en hello följande avsnitt:

## <a name="deploy-using-hello-azure-portal"></a>Distribuera med hello Azure-portalen

toodeploy hello mallen med hjälp av hello Azure-portalen fullständig hello följande steg:

1. Ändra hello mall, om så önskas. hello mallen distribuerar hello resurser och inställningar som anges i hello [resurser](#resources) i den här artikeln. toolearn mer om mallar och hur tooauthor dem, skrivskyddade hello [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-network%2ftoc.json)artikel.
2. Distribuera hello mallen med en hello följande metoder:
    - **Välj hello mall i hello portal:** fullständig hello stegen i hello [distribuera resurser från anpassade mall](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-resources-from-custom-template) artikel. Välj hello befintlig mall med namnet *101-vm-flera-ipconfig*.
    - **Direkt:** klickar du på hello efter knappen tooopen hello mall direkt i hello portal:<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-vm-multiple-ipconfig%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Oavsett hello metod du väljer, du behöver toosupply värden för hello [parametrar](#parameters) som angavs tidigare i den här artikeln. När hello VM har distribuerats kan ansluta toohello VM och Lägg till hello privata IP-adresser toohello operativsystem du har distribuerat genom att slutföra hello stegen i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln. Lägg inte till hello offentliga IP-adresser toohello operativsystem.

## <a name="deploy-using-powershell"></a>Distribuera med hjälp av PowerShell

toodeploy hello mallen med hjälp av PowerShell fullständig hello följande steg:

1. Distribuera hello mallen genom att slutföra hello stegen i hello [distribuera en mall med PowerShell](../azure-resource-manager/resource-group-template-deploy-cli.md) artikel. hello beskrivs flera alternativ för att distribuera en mall. Om du väljer toodeploy med hello `-TemplateUri parameter`, hello URI för den här mallen är *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Om du väljer toodeploy med hello `-TemplateFile` parameter, kopiera hello innehållet i hello [mallfilen](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) från GitHub i en ny fil på din dator. Ändra hello mallen innehåll, om så önskas. hello mallen distribuerar hello resurser och inställningar som anges i hello [resurser](#resources) i den här artikeln. toolearn mer om mallar och hur tooauthor dem, skrivskyddade hello [redigera Azure Resource Manager-mallar ](../azure-resource-manager/resource-group-authoring-templates.md)artikel.

    Oavsett hello alternativ du väljer toodeploy hello mall med du anger värden för hello parametervärden som anges i hello [parametrar](#parameters) i den här artikeln. Om du väljer toosupply parametrar med en fil med parametrar, kopiera hello innehållet på hello [parameterfilen](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) från GitHub i en ny fil på din dator. Ändra hello värden i hello-filen. Använd hello-fil som du skapade som hello värde hello `-TemplateParameterFile` parameter.

    toodetermine giltiga värden för hello OSVersion, ImagePublisher och imageOffer parametrar, fullständig hello stegen i hello [navigera och välj Windows VM-avbildningar artikel](../virtual-machines/windows/cli-ps-findimage.md) artikel.

    >[!TIP]
    >Om du inte vet om det finns en dnslabelprefix anger hello `Test-AzureRmDnsAvailability -DomainNameLabel <name-you-want-to-use> -Location <location>` kommandot toofind ut. Om det är tillgängligt hello kommandot returnerar `True`.

2. När hello VM har distribuerats kan ansluta toohello VM och Lägg till hello privata IP-adresser toohello operativsystem du har distribuerat genom att slutföra hello stegen i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln. Lägg inte till hello offentliga IP-adresser toohello operativsystem.

## <a name="deploy-using-hello-azure-cli"></a>Distribuera med hello Azure CLI

toodeploy hello mallen med hjälp av hello Azure CLI, version 1.0, fullständig hello följande steg:

1. Distribuera hello mallen genom att slutföra hello stegen i hello [distribuera en mall med hello Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) artikel. hello beskrivs flera alternativ för att distribuera hello mallen. Om du väljer toodeploy med hello `--template-uri` (-f), hello URI för den här mallen är *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Om du väljer toodeploy med hello `--template-file` (-f) parameter, kopiera hello innehållet i hello [mallfilen](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) från GitHub i en ny fil på din dator. Ändra hello mallen innehåll, om så önskas. hello mallen distribuerar hello resurser och inställningar som anges i hello [resurser](#resources) i den här artikeln. toolearn mer om mallar och hur tooauthor dem, skrivskyddade hello [redigera Azure Resource Manager-mallar ](../azure-resource-manager/resource-group-authoring-templates.md)artikel.

    Oavsett hello alternativ du väljer toodeploy hello mall med du anger värden för hello parametervärden som anges i hello [parametrar](#parameters) i den här artikeln. Om du väljer toosupply parametrar med en fil med parametrar, kopiera hello innehållet på hello [parameterfilen](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) från GitHub i en ny fil på din dator. Ändra hello värden i hello-filen. Använd hello-fil som du skapade som hello värde hello `--parameters-file` (-e) parameter.

    toodetermine giltiga värden för hello OSVersion, ImagePublisher och imageOffer parametrar, fullständig hello stegen i hello [navigera och välj Windows VM-avbildningar artikel](../virtual-machines/windows/cli-ps-findimage.md) artikel.

2. När hello VM har distribuerats kan ansluta toohello VM och Lägg till hello privata IP-adresser toohello operativsystem du har distribuerat genom att slutföra hello stegen i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln. Lägg inte till hello offentliga IP-adresser toohello operativsystem.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
