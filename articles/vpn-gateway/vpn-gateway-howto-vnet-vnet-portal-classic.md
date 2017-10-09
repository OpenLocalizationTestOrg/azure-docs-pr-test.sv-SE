---
title: 'Skapa en anslutning mellan Vnet: klassiska: Azure-portalen | Microsoft Docs'
description: "Hur tooconnect Azure virtuella nätverk tillsammans med PowerShell och hello klassiska Azure-portalen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: f29c3c091d049ff96cf59f4c28ab86a100bcb5fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-connection-classic"></a>Konfigurera VNet-till-VNet-anslutning (klassisk)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Den här artikeln beskrivs hur du toocreate en VPN-gateway-anslutningen mellan virtuella nätverk. hello virtuella nätverk kan vara i samma eller olika regioner hello och hello från samma eller olika prenumerationer. hello stegen i den här artikeln gäller toohello klassiska distributionsmodellen och hello Azure-portalen. Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure Portal (klassisk)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Ansluta olika distributionsmodeller – Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Ansluta olika distributionsmodeller – PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![VNet tooVNet anslutning Diagram](./media/vpn-gateway-howto-vnet-vnet-portal-classic/v2vclassic.png)

## <a name="about-vnet-to-vnet-connections"></a>Om VNet-till-VNet-anslutningar

Ansluta ett virtuellt nätverk tooanother virtuellt nätverk (VNet-till-VNet) i hello klassiska distributionsmodellen med hjälp av en VPN-gateway är liknande tooconnecting ett virtuellt nätverk tooan lokal plats. Båda typerna av anslutningen använder en VPN-gateway tooprovide en säker tunnel med IPsec/IKE.

Hej Vnet som du ansluter kan finnas i olika prenumerationer och olika regioner. Du kan kombinera VNet tooVNet kommunikation med flera platser konfigurationer. Det innebär att du kan etablera nätverkstopologier som kombinerar anslutningen för flera platser med en intern virtuell nätverksanslutning.

![VNet tooVNet anslutningar](./media/vpn-gateway-howto-vnet-vnet-portal-classic/aboutconnections.png)

### <a name="why"></a>Varför ska man ansluta virtuella nätverk?

Du kanske vill tooconnect virtuella nätverk för hello följande orsaker:

* **Geografisk redundans i flera regioner och geografisk närvaro**

  * Du kan ange din egna geografiska replikering eller synkronisering med en säker anslutning, utan att passera några Internet-slutpunkter.
  * Med Azure belastningsutjämnare och Microsoft eller tredje parts klustring teknik, kan du ställa in högtillgänglig arbetsbelastning med geo-redundans över flera Azure-regioner. Ett viktigt exempel är tooset in SQL Always On med Tillgänglighetsgrupper sprida över flera Azure-regioner.
* **Regional program på flera nivåer med stark isoleringsgräns**

  * Hej i samma region, du kan konfigurera flera nivåer program med flera VNets anslutna tillsammans med stark isolering och kommunikationen mellan nivå.
* **Mellan kommunikation mellan organisation i Azure-prenumeration**

  * Om du har flera Azure-prenumerationer, kan du ansluta arbetsbelastningar från olika prenumerationer tillsammans på ett säkert sätt mellan virtuella nätverk.
  * Du kan aktivera kommunikation mellan organisation med säkra VPN-teknik i Azure för företag och leverantörer.

Mer information om VNet-till-VNet-anslutningar finns [VNet-till-VNet överväganden](#faq) hello slutet av den här artikeln.

### <a name="before-you-begin"></a>Innan du börjar

Innan du påbörjar den här övningen, hämta och installera hello senaste versionen av hello Azure Service Management (SM) PowerShell-cmdlets. Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). Vi använder hello portal för de flesta av hello steg, men du måste använda PowerShell toocreate hello anslutningar mellan hello Vnet. Du kan inte skapa hello anslutningar med hello Azure-portalen.

## <a name="plan"></a>Steg 1 – Planera dina IP-adressintervall

Det är viktigt toodecide hello adressintervall när du ska använda tooconfigure dina virtuella nätverk. För den här konfigurationen måste du se till att ingen av VNet-intervall överlappar varandra eller med någon av hello lokala nätverk som de ansluter till.

hello nedan visas ett exempel på hur toodefine ditt Vnet. Använd hello-intervall som en riktlinje endast. Skriv ned hello intervallen för dina virtuella nätverk. Du behöver den här informationen för senare steg.

**Exempel**

| Virtual Network | Adressutrymmet | Region | Ansluter toolocal nätverksplats |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Östra USA |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Västra USA |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

## <a name="vnetvalues"></a>Steg 2 – Skapa hello virtuella nätverk

Skapa två virtuella nätverk i hello [Azure-portalen](https://portal.azure.com). Hello stegen toocreate klassiska virtuella nätverk, finns [skapa ett klassiskt virtuellt nätverk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). 

När du använder hello portal toocreate ett klassiskt virtuellt nätverk, ska du gå toohello virtuellt nätverk-bladet med hjälp av hello följande steg, annars visas inte hello alternativet toocreate ett klassiskt virtuellt nätverk:

1. Klicka på hello ”+” tooopen hello ”New-bladet.
2. Skriv ”virtuella nätverk” i hello 'Sökningen hello marketplace'. Om du i stället väljer nätverk -> virtuella nätverk, får du inte hello alternativet toocreate ett klassiskt virtuellt nätverk.
3. Hitta ”virtuella nätverk, från hello returnerade listan och klicka på den tooopen hello virtuellt nätverk-bladet. 
4. Välj 'Klassiskt' toocreate klassiska VNet på hello virtuellt nätverk-bladet. 

Om du använder den här artikeln som Övning använder du hello följande exempelvärden:

**Värden för TestVNet1**

Namn: TestVNet1<br>
Adressutrymmet: 10.11.0.0/16, 10.12.0.0/16 (valfritt)<br>
Undernätnamnet: standard<br>
Adressintervallet i undernätet: 10.11.0.1/24<br>
Resursgrupp: ClassicRG<br>
Plats: Östra USA<br>
GatewaySubnet: 10.11.1.0/27

**Värden för TestVNet4**

Namn: TestVNet4<br>
Adressutrymmet: 10.41.0.0/16, 10.42.0.0/16 (valfritt)<br>
Undernätnamnet: standard<br>
Adressintervallet i undernätet: 10.41.0.1/24<br>
Resursgrupp: ClassicRG<br>
Plats: Västra USA<br>
GatewaySubnet: 10.41.1.0/27

**När du skapar ditt Vnet, Kom ihåg hello följande inställningar:**

* **Virtuella nätverk adressutrymmen** – hello adressutrymmen för virtuella nätverk på sidan Ange hello-adressintervall som du vill toouse för det virtuella nätverket. Dessa är hello dynamiska IP-adresser som ska tilldelas toohello virtuella datorer och andra rollinstanser som du distribuerar toothis virtuellt nätverk.<br>hello adress blanksteg som du väljer inte kan överlappa hello-adressutrymmen för alla hello andra Vnet eller lokala platser som detta virtuella nätverk ska ansluta till.

* **Plats** – när du skapar ett virtuellt nätverk, kopplar du det till en Azure-plats (region). Om du vill att dina virtuella datorer som är distribuerade tooyour virtuellt nätverk toobe fysiskt belägna i USA, västra, markerar du den platsen. Du kan inte ändra hello plats som associeras med det virtuella nätverket när du har skapat.

**När du har skapat ditt Vnet, kan du lägga till hello följande inställningar:**

* **Adressutrymmet** – ytterligare adressutrymme krävs inte för den här konfigurationen, men du kan lägga till ytterligare adressutrymme när du har skapat hello virtuella nätverk.

* **Undernät** – undernät krävs inte för den här konfigurationen, men du kanske vill toohave dina virtuella datorer i ett undernät som är åtskild från andra rollinstanser.

* **DNS-servrar** – ange hello DNS-servernamn och IP-adress. Du skapar inte en DNS-server med den här inställningen. Det gör att du toospecify hello DNS-servrar som du vill toouse för namnmatchning för det här virtuella nätverket.

I det här avsnittet Konfigurera hello typ av hello lokal plats, och skapa hello gateway.

## <a name="localsite"></a>Steg 3 – Konfigurera hello lokala platsen

Azure använder hello inställningarna som anges i varje lokalt nätverk plats toodetermine hur tooroute trafik mellan hello Vnet. Varje virtuellt nätverk måste peka toohello respektive lokala nätverk som du vill tooroute trafik till. Du kan fastställa hello-namn som du vill toouse toorefer tooeach lokal nätverksplats. Det är bästa toouse något beskrivande.

Till exempel ansluter TestVNet1 tooa lokal nätverksplats som du skapar med namnet 'VNet4Local'. hello inställningar för VNet4Local innehåller hello adressprefix för TestVNet4.

hello lokal plats för varje virtuellt nätverk är hello andra VNet. hello följande Exempelvärden används för vår konfiguration:

| Virtual Network | Adressutrymmet | Region | Ansluter toolocal nätverksplats |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Östra USA |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Västra USA |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

1. Leta upp TestVNet1 i hello Azure-portalen. I hello **VPN-anslutningar** avsnittet hello-bladet klickar du på **Gateway**.

    ![Ingen gateway](./media/vpn-gateway-howto-vnet-vnet-portal-classic/nogateway.png)
2. På hello **ny VPN-anslutning** väljer **plats-till-plats**.
3. Klicka på **lokal plats** tooopen hello lokal plats och konfigurera hello inställningar.
4. På hello **lokal plats** sidan, namnge den lokala platsen. I vårt exempel kalla vi hello lokala platsen 'VNet4Local'.
5. För **IP-adressen för VPN-gateway**, du kan använda alla IP-adresser som du vill använda, så länge som den är i ett giltigt format. Normalt använder du hello faktiska externa IP-adressen för en VPN-enhet. Men för en klassiska VNet-till-VNet-konfiguration kan du använda hello offentlig IP-adress som är tilldelad toohello gateway för din VNet. Med tanke på att du har ännu inte har skapat hello virtuell nätverksgateway, kan du ange en giltig offentlig IP-adress som platshållare.<br>Lämna inte detta tomt - det är inte valfria för den här konfigurationen. I ett senare steg, gå tillbaka till de här inställningarna och konfigurera dem med hello motsvarande virtuella nätverk gateway IP-adresser när Azure genererar den.
6. För **klientens adressutrymme**, använda hello adressutrymme hello andra VNet. Läs tooyour planera exempel. Klicka på **OK** toosave dina inställningar och returnera tillbaka toohello **ny VPN-anslutning** bladet.

    ![lokala platsen](./media/vpn-gateway-howto-vnet-vnet-portal-classic/localsite.png)

## <a name="gw"></a>Steg 4 – skapa hello virtuell nätverksgateway

Varje virtuellt nätverk måste ha en virtuell nätverksgateway. hello virtuella nätverks-gatewayen dirigerar och krypterar trafik.

1. På hello **ny VPN-anslutning** bladet, Välj hello kryssrutan **skapa gateway omedelbart**.
2. Klicka på **undernät, storlek och routning**. På hello **gatewaykonfigurationen** bladet, klickar du på **undernät**.
3. hello gateway undernätsnamn fylls i automatiskt med hello obligatoriska namnet 'GatewaySubnet'. Hej **adressintervall** innehåller hello IP-adresser som är allokerade toohello VPN gateway-tjänster. Vissa konfigurationer tillåta ett gateway-undernät med /29, men bästa toouse en /28 eller minst/27 tooaccommodate framtida konfigurationer som kan kräva flera IP-adresser för hello gateway-tjänster. Vi använder 10.11.1.0/27 i vårt exempel inställningarna. Justera hello-adressutrymme och klicka sedan på **OK**.
4. Konfigurera hello **Gateway storlek**. Den här inställningen refererar toohello [Gateway-SKU](vpn-gateway-about-vpn-gateway-settings.md#gwsku).
5. Konfigurera hello **routning typen**. Hej routning för den här konfigurationen måste vara **dynamiska**. Du kan inte ändra hello routning senare om du inte går sönder ned hello gateway och skapa en ny.
6. Klicka på **OK**.
7. På hello **ny VPN-anslutning** bladet, klickar du på **OK** toobegin skapar hello virtuell nätverksgateway. Skapa en gateway kan ofta ta 45 minuter eller mer beroende på hello markerad gateway SKU.

## <a name="vnet4settings"></a>Steg 5 – konfigurera TestVNet4 inställningar

Upprepa hello steg för[skapa en lokal plats](#localsite) och [skapa hello virtuell nätverksgateway](#gw) tooconfigure TestVNet4, ersätter hello värden vid behov. Om du gör detta som en övning, använda hello [exempelvärden](#vnetvalues).

## <a name="updatelocal"></a>Steg 6 – Uppdatera hello lokala platser

När virtuella nätverks-gateway har skapats för båda Vnet, måste du justera hello lokala platser **IP-adressen för VPN-gateway** värden.

|Namn på virtuella nätverk|Anslutna platsen|IP-adressen för gateway|
|:--- |:--- |:--- |
|TestVNet1|VNet4Local|VPN-gateway IP-adress för TestVNet4|
|TestVNet4|VNet1Local|VPN-gateway IP-adress för TestVNet1|

### <a name="part-1---get-hello-virtual-network-gateway-public-ip-address"></a>Del 1 - Get hello virtuellt gateway offentlig IP-adress

1. Leta upp det virtuella nätverket i hello Azure-portalen.
2. Klicka på tooopen hello VNet **översikt** bladet. På bladet hello i **VPN-anslutningar**, du kan visa hello IP-adressen för din virtuella nätverksgateway.

  ![Offentlig IP-adress](./media/vpn-gateway-howto-vnet-vnet-portal-classic/publicIP.png)
3. Kopiera hello IP-adress. Du använder den i hello nästa avsnitt.
4. Upprepa dessa steg för TestVNet4

### <a name="part-2---modify-hello-local-sites"></a>Del 2 – ändra hello lokala platser

1. Leta upp det virtuella nätverket i hello Azure-portalen.
2. På hello VNet **översikt** bladet, klickar du på hello lokal plats.

  ![Lokal plats som har skapats](./media/vpn-gateway-howto-vnet-vnet-portal-classic/local.png)
3. På hello **plats-till-plats VPN-anslutningar** bladet Klicka hello namnet för hello lokal plats som du vill toomodify.

  ![Öppna lokal plats](./media/vpn-gateway-howto-vnet-vnet-portal-classic/openlocal.png)
4. Klicka på hello **lokal plats** som du vill toomodify.

  ![Ändra plats](./media/vpn-gateway-howto-vnet-vnet-portal-classic/connections.png)
5. Uppdatera hello **IP-adressen för VPN-gateway** och på **OK** toosave hello inställningar.

  ![gateway-IP](./media/vpn-gateway-howto-vnet-vnet-portal-classic/gwupdate.png)
6. Stäng hello andra blad.
7. Upprepa dessa steg för TestVNet4.

## <a name="getvalues"></a>Steg 7 – hämta värden från konfigurationsfilen för hello nätverk

När du skapar klassiska Vnet i hello Azure-portalen är hello-namnet som visas inte hello fullständiga namn som du använder för PowerShell. Till exempel ett VNet som visas med namnet toobe **TestVNet1** i hello-portalen kan ha en mycket längre namn i konfigurationsfilen för hello nätverk. hello namn kan se ut ungefär så: **grupp ClassicRG TestVNet1**. När du skapar dina anslutningar är det viktigt toouse hello värden som visas i konfigurationsfilen för hello nätverk.

I följande steg hello, ansluter du tooyour Azure-konto och hämta och visa hello network configuration file tooobtain hello värden som krävs för anslutningar.

1. Hämta och installera hello senaste versionen av hello Azure Service Management (SM) PowerShell-cmdlets. Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

2. Öppna PowerShell-konsol med utökade behörigheter och Anslut tooyour konto. Använd hello följande exempel toohelp du ansluta:

  ```powershell
  Login-AzureRmAccount
  ```

  Kontrollera hello prenumerationer för hello-kontot.

  ```powershell
  Get-AzureRmSubscription
  ```

  Om du har mer än en prenumeration väljer du hello prenumeration som du vill toouse.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```

  Använd sedan följande cmdlet tooadd hello tooPowerShell din Azure-prenumeration för hello klassiska distributionsmodellen.

  ```powershell
  Add-AzureAccount
  ```
3. Exportera och visa hello nätverket konfigurationsfilen. Skapa en katalog på datorn och sedan exportera hello configuration file toohello nätverksresursen. I det här exemplet exporteras hello nätverket konfigurationsfilen för**C:\AzureNet**.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
4. Öppna hello-filen med ett text-redigeraren och visa hello namn för din Vnet och platser. Dessa ska vara hello-namn som du använder när du skapar dina anslutningar.<br>VNet namn visas som **VirtualNetworkSite namn =**<br>Platsnamn listas som **LocalNetworkSiteRef namn =**

## <a name="createconnections"></a>Steg 8 – skapa hello VPN gateway-anslutningar

Du kan ange hello IPsec/IKE i förväg delade nycklar och skapa hello-anslutning när alla hello tidigare steg har slutförts. Den här uppsättningen anvisningar använder PowerShell. VNet-till-VNet-anslutningar för hello klassiska distributionsmodellen kan inte konfigureras i hello Azure-portalen.

I exemplen hello är exakt Observera att hello delad nyckel hello samma. hello delad nyckel måste alltid matcha. Vara säker på att tooreplace hello värdena i de här exemplen med hello exakt namn för din Vnet och lokala nätverksplatser.

1. Skapa hello TestVNet1 tooTestVNet4 anslutning.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet1' `
  -LocalNetworkSiteName '17BE5E2C_VNet4Local' -SharedKey A1b2C3D4
  ```
2. Skapa hello TestVNet4 tooTestVNet1 anslutning.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet4' `
  -LocalNetworkSiteName 'F7F7BFC7_VNet1Local' -SharedKey A1b2C3D4
  ```
3. Vänta tills hello anslutningar tooinitialize. Hello Status är 'Lyckade' när hello gateway har initierats.

  ```
  Error          :
  HttpStatusCode : OK
  Id             :
  Status         : Successful
  RequestId      :
  StatusCode     : OK
  ```

## <a name="faq"></a>VNet-till-VNet-överväganden för klassiska Vnet
* hello virtuella nätverk kan vara i hello samma eller olika prenumerationer.
* hello virtuella nätverk kan vara i hello samma eller olika Azure-regioner (platser).
* En molntjänst eller en slutpunkt för belastningsutjämning kan inte omfatta flera virtuella nätverk, även om de är sammankopplade.
* Ansluta flera virtuella nätverk till varandra kräver inte någon VPN-enheter.
* VNet-till-VNet stöder anslutande virtuella Azure-nätverk. Stöder inte ansluta virtuella datorer eller molntjänster som inte är distribuerat tooa virtuellt nätverk.
* VNet-till-VNet kräver dynamiska routnings-gatewayer. Azure statisk routning gateways stöds inte.
* Virtuell nätverksanslutning kan användas samtidigt med VPN på flera platser. Det finns högst 10 VPN-tunnlar för en VPN-gateway för virtuellt nätverk som ansluter tooeither andra virtuella nätverk eller lokala platser.
* hello adressutrymmen till hello virtuella nätverk och lokala lokala nätverksplatser inte får överlappa. Överlappande adressutrymmen kommer hello skapa virtuella nätverk eller överför netcfg configuration filer toofail.
* Redundanta tunnlar mellan ett virtuellt nätverkspar stöds inte.
* Alla VPN-tunnlar för hello VNet, inklusive P2S VPN hello tillgänglig bandbredd för hello VPN-gateway och dela hello samma VPN gateway drifttid SLA i Azure.
* VNet-till-VNet-trafik skickas via hello Azure stamnät.

## <a name="next-steps"></a>Nästa steg
Verifiera dina anslutningar. Se [verifiera en anslutning för VPN-Gateway](vpn-gateway-verify-connection-resource-manager.md).
