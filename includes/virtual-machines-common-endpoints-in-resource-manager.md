hello metoden tooAzure slutpunkter inte fungerar riktigt mellan hello klassisk och Resource Manager distributionsmodellerna. I hello distributionsmodell för Resource Manager, nu har du hello flexibilitet toocreate Nätverksfilter som styr hello flödet av trafik till och från dina virtuella datorer. Dessa filter kan du toocreate komplexa nätverksmiljöer utöver en enkel slutpunkt som hello klassiska distributionsmodellen. Den här artikeln innehåller en översikt över Nätverkssäkerhetsgrupper och hur de skiljer sig från att använda klassisk slutpunkter kan skapa dessa filtreringsregler, och exempel på scenarier för distribution.

## <a name="overview-of-resource-manager-deployments"></a>Översikt över Resource Manager distributioner
Slutpunkter i hello klassiska distributionsmodellen ersättas av Nätverkssäkerhetsgrupper och åtkomstregler som kontrollen åtkomstkontrollistan (ACL). Snabbsteg för att implementera Network Security Group ACL-regler är:

* Skapa en säkerhetsgrupp för nätverk.
* tooallow eller neka trafik, definiera Network Security Group ACL-regler.
* Tilldela din Nätverkssäkerhetsgruppen tooa nätverksgränssnittet eller undernät för virtuellt nätverk.

Om du vill tooalso utföra vidarebefordrade portar, du behöver tooplace belastningsutjämning framför den virtuella datorn och använda NAT-regler. Snabbsteg för att implementera en belastningsutjämnare och NAT-regler skulle vara följande:

* Skapa en belastningsutjämnare.
* Skapa en serverdelspool och Lägg till din toohello pool för virtuella datorer.
* Definiera NAT-regler för vidarebefordran av hello krävs port.
* Tilldela NAT-regler tooyour virtuella datorer.

## <a name="network-security-group-overview"></a>Översikt över Nätverkssäkerhetsgrupp
Nätverkssäkerhetsgrupper ange ett säkerhetslager du tooallow specifika portar och undernät tooaccess dina virtuella datorer. Du normalt ha alltid en Nätverkssäkerhetsgrupp som tillhandahåller den här säkerhetslager mellan virtuella datorer och hello utanför world. Nätverkssäkerhetsgrupper kan vara tillämpade tooa undernät för virtuellt nätverk eller ett visst nätverksgränssnitt för en virtuell dator. I stället för skapar endpoint ACL-regler kan skapa du nu Network Security Group ACL-regler. Dessa ACL-regler ger mycket större kontroll än att bara skapa en slutpunkt tooforward en viss port. Mer information finns i [vad är en Nätverkssäkerhetsgrupp?](../articles/virtual-network/virtual-networks-nsg.md)

> [!TIP]
> Du kan tilldela Nätverkssäkerhetsgrupper toomultiple undernät eller till nätverksgränssnitt. Det finns ingen 1:1-mappning. Du kan skapa en säkerhetsgrupp för nätverk med en gemensam uppsättning ACL-regler och använda toomultiple undernät eller till nätverksgränssnitt. Nätverkssäkerhetsgruppen kan dessutom vara tillämpade tooresources över din prenumeration (baserat på [roll baserad åtkomstkontroller](../articles/active-directory/role-based-access-control-what-is.md).

## <a name="load-balancers-overview"></a>Läsa in belastningsutjämning översikt
I hello klassiska distributionsmodellen skulle Azure utför alla hello NAT (Network Address Translation) och port vidarebefordran på en molnbaserad tjänst för dig. När du skapar en slutpunkt, anger du hello externa porten tooexpose tillsammans med hello Intern port toodirect trafik till. Nätverkssäkerhetsgrupper ensamt utför inte den här samma NAT och Portvidarebefordring av. 

tooallow du toocreate NAT-regler för denna port vidarebefordran, skapa en Azure belastningsutjämnare i resursgruppen. Igen, hello belastningsutjämning är detaljerade tillräckligt med tooonly gäller toospecific virtuella datorer om det behövs. hello Azure ladda belastningsutjämnaren NAT-regler fungerar tillsammans med Network Security Group ACL-regler tooprovide mycket större flexibilitet och kontroll än uppnås med hjälp av slutpunkter molntjänst. Mer information finns i hello [översikt över belastningsutjämnare](../articles/load-balancer/load-balancer-overview.md).

## <a name="network-security-group-acl-rules"></a>Nätverkssäkerhetsgruppen ACL-regler
ACL-regler kan du definiera vilka trafiken kan flöda till och från den virtuella datorn baserat på specifika portar eller portintervall protokoll. Regler som har tilldelats tooindividual virtuella datorer eller tooa undernät. följande skärmbild hello är ett exempel på ACL-regler för en vanliga webbserver:

![Lista över Nätverkssäkerhetsgruppen ACL-regler](./media/virtual-machines-common-endpoints-in-resource-manager/example-acl-rules.png)

ACL-regler tillämpas baserat på en prioritet mått som du anger - hello högre hello-värde, hello lägre hello prioritet. Varje Nätverkssäkerhetsgruppen har tre standard regler som är utformade toohandle hello flödet av Azure nätverkstrafiken med ett explicit `DenyAllInbound` som hello slutliga regel. Standard ACL-regler är angivna en låg prioritet toonot störa regler som du skapar.

## <a name="assigning-network-security-groups"></a>Tilldela Nätverkssäkerhetsgrupper
Du kan tilldela en Nätverkssäkerhetsgrupp tooa undernät eller ett nätverksgränssnitt. Den här metoden kan du toobe som detaljerade som behövs för att tillämpa din ACL-regler tooonly en specifik VM, eller se till att en gemensam uppsättning av ACL-regler är tillämpade tooall virtuella datorer som en del av ett undernät:

![Tillämpa NSG: er toonetwork gränssnitt eller undernät](./media/virtual-machines-common-endpoints-in-resource-manager/apply-nsg-to-resources.png)

hello funktionssätt hello Nätverkssäkerhetsgruppen ändras inte beroende på tilldelas tooa undernät eller ett nätverksgränssnitt. Ett vanligt distributionsscenario har hello Nätverkssäkerhetsgrupp tilldelad tooa undernät tooensure kompatibilitet för alla virtuella datorer kopplade toothat undernät. Mer information finns i [tillämpa Network Security groups tooresources](../articles/virtual-network/virtual-networks-nsg.md#associating-nsgs).

## <a name="default-behavior-of-network-security-groups"></a>Standardbeteendet för Nätverkssäkerhetsgrupper
Beroende på hur och när du skapar en säkerhetsgrupp för nätverk, kan standardregler skapas för virtuella Windows-datorer toopermit RDP-åtkomst på TCP-port 3389. Standardregler för virtuella Linux-datorer att SSH-åtkomst på TCP-port 22. Reglerna för automatisk ACL skapas under hello följande villkor:

* Om du skapar en virtuell Windows-dator via hello portal och acceptera hello standard åtgärd toocreate en Nätverkssäkerhetsgrupp skapas en ACL regeln tooallow TCP-port 3389 (RDP).
* Om du skapar en Linux VM hello-portalen och acceptera hello standard åtgärd toocreate en Nätverkssäkerhetsgrupp, skapas en ACL regeln tooallow TCP-port 22 (SSH).

Dessa standard ACL-regler har inte skapats under andra villkor. Du kan inte ansluta tooyour VM utan att skapa hello rätt ACL-regler. Dessa villkor är hello följande vanliga åtgärder:

* Skapa en Nätverkssäkerhetsgrupp hello-portalen som en separat åtgärd toocreating hello VM.
* Skapar en Nätverkssäkerhetsgrupp programmässigt via PowerShell, Azure CLI, Rest API: er och så vidare.
* Skapa en virtuell dator och tilldela den tooan befintlig säkerhetsgrupp för nätverk som inte redan har hello lämpliga regeln definieras.

I alla hello föregående fall, måste toocreate ACL-regler för VM tooallow hello lämpliga fjärrhantering anslutningar.

## <a name="default-behavior-of-a-vm-without-a-network-security-group"></a>Standardbeteendet för en virtuell dator utan en Nätverkssäkerhetsgrupp
Du kan skapa en virtuell dator utan att skapa en Nätverkssäkerhetsgrupp. I sådana fall måste du ansluta tooyour VM med RDP eller SSH utan att skapa någon ACL-regler. På samma sätt om du har installerat en webbtjänst på port 80 som tjänsten är automatiskt tillgänglig via fjärranslutning. hello VM har alla portar är öppna.

> [!NOTE]
> Du måste fortfarande toohave en offentlig IP-adress som tilldelats tooa VM för alla fjärranslutningar. Inte har en Nätverkssäkerhetsgrupp för hello undernäts- eller gränssnitt saknar en explicit hello VM tooany externa trafiken. hello standardåtgärden när du skapar en virtuell dator via hello portal är toocreate en ny offentlig IP-adress. För alla andra formulär för att skapa en virtuell dator, till exempel PowerShell, Azure CLI eller Resource Manager-mall, skapas en offentlig IP-adress inte automatiskt om du inte uttryckligen har begärt. hello standardåtgärden hello-portalen är också toocreate en Nätverkssäkerhetsgrupp. Den här standardåtgärd innebär inte hamna i en situation med en exponerade VM som har inget nätverk filtrering på plats.

## <a name="understanding-load-balancers-and-nat-rules"></a>Förstå belastningsutjämning och NAT-regler
Du kan skapa slutpunkter som också utföra vidarebefordrade portar i hello klassiska distributionsmodellen. När du skapar en virtuell dator i hello klassiska distributionsmodellen skulle ACL-regler för RDP eller SSH skapas automatiskt. Dessa regler skulle inte gränssnittshändelsen TCP-port 3389 eller TCP-port 22 respektive toohello utanför världen. I stället skulle ett högt värde TCP-port exponeras som mappar toohello lämplig intern port. Du kan också skapa egna ACL-regler på liknande sätt, till exempel exponera en webbserver på TCP-port 4280 toohello utanför världen. Du kan se dessa ACL-regler och portmappningarna i följande skärmbild från hello klassisk portal hello:

![Vidarebefordrade portar med klassiska slutpunkter](./media/virtual-machines-common-endpoints-in-resource-manager/classic-endpoints-port-forwarding.png)

Med Nätverkssäkerhetsgrupper hanteras vidarebefordrade portar funktionen av en belastningsutjämnare. Mer information finns i [belastningsutjämnare i Azure](../articles/load-balancer/load-balancer-overview.md). hello följande exempel visas en belastningsutjämnare med en NAT-regel tooperform vidarebefordrade portar TCP-port 4222 toohello interna TCP-port 22 en virtuell dator:

![Belastningsutjämningsreglerna NAT för vidarebefordrade portar](./media/virtual-machines-common-endpoints-in-resource-manager/load-balancer-nat-rules.png)

> [!NOTE]
> När du implementerar en belastningsutjämnare du normalt inte tilldelar hello Virtuella datorn en offentlig IP-adress. I stället har hello belastningsutjämnaren en offentlig IP-adress som tilldelats tooit. Du måste fortfarande toocreate din Nätverkssäkerhetsgruppen och ACL-regler toodefine hello flödet av trafik till och från den virtuella datorn. hello är load balancer NAT-regler helt enkelt toodefine vilka portar tillåts via hello belastningsutjämnare och hur de få fördelad över hello backend virtuella datorer. Därför behöver du toocreate en NAT-regel för trafik tooflow via hello belastningsutjämnaren. tooallow hello trafik tooreach hello VM, skapa en Network Security Group regeln.
