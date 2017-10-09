---
title: "aaaNetworking överväganden med en Azure Apptjänst-miljö"
description: "Beskriver hello ASE nätverkstrafik och hur tooset NSG: er och udr: er med din ASE"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ccompy
ms.openlocfilehash: d4d3000f4d4d75814b1e6d47079d967334eb1a3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="networking-considerations-for-an-app-service-environment"></a>Överväganden för nätverk för Apptjänst-miljö #

## <a name="overview"></a>Översikt ##

 Azure [Apptjänstmiljö] [ Intro] är en distribution av Azure App Service till ett undernät i ditt Azure-nätverk (VNet). Det finns två distributionstyper för en Apptjänst-miljö (ASE):

- **Externa ASE**: Visar hello ASE värd apparna på en internet-tillgänglig IP-adress. Mer information finns i [skapa en extern ASE][MakeExternalASE].
- **ILB ASE**: Visar hello ASE värd apparna på en IP-adress i ditt VNet. hello interna slutpunkten är en intern belastningsutjämnare (ILB), vilket är anledningen till den kallas en ILB ASE. Mer information finns i [skapa och använda en ILB ASE][MakeILBASE].

Det finns nu två versioner av Apptjänstmiljö: ASEv1 och ASEv2. Information om ASEv1 finns [introduktion tooApp miljö v1][ASEv1Intro]. ASEv1 kan distribueras i ett klassiskt eller Resource Manager VNet. ASEv2 kan endast distribueras till en Resource Manager-VNet.

Alla anrop från en ASE som går toohello internet lämna hello VNet via en VIP för hello ASE. hello offentliga IP-Adressen för den här VIP är sedan hello käll-IP för alla anrop från hello ASE som går toohello internet. Om hello appar i din ASE anrop tooresources i ditt virtuella nätverk eller i en VPN-anslutning, är en av hello IP-adresser i hello undernät som används av din ASE hello käll-IP. Eftersom hello ASE är inom hello VNet, kan den också komma åt resurser inom hello virtuella nätverk utan någon ytterligare konfiguration. Om hello virtuella nätverk är anslutna tooyour lokalt nätverk, har appar i din ASE också åtkomst tooresources det. Du behöver inte tooconfigure hello ASE eller en app några ytterligare.

![Externa ASE][1] 

Om du har en extern ASE är också hello offentliga VIP hello endpoint att apparna ASE matcha toofor:

* HTTP/S. 
* FTP/S. 
* Web-distribution.
* Fjärrfelsökning.

![ILB ASE][2]

Om du har en ILB ASE är hello IP-adressen för hello ILB hello slutpunkt för HTTP/S, FTP-/ S, web distribution och fjärrfelsökning.

hello normal app åtkomstportar är:

| Användning | Från | för|
|----------|---------|-------------|
|  HTTP/HTTPS  | Kan konfigureras av användaren |  80, 443 |
|  FTP/FTPS    | Kan konfigureras av användaren |  21, 990, 10001-10020 |
|  Visual Studio fjärrfelsökning  |  Kan konfigureras av användaren |  4016, 4018, 4020, 4022 |

Detta är SANT om du är på en extern ASE eller på en ILB ASE. Om du är på en extern ASE nådde dessa portar på hello offentliga VIP. Om du är på en ILB ASE nådde dessa portar på hello ILB. Om du kan låsa port 443, kan det finnas en effekt på vissa funktioner som exponeras i hello-portalen. Mer information finns i [Portal beroenden](#portaldep).

## <a name="ase-dependencies"></a>ASE beroenden ##

En ASE inkommande åtkomst beroende är:

| Användning | Från | för|
|-----|------|----|
| Hantering | App Service management-adresser | ASE undernät: 454, 455 |
|  ASE intern kommunikation | ASE undernät: alla portar | ASE undernät: alla portar
|  Tillåt Azure belastningsutjämnare inkommande | Azure belastningsutjämnare | ASE undernät: alla portar
|  Appen tilldelade IP-adresser | App som har tilldelats adresser | ASE undernät: alla portar

hello innehåller inkommande trafik kommandot och kontroll av hello ASE i tillägget toosystem övervakning. hello källa IP-adresser för den här trafiken visas i hello [ASE Management adresser] [ ASEManagement] dokumentet. hello nätverkssäkerhetskonfigurationen måste tooallow åtkomst från alla IP-adresser på port 454 och 455.

Inom hello ASE undernät som det finns många portar för intern kommunikation och de kan ändra.  Detta kräver att alla hello portar i hello ASE undernät toobe tillgänglig från hello ASE undernät. 

För hello kommunikation mellan hello Azure belastningsutjämnare och hello ASE undernät hello portar som minst är att öppna måste toobe 454 och 455 16001. Hej 16001 port används för keep alive trafik mellan hello belastningsutjämnare och hello ASE. Om du använder en ILB ASE kan du låsa trafik ned toojust hello 454, 455, 16001 portar.  Om du använder en extern ASE måste tootake till kontot hello normal app åtkomstportar.  Om du använder app som har tilldelats adresser tooopen måste den tooall portar.  När en adress tilldelas tooa viss app måste använda portar som inte är kända i förväg toosend HTTP och HTTPS-trafik toohello ASE hello belastningsutjämnaren.

Om du använder app som har tilldelats IP-adresser måste tooallow trafik från hello IP-adresser tilldelas tooyour appar toohello ASE undernät.

En ASE beror på flera externa system för utgående åtkomst. Dessa beroenden system definieras med DNS-namn och mappa inte tooa fast uppsättning IP-adresser. Därför hello ASE kräver utgående åtkomst från hello ASE undernät tooall externa IP-adresser i olika portar. En ASE har hello efter utgående beroenden:

| Användning | Från | för|
|-----|------|----|
| Azure Storage | ASE undernät | Table.Core.Windows.NET, blob.core.windows.net, queue.core.windows.net, file.core.windows.net: 80, 443, 445 (445 krävs endast för ASEv1.) |
| Azure SQL Database | ASE undernät | Database.Windows.NET: 1433, 11000 11999, 14000 14999 (Mer information finns i [SQL Database V12 portar används](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).)|
| Azure-hantering | ASE undernät | Management.Core.Windows.NET, management.azure.com: 443 
| SSL-certifikatverifiering |  ASE undernät            |  OCSP.msocsp.com, mscrl.microsoft.com, crl.microsoft.com: 443
| Azure Active Directory        | ASE undernät            |  Internet: 443
| App Service-hantering        | ASE undernät            |  Internet: 443
| Azure DNS                     | ASE undernät            |  Internet: 53
| ASE intern kommunikation    | ASE undernät: alla portar |  ASE undernät: alla portar

Om hello ASE förlorar åtkomst toothese beroenden, slutar fungera. När det händer länge tillräckligt, hello ASE har pausats.

### <a name="customer-dns"></a>Kunden DNS ###

Om hello virtuella nätverk konfigureras med en kunddefinierade DNS-server kan använda hello klienternas arbetsbelastningar. Hej ASE måste fortfarande toocommunicate med Azure DNS för hanteringsändamål. 

Om hello virtuella nätverk har konfigurerats med en kund DNS på Hej andra sidan av en VPN-anslutning, hello DNS-server måste kunna nås från hello undernät som innehåller hello ASE.

<a name="portaldep"></a>

## <a name="portal-dependencies"></a>Portalen beroenden ##

I tillägg toohello ASE funktionella beroenden finns det några extra artiklar relaterade toohello portaler. Några av hello funktionerna i hello Azure-portalen är beroende av direktåtkomst too_SCM site_. Det finns två URL: er för varje app i Azure App Service. hello första URL är tooaccess din app. hello andra URL: en är tooaccess hello SCM plats, som också kallas hello _Kudu konsolen_. Funktioner som använder hello SCM plats är:

-   Webjobs.
-   Funktioner
-   Logg för strömning
-   Kudu
-   Tillägg
-   Processutforskaren
-   Konsolen

När du använder en ILB ASE hello SCM platsen inte är tillgänglig utanför hello VNet internet. När din app finns i en ASE ILB, fungerar inte vissa funktioner från hello-portalen.  

Många av de funktioner som är beroende av hello SCM platsen finns också direkt i hello Kudu-konsolen. Du kan ansluta tooit direkt i stället för med hello-portalen. Om din app finns i en ASE ILB, kan du använda din publishing autentiseringsuppgifter toosign i. hello URL tooaccess hello SCM platsen i en app som finns i en ASE ILB har hello följande format: 

```
<appname>.scm.<domain name hello ILB ASE was created with> 
```

Om din ILB ASE är hello domännamn *contoso.net* och appnamnet på din är *testapp*, hello app har nåtts på *testapp.contoso.net*. hello SCM plats som medföljer den uppnås på *testapp.scm.contoso.net*.

### <a name="functions-and-web-jobs"></a>Funktioner och webjobs. ###

Både funktioner och webb-jobb är beroende av hello SCM plats men stöds för användning i hello-portalen, även om dina appar finns i en ASE ILB, så länge din webbläsare kan nå hello SCM plats.  Om du använder ett självsignerat certifikat med ILB-ASE måste tooenable din webbläsare tootrust som certifikat.  Har toobe i hello datorn förtroende lagra för Internet Explorer och kant som: hello certifikat.  Om du använder Chrome sedan det innebär att du har accepterat hello certifikat i hello webbläsare tidigare genom att träffa förmodligen hello scm plats direkt.  hello bästa lösningen är toouse ett kommersiella certifikat som är i hello webbläsare förtroendekedja för.  

## <a name="ase-ip-addresses"></a>ASE IP-adresser ##

En ASE har några IP-adresser toobe medveten om. De är:

- **Offentliga IP-adressen för inkommande**: används för appen trafik i en extern ASE och hantering av trafik i både en extern ASE och en ILB ASE.
- **Utgående offentliga IP-Adressen**: används som hello ”från”-IP för utgående anslutningar från hello ASE att lämna hello VNet, som inte dirigeras av en VPN-anslutning.
- **ILB IP-adress**: Om du använder en ILB ASE.
- **App-tilldelade IP-baserade SSL-adresser**: bara möjligt med en extern ASE och IP-baserade SSL har konfigurerats.

Alla IP-adresserna är synlig i en ASEv2 i hello Azure-portalen från hello ASE UI. Om du har en ILB ASE visas hello IP för hello ILB.

![IP-adresser][3]

### <a name="app-assigned-ip-addresses"></a>App-tilldelad IP-adresser ###

Med en extern ASE kan du tilldela IP-adresser tooindividual appar. Du kan göra det med en ILB ASE. Mer information om hur tooconfigure app-toohave sin egen IP-adress finns [binda en befintlig anpassad SSL-certifikat tooAzure webbappar](../../app-service-web/app-service-web-tutorial-custom-ssl.md).

När en app har sin egen IP-baserade SSL-adress, reserverar hello ASE två portar toomap toothat IP-adress. En port är för HTTP-trafik, och hello andra porten för HTTPS. De portarna som listas i hello ASE Användargränssnittet under hello IP-adresser. Trafik måste vara kan tooreach dessa portar från hello VIP eller hello appar som inte är tillgängligt. Det här kravet är viktiga tooremember när du konfigurerar Nätverkssäkerhetsgrupper (NSG: er).

## <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper ##

[Nätverkssäkerhetsgrupper] [ NSGs] ge hello möjlighet toocontrol åtkomst inom ett VNet. När du använder hello portal, det är en implicit neka-regel på hello lägsta prioritet toodeny allt. Du skapar är en Tillåt-regler.

I en ASE har du inte åtkomst toohello virtuella datorer används toohost hello ASE sig själv. De är i en Microsoft-hanterad prenumeration. Om du vill toorestrict åtkomst toohello appar på hello ASE, ange NSG: er på hello ASE undernät. Gör det måste du vara noggrann uppmärksam toohello ASE beroenden. Om du blockerar eventuella beroenden slutar hello ASE fungera.

NSG: er kan konfigureras via hello Azure portal och PowerShell. hello information här visar hello Azure-portalen. Du skapar och hanterar NSG: er i hello-portalen som en översta resurs under **nätverk**.

När hello inkommande och utgående krav beaktas, hello NSG: er ska se ut ungefär toohello NSG: er i det här exemplet. Hej VNet-adressintervallet är _192.168.250.0/16_, och hello undernätet som hello ASE _192.168.251.128/25_.

hello först två inkommande krav för hello ASE toofunction visas överst hello i hello listan i det här exemplet. De ASE hantering och Tillåt hello ASE toocommunicate med sig själv. hello andra poster alla konfigurerbara klient och kan styra åtkomst toohello ASE värd nätverksprogram. 

![Inkommande säkerhetsregler][4]

En standardregel kan hello IP-adresser i hello VNet tootalk toohello ASE undernät. En annan standardregel som aktiverar hello belastningsutjämnare, kallas även hello offentliga VIP, toocommunicate med hello ASE. standardregler för toosee hello Välj **standard regler** nästa toohello **Lägg till** ikon. Om du placerar en neka allt annat regel när hello NSG-regler visas kan du förhindra att trafik mellan hello VIP och hello ASE. tooprevent trafik i hello VNet, lägga till egna regeln tooallow inkommande. Använda en källa lika tooAzureLoadBalancer med ett mål för **alla** och ett portintervall på  **\*** . Eftersom hello NSG regeln är tillämpade toohello ASE undernät, behöver du inte toobe specifika i hello målet.

Om du har tilldelat en IP-adress tooyour app Kontrollera att du behåller hello öppna portar. Välj toosee hello portar **Apptjänstmiljö** > **IP-adresser**.  

Alla hello-objekt som visas i följande regler för utgående trafik hello behövs förutom hello sista objektet. De gör det möjligt för network access toohello ASE beroenden som noterades tidigare i den här artikeln. Om du blockerar någon av dem kan slutar din ASE fungera. hello sista objektet i listan hello gör det möjligt för din ASE toocommunicate med andra resurser i ditt VNet.

![Utgående säkerhetsregler][5]

När dina NSG: er har definierats kan du tilldela dem toohello undernät som din ASE på. Om du inte kommer ihåg hello ASE VNet eller undernät, kan du se den från hello ASE management portal. tooassign hello NSG tooyour undernät, gå toohello undernät UI och välj hello NSG.

## <a name="routes"></a>Vägar ##

Vägar blivit problematiska oftast när du konfigurerar ditt VNet med Azure ExpressRoute. Det finns tre typer av vägar i ett virtuellt nätverk:

-   Systemvägar
-   BGP-vägar
-   Användardefinierade vägar (udr: er)

BGP-vägar åsidosätta systemvägar. Udr: er åsidosätta BGP-vägar. Mer information om vägar i virtuella Azure-nätverk finns [användardefinierade vägar översikt][UDRs].

hello Azure SQL-databas att hello ASE använder toomanage hello system har en brandvägg. Den kräver kommunikation toooriginate från hello ASE offentliga VIP. Anslutningar toohello SQL-databas från hello ASE nekas om de skickas ned hello ExpressRoute-anslutning och en annan IP-adress.

Om svar tooincoming management-begäranden skickas ned hello ExpressRoute skiljer hello svarsadressen sig hello ursprungliga mål. Denna felmatchning bryter hello TCP-kommunikation.

För din ASE toowork är ditt virtuella nätverk har konfigurerats med en ExpressRoute hello enklaste sak toodo:

-   Konfigurera ExpressRoute tooadvertise _0.0.0.0/0_. Som standard den tvinga tunnlar alla utgående trafik lokalt.
-   Skapa en UDR. Tillämpa den toohello undernät som innehåller hello ASE med en adressprefixet _0.0.0.0/0_ och en nästa hopptyp av _Internet_.

Om du ändrar de här två, är inte avsedda internet trafik från hello ASE undernät hello ExpressRoute och hello ASE tvingas fungerar. 

> [!IMPORTANT]
> hello vägar som definierats i en UDR måste vara specifikt nog tootake företräde framför alla vägar annonseras av hello ExpressRoute konfiguration. hello använder föregående exempel hello bred 0.0.0.0/0-adressintervall. Den kan potentiellt av misstag åsidosättas av flödet annonser som används mer specifika adressintervall.
>
> ASEs stöds inte med ExpressRoute-konfigurationer som annonserar vägar från hello offentlig peering sökväg toohello privat peering sökväg mellan. ExpressRoute-konfigurationer med offentlig peering konfigurerats få vägannonser från Microsoft. hello annonser innehåller ett stort antal Microsoft Azure IP-adressintervall. Om hello-adressintervall mellan annonserade på hello privat peering sökväg, är alla utgående paket från hello ASES undernät kraft tunnel tooa kundens lokala nätverkets infrastruktur. Det här flödet i nätverk stöds för närvarande inte med ASEs. En lösning toothis problemet är toostop mellan reklam vägar från hello offentlig peering sökväg toohello privat peering sökväg.

toocreate UDR, Följ dessa steg:

1. Gå toohello Azure-portalen. Välj **nätverk** > **routningstabeller**.

2. Skapa en ny routingtabell i hello samma region som ditt VNet.

3. Inom din routningstabellen UI, Välj **vägar** > **Lägg till**.

4. Ange hello **nästa hopptyp** för**Internet** och hello **adressprefixet** för**0.0.0.0/0**. Välj **Spara**.

    Då visas ungefär hello följande:

    ![Funktionella vägar][6]

5. När du har skapat hello nya routningstabellen gå toohello undernät som innehåller din ASE. Välj din routningstabellen hello listan i hello-portalen. När du har sparat hello ändra bör du se hello NSG: er och vägar som anges med ditt undernät.

    ![NSG: er och vägar][7]

### <a name="deploy-into-existing-azure-virtual-networks-that-are-integrated-with-expressroute"></a>Distribuera i befintliga virtuella Azure-nätverk som är integrerade med ExpressRoute ###

toodeploy din ASE till ett virtuellt nätverk som är integrerad med ExpressRoute förkonfigurera hello undernät där du vill att hello ASE distribueras. Använda en Resource Manager-mall toodeploy den. toocreate en ASE i ett VNet har som redan ExpressRoute konfigureras:

- Skapa ett undernät toohost hello ASE.

    > [!NOTE]
    > Inget annat kan vara i hello undernät men hello ASE. Vara säker på att toochoose ett adressutrymme som gör det möjligt för framtida tillväxt. Du kan inte ändra denna inställning senare. Vi rekommenderar en storlek på `/25` med 128-adresser.

- Skapa udr: er (till exempel vägtabeller) som tidigare beskrivits och ange som i hello undernät.
- Skapa hello ASE med hjälp av en Resource Manager-mall som beskrivs i [skapar en ASE med hjälp av en Resource Manager-mall][MakeASEfromTemplate].

<!--Image references-->
[1]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow.png
[2]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow2.png
[3]: ./media/network_considerations_with_an_app_service_environment/networkase-ipaddresses.png
[4]: ./media/network_considerations_with_an_app_service_environment/networkase-inboundnsg.png
[5]: ./media/network_considerations_with_an_app_service_environment/networkase-outboundnsg.png
[6]: ./media/network_considerations_with_an_app_service_environment/networkase-udr.png
[7]: ./media/network_considerations_with_an_app_service_environment/networkase-subnet.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ASEManagement]: ./management-addresses.md
