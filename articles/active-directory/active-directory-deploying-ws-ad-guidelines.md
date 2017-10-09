---
title: "aaaGuidelines för distribution av Windows Server Active Directory på Azure Virtual Machines | Microsoft Docs"
description: "Om du känner till hur toodeploy AD DS och AD federationstjänster lokalt, Lär dig hur de fungerar på virtuella Azure-datorer."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
ms.assetid: 04df4c46-e6b6-4754-960a-57b823d617fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: femila
ms.openlocfilehash: 9ad5a5f138a6402cbb656d9160545846051207b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Riktlinjer för att distribuera Windows Server Active Directory på Azure virtual machines
Den här artikeln förklarar hello viktiga skillnader mellan distribuera Windows Server Active Directory Domain Services (AD DS) och Active Directory Federation Services (AD FS) lokalt och distribuera dem på Microsoft Azure-datorer.

## <a name="scope-and-audience"></a>Omfång och målgrupp
hello artikeln är avsedd för de redan erfarenhet av distribution av Active Directory lokalt. Den omfattar hello skillnaderna mellan distribution av Active Directory på virtuella nätverk för virtuella datorer/Azure för Microsoft Azure och traditionella lokala Active Directory-distributioner. Virtuella Azure-datorer och virtuella Azure-nätverk är en del av en infrastruktur-som-en-tjänst (IaaS för organisationer tooleverage bearbetningsresurser i hello molnet).

De som inte är bekant med AD-distribution finns hello [Distributionsguide](https://technet.microsoft.com/library/cc753963) eller [planera distributionen av AD FS](https://technet.microsoft.com/library/dn151324.aspx) efter behov.

Den här artikeln förutsätter att hello läsaren är bekant med hello följande begrepp:

* Windows Server AD DS-distribution och hantering
* Distribution och konfiguration av DNS-toosupport en Windows Server AD DS-infrastruktur
* Windows Server AD FS-distribution och hantering
* Distribuera, konfigurera och hantera förlitande parters program (webbplatser och webbprogram services) som kan använda Windows Server AD FS-token
* Allmän virtuella begrepp, till exempel hur tooconfigure en virtuell dator, virtuella diskar och virtuella nätverk

Den här artikeln visar hello kraven för ett scenario med hybridanvändning distribution där Windows Server AD DS eller AD FS är delvis distribuerade lokalt och delvis distribueras på virtuella Azure-datorer. hello dokumentet beskrivs först hello viktiga skillnader mellan Windows Server AD DS och AD FS som körs på Azure virtual machines jämfört med lokalt och viktiga punkter som påverkar design och distribution. hello resten av hello dokumentet ger riktlinjer för varje hello beslutspunkterna i detalj och hur tooapply hello riktlinjer toovarious distributionsscenarier.

Den här artikeln behandlar inte hello konfigurationen av [Azure Active Directory](http://azure.microsoft.com/services/active-directory/), vilket är en REST-baserad tjänst som tillhandahåller identity management och kontroll åtkomstfunktioner för molnprogram. Azure Active Directory (Azure AD) och Windows Server AD DS finns dock utformad toowork tillsammans tooprovide en identitets- och hanteringslösning för hybrid dagens IT-miljöer och moderna program. toohelp förstå hello skillnader och relationer mellan Windows Server AD DS och AD Azure bör du överväga att hello följande:

1. Du kan köra Windows Server AD DS i hello molnet på virtuella Azure-datorer när du använder Azure tooextend ditt lokala datacenter i hello moln.
2. Du kan använda Azure AD toogive dina användare enkel inloggning tooSoftware-som-en-tjänst (SaaS)-program. Microsoft Office 365 använder den här tekniken, till exempel och program som körs på Azure eller andra molnplattformar kan också använda den.
3. Du kan använda Azure AD (dess åtkomstkontrolltjänsten) toolet användare loggar in med identiteter från Facebook, Google, Microsoft och andra identitet providers tooapplications som finns i hello molnet eller lokalt.

Mer information om skillnaderna finns [Azure Identity](fundamentals-identity.md).

## <a name="related-resources"></a>Relaterade resurser
Du kan hämta och köra hello [Azure virtuella Readiness Assessment](https://www.microsoft.com/download/details.aspx?id=40898). hello assessment automatiskt kontrollera din lokala miljö och skapa en anpassad rapport som baseras på hello vägledning finns i det här avsnittet toohelp du migrerar hello miljö tooAzure.

Vi rekommenderar att du först granska hello självstudier och guider videor som täcker hello följande avsnitt:

* [Konfigurera ett virtuellt nätverk Cloud-Only i hello Azure-portalen](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
* [Konfigurera en plats-till-plats-VPN i hello Azure-portalen](../vpn-gateway/vpn-gateway-site-to-site-create.md)
* [Installera en ny Active Directory-skog på Azure-nätverk](active-directory-new-forest-virtual-machine.md)
* [Installera en replik Active Directory-domänkontrollant på Azure](active-directory-install-replica-active-directory-domain-controller.md)
* [Microsoft Azure IT Pro IaaS: (01) virtuella grunderna](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IT Pro IaaS: (05) att skapa virtuella nätverk och anslutning](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Introduktion
hello grundläggande krav för distribution av Windows Server Active Directory på Azure virtual machines skiljer sig något från distribution i lokala virtuella datorer (och toosome utsträckning, fysiska datorer). Till exempel i hello skiftläget för Windows Server AD DS, om hello-domänkontrollanter (DC) som du distribuerar på Azure virtual machines repliker i en befintlig lokal företagets domänskog och sedan hello Azure-distribution i stor utsträckning kan behandlas i hello precis som du kan hantera alla andra ytterligare Windows Server Active Directory-plats. Som är undernät måste definieras i Windows Server AD DS, en webbplats som har skapats, hello undernät länkade toothat plats och anslutna tooother platser med hjälp av lämplig platslänkar. Det finns emellertid vissa skillnader som är vanliga tooall Azure-distributioner och vissa som varierar bl.a toohello specifika distributionsscenariot. Nedan beskrivs två viktiga skillnader:

### <a name="azure-virtual-machines-may-need-connectivity-toohello-on-premises-corporate-network"></a>Virtuella Azure-datorer måste kanske anslutningen toohello lokalt företagets nätverk.
Ansluta virtuella Azure-datorer tillbaka tooan lokalt företagsnätverket kräver Azure-nätverk, som innehåller en plats-till-plats eller plats-till-punkt virtuellt privat nätverk (VPN) komponent kan tooseamlessly ansluta virtuella Azure-datorer och lokala datorer. Den här VPN-komponenten kan även aktivera lokal domän medlem datorer tooaccess en Windows Server Active Directory-domän vars domänkontrollanter finns endast på virtuella Azure-datorer. Det är viktigt toonote dock, om hello misslyckas VPN, autentisering och andra åtgärder som är beroende av Windows Server Active Directory fungerar inte. När användare inte kanske kan toosign i med befintliga cachelagrade autentiseringsuppgifter, alla peer-to-peer- eller klient-till-server autentiseringsförsök för vilka biljetter har ännu toobe eller som har blivit inaktuella misslyckas.

Se [virtuellt nätverk](http://azure.microsoft.com/documentation/services/virtual-network/) för en video demonstration och en lista med stegvisa självstudier, inklusive [konfigurera en plats-till-plats-VPN i hello Azure-portalen](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [!NOTE]
> Du kan också distribuera Windows Server Active Directory på Azure-nätverk som inte har anslutning med ett lokalt nätverk. hello riktlinjerna i det här avsnittet förutsätter dock att Azure-nätverk används eftersom det ger IP-adressering funktioner som är viktiga tooWindows Server.
> 
> 

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Statiska IP-adresser konfigureras med Azure PowerShell.
Dynamiska adresser tilldelas som standard, men Använd hello Set-AzureStaticVNetIP cmdlet tooassign statisk IP-adress i stället. Som anger en statisk IP-adress som behålls via tjänsten återställning och VM avstängning/restart. Mer information finns i [Statiska interna IP-adress för virtuella datorer](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/).

## <a name="BKMK_Glossary"></a>Termer och definitioner
hello följande är en ofullständig lista över villkor för olika Azure tekniker som kommer att referera till i den här artikeln.

* **Virtuella Azure-datorer**: hello IaaS erbjudande i Azure som gör att kunder toodeploy virtuella datorer som kör nästan alla traditionellt lokalt serverbelastning.
* **Virtuella Azure-nätverket**: hello nätverk tjänst i Azure som gör att kunderna skapa och hantera virtuella nätverk i Azure och länka dem på ett säkert sätt tootheir egna lokala nätverksinfrastruktur med hjälp av ett virtuellt privat nätverk (VPN).
* **Virtuella IP-adressen**: en internet-riktade IP-adress som är inte bunden tooa specifika dator eller ett nätverk för nätverkskortet. Molntjänster tilldelas en virtuell IP-adress för att ta emot nätverkstrafik som är omdirigerade tooan Azure VM. En virtuell IP-adress är en egenskap för en molntjänst som kan innehålla en eller flera virtuella Azure-datorer. Observera också att Azure-nätverk kan innehålla en eller flera molntjänster. Virtuella IP-adresser innehåller inbyggda funktioner för belastningsutjämning.
* **Dynamisk IP-adress**: hello IP-adress som endast är interna. Det bör konfigureras som en statisk IP-adress (med hjälp av cmdlet hello Set AzureStaticVNetIP) för virtuella datorer som är värdar för hello DC/DNS-server-roller.
* **Tjänsten återställning**: hello processen där Azure automatiskt returnerar en tjänsten tooa körs tillstånd igen när den identifierar den hello-tjänsten har avbrutits. Tjänsten återställning är en av hello aspekterna i Azure som har stöd för tillgänglighet och flexibilitet. Medan osannolik, liknande tooan oplanerad omstart hello resultatet efter en tjänst som återställning incident för en Domänkontrollant som körs på en virtuell dator, men har några sidoeffekter:
  
  * hello virtuellt nätverkskort i hello VM ändras
  * hello MAC-adress för hello virtuella nätverkskortet kommer att ändras
  * hello Processor/CPU-ID för hello VM ändras
  * hello ändras IP-konfiguration för hello virtuella nätverkskortet inte så länge hello VM är anslutna tooa virtuella nätverk och hello Virtuella datorns IP-adressen är statisk.
  
  Ingen av dessa beteenden påverkar Windows Server Active Directory eftersom det finns inga beroende hello MAC-adress eller Processor/CPU-ID och toobe som körs på Azure-nätverk som beskrivs ovan rekommenderas för alla Windows Server Active Directory-distributioner på Azure .

## <a name="is-it-safe-toovirtualize-windows-server-active-directory-domain-controllers"></a>Är det säkra toovirtualize Windows Server Active Directory-domänkontrollanter?
Distribuera Windows Server Active Directory-domänkontrollanter på Azure virtual machines är ämne toohello samma riktlinjer som körs domänkontrollanter på lokalt på en virtuell dator. Kör virtualiserade domänkontrollanter är säker så länge som riktlinjer för att säkerhetskopiera och återställa domänkontrollanter följs. Mer information om begränsningar och riktlinjer för att köra virtualiserade domänkontrollanter finns [köra Domänkontrollers i Hyper-V](https://technet.microsoft.com/library/dd363553).

Hypervisorer tillhandahåller eller trivialize tekniker som kan orsaka problem för många distribuerade system, inklusive Windows Server Active Directory. Till exempel på en fysisk server kan du klona en disk eller använda metoder som inte stöds tooroll tillbaka hello tillståndet för en server, inklusive användning av SAN-nätverk och så vidare, men gör på en fysisk server är mycket svårare än att återställa en ögonblicksbild av virtuell dator i en hypervisor. Azure erbjuder funktioner som kan medföra hello samma främmande villkor. Du bör till exempel inte kopiera VHD-filer av domänkontrollanter i stället för att utföra regelbundna säkerhetskopieringar eftersom återställa dem kan resultera i en liknande situation toousing ögonblicksbild återställningsfunktioner.

Sådana återställningar införa USN-bubblor som kan leda toopermanently avvikande tillstånd mellan domänkontrollanter. Som kan orsaka problem, till exempel:

* Kvarvarande objekt
* Inkonsekvent lösenord
* Inkonsekvent attributvärden
* Schemamatchningsfel om hello schemahanteraren återställs

Mer information om hur domänkontrollanter påverkas finns [USN och USN-återställning](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Från och med Windows Server 2012 [ytterligare skydd är inbyggda i tooAD DS](https://technet.microsoft.com/library/hh831734.aspx). Dessa skydd skydda virtualiserade domänkontrollanter mot hello ovannämnda problem, så länge hello underliggande hypervisor-plattformen stöder VM-Generationsid. Azure stöder VM-Generationsid, vilket innebär att domänkontrollanter som kör Windows Server 2012 eller senare på Azure virtuella datorer har hello ytterligare skydd.

> [!NOTE]
> Du bör avsluta och starta om en virtuell dator som kör hello domänkontrollant i Azure hello gästoperativsystem istället för att använda hello **avsluta** alternativ i hello Azure får. Idag gör med hello portal tooshut ned en virtuell dator hello VM toobe frigjorts. En frigjord virtuell dator har hello utnyttja inte medför kostnader, men också återställer hello VM-GenerationID som är önskvärt för en Domänkontrollant. När hello VM-GenerationID återställs hello invocationID för hello AD DS-databasen återställs även, hello RID-poolen förkastas och SYSVOL är markerad som icke-auktoritativ. Mer information finns i [introduktion tooActive virtualisering Directory Domain Services (AD DS)](https://technet.microsoft.com/library/hh831734.aspx) och [säker virtualisering av DFSR](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).
> 
> 

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Varför distribuerar Windows Server AD DS på virtuella datorer i Azure?
Många scenarier för distribution av Windows Server AD DS lämpar sig väl för distribution som virtuella datorer i Azure. Anta att du har ett företag i Europa som behöver tooauthenticate användare i en annan plats i Asien. hello företag inte har tidigare distribuerat Windows Server Active Directory-domänkontrollanter i Asien på grund av toohello kostnad toodeploy dem och begränsade expertis toomanage hello servrar efter distributionen. Därmed behandlas autentiseringsbegäranden från Asien av domänkontrollanter i Europa med något sämre resultat. I det här fallet kan du distribuera en Domänkontrollant på en virtuell dator som du har angett måste köras inom hello Azure-datacenter i Asien. Bifoga den DC tooan virtuella Azure-nätverket som är anslutet direkt toohello fjärrplatsen kommer att förbättra prestanda.

Azure passar också bra som en substitute toootherwise kostsamma (DR) katastrofåterställningsplatser. hello relativt billig vara värd för ett litet antal domänkontrollanter och ett enda virtuellt nätverk i Azure representerar ett bra alternativ.

Slutligen kan du toodeploy ett nätverksprogram i Azure, t.ex SharePoint, som kräver Windows Server Active Directory men har inga beroende hello lokala nätverk eller hello företagets Windows Server Active Directory. I det här fallet distribuera en isolerad skog i Azure toomeet hello SharePoint-serverns krav är optimalt. Igen och stöds distribuera nätverksprogram som kräver anslutning toohello lokala nätverk och hello företagets Active Directory också.

> [!NOTE]
> Eftersom det ger en nivå 3-anslutning hello VPN-komponent som tillhandahåller anslutningen mellan Azure-nätverk och ett lokalt nätverk kan också aktivera medlemsservrar som kör lokalt tooleverage domänkontrollanter som körs som virtuella Azure-datorer i Azure virtuella nätverk. Men om hello VPN är tillgänglig, kommunikation mellan lokala datorer och Azure-baserade domänkontrollanter kommer inte att fungera, vilket resulterar i autentisering och andra fel.  
> 
> 

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Skiljer från mellan distribution av Windows Server Active Directory-domänkontrollanter på Azure Virtual Machines jämfört med lokalt
* För alla Windows Server Active Directory-distributionsscenariot som innehåller fler än en enda virtuell dator, är det nödvändigt toouse virtuellt Azure-nätverk för konsekvens för IP-adress. Observera att den här guiden förutsätter att domänkontrollanter körs på Azure-nätverk.
* Precis som med lokala domänkontrollanter, rekommenderas statiska IP-adresser. En statisk IP-adress kan endast konfigureras med hjälp av Azure PowerShell. Se [Statiska interna IP-adress för virtuella datorer](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) för mer information. Om du har övervakning system eller andra lösningar som kontrollerar för statisk IP-adresskonfiguration hello gästoperativsystem kan tilldela du hello samma statiska IP-adress toohello egenskaper för nätverkskort för hello VM. Men tänk på att hello nätverkskort ignoreras om hello VM genomgår tjänsten återställning eller stängs av i hello portal och har frigjorts dess adress. I så fall hello statisk IP-adress gästen hello behöver toobe återställa.
* Distribuera virtuella datorer på ett virtuellt nätverk inte innebär (eller kräver) anslutning tillbaka tooan lokalt nätverk. hello virtuellt nätverk kan bara möjligheter. Du måste skapa ett virtuellt nätverk för privata kommunikation mellan Azure och ditt lokala nätverk. Du måste toodeploy en VPN-slutpunkten på hello lokalt nätverk. hello VPN öppnas från Azure toohello lokalt nätverk. Mer information finns i [översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md) och [konfigurera en plats-till-plats-VPN i hello Azure Portal](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [!NOTE]
> Ett alternativ för[skapa en punkt-till-plats-VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) är tillgängliga tooconnect enskilda Windows-baserade datorer direkt tooan virtuella Azure-nätverket.
> 
> 

* Oavsett om du skapar en virtuell nätverks- eller inte, Azure kostnader för utgående trafik, men inte ingång. Olika alternativ för Windows Server Active Directory-designen kan påverka hur mycket utgående trafik som genereras av en distribution. Till exempel begränsar distribuera en skrivskyddad domänkontrollant (RODC) utgående trafik eftersom den inte replikeras utgående. Men hello beslut toodeploy en RODC måste toobe vägas mot hello måste tooperform skrivåtgärder mot hello DC och hello [kompatibilitet](https://technet.microsoft.com/library/cc755190) att program och tjänster i hello platsen har med skrivskyddade domänkontrollanter. Mer information om trafik avgifter finns [Azure priser på snabb](http://azure.microsoft.com/pricing/).
* När du har fullständig kontroll över vilken server resurser toouse för virtuella datorer lokalt, till exempel RAM-minne, diskens storlek och så vidare på Azure måste du välja från en lista med förinställda server. För en Domänkontrollant krävs en datadisk i tillägg toohello operativsystemdisk i ordning toostore hello Windows Server Active Directory-databasen.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Kan du distribuera Windows Server AD FS på virtuella Azure-datorer?
Ja, du kan distribuera Windows Server AD FS på Azure virtual machines och hello [bästa praxis för AD FS-distribution](https://technet.microsoft.com/library/dn151324.aspx) lokalt gälla tooAD FS-distribution på Azure. Men hello bästa praxis som belastningsutjämning och hög tillgänglighet kräver tekniker utöver vad AD FS erbjuder sig själv. De måste tillhandahållas av hello underliggande infrastruktur. Nu ska vi gå igenom några av de bästa praxis och se hur de kan uppnås genom att använda virtuella Azure-datorer och Azure-nätverk.

1. **Aldrig exponera säkerhet token-tjänsten (STS) servrar direkt toohello Internet.**
   
    Detta är viktigt eftersom hello STS utfärdar säkerhetstoken. Därför STS-servrar som AD FS-servrarna ska behandlas med hello samma nivå av skydd som en domänkontrollant. Om en STS äventyras, har skadliga användare hello möjlighet tooissue åtkomsttoken som potentiellt innehåller anspråk av sina program för att välja toorelying från leverantörer och andra STS-servrar i betrodda organisationer.
2. **Distribuera Active Directory-domänkontrollanter för alla användardomäner i samma nätverk som hello AD FS-servrar hello.**
   
    AD FS-servrar använder Active Directory Domain Services tooauthenticate användare. Det rekommenderas toodeploy domänkontrollanter på hello samma nätverk som hello AD FS-servrarna. Detta ger affärskontinuitet om hello länken mellan hello Azure-nätverk och ditt lokala nätverk har brutits och aktiverar kortare svarstid och bättre prestanda för inloggningar.
3. **Distribuera flera AD FS-noder för hög tillgänglighet och belastningsutjämning hello.**
   
    I de flesta fall är hello fel i ett program som gör att AD FS acceptabelt eftersom hello-program som kräver att säkerhetstoken är ofta uppdragskritiska. Därför och eftersom AD FS finns nu i hello kritiska tooaccessing verksamhetskritiska program, hello AD FS-tjänsten måste ha hög tillgänglighet via flera AD FS-proxyservrarna och AD FS-servrarna. tooachieve distribution av förfrågningar, belastningsutjämnare distribueras vanligtvis framför både hello AD FS-proxyservrar och hello AD FS-servrar.
4. **Distribuera en eller flera Web Application Proxy-noder för internet-åtkomst.**
   
    När användare behöver tooaccess program som skyddas av hello AD FS-tjänsten, hello hello AD FS-tjänsten måste toobe tillgänglig från internet. Detta uppnås genom att distribuera hello Web Application Proxy-tjänsten. Det rekommenderas starkt toodeploy mer än en nod för hello hög tillgänglighet och belastningsutjämning.
5. **Begränsa åtkomst från hello Web Application Proxy noder toointernal nätverksresurser.**
   
    tooallow externa användare tooaccess AD FS från hello internet måste du toodeploy Web Application Proxy noder (eller AD FS-Proxy i tidigare versioner av Windows Server). Hej webbprogramproxy noder är direkt exponerade toohello Internet. De är inte obligatoriska toobe domänanslutna och de bara behöver åtkomst till toohello AD FS-servrar via TCP-portarna 443 och 80. Vi rekommenderar starkt att kommunikation tooall andra datorer (särskilt domänkontrollanter) är blockerad.
   
    Detta är vanligtvis uppnås lokalt med hjälp av en DMZ. Brandväggar används ett godkända läge för åtgärden toorestrict trafik från hello DMZ toohello lokalt nätverk (det vill säga endast trafik från hello anges IP-adresser och över anges portar är tillåtna, och alla andra trafik blockeras).

hello följande diagram visar traditionella lokala AD FS-distribution.

![Diagram över en traditionella lokala Active Directory Federation Services-distribution](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Eftersom Azure inte tillhandahåller kapaciteten för interna, kompletta brandväggen, måste andra alternativ toobe används toorestrict trafik. hello följande tabell visas varje alternativ och dess fördelar och nackdelar.

| Alternativ | Nytta | Nackdelen |
| --- | --- | --- |
| [ACL: er för Azure-nätverk](../virtual-network/virtual-networks-acl.md) |Mindre kostsamma och enklare inledande konfiguration |Ytterligare ACL nätverkskonfigurationen krävs om eventuella nya virtuella datorer läggs toohello distribution |
| [Barracuda NG brandväggen](https://www.barracuda.com/products/ngfirewall) |Godkända läge för åtgärden och kräver ingen nätverket ACL-konfiguration |Ökad kostnad och komplexitet för installationen |

hello anvisningar toodeploy AD FS i det här fallet är:

1. Skapa ett virtuellt nätverk med korsanslutningar, med hjälp av antingen en VPN-anslutning eller [ExpressRoute](http://azure.microsoft.com/services/expressroute/).
2. Distribuera domänkontrollanter på hello virtuellt nätverk. Det här steget är valfritt men rekommenderas.
3. Distribuera domänanslutna AD FS-servrarna på hello virtuellt nätverk.
4. Skapa en [interna belastningsutjämnade uppsättningen](http://azure.microsoft.com/blog/internal-load-balancing/) som inkluderar hello AD FS-servrar och använder en ny privat IP-adress inom hello virtuellt nätverk (dynamisk IP-adress).
   
   1. Uppdatera toocreate hello FQDN toopoint toohello privata (dynamisk) IP-adressen för DNS-hello interna belastningsutjämnade uppsättningen.
5. Skapa en tjänst i molnet (eller ett separat virtuellt nätverk) för hello Web Application Proxy-noder.
6. Distribuera hello Web Application Proxy-noder i hello molnbaserad tjänst eller ett virtuellt nätverk
   
   1. Skapa en extern belastningsutjämnad uppsättning som innehåller hello Web Application Proxy-noder.
   2. Uppdatera hello externa DNS-namn (FQDN) toopoint toohello cloud service offentlig IP-adress (hello virtuella IP-adress).
   3. Konfigurera AD FS-proxyservrarna toouse hello FQDN som motsvarar toohello interna belastningsutjämnad uppsättning för hello AD FS-servrarna.
   4. Uppdatera anspråksbaserad webbplatser toouse hello externa FQDN för sina anspråksleverantör.
7. Begränsa åtkomsten mellan Web Application Proxy tooany dator i hello AD FS virtuella nätverk.

toorestrict trafik hello belastningsutjämnad uppsättning för hello Azure intern belastningsutjämnare måste toobe som konfigurerats för endast trafik tooTCP portarna 80 och 443 och alla andra trafik toohello interna dynamisk IP-adress hello belastningsutjämnade uppsättningen har släppts.

![Diagram över ACL: er i AD FS nätverk med TCP 443 + 80 tillåts.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

Trafik toohello AD FS-servrarna skulle vara tillåten endast av hello följande källor:

* hello Azure intern belastningsutjämnare.
* hello IP-adressen för en administratör på hello lokalt nätverk.

> [!WARNING]
> förhindra Web Application Proxy-noder måste hello design når alla andra virtuella datorer i hello Azure-nätverk eller alla platser i hello lokalt nätverk. Som kan göras genom att konfigurera brandväggsregler i hello lokala enhet för Express Route-anslutningar eller hello VPN-enhet för plats-till-plats VPN-anslutningar.
> 
> 

En nackdel toothis alternativet är hello måste tooconfigure hello nätverk ACL: er för flera enheter, inklusive intern belastningsutjämnare, hello AD FS-servrar och andra servrar som läggs toohello virtuellt nätverk. Om en enhet läggs toohello distribution utan att konfigurera nätverket ACL: er toorestrict trafik tooit, kan hello hela distributionen vara i fara. Ändrar hello IP-adresser för hello Web Application Proxy noder någonsin, hello nätverket ACL: er måste återställas (vilket innebär att hello proxyservrar måste vara konfigurerade toouse [Statiska IP-adresser som dynamiska](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![AD FS på Azure med nätverket ACL: er.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Ett annat alternativ är toouse hello [Barracuda NG brandväggen](https://www.barracuda.com/products/ngfirewall) installation toocontrol trafik mellan AD FS-proxyservrar och hello AD FS-servrarna. Det här alternativet uppfyller bästa praxis för säkerhet och hög tillgänglighet och kräver mindre administration efter hello installationen eftersom hello Barracuda NG brandväggsinstallation ger en godkänd läge för brandväggen administration och den kan installeras direkt på Azure-nätverk. Som eliminerar hello måste tooconfigure nätverk ACL: er varje gång en ny server läggs toohello distribution. Men det här alternativet ökar första distributionen komplexiteten och kostnaden.

I det här fallet distribueras två virtuella nätverk i stället för en. Vi kallar dem VNet1 och VNet2. VNet1 innehåller hello proxyservrar och VNet2 innehåller hello STSs och hello nätverk anslutning tillbaka toohello företagets nätverk. VNet1 är därför fysiskt (men praktiskt taget) isolerade från VNet2 och i sin tur från hello företagsnätverket. VNet1 sedan anslutna tooVNet2 använder en särskild tunneling teknik som heter som Transport oberoende nätverk arkitektur (TINA). hello TINA tunnel är anslutna tooeach hello virtuella nätverk med hjälp av en brandvägg Barracuda NG – en Barracuda på varje hello virtuella nätverk.  För hög tillgänglighet rekommenderar vi att du distribuerar två Barracuda på varje virtuellt nätverk; en aktiv hello andra passiv. De erbjuder mycket omfattande firewalling funktioner som möjliggör toomimic hello driften av en traditionella lokala DMZ i Azure.

![AD FS på Azure med brandväggen.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Mer information finns i [AD FS: utöka en anspråksmedvetna lokalt klientprogrammet toohello Internet](#BKMK_CloudOnlyFed).

### <a name="an-alternative-tooad-fs-deployment-if-hello-goal-is-office-365-sso-alone"></a>En alternativ tooAD FS-distribution om hello målet är Office 365 SSO enbart
Det finns ett annat alternativ toodeploying AD FS helt om målet är endast tooenable inloggning för Office 365. I så fall kan du bara distribuera DirSync med lösenord synkronisering lokalt och uppnå samma slutresultatet hello med minimal distribution komplexitet, eftersom den här metoden inte kräver AD FS eller Azure.

hello följande tabell jämförs hur hello inloggning processer fungerar med och utan att distribuera AD FS.

| Office 365 enkel inloggning med AD FS och DirSync | Office 365 samma inloggning med DirSync + Lösenordssynkronisering |
| --- | --- |
| 1. hello användare loggar in tooa företagsnätverket och är autentiserad tooWindows Server Active Directory. |1. hello användare loggar in tooa företagsnätverket och är autentiserad tooWindows Server Active Directory. |
| 2. hello användare försöker tooaccess Office 365 (jag @contoso.com). |2. hello användare försöker tooaccess Office 365 (jag @contoso.com). |
| 3. Office 365 omdirigerar hello användaren tooAzure AD. |3. Office 365 omdirigerar hello användaren tooAzure AD. |
| 4. Eftersom Azure AD kan autentisera hello användare och förstår att det finns ett förtroende med AD FS på lokalt, omdirigerar hello användaren tooAD FS. |4. Azure AD kan inte acceptera Kerberos-biljetter direkt och det finns ingen betrodd relation så begär hello användaren ange autentiseringsuppgifter. |
| 5. hello användare skickar en Kerberos-biljett toohello AD FS STS. |5. hello användaren anger hello samma lokala lösenord och Azure AD verifierar dem mot hello användarnamn och lösenord som synkroniserades av DirSync. |
| 6. AD FS transformerar hello som krävs för Kerberos-biljetten toohello token format/anspråk och omdirigerar hello användaren tooAzure AD. |6. Azure AD omdirigerar hello användaren tooOffice 365. |
| 7. hello användaren autentiseras tooAzure AD (omvandling av en annan sker). |7. hello användaren kan logga in tooOffice 365 och OWA använder hello Azure AD-token. |
| 8. Azure AD omdirigerar hello användaren tooOffice 365. | |
| 9. hello användaren loggas tyst på tooOffice 365. | |

I hello Office 365 med DirSync med scenario för Lösenordssynkronisering (ingen AD FS) ersättas enkel inloggning med ”samma inloggning” där ”samma” innebär bara att användare måste ange sina autentiseringsuppgifter för samma lokala igen när du använder Office 365. Observera att dessa data kan registreras av hello användarens webbläsare toohelp minska efterföljande frågor.

### <a name="additional-food-for-thought"></a>Ytterligare mat for idé
* Om du distribuerar en AD FS-proxy på en virtuell Azure-dator, krävs anslutning toohello AD FS-servrarna. Om de finns lokalt, bör du använda hello plats-till-plats VPN-anslutningen som tillhandahålls av hello virtuellt nätverk tooallow hello Web Application Proxy noder toocommunicate med deras AD FS-servrarna.
* Om du distribuerar AD FS-servern på en virtuell Azure, anslutning tooWindows Server Active Directory-domänkontrollanter, Attributarkiv, och databaserna konfiguration krävs och kan också kräva en ExpressRoute eller en plats-till-plats VPN-anslutning mellan hello Azure virtuella nätverk och hello lokalt nätverk.
* Avgifter tillämpas tooall trafik från virtuella Azure-datorer (utgående trafik). Om kostnaden är drivande hello-faktor, är det lämpligt toodeploy hello Web Application Proxy-noder i Azure, lämnar hello AD FS-servrar lokalt. Om hello AD FS-servrar distribueras på virtuella Azure-datorer samt att ytterligare kostnader verkställda tooauthenticate lokala användare. Utgående trafik innebär en kostnad oavsett om huruvida den går igenom hello ExpressRoute eller hello VPN plats-till-plats-anslutning.
* Om du väljer toouse Azure interna server belastningsutjämning för hög tillgänglighet i AD FS-servrarna, Observera att nätverksbelastning ger avsökningar som används toodetermine hello hälsotillstånd hello virtuella datorer i hello-Molntjänsten. Hello gäller virtuella Azure-datorer (som skillnad från tooweb eller worker roller), en anpassad avsökning användas eftersom hello-agent som svarar toohello standard avsökningar inte finns på virtuella Azure-datorer. Du kan använda en anpassad TCP-avsökning för enkelhetens skull – detta endast kräver att en TCP-anslutning (ett TCP SYN segment skickas och svarade toowith ett TCP SYN ACK-segment) har upprättats toodetermine virtuella hälsa. Du kan konfigurera hello anpassad avsökningsåtgärd toouse alla TCP-port toowhich lyssnar aktivt dina virtuella datorer.

> [!NOTE]
> Datorer som behöver tooexpose hello samma uppsättning portar direkt toohello Internet (till exempel port 80 och 443) inte kan dela hello samma tjänst i molnet. Därför rekommenderas att du skapar en dedikerad molntjänst för alla Windows Server AD FS-servrarna i ordning tooavoid potentiella överlappar mellan krav på nätverksportar för ett program och Windows Server AD FS.
> 
> 

## <a name="deployment-scenarios"></a>Distributionsscenarier
hello följande avsnitt beskriver tar över som standard distribution scenarier toodraw uppmärksamhet tooimportant överväganden som måste beaktas. Varje scenario har länkar toomore information om hello beslut och faktorer tooconsider.

1. [AD DS: Distribuera ett AD DS-medvetna program med inga krav på företagets nätverksanslutning](#BKMK_CloudOnly)
   
    Till exempel distribueras en Internet-riktade SharePoint-tjänsten på en virtuell Azure-dator. hello-programmet har inga beroenden på företagets nätverksresurser. hello program kräver Windows Server AD DS men kräver inte hello företagets Windows Server AD DS.
2. [AD FS: Utöka en anspråksmedvetna lokalt klientprogrammet toohello Internet](#BKMK_CloudOnlyFed)
   
    Till exempel måste ett anspråksmedvetet program som har distribuerats lokalt och används av företagsanvändare toobecome tillgänglig från hello Internet. programmet hello måste toobe nås direkt via hello Internet efter affärspartner som använder sina egna företagets identiteter och efter befintliga företagsanvändare.
3. [AD DS: Distribuera en Windows Server AD DS-medvetna program som kräver anslutning toohello företagsnätverket](#BKMK_HybridExt)
   
    Till exempel distribueras ett LDAP-medvetna program som stöder Windows-integrerad autentisering och använder Windows Server AD DS som en lagringsplats för konfiguration och användarprofil data på en virtuell Azure-dator. Det är önskvärt hello programmet tooleverage hello befintliga företagets Windows Server AD DS och tillhandahålla enkel inloggning. hello programmet är inte medveten om anspråket.

### <a name="BKMK_CloudOnly"></a>1. AD DS: Distribuera ett AD DS-medvetna program med inga krav på företagets nätverksanslutning
![Endast molnbaserad AD DS-distributionen](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**bild 1**

#### <a name="description"></a>Beskrivning
SharePoint har distribuerats på en virtuell Azure-dator och hello programmet har inga beroenden på företagets nätverksresurser. hello programmet kräver Windows Server AD DS men *inte* kräver hello företagets Windows Server AD DS. Inga Kerberos eller federerade förtroenden krävs eftersom användare kan själva etablerade via hello program i hello Windows Server AD DS-domän som även är värd för hello molnet på virtuella Azure-datorer.

#### <a name="scenario-considerations-and-how-technology-areas-apply-toohello-scenario"></a>Överväganden för scenariot och hur teknikområden tillämpa toohello scenario
* [Nätverkstopologi](#BKMK_NetworkTopology): skapa en Azure-nätverk utan korsanslutningar (även kallat plats-till-plats-anslutning).
* [DC distributionskonfiguration](#BKMK_DeploymentConfig): distribuera en ny domänkontrollant i en ny, domän, Windows Server Active Directory-skog. Detta bör distribueras tillsammans med hello Windows DNS-server.
* [Windows Server Active Directory-platstopologi](#BKMK_ADSiteTopology): Använd hello standard Windows Server Active Directory-plats (alla datorer är i standard-First-Site-Name).
* [IP-adresser och DNS-](#BKMK_IPAddressDNS):
  
  * Ange en statisk IP-adress för hello DC med hello Set AzureStaticVNetIP Azure PowerShell-cmdlet.
  * Installera och konfigurera Windows Server DNS på hello domänkontrollers på Azure.
  * Konfigurera egenskaper för hello virtuella nätverk med hello namn och IP-adressen för hello virtuell dator som är värd för hello Domänkontrollant och DNS-server-roller.
* [Global katalog](#BKMK_GC): hello första domänkontrollanten i skogen hello måste vara en global katalogserver. Ytterligare domänkontrollanter bör också konfigureras som global katalog eftersom i en enda domän i skogen hello global katalog inte kräver några ytterligare arbete från hello DC.
* [Placering av hello Windows Server AD DS-databasen och SYSVOL](#BKMK_PlaceDB): lägga till en tooDCs för data-disk som körs som virtuella Azure-datorer i ordning toostore hello Windows Server Active Directory-databasen, loggar och SYSVOL.
* [Säkerhetskopiera och återställa](#BKMK_BUR): Bestäm var du vill att toostore säkerhetskopiering av systemtillståndet. Lägg till en annan disk toohello DC VM toostore säkerhetskopiering av data om det behövs.

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS: utöka en anspråksmedvetna lokalt klientprogrammet toohello Internet
![Federation med korsanslutningar](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**bild 2**

#### <a name="description"></a>Beskrivning
Ett anspråksmedvetet program som har distribuerats lokalt och används av företagsanvändare behov toobecome tillgänglig direkt från hello Internet. hello program fungerar som en webbtjänst klientdel tooa SQL-databas som den lagrar data. hello SQL-servrar som används av programmet hello finns också i hello företagsnätverk. Två Windows Server AD FS STSs och en belastningsutjämnare varit distribuerade lokalt tooprovide åtkomst toohello företagsanvändare. hello programmet nu måste toobe dessutom komma åt direkt via hello Internet efter affärspartner som använder sina egna företagets identiteter och efter befintliga företagsanvändare.

I en ansträngning toosimplify och uppfyller hello distribution och konfiguration av behoven hos det nya kravet valt två ytterligare web frontends och två Windows Server AD FS-proxyservrar installeras på virtuella Azure-datorer. Alla fyra virtuella datorer kommer att visas direkt toohello Internet och kommer att tillhandahållas på anslutningen toohello lokalt nätverk med Azure Virtual Network plats-till-plats VPN-funktionen.

#### <a name="scenario-considerations-and-how-technology-areas-apply-toohello-scenario"></a>Överväganden för scenariot och hur teknikområden tillämpa toohello scenario
* [Nätverkstopologi](#BKMK_NetworkTopology): skapa en Azure-nätverk och [konfigurera korsanslutningar](../vpn-gateway/vpn-gateway-site-to-site-create.md).
  
  > [!NOTE]
  > Kontrollera att hello URL definierats inom hello certifikatmall och hello resulterande certifikaten kan nås av hello Windows Server AD FS-instanser som körs på Azure för varje hello Windows Server AD FS-certifikat. Det här kan kräva anslutningar mellan lokala anslutningen tooparts av PKI-infrastrukturen. För exempel om hello CRL-slutpunkt är LDAP-baserade och värdbaserade uteslutande lokalt och sedan korsanslutningar kommer att krävas. Om detta inte är önskvärt kanske nödvändiga toouse certifikat som utfärdats av en Certifikatutfärdare vars CRL är tillgänglig via hello Internet.
  > 
  > 
* [Cloud services-konfiguration](#BKMK_CloudSvcConfig): Kontrollera att du har två molntjänster i ordning tillhandahåller två belastningsutjämnade virtuella IP-adresser. hello första molnet tjänstens virtuella IP-adressen kommer att dirigerad toohello två Windows Server AD FS-proxy virtuella datorer på portarna 80 och 443. hello konfigureras Windows Server AD FS-proxy virtuella datorer kommer att toopoint toohello IP-adressen för hello lokalt belastningsutjämnare som rör hello STSs för Windows Server AD FS. hello andra molntjänster tjänstens virtuella IP-adressen kommer att dirigerad toohello två virtuella datorer körs hello web klientdel igen på portarna 80 och 443. Konfigurera en anpassad avsökning tooensure hello belastningsutjämnare endast dirigerar trafik toofunctioning Windows Server AD FS-proxy och web klientdel virtuella datorer.
* [Konfigurationen av federationsservern](#BKMK_FedSrvConfig): Konfigurera Windows Server AD FS för federation server (STS) toogenerate säkerheten säkerhetstoken för hello Windows Server Active Directory-skog som skapats i hello molnet. Konfigurera förtroenden för förlitande part med hello olika program som du vill toogenerate-tokens för att ange anspråk providern federationsförtroenden med olika hello partner som du vill tooaccept identiteter från.
  
    I de flesta fall distribueras Windows Server AD FS-proxyservrar i en Internet-riktade kapacitet av säkerhetsskäl samtidigt som deras motsvarigheter i Windows Server AD FS federationstjänsten förbli isolerad från direkt Internetanslutning. Oavsett distributionsscenariot, måste du konfigurera din molntjänst med en virtuell IP-adress som ger en offentligt exponerade IP-adressen och porten som kan tooload saldo hela antingen två Windows Server AD FS STS instanser eller proxy instanser.
* [Windows Server AD FS-konfiguration för hög tillgänglighet](#BKMK_ADFSHighAvail): det är lämpligt toodeploy en Windows Server AD FS-servergrupp med minst två servrar för redundans och belastningsutjämning. Du kanske vill tooconsider med hello Windows interna databas (WID) för Windows Server AD FS-konfigurationsdata och använder hello intern belastningsutjämning möjligheterna för Azure toodistribute inkommande begäranden mellan hello servrar i servergruppen hello.

Mer information finns i hello [Distributionsguide](https://technet.microsoft.com/library/cc753963).

### <a name="BKMK_HybridExt"></a>3. AD DS: Distribuera en Windows Server AD DS-medvetna program som kräver anslutning toohello företagsnätverket
![Anslutningar mellan lokala AD DS-distributionen](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**bild 3**

#### <a name="description"></a>Beskrivning
LDAP-medvetna program distribueras på en virtuell Azure-dator. Den stöder Windows-integrerad autentisering och använder Windows Server AD DS som en lagringsplats för konfiguration och användaren profildata. hello målet är hello programmet tooleverage hello befintliga företagets Windows Server AD DS och tillhandahålla enkel inloggning. hello programmet är inte medveten om anspråket. Användarna måste också tooaccess hello program direkt från hello Internet. toooptimize för prestanda och kostnad beslutat att två ytterligare domänkontrollanter som är en del av hello företagsdomän distribueras tillsammans med programmet hello på Azure.

#### <a name="scenario-considerations-and-how-technology-areas-apply-toohello-scenario"></a>Överväganden för scenariot och hur teknikområden tillämpa toohello scenario
* [Nätverkstopologi](#BKMK_NetworkTopology): skapa en Azure-nätverk med [mellan lokal anslutning](../vpn-gateway/vpn-gateway-site-to-site-create.md).
* [Installationsmetoden](#BKMK_InstallMethod): distribuera replik domänkontrollanter från Windows Server Active Directory hello företagsdomän. För en replikdomänkontrollant kan du installera Windows Server AD DS på hello VM och du kan också använda hello installera från Media (IFM) funktionen tooreduce hello mängden data som behöver toobe replikeras toohello ny DC under installationen. En självstudiekurs finns [installerar en replik Active Directory-domänkontrollant på Azure](active-directory-install-replica-active-directory-domain-controller.md). Även om du använder IFM, kan det vara effektivare toobuild hello virtuella domänkontrollanter lokalt och flytta hello hela virtuell hårddisk (VHD) toohello moln istället för att replikera Windows Server AD DS under installationen. För säkerhet rekommenderas det att du tar bort hello VHD från hello lokala nätverk när det har varit kopierade tooAzure.
* [Windows Server Active Directory-platstopologi](#BKMK_ADSiteTopology): skapa en ny Azure-webbplats i Active Directory-platser och tjänster. Skapa en Windows Server Active Directory-undernät objektet toorepresent hello virtuella Azure-nätverket och Lägg till hello undernät toohello plats. Skapa en ny platslänk som innehåller hello nya Azure och hello plats i vilka hello virtuella Azure-nätverket VPN-slutpunkten finns i ordning toocontrol och optimera tooand för Windows Server Active Directory-trafik från Azure.
* [IP-adresser och DNS-](#BKMK_IPAddressDNS):
  
  * Ange en statisk IP-adress för hello DC med hello Set AzureStaticVNetIP Azure PowerShell-cmdlet.
  * Installera och konfigurera Windows Server DNS på hello domänkontrollers på Azure.
  * Konfigurera egenskaper för hello virtuella nätverk med hello namn och IP-adressen för hello virtuell dator som är värd för hello Domänkontrollant och DNS-server-roller.
* [Fördelade domänkontrollanter](#BKMK_DistributedDCs): konfigurera ytterligare virtuella nätverk efter behov. Om din Active Directory-platstopologi kräver domänkontrollanter i geografiska områden som motsvarar toodifferent Azure regioner än du därefter vill toocreate Active Directory-platser.
* [Skrivskyddade domänkontrollanter](#BKMK_RODC): du kan distribuera en RODC i hello Azure site beroende på dina krav för att utföra skrivåtgärder mot hello DC och hello kompatibilitet för program och tjänster på hello plats med skrivskyddade domänkontrollanter. Mer information om programkompatibilitet finns hello [guide för skrivskyddade domänkontrollanter kompatibilitet](https://technet.microsoft.com/library/cc755190).
* [Global katalog](#BKMK_GC): global katalog är nödvändiga tooservice inloggningsbegäranden i flera domäner skogar. Om du inte distribuera en GC i hello Azure site påförs kostnader för utgående trafik som autentiseringsbegäranden orsaka frågor GC-generationer på andra platser. Du kan aktivera cachelagring för hello Azure plats i Active Directory-platser och tjänster medlemskap i universell grupp toominimize som trafik.
  
    Om du distribuerar en GC konfigurera platslänkar och kostnader platslänkar så att hello GC i hello Azure site inte rekommenderas som en källdomänkontrollant av andra GC-generationer som behöver tooreplicate hello samma delvis partitioner.
* [Placering av hello Windows Server AD DS-databasen och SYSVOL](#BKMK_PlaceDB): lägga till en tooDCs för data-disk som körs på virtuella Azure-datorer i ordning toostore hello Windows Server Active Directory-databasen, loggar och SYSVOL.
* [Säkerhetskopiera och återställa](#BKMK_BUR): Bestäm var du vill att toostore säkerhetskopiering av systemtillståndet. Lägg till en annan disk toohello DC VM toostore säkerhetskopiering av data om det behövs.

## <a name="deployment-decisions-and-factors"></a>Distributionsbeslut och faktorer
Den här tabellen sammanfattas hello Windows Server Active Directory-teknikområden som påverkas i hello föregående scenarier och hello motsvarande beslut tooconsider med länkar toomore detalj nedan. Vissa teknikområden kanske inte är tillämpliga tooevery distributionsscenariot och vissa teknikområden kan vara mer kritiska tooa distributionsscenariot än andra teknikområden.

Till exempel om du distribuerar en replikdomänkontrollant på ett virtuellt nätverk och din skog har bara en enda domän, sedan välja toodeploy en global katalogserver i så fall inte kritiska toohello distributionsscenariot eftersom den inte skapar någon ytterligare replikering krav. På hello däremot om hello skogen har flera domäner och sedan hello beslut toodeploy en global katalog på ett virtuellt nätverk kan påverka tillgänglig bandbredd, prestanda, autentisering, katalogen och så vidare.

| Windows Server Active Directory teknikområde | Beslut | Faktorer |
| --- | --- | --- |
| [Nätverkstopologi](#BKMK_NetworkTopology) |Skapar du ett virtuellt nätverk? |<li>Krav för tooaccess Corp-resurser</li> <li>Autentisering</li> <li>Kontohantering</li> |
| [DC distributionskonfiguration](#BKMK_DeploymentConfig) |<li>Distribuera en separat skog utan några förtroenden?</li> <li>Distribuera en ny skog med federation?</li> <li>Distribuera en ny skog med Windows Server Active Directory skogsförtroende eller Kerberos?</li> <li>Utöka Corp-skogen genom att distribuera en replikdomänkontrollant?</li> <li>Utöka Corp-skogen genom att distribuera en ny underordnad domän eller ett domänträd?</li> |<li>Säkerhet</li> <li>Efterlevnad</li> <li>Kostnad</li> <li>Återhämtning och feltolerans</li> <li>Programkompatibilitet</li> |
| [Windows Server Active Directory-platstopologi](#BKMK_ADSiteTopology) |Hur du för att konfigurera undernät, platser och platslänkar med Azure Virtual Network toooptimize trafik och minimerar kostnaden? |<li>Definitioner av undernät och plats</li> <li>Plats länkegenskaper och ändringsmeddelande</li> <li>Replikering komprimering</li> |
| [IP-adresser och DNS](#BKMK_IPAddressDNS) |Hur tooconfigure IP-adresser och namnmatchning? |<li>Använd hello Använd hello Set-AzureStaticVNetIP cmdlet tooassign statisk IP-adress</li> <li>Installera Windows Server DNS-servern och konfigurera hello egenskaper för virtuellt nätverk med hello namn och IP-adressen för hello virtuell dator som är värd för hello Domänkontrollant och DNS-server-roller</li> |
| [Fördelade domänkontrollanter](#BKMK_DistributedDCs) |Hur tooreplicate tooDCs på separata virtuella nätverk? |Om din Active Directory-platstopologi kräver domänkontrollanter i geografiska områden som motsvarar toodifferent Azure regioner än du därefter vill toocreate Active Directory-platser. [Konfigurera virtuella nätverk toovirtual network Connectivity](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) tooreplicate mellan domänkontrollanter på separata virtuella nätverk. |
| [Skrivskyddade domänkontrollanter](#BKMK_RODC) |Använda skrivskyddade eller skrivbara domänkontrollanter? |<li>Filtrera HBI/PII attribut</li> <li>Filtrera hemligheter</li> <li>Gränsen för utgående trafik</li> |
| [Global katalog](#BKMK_GC) |Installera global katalog? |<li>För enkel domänskog, se alla domänkontrollanter global katalog</li> <li>För skog krävs GC-generationer för autentisering</li> |
| [Installationsmetod](#BKMK_InstallMethod) |Hur tooinstall DC i Azure? |Antingen: <li>Installera AD DS med Windows PowerShell eller Dcpromo</li> <li>Flytta virtuell Hårddisk på en lokal virtuell Domänkontrollant</li> |
| [Placering av hello Windows Server AD DS-databasen och SYSVOL](#BKMK_PlaceDB) |Om toostore Windows Server AD DS-databasen, loggar och SYSVOL? |Ändra Dcpromo.exe standardvärden. Dessa viktiga Active Directory-filer *måste* placeras på Azure Datadiskar i stället för operativsystemet diskar som implementerar skrivcache. |
| [Säkerhetskopiering och återställning](#BKMK_BUR) |Hur toosafeguard och återställa data? |Säkerhetskopiera system tillstånd |
| [Konfigurationen av federationsservern](#BKMK_FedSrvConfig) |<li>Distribuera en ny skog med federation i hello molnet?</li> <li>Distribuera AD FS lokala och exponera en proxy i hello molnet?</li> |<li>Säkerhet</li> <li>Efterlevnad</li> <li>Kostnad</li> <li>Åtkomst tooapplications affärspartners</li> |
| [Cloud services-konfiguration](#BKMK_CloudSvcConfig) |En tjänst i molnet distribueras implicit hello första gången du skapar en virtuell dator. Behöver du toodeploy ytterligare molntjänster? |<li>Kräver direkt exponering toohello Internet när du en virtuell dator eller på virtuella datorer?</li> <li> Kräver hello service belastningsutjämning?</li> |
| [Federation server-krav för offentliga och privata IP-adresser (dynamisk IP jämfört med virtuella IP-adresser)](#BKMK_FedReqVIPDIP) |<li>Behöver hello Windows Server AD FS-instans toobe nås direkt från hello Internet?</li> <li>Kräver hello program som distribueras i molnet hello sin egen mot Internet IP-adress och port?</li> |Skapa en molntjänst för varje virtuell IP-adress som krävs av din distribution |
| [Windows Server AD FS-konfiguration för hög tillgänglighet](#BKMK_ADFSHighAvail) |<li>Hur många noder i min Windows Server AD FS-serverkluster?</li> <li>Hur många noder toodeploy i Windows Server AD FS-proxy servergruppen?</li> |Återhämtning och feltolerans |

### <a name="BKMK_NetworkTopology"></a>Nätverkstopologi
I ordning toomeet hello IP-adress konsekvens och DNS-krav för Windows Server AD DS, är det nödvändigt toofirst skapa en [virtuella Azure-nätverket](../virtual-network/virtual-networks-overview.md) och bifoga tooit dina virtuella datorer. När skapandet måste du bestämma om toooptionally utöka anslutningen tooyour lokalt företagets nätverk, vilket transparent ansluter Azure virtuella datorer tooon lokala datorer – detta uppnås med hjälp av traditionella VPN-teknik och kräver att en VPN-slutpunkten exponeras i hello kanten av hello företagsnätverket. Det vill säga initieras hello VPN från Azure toohello företagsnätverket, inte tvärtom.

Observera att ytterligare avgifter gäller när du utökar ett virtuellt nätverk tooyour lokalt nätverk utöver hello standard avgifter som gäller tooeach VM. Det finns mer specifikt avgifter för CPU-tid för hello Azure virtuella nätverksgateway och hello utgående trafik som genereras av varje virtuell dator som kommunicerar med lokala datorer via hello VPN. Mer information om nätverket trafik avgifter finns [Azure priser på snabb](http://azure.microsoft.com/pricing/).

### <a name="BKMK_DeploymentConfig"></a>DC distributionskonfiguration
hello sättet att konfigurera hello DC beror på hello krav för Hej tjänst du vill toorun på Azure. Exempelvis kan du distribuera en ny skog isolerade från företagets skogen, för att testa en--konceptbevis, ett nytt program eller några andra kortvarigt projekt som kräver katalogtjänster men inte specifika toointernal företagets resurser.

Som en fördel en isolerad skog Domänkontrollant inte replikerar med lokala domänkontrollanter, vilket resulterar i mindre utgående nätverkstrafik som genereras av hello-system, direkt minskar kostnaderna. Mer information om nätverket trafik avgifter finns [Azure priser på snabb](http://azure.microsoft.com/pricing/).

Ett annat exempel anta att du har sekretesskrav för en tjänst, men hello-tjänsten är beroende av åtkomst tooyour internt Windows Server Active Directory. Om du får toohost data hello-tjänst i molnet hello kan du distribuera en ny underordnad domän för den interna skogen på Azure. I det här fallet kan du distribuera en Domänkontrollant för hello ny underordnad domän (utan hello global katalog i ordning toohelp adress sekretessen). Det här scenariot, tillsammans med en replik DC-distribution kräver ett virtuellt nätverk för anslutning till din lokala domänkontrollanter.

Om du skapar en ny skog, Välj om toouse [Active Directory-förtroenden](https://technet.microsoft.com/library/cc771397) eller [federationsförtroenden](https://technet.microsoft.com/library/dd807036). Balansera hello krav enligt kompatibilitet, säkerhet, kompatibilitet, kostnad och återhämtning. Till exempel tootake nytta av [Selektiv autentisering](https://technet.microsoft.com/library/cc755844) du kan välja toodeploy en ny skog i Azure och skapa ett Windows Server Active Directory-förtroende mellan hello lokal skog och hello molnet skog. Om programmet hello är anspråksmedvetna, men kan du distribuera federationsförtroenden i stället för Active Directory skogsförtroenden. En annan faktor ska hello kostnad tooeither replikera mer data genom att utöka din lokala Windows Server Active Directory toohello molnet eller skapa fler utgående trafik på grund av autentisering och frågebelastningen.

Krav för tillgänglighet och feltolerans även påverka ditt val. Till exempel om hello länken avbryts bryts programmen med antingen en Kerberos-förtroende eller ett federationsförtroende alla sannolikt helt om du har distribuerat tillräcklig infrastruktur på Azure. Alternativa konfigurationer, till exempel domänkontrollanter för repliken (skrivbar eller RODC: ar) öka hello sannolikheten att kan tootolerate länk avbrott.

### <a name="BKMK_ADSiteTopology"></a>Windows Server Active Directory-platstopologi
Du måste definiera toocorrectly platser och platslänkar i ordning toooptimize trafik och minimerar kostnaden. Påverkar Hej replikeringstopologi mellan domänkontrollanter och hello flödet av autentiseringstrafik platser, platslänkar och undernät. Överväg följande trafik avgifter hello och distribuera och konfigurera domänkontrollanter enligt distributionsscenariot toohello krav:

* Det finns en avgift per timme för själva hello-gatewayen:
  
  * Det kan startas och stoppas efter önskemål
  * Om Stoppad är virtuella Azure-datorer isolerade från hello företagsnätverk
* Inkommande trafik är ledig
* Utgående trafik debiteras enligt för[Azure priser på snabb](http://azure.microsoft.com/pricing/). Du kan optimera webbplatsen länkegenskaper mellan lokala platser och hello molnet platser på följande sätt:
  
  * Om du använder flera virtuella nätverk, konfigurera hello-länkar och deras kostnader korrekt tooprevent Windows Server AD DS från prioritera hello Azure plats över något som kan ge hello samma servicenivå utan kostnad. Du kan också inaktivera hello Bridge alla platsen länk (BASL) alternativet (som är aktiverad som standard). Detta säkerställer att endast direkt anslutna platser replikerar med varandra. Domänkontrollanter i transitivt anslutna platser inte längre kan tooreplicate direkt med varandra, men måste replikera via en gemensam plats eller platser. Om hello mellanliggande platser inaktiveras av någon anledning replikering mellan domänkontrollanter i transitivt anslutna platser kommer inte att ske även om anslutningen mellan hello platser är tillgänglig. Slutligen där delar av transitiv replikering beteende vara önskvärt, Skapa platslänksbryggor som innehåller hello lämpliga platslänkar och platser, till exempel lokalt, företagets nätverksplatser.
  * [Konfigurera platslänkkostnader](https://technet.microsoft.com/library/cc794882) korrekt tooavoid oönskade trafik. Till exempel om **försök nästa närmaste platsen** aktiveras, gör att hello-virtuella nätverk platser inte är hello bredvid närmaste genom att öka hello kostnaden för för hello platslänk objektet som ansluter hello Azure site tillbaka toohello företagsnätverk.
  * Konfigurera platslänk [intervall](https://technet.microsoft.com/library/cc794878) och [scheman](https://technet.microsoft.com/library/cc816906) enligt tooconsistency krav och frekvensen för objektet ändras. Justera replikeringsschema med latens tolerans. Domänkontrollanter replikeras endast hello senaste status för ett värde så minskar hello replikeringsintervall kan spara kostnader om det finns en tillräcklig förändringstakten för objektet.
* Om minimera kostnaderna är en prioritet, se till att replikeringen är schemalagd och meddelanden om ändringar har inte aktiverats. Det här är standardkonfigurationen hello vid replikering mellan platser. Detta är inte viktigt om du distribuerar en RODC på ett virtuellt nätverk eftersom hello RODC inte replikeras ändringarna utgående. Men om du distribuerar en skrivbar Domänkontrollant. Du bör kontrollera hello platslänk inte är konfigurerade tooreplicate uppdateringar med onödiga frekvens. Kontrollera att varje plats som innehåller en GC replikerar domänpartitioner från en källdomänkontrollant på en plats som är kopplad till en platslänk eller platslänkar som har en lägre kostnad än hello GC i hello Azure plats om du distribuerar en global katalogserver (GC).
* Det är möjligt toofurther fortfarande minska nätverkstrafiken som genererats av replikering mellan platser genom att ändra hello komprimeringsalgoritm för replikering. hello komprimeringsalgoritm styrs av hello REG_DWORD registret posten HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator komprimeringsalgoritm. hello standardvärdet är 3, som korrelerar toohello Xpress komprimera algoritm. Du kan ändra hello värdet too2, vilka ändringar hello algoritmen tooMSZip. Detta ökar hello komprimering i de flesta fall, men detta sker på hello bekostnad av processoranvändningen. Mer information finns i [hur Active Directory-replikeringstopologi fungerar](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>IP-adresser och DNS
Virtuella Azure-datorer tilldelas ”DHCP-hyrd adresser” som standard. Eftersom virtuella Azure-nätverket dynamiska adresser kvarstår med en virtuell dator för hello livstid hello virtuella uppfylls hello kraven för Windows Server AD DS.

Därför när du använder en dynamisk adress på Azure fungerar du med en statisk IP-adress eftersom det är dirigerbara under hello hello lån och hello hello lån är lika toohello livstid hello-Molntjänsten.

Hello dynamisk adress frigörs om hello VM har stängts av. tooprevent hello IP-adress från flyttats, kan du [använda Set-AzureStaticVNetIP tooassign en statisk IP-adress](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

För namnmatchning, distribuera din egen (eller använda din befintliga) DNS-infrastruktur; Azure-tillhandahållna DNS uppfyller inte hello avancerade namnmatchningen Windows Server AD DS. Till exempel den inte stöder dynamiska SRV-poster och så vidare. Det är ett kritiskt konfigurationsobjekt för domänkontrollanter och domänanslutna klienter för namnmatchning. Domänkontrollanter måste kunna registrera resursposter och lösa andra DC-resursposter.
För feltolerans och prestanda felorsaker är det optimala tooinstall hello Windows Server DNS-tjänsten på hello domänkontrollanter som körs på Azure. Konfigurera sedan hello virtuella Azure-Nätverksegenskaper med hello namn och IP-adressen för hello DNS-server. När andra virtuella datorer på det virtuella nätverket för hello startar konfigureras deras DNS-matchare klientinställningar med DNS-server som en del av hello dynamisk IP-adressallokering.

> [!NOTE]
> Du kan inte ansluta till lokala datorer tooa Windows Server AD DS Active Directory-domän som ligger på Azure direkt via hello Internet. Hej portkrav för Active Directory och hello domänanslutning åtgärden återge den opraktiska toodirectly exponera hello nödvändiga portarna och i praktiken en helt DC toohello Internet.
> 
> 

Virtuella datorer att registrera sina DNS-namn automatiskt vid start eller när det finns en namnändring.

Läs mer om det här exemplet och ett annat exempel som visar hur tooprovision hello första virtuella datorn och installera AD DS på den [installera en ny Active Directory-skog i Microsoft Azure](active-directory-new-forest-virtual-machine.md). Mer information om hur du använder Windows PowerShell finns [installera Azure PowerShell](/powershell/azureps-cmdlets-docs) och [Azure Management-Cmdlets](/powershell/module/azurerm.compute/#virtual_machines).

### <a name="BKMK_DistributedDCs"></a>Fördelade domänkontrollanter
Azure ger fördelar när värd för flera domänkontrollanter i olika virtuella nätverk:

* Flera platser feltolerans
* Fysisk närhet toobranch kontor (kortare svarstid)

Information om hur du konfigurerar direkt kommunikation mellan virtuella nätverk finns i [konfigurera nätverksanslutningen för virtuella nätverk toovirtual](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Skrivskyddade domänkontrollanter
Du behöver för toochoose toodeploy Läs- eller skrivbara domänkontrollanter. Du kan vara lutande toodeploy skrivskyddade domänkontrollanter eftersom du inte fysisk kontroll över dem, men skrivskyddade domänkontrollanter är utformad toobe som distribuerats i platser där deras fysisk säkerhet är i fara, till exempel avdelningskontor.

Hello fysiska säkerhetsrisk på ett filialkontor finns inte i Azure, men skrivskyddade domänkontrollanter kan fortfarande visa sig toobe mer kostnadseffektivt eftersom hello funktioner ger är väl lämpade toothese miljöer än för mycket olika skäl. Till exempel skrivskyddade domänkontrollanter har ingen utgående replikering och är kan tooselectively fylla hemligheter (lösenord). På hello nackdelen hello bristande dessa hemligheter kan kräva på begäran utgående trafik toovalidate dem som autentiserar en användare eller dator. Men hemligheter kan selektivt förinställd och cachelagras.

Skrivskyddade domänkontrollanter ger ytterligare en fördel i och runt HBI-och personligt identifierbar information eftersom du kan lägga till attribut som innehåller känsliga data toohello RODC filtrerad attributuppsättning (FAS). hello FAS är en anpassningsbar uppsättning attribut som inte är replikerad tooRODCs. Du kan använda hello FAS för säkerhets skull om du inte har behörighet eller inte vill toostore personligt Identifierbar eller Affärskritiska på Azure. Mer information finns i [RODC filtrerade attributuppsättningen [(https://technet.microsoft.com/library/cc753459)].

Se till att program kommer att vara kompatibel med skrivskyddade domänkontrollanter du planerar toouse. Många Windows Server Active Directory-aktiverade program fungerar bra med RODC: er, men vissa program kan utföra ineffektivt eller misslyckas om de inte har åtkomst tooa skrivbar Domänkontrollant. Mer information finns i [guide för skrivskyddade domänkontrollanter kompatibilitet](https://technet.microsoft.com/library/cc755190).

### <a name="BKMK_GC"></a>Global katalog
Du behöver toochoose om tooinstall en global katalog (GC). Du bör konfigurera alla domänkontrollanter som globala katalogservrar i en enda domän i skogen. Den kommer inte att öka kostnader eftersom det finns inga ytterligare replikeringstrafik.

I en skog är GC-generationer nödvändiga tooexpand medlemskap i universell grupp under hello autentiseringsprocessen. Om du inte distribuera en GC generera arbetsbelastningar på hello virtuella nätverk som autentiseras mot en Domänkontrollant på Azure indirekt utgående autentisering trafik tooquery GC-generationer lokala under varje autentiseringsförsök.

hello kostnader som är kopplade till global katalog är mindre förutsägbar eftersom de värd för varje domän (i del). Om hello arbetsbelastningen är värd för en tjänst mot Internet och autentiserar användare mot Windows Server AD DS, kan det vara helt oförutsägbart hello kostnader. toohelp minska GC frågor utanför hello molnet platsen under autentiseringen, kan du [aktivera cachelagring av universella gruppmedlemskap](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Installationsmetod
Du behöver toochoose hur tooinstall hello domänkontrollanter på hello virtuellt nätverk:

* Främja nya domänkontrollanter. Mer information finns i [installera en ny Active Directory-skog på Azure-nätverk](active-directory-new-forest-virtual-machine.md).
* Flytta hello VHD från en lokal virtuell DC toohello moln. I det här fallet måste du se till att hello lokala virtuella domänkontrollanten ”flyttas”, inte ”kopierade” eller ”klonade”.

Använd endast Azure virtuella datorer för domänkontrollanter (som skillnad från tooAzure ”web” eller ”worker” roll virtuella datorer). De är beständiga och hållbarhet för tillstånd för en Domänkontrollant krävs. Virtuella Azure-datorer är utformade för arbetsbelastningar som till exempel domänkontrollanter.

Använd inte SYSPREP toodeploy eller klona domänkontrollanter. hello möjlighet tooclone domänkontrollanter är endast tillgängliga från och med Windows Server 2012. hello funktionen kloning kräver stöd för VMGenerationID i hello underliggande hypervisor. Hyper-V i Windows Server 2012 och Azure virtuella nätverk både stöder VMGenerationID, precis som virtualiseringsleverantörer programvara från tredje part.

### <a name="BKMK_PlaceDB"></a>Placering av hello Windows Server AD DS-databasen och SYSVOL
Välj där toolocate hello Windows Server AD DS-databasen, loggar och SYSVOL. De måste distribueras på Azure datadiskar.

> [!NOTE]
> Azure datadiskar är begränsad too1 TB.
> 
> 

Datadiskenheterna göra inte cache skrivningar som standard. Diskenheter för data som är bifogade tooa VM använda skrivning via cachelagring. Skriv ner cachelagring gör att hello skrivning är allokerat toodurable Azure storage innan hello transaktionen har slutförts från hello perspektiv hello Virtuella datorns operativsystem. Det ger hållbarhet på hello bekostnad av något långsammare skrivningar.

Detta är viktigt för Windows Server AD DS eftersom skrivåtgärder bakomliggande disk-cachelagring upphäver bedömning gjorda av hello DC. Windows Server AD DS försöker toodisable skrivcache men det är in toohello disk-i/o system toohonor. Fel toodisable skrivcache, i vissa fall kan utgöra en USN-återställning ledde kvarstående objekt och andra problem.

Som bästa praxis för virtuella domänkontrollanter, hello följande:

* Ange hello värden Cache inställning för hello Azure-datadisk för ingen. Detta förhindrar problem med skrivning till cacheminnet för AD DS-åtgärder.
* Lagra hello-databasen, loggfilerna och SYSVOL på hello antingen samma data disk eller olika datadiskar. Detta är vanligtvis en annan disk än hello-disk som används för hello själva operativsystemet. hello viktiga takeaway är att hello Windows Server AD DS-databasen och SYSVOL måste inte lagras i en typ av operativsystem för Azure disk. Som standard installerar hello AD DS-installationen komponenterna i mappen % systemroot %, vilket inte rekommenderas för Azure.

### <a name="BKMK_BUR"></a>Säkerhetskopiering och återställning
Ta reda på vad som och stöds inte i allmänhet för säkerhetskopiering och återställning av en Domänkontrollant och mer specifikt de körs i en virtuell dator. Se [säkerhetskopierar och återställer överväganden för virtualiserade domänkontrollanter](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers).

Skapa säkerhetskopior med hjälp av endast programvara för säkerhetskopiering som är specifikt medveten om säkerhetskopiering krav för Windows Server AD DS, till exempel Windows Server Backup.

Kopiera inte eller klona domänkontrollanter VHD-filer i stället för att utföra regelbundna säkerhetskopieringar. En återställning ska någonsin krävs, gör det klonade eller kopierade virtuella hårddiskar utan Windows Server 2012 och en hypervisor som stöds, ger dig USN-bubblor.

### <a name="BKMK_FedSrvConfig"></a>Konfigurationen av federationsservern
hello konfiguration av Windows Server AD FS-federationsservrar (STSs) beror delvis på om hello program som du vill toodeploy på Azure måste tooaccess resurser i ditt lokala nätverk.

Om hello program uppfyller följande kriterier hello, kan du distribuera hello program avskilt från ditt lokala nätverk.

* De accepterar SAML säkerhetstoken
* De är exposable toohello Internet
* De inte komma åt lokala resurser

I detta fall konfigurera Windows Server AD FS STSs enligt följande:

1. Konfigurera en isolerad domän-skog på Azure.
2. Ge federerad åtkomst toohello skog genom att konfigurera en Windows Server AD FS-federationsservergrupp.
3. Konfigurera Windows Server AD FS (federationsservergruppen och federationsserverproxygrupp?) i hello lokala skog.
4. Upprätta en federationsförtroenderelation mellan hello lokala och Azure AD FS i Windows Server-instanser.

Hej på andra sidan, om hello program kräver åtkomst tooon lokala resurser, du kan konfigurera Windows Server AD FS med hello program på Azure på följande sätt:

1. Konfigurera anslutningen mellan lokala nätverk och Azure.
2. Konfigurera en federationsservergrupp för Windows Server AD FS i hello lokala skog.
3. Konfigurera en Windows Server AD FS federationsservergrupp på Azure.

Den här konfigurationen har hello utnyttja minska hello exponering av lokala resurser, liknande tooconfiguring Windows Server AD FS med program i ett perimeternätverk.

Observera att i båda fallen måste du upprätta förtroenden relationer med flera identitetsleverantörer om business-to-business samarbete krävs.

### <a name="BKMK_CloudSvcConfig"></a>Cloud services-konfiguration
Molntjänster är nödvändigt om du vill tooexpose en virtuell dator direkt toohello Internet eller tooexpose en Internet-riktade belastningsutjämnade program. Det är möjligt att varje tjänst i molnet erbjuder en konfigurerbar virtuella IP-adress.

### <a name="BKMK_FedReqVIPDIP"></a>Federation server-krav för offentliga och privata IP-adresser (dynamisk IP jämfört med virtuella IP-adresser)
Varje virtuell Azure-dator tar emot en dynamisk IP-adress. En dynamisk IP-adress är en privat adress som är tillgängliga i Azure. I de flesta fall ska det vara nödvändigt tooconfigure en virtuell IP-adress för Windows Server AD FS-distributioner. hello virtuella IP-Adressen är nödvändiga tooexpose Windows Server AD FS slutpunkter toohello Internet och ska användas av externa partners och av klienter för autentisering och den löpande hanteringen. En virtuell IP-adress är en egenskap för en molnbaserad tjänst som innehåller en eller flera virtuella Azure-datorer. Om hello anspråksmedvetet program som distribueras på Azure och Windows Server AD FS är både Internet-riktade och dela vanliga portar, varje kräver en virtuell IP-adressen för sin egen och kommer därför att nödvändiga toocreate en molntjänst för programmet hello och en andra för Windows Server AD FS.

Definitioner av hello villkoren virtuell IP-adress och dynamiska IP-adress finns [termer och definitioner](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Windows Server AD FS-konfiguration för hög tillgänglighet
Även om det är möjligt toodeploy fristående Windows Server AD FS-federationstjänster rekommenderas toodeploy en grupp med minst två noder för både AD FS STS och proxyservrar för produktionsmiljöer.

Se [topologiöverväganden för AD FS 2.0 distribution](https://technet.microsoft.com/library/gg982489) i hello [AD FS 2.0-designguide](https://technet.microsoft.com/library/dd807036) toodecide vilken distributionskonfiguration alternativ bäst passar dina specifika behov.

> [!NOTE]
> I ordning tooget belastningsutjämning för Windows Server AD FS-slutpunkter i Azure, konfigurera alla medlemmar i hello Windows Server AD FS-servergrupp på hello samma molntjänst och använda hello-belastningsutjämning funktion i Azure för HTTP (standard: 80) och HTTPS-portar (standard: 443). Mer information finns i [Azure belastningsutjämnare avsökningen](https://msdn.microsoft.com/library/azure/jj151530).
> Windows Server Utjämning av nätverksbelastning (NLB) stöds inte på Azure.
> 
> 

