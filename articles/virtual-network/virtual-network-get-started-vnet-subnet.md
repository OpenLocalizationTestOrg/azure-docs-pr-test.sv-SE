---
title: "aaaCreate ditt första Azure-virtuellt nätverk | Microsoft Docs"
description: "Lär dig hur toocreate ett Azure Virtual Network (VNet), ansluta två virtuella datorer (VM) toohello VNet och ansluta toohello virtuella datorer."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2016
ms.author: jdial
ms.openlocfilehash: 1981524cf706d5ebc83b1ff77735617550ff058a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-virtual-network"></a>Skapa ditt första virtuella nätverk

Lär dig hur toocreate ett virtuellt nätverk (VNet) med två undernät, skapa två virtuella datorer (VM) och ansluta varje VM-tooone av hello undernät, som visas i följande bild hello:

![Diagram över virtuellt nätverk](./media/virtual-network-get-started-vnet-subnet/vnet-diagram.png)

Azure-nätverk (VNet) är en representation av ditt eget nätverk i hello molnet. Du kan själv styra dina Azure-nätverksinställningar och definiera DHCP-adressblock, DNS-inställningar, säkerhetsprinciper och routning. Mer om VNet begrepp läsa hello toolearn [översikt över virtuella nätverk](virtual-networks-overview.md) artikel. Utför följande steg toocreate hello resurser hello bilden hello:

1. [skapa ett VNet med två undernät](#create-vnet)
2. [Skapa två virtuella datorer, var och en med ett nätverksgränssnitt (NIC)](#create-vms), och koppla en network security group (NSG) tooeach NIC
3. [Ansluta tooand från hello virtuella datorer](#connect-to-from-vms)
4. [ta bort alla resurser](#delete-resources). Du betalar avgifter för några av hello resurser som skapades i den här övningen när de har etablerats. toominimize hello kostnader, när du har slutfört hello övningen bör toocomplete hello stegen i det här avsnittet toodelete hello-resurser som du skapar.

Du har en grundläggande förståelse för hur du kan använda ett virtuellt nätverk när du slutför hello stegen i den här artikeln. Nästa steg tillhandahålls så att du kan lära dig mer om hur toouse Vnet på en djupare nivå.

## <a name="create-vnet"></a>Skapa ett virtuellt nätverk med två undernät

toocreate ett virtuellt nätverk med två undernät, fullständig hello steg som följer. Olika undernät är vanligtvis används toocontrol hello trafikflödet mellan undernät.

1. Logga in toohello [Azure-portalen](<https://portal.azure.com>). Om du inte redan har ett konto kan du [registrera dig för en kostnadsfri utvärderingsmånad](https://azure.microsoft.com/free). 
2. I hello **Favoriter** hello portalen klickar du på **ny**.
3. I hello **ny** bladet, klickar du på **nätverk**. I hello **nätverk** bladet, klickar du på **för virtuella nätverk**som visas i följande bild hello:

    ![Diagram över virtuellt nätverk](./media/virtual-network-get-started-vnet-subnet/virtual-network.png)

4.  I hello **för virtuella nätverk** bladet lämna *Resource Manager* markerad som hello distributionsmodell och klicka på **skapa**.
5.  I hello **skapa virtuellt nätverk-bladet** som visas, ange följande värden hello och klicka sedan på **skapa**:

    |**Inställning**|**Värde**|**Detaljer**|
    |---|---|---|
    |**Namn**|*MyVNet*|hello-namnet måste vara unikt inom hello resursgrupp.|
    |**Adressutrymme**|*10.0.0.0/16*|Du kan ange valfritt adressutrymme med CIDR-notation.|
    |**Namn på undernät**|*Klient*|Hej undernätsnamn måste vara unika inom hello virtuellt nätverk.|
    |**Adressintervall för undernätet**|*10.0.0.0/24*| hello-intervall som du anger måste finnas i hello-adressutrymmet som definierats för hello virtuellt nätverk.|
    |**Prenumeration**|*[din prenumeration]*|Välj en prenumeration toocreate hello VNet i. Ett VNet ligger i en enskild prenumeration.|
    |**Resursgrupp**|**Skapa ny:** *MyRG*|Skapa en resursgrupp. hello resursgruppens namn måste vara unikt inom hello-prenumeration som du har valt. Mer om resursgrupper läsa hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) översiktsartikel.|
    |**Plats**|*USA, västra*| Vanligtvis är hello plats som ligger närmast tooyour fysiska markerad.|

    Hej VNet tar några sekunder toocreate. När den har skapats kan du se hello Azure portalens instrumentpanel.

6. Med hello virtuellt nätverk som skapats i hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**. Klicka på hello **MyVNet** virtuellt nätverk i hello **alla resurser** bladet. Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange *MyVNet* i hello **filtrera efter namn...** rutan tooeasily åtkomst hello virtuella nätverk.
7. Hej **MyVNet** blad öppnas och visar information om hello VNet, enligt följande bild hello:

    ![Diagram över virtuellt nätverk](./media/virtual-network-get-started-vnet-subnet/myvnet.png)

8. I hello föregående bild visas klickar du på **undernät** toodisplay en lista över hello undernät i hello virtuella nätverk. hello undernät som finns är **frontend**, hello undernät som du skapade i steg 5.
9. I hello MyVNet - undernät-bladet klickar du på **+ undernät** toocreate ett undernät med hello följande information och klickar på **OK** toocreate hello undernät:

    |**Inställning**|**Värde**|**Detaljer**|
    |---|---|---|
    |**Namn**|*Server*|hello namn måste vara unika inom hello virtuellt nätverk.|
    |**Adressintervall**|*10.0.1.0/24*|hello-intervall som du anger måste finnas i hello-adressutrymmet som definierats för hello virtuellt nätverk.|
    |**Nätverkssäkerhetsgrupp** och **Routningstabell**|*Ingen* (standardvärde)|Vi går igenom nätverkssäkerhetsgrupper (NSG:er) senare i den här artikeln. Mer om användardefinierade vägar, läsa hello toolearn [användardefinierade vägar](virtual-networks-udr-overview.md) artikel.|

10. När hello Nytt undernät läggs toohello VNet, kan du stänga hello **MyVNet – undernät** bladet och Stäng hello **alla resurser** bladet.

## <a name="create-vms"></a>Skapa virtuella datorer

Du kan skapa hello virtuella datorer med hello VNet och undernät som har skapats. I den här övningen både virtuella datorer som kör hello operativsystemet Windows Server, men de kan köra alla operativsystem som stöds av Azure, inklusive flera olika Linux-distributioner.

### <a name="create-web-server-vm"></a>Skapa hello webbserver VM

toocreate hello webbservern VM, fullständig hello följande steg:

1. Klicka på i hello Azure portal Favoriter **ny**, **Compute**, sedan **Windows Server 2016 Datacenter**.
2. I hello **Windows Server 2016 Datacenter** bladet, klickar du på **skapa**.
3. I hello **grunderna** bladet som visas, ange eller välj hello följande värden och klicka på **OK**:

    |**Inställning**| **Värde**|**Detaljer**|
    |---|---|---|
    |**Namn**|*MyWebServer*|Den här virtuella datorn fungerar som en webbserver som internetresurser ansluter till.|
    |**Typ av virtuell datordisk**|*SSD*|
    |**Användarnamn**|*Ditt val*|
    |**Lösenord och Bekräfta lösenord**|*Ditt val*|
    | **Prenumeration**|*<Your subscription>*|hello prenumeration måste vara samma prenumeration som du valde i steg 5 i hello hello [skapa ett virtuellt nätverk med två undernät](#create-vnet) i den här artikeln. Hej VNet som du ansluter en VM-toomust finns i hello samma prenumeration som hello VM.|
    |**Resursgrupp**|**Använd befintlig:** Välj *MyRG*|Även om vi använder samma resursgrupp som vi gjorde hello VNet, hello hello resurser inte har tooexist i hello samma resursgrupp.|
    |**Plats**|*USA, västra*|hello måste vara samma plats som du angav i steg 5 i hello hello [skapa ett virtuellt nätverk med två undernät](#create-vnet) i den här artikeln. Virtuella datorer och hello Vnet som de ansluter toomust finns i hello samma plats.|

4. I hello **välja en storlek** bladet, klickar du på *DS1_V2 Standard*, klicka på **Välj**. Läs hello [Windows VM-storlekar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel för en lista över alla Windows VM-storlekar som stöds av Azure.
5. I hello **inställningar** bladet ange eller välj hello följande värden och klicka på **OK**:

    |**Inställning**|**Värde**|**Detaljer**|
    |---|---|---|
    |**Lagring: Använd hanterade diskar**|*Ja*||
    |**Virtuellt nätverk**| Välj *MyVNet*|Du kan välja alla virtuella nätverk som finns i hello samma plats som hello VM som du skapar. Mer om Vnet och undernät, läsa hello toolearn [för virtuella nätverk](virtual-networks-overview.md) artikel.|
    |**Undernät**|Välj *Klient*|Du kan välja alla undernät som finns inom hello virtuella nätverk.|
    |**Offentlig IP-adress**|Acceptera standardvärdet för hello|En offentlig IP-adress kan du tooconnect toohello VM från hello Internet. Mer om den offentliga IP-adresser, läsa hello toolearn [IP-adresser](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) artikel.|
    |**Nätverkssäkerhetsgrupp (brandvägg)**|Acceptera standardvärdet för hello|Klicka på hello **(nya) MyWebServer nsg** standard NSG hello portal skapas tooview dess inställningar. I hello **skapa nätverkssäkerhetsgruppen** bladet som öppnas, Observera att den har en regel för inkommande trafik som tillåter TCP/3389 (RDP)-trafik från alla käll-IP-adresser.|
    |**Alla andra värden**|Acceptera hello standardvärden|Mer om hello återstående inställningar, läsa hello toolearn [om VMs](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.|

    Nätverkssäkerhetsgrupper (NSG) kan du toocreate inkommande/utgående regler för hello typ av nätverkstrafik som kan flöda tooand från hello VM. Som standard alla inkommande trafik toohello VM nekas. Du kan lägga till ytterligare regler för inkommande trafik för TCP/80 (HTTP) och TCP/443 (HTTPS) för en webbserver i produktionsmiljö. Det finns ingen regel för utgående trafik eftersom all utgående trafik tillåts som standard. Du kan lägga till/ta bort regler toocontrol trafik per dina principer. Läs hello [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md) artikel toolearn mer om NSG: er.

6.  I hello **sammanfattning** bladet granskar hello inställningar och klickar på **OK** toocreate hello VM. Panelen status visas på portalens instrumentpanel för hello som hello VM skapar. Det kan ta några minuter toocreate. Du behöver inte toowait för den toocomplete. Du kan fortsätta toohello nästa steg när hello virtuell dator skapas.

### <a name="create-database-server-vm"></a>Skapa hello databasserver VM

toocreate hello databasserver VM, fullständig hello följande steg:

1.  Klicka på i hello Favoriter **ny**, **Compute**, sedan **Windows Server 2016 Datacenter**.
2.  I hello **Windows Server 2016 Datacenter** bladet, klickar du på **skapa**.
3.  I hello **bladet grundläggande**, ange eller välj hello följande värden och sedan på **OK**:

    |**Inställning**|**Värde**|**Detaljer**|
    |---|---|---|
    |**Namn**|*MyDBServer*|Den här virtuella datorn fungerar som en databasserver som hello webbservern ansluter till, men den hello Internet kan inte ansluta till.|
    |**Typ av virtuell datordisk**|*SSD*||
    |**Användarnamn**|Ditt val||
    |**Lösenord och Bekräfta lösenord**|Ditt val||
    |**Prenumeration**|<Your subscription>|hello prenumeration måste vara samma prenumeration som du valde i steg 5 i hello hello [skapa ett virtuellt nätverk med två undernät](#create-vnet) i den här artikeln.|
    |**Resursgrupp**|**Använd befintlig:** Välj *MyRG*|Även om vi använder samma resursgrupp som vi gjorde hello VNet, hello hello resurser inte har tooexist i hello samma resursgrupp.|
    |**Plats**|*USA, västra*|hello måste vara samma plats som du angav i steg 5 i hello hello [skapa ett virtuellt nätverk med två undernät](#create-vnet) i den här artikeln.|

4.  I hello **välja en storlek** bladet, klickar du på *DS1_V2 Standard*, klicka på **Välj**.
5.  I hello **inställningar** bladet ange eller välj hello följande värden och klicka på **OK**:

    |**Inställning**|**Värde**|**Detaljer**|
    |----|----|---|
    |**Lagring: Använd hanterade diskar**|*Ja*||
    |**Virtuellt nätverk**|Välj *MyVNet*|Du kan välja alla virtuella nätverk som finns i hello samma plats som hello VM som du skapar.|
    |**Undernät**|Välj *backend-* genom att klicka på hello **undernät** rutan, sedan välja **backend-** från hello **Välj ett undernät** bladet|Du kan välja alla undernät som finns inom hello virtuella nätverk.|
    |**Offentlig IP-adress**|Ingen – hello standardadressen Klicka **ingen** från hello **Välj offentlig IP-adress** bladet|Utan en offentlig IP-adress kan du bara ansluta toohello VM från en annan virtuell dator ansluten toohello samma virtuella nätverk. Du kan inte ansluta tooit direkt från hello Internet.|
    |**Nätverkssäkerhetsgrupp (brandvägg)**|Acceptera standardvärdet för hello| Som hello standard NSG skapas för hello MyWebServer VM denna NSG också har hello standard samma inkommande regel. Du kan lägga till en ytterligare regel för inkommande trafik för TCP/1433 (MS SQL) för en databasserver. Det finns ingen regel för utgående trafik eftersom all utgående trafik tillåts som standard. Du kan lägga till/ta bort regler toocontrol trafik per dina principer.|
    |**Alla andra värden**|Acceptera hello standardvärden||

6.  I hello **sammanfattning** bladet granskar hello inställningar och klickar på **OK** toocreate hello VM. Panelen status visas på portalens instrumentpanel för hello som hello VM skapar. Det kan ta några minuter toocreate. Du behöver inte toowait för den toocomplete. Du kan fortsätta toohello nästa steg när hello virtuell dator skapas.

## <a name="review"></a>Granska resurser

Om du har skapat en VNet och två virtuella datorer, hello Azure-portalen skapas flera ytterligare resurser för dig i hello MyRG resursgruppen. Granska hello innehållet i hello MyRG resursgruppen genom att slutföra hello följande steg:

1. I hello **Favoriter** rutan klickar du på **fler tjänster**.
2. I hello **fler tjänster** rutan, skriver *resursgrupper* i hello som har hello word *Filter* i den. Klicka på **resursgrupper** när du ser i hello filtrerad lista.
3. I hello **resursgrupper** rutan klickar du på hello *MyRG* resursgruppen. Du kan ange om du har många befintliga resursgrupper i din prenumeration, *MyRG* i hello som innehåller hello text *filtrera efter namn...* tooquickly resursgrupp åtkomst hello MyRG.
4.  I hello **MyRG** bladet visas som hello resursgruppen innehåller 12 resurser som visas i följande bild hello:

    ![Innehåll i resursgruppen](./media/virtual-network-get-started-vnet-subnet/resource-group-contents.png)

Mer om virtuella datorer, diskar och storage-konton kan läsa hello toolearn [virtuella](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Disk](../virtual-machines/windows/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-network%2ftoc.json), och [lagringskonto](../storage/common/storage-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) översikt över artiklar. Du kan se hello två standard NSG: er hello portal skapas automatiskt. Du kan också se den hello portalen skapas två gränssnitt (NIC)-nätverksresurser. Ett nätverkskort aktiverar en VM tooconnect tooother resurser över hello virtuella nätverk. Läs hello [NIC](virtual-network-network-interface.md) artikel toolearn mer om nätverkskort. hello portal skapas också en offentlig IP-adressresurs. Offentliga IP-adresser är en inställning för en offentlig IP-adressresurs. Mer om den offentliga IP-adresser, läsa hello toolearn [IP-adresser](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) artikel.

## <a name="connect-to-from-vms"></a>Ansluta toohello virtuella datorer

Du kan nu ansluta toohello virtuella datorer genom att slutföra hello stegen i följande avsnitt hello med ditt VNet och två virtuella datorer skapas:

### <a name="connect-from-internet"></a>Ansluta toohello webbservern VM från hello Internet

tooconnect toohello webbservern VM från hello Internet, fullständig hello följande steg:

1. Hello-portalen, öppna hello MyRG resursgruppen genom att slutföra hello stegen i hello [granska resurser](#review) i den här artikeln.
2. I hello **MyRG** bladet, klickar du på hello **MyWebServer** VM.
3. I hello **MyWebServer** bladet, klickar du på **Anslut**som visas i följande bild hello:

    ![Anslut tooweb server VM](./media/virtual-network-get-started-vnet-subnet/webserver.png)

4. Tillåt din webbläsare toodownload hello *MyWebServer.rdp* filen och sedan öppna den.
5. Om du får en dialogrutan visas som informerar du hello utgivaren av hello anslutning inte kan verifieras, klickar du på **Anslut**.
6. När du anger dina autentiseringsuppgifter, se till att logga in med hello användarnamn och lösenord som du angav i steg 3 i hello [skapa hello webbservern VM](#create-web-server-vm) i den här artikeln. Om hello **Windows-säkerhet** visas som visas inte i listan hello korrekt autentiseringsuppgifter kan du behöva tooclick **fler alternativ**, sedan **Använd ett annat konto**, så att du kan Ange hello rätt användarnamn och lösenord). Klicka på **OK** tooconnect toohello VM.
7. Om du får en **anslutning till fjärrskrivbord** rutan informerar du som hello identitet hello fjärrdatorn inte kan verifieras klickar du på **Ja**.
8. Du är nu ansluten toohello MyWebServer VM från hello Internet. Lämna hello fjärrskrivbordsanslutning öppna toocomplete hello stegen i nästa avsnitt om hello.

anslutning till hello är toohello offentlig IP-adress som tilldelats toohello offentliga IP-adress resurs hello portal skapade i steg 5 i hello [skapa ett virtuellt nätverk med två undernät](#create-vnet) i den här artikeln. hello anslutningen tillåts eftersom hello Standardregeln som skapats i hello **MyWebServer nsg** NSG tillåts TCP/3389 (RDP) inkommande toohello VM från alla käll-IP-adresser. Om du försöker tooconnect toohello VM via någon annan port hello anslutningen misslyckas, såvida inte du lägger till ytterligare regler för inkommande trafik toohello NSG vilket gör att hello ytterligare portar.

>[!NOTE]
>Om du lägger till ytterligare regler för inkommande trafik toohello NSG, kontrollera att hello samma portar är öppna på hello Windows-brandväggen eller hello misslyckas anslutningen.
>

### <a name="connect-to-internet"></a>Ansluta toohello Internet från hello webbserver VM

tooconnect utgående toohello Internet från hello webbserver VM, fullständig hello följande steg:

1. Om du inte redan har en anslutning till toohello MyWebServerVM öppna, gör en anslutning till toohello VM genom att slutföra hello stegen i hello [Anslut toohello webbservern VM från hello Internet](#connect-from-internet) i den här artikeln.
2. Öppna Internet Explorer hello Windows-skrivbordet. I hello **installationsprogrammet Internet Explorer 11** dialogrutan klickar du på **inte använda rekommenderade inställningar**, klicka på **OK**. Är det rekommenderade tooaccept hello rekommenderade inställningar för en produktionsserver.
3. Ange i hello adressfältet i Internet Explorer, [bing.com](http:www.bing.com). Om du får en dialogruta för Internet Explorer klickar du på **Lägg till**, sedan **Lägg till** i hello **tillförlitliga platser** dialogrutan och klicka på **Stäng**. Upprepa den här processen i eventuella andra dialogrutor i Internet Explorer.
4. Sök på hello Bing sida, ange *whatsmyipaddress*, klicka sedan på hello förstoringsglas. Bing returnerar hello offentliga IP-adress som tilldelats toohello offentliga IP-adressresurs skapas av hello portalen när du skapar hello VM. Om du undersöker hello inställningar för hello **MyWebServer ip** resurs måste du se hello samma IP-adress som tilldelats toohello offentliga IP-adressresurs som hello bilden nedan. hello IP-adress som tilldelats tooyour VM skiljer sig dock.

    ![Anslut tooweb server VM](./media/virtual-network-get-started-vnet-subnet/webserver-pip.png)

5.  Lämna hello fjärrskrivbordsanslutning öppna toocomplete hello stegen i nästa avsnitt om hello.

Du är kan tooconnect toohello Internet från hello VM eftersom alla utgående anslutning från hello VM tillåts som standard. Du kan begränsa utgående anslutning genom att lägga till ytterligare regler toohello NSG tillämpas toohello NIC, toohello undernät hello nätverkskort är ansluten till, eller båda.

Om hello VM placeras i hello stoppats (frigjorts) tillstånd med hjälp av hello portal, hello offentlig IP-adress kan ändras. Om du kräver att hello offentliga IP-adress aldrig ändras, kan du använda hello statisk tilldelningsmetod för hello IP-adress i stället för hello dynamisk fördelning (som standard hello). Mer om toolearn hello skillnaderna mellan allokeringsmetoder, läsa hello [IP-adressen typer och fördelningsmetoder](virtual-network-ip-addresses-overview-arm.md) artikel.

### <a name="webserver-to-dbserver"></a>Ansluta toohello databasserver VM från hello webbserver VM

tooconnect toohello databasserver VM från hello webbserver VM, fullständig hello följande steg:

1. Om du inte redan har en anslutning till toohello MyWebServer VM öppna, gör en anslutning till toohello VM genom att slutföra hello stegen i hello [Anslut toohello webbservern VM från hello Internet](#connect-from-internet) i den här artikeln.
2. Klicka på hello Start-knappen i hello nedre vänstra hörnet på skrivbordet för Windows hello och börja skriva *fjärrskrivbord*. När hello Start-menyn i listan visas **anslutning till fjärrskrivbord**, klickar du på den.
3. I hello **anslutning till fjärrskrivbord** dialogrutan Ange *MyDBServer* för hello datornamn och klicka på **Anslut**.
4. Ange hello användarnamn och lösenord du angav i steg 3 i hello [skapa hello databasserver VM](#create-database-server-vm) avsnitt i den här artikeln, klicka sedan på **OK**.
5. Om du får en dialogrutan visas som informerar du hello identitet hello fjärrdatorn inte kan verifieras, klickar du på **Ja**.
6. Lämna hello fjärrskrivbordsanslutning tooboth servrar öppna toocomplete hello stegen i hello nästa avsnitt.

Du är kan toomake hello anslutning toohello databasservern VM från hello webbservern VM för hello följande orsaker:

- TCP/3389 inkommande anslutningar har aktiverats för alla käll-IP i hello standard NSG skapade i steg 5 i hello [skapa hello databasserver VM](#create-database-server-vm) i den här artikeln.
- Du har initierat hello-anslutningen från hello webbserver VM, som är anslutna toohello samma virtuella nätverk som databasserver för hello VM. tooconnect tooa VM som inte har en offentlig IP-adress som tilldelats tooit, måste du ansluta från en annan virtuell dator ansluten toohello samma virtuella nätverk, även om hello VM är anslutna tooa olika undernät.
- Även om hello virtuella datorer är anslutna toodifferent undernät, skapar Azure standardvägar som gör anslutningen mellan undernät. Du kan åsidosätta hello standardvägar genom att skapa egna men. Läs hello [användardefinierade vägar](virtual-networks-udr-overview.md) artikel toolearn mer om routning i Azure.

Om du försöker tooinitiate en anslutning till toohello databasserver VM från hello Internet, som du gjorde i hello [Anslut toohello webbservern VM från hello Internet](#connect-from-internet) avsnitt i den här artikeln du ser att hello **Anslut** alternativet är nedtonad. Ansluta är nedtonad eftersom det inte finns några offentliga IP-adress som tilldelats toohello VM, inkommande anslutningar tooit från hello Internet inte är möjligt.

### <a name="connect-toohello-internet-from-hello-database-server-vm"></a>Ansluta toohello Internet från hello databasserver VM

Anslut utgående toohello Internet från hello databasserver VM genom att slutföra hello följande steg:

1. Om du inte redan har en anslutning till toohello MyDBServer VM öppna från hello MyWebServer VM, fullständig hello stegen i hello [Anslut toohello databasservern VM från hello webbservern VM](#webserver-to-dbserver) i den här artikeln.
2. Öppna Internet Explorer från hello Windows-skrivbordet på hello MyDBServer VM och svara toohello dialogrutor som du gjorde i steg 2 och 3 av hello [ansluta toohello Internet från hello webbserver VM](#connect-to-internet) i den här artikeln.
3. Ange i adressfältet hello [bing.com](http:www.bing.com).
4. Klicka på **Lägg till** hello Internet Explorer i dialogrutan som visas sedan **Lägg till**, sedan **Stäng** i hello **betrodda** dialogrutan platser. Gör på samma sätt i eventuella ytterligare dialogrutor.
5. Sök på hello Bing sida, ange *whatsmyipaddress*, klicka sedan på hello förstoringsglas. Bing returnerar hello offentliga IP-adress som är tilldelad toohello VM hello Azure-infrastrukturen. 6. Stäng hello remote desktop toohello MyDBServer VM från hello MyWebServer VM och sedan stänga hello fjärranslutning toohello MyWebServer VM.

hello utgående anslutning toohello Internet tillåts eftersom all utgående trafik tillåts som standard, även om en offentlig IP-adressresurs inte är tilldelad toohello MyDBServer VM. Alla virtuella datorer som standard är kan tooconnect utgående toohello Internet, med eller utan en offentlig IP-adress resurs som är tilldelad toohello VM. Du är inte kan tooconnect toohello offentliga IP-adressen från hello Internet men som du skulle kunna toofor hello MyWebServer VM som har en offentlig IP-adress resurs som är tilldelad.

## <a name="delete-resources"></a>Ta bort alla resurser

toodelete alla resurser skapas i den här artikeln, fullständig hello följande steg:

1. tooview hello MyRG resursgruppen skapas i den här artikeln har slutförts steg 1-3 i hello [granska resurser](#review) i den här artikeln. Granska igen hello resurser i hello resursgrupp. Om du har skapat hello MyRG resursgrupp per föregående steg finns hello 12 resurser hello bilden i steg 4.
2. Hej MyRG-bladet, klickar du på hello **ta bort** knappen.
3. hello portalen måste du tootype hello namnet på hello resurs grupp tooconfirm som du vill toodelete den. Om du ser resurser än hello resurserna som visas i steg 4 i hello [granska resurser](#review) avsnitt i den här artikeln klickar du på **Avbryt**. Om bara hello 12 resurser som skapats som en del av den här artikeln, anger du *MyRG* hello resursgruppens namn, sedan klickar du på **ta bort**. Tar bort en resursgrupp alla resurser inom hello resursgrupp, så alltid att tooconfirm hello innehållet i en resursgrupp innan den tas bort. hello portal tar bort alla resurser som ingår i hello resursgrupp och sedan tar bort hello resursgruppen sig själv. Den här processen tar flera minuter.

## <a name="next-steps"></a>Nästa steg

I den här övningen har du skapat ett VNet och två virtuella datorer. Du angav anpassade inställningar när du skapade de virtuella datorerna och godkände flera standardinställningar. Vi rekommenderar att du läser hello följande artiklar, innan du distribuerar produktion Vnet och virtuella datorer, tooensure som du känner till alla tillgängliga inställningar:

- [Virtuella nätverk](virtual-networks-overview.md)
- [Offentliga IP-adresser](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- [Nätverksgränssnitt](virtual-network-network-interface.md)
- [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md)
- [Virtuella datorer](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
