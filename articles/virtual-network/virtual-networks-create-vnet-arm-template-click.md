---
title: "aaaCreate ett virtuellt nätverk | Azure Resource Manager-mall | Microsoft Docs"
description: "Lär dig hur toocreate ett virtuellt nätverk med en Azure Resource Manager-mall."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 69530861-2f97-4a6e-b336-a7baf2690044
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b9c289433ff2a84bec19eac25fa28ab40d131c7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-an-azure-resource-manager-template"></a>Skapa ett virtuellt nätverk med en Azure Resource Manager-mall

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure har två distributionsmodeller: Azure Resource Manager och klassisk. Microsoft rekommenderar att skapa resurser via hello Resource Manager-modellen. Mer om toolearn hello skillnaderna mellan hello två modeller, läsa hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) artikel.
 
Den här artikeln förklarar hur toocreate ett VNet via hello Resource Manager distribution modellen med en Azure Resource Manager-mall. Du kan också skapa ett VNet via Resource Manager med andra verktyg eller skapa ett VNet via hello klassiska distributionsmodellen genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
- [Portal](virtual-networks-create-vnet-arm-pportal.md)
- [PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [CLI](virtual-networks-create-vnet-arm-cli.md)
- [Mall](virtual-networks-create-vnet-arm-template-click.md)
- [Portal (klassisk)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (klassisk)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [CLI (klassisk)](virtual-networks-create-vnet-classic-cli.md)

Du får lära dig hur toodownload och ändra befintliga ARM-mall från GitHub och distribuerar hello-mall från GitHub, PowerShell och hello Azure CLI.

Om du bara distribuerar hello ARM-mallen direkt från GitHub utan ändringar, hoppar du över för[distribuera en mall från github](#deploy-the-arm-template-by-using-click-to-deploy).

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="download-and-understand-hello-azure-resource-manager-template"></a>Hämta och förstå hello Azure Resource Manager-mall
Du kan hämta hello befintlig mall för att skapa ett VNet och två undernät från GitHub, göra ändringar du vill och återanvända den. toodo slutföra så hello följande steg:

1. Navigera för[hello exempelmallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Klicka på **azuredeploy.json** och klicka sedan på **RAW**.
3. Spara hello filen tooa en lokal mapp på datorn.
4. Om du är bekant med mallar kan du hoppa över toostep 7.
5. Öppna hello-filen som du just har sparat och titta på hello innehållet under **parametrar** på rad 5. ARM-mallparametrarna agerar platshållare för värden som kan anges under distributionen.
   
   | Parameter | Beskrivning |
   | --- | --- |
   | **Plats** |Azure-region där hello VNet kommer att skapas |
   | **vnetName** |Namn för hello nya VNet |
   | **addressPrefix** |Adressutrymmet för hello VNet, i CIDR-format |
   | **subnet1Name** |Namn för hello första VNet |
   | **subnet1Prefix** |CIDR-block för hello första undernätet |
   | **subnet2Name** |Namn för hello andra VNet |
   | **subnet2Prefix** |CIDR-block för hello andra undernätet |
   
   > [!IMPORTANT]
   > Azure Resource Manager-mallar som finns på GitHub kan ändras med tiden. Kontrollera inställningarna i hello mallen innan du använder den.
   > 
   > 
6. Kontrollera hello innehåll under **resurser** och notera hello följande:
   
   * **type**. Typ av resurs som skapas av hello mallen. I det här fallet **Microsoft.Network/virtualNetworks**, vilket motsvarar ett VNet.
   * **name**. Namnet för hello resurs. Meddelande hello användning av **[parameters('vnetName')]**, vilket innebär hello namn kommer att anges som indata av hello användare eller en parameterfil under distributionen.
   * **properties**. Lista över egenskaper för hello resurs. Denna mall används hello egenskaper för adressutrymmet och undernätsegenskaperna vid skapandet av ett VNet.
7. Gå tillbaka för[hello exempelmallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Klicka på **azuredeploy-parameters.json** och klicka sedan på **RAW**.
9. Spara hello filen tooa en lokal mapp på datorn.
10. Öppna hello-filen som du just har sparat och redigera hello värden för hello parametrar. Använd följande värden under toodeploy hello VNet som beskrivs i scenariot hello hello:

    ```json
        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
    ```

11. Spara hello-filen.


## <a name="deploy-hello-template-using-powershell"></a>Distribuera hello-mallen med hjälp av PowerShell

Slutför hello följande steg toodeploy hello mallen som du hämtade med hjälp av PowerShell:

1. Installera och konfigurera Azure PowerShell genom att slutföra hello stegen i hello [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.
2. Kör följande kommando toocreate hello en ny resursgrupp:

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    hello-kommando skapar en resursgrupp med namnet *TestRG* i hello *centrala USA* azure-region. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

    Förväntad utdata:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/[Id]/resourceGroups/TestRG

3. Kör följande kommando toodeploy hello nya VNet med hjälp av hello mallen och parametern filer du hämtade och ändrade ovan hello:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

    Förväntad utdata:
   
        DeploymentName    : TestVNetDeployment
        ResourceGroupName : TestRG
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            location         String                     Central US
                            vnetName         String                     TestVNet
                            addressPrefix    String                     192.168.0.0/16
                            subnet1Prefix    String                     192.168.1.0/24
                            subnet1Name      String                     FrontEnd
                            subnet2Prefix    String                     192.168.2.0/24
                            subnet2Name      String                     BackEnd
   
        Outputs           :
4. Hello kör följande kommando tooview hello egenskaper för hello nya VNet:

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

    Förväntad utdata:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="deploy-hello-template-using-click-to-deploy"></a>Distribuera hello mallen med Klicka för att distribuera

Du kan återanvända fördefinierade Azure Resource Manager mallar överförda tooa GitHub databas som hanteras av Microsoft och öppna toohello community. De här mallarna kan distribueras direkt från GitHub, eller hämtade och ändrade toofit dina behov. toodeploy en mall som skapar ett VNet med två undernät fullständig hello följande steg:

1. Från en webbläsare navigerar för[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Bläddra nedåt hello listan över mallar och klickar på **101-vnet-two-subnets**. Kontrollera hello **README.md** filen enligt nedan.

    ![README.md-filen i GitHub](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Klicka på **distribuera tooAzure**. Ange dina inloggningsuppgifter för Azure vid behov. 
4. I hello **parametrar** bladet ange hello värden du toouse toocreate ditt nya VNet, och klickar sedan på **OK**. hello visar följande bild hello värden för hello scenario:
   
    ![ARM-mallparametrar](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

5. Klicka på **resursgruppen** och välj en resurs grupp tooadd hello VNet till, eller klicka på **Skapa nytt** tooadd hello VNet tooa ny resursgrupp. hello följande bild visar hello resurs resursgruppsinställningarna för en ny resursgrupp som kallas **TestRG**:

    ![Resursgrupp](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

6. Om det behövs ändrar hello **prenumeration** och **plats** inställningar för din VNet.
7. Om du inte vill toosee hello VNet som en panel i hello **startsidan**, inaktivera **PIN-kod tooStartboard**.
8. Klicka på **juridiska villkor**, Läs hello villkoren och klicka på **köpa** tooagree. 
9. Klicka på **skapa** toocreate hello virtuella nätverk.
   
    ![Ikonen för Skicka in distribution i preview-portalen](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

10. När hello distributionen är klar i hello Azure-portalen klickar du på **fler tjänster**, typen *virtuella nätverk* hello filter i rutan som visas och klicka sedan på virtuella nätverk toosee hello virtuella nätverk bladet. Hello-bladet klickar du på *TestVNet*. I hello *TestVNet* bladet, klickar du på **undernät** toosee hello skapade undernät, som visas i följande bild hello:
    
     ![Skapa VNet i preview-portalen](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.png)

## <a name="next-steps"></a>Nästa steg

Lär dig hur tooconnect:

- En virtuell dator (VM) tooa virtuellt nätverk genom att läsa hello [skapa en virtuell Windows-dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md) eller [skapa ett Linux VM](../virtual-machines/linux/quick-create-portal.md) artiklar. I stället för att skapa en VNet och undernät i hello steg hello artiklar, kan du välja ett befintligt VNet och undernät tooconnect en virtuell dator till.
- Hej virtuellt nätverk tooother virtuella nätverk genom att läsa hello [ansluta Vnet](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) artikel.
- hello virtuellt nätverk tooan lokalt nätverk via ett plats-till-plats virtuellt privat nätverk (VPN) eller en ExpressRoute-kretsen. Lär dig hur genom att läsa hello [ansluta ett virtuellt nätverk tooan lokalt nätverk med hjälp av en plats-till-plats-VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) och [länka VNet-tooan ExpressRoute-krets](../expressroute/expressroute-howto-linkvnet-arm.md) artiklar.
