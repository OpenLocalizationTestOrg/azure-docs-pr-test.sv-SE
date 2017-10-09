---
title: "Ansluta en dator tooan Azure virtuellt nätverk med punkt-till-plats och certifikatautentisering: PowerShell | Microsoft Docs"
description: "På ett säkert sätt ansluta ett virtuellt nätverk för dator-tooyour genom att skapa en punkt-till-plats VPN-gateway-anslutningen genom att använda certifikat. Den här artikeln gäller toohello Resource Manager-modellen och använder PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2017
ms.author: cherylmc
ms.openlocfilehash: b962e4b1946a4ae17d4eb2b920ed54437bc26b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-powershell"></a>Konfigurera en punkt-till-plats-anslutning tooa VNet med hjälp av autentisering med datorcertifikat: PowerShell

Den här artikeln visar hur toocreate ett VNet med en punkt-till-plats-anslutning i hello Resource Manager distribution modellen med hjälp av PowerShell. Den här konfigurationen använder certifikat tooauthenticate hello ansluter klienten. Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (klassisk)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

En punkt-till-plats (P2S) VPN-gateway kan du skapa en säker anslutning tooyour virtuellt nätverk från en enskild klientdator. Punkt-till-plats VPN-anslutningar är användbara när du vill tooconnect tooyour virtuella nätverk från en annan plats, exempelvis när du hemifrån home eller en konferens. P2S VPN är också en bra lösning toouse i stället för en plats-till-plats VPN-anslutning när du har bara ett fåtal klienter som behöver tooconnect tooa VNet.

P2S använder hello Secure Socket Tunneling Protocol (SSTP), vilket är ett SSL-baserad VPN-protokoll. En P2S VPN-anslutning upprättas genom att starta från hello-klientdator.

![Ansluta en dator tooan Azure VNet - punkt-till-plats-anslutning diagram](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

Punkt-till-plats certifikat autentisering anslutningar kräver hello följande:

* En RouteBased VPN-gateway.
* hello offentliga nyckel (.cer-fil) för ett rotcertifikat som är överförda tooAzure. När hello certifikat har överförts betraktas som ett betrott certifikat och används för autentisering.
* Ett klientcertifikat som genereras från hello rotcertifikat och installeras på varje klientdator som ansluter toohello VNet. Det här certifikatet används för klientautentisering.
* Ett konfigurationspaket för VPN-klienten. hello VPN-klientpaketet konfiguration innehåller hello nödvändig information för hello klienten tooconnect toohello VNet. hello paketet konfigurerar hello befintliga VPN-klient som är inbyggda toohello Windows-operativsystem. Alla klienter som ansluter måste konfigureras med hjälp av hello konfigurationspaket.

Punkt-till-plats-anslutningar kräver inte någon VPN-enhet eller en lokal offentlig IP-adress. hello VPN-anslutningen har skapats via SSTP (Secure Socket Tunneling Protocol). Vi stöder SSTP version 1.0, 1.1 och 1.2 på hello serversidan. hello klienten avgör vilken version toouse. För Windows 8.1 och senare, använder SSTP version 1.2 som standard. 

Mer information om anslutningar för punkt-till-plats finns hello [punkt-till-plats vanliga frågor och svar](#faq) hello slutet av den här artikeln.

## <a name="before-beginning"></a>Innan du börjar

* Kontrollera att du har en Azure-prenumeration. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial).
* Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets. Mer information om hur du installerar PowerShell-cmdlets finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

### <a name="example"></a>Exempelvärden

Du kan använda hello exempel värden toocreate en testmiljö eller referera toothese värden toobetter förstå hello exemplen i den här artikeln. Vi ange hello variabler i avsnittet [1](#declare) av hello artikel. Du kan antingen använda hello steg som en genomgång och använda hello värden utan att ändra dem eller ändra dem tooreflect din miljö. 

* **Namn: VNet1**
* **Adressutrymme: 192.168.0.0/16** and **10.254.0.0/16**<br>Det här exemplet använder vi mer än en adressutrymme tooillustrate som den här konfigurationen fungerar med flera adressutrymmen. Flera adressutrymmen krävs dock inte för den här konfigurationen.
* **Undernätsnamn: FrontEnd**
  * **Adressintervall för undernätet: 192.168.1.0/24**
* **Undernätsnamn: BackEnd**
  * **Adressintervall för undernätet: 10.254.1.0/24**
* **Undernätsnamn: GatewaySubnet**<br>Hej undernätsnamn *GatewaySubnet* är obligatoriskt för hello VPN-gateway toowork.
  * **Adressintervall för gateway-undernätet: 192.168.200.0/24** 
* **VPN-klientadresspool: 172.16.201.0/24**<br>VPN-klienter som ansluter toohello VNet med den här punkt-till-plats-anslutningen för att ta emot en IP-adress från hello VPN-klientadresspool.
* **Prenumerationen:** om du har mer än en prenumeration, kontrollera att du använder hello korrekt.
* **Resursgrupp: TestRG**
* **Plats: Östra USA**
* **DNS-Server: IP-adress** för hello DNS-server som du vill toouse för namnmatchning.
* **GW-namn: Vnet1GW**
* **Offentligt IP-namn: VNet1GWPIP**
* **VPNType: RouteBased** 

## <a name="declare"></a>1. Logga in och ange variabler

I det här avsnittet, logga in och deklarera hello-värden som används för den här konfigurationen. hello deklareras värden används i hello exempelskript. Ändra hello värden tooreflect din egen miljö. Eller, du kan använda hello deklarerats värden och gå igenom hello steg som övning.

1. Öppna PowerShell-konsol med utökade privilegier och logga in tooyour Azure-konto. Denna cmdlet efterfrågar hello inloggningsuppgifter. När du loggar in hämtar den inställningarna för ditt konto, så att de är tillgängliga tooAzure PowerShell.

  ```powershell
  Login-AzureRmAccount
  ```
2. Hämta en lista över dina Azure-prenumerationer.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Ange hello prenumeration som du vill toouse.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
4. Deklarera hello variabler som du vill toouse. Använd hello följande exempel, ersätter hello värden för din egen vid behov.

  ```powershell
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $DNS = "10.1.1.3"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <a name="ConfigureVNet"></a>2. Konfigurera ett virtuellt nätverk

1. Skapa en resursgrupp.

  ```powershell
  New-AzureRmResourceGroup -Name $RG -Location $Location
  ```
2. Skapa hello undernät för virtuellt nätverk hello, namnge dem *klientdel*, *BackEnd*, och *GatewaySubnet*. Dessa prefix måste vara en del av hello VNet-adressutrymmet som du har deklarerats.

  ```powershell
  $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
  $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
  $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
  ```
3. Skapa hello virtuellt nätverk.

  I det här exemplet är hello DNS-servern valfria. Ingen ny DNS-server skapas när du anger ett värde. hello ska DNS-serverns IP-adress som du anger vara en DNS-server som kan lösa hello namn för hello-resurser som du ansluter till. Vi använde en privat IP-adress för det här exemplet, men det är troligt att detta inte är hello IP-adressen för DNS-servern. Vara säker på att toouse egna värden.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS
  ```
4. Ange hello variabler för hello virtuella nätverk som du skapade.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  ```
5. En VPN-gateway måste ha en offentlig IP-adress. Du först begära hello IP-adressresurs och läser sedan jobbhistorikposten tooit när du skapar din virtuella nätverksgateway. hello IP-adressen tilldelas dynamiskt toohello resurs när hello VPN-gateway har skapats. VPN-gateway stöder för närvarande endast *dynamisk* offentlig IP-adressallokering. Du kan inte begära en statisk offentlig IP-adresstilldelning. Dock betyder det att hello IP-adressen ändras när den har tilldelats tooyour VPN-gateway. hello endast tid hello offentliga IP-adressändringarna är när hello gateway bort och återskapas. Den ändras inte vid storleksändring, återställning eller annat internt underhåll/uppgraderingar av din VPN-gateway.

  Begär en dynamiskt tilldelad offentlig IP-adress.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```

## <a name="creategateway"></a>3. Skapa hello VPN-gateway

Konfigurera och skapa hello virtuell nätverksgateway för din VNet.

* Hej *- GatewayType* måste vara **Vpn** och hello *- VpnType* måste vara **RouteBased**.
* En VPN-gateway kan ta upp too45 minuter toocomplete, beroende på hello [gateway-sku](vpn-gateway-about-vpn-gateway-settings.md) du väljer.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 `
```

## <a name="addresspool"></a>4. Lägg till hello VPN-klientadresspool

Du kan lägga till hello VPN-klientadresspool när hello VPN-gateway är klar att skapa. hello VPN-klientadresspool är hello-intervall som hello VPN-klienter tar emot en IP-adress vid anslutning. Använd en privat IP-adressintervall som inte överlappar hello lokal plats som du ansluter från eller med hello virtuella nätverk som du vill tooconnect till. I det här exemplet hello VPN-klientadresspool har deklarerats som en [variabeln](#declare) i steg 1.

```powershell
$Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
```

## <a name="Certificates"></a>5. Generera certifikat

Certifikat som används av Azure tooauthenticate VPN-klienter för plats-till-plats-VPN. Du kan överföra hello offentlig nyckelinformation för hello root certificate tooAzure. 'betrodda' anses hello offentliga nyckel. Klientcertifikat måste genereras från hello betrodda rotcertifikat och sedan installeras på varje dator i hello certifikat-aktuell användare/personliga certifikatarkiv. hello certifikat är används tooauthenticate hello klient när den upprättar en anslutning toohello VNet. 

Om du använder självsignerade certifikat, måste de skapas med specifika parametrar. Du kan skapa ett självsignerat certifikat med hjälp av hello instruktioner för [PowerShell och Windows 10](vpn-gateway-certificates-point-to-site.md), eller om du inte har Windows 10 kan du använda [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Det är viktigt att du följer hello stegen i hello instruktioner vid generering av självsignerade rotcertifikat och klientcertifikat. Annars hello-certifikat som du skapar är inte kompatibel med P2S-anslutningar och du får ett anslutningsfel.

### <a name="cer"></a>1. Hämta hello .cer-fil för hello rotcertifikat

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]


### <a name="generate"></a>2. Generera ett klientcertifikat

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>6. Överför hello root certificate offentlig nyckelinformation

Kontrollera att din VPN-gateway har skapats. Du kan överföra hello .cer-filen (som innehåller information om hello offentliga nycklar) för en betrodd rot certifikat tooAzure när den är klar. När a.cer filen har överförts kan Azure använda den tooauthenticate klienter som har installerat ett klientcertifikat som genereras från hello betrott rotcertifikat. Du kan överföra ytterligare betrodda rotcertifikat certifikatfiler - upp tooa totalt 20 – senare, om det behövs.

1. Deklarera hello variabel för din certifikatnamn ersätta hello värdet med din egen.

  ```powershell
  $P2SRootCertName = "P2SRootCert.cer"
  ```
2. Ersätt hello sökväg med din egen och kör sedan hello-cmdlets.

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
  ```
3. Överför hello offentlig nyckelinformation tooAzure. När hello certifikatinformationen överförs Azure tar hänsyn till den här toobe ett betrott rotcertifikat.

   ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
  ```

## <a name="clientconfig"></a>7. Hämta hello VPN-klientpaketet konfiguration

tooconnect tooa VNet med en punkt-till-plats-VPN varje klient måste installera ett konfigurationspaket till klienten som konfigurerar hello inbyggda VPN-klienten med hello inställningar och filer som är nödvändiga tooconnect toohello virtuellt nätverk. hello VPN-klientpaketet configuration konfigurerar hello inbyggda Windows VPN-klienten, den installera inte en ny eller en annan VPN-klienten. 

Du kan använda samma VPN-klientkonfiguration paketet på varje klientdator hello så länge hello versionen matchar hello arkitektur för hello-klienten. Hello lista av klientoperativsystem som stöds finns i hello [punkt-till-plats-anslutningar och svar](#faq) hello slutet av den här artikeln.

1. När hello gateway har skapats kan du generera och hämta hello klientpaketet för konfiguration. Det här exemplet hämtar hello-paket för 64-bitarsklienter. Om du vill toodownload hello 32-bitars klienten Ersätt 'Amd64' med 'x86'. Du kan också hämta hello VPN-klienten med hjälp av hello Azure-portalen.

  ```powershell
  Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
  -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
  ```
2. Kopiera och klistra in hello länk som returneras tooa web webbläsare toodownload hello paketet, tar hand tooremove hello citattecken runt hello länk. 
3. Hämta och installera hello paketet på hello-klientdator. Om du ser ett SmartScreen-popup-fönster klickar du på **Mer information** och sedan på **Kör ändå**. Du kan också spara hello paketet tooinstall på andra datorer.
4. Hello klientdator, navigera för**nätverksinställningar** och på **VPN**. hello VPN-anslutningen visar hello namnet på hello virtuellt nätverk som den ansluter till.

## <a name="clientcertificate"></a>8. Installera ett exporterat klientcertifikat

Om du vill toocreate en P2S-anslutning från en klientdator än hello du används klientcertifikat för toogenerate hello måste du tooinstall ett klientcertifikat. När du installerar ett klientcertifikat, måste hello lösenordet som skapades när hello klientcertifikat exporterades. Vanligtvis är det bara gäller att om du dubbelklickar på hello certifikat och installera den.

Kontrollera att hello Klientcertifikatet har exporterats som en .pfx tillsammans med hello hela certifikatkedjan (som standard hello). Annars hello rot certifikatinformationen inte finns på hello klientdator och hello-klienten inte kan tooauthenticate korrekt. Mer information finns i [Installera ett exporterat klientcertifikat](vpn-gateway-certificates-point-to-site.md#install). 

## <a name="connect"></a>9. Ansluta tooAzure

1. tooconnect tooyour VNet, hello klientdator, navigera tooVPN anslutningar och leta upp hello VPN-anslutning som du skapade. Det heter hello samma namn som det virtuella nätverket. Klicka på **Anslut**. Ett popup-meddelande kan visas som refererar toousing hello certifikat. Klicka på **Fortsätt** toouse utökade behörigheter. 
2. På hello **anslutning** statussidan, klickar du på **Anslut** toostart hello anslutning. Om du ser en **Välj certifikat** skärmen och kontrollera att hello klienten certifikatet visar är hello något som du vill toouse tooconnect. Om det inte använda hello listrutepilen tooselect hello rätt certifikat och klicka sedan på **OK**.

  ![VPN-klienten ansluter tooAzure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. Anslutningen upprättas.

  ![Anslutning upprättad](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Felsöka P2S-anslutningar

[!INCLUDE [client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify"></a>10. Verifiera din anslutning

1. tooverify att VPN-anslutningen är aktiv, öppna en upphöjd kommandotolk och kör *ipconfig/all*.
2. Visa hello resultat. Observera att hello IP-adress som du fått är en av hello-adresser inom hello punkt-till-plats VPN-Klientadresspool som du angav i konfigurationen. hello resultatet är liknande toothis exempel:

  ```
  PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
  ```

## <a name="connectVM"></a>Ansluta tooa virtuell dator

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="addremovecert"></a>Lägga till eller ta bort ett rotcertifikat

Du kan lägga till och ta bort betrodda rotcertifikat från Azure. När du tar bort ett rotcertifikat klienter som har ett certifikat som genereras från hello rotcertifikatet inte kan autentisera och inte kan tooconnect. Om du vill att en klient tooauthenticate och ansluter behöver du ett nytt klientcertifikat genereras från ett rotcertifikat som är betrodd (överförda) tooAzure tooinstall.

### <a name="addtrustedroot"></a>tooadd ett betrott rotcertifikat

Du kan lägga upp too20 root certificate .cer filer tooAzure. hello följande steg hjälp om du lägger till ett rotcertifikat:

#### <a name="certmethod1"></a>Metod 1

Detta är hello effektivaste metoden tooupload ett rotcertifikat.

1. Förbered hello .cer-filen tooupload:

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert3.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64_3 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64_3
  ```
2. Överför hello-fil. Du kan bara ladda upp en fil i taget.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64_3
  ```

3. tooverify som hello certifikatfilen upp:

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

#### <a name="certmethod2"></a>Metod 2

Den här metoden är har fler steg än metod 1, men har hello samma resultat. Det ingår om du behöver information om tooview hello certifikat.

1. Skapa och förbereda hello nya root certificate tooadd tooAzure. Exportera hello offentlig nyckel som en Base64-kodad X.509 (. CER) och öppna den med en textredigerare. Kopiera hello värden som visas i följande exempel hello:

  ![certifikat](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

  > [!NOTE]
  > När du kopierar hello certifikatdata måste du kopiera hello text som en kontinuerlig rad utan vagnreturer eller radmatningstecken. Du kan behöva toomodify vyn i hello text editor too'Show symbolen/Visa alla tecken toosee hello transport returnerar och radmatningar.
  >
  >

2. Ange hello certifikatets namn och viktig information som en variabel. Ersätt hello information med ditt eget, vilket visas i hello följande exempel:

  ```powershell
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
3. Lägg till nytt rotcertifikat för hello. Du kan bara lägga till ett certifikat i taget.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
4. Du kan verifiera att hello det nya certifikatet har lagts till korrekt genom att använda hello följande exempel:

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

### <a name="removerootcert"></a>tooremove ett rotcertifikat

1. Deklarera hello variabler.

  ```powershell
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
2. Ta bort hello certifikat.

  ```powershell
  Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
3. Använd hello följande exempel tooverify som hello certifikatet har tagits bort.

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

## <a name="revoke"></a>Återkalla ett klientcertifikat

Du kan återkalla certifikat. hello certifikat listan över återkallade certifikat kan du tooselectively neka punkt-till-plats-anslutning baserat på enskilda klientcertifikat. Det här skiljer sig från att ta bort ett betrott rotcertifikat. Om du tar bort en betrodda certifikat .cer från Azure återkallar hello åtkomst för alla klientcertifikat som genererats/signerats av hello återkallade rotcertifikat. Återkalla ett certifikat kan i stället för hello rotcertifikatet, hello andra certifikat som har genererats från hello root certificate toocontinue toobe används för autentisering.

hello vanligt är toouse hello certifikat toomanage rotåtkomst på grupp eller organisation nivåer, medan återkallade klientcertifikaten för detaljerad åtkomstkontroll på enskilda användare.

### <a name="revokeclientcert"></a>toorevoke ett klientcertifikat

1. Hämta hello Klientcertifikatets tumavtryck. Mer information finns i [hur tooretrieve hello tumavtrycket för ett certifikat](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopiera hello information tooa textredigerare och ta bort alla blanksteg så att det är en kontinuerlig sträng. Den här strängen har deklarerats som en variabel i hello nästa steg.
3. Deklarera hello variabler. Kontrollera att toodeclare hello tumavtryck som du hämtade i hello föregående steg.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
4. Lägg till hello tumavtrycket toohello lista över återkallade certifikat. ”Lyckades” visas när hello tumavtryck har lagts till.

  ```powershell
  Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
  -Thumbprint $RevokedThumbprint1
  ```
5. Kontrollera att hello tumavtryck har lagts till toohello lista över återkallade certifikat.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```
6. När hello tumavtryck har lagts att hello certifikatet inte längre använda tooconnect. Klienter som försöker tooconnect med det här certifikatet får ett meddelande om hello certifikatet är inte längre giltig.

### <a name="reinstateclientcert"></a>tooreinstate ett klientcertifikat

Du kan återställa ett klientcertifikat genom att ta bort hello tumavtrycket hello listan över återkallade certifikat.

1. Deklarera hello variabler. Kontrollera att du deklarera hello rätt tumavtrycket för hello certifikat som du vill tooreinstate.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
2. Ta bort hello certifikatets tumavtryck från hello lista över återkallade certifikat.

  ```powershell
  Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
  ```
3. Kontrollera om hello tumavtrycket tas bort från hello återkallas lista.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```

## <a name="faq"></a>Vanliga frågor och svar om punkt-till-plats

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Nästa steg
När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Mer information finns i [Virtuella datorer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). toounderstand mer information om nätverk och virtuella datorer, se [översikt över Azure och Linux VM](../virtual-machines/linux/azure-vm-network-overview.md).
