Metod för Azure-slutpunkter inte fungerar riktigt mellan klassisk och Resource Manager distributionsmodellerna. I Resource Manager-distributionsmodellen nu har du möjlighet att skapa Nätverksfilter som styr flödet av trafik till och från dina virtuella datorer. Dessa filter kan du skapa komplexa nätverksmiljöer utöver en enkel slutpunkt som i den klassiska distributionsmodellen. Den här artikeln innehåller en översikt över Nätverkssäkerhetsgrupper och hur de skiljer sig från att använda klassisk slutpunkter kan skapa dessa filtreringsregler, och exempel på scenarier för distribution.

## <a name="overview-of-resource-manager-deployments"></a>Översikt över Resource Manager distributioner
Slutpunkterna i den klassiska distributionsmodellen ersätts med Nätverkssäkerhetsgrupper och regler för åtkomstkontroll åtkomstkontrollistan (ACL). Snabbsteg för att implementera Network Security Group ACL-regler är:

* Skapa en säkerhetsgrupp för nätverk.
* Definiera Network Security Group ACL-regler för att tillåta eller neka trafik.
* Tilldela säkerhetsgrupp för nätverk till ett nätverksgränssnitt eller undernät för virtuellt nätverk.

Om du vill också utföra vidarebefordrade portar, måste du placera en belastningsutjämnare framför den virtuella datorn och använda NAT-regler. Snabbsteg för att implementera en belastningsutjämnare och NAT-regler skulle vara följande:

* Skapa en belastningsutjämnare.
* Skapa en serverdelspool och lägga till dina virtuella datorer i poolen.
* Definiera NAT-regler för vidarebefordran för nödvändig port.
* Tilldela NAT-regler till dina virtuella datorer.

## <a name="network-security-group-overview"></a>Översikt över Nätverkssäkerhetsgrupp
Nätverkssäkerhetsgrupper tillhandahåller ett säkerhetslager att tillåta specifika portar och undernät åtkomst till dina virtuella datorer. Du normalt ha alltid en Nätverkssäkerhetsgrupp som tillhandahåller den här säkerhetslager mellan dina virtuella datorer och omvärlden. Nätverkssäkerhetsgrupper kan tillämpas på ett undernät för virtuellt nätverk eller ett visst nätverksgränssnitt för en virtuell dator. I stället för skapar endpoint ACL-regler kan skapa du nu Network Security Group ACL-regler. Dessa ACL-regler ger mycket större kontroll än att bara skapa en slutpunkt för att vidarebefordra en viss port. Mer information finns i [vad är en Nätverkssäkerhetsgrupp?](../articles/virtual-network/virtual-networks-nsg.md)

> [!TIP]
> Du kan tilldela Nätverkssäkerhetsgrupper till flera undernät och nätverksgränssnitt. Det finns ingen 1:1-mappning. Du kan skapa en säkerhetsgrupp för nätverk med en gemensam uppsättning ACL-regler och gäller för flera undernät eller nätverksgränssnitt. Dessutom Nätverkssäkerhetsgruppen kan tillämpas på resurser i din prenumeration (baserat på [roll baserad åtkomstkontroller](../articles/active-directory/role-based-access-control-what-is.md).

## <a name="load-balancers-overview"></a>Läsa in belastningsutjämning översikt
I den klassiska distributionsmodellen utför Azure alla NAT (Network Address Translation) och port vidarebefordran på en molnbaserad tjänst för dig. När du skapar en slutpunkt, anger du den externa porten att exponera tillsammans med den interna porten att dirigera trafik till. Nätverkssäkerhetsgrupper ensamt utför inte den här samma NAT och Portvidarebefordring av. 

Om du vill kan du skapa NAT-regler för denna port vidarebefordran, skapa en Azure belastningsutjämnare i resursgruppen. Load balancer är detaljerade till gäller endast för specifika virtuella datorer om det behövs. Azure load balancer NAT-regler fungerar tillsammans med nätverk Säkerhet grupp ACL-regler för att tillhandahålla mycket större flexibilitet och kontroll än uppnås med hjälp av slutpunkter molntjänst. Mer information finns i [översikt över belastningsutjämnare](../articles/load-balancer/load-balancer-overview.md).

## <a name="network-security-group-acl-rules"></a>Nätverkssäkerhetsgruppen ACL-regler
ACL-regler kan du definiera vilka trafiken kan flöda till och från den virtuella datorn baserat på specifika portar eller portintervall protokoll. Regler kopplade till enskilda virtuella datorer eller till ett undernät. Följande skärmbild visas ett exempel av ACL-regler för en vanliga webbserver:

![Lista över Nätverkssäkerhetsgruppen ACL-regler](./media/virtual-machines-common-endpoints-in-resource-manager/example-acl-rules.png)

ACL-regler tillämpas baserat på en prioritet mått som du anger - Ju högre värde, desto lägre prioritet. Varje Nätverkssäkerhetsgruppen har tre standardregler som utformats för att hantera flödet av Azure nätverkstrafiken med ett explicit `DenyAllInbound` som sista regeln. Standard ACL-regler får inte störa regler som du skapar en låg prioritet.

## <a name="assigning-network-security-groups"></a>Tilldela Nätverkssäkerhetsgrupper
Du kan tilldela en Nätverkssäkerhetsgrupp ett undernät eller ett nätverksgränssnitt. Den här metoden kan du vara som detaljerade efter behov när du tillämpar ACL-regler på endast en specifik VM, eller se till att en gemensam uppsättning ACL-regler tillämpas på alla virtuella datorer som en del av ett undernät:

![Tillämpa NSG: er för nätverksgränssnitt eller undernät](./media/virtual-machines-common-endpoints-in-resource-manager/apply-nsg-to-resources.png)

Beteendet för Nätverkssäkerhetsgruppen ändras inte beroende på tilldelas till ett undernät eller ett nätverksgränssnitt. Ett vanligt distributionsscenario har tilldelats ett undernät för att säkerställa att alla virtuella datorer kopplade till undernätet Nätverkssäkerhetsgruppen. Mer information finns i [tillämpa nätverkssäkerhetsgrupper på resurser](../articles/virtual-network/virtual-networks-nsg.md#associating-nsgs).

## <a name="default-behavior-of-network-security-groups"></a>Standardbeteendet för Nätverkssäkerhetsgrupper
Beroende på hur och när du skapar en säkerhetsgrupp för nätverk, kan standardregler skapas för virtuella Windows-datorer så att en RDP-åtkomst på TCP-port 3389. Standardregler för virtuella Linux-datorer att SSH-åtkomst på TCP-port 22. Reglerna för automatisk ACL skapas på följande villkor:

* Om du skapar en Windows-VM via portalen och acceptera standardåtgärden om du vill skapa en säkerhetsgrupp för nätverk, skapas en ACL-regel som tillåter TCP-port 3389 (RDP).
* Om du skapar en Linux VM via portalen och acceptera standardåtgärden om du vill skapa en säkerhetsgrupp för nätverk, skapas en ACL-regel som tillåter TCP-port 22 (SSH).

Dessa standard ACL-regler har inte skapats under andra villkor. Du kan inte ansluta till den virtuella datorn utan att skapa lämpliga ACL-regler. Dessa villkor är följande vanliga åtgärder:

* Skapa en säkerhetsgrupp för nätverk via portalen som en separat åtgärd för att skapa den virtuella datorn.
* Skapar en Nätverkssäkerhetsgrupp programmässigt via PowerShell, Azure CLI, Rest API: er och så vidare.
* Skapa en virtuell dator och tilldela den till en befintlig säkerhetsgrupp för nätverk som inte redan har rätt ACL-regel definieras.

I alla föregående fall måste du skapa ACL-regler för den virtuella datorn så att rätt fjärrhantering-anslutningar.

## <a name="default-behavior-of-a-vm-without-a-network-security-group"></a>Standardbeteendet för en virtuell dator utan en Nätverkssäkerhetsgrupp
Du kan skapa en virtuell dator utan att skapa en Nätverkssäkerhetsgrupp. I sådana fall måste ansluta du till den virtuella datorn med RDP eller SSH utan att skapa någon ACL-regler. På samma sätt om du har installerat en webbtjänst på port 80 som tjänsten är automatiskt tillgänglig via fjärranslutning. Den virtuella datorn har alla portar är öppna.

> [!NOTE]
> Du måste ha en offentlig IP-adress till en virtuell dator för alla fjärranslutningar. Inte har en Nätverkssäkerhetsgrupp för gränssnittet undernäts- eller saknar en explicit så att alla externa trafiken. Standardåtgärden när du skapar en virtuell dator via portalen är att skapa en ny offentlig IP-adress. För alla andra formulär för att skapa en virtuell dator, till exempel PowerShell, Azure CLI eller Resource Manager-mall, skapas en offentlig IP-adress inte automatiskt om du inte uttryckligen har begärt. Standardåtgärden via portalen är också att skapa en Nätverkssäkerhetsgrupp. Den här standardåtgärd innebär inte hamna i en situation med en exponerade VM som har inget nätverk filtrering på plats.

## <a name="understanding-load-balancers-and-nat-rules"></a>Förstå belastningsutjämning och NAT-regler
Du kan skapa slutpunkter som också utföra vidarebefordrade portar i den klassiska distributionsmodellen. När du skapar en virtuell dator i den klassiska distributionsmodellen skulle ACL-regler för RDP eller SSH skapas automatiskt. Dessa regler skulle inte gränssnittshändelsen TCP-port 3389 eller TCP-port 22 respektive till omvärlden. I stället skulle ett högt värde TCP-port exponeras som mappar till lämplig intern port. Du kan också skapa egna ACL-regler på liknande sätt, till exempel exponera en webbserver på TCP-port 4280 för allmänheten. Du kan se dessa ACL-regler och portmappningarna i följande skärmbild från den klassiska portalen:

![Vidarebefordrade portar med klassiska slutpunkter](./media/virtual-machines-common-endpoints-in-resource-manager/classic-endpoints-port-forwarding.png)

Med Nätverkssäkerhetsgrupper hanteras vidarebefordrade portar funktionen av en belastningsutjämnare. Mer information finns i [belastningsutjämnare i Azure](../articles/load-balancer/load-balancer-overview.md). I följande exempel visas en belastningsutjämnare med en NAT-regel för vidarebefordran av port TCP-port 4222 interna TCP-port 22 en virtuell dator:

![Belastningsutjämningsreglerna NAT för vidarebefordrade portar](./media/virtual-machines-common-endpoints-in-resource-manager/load-balancer-nat-rules.png)

> [!NOTE]
> När du implementerar en belastningsutjämnare tilldela du normalt inte Virtuellt datorn en offentlig IP-adress. I stället har belastningsutjämnaren en offentlig IP-adress som tilldelats. Du måste skapa Nätverkssäkerhetsgruppen och ACL-regler för att definiera flödet av trafik till och från den virtuella datorn. Load balancer NAT-regler är bara att definiera vilka portar tillåts via belastningsutjämnaren och hur de få fördelad över serverdelen virtuella datorer. Därför måste du skapa en NAT-regel för trafik till belastningsutjämnaren. Skapa en Network Security Group ACL-regel för att tillåta trafik till den virtuella datorn.
