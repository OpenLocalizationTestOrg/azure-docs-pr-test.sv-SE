---
title: "aaaInstall SAP HANA på SAP HANA i Azure (stora instanser) | Microsoft Docs"
description: "Hur tooinstall SAP HANA på en SAP HANA i Azure (stora instans)."
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
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
ms.openlocfilehash: b2fe242270a1166cabcfae2f9249a8dd70ff3b93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-sap-hana-large-instances-on-azure"></a>Hur tooinstall och konfigurera SAP HANA (stora instanser) på Azure

Följande är några viktiga definitioner tooknow innan du läser den här guiden. I [SAP HANA (stora instanser) översikt och arkitektur för Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) vi har fört två olika klasser av HANA stora instans enheter med:

- S72, S72m, S144, S144m, S192 och S192m som vi refererar tooas hello 'Type I klassen' av SKU: er.
- S384, S384m, S384xm, S576, S768 och S960 som vi refererar tooas hello typ II klass av SKU: er.

hello klassen Specificerare är pågående toobe används i hela hello HANA stora instans dokumentationen tooeventually finns toodifferent funktioner och krav utifrån HANA stora instans SKU: er.

Övriga definitioner som vi använder ofta är:
- **Stora instans stämpel:** maskinvara infrastruktur-stacken, som är SAP HANA TDI certifierad och dedikerade toorun SAP HANA-instanser i Azure.
- **SAP HANA i Azure (stora instanser):** officiellt namn för hello erbjudande på Azure toorun HANA instanser i på SAP HANA TDI certifierad maskinvara som distribueras i stora instans tidsstämplar i olika Azure-regioner. hello relaterade termen **HANA stora instans** kort för SAP HANA i Azure (stora instanser) och är ofta används den här tekniska distributionsguiden.


du är ansvarig för hello installation av SAP HANA och du kan börja hello aktivitet efter leverans av en ny SAP HANA på Azure (stora instanser)-server. Och när hello anslutning mellan Azure VNet(s) och hello HANA stora instans enhet(er) har upprättats. 

> [!Note]
> Per SAP princip måste hello installation av SAP HANA utföras av en person som certifierad tooperform SAP HANA-installationer. En person som har klarat hello certifierade SAP-teknik associera examen, SAP HANA-Installation-certifiering, eller genom att en SAP-certifierade integrator (SI).

Kontrollera igen, särskilt när du planerar tooinstall HANA 2.0 [SAP stöd Obs #2235581 - SAP HANA: operativsystem som stöds](https://launchpad.support.sap.com/#/notes/2235581/E) i ordning toomake till att hello OS stöds med hello SAP HANA släpper du valt tooinstall. Du upptäcker att hello Operativsystemet stöds för HANA 2.0 är mer begränsad hello operativsystem som stöds för HANA 1.0. 

## <a name="first-steps-after-receiving-hello-hana-large-instance-units"></a>Första stegen när du har fått hello HANA stora instans enhet(er)

**Första steget** efter mottagandet hello HANA stora instansen och har upprättats åtkomst och anslutningar toohello instanser är tooregister hello OS i hello-instansen med OS-providern. Det här steget inkluderar registrera ditt SUSE Linux-operativsystem i en instans av SUSE SMT som du behöver toohave distribueras på en virtuell dator i Azure. hello HANA stora instans enhet kan ansluta toothis SMT instans (se senare i den här dokumentationen). Eller RedHat-OS måste toobe som registrerats med hello Red Hat prenumeration Manager behöver du tooconnect till. Se även anmärkningar i det här [dokument](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Det här steget är också nödvändigt toobe kan toopatch hello OS. En aktivitet som är hello ansvar hello kunden. Hitta dokumentation tooinstall för SUSE, och konfigurera SMT [här](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

**Andra steg** är toocheck för nya korrigeringsfiler och korrigeringar av hello specifika OS/versionen. Kontrollera om hello korrigeringsnivå av hello HANA stora instans på hello senaste tillstånd. Baserat på tidsinställningen för korrigering/versioner och ändringar toohello operativsystemsavbildning Microsoft kan distribuera, kan det finnas fall där hello senaste korrigeringarna ingår inte kanske. Därför är det ett obligatoriskt steg när du har tagit över en HANA stora instans-enhet, toocheck om korrigeringsprogram som är relevanta för säkerhet, funktioner, tillgänglighet och prestanda under tiden publicerades av hello viss Linux leverantör och måste toobe tillämpas.

**Tredje steget** är toocheck ut hello relevanta SAP-information för att installera och konfigurera SAP HANA på hello specifika OS/versionen. På grund av toochanging rekommendationer eller ändringar tooSAP anteckningar eller konfigurationer som är beroende av enskilda installationsscenarier Microsoft kommer inte alltid kan toohave en HANA stora instans-enhet som har konfigurerats helt. Därför är det obligatoriska du som kund, tooread hello SAP anteckningar relaterade tooSAP HANA på din exakt Linux-versionen. Även kontrollera hello konfigurationer av hello OS/slutversionen nödvändigt och tillämpa hello konfigurationsinställningarna där det inte redan har gjort.

I specifika, kontrollera hello följande parametrar och slutligen justerade efter:

- NET.Core.rmem_max = 16777216
- NET.Core.wmem_max = 16777216
- NET.Core.rmem_default = 16777216
- NET.Core.wmem_default = 16777216
- NET.Core.optmem_max = 16777216
- NET.IPv4.tcp_rmem = 65536 16777216 16777216
- NET.IPv4.tcp_wmem = 65536 16777216 16777216

Från och med SLES12 SP1 och RHEL 7.2 kan måste dessa parametrar anges i en konfigurationsfil i hello /etc/sysctl.d directory. Till exempel måste en konfigurationsfil med hello namn 91-NetApp-HANA.conf skapas. Parametrarna måste vara set in/etc/sysctl.conf för äldre SLES och RHEL versioner.

Alla RHEL släpper och börjar med SLES12 hello 
- sunrpc.tcp_slot_table_entries = 128

Parametern måste anges in/etc/modprobe.d/sunrpc-local.conf. Om hello-filen inte finns, måste det först skapas genom att lägga till följande post hello: 
- alternativ sunrpc tcp_max_slot_table_entries = 128

**Fjärde steget** är toocheck hello systemtiden din HANA stora instans enhet. hello instanser distribueras med en tidszon för system som representerar hello platsen för hello Azure-region hello HANA stora instans stämpel finns i. Du är ledigt toochange hello systemklockan eller tidszon hello-instanser som du äger. Gör detta och beställa fler instanser i din klient förberedas måste tooadapt hello tidszon hello nyligen levereras instanser. Microsoft har ingen insikter om hello systemtidszonen konfigurera med hello instanser efter hello övergång. Därför nyligen distribuerade instanser kan inte anges i hello samma tidszon som hello något du ändrat till. Därför är det ditt ansvar som kunden toocheck och anpassa hello tidszon hello instanser överlämnas om det behövs. 

**Femte steget** är toocheck etc/hosts. Eftersom hello blad hämta överlämnas har de olika IP-adresser som är tilldelade för olika ändamål (se nästa avsnitt). Kontrollera hello etc/hosts-filen. I fall där enheter läggs till i en befintlig klient inte förväntar dig toohave etc/hosts hello nyligen distribuerade system som hanteras på rätt sätt med hello IP-adresser för tidigare levererat system. Därför det är på du som kund toocheck hello rätt inställningar genom att en nyligen distribuerade instans kan interagera och lösa hello namnen på tidigare distribuerade enheter i din klient. 

## <a name="networking"></a>Nätverk
Vi förutsätter att du följt hello rekommendationer utforma Azure-Vnet och ansluter dessa Vnet toohello HANA stora instanser som beskrivs i dessa dokument:

- [Översikt över SAP HANA (stora instans) och arkitektur på Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Infrastruktur för SAP HANA (stora instanser) och anslutningar på Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Det finns vissa detaljer värt toomention om hello nätverk hello enda enheter. Varje enhet HANA stora instans levereras med två eller tre IP-adresser som är tilldelade tootwo eller tre NIC-portar för hello enhet. Tre IP-adresser används i HANA skalbar konfigurationer och hello HANA System Replication scenario. En av hello IP-adresser tilldelas toohello NIC av hello enhet ligger utanför hello Server IP-adresspool som beskrevs i hello [SAP HANA (stora instans) översikt och arkitektur för Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).

hello-distribution för enheter med två IP-adresser som tilldelats bör se ut som:

- eth0.xx bör ha en IP-adress som ligger utanför hello Server IP-adressintervall i poolen du skickat tooMicrosoft. Den här IP-adressen användas för att underhålla i/etc/hosts på hello OS.
- eth1.xx bör ha en IP-adress som används för kommunikation tooNFS. Dessa adresser bör därför **inte** måste toobe underhålls i etc/värdar i ordning tooallow instans tooinstance trafik i hello-klient.

En bladet konfiguration med två IP-adresser som tilldelats är inte lämplig för distribution fall HANA System replikering eller HANA skalbara. Om med två IP-adresser som är tilldelade endast och förskjutning toodeploy konfiguration, kontaktar du SAP HANA på Azure-tjänsthantering tooget tredje IP-adress i ett tredje VLAN tilldelat. För stora HANA-instans enheter med tre IP-adresser som har tilldelats tre NIC-portar, gäller hello följande användningsregler:

- eth0.xx bör ha en IP-adress som ligger utanför hello Server IP-adressintervall i poolen du skickat tooMicrosoft. Denna IP-adress skall därför inte användas för att underhålla i/etc/hosts på hello OS.
- eth1.xx bör ha en IP-adress som används för kommunikation tooNFS lagring. Den här typen av adresser bör därför inte behållas i etc/hosts.
- eth2.xx ska endast använda toobe underhålls i etc/hosts för kommunikation mellan olika instanser av hello. Dessa adresser är också hello IP-adresser som behöver toobe underhålls i skalbar HANA konfigurationer som IP-adresser HANA används för konfiguration av hello mellan noder.



## <a name="storage"></a>Lagring

Hej lagringslayout för SAP HANA i Azure (stora instanser) konfigureras av SAP HANA på Azure Service Management via SAP rekommenderade riktlinjer enligt beskrivningen i [lagringskraven för SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) vitboken. Hej grov storlekar på hello olika volymer med hello olika HANA stora instanser SKU: er fick dokumenterade i [SAP HANA (stora instans) översikt och arkitektur för Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

hello namnkonventioner för hello lagringsvolymer finns i hello följande tabell:

| Lagringsanvändning | Monteringspunkt | Volymnamn | 
| --- | --- | ---|
| HANA data | /Hana/data/SID/mnt0000<m> | Lagring IP: / hana_data_SID_mnt00001_tenant_vol |
| HANA logg | /Hana/log/SID/mnt0000<m> | Lagring IP: / hana_log_SID_mnt00001_tenant_vol |
| HANA loggsäkerhetskopiering | /Hana/log/Backups | Lagring IP: / hana_log_backups_SID_mnt00001_tenant_vol |
| HANA delade | /Hana/Shared/SID | Lagring IP: / hana_shared_SID_mnt00001_tenant_vol/delade |
| usr/sap | /usr/SAP/SID | Lagring IP: / hana_shared_SID_mnt00001_tenant_vol/usr_sap |

Där SID = hello HANA instans System-ID 

Och = en intern uppräkning av åtgärder när du distribuerar en klient.

Som du ser HANA delade usr/sap delar hello samma volym. hello nomenklaturen för hello monteringspunkter innehåller hello System-ID för hello HANA instanser samt hello montera nummer. Skala upp distributioner finns det bara en monteringspunkt som mnt00001. Medan i skalbar distribution visas så många monteringar som måste ha worker och master noder. Hello skalbar miljö, data, log är loggen Säkerhetskopiera volymer delad och anslutna tooeach nod i hello skalbar konfiguration. För konfigurationer med flera instanser av SAP är en annan uppsättning volymer skapade och anslutna toohello HAN stora instans enhet.

När du läsa hello dokumentet och se ut en HANA stora instans-enhet upptäcker du att hello enheter ingår i stället generösa diskvolymen för HANA-data och att det finns en volym HANA/loggsäkerhetskopiering. hello beror varför vi storlek hello HANA/data så stor på att hello lagring ögonblicksbilder vi erbjuder för du som kund använder hello samma volym. Hello innebär mer lagringsutrymme ögonblicksbilder du utföra hello mer utrymme som förbrukas av ögonblicksbilder i din tilldelade lagringsvolymer. hello HANA/loggsäkerhetskopiering volym är inte tankar toobe hello volym tooput databassäkerhetskopieringar i. Det är storlek toobe används som säkerhetskopieringsvolymen för säkerhetskopior av hello HANA transaktionsloggen. I framtida versioner av hello lagring snapshot self service, vi gäller den här mer specifika volymen toohave ofta ögonblicksbilder. Och med mer frekventa replikeringar toohello disaster recovery plats om du vill ha toooption i för hello disaster recovery funktionalitet som tillhandahålls av hello HANA stora instans infrastruktur. Mer information finns i [SAP HANA (stora instanser) med hög tillgänglighet och katastrofåterställning i Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

Dessutom toohello lagring som du kan köpa ytterligare lagringsutrymme kapacitet i steg om 1 TB. Den här ytterligare lagringsutrymme kan läggas till som nya volymer tooa HANA stora instanser.

Under onboarding med SAP HANA på Azure-tjänsthantering hello kund som anger ett användar-ID (UID) och grupp-ID (GID) för hello sidadm användar- och sapsys grupp (ex: 1000,500) är det nödvändigt att dessa samma värden som används under installationen av hello SAP HANA-system. Du vill toodeploy flera HANA instanser på en enhet, kan du hämta flera uppsättningar av volymer (en uppsättning för varje instans). Vid tidpunkten för distribution måste därför toodefine:

- hello SID för hello olika HANA instanser (sidadm härleds ur den).
- Minne storlekar på hello olika HANA instanser. Eftersom hello minnesstorlek per instanser definierar hello storleken på hello volymer i varje enskild volym-uppsättning.

Baserat på providern lagringsrekommendationer hello följande alternativ för monteringspunkter har konfigurerats för alla monterade volymer (utesluter Start LUN):

- NFS-rw drivrutiner = 4, hårt, timeo = 600, rsize = 1048576 wsize = 1048576, Intro noatime, låsa 0 0

Dessa montera punkter konfigureras i/etc/fstab som visas i hello följande bilder:

![fstab av monterade volymer i HANA stora instans enhet](./media/hana-installation/image1_fstab.PNG)

hello utdata från hello kommando df -h på en enhet S72m HANA stora instans ser ut som:

![fstab av monterade volymer i HANA stora instans enhet](./media/hana-installation/image2_df_output.PNG)


hello lagringsstyrenhet och noder i hello stora instans stämplar är synkroniserade tooNTP servrar. Med du synkroniserar hello SAP HANA på Azure (stora instanser)-enheter och virtuella Azure-datorer mot en NTP-server, bör det finnas några betydande tid drift sker mellan hello infrastruktur och hello beräkning enheter i Azure eller stora instans stämplar.

I ordning toooptimize SAP HANA toohello lagringsutrymme som används under, bör du också ange hello följande parametrar för SAP HANA-konfiguration:

- max_parallel_io_requests 128
- async_read_submit på
- async_write_submit_active på
- alla async_write_submit_blocks
 
För SAP HANA 1.0 versioner upp tooSPS12, dessa parametrar kan anges under hello installationen av hello SAP HANA-databas, enligt beskrivningen i [SAP Obs #2267798 - konfigurationen av hello SAP HANA-databas](https://launchpad.support.sap.com/#/notes/2267798)

Du kan också konfigurera hello parametrar efter installationen av hello SAP HANA-databas med hjälp av hello hdbparam framework. 

Hej hdbparam framework har ersatts med SAP HANA 2.0. Därför måste hello parametrar anges med hjälp av SQL-kommandon. Mer information finns i [SAP Obs #2399079: eliminering av hdbparam i HANA 2](https://launchpad.support.sap.com/#/notes/2399079).


## <a name="operating-system"></a>Operativsystem

Växlingsutrymme för hello levereras OS-avbildningen har angetts too2 GB enligt toohello [SAP stöd Obs #1999997 – vanliga frågor och svar: SAP HANA minne](https://launchpad.support.sap.com/#/notes/1999997/E). Någon annan inställning önskade behov toobe som du har angett som en kund.

[SUSE Linux Enterprise Server 12 SP1 för SAP-program](https://www.suse.com/products/sles-for-sap/hana) hello fördelning av Linux som installerats för SAP HANA i Azure (stora instanser). Viss distributionen ger funktioner för SAP-specifika &quot;out of box hello&quot; (inklusive förinställda parametrar för att köra SAP på SLES effektivt).

Se [resurs bibliotek/faktablad](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) på hello SUSE webbplats och [SAP på SUSE](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) på hello SAP Community nätverket Lagringsnodernas för flera användbara resurser med toodeploying SAP HANA på SLES (inklusive hello installation Hög tillgänglighet, säkerhetsåtgärder härdning specifika tooSAP och mer).

Ytterligare och användbara SAP på SUSE-relaterade länkar:

- [SAP HANA på platsen för SUSE Linux](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [Bästa praxis för SAP: sätta replikering – SAP NetWeaver på SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113).
- [ClamSAP – SLES virusskydd för SAP](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (inklusive SLES 12 för SAP-program).

SAP stöd anteckningar tillämpliga tooimplementing SAP HANA på SLES 12:

- [Stöd för SAP-kommentar #1944799 – SAP HANA riktlinjer för Installation av operativsystemet SLES](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html).
- [Stöd för SAP-kommentar #2205917 – SAP HANA DB rekommenderade OS-inställningar för SLES 12 för SAP-program](https://launchpad.support.sap.com/#/notes/2205917/E).
- [SAP stöd Obs! #1984787 – SUSE Linux Enterprise Server 12 installationsinformation](https://launchpad.support.sap.com/#/notes/1984787).
- [Stöd för SAP-kommentar #171356 – SAP-program på Linux: allmän Information](https://launchpad.support.sap.com/#/notes/1984787).
- [Stöd för SAP-kommentar #1391070 – Linux UUID lösningar](https://launchpad.support.sap.com/#/notes/1391070).

[Red Hat Enterprise Linux för SAP HANA](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) är ett annat erbjudande för att köra SAP HANA på HANA stora instanser. Versioner av RHEL 6.7 och 7.2 är tillgängliga. 

Ytterligare och användbara SAP på Red Hat relaterade länkar:
- [SAP HANA på Red Hat Linux plats](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

SAP stöd anteckningar tillämpliga tooimplementing SAP HANA på Red Hat:

- [Stöd för SAP-kommentar #2009879 - SAP HANA riktlinjer för Red Hat Enterprise Linux (RHEL) operativsystem](https://launchpad.support.sap.com/#/notes/2009879/E).
- [SAP stöd Obs! #2292690 - SAP HANA DB rekommenderas OS-inställningar för RHEL 7](https://launchpad.support.sap.com/#/notes/2292690).
- [SAP stöd Obs! #2247020 - SAP HANA DB rekommenderas OS-inställningar för RHEL 6.7](https://launchpad.support.sap.com/#/notes/2247020).
- [Stöd för SAP-kommentar #1391070 – Linux UUID lösningar](https://launchpad.support.sap.com/#/notes/1391070).
- [SAP stöd Obs! #2228351 - Linux SAP HANA-databas Service Pack 11 revision 110 (eller högre) på RHEL 6 eller SLES 11](https://launchpad.support.sap.com/#/notes/2228351).
- [SAP stöd Obs! #2397039 – vanliga frågor och svar SAP-på RHEL](https://launchpad.support.sap.com/#/notes/2397039).
- [Stöd för SAP-kommentar #1496410 - Red Hat Enterprise Linux 6.x: Installation och uppgradering](https://launchpad.support.sap.com/#/notes/1496410).
- [Stöd för SAP-kommentar #2002167 - Red Hat Enterprise Linux 7.x: Installation och uppgradering](https://launchpad.support.sap.com/#/notes/2002167).

## <a name="time-synchronization"></a>Tidssynkronisering

SAP-program som bygger på hello SAP NetWeaver arkitektur är känsliga på tidsskillnaderna för hello olika komponenter som utgör hello SAP-system. SAP ABAP kort Dumpar med hello Felrubrik av ZDATE\_stor\_tid\_DIFF är troligen bekant, dessa kort Dumpar ut när hello systemtiden på olika servrar eller virtuella datorer går för långt ifrån varandra.

För SAP HANA i Azure (stora instanser) tidssynkronisering i Azure & #39, gäller t toohello beräkning enheter i hello stora instans stämplar. Synkroniseringen kan inte användas för SAP-program som körs i enhetligt virtuella Azure-datorer som Azure garanterar ett system &#39; s tid är korrekt synkroniserad. Därför en gång till gång server måste ställas in som kan användas av SAP-programservrar som kör på virtuella Azure-datorer och hello SAP HANA-databas instanser som körs på HANA stora instanser. hello lagringsinfrastruktur i stora instans stämplar är tidsinställningen synkroniserad med NTP-servrar.

## <a name="setting-up-smt-server-for-suse-linux"></a>Konfigurera SMT server för SUSE Linux
SAP HANA stora instanser har inte direkt anslutning toohello Internet. Därför är inte ett enkelt att tooregister sådan enhet med hello OS-providern och toodownload och tillämpa korrigeringar. SUSE Linux hello gäller kan en lösning vara tooset in en SMT-server i en Azure VM. Hello Azure VM bör finnas toobe i ett Azure-VNet som är anslutna toohello HANA stora instans. Med sådana en SMT kan hello HANA stora instans enhet registrera och ladda ned korrigeringar. 

SUSE innehåller en större handbok om deras [prenumeration hanteringsverktyg för SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

Som ett villkor för hello-installation av en SMT-server som uppfyller hello aktivitet för HANA stort instans, behöver du:

- Ett virtuellt Azure-nätverk som är anslutna toohello HANA stora instans ER krets.
- Ett konto som är associerad med en organisation, SUSE. Medan hello organisation behöver toohave vissa giltig SUSE prenumeration.

### <a name="installation-of-smt-server-on-azure-vm"></a>Installation av SMT server på Azure VM

I det här steget kan installera du hello SMT server i en Azure VM. hello första åtgärd är toolog i toohello [SUSE Customer Center](https://scc.suse.com/)

Eftersom du har loggat in hittar tooOrganization--> inloggningsuppgifter för organisationen. I detta avsnitt bör du hitta hello-autentiseringsuppgifter som är nödvändiga tooset hello SMT Server.

hello tredje steget är tooinstall en SUSE Linux VM i hello Azure VNet. toodeploy hello VM, ta en bild med SLES 12 SP2 galleriet Azure. I processen för distribution av hello inte definierar ett DNS-namn och inte använder statiska IP-adresser som visas i den här skärmbilden

![distribution av virtuella datorer för SMT server](./media/hana-installation/image3_vm_deployment.png)

hello distribuerade virtuella datorn har en mindre VM och fick hello interna IP-adress i hello Azure VNet för 10.34.1.4. Namnet på hello VM var smtserver. Efter installationen av hello kontrollerades hello anslutningen toohello HANA stora instans enhet(er). Beroende på hur du ordnade namnmatchning kanske du behöver tooconfigure lösning av hello HANA stora instans enheter i etc/hosts av hello Azure VM. Lägg till en annan disk toohello VM som ska toobe används toohold hello korrigeringar. hello startdisk själva kan vara för liten. I hello fall påvisa fick hello disk monterade för/srv/www/htdocs som visas i följande skärmbild hello. En disk på 100 GB bör vara tillräckligt.

![distribution av virtuella datorer för SMT server](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Logga in toohello HANA stora instans enheter, underhålla/etc/hosts och kontrollera om du kan nå hello Azure VM som ska toorun hello SMT server hello nätverket.

När den här kontrollen görs har, måste toolog i toohello virtuella Azure-datorn som ska köra hello SMT server. Om du använder putty toolog i toohello VM måste tooexecute denna sekvens med kommandon i bash-fönstret:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Starta om inställningarna bash tooactivate hello när köra dessa kommandon. Starta YAST.

Gå tooSoftware underhåll och Sök efter smt i YAST. Välj smt som växlar automatiskt tooyast2 smt som visas nedan

![SMT i yast](./media/hana-installation/image5_smt_in_yast.PNG)


Acceptera hello val för installation på hello smtserver. När du har installerat gå toohello SMT serverkonfiguration och ange hello organisations autentiseringsuppgifter från hello SUSE Customer Center som hämtats tidigare. Även ange det Virtuella Azure-värdnamnet som hello SMT-Serveradress. I den här demonstrationen var https://smtserver som visas i hello nästa graphics.

![Konfiguration av SMT server](./media/hana-installation/image6_configuration_of_smtserver1.png)

Som nästa steg måste tootest om hello anslutning toohello SUSE kunden Center fungerar. Som du ser i hello följande bilder i hello demonstration fallet fungerade.

![Testa ansluta tooSUSE Customer Center](./media/hana-installation/image7_test_connect.png)

En gång hello SMT installationen startar måste tooprovide lösenord. Eftersom det är en ny installation måste toodefine lösenordet enligt hello nästa grafik.

![Ange lösenord för databas](./media/hana-installation/image8_define_db_passwd.PNG)

hello nästa interaktion som du har är när ett certifikat skapas. Gå igenom hello dialogrutan enligt nästa och hello steg ska fortsätta.

![Skapa ett certifikat för SMT server](./media/hana-installation/image9_certificate_creation.PNG)

Det kan finnas vissa antal minuter som används i hello steg i ”Kör kontroll av synkronisering' hello slutet av hello konfiguration. Efter hello installation och konfiguration av hello SMT server, bör du hitta hello directory lagringsplatsen under hello montera punkt /srv/www/htdocs/plus vissa underkataloger under lagringsplatsen. 

Starta om hello SMT server och dess relaterade tjänster med dessa kommandon.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

### <a name="download-of-packages-onto-smt-server"></a>Hämtning av paket till SMT server

När alla hello tjänsterna startas om, Välj hello paket i SMT-hantering med Yast. hello paketet markeringen beror på hello OS-avbildningen av hello HANA stora instans server och inte hello SLES släpper eller version av hello VM hello SMT server som körs. Ett exempel på hello urvalssidan visas nedan.

![Välj paket](./media/hana-installation/image10_select_packages.PNG)

När du är klar med hello paketet måste toostart hello inledande kopia av hello väljer paket toohello SMT server du konfigurerar. Den här kopian utlöses hello Shell med hello kommandot smt-spegling som visas nedan


![Hämta paket tooSMT server](./media/hana-installation/image11_download_packages.PNG)

Hello paket ska hämta kopieras till hello kataloger som skapas under hello montera punkt /srv/www/htdocs som du ser ovan. Den här processen kan ta en stund. Beroende på hur många paket som du väljer, det kan ta upp tooone timme eller mer.
Eftersom den här processen är klar måste toomove toohello SMT klientinstallationen. 

### <a name="set-up-hello-smt-client-on-hana-large-instance-units"></a>Ställ in hello SMT klienten på HANA stora instans enheter

hello-klienter är i det här fallet hello HANA stora instans. hello SMT serverinstallation kopieras hello skriptet clientSetup4SMT.sh till hello Azure VM. Kopiera skriptet över toohello HANA stora instans-enhet som du vill tooconnect tooyour SMT servern. Starta hello skript i hello -h och ge den som parametern SMT serverns hello namn. I det här exemplet smtserver.

![Konfigurera SMT-klienten](./media/hana-installation/image12_configure_client.PNG)

Det kan finnas ett scenario där hello belastningen på hello certifikatet från hello-servern av hello-klienten är klar, men hello registreringen misslyckades enligt nedan.

![Registrering av klienten misslyckas](./media/hana-installation/image13_registration_failed.PNG)

Läs om hello-registrering misslyckades [SUSE stöder dokumentet](https://www.suse.com/de-de/support/kb/doc/?id=7006024) och köra hello stegen som beskrivs det.

> [!IMPORTANT] 
> Som servernamn måste tooprovide hello namnet på hello VM, i det här fallet smtserver utan hello fullständigt kvalificerade domännamnet. Bara hello VM namn fungerar. 

När dessa steg har utförts, måste tooexecute hello följande kommando på hello HANA stora instans enhet

```
SUSEConnect –cleanup
```

> [!Note] 
> I våra tester hade vi alltid toowait några minuter efter att steget. hello avslutades körs omedelbart clientSetup4SMT.sh när hello korrigerande åtgärder enligt hello SUSE artikel med meddelanden hello certifikatet inte giltigt ännu. Väntar på vanligtvis 5 – 10 minuter och köra clientSetup4SMT.sh avslutades i en lyckad klientkonfiguration.

Om du körde i hello problemet som behövs av toofix baserat på hello steg i hello SUSE artikel måste toorestart clientSetup4SMT.sh på hello HANA stora instans enheten igen. Nu ska den slutföras enligt nedan.

![Klienten har registrerats](./media/hana-installation/image14_finish_client_config.PNG)

Med det här steget konfigurerad hello SMT klienter hello HANA stora instans enhet tooconnect mot hello SMT-server som du har installerat i hello Azure VM. Du kan nu ta 'zypper upp' eller 'zypper i' tooinstall OS korrigeringsprogram tooHANA stora instanser eller installera ytterligare paket. Det är att förstå att du bara kan få korrigeringsprogram som du hämtade innan på hello SMT-servern.


## <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>Exempel på en SAP HANA-installation på HANA stora instanser
Detta avsnitt visar hur tooinstall SAP HANA på en enhet med stora HANA-instans. Vi har ser ut som hello start-tillstånd:

- Du har angett Microsoft alla hello data toodeploy du en SAP HANA stora instans.
- Du har fått hello SAP HANA stora instans från Microsoft.
- Du har skapat ett virtuellt Azure-nätverk som är anslutna tooyour lokalt nätverk.
- Du har anslutit hello ExpressRotue krets för HANA stora instanser toohello samma virtuella Azure-nätverk.
- Du har installerat en Azure VM som du använder som en hopp för HANA stora instanser.
- Du kontrollerat att du kan ansluta från hello hopp tooyour HANA stora instans enheten och vice versa.
- Du har markerat om alla hello nödvändiga paketen och korrigeringsprogram som är installerade.
- Du kan läsa hello SAP anteckningar och dokumentation om HANA installation på hello OS du använder och kontrollerat att hello HANA versionen av choice stöds på hello OS-versionen.

Vad som visas i nästa hello-sekvenser är hello hämtningen av hello HANA installationen paket toohello hopp rutan VM, i det här fallet körs på en Windows-Operativsystemet, hello kopia av hello paket toohello HANA stora instans enhet och hello sekvens av hello-installationen.

### <a name="download-of-hello-sap-hana-installation-bits"></a>Hämta hello SAP HANA installationen bitar
Eftersom hello HANA stora instans enheter inte har direkt anslutning toohello internet, du kan inte direkt hämta hello-paket från SAP toohello HANA stora instans VM. tooovercome hello saknas direkt anslutning till internet, måste hello hopp rutan. Du kan hämta hello paket toohello hopp rutan VM.

I ordning installationspaket toodownload hello HANA behöver du en SAP-S-användare eller en annan användare, vilket gör att du tooaccess hello SAP Marketplace. Gå igenom den här sekvensen skärmar när du loggar in:

Gå för[SAP Service Marketplace](https://support.sap.com/en/index.html) > Klicka på hämta programvara > installationer och uppgradering > av alfabetiskt Index > H Under – SAP HANA-plattformen Edition > SAP HANA-plattformen version 2.0 > Installation > hämta hello följande filer

![Hämta HANA installation](./media/hana-installation/image16_download_hana.PNG)

Hello demonstration om hämtat vi installationspaket för SAP HANA 2.0. Hello Azure hopp rutan VM Expandera hello självextraherande arkiv till hello directory som visas nedan.

![Extrahera HANA installation](./media/hana-installation/image17_extract_hana.PNG)

Kopiera hello directory som skapats av hello extrahering i hello fallet ovan 51052030 toohello HANA stora instans enhet till hello /hana/shared volym i en katalog som du skapade som hello Arkiv extraheras.

> [!Important]
> Kopiera hello installationspaket till hello rot eller inte starta LUN eftersom utrymme är begränsat och måste användas av andra processer samt toobe.


### <a name="install-sap-hana-on-hello-hana-large-instance-unit"></a>Installera SAP HANA på hello HANA stora instans enhet
I ordning tooinstall SAP HANA måste toolog i som användarrot. Endast rot har tillräckligt med behörighet tooinstall SAP HANA.
hello måste du först toodo är tooset behörigheter på hello directory du kopieras över till hana-delat. hello behörigheter måste tooset som

```
chmod –R 744 <Installation bits folder>
```

Om du vill tooinstall SAP HANA använder hello grafiska installationen, hello gtk2 paketet måste toobe installerad på hello HANA stora instanser. Kontrollera om det är installerat med hello kommando

```
rpm –qa | grep gtk2
```

Vi visar hello SAP HANA-installationen med hello grafiskt användargränssnitt i ytterligare steg. Gå till installationskatalogen för hello och navigera till hello sub directory HDB_LCM_LINUX_X86_64 som nästa steg. Start

```
./hdblcmgui 
```
out-of-katalogen. Nu komma vägleds du genom en sekvens med skärmar där du behöver tooprovide hello data för hello-installationen. I hello fall påvisa installerar vi hello SAP HANA-databasservern och hello SAP HANA-klientkomponenter. Vår markeringen är därför SAP HANA-databas som visas nedan

![Välj HANA i installationen](./media/hana-installation/image18_hana_selection.PNG)

Hello nästa skärm väljer du hello alternativet Installera nya System

![Välj HANA nyinstallation](./media/hana-installation/image19_select_new.PNG)

När det här steget måste tooselect mellan flera ytterligare komponenter som kan installeras dessutom toohello SAP HANA-databasservern.

![Välj ytterligare HANA komponenter](./media/hana-installation/image20_select_components.PNG)

För hello syftet med den här dokumentationen, vi valde hello SAP HANA-klienten och hello SAP HANA Studio. Vi kan också installera en instans av skala upp. Därför hello nästa skärm måste toochoose enda värd-systemet 

![Välj skala upp installation](./media/hana-installation/image21_single_host.PNG)

Hello nästa skärm måste tooprovide vissa data

![Ange SAP HANA-SID](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Som HANA System-ID (SID) behöver du tooprovide hello samma SID som du angav Microsoft när du sorterade hello HANA stora instans distribution. Om du väljer en annan SID gör hello installationen misslyckas på grund av tooaccess behörighet problem på hello olika volymer

Som installationskatalog kan du använda hello /hana/shared directory. I hello nästa steg behöver du tooprovide hello platser för hello HANA datafiler och hello HANA loggfiler


![Ange HANA loggplatsen](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Du måste definiera som data och loggfiler hello volymer som redan medföljde hello monteringspunkter som innehåller hello SID som du valde i hello skärmen markeringen innan den här skärmen. Hello SID matchningsfel med hello något du har skrivit in, hello inloggningsskärmen före, gå tillbaka och justera hello SID toohello-värdet som du har på hello monteringspunkter.

I nästa steg för hello, granska hello värdnamn och slutligen korrigera den. 

![Granska värdnamn](./media/hana-installation/image24_review_host_name.PNG)

I hello nästa steg behöver du också tooretrieve data som du gav tooMicrosoft när du sorterade hello HANA stora instans distribution. 

![Ge systemanvändare UID och GID](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Du behöver tooprovide hello samma System användar-ID och ID användargrupp som du tillhandahöll Microsoft när du beställer hello enhet distribution. Om du inte toogive hello mycket misslyckas samma ID, hello installationen av SAP HANA på hello HANA stora instans enhet.

Hello två skärmar, vilket vi inte visas i den här dokumentationen behöver tooprovide hello lösenord för hello systemanvändare hello SAP HANA-databas och hello hello sapadm användare, som används för hello SAP värd-Agent som installeras som en del av lösenord hello SAP HANA-databasinstansen.

När du har definierat hello lösenord en bekräftelseskärm visas. Kontrollera alla hello data i listan och fortsätter med installationen av hello. Du når ett förlopp dokument hello Installationsförlopp som hello en nedan

![Kontrollera installationens förlopp](./media/hana-installation/image27_show_progress.PNG)

Eftersom hello installationen är klar bör du en bild som hello efter

![Installationen är klar](./media/hana-installation/image28_install_finished.PNG)

Hello SAP HANA-instansen ska nu vara igång och körs och redo för användning. Du ska kunna tooconnect tooit från SAP HANA Studio. Kontrollera också att du kontrollerar för hello senaste korrigeringarna för SAP HANA och tillämpa dessa korrigeringar.
























































 







 




