---
title: "Ansluta en dator tooa virtuellt nätverk med punkt-till-plats och certifikatautentisering: Azure Portal | Microsoft Docs"
description: "På ett säkert sätt ansluta en dator tooyour Azure Virtual Network genom att skapa en punkt-till-plats VPN-gateway-anslutningen genom att använda certifikat. Den här artikeln gäller toohello Resource Manager-modellen och använder hello Azure-portalen."
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
ms.date: 08/10/2017
ms.author: cherylmc
ms.openlocfilehash: 1419d6b4c160140b62d656b25bd02f6af7fd6655
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-azure-portal"></a>Konfigurera en punkt-till-plats-anslutning tooa VNet med hjälp av autentisering med datorcertifikat: Azure-portalen

Den här artikeln visar hur toocreate ett VNet med en punkt-till-plats-anslutning i hello Resource Manager distribution modellen med hello Azure-portalen. Den här konfigurationen använder certifikat tooauthenticate hello ansluter klienten. Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (klassisk)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

En punkt-till-plats (P2S) VPN-gateway kan du skapa en säker anslutning tooyour virtuellt nätverk från en enskild klientdator. Punkt-till-plats VPN-anslutningar är användbara när du vill tooconnect tooyour virtuella nätverk från en annan plats, exempelvis när du hemifrån home eller en konferens. P2S VPN är också en bra lösning toouse i stället för en plats-till-plats VPN-anslutning när du har bara ett fåtal klienter som behöver tooconnect tooa VNet. 

P2S använder hello Secure Socket Tunneling Protocol (SSTP), vilket är ett SSL-baserad VPN-protokoll. En P2S VPN-anslutning upprättas genom att starta från hello-klientdator.

![Punkt-till-plats-diagram](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

Punkt-till-plats certifikat autentisering anslutningar kräver hello följande:

* En RouteBased VPN-gateway.
* hello offentliga nyckel (.cer-fil) för ett rotcertifikat som är överförda tooAzure. När hello certifikat har överförts betraktas som ett betrott certifikat och används för autentisering.
* Ett klientcertifikat som genereras från hello rotcertifikat och installeras på varje klientdator som ansluter toohello VNet. Det här certifikatet används för klientautentisering.
* Ett konfigurationspaket för VPN-klienten. hello VPN-klientpaketet konfiguration innehåller hello nödvändig information för hello klienten tooconnect toohello VNet. hello paketet konfigurerar hello befintliga VPN-klient som är inbyggda toohello Windows-operativsystem. Alla klienter som ansluter måste konfigureras med hjälp av hello konfigurationspaket.

Punkt-till-plats-anslutningar kräver inte någon VPN-enhet eller en lokal offentlig IP-adress. hello VPN-anslutningen har skapats via SSTP (Secure Socket Tunneling Protocol). Vi stöder SSTP version 1.0, 1.1 och 1.2 på hello serversidan. hello klienten avgör vilken version toouse. För Windows 8.1 och senare, använder SSTP version 1.2 som standard.

Mer information om anslutningar för punkt-till-plats finns hello [punkt-till-plats vanliga frågor och svar](#faq) hello slutet av den här artikeln.

#### <a name="example"></a>Exempelvärden

Du kan använda följande värden toocreate en testmiljö hello eller referera toothese värden toobetter förstå hello exemplen i den här artikeln:

* **Namn på virtuellt nätverk:** VNet1
* **Adressutrymme:** 192.168.0.0/16<br>I det här exemplet använder vi bara ett adressutrymme. Du kan ha fler än ett adressutrymme för ditt virtuella nätverk.
* **Namn på undernät:** FrontEnd
* **Adressintervall för undernät:** 192.168.1.0/24
* **Prenumerationen:** om du har mer än en prenumeration, kontrollera att du använder hello korrekt.
* **Resursgrupp:** TestRG
* **Plats:** Östra USA
* **GatewaySubnet:** 192.168.200.0/24<br>
* **DNS-Server:** (valfritt) IP-adressen för hello DNS-server som du vill toouse för namnmatchning.
* **Namn på virtuell nätverksgateway:** VNet1GW
* **Typ av gateway:** VPN
* **Typ av VPN:** Routningsbaserad
* **Namn på offentlig IP-adress:** VNet1GWpip
* **Anslutningstyp:** Punkt-till-plats
* **Klientadresspool:** 172.16.201.0/24<br>VPN-klienter som ansluter toohello VNet med den här punkt-till-plats-anslutningen får en IP-adress från hello klient-adresspool.

## <a name="createvnet"></a>1. Skapa ett virtuellt nätverk

Kontrollera att du har en Azure-prenumeration innan du börjar. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-p2s-vnet-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>2. Lägga till ett gatewayundernät

Innan du ansluter din virtuella nätverksgateway tooa, måste du först toocreate hello gateway-undernätet hello virtuellt nätverk toowhich du vill ha tooconnect. hello gateway-tjänster använder hello IP-adresser som anges i hello gateway-undernätet. Skapa en gateway-undernätet med CIDR-block av /28 eller minst/27 tooprovide tillräckligt med IP-adresser tooaccommodate ytterligare framtida konfigurationskrav.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-p2s-rm-portal-include.md)]

## <a name="dns"></a>3. Ange en DNS-server (valfritt)

När du har skapat ditt virtuella nätverk kan du lägga till hello IP-adressen för en DNS-server toohandle namnmatchning. hello DNS-server är valfritt för den här konfigurationen, men krävs om du vill namnmatchning. Ingen ny DNS-server skapas när du anger ett värde. hello ska DNS-serverns IP-adress som du anger vara en DNS-server som kan lösa hello namn för hello-resurser som du ansluter till. Vi använde en privat IP-adress för det här exemplet, men det är troligt att detta inte är hello IP-adressen för DNS-servern. Vara säker på att toouse egna värden.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>4. Skapa en virtuell nätverksgateway

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

## <a name="generatecert"></a>5. Generera certifikat

Certifikat som används av Azure tooauthenticate klienter som ansluter tooa VNet via en punkt-till-plats VPN-anslutning. När du hämtar ett rotcertifikat du [överför](#uploadfile) hello offentlig nyckelinformation tooAzure. hello rotcertifikat anses 'betrodda' i Azure för anslutningen P2S toohello virtuella nätverket. Du också generera klientcertifikat från hello betrodda rotcertifikat och installera dem på varje klientdator. hello klientcertifikatet är används tooauthenticate hello klient när den upprättar en anslutning toohello VNet. 

### <a name="getcer"></a>1. Hämta hello .cer-fil för hello rotcertifikat

[!INCLUDE [root-certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>2. Generera ett klientcertifikat

[!INCLUDE [generate-client-cert](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>6. Lägg till hello klient-adresspool

hello-klientadresspool är ett intervall med privata IP-adresser som du anger. hello-klienter som ansluter via en punkt-till-plats-VPN ta emot en IP-adress från det här intervallet. Använda en privat IP-adressintervall som inte överlappar med hello lokal plats som du ansluter från eller hello virtuella nätverk som du vill tooconnect till.

1. När hello virtuella nätverkets gateway har skapats, navigera toohello **inställningar** på hello virtuellt gateway-sidan. I hello **inställningar** klickar du på **punkt-till-plats configuration** tooopen hello **punkt-till-plats-konfiguration** sidan.

  ![Sidan Punkt-till-plats](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/gatewayblade.png)
2. På hello **punkt-till-plats-konfiguration** sidan kan du ta bort hello automatiskt ifylld intervall och sedan lägga till hello privat IP-adressintervall som du vill toouse. Klicka på **spara** toovalidate och spara hello inställningar.

  ![Klientadresspool](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>7. Överför hello rot certifikatdata offentliga certifikat

När hello gateway har skapats kan överföra du hello offentlig nyckelinformation för hello root certificate tooAzure. När hello certifikatets offentliga data överförs kan Azure använda den tooauthenticate klienter som har installerat ett klientcertifikat som genereras från hello betrott rotcertifikat. Du kan överföra ytterligare betrodda certifikat upp tooa totalt 20.

1. Certifikat har lagts till på hello **punkt-till-plats configuration** sida i hello **rotcertifikat** avsnitt.  
2. Kontrollera att du har exporterat hello rotcertifikat som en Base64-kodad X.509 (.cer)-fil. Du behöver tooexport hello certifikat i det här formatet så att du kan öppna hello certifikat med en textredigerare.
3. Öppna hello certifikat med en textredigerare, till exempel Anteckningar. När du kopierar hello certifikatdata måste du kopiera hello text som en kontinuerlig rad utan vagnreturer eller radmatningstecken. Du kan behöva toomodify vyn i hello text editor too'Show symbolen/Visa alla tecken toosee hello transport returnerar och radmatningar. Kopiera endast hello efter avsnittet som en kontinuerlig rad:

  ![Certifikatdata](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Klistra in hello certifikat i hello **offentliga certifikatdata** fältet. **Namnet** hello certifikat och klicka sedan på **spara**. Du kan lägga upp too20 betrodda rotcertifikat.

  ![Certifikatuppladdning](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>8. Generera och installera hello VPN-klientpaketet konfiguration

tooconnect tooa VNet med en punkt-till-plats-VPN varje klient måste installera ett konfigurationspaket till klienten som konfigurerar hello inbyggda VPN-klienten med hello inställningar och filer som är nödvändiga tooconnect toohello virtuellt nätverk. hello VPN-klientpaketet configuration konfigurerar hello inbyggda Windows VPN-klienten, den installera inte en ny eller en annan VPN-klienten.

Du kan använda samma VPN-klientkonfiguration paketet på varje klientdator hello så länge hello versionen matchar hello arkitektur för hello-klienten. Hello lista av klientoperativsystem som stöds finns i hello [punkt-till-plats-anslutningar och svar](#faq) hello slutet av den här artikeln.

### <a name="step-1---generate-and-download-hello-client-configuration-package"></a>Steg 1 – generera och hämta hello klientpaketet för konfiguration

1. På hello **punkt-till-plats configuration** klickar du på **hämta VPN-klienten** tooopen hello **hämta VPN-klienten** sidan. Det tar en minut eller två för hello paketet toogenerate.

  ![Hämtning av VPN-klient 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Välj rätt hello-paket för din klient och klicka sedan på **hämta**. Spara hello konfigurationspaketet. Du kan installera hello VPN-klientpaketet konfiguration på varje klientdator som ansluter toohello virtuellt nätverk.

  ![Hämtning av VPN-klient 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

### <a name="step-2---install-hello-client-configuration-package"></a>Steg 2 – installera hello klientpaketet konfiguration

1. Kopiera hello konfigurationsfilen lokalt toohello dator som du vill tooconnect tooyour virtuellt nätverk. 
2. Dubbelklicka på hello .exe tooinstall hello Filpaketet på hello-klientdator. Eftersom du har skapat hello konfigurationspaketet är inte signerad och du kan se en varning. Om du får ett popup-fönster i Windows SmartScreen klickar du på **mer info** (på hello vänster), sedan **kör ändå** tooinstall hello paketet.
3. Installera hello paketet på hello-klientdator. Om du får ett popup-fönster i Windows SmartScreen klickar du på **mer info** (på hello vänster), sedan **kör ändå** tooinstall hello paketet.
4. Hello klientdator, navigera för**nätverksinställningar** och på **VPN**. hello VPN-anslutningen visar hello namnet på hello virtuellt nätverk som den ansluter till.

## <a name="installclientcert"></a>9. Installera ett exporterat klientcertifikat

Om du vill toocreate en P2S-anslutning från en klientdator än hello du används klientcertifikat för toogenerate hello måste du tooinstall ett klientcertifikat. När du installerar ett klientcertifikat, måste hello lösenordet som skapades när hello klientcertifikat exporterades. Vanligtvis är det bara gäller att om du dubbelklickar på hello certifikat och installera den.

Kontrollera att hello Klientcertifikatet har exporterats som en .pfx tillsammans med hello hela certifikatkedjan (som standard hello). Annars hello rot certifikatinformationen inte finns på hello klientdator och hello-klienten inte kan tooauthenticate korrekt. Mer information finns i [Installera ett exporterat klientcertifikat](vpn-gateway-certificates-point-to-site.md#install).

## <a name="connect"></a>10. Ansluta tooAzure

1. tooconnect tooyour VNet, hello klientdator, navigera tooVPN anslutningar och leta upp hello VPN-anslutning som du skapade. Det heter hello samma namn som det virtuella nätverket. Klicka på **Anslut**. Ett popup-meddelande kan visas som refererar toousing hello certifikat. Klicka på **Fortsätt** toouse utökade behörigheter.

2. På hello **anslutning** statussidan, klickar du på **Anslut** toostart hello anslutning. Om du ser en **Välj certifikat** skärmen och kontrollera att hello klienten certifikatet visar är hello något som du vill toouse tooconnect. Om det inte använda hello listrutepilen tooselect hello rätt certifikat och klicka sedan på **OK**.

  ![VPN-klienten ansluter tooAzure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. Anslutningen upprättas.

  ![Anslutning upprättad](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Felsöka P2S-anslutningar

[!INCLUDE [verifies client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify"></a>11. Verifiera din anslutning

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

## <a name="add"></a>Lägg till eller ta bort betrodda rotcertifikat

Du kan lägga till och ta bort betrodda rotcertifikat från Azure. När du tar bort ett rotcertifikat klienter som har ett certifikat som genereras från samma rot kommer inte att kunna tooauthenticate och därför inte kan tooconnect. Om du vill att en klient tooauthenticate och ansluter behöver du ett nytt klientcertifikat genereras från ett rotcertifikat som är betrodd (överförda) tooAzure tooinstall.

### <a name="tooadd-a-trusted-root-certificate"></a>tooadd ett betrott rotcertifikat

Du kan lägga upp too20 betrodda rot certifikat .cer filer tooAzure. Instruktioner finns i avsnittet hello [Överför ett betrott rotcertifikat](#uploadfile) i den här artikeln.

### <a name="tooremove-a-trusted-root-certificate"></a>tooremove ett betrott rotcertifikat

1. tooremove ett betrott rotcertifikat navigera toohello **punkt-till-plats configuration** för din virtuella nätverksgateway.
2. I hello **rotcertifikat** avsnittet hello sidan hitta hello certifikat som du vill tooremove.
3. På hello knappen Nästa toohello certifikat och klicka på ”Ta bort'.

## <a name="revokeclient"></a>Återkalla ett klientcertifikat

Du kan återkalla certifikat. hello certifikat listan över återkallade certifikat kan du tooselectively neka punkt-till-plats-anslutning baserat på enskilda klientcertifikat. Det här skiljer sig från att ta bort ett betrott rotcertifikat. Om du tar bort en betrodda certifikat .cer från Azure återkallar hello åtkomst för alla klientcertifikat som genererats/signerats av hello återkallade rotcertifikat. Återkalla ett certifikat kan i stället för hello rotcertifikatet, hello andra certifikat som har genererats från hello root certificate toocontinue toobe används för autentisering.

hello vanligt är toouse hello certifikat toomanage rotåtkomst på grupp eller organisation nivåer, medan återkallade klientcertifikaten för detaljerad åtkomstkontroll på enskilda användare.

### <a name="toorevoke-a-client-certificate"></a>toorevoke ett klientcertifikat

Du kan återkalla ett klientcertifikat genom att lägga till hello tumavtrycket toohello återkallningslistan.

1. Hämta hello Klientcertifikatets tumavtryck. Mer information finns i [hur tooretrieve hello tumavtrycket för ett certifikat](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopiera hello information tooa textredigerare och ta bort alla blanksteg så att det är en kontinuerlig sträng.
3. Navigera toohello virtuell nätverksgateway **punkt-till-plats-konfiguration** sidan. Detta är hello samma sida som du använde för[Överför ett betrott rotcertifikat](#uploadfile).
4. I hello **återkallade certifikat** avsnittet, ange ett eget namn för hello-certifikatet (det inte är toobe hello Certifikatets CN-namn).
5. Kopiera och klistra in hello tumavtrycket sträng toohello **tumavtrycket** fältet.
6. hello tumavtrycket validerar och läggs till automatiskt toohello listan över återkallade certifikat. Ett meddelande som visas på hello-skärmen som hello listan uppdateras. 
7. När uppdatering har slutförts att hello certifikatet inte längre använda tooconnect. Klienter som försöker tooconnect med det här certifikatet får ett meddelande om hello certifikatet är inte längre giltig.

## <a name="faq"></a>Vanliga frågor och svar om punkt-till-plats

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Nästa steg
När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Mer information finns i [Virtuella datorer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). toounderstand mer information om nätverk och virtuella datorer, se [översikt över Azure och Linux VM](../virtual-machines/linux/azure-vm-network-overview.md).
