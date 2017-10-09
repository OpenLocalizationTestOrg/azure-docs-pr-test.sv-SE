---
title: aaaInfrastructure och anslutningen tooSAP HANA i Azure (stora instanser) | Microsoft Docs
description: "Konfigurera kräver anslutning infrastruktur toouse SAP HANA i Azure (stora instanser)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0af34fbd82413bf63981036a76eaa264d8299ec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-large-instances-infrastructure-and-connectivity-on-azure"></a>Infrastruktur för SAP HANA (stora instanser) och anslutningar på Azure 

Vissa definitioner gång innan du läser den här guiden. I [SAP HANA (stora instanser) översikt och arkitektur för Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) vi har fört två olika klasser av HANA stora instans enheter med:

- S72, S72m, S144, S144m, S192 och S192m som vi refererar tooas hello 'Type I klassen' av SKU: er.
- S384, S384m, S384xm, S576, S768 och S960 som vi refererar tooas hello typ II klass av SKU: er.

hello klassen Specificerare är pågående toobe används i hela hello HANA stora instans dokumentationen tooeventually finns toodifferent funktioner och krav utifrån HANA stora instans SKU: er.

Övriga definitioner som vi använder ofta är:
- **Stora instans stämpel:** maskinvara infrastruktur-stacken, som är SAP HANA TDI certifierad och dedikerade toorun SAP HANA-instanser i Azure.
- **SAP HANA i Azure (stora instanser):** officiellt namn för hello erbjudande på Azure toorun HANA instanser i på SAP HANA TDI certifierad maskinvara som distribueras i stora instans tidsstämplar i olika Azure-regioner. hello relaterade termen **HANA stora instans** kort för SAP HANA i Azure (stora instanser) och är ofta används den här tekniska distributionsguiden.
 

När hello inköp av SAP HANA i Azure (stora instanser) är slutförd mellan dig och hello Microsoft enterprise-kontoteamet krävs hello följande information av Microsoft toodeploy HANA stora instans enheter:

- Kundens namn
- Kontaktinformation (inklusive e-postadress och telefonnummer)
- Tekniska kontaktinformation (inklusive e-postadress och phone tal)
- Tekniska nätverk kontaktinformation (inklusive e-postadress och phone tal)
- Distribution av Azure-region (västra USA, östra USA, östra, Australien sydost, västra Europa och Norra Europa från och med juli 
- 2017)
- Bekräfta SAP HANA på Azure (stora instanser) SKU (konfiguration)
- Redan detaljerad i hello översikt och dokument arkitekturen för stora HANA-instanser för varje Azure-Region som distribueras på:
    - En /29 IP-adressintervall för ER P2P-anslutningar som ansluter Azure Vnet tooHANA stora instanser
    - Ett/24 CIDR-Block som används för hello HANA stora instanser Server IP-Pool
- hello IP-adress intervallvärden används i hello VNet-adressutrymmet attribut för varje virtuellt Azure-nätverk som ansluter toohello HANA stora instanser
- Data för varje HANA stora instanser system:
  - Önskade värdnamnet - helst med fullständigt kvalificerade domännamnet.
  - Önskad IP-adress för hello HANA stora instans enhet utanför hello Server IP-adresspoolintervall - Kom ihåg att hello 30 första IP-adresser i hello Serverpoolen i IP-adressintervallet är reserverade för intern användning i HANA stora instanser
  - SAP HANA SID namn hello SAP HANA-instansen (obligatorisk toocreate hello nödvändiga diskutrymmet för SAP HANA-relaterade volymer). hello HANA SID som krävs för att skapa hello behörigheter för <sidadm> på hello NFS volymer som får bifogade toohello HANA stora instans enhet. Det är även används som en hello namn komponent i hello diskvolymer som hämta monterade. Om du vill toorun mer än en HANA instans på hello enhet, måste du toolist flera HANA SID. Var och en hämtar en separat uppsättning volymer som har tilldelats.
  - hello groupid hello hana sidadm användaren har i hello Linux OS är obligatoriska toocreate hello nödvändiga SAP HANA-relaterade diskvolymer. hello SAP HANA-installationen skapar vanligtvis hello sapsys gruppen med grupp-id 1001. hello hana sidadm användaren tillhör gruppen
  - hello userid hello hana sidadm användaren har i hello Linux OS är obligatoriska toocreate hello nödvändiga SAP HANA-relaterade diskvolymer. Om du kör flera instanser av HANA på hello-enhet behöver du toolist alla hello <sid>adm-användare 
- Azure prenumerations-ID för hello Azure-prenumeration toowhich SAP HANA på Azure HANA stora instanser kommer toobe anslutna direkt. Prenumerations-ID refererar till hello Azure-prenumeration som toobe debiteras med hello HANA stora instans enhet(er).

När du har angett hello information Microsoft tillhandahåller SAP HANA i Azure (stora instanser) och returnerar hello information behövs toolink ditt Azure Vnet tooHANA stora instanser och tooaccess hello HANA stora instans enheter.

## <a name="connecting-azure-vms-toohana-large-instances"></a>Ansluta virtuella datorer i Azure tooHANA stora instanser

Som nämnts i [SAP HANA (stora instanser) översikt och arkitektur för Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) hello minimal distribution av HANA stora instanser med hello SAP programnivå i Azure ser ut som:

![Azure-VNet ansluten tooSAP HANA på Azure (stora instanser) och lokalt](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Titta närmare på hello Azure VNet sida, vi är medvetna om behovet av hello:

- hello definition av ett virtuellt Azure-nätverk som är pågående toobe används toodeploy hello virtuella datorer i hello SAP programnivå i.
- Det innebär att automatiskt ett standardundernät i hello Azure Vnet har definierats som verkligen hello en används toodeploy hello virtuella datorer i.
- hello Azure VNet som skapas måste toohave minst ett VM-undernät och en ExpressRoute-Gateway-undernätet. Dessa undernät ska tilldelas hello IP-adressintervall som anges och beskrivs i följande avsnitt hello.

Därför ska vi titta lite närmare i hello Azure VNet har skapats för HANA stora instanser

### <a name="creating-hello-azure-vnet-for-hana-large-instances"></a>Skapa hello Azure VNet för HANA stora instanser

>[!Note]
>hello Azure VNet för HANA stora instansen måste skapas med hello Azure Resource Manager-distributionsmodellen. hello äldre Azure distributionsmodell, ofta kallade klassiska distributionsmodellen stöds inte med hello HANA stora instans lösning.

Hej VNet kan skapas med hello Azure-portalen, PowerShell, Azure-mall eller Azure CLI (se [skapa ett virtuellt nätverk med hello Azure-portalen](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). I följande exempel hello, titta i ett virtuellt nätverk som skapats via hello Azure-portalen.

Om vi tittar i hello definitioner av ett virtuellt Azure-nätverk via hello Azure-portalen, nu ska vi titta i vissa hello definitioner och hur de hör ihop toowhat vi lista över olika IP-adressintervall. När vi pratar om hello **adressutrymme**, vi menar hello adressutrymme som hello Azure VNet tillåts toouse. Det här adressutrymmet är också hello-adressintervall som hello VNet använder för distribution av BGP-väg. Detta **adressutrymme** här:

![Adress utrymme för Azure VNet som visas i hello Azure-portalen](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

Hello om föregående med 10.16.0.0/16, gavs hello Azure VNet en ganska stor och brett IP-adressintervallet toouse. Behöver alla hello IP-adressintervall i efterföljande undernät i detta virtuella nätverk kan intervallen inom den-adressutrymme. Vanligtvis rekommenderar inte vi sådana stora adressintervall för enkel VNet i Azure. Men vi hämta ytterligare ett steg och titta i hello undernät som definierats i hello Azure VNet:

![Azure VNet-undernät och deras IP-adressintervall](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Som du ser tittar vi på ett virtuellt nätverk för med en första VM-undernät (kallas här 'default') och ett undernät som kallas ”GatewaySubnet'.
I följande avsnitt hello, vi refererar toohello IP-adressintervall hello-undernät som anropades 'default' i hello grafik som **Azure VM-undernät IP-adressintervall**. I följande avsnitt hello, vi refererar toohello IP-adressintervall hello Gateway-undernät som **VNet Gateway-undernätet IP-adressintervall**. 

Hello fallet genom hello två bilder ovan visas i den hello **VNet-adressutrymmet** täcker båda hello **Azure VM-undernät IP-adressintervall** och hello **VNet Gateway undernät IP-adress intervallet**. 

I annat fall där du behöver tooconserve eller vara specifika med IP-adressintervall kan du begränsa hello **VNet-adressutrymmet** med ett VNet toohello specifika adressintervall som används av varje undernät. I det här fallet kan du definiera hello **VNet-adressutrymmet** av ett VNet som flera specifika intervall som visas här:

![Azure VNet-adressutrymmet med två blanksteg](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

I det här fallet hello **VNet-adressutrymmet** har två blanksteg som definierats. Dessa två blanksteg identiska toohello IP-adressintervall definieras för **Azure VM-undernät IP-adressintervall** och hello **VNet Gateway-undernätet IP-adressintervall.**

Du kan använda alla namngivningsstandarden som du vill för dessa klient undernät (VM-undernät). Dock **måste alltid vara en och bara en gateway-undernät för varje VNet** som ansluter toohello SAP HANA i Azure (stora instanser) ExpressRoute-kretsen. Och **gateway-undernätet måste alltid ha namnet ”GatewaySubnet”** tooensure rätt placering av hello ExpressRoute-gateway.

> [!WARNING] 
> Det är viktigt att hello gateway-undernätet alltid heter ”GatewaySubnet”.

Flera Virtuella undernät kan även använda icke sammanhängande adressintervall användas. Men som tidigare nämnts dessa adressintervall måste omfattas av hello **VNet-adressutrymmet** hello VNet i sammansatt form eller i en lista över hello exakt adressintervall hello Virtuella datorundernät och hello gateway-undernätet.

Sammanfatta hello viktiga fakta om ett virtuellt Azure-nätverk som ansluter tooHANA stora instanser:

- Du behöver toosubmit tooMicrosoft hello **VNet-adressutrymmet** när du utför en inledande distribution till HANA stora instanser. 
- Hej **VNet-adressutrymmet** kan vara ett större område som täcker hello-intervall för Virtuella Azure-undernät IP-adress range(s) och hello VNet Gateway-undernätet IP-adressintervall.
- Eller kan du skicka som **VNet-adressutrymmet** flera områden som täcker hello olika IP-adressintervall för VM-undernät IP-adress range(s) och hello VNet Gateway-undernätet IP-adressintervall.
- hello definierats **VNet-adressutrymmet** är använda BGP-routning spridningen.
- hello hello Gateway-undernätet måste vara: **”GatewaySubnet”.**
- Hej **VNet-adressutrymmet** används som filter i hello HANA stora instans sida tooallow eller neka trafik toohello HANA stora instans enheter från Azure. Om hello information för BGP-routning av hello Azure VNet och hello IP-adressintervall som konfigurerats för att filtrera på hello HANA stora instans på klientsidan inte matchar, kan det uppstå problem i anslutningen.
- Det finns vissa uppgifter om hello Gateway-undernät som beskrivs längre ned i avsnittet 'Ansluta ett virtuellt nätverk tooHANA stora instans ExpressRoute'



### <a name="different-ip-address-ranges-toobe-defined"></a>Olika IP-adressintervall toobe definierats 

Vi introducerade redan vissa hello IP-adress adressintervall nödvändiga toodeploy HANA stora instanser i tidigare avsnitt. Men det finns vissa flera IP-adressintervall, som är viktiga. Vi gå igenom några ytterligare information. hello skicka följande IP-adresser som inte alla måste toobe tooMicrosoft måste toobe definierade innan du skickar en begäran om inledande distributionen:

- **VNet-adressutrymmet:** redan introduceras tidigare, eller är hello IP-adress range(s) du har tilldelat (eller planerar tooassign) tooyour adressutrymme parameter i hello Azure virtuella nätverk (VNet) ansluter toohello SAP HANA stora instans miljö. Du rekommenderas att parametern adressutrymme är ett värde för flerradiga som består av hello Azure VM-undernät range(s) och hello Azure Gateway undernätsintervall som visas i hello graphics tidigare. Det här intervallet får inte överlappa med din lokala eller Server IP-Pool eller ER P2P-adressintervall. Hur tooget det eller de här IP-adressen range(s)? Leverantören företagsnätverket team eller tjänst bör ge en eller flera IP-adress Range(s), som är eller som inte används i nätverket. Exempel: Om ditt Azure VM-undernät (se ovan) är 10.0.1.0/24 och Azure-Gatewayundernät (se följande) är 10.0.2.0/28, sedan ditt Azure VNet-adressutrymmet rekommenderas toobe två rader. 10.0.1.0/24 och 10.0.2.0/28. Hello adressutrymme värden kan aggregeras, men vi rekommenderar matchar dem toohello undernät adressintervall tooavoid oavsiktliga återanvändning av oanvända IP-adressintervall i större adressutrymmen i hello framtida någon annanstans i nätverket. **Hej VNET-adressutrymmet är ett IP-adressintervall, vilka behov toobe skickats tooMicrosoft vid begäran om en inledande distribution**

- **Azure VM-undernät IP-adressintervall:** den här IP-adressintervall, enligt beskrivningen tidigare redan är hello något du har tilldelat (eller planerar tooassign) toohello Azure VNet undernät parameter i ditt Azure VNET ansluter toohello stora instans för SAP HANA-miljö . Den här IP-adressintervallet är används tooassign IP-adresser tooyour virtuella Azure-datorer. hello IP-adresser utanför det här intervallet tillåts tooconnect tooyour stora instans för SAP HANA-servrarna. Om det behövs kan flera Virtuella Azure-undernät användas. Ett/24 CIDR-block rekommenderas av Microsoft för varje Azure VM-undernät. Detta adressintervall måste vara en del av hello-värden som används i hello Azure VNet-adressutrymmet. Hur tooget den här IP-adressintervall? Leverantören företagsnätverket team eller tjänst bör ge ett IP-adressintervall som inte redan används i nätverket.

- **VNet Gateway-undernätet IP-adressintervall:** beroende på hello funktioner som du planerar toouse, hello rekommenderade storleken är:
   - Ultrahöga prestanda ExpressRoute-gateway: / 26 Adressblock - krävs för klass som typ II SKU: er
   - Existerar med VPN- och ExpressRoute med ett högpresterande ExpressRoute-Gateway (eller mindre): / 27-Adressblock
   - Alla andra situationer: / 28 Adressblock. Detta adressintervall måste vara en del av hello-värden som används i hello ”VNet-adressutrymmet” värden. Detta adressintervall måste vara en del av hello-värden som används i hello Azure VNet-adressutrymmet värden som du behöver toosubmit tooMicrosoft. Hur tooget den här IP-adressintervall? Leverantören företagsnätverket team eller tjänst bör ge ett IP-adressintervall som inte redan används i nätverket. 

- **Adressintervall för ER P2P anslutning:** detta intervall är hello IP-intervall för SAP HANA stora instans ExpressRoute (ER) P2P-anslutning. Det här intervallet av IP-adresser måste vara en /29 CIDR IP-adressintervall. Det här intervallet får inte överlappa med din lokala eller andra Azure-IP-adressintervall. Den här IP-adressintervallet är används tooset upp hello ER anslutningar från Azure VNet ExpressRoute-Gateway toohello stora instans för SAP HANA-servrar. Hur tooget den här IP-adressintervall? Leverantören företagsnätverket team eller tjänst bör ge ett IP-adressintervall som inte redan används i nätverket. **Det här intervallet är ett IP-adressintervall, vilka behov toobe skickats tooMicrosoft vid begäran om en inledande distribution**
  
- **Servern IP-adresspoolintervall:** den här IP-adressintervallet är används tooassign hello enskilda IP-adress tooHANA stora instans servrar. hello rekommenderas undernät är ett/24 CIDR blockera - men om behövs det kan vara mindre tooa minst tillhandahåller 64 IP-adresser. I detta intervall är hello 30 första IP-adresser reserverade för användning av Microsoft. Se till att detta tas med i beräkningen när du väljer hello storleken på hello intervall. Det här intervallet får inte överlappa med din lokala eller andra Azure-IP-adresser. Hur tooget den här IP-adressintervall? Leverantören företagsnätverket team eller tjänst bör ge ett IP-adressintervall som inte redan används i nätverket. Ett/24 (rekommenderas) unika CIDR blockera toobe som används för att tilldela hello specifika IP-adresser som behövs för SAP HANA i Azure (stora instanser). **Det här intervallet är ett IP-adressintervall, vilka behov toobe skickats tooMicrosoft vid begäran om en inledande distribution**
 
Du behöver toodefine planera hello IP-adressintervall ovan, men behöver inte alla dem toobe överförs tooMicrosoft. toosummarize hello ovan hello IP-adressintervall som du är obligatoriska tooname tooMicrosoft är:

- Azure VNet adress Space(s)
- Adressintervall för ER P2P-anslutning
- Servern IP-adressintervall i poolen

Lägger till ytterligare Vnet som behöver tooconnect tooHANA stora instanser måste du toosubmit hello nytt Azure VNet adressutrymme som du lägger till tooMicrosoft. 

Följande är ett exempel på olika hello-intervall och några exempel intervall som du behöver tooconfigure och slutligen ange tooMicrosoft. Som du ser inte slås samman i hello första exemplet hello värde för hello Azure VNet-adressutrymmet, men har definierats från hello intervall med hello första Virtuella Azure-undernät IP-adressintervall och hello VNet Gateway-undernätet IP-adressintervall. Med hjälp av flera Virtuella undernät i hello Azure VNet skulle arbete i enlighet med detta genom att konfigurera och skickar hello ytterligare IP hello-adressintervall i ytterligare VM-adressundernät som en del av hello Azure VNet-adressutrymmet.

![IP-adressintervall som krävs i SAP HANA på minimal distribution av Azure (stora instanser)](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Du kan också ha hello möjligheten att sammanställa hello data som du skickar tooMicrosoft. I så fall skulle hello adressutrymme hello Azure VNet bara innehålla ett blanksteg. Med hjälp av hello IP-adressintervall som används i hello exempel tidigare. Den här aggregerade adressutrymmet för VNet kan se ut så:

![Andra risken för IP-adressintervall som krävs i SAP HANA på minimal distribution av Azure (stora instanser)](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Som du ser ovan, i stället för två mindre områden som definierats i hello Azure VNet hello adressutrymme har vi ett större område som täcker 4096 IP-adresser. En stor definition av hello adressutrymme lämnar det oanvända vissa snarare stora intervall. Eftersom hello VNet-adressutrymmet värden används för BGP-väg spridningen, användning av hello oanvända adressintervall lokala eller någon annanstans i nätverket kan orsaka problem för routning. Det är därför rekommenderade tookeep hello adressutrymme anpassad efter hello faktiska undernätsadressutrymmet används. Om det behövs, utan att det medför driftstopp på hello VNet, du kan alltid lägga till nya adressutrymme värden senare.
 
> [!IMPORTANT] 
> Varje IP-adress intervallet på ER-P2P, IP-Serverpoolen, Azure VNet-adressutrymmet måste **inte** överlappar varandra eller andra intervall används någon annanstans i nätverket; varje måste vara diskret och som hello två grafik tidigare visa får inte vara en undernät för alla andra intervall. Om överlappningar uppstår mellan dataområden hello Azure VNet inte att ansluta toohello ExpressRoute-kretsen.

### <a name="next-steps-after-address-ranges-have-been-defined"></a>Nästa steg efter adressintervall har definierats
När hello IP-adressintervall har definierats, måste hello följande aktiviteter toohappen:

1. Skicka hello IP-adressintervall för Azure VNet-adressutrymmet, hello ER P2P anslutning och servern IP-adresspoolintervall, tillsammans med andra data som har varit listat hello början av hello dokumentet. Vid denna tidpunkt, kan du också starta toocreate hello VNet och hello VM-undernät. 
2. En Express Route-krets skapas av Microsoft mellan din Azure-prenumeration och hello HANA stora instans stämpeln.
3. En klientnätverket skapas på hello stora instans stämpel av Microsoft.
4. Microsoft konfigurerar nätverk i hello SAP HANA på Azure (stora instanser) infrastruktur tooaccept IP-adresser i ditt Azure VNet-adressutrymmet som kommunicerar med HANA stora instanser.
5. Beroende på hello specifika SAP HANA på Azure (stora instanser) SKU köpt, Microsoft tilldelar en beräknings-enhet i en klientnätverket, allokera montera lagring och installera hello operativsystem (SUSE eller Red Hat Linux). IP-adresser för dessa enheter tas ut ur hello Server Pool för IP-adressen adressintervallet som du skickat tooMicrosoft.

Hello slutet av hello distributionsprocessen ger Microsoft hello följande data tooyou:
- Information som behövs för tooconnect din Azure VNet(s) toohello ExpressRoute-krets som ansluter Azure Vnet tooHANA stora instanser:
     - Auktorisering nycklar
     - ExpressRoute PeerID
- Data tooaccess HANA stora instanser när du har fastställt ExpressRoute-krets och Azure VNet.

Du kan också hitta hello sifferföljd ansluta HANA stora instanser i hello dokumentet [avslutas tooEnd installationsprogrammet för SAP HANA stora instanser](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). Många av hello följande visas i ett exempel på distribution i detta dokument. 


## <a name="connecting-a-vnet-toohana-large-instance-expressroute"></a>Ansluta ett virtuellt nätverk tooHANA stora instans ExpressRoute

Du kan starta ansluter hello VNet som du skapade innan tooHANA stora instanser som du definierat alla hello IP-adressintervall och nu fått tillbaka hello data från Microsoft. En gång hello Azure VNet har skapats, en ExpressRoute-gateway måste skapas på hello VNet toolink hello VNet toohello ExpressRoute-krets som ansluter toohello kunden klient på hello stora instans stamp.

> [!NOTE] 
> Det här steget kan ta upp too30 minuter toocomplete som hello ny gateway har skapats i hello avses Azure-prenumeration och sedan anslutna toohello angetts Azure VNet.

Om det finns redan en gateway, måste du kontrollera om det är en ExpressRoute-gateway eller inte. Om inte, hello gateway måste tas bort och återskapas som en ExpressRoute-gateway. Om en ExpressRoute-gateway är redan skapat eftersom hello Azure VNet är redan ansluten toohello ExpressRoute-krets för lokal anslutning, Fortsätt toohello länka Vnet nedan.

- Använd antingen hello (nya) [Azure-portalen](https://portal.azure.com/), eller PowerShell toocreate en ExpressRoute-VPN-gateway är ansluten tooyour VNet.
  - Om du använder hello Azure-portalen, lägger du till en ny **virtuell nätverksgateway** och välj sedan **ExpressRoute** som hello gateway-typen.
  - Om du väljer PowerShell i stället först hämta och använda hello senaste [Azure PowerShell SDK](https://azure.microsoft.com/downloads/) tooensure en optimal upplevelse. hello följande kommandon skapar en ExpressRoute-gateway. hello texter föregås av en  _$_  är användardefinierade variabler som behöver toobe uppdateras med din specifika information.

```PowerShell
# These Values should already exist, update toomatch your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used toocreate hello gateway, update for how you wish hello GW components toobe named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

# These Commands create hello Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

I det här exemplet användes hello HighPerformance gateway SKU. Alternativen är HighPerformance eller UltraPerformance som hello endast gateway-SKU: er som stöds för SAP HANA i Azure (stora instanser).

> [!IMPORTANT]
> För HANA stora instanser av hello SKU typer S384, S384m, S384xm, S576, S768 och S960 (typen II klassen SKU: er), hello användning av hello UltraPerformance Gateway-SKU är obligatoriskt.

### <a name="linking-vnets"></a>Länka Vnet

Nu när hello Azure VNet har en ExpressRoute-gateway kan använda du hello auktoriseringsinformation som tillhandahålls av Microsoft tooconnect hello ExpressRoute-gateway toohello SAP HANA i Azure (stora instanser) ExpressRoute-kretsen skapas för den här anslutningen. Det här steget kan utföras med hello Azure-portalen eller PowerShell. hello portal rekommenderas, men PowerShell instruktioner är som följer. 

- Du kan köra följande kommandon för varje gateway för virtuellt nätverk med hjälp av en annan AuthGUID för varje anslutning hello. hello först två poster visas i hello skript följande komma från hello information som tillhandahålls av Microsoft. Dessutom är hello AuthGUID specifika för varje virtuellt nätverk och dess gateway. Innebär att om du vill tooadd en annan Azure VNet, du behöver tooget en annan AuthID för ExpressRoute-krets som ansluter HANA stora instanser till Azure. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define hello name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between hello ER Circuit and your Gateway using hello Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Om du vill tooconnect hello gateway toomultiple ExpressRoute-kretsar som är associerade med din prenumeration kan behöva du tooexecute det här steget mer än en gång. Till exempel du förmodligen kommer tooconnect hello samma virtuella nätverk Gateway toohello ExpressRoute-krets som ansluter hello VNet tooyour lokalt nätverk.

## <a name="adding-more-ip-addresses-or-subnets"></a>Lägga till flera IP-adresser och undernät

Använd antingen hello Azure-portalen, PowerShell eller CLI när du lägger till flera IP-adresser eller undernät.

I det här fallet är hello rekommendation tooadd hello nya IP-adressintervall som nya intervallet toohello VNet-adressutrymmet i stället för att generera ett nytt aggregerade intervall. I båda fallen måste toosubmit den här ändringen tooMicrosoft tooallow anslutningen utanför de nya IP-adressintervallet toohello HANA stora instans-enheterna i din klient. Du kan öppna en Azure-supporten begäran tooget hello nya VNet-adressutrymmet lagts till. När du får bekräftelsen utför hello nästa steg.

toocreate ett ytterligare undernät från hello Azure-portalen finns i artikeln hello [skapa ett virtuellt nätverk med hello Azure-portalen](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), och toocreate från PowerShell, se [skapa ett virtuellt nätverk med PowerShell](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="adding-vnets"></a>Lägga till Vnet

När du först ansluta en eller flera virtuella Azure-nätverk, kanske du vill tooadd extra som åtkomst till SAP HANA i Azure (stora instanser). Först begära en Azure-supporten kan du inkludera både hello specifik information identifierande hello viss Azure-distribution och hello IP-adressutrymme range(s) av hello Azure VNet-adressutrymmet i din begäran. SAP HANA på Azure Service Management ger hello nödvändig information du behöver tooconnect hello ytterligare Vnet och ExpressRoute. För varje VNet behöver du ett unikt Auktoriseringsnyckeln tooconnect toohello ExpressRoute-krets tooHANA stora instanser.

Steg tooadd ett nytt Azure VNet:

1. Slutföra hello första steget i hello onboarding-processen, se hello _skapar Azure VNet_ avsnitt.
2. Slutför hello andra steg i hello onboarding-processen, se hello _skapar gateway-undernätet_ avsnitt.
3. tooconnect din ytterligare Vnet toohello HANA stora instans ExpressRoute-krets, öppna en Azure-supportbegäran med information på hello nya VNet och begär en ny nyckel för auktorisering.
4. När du meddelas att hello auktorisering är slutförd, använda hello Microsoft auktoriseringsinformation toocomplete hello tredje steget i hello onboarding-processen, se hello _länka Vnet_ avsnitt.

## <a name="increasing-expressroute-circuit-bandwidth"></a>Ökad bandbredd för ExpressRoute-krets

Kontakta SAP HANA på Azure Service Management. Skapa en Azure-supporten om du är uppmanade tooincrease hello bandbredden för hello SAP HANA i Azure (stora instanser) ExpressRoute-kretsen. (Du kan begära en ökad för en enskild kretsbandbredden in tooa högst 10 Gbit/s). Sedan visas meddelandet när hello åtgärd har slutförts; Ingen ytterligare åtgärd behövs tooenable denna högre hastighet i Azure.

## <a name="adding-an-additional-expressroute-circuit"></a>Lägga till en ytterligare ExpressRoute-krets

Kontakta SAP HANA på Azure Service Management om du rekommenderas att en ytterligare ExpressRoute-krets krävs, gör ett Azure-supporten begäran toocreate nya ExpressRoute-krets (och tooget auktorisering information tooconnect tooit). hello-adressutrymmet som ska användas på hello Vnet måste definieras innan du gör hello begäran för SAP HANA på Azure-tjänsthantering tooprovide tillstånd.

När nya hello-kretsen har skapats och hello SAP HANA på Azure Service Management-konfigurationen är klar ska tooreceive meddelande med hello information du behöver tooproceed. Följ hello-stegen ovan för att skapa och koppla hello nya VNet toothis ytterligare krets. Du är inte kan tooconnect Azure Vnet toothis ytterligare krets om de redan är ansluten tooanother SAP HANA i Azure (stora instans) ExpressRoute-kretsen i hello samma Azure-Region.

## <a name="deleting-a-subnet"></a>Om du tar bort ett undernät

tooremove ett VNet i undernät antingen hello Azure-portalen, PowerShell eller CLI kan användas. Om ditt Azure VNet IP-adress intervallet-/ Azure VNet-adressutrymmet var ett sammansatt intervall, finns det inga Följ för du med Microsoft. Förutom att hello sprids fortfarande VNet BGP route-adressutrymmet som innehåller hello bort undernät. Om du har angett hello Azure VNet IP-adress intervallet-/ Azure VNet-adressutrymmet som flera IP-adressintervall för vilket som har tilldelade tooyour bort undernät, bör du ta bort som out-of-VNet-adressutrymmet och därefter meddela SAP HANA på Azure-tjänsthantering tooremove från hello allt som SAP HANA i Azure (stora instanser) tillåts toocommunicate med.

När det inte finns ännu specifika, dedikerad Azure.com anvisningar om att ta bort undernät, hello processen för att ta bort undernät är omvänd hello hello processen för att lägga till dem för. Se artikeln hello [skapa ett virtuellt nätverk med hello Azure-portalen](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för mer information om hur du skapar undernät.

## <a name="deleting-a-vnet"></a>Om du tar bort ett virtuellt nätverk

Använd antingen hello Azure-portalen, PowerShell eller CLI när du tar bort ett virtuellt nätverk. SAP HANA på Azure-tjänsthantering tar bort hello befintliga tillstånd på hello SAP HANA i Azure (stora instanser) ExpressRoute-kretsen och ta bort hello Azure VNet IP-adress intervallet-/ Azure VNet-adressutrymmet för hello kommunikation med HANA stora instanser.

När hello VNet har tagits bort, öppnar du ett Azure-supporten begäran tooprovide hello IP-adressutrymme range(s) toobe tas bort.

När det inte finns ännu specifika, dedikerad Azure.com anvisningar om att ta bort Vnet, hello processen för att ta bort Vnet är omvänd hello av hello process för att lägga till dem, vilket beskrivs ovan. Se hello artiklar [skapa ett virtuellt nätverk med hello Azure-portalen](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) och [skapa ett virtuellt nätverk med PowerShell](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för mer information om hur du skapar Vnet.

tooensure allt har tagits bort, ta bort hello följande objekt:

- Hej ExpressRoute-anslutning, VNet Gateway offentlig IP för Gateway för virtuellt nätverk och virtuella nätverk

## <a name="deleting-an-expressroute-circuit"></a>Om du tar bort en ExpressRoute-krets

tooremove en ytterligare SAP HANA i Azure (stora instanser) ExpressRoute-kretsen öppna en Azure-supportbegäran med SAP HANA på Azure-tjänsthantering och begära att hello kretsen ska tas bort. Inom hello Azure-prenumeration, kan du ta bort eller behålla hello VNet efter behov. Dock måste du ta bort hello anslutning mellan hello HANA stora instanser ExpressRoute-krets och hello länkade gateway för virtuellt nätverk.

Om du vill också tooremove ett VNet, följer du hello vägledning om hur du raderar ett VNet i hello ovan.


