---
title: aaaCreate Programgateway en Azure - mallar | Microsoft Docs
description: "Den här sidan innehåller instruktioner toocreate en gateway för Azure-program med hello Azure Resource Manager-mall"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: fc18e553852551326d6a302abe2c7f8a08c2eb6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-resource-manager-template"></a>Skapa en Programgateway med hello Azure Resource Manager-mall

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-gateway-arm.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-create-gateway.md)
> * [Azure Resource Manager-mall](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway är en Layer 7-belastningsutjämnare. Det ger redundans och prestanda inkommande HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt. Application Gateway innehåller många ADC-funktioner (Application Delivery Controller), inklusive HTTP-belastningsutjämning, cookie-baserad sessionstilldelning, SSL-avlastning (Secure Sockets Layer), anpassade hälsoavsökningar, stöd för flera platser och mycket mer. toofind en fullständig lista över funktioner som stöds finns [Programgateway översikt](application-gateway-introduction.md)

Den här artikeln beskriver hur du hämtar och ändrar en befintlig Azure Resource Manager-mall från GitHub och distribuerar hello-mall från GitHub, PowerShell och hello Azure CLI.

Om du bara distribuerar hello Azure Resource Manager-mallen direkt från GitHub utan ändringar, kan du hoppa över toodeploy en mall från GitHub.

## <a name="scenario"></a>Scenario

I det här scenariot ska du:

* Skapa en Programgateway med Brandvägg för webbaserade program.
* Skapa ett virtuellt nätverk med namnet VirtualNetwork1 med det reserverade CIDR-blocket 10.0.0.0/16.
* Skapa undernätet Appgatewaysubnet som använder 10.0.0.0/28 som dess CIDR-block.
* Ställa in två tidigare konfigurerade backend-IP-adresser för hello webbservrar du tooload Utjämna hello trafiken. I det här exemplet mallen hello backend-IP-adresser är 10.0.1.10 och 10.0.1.11.

> [!NOTE]
> Dessa inställningar är hello parametrar för den här mallen. toocustomize hello mall som du kan ändra regler, hello lyssnare, SSL och andra alternativ i hello azuredeploy.json-filen.

![Scenario](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-hello-azure-resource-manager-template"></a>Hämta och förstå hello Azure Resource Manager-mall

Du kan ladda ned hello befintliga Azure Resource Manager-mall toocreate ett virtuellt nätverk och två undernät från GitHub, göra ändringar du vill och återanvända den. toodo så Använd hello följande steg:

1. Navigera för[skapa Programgateway med Brandvägg för webbaserade program aktiverat](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).
1. Klicka på **azuredeploy.json** och klicka sedan på **RAW**.
1. Spara hello tooa lokal mapp på datorn.
1. Om du är bekant med Azure Resource Manager-mallar kan du hoppa över toostep 7.
1. Öppna hello-fil som du har sparat och titta på hello innehållet under **parametrar** i rad
1. Azure Resource Manager-mallens parametrar fungerar som platshållare för värden som kan anges under distributionen.

  | Parameter | Beskrivning |
  | --- | --- |
  | **subnetPrefix** |CIDR-block för hello programmet gateway-undernätet. |
  | **applicationGatewaySize** | Storleken på hello Programgateway.  Brandvägg kan bara medelstora och stora. |
  | **backendIpaddress1** |IP-adress hello första webbserver. |
  | **backendIpaddress2** |IP-adress hello andra webbservern. |
  | **wafEnabled** | Ange toodetermine om Brandvägg är aktiverad.|
  | **wafMode** | Läget för hello Brandvägg för webbaserade program.  Alternativen är **förebyggande** eller **identifiering**.|
  | **wafRuleSetType** | RuleSet-typ för Brandvägg.  OWASP är för närvarande hello stöds endast alternativet. |
  | **wafRuleSetVersion** |RuleSet-version. OWASP CRS 2.2.9 och 3.0 finns för närvarande hello stöds alternativ. |

1. Kontrollera hello innehåll under **resurser** och meddelande hello följande egenskaper:

   * **type**. Typ av resurs som skapas av hello mallen. I det här fallet hello typen är `Microsoft.Network/applicationGateways`, som representerar en Programgateway.
   * **name**. Namnet för hello resurs. Meddelande hello användning av `[parameters('applicationGatewayName')]`, vilket innebär att hello-namn har angetts som indata av dig eller en parameterfil under distributionen.
   * **properties**. Lista över egenskaper för hello resurs. Den här mallen använder hello virtuella nätverk och offentlig IP-adress när programmet gateway skapas.

   > [!NOTE]
   > Mer information om mallar finns: [referens för Resource Manager-mallar](/templates/)

1. Gå tillbaka för[https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).
1. Klicka på **azuredeploy-parameters.json**, och klicka sedan på **RAW**.
1. Spara hello tooa lokal mapp på datorn.
1. Öppna hello-filen som du sparade och redigera hello värden för hello parametrar. Använd hello följande värden toodeploy hello Programgateway beskrivs i vårt scenario.

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "addressPrefix": {
            "value": "10.0.0.0/16"
            },
            "subnetPrefix": {
            "value": "10.0.0.0/28"
            },
            "applicationGatewaySize": {
            "value": "WAF_Medium"
            },
            "capacity": {
            "value": 2
            },
            "backendIpAddress1": {
            "value": "10.0.1.10"
            },
            "backendIpAddress2": {
            "value": "10.0.1.11"
            },
            "wafEnabled": {
            "value": true
            },
            "wafMode": {
            "value": "Detection"
            },
            "wafRuleSetType": {
            "value": "OWASP"
            },
            "wafRuleSetVersion": {
            "value": "3.0"
            }
        }
    }
    ```

1. Spara hello-filen. Du kan testa hello JSON-mall och parametern-mallen med hjälp av online-verifiering JSON-verktyg som [JSlint.com](http://www.jslint.com/).

## <a name="deploy-hello-azure-resource-manager-template-by-using-powershell"></a>Distribuera hello Azure Resource Manager-mallen med hjälp av PowerShell

Om du aldrig har använt Azure PowerShell gå: [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) och följ hello instruktioner toosign till Azure och välja din prenumeration.

1. Inloggningen tooPowerShell

    ```powershell
    Login-AzureRmAccount
    ```

1. Kontrollera hello prenumerationer för hello-kontot.

    ```powershell
    Get-AzureRmSubscription
    ```

    Du kan ange tooauthenticate med dina autentiseringsuppgifter.

1. Välj vilka av dina Azure-prenumerationer toouse.

    ```powershell
    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
    ```

1. Om det behövs kan du skapa en resursgrupp med hjälp av hello **New-AzureResourceGroup** cmdlet. I följande exempel hello, skapar du en resursgrupp med namnet AppgatewayRG i östra USA plats.

    ```powershell
    New-AzureRmResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. Kör hello **ny AzureRmResourceGroupDeployment** cmdlet toodeploy hello nytt virtuellt nätverk med hjälp av hello föregående mall och parametern filer som du hämtade och ändrade.
    
    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-hello-azure-cli"></a>Distribuera hello Azure Resource Manager-mall med hjälp av hello Azure CLI

toodeploy hello Azure Resource Manager-mall som du hämtade med Azure CLI, så hello följande steg:

1. Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](/cli/azure/install-azure-cli) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.

1. Om nödvändigt kör hello `az group create` kommandot toocreate en resursgrupp, enligt följande kodavsnitt hello. Observera hello hello kommandots utdata. hello-listan som visas efter hello utdata förklarar hello parametrar som används. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    **-n (eller --name)**. Namn för hello ny resursgrupp. I vårt scenario är det *appgatewayRG*.
    
    **-l (eller --location)**. Azure-region där hello ny resursgrupp skapas. I vårt scenario, den har *westus*.

1. Kör hello `az group deployment create` cmdlet toodeploy hello nytt virtuellt nätverk med hjälp av hello mallen och parametern filer du hämtade och ändrade i hello föregående steg. hello-listan som visas efter hello utdata förklarar hello parametrar som används.

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-click-to-deploy"></a>Distribuera hello Azure Resource Manager-mallen med Klicka för att distribuera

Klicka för att distribuera är ett annat sätt toouse Azure Resource Manager-mallar. Det är ett enkelt sätt toouse mallar med hello Azure-portalen.

1. Gå för[skapa en Programgateway med Brandvägg för webbaserade program](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).

1. Klicka på **distribuera tooAzure**.

    ![Distribuera tooAzure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. Fyll i hello parametrar för hello Distributionsmall på hello-portalen och klicka på **OK**.

    ![Parametrar](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. Välj **acceptera toohello villkoren ovan** och på **inköp**.

1. På hello anpassad distribution bladet, klickar du på **skapa**.

## <a name="providing-certificate-data-tooresource-manager-templates"></a>Att tillhandahålla certifikat data tooResource Manager-mallar

När du använder SSL med en mall måste hello certifikat toobe som anges i en base64-sträng i stället för som överförs. tooconvert en PFX- eller .cer tooa base64-sträng med någon av följande kommandon hello. hello konvertera följande kommandon hello certifikat tooa base64-sträng, vilket kan anges toohello mall. hello förväntade utdata är en sträng som kan lagras i en variabel och klistras in i hello mallen.

### <a name="macos"></a>macOS
```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a>Windows
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a>Ta bort alla resurser

toodelete alla resurser skapas i den här artikeln, någon av följande hello:

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzureRmResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a>Nästa steg

Om du vill tooconfigure SSL-avlastning, besök: [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl.md).

Om du vill tooconfigure ett program gateway toouse med en intern belastningsutjämnare: [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).

Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

