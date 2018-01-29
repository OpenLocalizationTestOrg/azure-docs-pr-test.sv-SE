---
title: "Ansluta en dator till ett virtuellt Azure-nätverk med punkt-till-plats och intern Azure-certifikatautentisering: Azure Portal | Microsoft Docs"
description: "Anslut en dator till ditt virtuella Azure-nätverk på ett säkert sätt genom att skapa en VPN-gatewayanslutning för punkt-till-plats med certifikatautentisering. Den här artikeln gäller Resource Manager-distributionsmodellen och använder Azure-portalen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2018
ms.author: cherylmc
ms.openlocfilehash: 39129572ac9908429dc9b9ef64930e896afc355f
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/18/2018
---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-native-azure-certificate-authentication-azure-portal"></a>Konfigurera en punkt-till-plats-anslutning till ett virtuellt nätverk med intern Azure-certifikatautentisering: Azure-portalen

Den här artikeln visar dig hur du skapar ett virtuellt nätverk med en punkt-till-plats-anslutning i Resource Manager-distributionsmodellen med Azure Portal. Den här konfigurationen använder certifikat för autentisering. I den här konfigurationen utförs certifikatverifieringen av Azure VPN-gatewayen, i stället för en RADIUS-server. Du kan också skapa den här konfigurationen med ett annat distributionsverktyg eller en annan distributionsmodell genom att välja ett annat alternativ i listan nedan:

> [!div class="op_single_selector"]
> * [Azure-portalen](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (klassisk)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Med en VPN-gateway med P2S-konfiguration (punkt-till-plats) kan du skapa en säker anslutning till ditt virtuella nätverk från en enskild klientdator. Punkt-till-plats-VPN-anslutningar är användbara när du vill fjärransluta till ditt VNet, exempelvis när du distansarbetar från hemmet eller en konferens. En P2S-VPN-anslutning är också en bra lösning att använda i stället för en plats-till-plats-VPN-anslutning när du bara har ett fåtal klienter som behöver ansluta till ett VNet. En P2S VPN-anslutning startas från Windows- och Mac-enheter.

Anslutande klienter kan använda följande autentiseringsmetoder:

* RADIUS-server
* Intern Azure-certifikatautentisering med VPN-gateway

Den här artikeln beskriver hur du konfigurerar en P2S-konfiguration med autentisering med hjälp av Azures interna certifikatautentisering. Om du vill använda RADIUS för att autentisera användare som ansluter läser du [P2S using RADIUS authentication](point-to-site-how-to-radius-ps.md) (P2S med RADIUS-autentisering).

![Ansluta en dator till Azure VNet – punkt-till-plats-anslutningsdiagram](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/p2snativeps.png)

Punkt-till-plats-anslutningar kräver inte någon VPN-enhet eller en offentlig IP-adress. P2S skapar VPN-anslutningen via SSTP (Secure Socket Tunneling Protocol) eller IKEv2.

* SSTP är en SSL-baserad VPN-tunnel som endast stöds på Windows-klientplattformar. Den kan ta sig förbi brandväggar, vilket gör den till ett utmärkt alternativ för att ansluta till Azure var som helst. På serversidan stöder vi SSTP version 1.0, 1.1 och 1.2. Klienten avgör vilken version som ska användas. För Windows 8.1 och senare, använder SSTP version 1.2 som standard.

* IKEv2 VPN, en standardbaserad IPsec VPN-lösning. IKEv2 VPN kan användas för att ansluta från Mac-enheter (OSX-versionerna 10.11 och senare).

Punkt-till-plats-anslutningar med intern Azure-certifikatautentisering kräver följande:

* En RouteBased VPN-gateway.
* Den offentliga nyckeln (CER-fil) för ett rotcertifikat, som överförts till Azure. När certifikatet har laddats upp betraktas det som betrott och används för autentisering.
* Ett klientcertifikat som genereras från rotcertifikatet och installeras på varje klientdator som ska ansluta till det virtuella nätverket. Det här certifikatet används för klientautentisering.
* En VPN-klientkonfiguration. VPN-klientkonfigurationsfilerna innehåller all information som krävs för att klienten ska kunna ansluta till det virtuella nätverket. Filerna konfigurerar den befintliga VPN-klienten som är inbyggd i operativsystemet. Alla klienter som ansluter måste vara konfigurerade med inställningarna i konfigurationsfilerna.

Mer information om punkt-till-plats-anslutningar finns i [About Point-to-Site connections](point-to-site-about.md) (Om punkt-till-plats-anslutningar).

#### <a name="example"></a>Exempelvärden

Du kan använda följande värden till att skapa en testmiljö eller hänvisa till dem för att bättre förstå exemplen i den här artikeln.

* **Namn på virtuellt nätverk:** VNet1
* **Adressutrymme:** 192.168.0.0/16<br>I det här exemplet använder vi bara ett adressutrymme. Du kan ha fler än ett adressutrymme för ditt virtuella nätverk.
* **Namn på undernät:** FrontEnd
* **Adressintervall för undernät:** 192.168.1.0/24
* **Prenumeration:** Kontrollera att du använder rätt prenumeration om du har mer än en.
* **Resursgrupp:** TestRG
* **Plats:** Östra USA
* **GatewaySubnet:** 192.168.200.0/24<br>
* **DNS-server: IP-adress** (valfri) för den DNS-server som du vill använda för namnmatchning.
* **Namn på virtuell nätverksgateway:** VNet1GW
* **Typ av gateway:** VPN
* **Typ av VPN:** Routningsbaserad
* **Namn på offentlig IP-adress:** VNet1GWpip
* **Anslutningstyp:** Punkt-till-plats
* **Klientadresspool:** 172.16.201.0/24<br>VPN-klienter som ansluter till det virtuella nätverket med den här punkt-till-plats-anslutningen får en IP-adress från klientadresspoolen.

## <a name="createvnet"></a>1. Skapa ett virtuellt nätverk

Kontrollera att du har en Azure-prenumeration innan du börjar. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial).
[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-p2s-vnet-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>2. Lägga till ett gatewayundernät

Innan du ansluter det virtuella nätverket till en gateway, måste du skapa gateway-undernätet för det virtuella nätverk som du vill ansluta till. Gatewaytjänsterna använder de IP-adresser som finns angivna i gatewayundernätet. Om möjligt är det bäst att skapa ett gateway-undernät med CIDR-block /28 eller /27 för att tillhandahålla tillräckligt med IP-adresser för att hantera ytterligare framtida konfigurationskrav.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-p2s-rm-portal-include.md)]

## <a name="dns"></a>3. Ange en DNS-server (valfritt)

När du har skapat ditt virtuella nätverk kan du lägga till IP-adressen för en DNS-server för att hantera namnmatchning. DNS-server är valfritt för den här konfigurationen men krävs om du vill använda namnmatchning. Ingen ny DNS-server skapas när du anger ett värde. Den angivna IP-adressen för DNS-servern måste vara en DNS-server som kan matcha namnen för de resurser som du ansluter till. I det här exemplet har vi använt en privat IP-adress, men förmodligen inte IP-adressen till din DNS-server. Använd dina egna värden. Värdet som du anger används av de resurser som du distribuerar till det virtuella nätverket, inte av P2S-anslutningen eller VPN-klienten.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>4. Skapa en virtuell nätverksgateway

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

## <a name="generatecert"></a>5. Generera certifikat

Certifikat används av Azure för att autentisera klienter som ansluter till ett virtuella nätverk över en VPN-anslutning från punkt till plats. När du har hämtat ett rotcertifikat [laddar du upp](#uploadfile) information om den offentliga nyckeln till Azure. Rotcertifikatet betraktas av Azure som betrott för punkt till plats-anslutning till det virtuella nätverket. Du skapar också klientcertifikat från det betrodda rotcertifikatet och installerar dem sedan på varje klientdator. Klientcertifikatet används för att autentisera klienten när den påbörjar en anslutning till det virtuella nätverket. 

### <a name="getcer"></a>1. Hämta .cer-filen för rotcertifikatet

[!INCLUDE [root-certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>2. Generera ett klientcertifikat

[!INCLUDE [generate-client-cert](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>6. Lägga till klientadresspoolen

Klientens adresspool är ett intervall med privata IP-adresser som du anger. Klienterna som ansluter via en VPN-anslutning från punkt till plats får en IP-adress från det här intervallet. Använd ett intervall för privata IP-adresser som inte överlappar med den lokala platsen som du ansluter från, eller med det virtuella nätverk som du vill ansluta till.

1. När den virtuella nätverksgatewayen har skapats går du till avsnittet **Inställningar** på sidan för den virtuella nätverksgatewayen. I avsnittet **Inställningar** klickar du på **Punkt-till-plats-konfiguration** så att sidan **Konfiguration** öppnas.

  ![Sidan Punkt-till-plats](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/gatewayblade.png)
2. På sidan **Punkt-till-plats-konfiguration** kan du ta bort det automatiskt ifyllda intervallet och sedan lägga till det intervall med privata IP-adresser som du vill använda. Validera och spara inställningen genom att klicka på **Spara**.

  ![Klientadresspool](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>7. Ladda upp offentliga certifikatdata från rotcertifikatet

När gatewayen har skapats laddar du upp informationen om den offentliga nyckeln till Azure. När informationen har laddats upp kan Azure använda den för att autentisera klienter som har ett installerat klientcertifikat skapat från det betrodda rotcertifikatet. Du kan ladda upp totalt ytterligare 20 stycken betrodda rotcertifikat.

1. Certifikat läggs till på sidan **Punkt-till-plats-konfiguration** i avsnittet **Rotcertifikat**.  
2. Kontrollera att du har exporterat rotcertifikatet som en Base64-kodad X.509-fil (.cer). Du behöver exportera certifikatet i det här formatet så att du kan öppna certifikatet med textredigerare.
3. Öppna certifikatet med en textredigerare, till exempel Anteckningar. När du kopierar certifikatdata ska du se till att kopiera texten som en kontinuerlig rad utan vagnreturer och radmatningar. Du kan behöva ändra vyn i textredigeraren till ”Visa symbol/Visa alla tecken” så att vagnreturer och radmatningar visas. Kopiera enbart följande avsnitt som en kontinuerlig rad:

  ![Certifikatdata](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Klistra in certifikatdata i fältet **Offentliga certifikatdata**. Ge certifikatet ett **Namn** och klicka sedan på **Spara**. Du kan lägga till upp till 20 betrodda rotcertifikat.

  ![Certifikatuppladdning](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="installclientcert"></a>8. Installera ett exporterat klientcertifikat

Om du vill skapa en P2S-anslutning från en annan klientdator än den som du använde för att generera klientcertifikat, måste du installera ett klientcertifikat. När du installerar ett klientcertifikat behöver du lösenordet som skapades när klientcertifikatet exporterades.

Kontrollera att klientcertifikatet har exporterats som PFX-fil tillsammans med hela certifikatkedjan (standardinställningen). I annat fall finns inte rotcertifikatuppgifterna på klientdatorn och klienten kan inte autentiseras.

Installationsanvisningar finns i [Install a client certificate](point-to-site-how-to-vpn-client-install-azure-cert.md) (Installera ett klientcertifikat).

## <a name="clientconfig"></a>9. Skapa och installera VPN-klientkonfigurationspaketet

VPN-klientkonfigurationsfilerna innehåller inställningar för att konfigurera enheter så att de ansluter till ett virtuellt nätverk via en P2S-anslutning. Anvisningar som beskriver hur du genererar och installerar VPN-klientkonfigurationsfiler finns i [Create and install VPN client configuration files for native Azure certificate authentication P2S configurations](point-to-site-vpn-client-configuration-azure-cert.md) (Skapa och installera VPN-klientkonfigurationsfiler för intern Azure-certifikatautentisering med P2S-konfigurationer).

## <a name="connect"></a>10. Anslut till Azure

### <a name="to-connect-from-a-windows-vpn-client"></a>Så här ansluter du från en Windows VPN-klient

1. Anslut till ditt VNet genom att gå till VPN-anslutningarna på klientdatorn och leta upp den VPN-anslutning som du skapade. Den har samma namn som ditt virtuella nätverk. Klicka på **Anslut**. Ett popup-meddelande med information om certifikatanvändningen kanske visas. Klicka på **Fortsätt** för att använda utökade privilegier.

2. På statussidan **Anslutning** klickar du på **Anslut** för att initiera anslutningen. Om du ser skärmen **Välj certifikat** kontrollerar du att klientcertifikatet som visas är det som du vill använda för att ansluta. Om det inte är det använder du pilen i listrutan för att välja rätt certifikat. Klicka sedan på **OK**.

  ![VPN-klient ansluter till Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. Anslutningen upprättas.

  ![Anslutning upprättad](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

#### <a name="troubleshoot-windows-p2s-connections"></a>Felsöka Windows P2S-anslutningar

[!INCLUDE [verifies client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="to-connect-from-a-mac-vpn-client"></a>Så här ansluter du från en Mac VPN-klient

Från dialogrutan Nätverk letar du upp den klientprofil som du vill använda och klickar sedan på **Anslut**.

  ![Mac-anslutning](./media/vpn-gateway-howto-point-to-site-rm-ps/applyconnect.png)

## <a name="verify"></a>Så här verifierar du anslutningen

Dessa anvisningar gäller för Windows-klienter.

1. Verifiera att VPN-anslutningen är aktiv genom att öppna en upphöjd kommandotolk och köra *ipconfig/all*.
2. Granska resultaten. Observera att IP-adressen som du fick är en av adresserna i klientadresspoolen för VPN för punkt-till-plats som du angav i konfigurationen. Resultatet är ungefär som i det här exemplet:

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

## <a name="connectVM"></a>Ansluta till en virtuell dator

Dessa anvisningar gäller för Windows-klienter.

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="add"></a>Så här lägger du till eller tar bort betrodda rotcertifikat

Du kan lägga till och ta bort betrodda rotcertifikat från Azure. När du tar bort ett rotcertifikat kommer klienter som har ett certifikat som genereras från den roten inte att kunna autentisera, och därmed kommer de inte heller att kunna ansluta. Om du vill att en klient ska kunna autentisera och ansluta måste du installera ett nytt klientcertifikat som genererats från ett rotcertifikat som är betrott (uppladdat) i Azure.

### <a name="to-add-a-trusted-root-certificate"></a>Lägga till ett betrott rotcertifikat

Du kan lägga till upp till 20 betrodda CER-filer för rotcertifikat i Azure. Mer information finns i avsnittet [Ladda upp ett betrott rotcertifikat](#uploadfile) i den här artikeln.

### <a name="to-remove-a-trusted-root-certificate"></a>Så här tar du bort ett betrott rotcertifikat

1. Om du vill ta bort ett betrott rotcertifikat går du till sidan **Punkt-till-plats-konfiguration** för den virtuella nätverksgatewayen.
2. I avsnittet **Rotcertifikat** letar du upp det certifikat som du vill ta bort.
3. Klicka på ellipsen bredvid certifikatet och klicka sedan på Ta bort.

## <a name="revokeclient"></a>Så här återkallar du ett klientcertifikat

Du kan återkalla certifikat. Du kan använda listan över återkallade certifikat för att selektivt neka punkt-till-plats-anslutningar baserat på enskilda klientcertifikat. Det här skiljer sig från att ta bort ett betrott rotcertifikat. Om du tar bort CER-filen för ett betrott rotcertifikat i Azure återkallas åtkomsten för alla klientcertifikat som genererats/signerats med det återkallade rotcertifikatet. När du återkallar ett klientcertifikat, snarare än rotcertifikatet, så kan de andra certifikat som har genererats från rotcertifikatet fortfarande användas för autentisering.

Den vanligaste metoden är att använda rotcertifikatet för att hantera åtkomst på grupp- eller organisationsnivå, och att återkalla klientcertifikat för mer detaljerad åtkomstkontroll för enskilda användare.

### <a name="revoke-a-client-certificate"></a>Återkalla ett klientcertifikat

Du kan återkalla ett klientcertifikat genom att lägga till tumavtrycket i listan över återkallade certifikat.

1. Hämta klientcertifikatets tumavtryck. Mer information finns i [Gör så här: Hämta tumavtrycket för ett certifikat](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopiera informationen till en textredigerare och ta bort alla blanksteg så att strängen är i ett stycke.
3. Gå till sidan **Punkt-till-plats-konfiguration** för den virtuella nätverksgatewayen. Det här är den sida du använde när du [laddade upp ett betrott rotcertifikat](#uploadfile).
4. I avsnittet **Återkallade certifikat** anger du ett eget namn på certifikatet (det inte behöver vara certifikatets CN-namn).
5. Kopiera och klistra in tumavtryckssträngen i fältet **Tumavtryck** fält.
6. Tumavtrycket valideras och läggs automatiskt till i listan över återkallade certifikat. Ett meddelande om att listan uppdateras visas på skärmen. 
7. När uppdateringen har slutförts kan certifikatet inte längre användas för att ansluta. Klienter som försöker ansluta med det här certifikatet får ett meddelande om att certifikatet inte längre är giltigt.

## <a name="faq"></a>Vanliga frågor och svar om punkt-till-plats

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="next-steps"></a>Nästa steg
När anslutningen är klar kan du lägga till virtuella datorer till dina virtuella nätverk. Mer information finns i [Virtuella datorer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Mer information om virtuella datorer och nätverk finns i [Azure and Linux VM network overview](../virtual-machines/linux/azure-vm-network-overview.md) (Översikt över nätverk för virtuella Azure- och Linux-datorer).
