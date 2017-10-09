---
title: "Ansluta klassiska virtuella nätverk tooAzure Resource Manager VNets: Portal | Microsoft Docs"
description: "Lär dig hur toocreate en VPN-anslutning mellan klassiska Vnet och Resource Manager VNets med hjälp av VPN-Gateway och hello-portalen"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 5a90498c-4520-4bd3-a833-ad85924ecaf9
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
ms.openlocfilehash: bef63b4e829335b2e1a9434a35ebfe33b4fd7373
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-hello-portal"></a>Ansluta virtuella nätverk från olika distributionsmodeller med hello-portalen

Den här artikeln visar hur tooconnect klassiska Vnet tooResource Manager VNets tooallow hello resurser i hello separat distribution modeller toocommunicate med varandra. hello stegen i den här artikeln används främst hello Azure-portalen, men du kan också skapa den här konfigurationen med hjälp av hello PowerShell genom att välja hello artikel från den här listan.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Ansluta ett klassiskt virtuellt nätverk tooa Resource Manager VNet är liknande tooconnecting ett VNet tooan lokal plats. Båda typerna av anslutningen använder en VPN-gateway tooprovide en säker tunnel med IPsec/IKE. Du kan skapa en anslutning mellan Vnet som finns i olika prenumerationer och i olika regioner. Du kan också ansluta Vnet som redan har anslutningar tooon lokala nätverk, så länge hello-gateway som de har konfigurerats med är dynamisk eller ruttbaserad. Mer information om VNet-till-VNet-anslutningar finns hello [VNet-till-VNet vanliga frågor och svar](#faq) hello slutet av den här artikeln. 

Om ditt Vnet i hello samma region som du kanske vill tooinstead bör överväga att ansluta dem med hjälp av VNet-Peering. Ingen VPN-gateway används för VNet-peering. Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md). 

### <a name="prerequisites"></a>Krav

* Dessa instruktioner förutsätter att båda Vnet har redan skapats. Om du använder den här artikeln som en övning och inte har Vnet finns länkar i hello steg toohelp skapar du dem.
* Kontrollera att hello-adressintervall för hello Vnet inte överlappar varandra eller överlappar någon hello-intervall för andra anslutningar som hello gateways kan anslutas till.
* Installera hello senaste PowerShell-cmdlets för både Resource Manager och Service Management (klassisk). I den här artikeln använder vi både hello Azure portal och PowerShell. PowerShell är obligatoriska toocreate hello anslutning från hello klassiska VNet toohello Resource Manager VNet. Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). 

### <a name="values"></a>Exempelinställningar

Du kan använda dessa värden toocreate en testmiljö eller referera toothem toobetter förstå hello exemplen i den här artikeln.

**Klassiska VNet**

VNet-name = ClassicVNet <br>
Adressutrymmet = 10.0.0.0/24 <br>
Undernät-1 = 10.0.0.0/27 <br>
Resursgruppens namn = ClassicRG <br>
Plats = västra USA <br>
GatewaySubnet = 10.0.0.32/28 <br>
Lokal plats = RMVNetLocal <br>

**Hanteraren för virtuella nätverk**

VNet-name = RMVNet <br>
Adressutrymmet = 192.168.0.0/16 <br>
Undernät-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Resursgruppens namn = RG1 <br>
Plats = östra USA <br>
Gateway för virtuella nätverksnamnet = RMGateway <br>
Gateway-typ = VPN <br>
VPN-typ = ruttbaserad <br>
Gatewaynamnet offentlig IP-adress = rmgwpip <br>
Lokal nätverksgateway = ClassicVNetLocal <br>
Anslutningens namn = RMtoClassic

### <a name="connection-overview"></a>Översikt över anslutning

För den här konfigurationen måste skapa du en VPN-anslutning för gateway via en IPsec/IKE VPN-tunnel mellan hello virtuella nätverk. Kontrollera att ingen av VNet-intervall överlappar varandra eller med någon av hello lokala nätverk som de ansluter till.

hello visar följande tabell ett exempel på hur hello exempel Vnet och lokala platser definieras:

| Virtual Network | Adressutrymmet | Region | Ansluter toolocal nätverksplats |
|:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |Västra USA | RMVNetLocal (192.168.0.0/16) |
| RMVNet | (192.168.0.0/16) |Östra USA |ClassicVNetLocal (10.0.0.0/24) |

## <a name="classicvnet"></a>1. Konfigurera hello klassiska VNet-inställningarna

I det här avsnittet skapar du hello lokala nätverk (lokal plats) och hello virtuell nätverksgateway för din klassiska VNet. Om du inte har ett klassiskt virtuellt nätverk och kör de här stegen som Övning, kan du skapa ett VNet med hjälp av [i den här artikeln](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) och hello [exempel](#values) värden från ovan.

När du använder hello portal toocreate ett klassiskt virtuellt nätverk, ska du gå toohello virtuellt nätverk-bladet med hjälp av hello följande steg, annars visas inte hello alternativet toocreate ett klassiskt virtuellt nätverk:

1. Klicka på hello ”+” tooopen hello ”New-bladet.
2. Skriv ”virtuella nätverk” i hello 'Sökningen hello marketplace'. Om du i stället väljer nätverk -> virtuella nätverk, får du inte hello alternativet toocreate ett klassiskt virtuellt nätverk.
3. Hitta ”virtuella nätverk, från hello returnerade listan och klicka på den tooopen hello virtuellt nätverk-bladet. 
4. Välj 'Klassiskt' toocreate klassiska VNet på hello virtuellt nätverk-bladet. 

Om du redan har ett VNet med en VPN-gateway måste du kontrollera att hello-gateway är dynamiska. Om den är statisk, måste du först ta bort hello VPN-gateway sedan fortsätta.

Skärmbilderna anges som exempel. Vara säker på att tooreplace hello värdena med dina egna, eller använda hello [exempel](#values) värden.

### <a name="part-1---configure-hello-local-site"></a>Del 1 – konfigurera hello lokala platsen

Öppna hello [Azure-portalen](https://ms.portal.azure.com) och logga in med ditt Azure-konto.

1. Navigera för**alla resurser** och leta upp hello **ClassicVNet** i hello-listan.
2. På hello **översikt** bladet i hello **VPN-anslutningar** klickar du på hello **Gateway** grafisk toocreate en gateway.

    ![Konfigurera en VPN-gateway](./media/vpn-gateway-connect-different-deployment-models-portal/gatewaygraphic.png "konfigurera en VPN-gateway")
3. På hello **ny VPN-anslutning** bladet för **anslutningstypen**väljer **plats-till-plats**.
4. För **lokal plats**, klickar du på **konfigurera nödvändiga inställningar**. Då öppnas hello **lokal plats** bladet.
5. På hello **lokal plats** bladet skapar en Resource Manager VNet med namnet toorefer toohello. Till exempel 'RMVNetLocal'.
6. Om hello VPN-gateway för hello Resource Manager VNet redan har en offentlig IP-adress, använder du hello värdet för hello **IP-adressen för VPN-gateway** fältet. Om du utföra följande steg som Övning, eller ännu inte har en virtuell nätverksgateway för Resource Manager-VNet, kan du konfigurera en platshållare för IP-adress. Se till att hello platshållare IP-adress använder ett ogiltigt format. Senare kan ersätta du hello platshållare IP-adress med hello offentliga IP-adressen för hello Resource Manager virtuell nätverksgateway.
7. För **klientens adressutrymme**, använda hello värden för hello virtuellt IP-adressutrymmen för hello Resource Manager VNet. Den här inställningen är används toospecify hello adress blanksteg tooroute toohello Resource Manager virtuella nätverk.
8. Klicka på **OK** toosave hello värden och returnera toohello **ny VPN-anslutning** bladet.

### <a name="part-2---create-hello-virtual-network-gateway"></a>Del 2 – Skapa hello virtuell nätverksgateway

1. På hello **ny VPN-anslutning** bladet, Välj hello **skapa gateway omedelbart** kryssrutan och klicka på **valfria gatewaykonfigurationen** tooopen hello  **Gateway-konfigurationen** bladet. 

    ![Öppna gateway konfiguration bladet](./media/vpn-gateway-connect-different-deployment-models-portal/optionalgatewayconfiguration.png "öppna nätverksgateway configuration-bladet")
2. Klicka på **undernät - konfigurera nödvändiga inställningar** tooopen hello **Lägg till undernät** bladet. Hej **namn** har redan konfigurerats med hello krävs värdet **GatewaySubnet**.
3. Hej **adressintervall** refererar toohello intervallet för hello gateway-undernätet. Även om du kan skapa en gateway-undernät med en /29 adressintervall (3 adresser), rekommenderar vi att skapa en gateway-undernät som innehåller flera IP-adresser. Detta kommer att underlätta framtida konfigurationer som kan kräva mer tillgängliga IP-adresser. Använd om möjligt minst/27 eller /28. Om du använder de här stegen som Övning, kan du läsa toohello [exempel](#values) värden. Klicka på **OK** toocreate hello gateway-undernätet.
4. På hello **gatewaykonfigurationen** bladet **storlek** refererar toohello gateway SKU. Välj hello gateway SKU för VPN-gateway.
5. Kontrollera hello **routning typen** är **dynamiska**, klicka på **OK** tooreturn toohello **ny VPN-anslutning** bladet.
6. På hello **ny VPN-anslutning** bladet, klickar du på **OK** toobegin skapar din VPN-gateway. Skapa en VPN-gateway kan ta upp too45 minuter toocomplete.

### <a name="ip"></a>Del 3 - kopia hello virtuell nätverksgateway offentlig IP-adress

När hello virtuella nätverkets gateway har skapats kan visa du hello gateway IP-adress. 

1. Navigera tooyour klassiska virtuella nätverk och klickar på **översikt**.
2. Klicka på **VPN-anslutningar** tooopen hello VPN-anslutningar bladet. Du kan visa hello offentliga IP-adressen på hello VPN-anslutningar bladet. Detta är hello offentlig IP-adress som tilldelats tooyour virtuell nätverksgateway. 
3. Skriv ned eller kopiera hello IP-adress. Du använder den i senare steg när du arbetar med Resource Manager lokala nätverket gateway konfigurationsinställningar. Du kan också visa hello statusen för gateway-anslutningar. Meddelande hello lokal nätverksplats du skapade anges som 'Ansluta'. hello status ändras när du har skapat dina anslutningar.
4. Stäng hello bladet när du har kopierat hello gateway IP-adress.

## <a name="rmvnet"></a>2. Konfigurera hello Resource Manager VNet-inställningarna

I det här avsnittet skapar du hello virtuell nätverksgateway och hello lokal nätverksgateway för Resource Manager-VNet. Om du inte har ett VNet Resource Manager och kör de här stegen som Övning, kan du skapa ett VNet med hjälp av [i den här artikeln](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) och hello [exempel](#values) värden från ovan.

Skärmbilderna anges som exempel. Vara säker på att tooreplace hello värdena med dina egna, eller använda hello [exempel](#values) värden.

### <a name="part-1---create-a-gateway-subnet"></a>Del 1 – skapa ett gateway-undernät

Innan du skapar en virtuell nätverksgateway, måste du först toocreate hello gateway-undernätet. Skapa en gateway-undernät med CIDR-antal för /28 eller större. (/ 27/26, etc.)

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Del 2 – Skapa en virtuell nätverksgateway

[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

### <a name="createlng"></a>Del 3 – skapa en lokal nätverksgateway

hello lokal nätverksgateway anger hello-adressintervall och hello offentlig IP-adress som är kopplad till ditt klassiska VNet och dess virtuella nätverks-gatewayen.

Om du gör dessa steg som Övning Se toothese inställningar:

| Virtual Network | Adressutrymmet | Region | Ansluter toolocal nätverksplats |Offentliga IP-adressen för gateway|
|:--- |:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |Västra USA | RMVNetLocal (192.168.0.0/16) |hello offentlig IP-adress som är tilldelad toohello ClassicVNet gateway|
| RMVNet | (192.168.0.0/16) |Östra USA |ClassicVNetLocal (10.0.0.0/24) |hello offentlig IP-adress som är tilldelad toohello RMVNet gateway.|

[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="modifylng"></a>3. Ändra inställningar för hello klassiska VNet lokala platsen

I det här avsnittet kan du ersätta hello platshållare IP-adress som du använde när du anger hello lokala platsinställningar med hello Resource Manager VPN gateway IP-adress. Det här avsnittet använder hello klassiska (SM) PowerShell-cmdlets.

1. Navigera toohello klassiskt virtuellt nätverk i hello Azure-portalen.
2. Klicka på hello bladet för det virtuella nätverket **översikt**.
3. I hello **VPN-anslutningar** klickar du på hello namnet på den lokala platsen i hello bild.

    ![VPN-anslutningar](./media/vpn-gateway-connect-different-deployment-models-portal/vpnconnections.png "VPN-anslutningar")
4. På hello **plats-till-plats VPN-anslutningar** bladet Klicka hello namnet för hello plats.

    ![Site-name](./media/vpn-gateway-connect-different-deployment-models-portal/sitetosite3.png "lokala platsnamn")
5. Hello anslutning bladet för den lokala platsen Klicka hello namnet på hello lokal plats tooopen hello **lokal plats** bladet.

    ![Öppna lokal plats](./media/vpn-gateway-connect-different-deployment-models-portal/openlocal.png "öppna lokala platsen")
6. På hello **lokal plats** bladet, Ersätt hello **IP-adressen för VPN-gateway** med hello IP-adressen för hello Resource Manager-gateway.

    ![Gateway-ip-adress](./media/vpn-gateway-connect-different-deployment-models-portal/gwipaddress.png "Gateway IP-adress")
7. Klicka på **OK** tooupdate hello IP-adress.

## <a name="RMtoclassic"></a>4. Skapa Resource Manager tooclassic anslutning

I följande steg ska du konfigurera hello anslutning från hello Resource Manager VNet toohello klassiska virtuella nätverk med hjälp av hello Azure-portalen.

1. I **alla resurser**, leta upp hello lokal nätverksgateway. I vårt exempel hello lokal nätverksgateway är **ClassicVNetLocal**.
2. Klicka på **Configuration** och kontrollera att värdet för hello IP-adress är hello VPN-gateway för hello klassiska virtuella nätverk. Uppdatera vid behov och klicka sedan på **spara**. Stäng hello-bladet.
3. I **alla resurser**, klicka på hello lokal nätverksgateway.
4. Klicka på **anslutningar** tooopen hello anslutningar bladet.
5. På hello **anslutningar** bladet, klickar du på  **+**  tooadd en anslutning.
6. På hello **Lägg till anslutning** bladet, namnet hello anslutning. Till exempel 'RMtoClassic'.
7. **Plats-till-plats** har redan valts på det här bladet.
8. Välj hello virtuell nätverksgateway som du vill tooassociate med den här platsen.
9. Skapa en **delad nyckel**. Den här nyckeln används också i hello-anslutningen som du skapar från hello klassiska VNet toohello Resource Manager VNet. Du kan generera hello nyckel eller utgör en. Vi använder 'abc123' i vårt exempel, men du kan (och bör) använder något mer komplicerad.
10. Klicka på **OK** toocreate hello anslutning.

##<a name="classictoRM"></a>5. Skapa klassiska tooResource för Anslutningshanteraren

I följande steg ska konfigurera du hello anslutning från hello klassiska VNet toohello Resource Manager VNet. Dessa steg kräver PowerShell. Du kan inte skapa den här anslutningen i hello-portalen. Kontrollera att du har hämtat och installerat både hello klassiska (SM) och Resource Manager (RM) PowerShell-cmdlets.

### <a name="1-connect-tooyour-azure-account"></a>1. Ansluta tooyour Azure-konto

Öppna hello PowerShell-konsol med utökade behörigheter och logga in tooyour Azure-konto. hello efterfrågar följande cmdlet hello inloggningsuppgifterna för Azure-konto. När du loggar in laddas inställningarna för ditt konto så att de är tillgängliga tooAzure PowerShell.

```powershell
Login-AzureRmAccount
```
   
Hämta en lista över dina Azure-prenumerationer om du har mer än en prenumeration.

```powershell
Get-AzureRmSubscription
```

Ange hello prenumeration som du vill toouse. 

```powershell
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```

Lägg till din Azure-konto toouse hello klassiska PowerShell-cmdlets (SM). toodo så du kan använda hello följande kommando:

```powershell
Add-AzureAccount
```

### <a name="2-view-hello-network-configuration-file-values"></a>2. Visa hello filen konfigurationsvärden för nätverk

När du skapar ett VNet i hello Azure-portalen, visas inte hello fullständiga namn som använder Azure hello Azure-portalen. Ett virtuellt nätverk som visas toobe med namnet 'ClassicVNet' i hello Azure-portalen kan ha en mycket längre namn i konfigurationsfilen för hello nätverk. hello namn kan se ut ungefär så: 'Grupp ClassicRG ClassicVNet'. I följande steg ska hämta du hello fil- och visa hello konfigurationsvärden nätverk.

Skapa en katalog på datorn och sedan exportera hello configuration file toohello nätverksresursen. I det här exemplet är hello nätverket konfigurationsfilen exporterade tooC:\AzureNet.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Öppna hello-filen med ett text-redigeraren och visa hello namn för din klassiska VNet. Använda hello namn i konfigurationsfilen för hello nätverk när du kör PowerShell-cmdlets.

- VNet namn visas som **VirtualNetworkSite namn =**
- Platsnamn listas som **LocalNetworkSite namn =**

### <a name="3-create-hello-connection"></a>3. Skapa hello-anslutning

Ange hello delad nyckel och skapa hello anslutning från hello klassiska VNet toohello Resource Manager VNet. Du kan inte ange hello delad nyckel med hjälp av hello portal. Kontrollera att du kör dessa steg när du är inloggad med hello klassiska versionen av hello PowerShell-cmdlets. så, Använd toodo **Add-AzureAccount**. Annars kommer du inte att kunna tooset hello '-AzureVNetGatewayKey'.

- I det här exemplet **- VNetName** är hello namn av hello klassiska virtuella nätverk som finns i konfigurationsfilen nätverk. 
- Hej **- LocalNetworkSiteName** hello namn du angav för hello lokal plats, som finns i konfigurationsfilen nätverk.
- Hej **- SharedKey** är ett värde som du skapar och ange. I det här exemplet används vi *abc123*, men du kan skapa något mer komplicerad. hello viktig sak är att hello-värde som du anger här måste vara hello samma värde som du angav när du skapar Resource Manager tooclassic-anslutning.

```powershell
Set-AzureVNetGatewayKey -VNetName "Group ClassicRG ClassicVNet" `
-LocalNetworkSiteName "172B9E16_RMVNetLocal" -SharedKey abc123
```

##<a name="verify"></a>6. Verifiera dina anslutningar

Du kan verifiera dina anslutningar med hjälp av hello Azure-portalen eller PowerShell. När du verifierar, kanske du måste toowait en minut eller två när hello anslutningen skapas. När en anslutning lyckas hello anslutningen tillstånd ändras från 'Ansluta' too'Connected'.

### <a name="tooverify-hello-connection-from-your-classic-vnet-tooyour-resource-manager-vnet"></a>tooverify hello anslutning från din klassiska VNet tooyour Resource Manager VNet

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

###<a name="tooverify-hello-connection-from-your-resource-manager-vnet-tooyour-classic-vnet"></a>tooverify hello anslutning från hanteraren för filserverresurser VNet-tooyour klassiska virtuella nätverk

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]
