---
title: "Ansluta en dator tooa virtuellt nätverk med punkt-till-plats och certifikatautentisering: klassisk Azure-portalen | Microsoft Docs"
description: "På ett säkert sätt ansluta tooyour klassiska Azure-nätverk genom att skapa en punkt-till-plats VPN-gateway-anslutningen med hjälp av hello Azure-portalen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 65e14579-86cf-4d29-a6ac-547ccbd743bd
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: cherylmc
ms.openlocfilehash: 9b53ba43ee4dfb61defeec458905fb1f1b18c3a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-classic-azure-portal"></a>Konfigurera en punkt-till-plats-anslutning tooa VNet använder certifikatautentisering (klassisk): Azure-portalen

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Den här artikeln visar hur toocreate ett VNet med en punkt-till-plats-anslutning i hello klassiska modellen med hello Azure-portalen. Den här konfigurationen använder certifikat tooauthenticate hello ansluter klienten. Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (klassisk)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>

En punkt-till-plats (P2S) VPN-gateway kan du skapa en säker anslutning tooyour virtuellt nätverk från en enskild klientdator. Punkt-till-plats VPN-anslutningar är användbara när du vill tooconnect tooyour virtuella nätverk från en annan plats, exempelvis när du hemifrån home eller en konferens. P2S VPN är också en bra lösning toouse i stället för en plats-till-plats VPN-anslutning när du har bara ett fåtal klienter som behöver tooconnect tooa VNet. 

P2S använder hello Secure Socket Tunneling Protocol (SSTP), vilket är ett SSL-baserad VPN-protokoll. En P2S VPN-anslutning upprättas genom att starta från hello-klientdator.


![Punkt-till-plats-diagram](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)


Punkt-till-plats certifikat autentisering anslutningar kräver hello följande:

* En dynamisk VPN-gateway.
* hello offentliga nyckel (.cer-fil) för ett rotcertifikat som är överförda tooAzure. Detta anses vara ett betrott certifikat och används för autentisering.
* Ett certifikat skapas från hello rotcertifikat och installeras på varje klientdator som ansluter. Det här certifikatet används för klientautentisering.
* Ett konfigurationspaket för VPN-klienter måste skapas och installeras på varje klientdator som ansluter. hello klientpaketet configuration konfigurerar hello inbyggda VPN-klienten som redan är hello operativsystem med hello nödvändig information tooconnect toohello VNet.

Punkt-till-plats-anslutningar kräver inte någon VPN-enhet eller en lokal offentlig IP-adress. hello VPN-anslutningen har skapats via SSTP (Secure Socket Tunneling Protocol). Vi stöder SSTP version 1.0, 1.1 och 1.2 på hello serversidan. hello klienten avgör vilken version toouse. För Windows 8.1 och senare, använder SSTP version 1.2 som standard. 

Mer information om anslutningar för punkt-till-plats finns hello [punkt-till-plats vanliga frågor och svar](#faq) hello slutet av den här artikeln.

### <a name="example-settings"></a>Exempelinställningar

Du kan använda följande värden toocreate en testmiljö hello eller referera toothese värden toobetter förstå hello exemplen i den här artikeln:

* **Namn: VNet1**
* **Adressutrymme: 192.168.0.0/16**<br>I det här exemplet använder vi bara ett adressutrymme. Du kan ha fler än ett adressutrymme för ditt virtuella nätverk.
* **Undernätsnamn: FrontEnd**
* **Adressintervall för undernätet: 192.168.1.0/24**
* **Prenumerationen:** om du har mer än en prenumeration, kontrollera att du använder hello korrekt.
* **Resursgrupp: TestRG**
* **Plats: Östra USA**
* **Anslutningstyp: Punkt-till-plats**
* **Adressutryme för klienten: 172.16.201.0/24**. VPN-klienter som ansluter toohello VNet med den här punkt-till-plats-anslutningen tar emot en IP-adress från hello angivna poolen.
* **GatewaySubnet: 192.168.200.0/24**. hello Gateway-undernätet måste använda hello namn 'GatewaySubnet'.
* **Storlek:** väljer hello gateway SKU som du vill toouse.
* **Routningstyp: dynamisk**

## <a name="vnetvpn"></a>1. Skapa ett virtuellt nätverk och en VPN-gateway

Kontrollera att du har en Azure-prenumeration innan du börjar. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial).

### <a name="createvnet"></a>Del 1: Skapa ett virtuellt nätverk

Om du inte redan har ett virtuellt nätverk, skapa ett. Skärmbilderna anges som exempel. Vara säker på att tooreplace hello värden med dina egna. toocreate ett VNet med hjälp av hello Azure-portalen, Använd hello följande steg:

1. Från en webbläsare, navigerar du toohello [Azure-portalen](http://portal.azure.com) och vid behov, logga in med ditt Azure-konto.
2. Klicka på **Ny**. I hello **Sök hello marketplace** skriver du ”virtuella nätverk”. Leta upp **virtuellt nätverk** från hello returnerade listan och klicka på tooopen hello **virtuellt nätverk** sidan.

  ![Sidan Sök efter virtuellt nätverk](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png)
3. Hello nedre delen av hello virtuellt nätverk sida från hello **Välj en distributionsmodell** väljer **klassiska**, och klicka sedan på **skapa**.

  ![Välj distributionsmodell](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png)
4. På hello **skapa virtuellt nätverk** konfigurerar hello VNet-inställningarna. På den här sidan lägger du till ditt första adressutrymme och ett enda adressintervall för ett undernät. När du har skapat hello VNet, kan du gå tillbaka och lägga till ytterligare undernät och adressutrymmen.

  ![Sidan Skapa virtuellt nätverk](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)
5. Kontrollera att hello **prenumeration** är hello korrekt. Du kan ändra prenumerationer med hjälp av hello i listrutan.
6. Klicka på **Resursgrupp** och välj antingen en befintlig resursgrupp, eller skapa en ny genom att ange ett namn för din nya resursgrupp. Om du skapar en ny resursgrupp planerade namn hello resursgruppen enligt tooyour konfigurationsvärden. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Välj därefter hello **plats** inställningar för din VNet. hello platsen avgör var hello resurser som du distribuerar toothis VNet kommer att finnas.
8. Välj **PIN-kod toodashboard** om du vill toobe kan toofind ditt VNet enkelt på hello instrumentpanelen och klicka sedan på **skapa**.

  ![PIN-kod toodashboard](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. När du klickar på Skapa, visas en panel på instrumentpanelen som visar hello förloppet för ditt VNet. hello panelen ändringar som hello VNet skapas.

  ![Skapa en virtuell nätverksikon](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. När det virtuella nätverket har skapats kan du se **Skapad** visas under **Status** på hello nätverk sida i hello klassiska Azure-portalen.
11. Lägg till en DNS-server (valfritt). När du har skapat ditt virtuella nätverk kan du lägga till hello IP-adressen för en DNS-server för namnmatchning. hello DNS-serverns IP-adress som du anger bör vara hello-adressen för en DNS-server som kan lösa hello namn för hello resurser i ditt VNet.<br>Öppna hello inställningarna för det virtuella nätverket tooadd en DNS-server, klickar du på DNS-servrar och Lägg hello IP-adressen för hello DNS-server som du vill toouse.

### <a name="gateway"></a>Del 2: Skapa gateway-undernät och en dynamisk routningsgateway

I det här steget skapar du ett gateway-undernät och en dynamisk routningsgateway. I hello Azure portal för hello klassiska distributionsmodellen, skapa hello gateway-undernät och hello gateway kan göras via hello samma konfigurationssidor.

1. Navigera toohello virtuella nätverk som du vill toocreate en gateway i hello-portalen.
2. På sidan hello för det virtuella nätverket på hello **översikt** under hello VPN-anslutningar, klickar du på **Gateway**.

  ![Klicka på toocreate en gateway](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. På hello **ny VPN-anslutning** väljer **punkt-till-plats**.

  ![Punkt-till-plats-anslutningstyp](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. För **klientens adressutrymme**, lägga till hello IP-adressintervall. Detta är hello-intervall som hello VPN-klienter tar emot en IP-adress vid anslutning. Använd en privat IP-adressintervall som inte överlappar hello lokal plats som du ansluter från eller med hello virtuella nätverk som du vill tooconnect till. Du kan ta bort hello automatiskt ifylld intervall och sedan lägga till hello privat IP-adressintervall som du vill toouse.

  ![Adressutryme för klienten](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Välj hello **skapa gateway omedelbart** kryssrutan.

  ![Skapa gatewayen omedelbart](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png)
6. Klicka på **valfria gatewaykonfigurationen** tooopen hello **gatewaykonfigurationen** sidan.

  ![Klicka på valfri gatewaykonfiguration](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)
7. Klicka på **undernät konfigurera nödvändiga inställningar** tooadd hello **gatewayundernät**. Det är möjligt toocreate ett gatewayundernät så liten som /29, rekommenderar vi att du skapar ett större undernät som innehåller flera adresser genom att välja minst /28 eller minst/27. Detta gör att tillräckligt många adresser tooaccommodate möjliga ytterligare konfigurationer som du vill i hello framtida. När du arbetar med gateway-undernät kan du undvika att associera ett network security group (NSG) toohello gateway-undernät. Associera en säkerhet grupp toothis undernät kan orsaka din VPN-gateway toostop som fungerar som förväntat.

  ![Lägg till GatewaySubnet](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
8. Välj hello gateway **storlek**. hello storleken är hello gateway SKU för din virtuella nätverksgateway. I hello portal hello standard SKU är **grundläggande**. För mer information om gateway-SKU:er, kan du se [Om VPN-gatewayinställningar](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

  ![Gateway-storlek](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Välj hello **routning typen** för din gateway. P2S konfigurationer kräver en **Dynamisk** routningstyp. Klicka på **OK** när du har konfigurerat den här sidan.

  ![Konfigurera routningstyp](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. På hello **ny VPN-anslutning** klickar du på **OK** längst hello hello sidan toobegin skapar din virtuella nätverksgateway. En VPN-gateway kan ta upp too45 minuter toocomplete, beroende på hello gateway-sku som du väljer.

## <a name="generatecerts"></a>2. Skapa certifikat

Certifikat som används av Azure tooauthenticate VPN-klienter för plats-till-plats-VPN. Du kan överföra hello offentlig nyckelinformation för hello root certificate tooAzure. 'betrodda' anses hello offentliga nyckel. Klientcertifikat måste genereras från hello betrodda rotcertifikat och sedan installeras på varje dator i hello certifikat-aktuell användare/personliga certifikatarkiv. hello certifikat är används tooauthenticate hello klient när den upprättar en anslutning toohello VNet. 

Om du använder självsignerade certifikat, måste de skapas med specifika parametrar. Du kan skapa ett självsignerat certifikat med hjälp av hello instruktioner för [PowerShell och Windows 10](vpn-gateway-certificates-point-to-site.md), eller [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Det är viktigt att du följer hello stegen i dessa anvisningar när du arbetar med självsignerade rotcertifikat och generera klientcertifikat från hello självsignerade rotcertifikat. Annars hello-certifikat som du skapar är inte kompatibel med P2S-anslutningar och du får ett anslutningsfel.

### <a name="cer"></a>Del 1: Hämta hello offentliga nyckel (.cer) för hello rotcertifikat

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="genclientcert"></a>Del 2: Generera ett klientcertifikat

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>3. Överför hello root certificate .cer-fil

Du kan överföra hello .cer-filen (som innehåller information om hello offentliga nycklar) för en betrodd rot certifikat tooAzure efter hello gateway har skapats. Du överföra inte hello privata nyckeln för hello root certificate tooAzure. När a.cer filen har överförts kan Azure använda den tooauthenticate klienter som har installerat ett klientcertifikat som genereras från hello betrott rotcertifikat. Du kan överföra ytterligare betrodda rotcertifikat certifikatfiler - upp tooa totalt 20 – senare, om det behövs.  

1. På hello **VPN-anslutningar** avsnittet hello sidan för din VNet, klickar du på hello **klienter** grafisk tooopen hello **punkt-till-plats VPN anslutning** sidan.

  ![Klienter](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. På hello **punkt-till-plats-anslutning** klickar du på **hantera certifikat** tooopen hello **certifikat** sidan.<br>

  ![Sidan Certifikat](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. På hello **certifikat** klickar du på **överför** tooopen hello **överför certifikat** sidan.<br>

    ![Sidan Överför certifikat](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. Klicka på hello mappen grafiska toobrowse för hello .cer-fil. Välj hello-filen och klicka sedan på **OK**. Uppdatera hello sidan toosee hello upp certifikatet på hello **certifikat** sidan.

  ![Överför certifikat](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="vpnclientconfig"></a>4. Konfigurera hello-klienten

varje klient måste tooconnect tooa VNet med en punkt-till-plats-VPN, installera ett paket tooconfigure hello inbyggda Windows VPN-klienten. hello konfigurationspaket konfigurerar hello inbyggda Windows VPN-klienten med hello inställningar nödvändiga tooconnect toohello virtuellt nätverk.

Du kan använda samma VPN-klientkonfiguration paketet på varje klientdator hello så länge hello versionen matchar hello arkitektur för hello-klienten. Hello lista av klientoperativsystem som stöds finns i hello [punkt-till-plats-anslutningar och svar](#faq) hello slutet av den här artikeln.

### <a name="generateconfigpackage"></a>Del 1: Skapa och installera hello VPN-klientpaketet konfiguration

1. I hello Azure-portalen i hello **översikt** sidan för din VNet i **VPN-anslutningar**, klicka på hello klienten grafiska tooopen hello **punkt-till-plats VPN anslutning** sidan.
2. Hello överst i hello **punkt-till-plats VPN anslutning** klickar du på hello-paketet som motsvarar toohello klientoperativsystem som kommer att installeras:

  * För 64-bitarsklienter, väljer du **VPN-klient (64-bitars)**.
  * För 32-bitarsklienter, väljer du **VPN-klient (32-bitars)**.

  ![Hämta konfigurationspaketet för VPN-klienten](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. När hello paketeras genererar, hämta och installera den på din klientdator. Om du ser ett SmartScreen-popup-fönster klickar du på **Mer information** och sedan på **Kör ändå**. Du kan också spara hello paketet tooinstall på andra datorer.

### <a name="installclientcert"></a>Del 2: Installera hello-klientcertifikat

Om du vill toocreate en P2S-anslutning från en klientdator än hello du används klientcertifikat för toogenerate hello måste du tooinstall ett klientcertifikat. När du installerar ett klientcertifikat, måste hello lösenordet som skapades när hello klientcertifikat exporterades. Detta är vanligtvis bara gäller att om du dubbelklickar på hello certifikat och installera den. Mer information finns i [Installera ett exporterat klientcertifikat](vpn-gateway-certificates-point-to-site.md#install).

## <a name="connect"></a>5. Ansluta tooAzure

### <a name="connect-tooyour-vnet"></a>Ansluta tooyour VNet

1. tooconnect tooyour VNet, hello klientdator, navigera tooVPN anslutningar och leta upp hello VPN-anslutning som du skapade. Det heter hello samma namn som det virtuella nätverket. Klicka på **Anslut**. Ett popup-meddelande kan visas som refererar toousing hello certifikat. Om det händer, klickar du på **Fortsätt** toouse utökade behörigheter.
2. På hello **anslutning** statussidan, klickar du på **Anslut** toostart hello anslutning. Om du ser en **Välj certifikat** skärmen och kontrollera att hello klienten certifikatet visar är hello något som du vill toouse tooconnect. Om det inte använda hello listrutepilen tooselect hello rätt certifikat och klicka sedan på **OK**.

  ![VPN-klientanslutning](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. Anslutningen upprättas.

  ![Anslutningen upprättad](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Felsöka P2S-anslutningar

[!INCLUDE [verify-client-certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="verifyvpnconnect"></a>Kontrollera hello VPN-anslutning

1. tooverify att VPN-anslutningen är aktiv, öppna en upphöjd kommandotolk och kör *ipconfig/all*.
2. Visa hello resultat. Observera att hello IP-adress som du fått är en av hello adresser i adressintervallet för anslutning av hello punkt-till-plats som du angav när du skapade ditt VNet. hello resultaten ska vara liknande toothis exempel:

  ```
    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled
  ```

## <a name="connectVM"></a>Ansluta tooa virtuell dator

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-p2s-classic-include.md)]

## <a name="add"></a>Lägg till eller ta bort betrodda rotcertifikat

Du kan lägga till och ta bort betrodda rotcertifikat från Azure. När du tar bort ett rotcertifikat klienter som har ett certifikat som genereras från samma rot kommer inte att kunna tooauthenticate och därför inte kan tooconnect. Om du vill att en klient tooauthenticate och ansluter behöver du ett nytt klientcertifikat genereras från ett rotcertifikat som är betrodd (överförda) tooAzure tooinstall.

### <a name="addtrustedroot"></a>tooadd ett betrott rotcertifikat

Du kan lägga upp too20 betrodda rot certifikat .cer filer tooAzure. Instruktioner finns i [avsnitt 3 - .cer rotcertifikatfilen för överför hello](#upload).

### <a name="removetrustedroot"></a>tooremove ett betrott rotcertifikat

1. På hello **VPN-anslutningar** avsnittet hello sidan för din VNet, klickar du på hello **klienter** grafisk tooopen hello **punkt-till-plats VPN anslutning** sidan.

  ![Klienter](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. På hello **punkt-till-plats-anslutning** klickar du på **hantera certifikat** tooopen hello **certifikat** sidan.<br>

  ![Sidan Certifikat](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. På hello **certifikat** klickar du på hello knappen Nästa toohello certifikat som du vill tooremove och klickar sedan **ta bort**.

  ![Ta bort rotcertifikat](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)<br>

## <a name="revokeclient"></a>Återkalla ett klientcertifikat

Du kan återkalla certifikat. hello certifikat listan över återkallade certifikat kan du tooselectively neka punkt-till-plats-anslutning baserat på enskilda klientcertifikat. Detta skiljer sig från att ta bort ett betrott rotcertifikat. Om du tar bort en betrodda certifikat .cer från Azure återkallar hello åtkomst för alla klientcertifikat som genererats/signerats av hello återkallade rotcertifikat. Återkalla ett certifikat kan i stället för hello rotcertifikatet, hello andra certifikat som har genererats från hello root certificate toocontinue toobe används för autentisering för hello punkt-till-plats-anslutning.

hello vanligt är toouse hello certifikat toomanage rotåtkomst på grupp eller organisation nivåer, medan återkallade klientcertifikaten för detaljerad åtkomstkontroll på enskilda användare.

### <a name="revokeclientcert"></a>toorevoke ett klientcertifikat

Du kan återkalla ett klientcertifikat genom att lägga till hello tumavtrycket toohello återkallningslistan.

1. Hämta hello Klientcertifikatets tumavtryck. Mer information finns i [så här: hämta hello tumavtrycket för ett certifikat](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopiera hello information tooa textredigerare och ta bort alla blanksteg så att det är en kontinuerlig sträng.
3. Navigera toohello **'klassiskt virtuellt nätverk name' > punkt-till-plats VPN-anslutning > certifikat** och klickar sedan på **listan över återkallade certifikat** tooopen hello återkallelsesida lista. 
4. På hello **listan över återkallade certifikat** klickar du på **+ Lägg till certifikat** tooopen hello **lista över Lägg till certifikat toorevocation** sidan.
5. På hello **lista över Lägg till certifikat toorevocation** och klistra in hello tumavtrycket för certifikatet som en kontinuerlig rad med text, utan blanksteg. Klicka på **OK** på hello hello sidans nederkant.
6. När uppdatering har slutförts att hello certifikatet inte längre använda tooconnect. Klienter som försöker tooconnect med det här certifikatet får ett meddelande om hello certifikatet är inte längre giltig.

## <a name="faq"></a>Vanliga frågor och svar om punkt-till-plats

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Nästa steg
När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Mer information finns i [Virtuella datorer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). toounderstand mer information om nätverk och virtuella datorer, se [översikt över Azure och Linux VM](../virtual-machines/linux/azure-vm-network-overview.md).
