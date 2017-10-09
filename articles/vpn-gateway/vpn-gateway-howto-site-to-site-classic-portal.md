---
title: "Ansluta din lokala nätverk tooan virtuella Azure-nätverket: plats-till-plats-VPN (klassisk): Portal | Microsoft Docs"
description: "Steg toocreate en IPSec-anslutning från ditt lokala nätverk tooan virtuella Azure-nätverket via hello offentliga Internet. Dessa steg hjälper dig att skapa en anslutning för plats-till-plats VPN-Gateway av mellan platser med hjälp av hello portal."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/010/2017
ms.author: cherylmc
ms.openlocfilehash: b260bdf610b264458660b278bd32bf0fc5b519ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-site-to-site-connection-using-hello-azure-portal-classic"></a>Skapa en plats-till-plats-anslutning med hello Azure-portalen (klassisk)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Den här artikeln visar hur hello toouse Azure portal toocreate plats-till-plats VPN-gateway-anslutningen från ditt lokala nätverk toohello VNet. hello stegen i den här artikeln gäller toohello klassiska distributionsmodellen. Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (klassisk)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

En plats-till-plats VPN-gateway-anslutningen har använt tooconnect ditt lokala nätverk tooan virtuella Azure-nätverket via en IPsec/IKE (IKEv1 eller IKEv2) VPN-tunnel. Den här typen av anslutning kräver en VPN-enhet som finns lokalt som har ett externt Internetriktade offentliga IP-adress som tilldelats tooit. Mer information om VPN-gatewayer finns i [Om VPN-gateway](vpn-gateway-about-vpngateways.md).

![Diagram över plats-till-plats-anslutning med VPN-gateway](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a>Innan du börjar

Kontrollera att du har uppfyllt hello följande villkor innan du påbörjar konfiguration:

* Kontrollera att du vill att toowork i hello klassiska distributionsmodellen. Om du vill toowork i hello Resource Manager-distributionsmodellen, se [skapa en plats-till-plats-anslutning (Resource Manager)](vpn-gateway-howto-site-to-site-resource-manager-portal.md). När det är möjligt rekommenderar vi att du använder hello Resource Manager-modellen.
* Kontrollera att du har en kompatibel VPN-enhet och någon som kan tooconfigure den. Se [Om VPN-enheter](vpn-gateway-about-vpn-devices.md) för mer information om kompatibla VPN-enheter och enhetskonfiguration.
* Kontrollera att du har en extern offentlig IPv4-adress för VPN-enheten. Den här IP-adressen får inte finnas bakom en NAT.
* Om du är bekant med hello IP-adressintervall som finns i ditt lokala nätverk konfiguration måste du toocoordinate med någon som kan ge de detaljer du. När du skapar den här konfigurationen måste du ange hello IP-adressintervall adressprefix Azure dirigerar tooyour lokal plats. Ingen av hello undernät i ditt lokala nätverk kan över lap med hello virtuella undernät som du vill tooconnect till.
* För närvarande är nödvändiga toospecify hello delad nyckel PowerShell och skapa hello VPN gateway-anslutningen. Installera hello senaste versionen av hello Azure Service Management (SM) PowerShell-cmdlets. Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). När du arbetar med PowerShell i den här konfigurationen ska du kontrollera att du kör som administratör. 

### <a name="values"></a>Exempel på konfigurationsvärden för övningen

hello exemplen i den här artikeln används hello följande värden. Du kan använda dessa värden toocreate en testmiljö eller referera toothem toobetter förstå hello exemplen i den här artikeln.

* **VNet-namn:** TestVNet1
* **Adressutrymme:** 
  * 10.11.0.0/16
  * 10.12.0.0/16 (valfritt för den här övningen)
* **Undernät:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (valfritt för den här övningen)
* **GatewaySubnet:** 10.11.255.0/27
* **Resursgrupp:** TestRG1
* **Plats:** Östra USA
* **DNS-server:** 10.11.0.3 (valfritt för den här övningen)
* **Namn på lokal plats:** Site2
* **Klientens adressutrymme:** hello adressutrymme som finns på den lokala platsen.

## <a name="CreatVNet"></a>1. Skapa ett virtuellt nätverk

När du skapar ett virtuellt nätverk toouse för en S2S-anslutning måste toomake att hello-adressutrymmen som du anger inte överlappar med någon av hello klienten adressutrymmen för hello lokala platser som du vill tooconnect till. Om du har överlappande undernät fungerar inte anslutningen ordentligt.

* Om du redan har ett virtuellt nätverk kontrollerar du att hello inställningarna är kompatibel med din design av VPN-gateway. Särskilt noga tooany undernät som kan överlappa andra nätverk. 

* Om du inte redan har ett virtuellt nätverk, skapa ett. Skärmbilderna anges som exempel. Vara säker på att tooreplace hello värden med dina egna.

### <a name="toocreate-a-virtual-network"></a>toocreate ett virtuellt nätverk

1. Från en webbläsare, navigerar du toohello [Azure-portalen](http://portal.azure.com) och vid behov, logga in med ditt Azure-konto.
2. Klicka på **+**. I hello **Sök hello marketplace** skriver du ”virtuella nätverk”. Leta upp **virtuellt nätverk** från hello returnerade listan och klicka på tooopen hello **virtuellt nätverk** sidan.

  ![Sidan Sök efter virtuellt nätverk](./media/vpn-gateway-howto-site-to-site-classic-portal/newvnetportal700.png)
3. Hello nedre delen av hello virtuellt nätverk sida från hello **Välj en distributionsmodell** listrutan, Välj **klassiska**, och klicka sedan på **skapa**.

  ![Välj distributionsmodell](./media/vpn-gateway-howto-site-to-site-classic-portal/selectmodel.png)
4. På hello **skapa virtuella network(classic)** konfigurerar hello VNet-inställningarna. På den här sidan lägger du till ditt första adressutrymme och ett enda adressintervall för ett undernät. När du har skapat hello VNet, kan du gå tillbaka och lägga till ytterligare undernät och adressutrymmen.

  ![Sidan Skapa virtuellt nätverk](./media/vpn-gateway-howto-site-to-site-classic-portal/createvnet.png "Sidan Skapa virtuellt nätverk")
5. Kontrollera att hello **prenumeration** är hello korrekt. Du kan ändra prenumerationer med hjälp av hello i listrutan.
6. Klicka på **Resursgrupp** och välj en befintlig resursgrupp eller skapa en ny genom att ange ett namn. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Välj därefter hello **plats** inställningar för din VNet. hello platsen avgör var hello resurser som du distribuerar toothis VNet kommer att finnas.
8. Om du vill toobe kan toofind ditt VNet enkelt på hello-instrumentpanelen väljer **PIN-kod toodashboard**. Klicka på **skapa** toocreate ditt VNet.

  ![PIN-kod toodashboard](./media/vpn-gateway-howto-site-to-site-classic-portal/pintodashboard150.png "toodashboard PIN-kod")
9. När du klickar på ”Skapa” visas en sida vid sida på hello instrumentpanel som visar hello förloppet för ditt VNet. hello panelen ändringar som hello VNet skapas.

  ![Ikonen Skapa ett virtuell nätverk](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Skapar det virtuella nätverket")

När det virtuella nätverket har skapats kan du se **Skapad** visas under **Status** på hello nätverk sida i hello klassiska Azure-portalen.

## <a name="additionaladdress"></a>2. Lägg till ytterligare adressutrymme

När du har skapat ditt virtuella nätverk kan du lägga till ytterligare adressutrymme. Lägga till ytterligare adressutrymme är inte en obligatorisk del av en S2S-konfiguration, men om du behöver flera adressutrymmen använder hello följande steg:

1. Leta upp hello virtuella nätverk i hello-portalen.
2. På sidan hello för det virtuella nätverket under hello **inställningar** klickar du på **adressutrymmet**.
3. På sidan för hello adressutrymme, klickar du på **+ Lägg till** och ange ytterligare adressutrymme.

## <a name="dns"></a>3. Ange en DNS-server

Det är inte obligatoriskt med DNS-inställningar för en S2S-konfiguration, men DNS krävs om du vill använda namnmatchning. Ingen ny DNS-server skapas när du anger ett värde. hello ska DNS-serverns IP-adress som du anger vara en DNS-server som kan lösa hello namn för hello-resurser som du ansluter till. Vi använde en privat IP-adress för hello exempelinställningar. hello IP-adress använder vi är förmodligen inte hello IP-adressen för DNS-servern. Vara säker på att toouse egna värden.

När du har skapat ditt virtuella nätverk kan du lägga till hello IP-adressen för en DNS-server toohandle namnmatchning. Öppna hello inställningarna för det virtuella nätverket, klicka på DNS-servrar och Lägg till hello IP-adressen för hello DNS-server som du vill toouse för namnmatchning.

1. Leta upp hello virtuella nätverk i hello-portalen.
2. På sidan hello för det virtuella nätverket under hello **inställningar** klickar du på **DNS-servrar**.
3. Lägg till en DNS-server.
4. toosave dina inställningar klickar du på **spara** hello överst på hello sidan.

## <a name="localsite"></a>4. Konfigurera hello lokala platsen

hello lokala plats hänvisar vanligtvis tooyour lokal plats. Den innehåller hello IP-adressen för hello VPN-enhet toowhich skapar du en anslutning och hello IP-adressintervall som vidarebefordras via hello VPN-gateway toohello VPN-enhet.

1. Navigera toohello virtuella nätverk som du vill toocreate en gateway i hello-portalen.
2. På sidan hello för det virtuella nätverket på hello **översikt** under hello VPN-anslutningar, klickar du på **Gateway** tooopen hello **ny VPN-anslutning** sidan.

  ![Klicka på tooconfigure gatewayinställningarna](./media/vpn-gateway-howto-site-to-site-classic-portal/beforegw125.png "tooconfigure gateway inställningar klickar du på")
3. På hello **ny VPN-anslutning** väljer **plats-till-plats**.
4. Klicka på **lokala plats - konfigurera nödvändiga inställningar** tooopen hello **lokal plats** sidan. Konfigurera inställningar för hello och klicka sedan på **OK** toosave hello inställningar.
  - **Namn:** skapar du ett namn för den lokala platsen toomake det enkelt för du tooidentify.
  - **IP-adressen för VPN-gateway:** hello offentliga IP-adress för hello VPN-enhet för ditt lokala nätverk. hello VPN-enheten kräver en offentlig IP-adress för IPv4. Ange en giltig offentlig IP-adress för hello VPN-enhet toowhich som du vill tooconnect. Det får inte vara bakom NAT och har toobe som kan nås av Azure. Om du inte vet hello IP-adress för VPN-enhet som du kan placera alltid ett värde för platshållaren (så länge det är i hello-format för en giltig offentlig IP-adress) och ändra dem senare.
  - **Klientens adressutrymme:** lista hello IP-adressintervall som ska dirigeras toohello lokala lokalt nätverk via den här gatewayen. Du kan lägga till flera adressintervall. Kontrollera att hello du anger här inte överlappar intervallen för andra virtuella nätverket ansluter till nätverk eller med hello adressintervall hello virtuella själva nätverket.

  ![Lokal plats](./media/vpn-gateway-howto-site-to-site-classic-portal/localnetworksite.png "Konfigurera lokal plats")

## <a name="gatewaysubnet"></a>5. Konfigurera hello gateway-undernät

Du måste skapa ett gatewayundernät för din VPN-gateway. hello gateway-undernätet innehåller hello IP-adresser som hello VPN gateway-tjänster.

1. På hello **ny VPN-anslutning** sidan, Välj hello kryssrutan **skapa gateway omedelbart**. Hej ”valfria gateway configuration” visas. Om du inte markerar kryssrutan hello visas inte hello sidan tooconfigure hello gateway-undernätet.

  ![Gateway-konfiguration - Undernät, storlek, routningstyp](./media/vpn-gateway-howto-site-to-site-classic-portal/optional.png "Gateway-konfiguration - Undernät, storlek, routningstyp")
2. tooopen hello **gatewaykonfigurationen** klickar du på **valfria gateway-konfiguration – undernät, storlek och routning**.
3. På hello **Gatewaykonfigurationen** klickar du på **undernät - konfigurera nödvändiga inställningar** tooopen hello **Lägg till undernät** sidan.

  ![Gateway-konfiguration - Gatewayundernät](./media/vpn-gateway-howto-site-to-site-classic-portal/subnetrequired.png "Gateway-konfiguration - Gatewayundernät")
4. På hello **Lägg till undernät** lägger du till hello gateway-undernätet. hello storleken på hello gateway-undernätet som du anger beror på hello VPN gateway-konfigurationen som du vill toocreate. Det är möjligt toocreate ett gatewayundernät så liten som /29, rekommenderar vi att du använder minst/27 eller /28. Då skapas ett större undernät som innehåller fler adresser. Med ett större gateway-undernät tillåter tillräckligt med IP-adresser tooaccommodate framtida konfigurationer.

  ![Lägg till gatewayundernät](./media/vpn-gateway-howto-site-to-site-classic-portal/addgwsubnet.png "Lägg till gatewayundernät")

## <a name="sku"></a>6. Ange hello SKU- och VPN-typ

1. Välj hello gateway **storlek**. Detta är hello gateway SKU att du använder toocreate din virtuella nätverksgateway. I hello portal hello standard SKU = **grundläggande**. Klassiska VPN-gatewayer använda hello gamla (äldre) gateway SKU: er. Mer information om hello äldre gateway-SKU: er finns [arbeta med virtuell nätverksgateway SKU: er (gamla SKU: er)](vpn-gateway-about-skus-legacy.md).

  ![Välj SKU- och VPN-typ](./media/vpn-gateway-howto-site-to-site-classic-portal/sku.png "Välj SKU- och VPN-typ")
2. Välj hello **routning typen** för din gateway. Detta kallas också hello VPN. Det är viktigt tooselect hello rätt gateway-typ eftersom du inte kan konvertera hello gateway från en typ tooanother. VPN-enheten måste vara kompatibel med hello routning typ du väljer. Mer information om VPN-typ finns i [Om VPN-gatewayinställningar](vpn-gateway-about-vpn-gateway-settings.md#vpntype). Du kan se artiklar hänvisar too'RouteBased' och 'PolicyBased' VPN typer. ' Dll 'motsvarar too'RouteBased', och 'Statiska' motsvarar 'PolicyBased'.
3. Klicka på **OK** toosave hello inställningar.
4. På hello **ny VPN-anslutning** klickar du på **OK** längst hello hello sidan toobegin skapar din virtuella nätverksgateway. Beroende på hello SKU som du väljer, kan det ta upp too45 minuter toocreate en virtuell nätverksgateway.

## <a name="vpndevice"></a>7. Konfigurera din VPN-enhet

Plats-till-platsanslutningar tooan lokalt nätverk kräver en VPN-enhet. I det här steget konfigurerar du VPN-enheten. När du konfigurerar VPN-enhet behöver du hello följande:

- En delad nyckel. Detta är hello samma delade nyckel som du anger när du skapar din plats-till-plats VPN-anslutning. I vårt exempel använder vi en enkel delad nyckel. Vi rekommenderar att du generera en mer komplex viktiga toouse.
- hello offentliga IP-adressen för din virtuella nätverksgateway. Du kan visa hello offentliga IP-adressen med hjälp av hello Azure-portalen, PowerShell eller CLI.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. Skapa hello-anslutning
I det här steget ange hello delad nyckel och skapa hello-anslutning. hello nyckeln som du anger måste vara hello samma nyckel som används i din konfiguration för VPN-enhet.

> [!NOTE]
> Det här steget är för närvarande inte tillgänglig i hello Azure-portalen. Du måste använda hello Service Management (SM) version av hello Azure PowerShell-cmdlets.
>

### <a name="step-1-connect-tooyour-azure-account"></a>Steg 1. Ansluta tooyour Azure-konto

1. Öppna PowerShell-konsol med utökade behörigheter och Anslut tooyour konto. Använd hello följande exempel toohelp du ansluta:

  ```powershell
  Add-AzureAccount
  ```
2. Kontrollera hello prenumerationer för hello-kontot.

  ```powershell
  Get-AzureSubscription
  ```
3. Om du har mer än en prenumeration väljer du hello prenumeration som du vill toouse.

  ```powershell
  Select-AzureSubscription -SubscriptionId "Replace_with_your_subscription_ID"
  ```

### <a name="step-2-set-hello-shared-key-and-create-hello-connection"></a>Steg 2. Ange hello delad nyckel och skapa hello-anslutning

När du arbetar med PowerShell och hello klassiska distributionsmodellen är ibland hello namnen på de resurser i hello-portalen inte hello namn hello Azure förväntar sig toosee när du använder PowerShell. hello följande steg när du exporterar hello filen tooobtain hello exakt konfigurationsvärden nätverk för hello namn.

1. Skapa en katalog på datorn och sedan exportera hello configuration file toohello nätverksresursen. I det här exemplet är hello nätverket konfigurationsfilen exporterade tooC:\AzureNet.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. Öppna konfigurationsfilen för hello nätverk med en xml-redigerare och kontrollera hello värden för 'LocalNetworkSite name' och 'VirtualNetworkSite name'. Ändra hello tooreflect hello exempelvärden som du behöver. När du anger ett namn som innehåller blanksteg, Använd enkla citattecken runt hello värde.

3. Ange hello delad nyckel och skapa hello-anslutning. hello '-SharedKey' är ett värde som du skapar och ange. I exemplet hello vi använde 'abc123', men du kan generera (och bör) använder något mer komplicerad. hello viktig sak är att hello-värde som du anger här måste vara hello samma värde som du angav när du konfigurerar VPN-enhet.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group TestRG1 TestVNet1' `
  -LocalNetworkSiteName 'D1BFC9CB_Site2' -SharedKey abc123
  ```
När hello anslutning upprättas hello resultatet är: **Status: lyckade**.

## <a name="verify"></a>9. Verifiera din anslutning

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

Om du har problem med att ansluta, se hello **Felsök** avsnitt på hello innehållsförteckning hello vänster.

## <a name="reset"></a>Hur tooreset en VPN-gateway

Du kan behöva återställa en Azure VPN-gateway om VPN-anslutningen mellan flera platser i en eller flera VPN-tunnlar för plats-till-plats bryts. I så fall kan din lokala VPN-enheter är alla fungerar, men är inte tooestablish IPsec-tunnlar med hello Azure VPN-gatewayer. Stegvisa anvisningar finns i [Återställa en VPN-gateway](vpn-gateway-resetgw-classic.md).

## <a name="changesku"></a>Hur toochange en gateway-SKU

Hello stegen toochange en gateway-SKU, finns [ändra storlek på en gateway-SKU](vpn-gateway-about-SKUS-legacy.md).

## <a name="next-steps"></a>Nästa steg

* När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Mer information finns i [Virtuella datorer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Information om tvingad tunneltrafik finns i [Om forcerade tunnlar](vpn-gateway-about-forced-tunneling.md).