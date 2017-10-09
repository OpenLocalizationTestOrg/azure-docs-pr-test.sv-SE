---
title: "Snabbstart: Manuell installation av single instance SAP HANA på Azure Virtual Machines | Microsoft Docs"
description: "Snabbstartsguide för manuell installation av single instance SAP HANA på Azure Virtual Machines"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 57b58b8e07379eed5641f5f89d55b38f52c69e44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Snabbstart: Manuell installation av single instance SAP HANA på Azure Virtual Machines
## <a name="introduction"></a>Introduktion
Den här guiden hjälper dig att konfigurera en enskild instans SAP HANA på virtuella Azure-datorer (VM) när du installerar SAP NetWeaver 7.5 och SAP HANA 1.0 SP12 manuellt. hello fokuserar den här guiden på distribuera SAP HANA på Azure. Ersätter inte SAP-dokumentationen. 

>[!Note]
>Den här guiden beskriver distributioner av SAP HANA i virtuella Azure-datorer. Information om hur du distribuerar SAP HANA till HANA stora instanser finns [med SAP på virtuella Azure-datorer (VM)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started).
 
## <a name="prerequisites"></a>Krav
Den här handboken förutsätts att du är bekant med denna infrastruktur som en tjänst (IaaS) grunderna som:
 * Hur hello toodeploy virtuella datorer eller virtuella nätverk via Azure-portalen eller PowerShell.
 * hello Azure plattformsoberoende kommandoradsgränssnittet (CLI), inklusive hello alternativet toouse JavaScript Object Notation (JSON) mallar.

Den här guiden förutsätter också att du är bekant med:
* SAP HANA och SAP NetWeaver och hur tooinstall dem lokalt.
* Installera och SAP HANA och SAP programinstanser på Azure.
* Hej följande koncept och procedurer:
   * Planera för SAP-distribution på Azure, inklusive Azure Virtual Network planerings- och användningen av Azure Storage. Se [SAP NetWeaver på Azure virtuella datorer (VM) - guide för planering och implementering](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).
   * Distribution av principer och sätt toodeploy virtuella datorer i Azure. Se [distribution av virtuella datorer i Azure för SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).
   * Hög tillgänglighet för SAP NetWeaver ASCS (ABAP SAP centrala Services), SCS (SAP centrala Services) och ÄNDARE (utvärderas inleverans lösning) på Azure. Se [hög tillgänglighet för SAP NetWeaver på Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide).
   * Information om hur tooimprove effektivitet för att använda en multi-SID-installation av ASCS/SCS på Azure. Se [skapa en konfiguration för SAP NetWeaver multi-SID](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid). 
   * Principer för SAP NetWeaver som körs baserat på Linux-driven virtuella datorer i Azure. Se [körs SAP NetWeaver på Microsoft Azure SUSE Linux virtuella datorer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart). Den här guiden innehåller specifika inställningar för Linux i virtuella Azure-datorer och information om hur tooproperly bifoga Azure storage diskar tooLinux virtuella datorer.

Just nu är certifierats Azure Virtual Machines av SAP för SAP HANA skala upp konfigurationer. Skalbar konfigurationer med SAP HANA arbetsbelastningar stöds inte ännu. För SAP HANA med hög tillgänglighet i fall av skala upp konfigurationer finns [hög tillgänglighet för SAP HANA på virtuella Azure-datorer (VM)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).

Om du vill tooget en SAP HANA-instans eller S/4HANA eller BW/4HANA system som distribueras i mycket snabb tid, bör du hello användning av [SAP installation Molnbibliotek](http://cal.sap.com). Det finns mer information om hur du distribuerar till exempel en S/4HANA systemet via SAP CAL på Azure i [handboken](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Allt du behöver toohave är en Azure-prenumeration och en SAP-användare som kan registreras med SAP Molnbibliotek för installation.

## <a name="additional-resources"></a>Ytterligare resurser
### <a name="sap-hana-backup"></a>SAP HANA-säkerhetskopiering
Information om säkerhetskopiering av databaser för SAP HANA på Azure Virtual Machines finns i:
* [Säkerhetskopiering guide för SAP HANA på Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
* [SAP HANA Azure Backup på filnivå](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
* [SAP HANA säkerhetskopia av ögonblicksbilder för lagring](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

### <a name="sap-cloud-appliance-library"></a>SAP Molnbibliotek installation
Information om hur du använder SAP installation Molnbibliotek toodeploy S/4HANA eller BW/4HANA finns [distribuera SAP S/4HANA eller BW/4HANA på Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="sap-hana-supported-operating-systems"></a>SAP HANA-stödda operativsystem
Information om operativsystem som stöds av SAP HANA finns [SAP stöd Obs #2235581 - SAP HANA: operativsystem som stöds](https://launchpad.support.sap.com/#/notes/2235581/E). Virtuella Azure-datorer stöder endast en delmängd av dessa operativsystem. hello är följande operativsystem stöds toodeploy SAP HANA i Azure: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

För ytterligare SAP-dokumentation om SAP HANA och olika Linux-operativsystem, se:

* [Stöd för SAP-kommentar #171356 - SAP-program på Linux: allmän Information](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP stöd Obs #1944799 - SAP HANA riktlinjer för Installation av operativsystemet SLES](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
* [Stöd för SAP-kommentar #2205917 - SAP HANA DB rekommenderade OS-inställningar för SLES 12 för SAP-program](https://launchpad.support.sap.com/#/notes/2205917/E)
* [SAP-stöd kommentar #1984787 - SUSE Linux Enterprise Server 12: Installationsinformation](https://launchpad.support.sap.com/#/notes/1984787)
* [Stöd för SAP-kommentar #1391070 - lösningar för Linux-UUID](https://launchpad.support.sap.com/#/notes/1391070)
* [SAP stöd Obs #2009879 - SAP HANA riktlinjer för Red Hat Enterprise Linux (RHEL)-operativsystem](https://launchpad.support.sap.com/#/notes/2009879)
* [2292690 - SAP HANA DB: rekommenderas OS-inställningar för RHEL 7](https://launchpad.support.sap.com/#/notes/2292690/E)

### <a name="sap-monitoring-in-azure"></a>SAP övervakning i Azure
Information om SAP övervakning i Azure finns i:

* [SAP-kommentar 2191498](https://launchpad.support.sap.com/#/notes/2191498/E). Detta beskrivs SAP ”förbättrad övervakning” med Linux virtuella datorer i Azure. 
* [SAP-kommentar 1102124](https://launchpad.support.sap.com/#/notes/1102124/E). Detta beskrivs information om SAPOSCOL på Linux. 
* [SAP-kommentar 2178632](https://launchpad.support.sap.com/#/notes/2178632/E). Detta beskrivs övervakning nyckelvärden för SAP på Microsoft Azure.

### <a name="azure-vm-types"></a>Azure VM-typer
Azure VM-typer och SAP-stödda arbetsbelastningsscenarier som används med SAP HANA dokumenteras i [SAP certifierade IaaS plattformar](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). 

Azure VM-typer som är certifierade av SAP för SAP NetWeaver eller hello S/4HANA programnivå dokumenteras i [SAP Obs 1928533 - SAP-program i Azure: produkter och Virtuella Azure-typer](https://launchpad.support.sap.com/#/notes/1928533/E).

>[!Note]
>Azure-SAP-Linux integration stöds bara på Azure Resource Manager och inte hello som klassiska distributionsmodellen. 

## <a name="manual-installation-of-sap-hana"></a>Manuell installation av SAP HANA
Den här handboken beskrivs hur toomanually installera SAP HANA på Azure Virtual Machines på två olika sätt:

* Med hjälp av SAP programvara etablering Manager (SWPM) som en del av en distribuerad installation NetWeaver i ”installera databasinstans” hello-steg
* Med hjälp av hello SAP HANA-databas livscykel Enhetshanteraren, HDBLCM och sedan installera NetWeaver

Du kan också använda SWPM tooinstall alla komponenter (SAP HANA hello SAP-programserver och hello ASCS-instans) i en enda virtuell dator, enligt beskrivningen i det här [SAP HANA-bloggen meddelande](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/). Det här alternativet är inte beskrivs i den här snabbstartsguide, men hello problem som du måste ta hänsyn till är hello samma.

Innan du startar en installation, rekommenderar vi att du läser hello ”förbereda Azure virtuella datorer för manuell installation av SAP HANA” senare i den här handboken. Då kan du förhindra flera grundläggande fel som kan uppstå när du använder endast en Azure VM standardkonfiguration.

## <a name="key-steps-for-sap-hana-installation-when-you-use-sap-swpm"></a>Viktiga steg för SAP HANA-installationen när du använder SAP SWPM
Det här avsnittet innehåller hello viktiga steg för installation av en manuell, single instance SAP HANA när du använder SAP SWPM tooperform en distribuerad SAP NetWeaver 7.5 installation. hello enskilda stegen beskrivs i detalj i skärmbilder senare i den här guiden.

1. Skapa ett Azure-nätverk som innehåller två test virtuella datorer.
2. Distribuera hello två virtuella Azure-datorer med operativsystem (i vårt exempel SUSE Linux Enterprise Server (SLES) och SLES för SAP program 12 SP1), enligt toohello Azure Resource Manager-modellen.
3. Koppla två Azure standard eller premium-lagring diskar (till exempel 75 GB eller 500 GB diskar) toohello programserver VM.
4. Ansluta premium storage diskar toohello HANA DB server VM. Mer information finns i avsnittet hello ”Disk installationen” senare i den här guiden.
5. Beroende på storleken eller dataflöde krav, bifoga flera diskar och skapa stripe-volymer med hjälp av hantering av logisk volym eller en administrationsverktyget för flera enheter (MDADM) på hello OS nivå i hello VM.
6. Skapa XFS filsystem på hello anslutna diskar och logiska volymer.
7. Montera hello nya XFS file systems på hello OS-nivå. Använda en filsystem för alla hello SAP-program. Använd hello andra filsystem för hello /sapmnt katalog och säkerhetskopior, t.ex. Montera hello XFS filsystem på hello premium lagringsdiskar som /hana och /usr/sap på hello SAP HANA DB-servern. Det är nödvändigt tooprevent hello rot filsystem, som inte är stor på Linux virtuella Azure-datorer från att fylla.
8. Ange hello lokala IP-adresser i hello test virtuella datorer i hello/etc/hosts-filen.
9. Ange hello **nofail** parameter i filen hello/etc/fstab.
10. Ange Linux kernel-parametrarna enligt toohello Linux OS-versionen som du använder. Mer information finns i hello lämpliga SAP kommentarer som diskuterar HANA och hello ”Kernel parametrar” avsnittet i den här guiden.
11. Lägg till växlingsutrymme.
12. Du kan också installera en grafisk skrivbord på hello test virtuella datorer. Annars Använd en fjärrinstallation av SAPinst.
13. Hämta hello SAP-program från hello SAP Service Marketplace.
14. Installera hello SAP ASCS-instansen på hello app-servern VM.
15. Dela hello /sapmnt directory bland hello testa virtuella datorer med hjälp av NFS. hello-programserver VM är hello NFS-servern.
16. Installera hello-databasinstans, inklusive HANA, med hjälp av SWPM på hello DB server VM.
17. Installera hello primära programservern (PROVIDERADRESSER) på hello programserver VM.
18. Starta SAP-hanteringskonsolen (SAP MC). Ansluta till SAP-Gränssnittet eller HANA Studio, t.ex.

## <a name="key-steps-for-sap-hana-installation-when-you-use-hdblcm"></a>Viktiga steg för SAP HANA-installationen när du använder HDBLCM
Det här avsnittet innehåller hello viktiga steg för installation av en manuell, single instance SAP HANA när du använder SAP HDBLCM tooperform en distribuerad SAP NetWeaver 7.5 installation. hello enskilda stegen beskrivs i detalj i skärmdumpar i den här guiden.

1. Skapa ett Azure-nätverk som innehåller två test virtuella datorer.
2. Distribuera två virtuella Azure-datorer med operativsystem (i vårt exempel SLES och SLES för SAP program 12 SP1) enligt toohello Azure Resource Manager-modellen.
3. Koppla två Azure standard eller premium-lagring diskar (till exempel 75 GB eller 500 GB diskar) toohello applikationsserver VM.
4. Ansluta premium storage diskar toohello HANA DB server VM. Mer information finns i avsnittet hello ”Disk installationen” senare i den här guiden.
5. Beroende på storleken eller dataflöde, bifoga flera diskar och skapa stripe-volymer med hjälp av hantering av logisk volym eller en administrationsverktyget för flera enheter (MDADM) på hello OS nivå i hello VM.
6. Skapa XFS filsystem på hello anslutna diskar och logiska volymer.
7. Montera hello nya XFS file systems på hello OS-nivå. Använd hello andra en för hello /sapmnt directory och säkerhetskopior, till exempel använder en filsystem för alla hello SAP-program. Montera hello XFS filsystem på hello premium lagringsdiskar som /hana och /usr/sap på hello SAP HANA DB-servern. Den här processen är nödvändigt toohelp förhindrar hello rot filsystem, som inte är stor på Linux Azure Virtual Machines, fyller upp.
8. Ange hello lokala IP-adresser i hello test virtuella datorer i hello/etc/hosts-filen.
9. Ange hello **nofail** parameter i filen hello/etc/fstab.
10. Ange parametrarna för kernel enligt toohello Linux OS-versionen som du använder. Mer information finns i hello lämpliga SAP kommentarer som diskuterar HANA och hello ”Kernel parametrar” avsnittet i den här guiden.
11. Lägg till växlingsutrymme.
12. Du kan också installera en grafisk skrivbord på hello test virtuella datorer. Annars Använd en fjärrinstallation av SAPinst.
13. Hämta hello SAP-program från hello SAP Service Marketplace.
14. Skapa en grupp, sapsys, med grupp-ID 1001 på hello HANA DB server VM.
15. Installera SAP HANA på hello DB server-dator med hjälp av HANA databasen livscykel Manager (HDBLCM).
16. Installera hello SAP ASCS-instansen på hello app-servern VM.
17. Dela hello /sapmnt directory bland hello testa virtuella datorer med hjälp av NFS. hello-programserver VM är hello NFS-servern.
18. Installera hello-databasinstans, inklusive HANA, med hjälp av SWPM på hello HANA DB server VM.
19. Installera hello primära programservern (PROVIDERADRESSER) på hello programserver VM.
20. Starta SAP MC. Ansluta till SAP-Gränssnittet eller HANA Studio.

## <a name="preparing-azure-vms-for-a-manual-installation-of-sap-hana"></a>Förbereder virtuella Azure-datorer för en manuell installation av SAP HANA
Det här avsnittet beskriver hello följande avsnitt:

* OS-uppdateringar
* Installationen av disk
* Kernel-parametrar
* Filsystem
* Hej/etc/hosts-filen
* filen hello/etc/fstab

### <a name="os-updates"></a>OS-uppdateringar
Sök efter Linux OS-uppdateringar och korrigeringar innan du installerar ytterligare programvara. Genom att installera en korrigering, kanske du kan tooavoid anropet toohello stöd skrivbord.

Kontrollera att du använder:
* SUSE Linux Enterprise Server för SAP-program.
* Red Hat Enterprise Linux för SAP-program eller Red Hat Enterprise Linux för SAP HANA. 

Om du inte redan gjort registrera hello OS-distribution med Linux-prenumeration från leverantör av hello Linux. Observera att SUSE operativsystemsavbildningar för SAP-program som redan innehåller tjänster och som är registrerade automatiskt.

Här är ett exempel på sökning efter tillgängliga korrigeringar för SUSE Linux med hjälp av hello **zypper** kommando:

 `sudo zypper list-patches`

Beroende på hello typ av problemet, är korrigeringsprogram som klassificerade efter kategori och allvarlighetsgrad. Vanliga värden för kategori: **säkerhet**, **rekommenderas**, **valfria**, **funktionen**, **dokument**, eller **yast**.
Vanliga värden för allvarlighetsgrad: **kritiska**, **viktiga**, **måttlig**, **låg**, eller **ospecificerade**.

Hej **zypper** ser kommandot ut bara hello för programuppdateringar som måste din installerade paket. Du kan till exempel använda det här kommandot:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

Du kan lägga till parametern hello `--dry-run` tootest hello uppdateringen utan att faktiskt uppdatera hello system.


### <a name="disk-setup"></a>Installationen av disk
hello rot file system i en Linux VM på Azure har en storleksbegränsning. Därför är det nödvändigt tooattach ytterligare disk space tooan virtuella Azure-datorn för att köra SAP. Hello användning av Azure standardlagring diskar kan vara tillräcklig för SAP programserver virtuella Azure-datorer. Men för SAP HANA DBMS virtuella Azure-datorer är hello användning av Azure Premium Storage diskar för produktion och icke-produktion implementeringar obligatorisk.

Baserat på hello [lagringskraven för SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), hello följande Azure Premium Storage configuration föreslås: 

| VM-SKU | RAM |  / hana/data och loggfilen/hana / <br /> stripe-volymer med LVM eller MDADM | / hana/delade | / Root volym | / usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

I hello föreslagna diskkonfigurationen placeras hello HANA datavolym och loggvolym på samma uppsättning Azure premium lagringsdiskar stripe med LVM eller MDADM hello. Det är inte nödvändigt toodefine alla RAID-redundans nivå eftersom Azure Premium-lagring för att hålla tre bilder för hello diskar för redundans. toomake som är viktigt att du konfigurerar tillräckligt med utrymme finns hello [lagringskraven för SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) och [uppdatering Guide och SAP HANA-serverinstallation](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm). Även överväga hello annan virtuell hårddisk (VHD) genomströmning mängder hello olika Azure premium lagringsdiskar enligt beskrivningen i [Premium-lagring med hög prestanda och hanterade diskar för virtuella datorer](https://docs.microsoft.com/azure/storage/storage-premium-storage). 

Du kan lägga till flera premium-lagring diskar toohello HANA DBMS virtuella datorer för att lagra säkerhetskopior av databasen eller transaktionsloggen.

Mer information om hello två huvudsakliga verktyg som används för tooconfigure striping finns hello följande artiklar:

* [Konfigurera programvarubaserad RAID på Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Konfigurera LVM på en virtuell Linux-dator i Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Mer information om att koppla diskar tooAzure virtuella datorer som kör Linux som en gäst-OS, finns [lägga till en disk tooa Linux VM](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure Premium-lagring kan du toodefine disk cachelagring lägen. För hello stripe /hana/data och /hana/log ska diskcachelagring inaktiveras. För hello andra volymer (diskar) hello cachelagring läge ska anges för**ReadOnly**.

Mer information finns i [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../../../storage/common/storage-premium-storage.md).

toofind exempel JSON-mallar för att skapa virtuella datorer, gå för[Azure Quickstart mallar](https://github.com/Azure/azure-quickstart-templates).
hello vm-enkel-sles mallen är en grundläggande mall. Den innehåller ett avsnitt för lagring, med en ytterligare 100 GB data-disk. Den här mallen kan användas som bas. Du kan anpassa hello mallen tooyour viss konfiguration.

>[!Note]
>Det är viktigt tooattach hello Azure storage disk med hjälp av en UUID som beskrivs i [kör SAP NetWeaver på Microsoft Azure SUSE Linux virtuella datorer](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

I hello testmiljö har två Azure standardlagring diskar anslutna toohello SAP app-servern VM, som visas i följande skärmbild hello. En disk lagras alla hello SAP-program (inklusive NetWeaver 7.5, SAP-GUI och SAP HANA) för installation. hello andra disken sett att tillräckligt med ledigt utrymme är tillgängligt för ytterligare krav (till exempel säkerhetskopierings- och data) och för hello /sapmnt directory (det vill säga SAP-profiler) toobe delas med alla virtuella datorer som tillhör toohello samma SAP liggande.

![SAP HANA app server diskar fönster som visar två diskar och deras storlek](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Kernel-parametrar
SAP HANA kräver särskilda Linux kernel-inställningar som inte är en del av hello standard Azure-galleriet bilder och måste anges manuellt. Hello parametrarna kan vara olika beroende på om du använder SUSE eller Red Hat. hello SAP anteckningar ovan ger information om dessa parametrar. I hello skärmbilder visas, användes SUSE Linux 12 SP1. 

SLES för SAP program 12 GA och SLES för SAP program 12 SP1 har du ett nytt verktyg **justerade adm**, som ersätter hello gamla **sapconf** verktyget. En speciell profil för SAP HANA är tillgänglig för **justerade adm**. tootune hello system för SAP HANA ange hello följande som rotanvändare:

   `tuned-adm profile sap-hana`

Mer information om **justerade adm**, se hello [SUSE dokumentationen om justerade adm](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

I följande skärmbild hello, kan du se hur **justerade adm** ändrade hello `transparent_hugepage` och `numa_balancing` värden, enligt toohello nödvändiga SAP HANA-inställningar.

![hello justerade adm verktyget ändras värden enligt toorequired SAP HANA-inställningar](./media/hana-get-started/image005.jpg)

toomake hello SAP HANA kernel inställningar för permanent **grub2** på SLES 12. Mer information om **grub2**, gå toohello [Configuration filstruktur](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) avsnitt i hello SUSE dokumentation.

hello följande skärmbild visar hur hello kernel inställningar har ändrats i hello konfigurationsfilen och sedan kompileras med hjälp av **grub2 mkconfig**:

![Kernel-inställningar har ändrats i hello konfigurationsfilen och kompileras med hjälp av grub2 mkconfig](./media/hana-get-started/image006.jpg)

Ett annat alternativ är toochange hello inställningar med hjälp av YaST och hello **startprogram** > **Kernel parametrar** inställningar:

![hello Kernel parametrar inställningsfliken i YaST startprogram](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>Filsystem
hello visar följande skärmbild två filsystem som har skapats på hello SAP app-servern VM ovanpå hello två anslutna Azure standardlagring diskar. Båda filsystem är av typen XFS som är monterade för/sapdata och /sapsoftware.

Det är inte obligatoriskt toostructure din filsystem i det här sättet. Du har andra alternativ för att strukturera hello diskutrymme. hello mest viktigt övervägande är tooprevent hello rot filsystem från slut på ledigt utrymme.

![Två filsystem som skapats på hello SAP app-servern VM](./media/hana-get-started/image008.jpg)

Hello SAP HANA DB VM under installationen av en databas, när du använder SAPinst (SWPM) och hello **vanliga** installationsalternativ, allt installeras under /hana och /usr/sap. hello standardplatsen för säkerhetskopiering av hello SAP HANA-loggen är under /usr/sap. Igen, eftersom det är viktigt tooprevent hello rot filsystem från slut på utrymme i lagringspoolen, se till att det finns tillräckligt med ledigt diskutrymme under /hana och /usr/sap innan du installerar SAP HANA med hjälp av SWPM.

En beskrivning av hello standard filsystemet layouten för SAP HANA finns hello [uppdatering Guide och SAP HANA-serverinstallation](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm).

![Ytterligare filsystem som skapats på hello SAP app-servern VM](./media/hana-get-started/image009.jpg)

När du installerar SAP NetWeaver på en standard SLES/SLES för SAP program 12 Azure-galleriet avbildningen visas ett meddelande om att det inte finns några växlingsutrymme som visas i följande skärmbild hello. toodismiss detta meddelande, du kan lägga till en växlingsfil manuellt med hjälp av **dd**, **mkswap**, och **swapon**. toolearn hur, söka efter ”lägga till en växlingsfil manuellt” i hello [Using hello YaST Partitioneraren](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) avsnitt i hello SUSE dokumentation.

Ett annat alternativ är tooconfigure växlingsutrymme med hello Linux VM-agenten. Mer information finns i hello [användarhandboken för Azure Linux-agenten](../../linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

![Popup-meddelande som talar om att det finns inte tillräckligt växlingsutrymme](./media/hana-get-started/image010.jpg)


### <a name="hello-etchosts-file"></a>Hej/etc/hosts-filen
Innan du börjar tooinstall SAP, kontrollera att du inkluderar hello värdnamn och IP-adresser för hello SAP virtuella datorer i hello/etc/hosts-filen. Distribuera alla hello SAP VM: ar inom ett virtuellt Azure-nätverk och sedan använda hello interna IP-adresser, som visas här:

![Värdnamn och IP-adresser för hello SAP virtuella datorer som finns i hello/etc/hosts-filen](./media/hana-get-started/image011.jpg)

### <a name="hello-etcfstab-file"></a>filen hello/etc/fstab

Det är bra tooadd hello **nofail** parameterfilen toohello fstab. Det här sättet om något går fel med hello diskar hello VM inte låser sig i hello startprocessen. Men kom ihåg att ytterligare diskutrymme inte kanske är tillgänglig och processer kan fylla hello rot-filsystemet. SAP HANA startar inte om /hana saknas.

![Lägg till hello nofail parametern toohello fstab fil](./media/hana-get-started/image000c.jpg)

## <a name="graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>Den grafiska gör väldigt lätt skrivbordet på SLES 12/SLES för SAP program 12
Det här avsnittet beskriver hello följande avsnitt:

* Installera hello gör väldigt lätt skrivbords- och xrdp på SLES 12/SLES för SAP program 12
* Kör Java-baserad SAP MC med Firefox i SLES 12/SLES för SAP program 12

Du kan också använda alternativ, till exempel Xterminal eller VNC (som inte beskrivs i den här guiden).

### <a name="installing-hello-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>Installera hello gör väldigt lätt skrivbords- och xrdp på SLES 12/SLES för SAP program 12
Om du har en Windows-bakgrund, kan du enkelt använda en grafisk desktop direkt i hello SAP virtuella Linux-datorer toorun Firefox SAPinst, SAP GUI, SAP MC eller HANA Studio och ansluta toohello VM via hello Remote Desktop Protocol (RDP) från en Windows-dator. Beroende på företagets principer för hur du lägger till grafiska användargränssnitt tooproduction och icke-produktion Linux-baserade system kan du vilja ha tooinstall gör väldigt lätt på servern. tooinstall hello gör väldigt lätt skrivbord på ett Azure SLES 12/SLES för SAP program 12 VM:

1. Installera hello gör väldigt lätt skrivbordet genom att ange hello följande kommando (till exempel i en PuTTY fönster):

   `zypper in -t pattern gnome-basic`

2. Installera xrdp tooallow en anslutning toohello VM via RDP:

   `zypper in xrdp`

3. Redigera /etc/sysconfig/windowmanager och ange hello standard fönstret manager tooGNOME:

   `DEFAULT_WM="gnome"`

4. Kör **chkconfig** toomake till att xrdp startas automatiskt efter en omstart:

   `chkconfig -level 3 xrdp on`

5. Om du har ett problem med hello RDP-anslutning du försök toorestart (från PuTTY fönster, till exempel):

   `/etc/xrdp/xrdp.sh restart`

6. Om en omstart xrdp som nämns i hello tidigare steg inte fungerar, kontrollera en .pid-fil:

   `check /var/run` 

   Leta efter `xrdp.pid`. Om du hittar det kan ta bort den och försök toorestart igen.

### <a name="starting-sap-mc"></a>Från SAP MC
När du har installerat hello gör väldigt lätt skrivbordet kanske startar hello grafiska Java-baserad SAP MC från Firefox när det körs i en Azure SLES 12/SLES för SAP program 12 VM visas ett fel på grund av hello saknas Java-webbläsaren plugin-programmet.

hello URL toostart hello SAP MC är `<server>:5<instance_number>13`.

Mer information finns i [Start hello webbaserade SAP-hanteringskonsolen](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm).

hello visar följande skärmbild hello felmeddelande som visas när hello Java-webbläsaren plugin saknas:

![Felmeddelande som anger saknas Java-webbläsaren plugin-program](./media/hana-get-started/image013.jpg)

Enkelriktade toosolve hello problemet är tooinstall hello saknas plugin-program med hjälp av YaST, som visas i följande skärmbild hello:

![Med hjälp av YaST tooinstall saknas plugin-program](./media/hana-get-started/image014.jpg)

När du har angett hello SAP Management-konsolens URL, visas ett meddelande frågar du tooactivate hello plugin-program:

![Dialogrutan begär plugin-aktivering](./media/hana-get-started/image015.jpg)

Du kan också få ett felmeddelande om en saknad fil javafx.properties. Detta är relaterade toohello behovet av Oracle Java 1.8 SAP GUI 7.4. (Se [SAP-kommentar 2059429](https://launchpad.support.sap.com/#/notes/2059424).) Varken hello IBM Java-version eller hello openjdk paket levereras med SLES/SLES för SAP program 12 innehåller hello nödvändiga javafx.properties-fil. hello-lösning är toodownload och installera Java SE 8 från Oracle.

Information om ett liknande problem med openjdk på openSUSE finns hello konversation [SAPGui 7.4 Java för openSUSE 42.1 Leap](https://scn.sap.com/thread/3908306).

## <a name="manual-installation-of-sap-hana-swpm"></a>Manuell installation av SAP HANA: SWPM
hello serie med skärmbilder i det här avsnittet visar hello viktiga steg för att installera SAP NetWeaver 7.5 och SAP HANA SP12 när du använder SWPM (SAPinst). Som en del av en installation av NetWeaver 7.5 Installera SWPM även hello HANA-databas som en enda instans.

I en testmiljö där exempel installerat vi en avancerad Business Application Programming (ABAP) app-servern. I följande skärmbild hello visas vi använde hello **distribuerade System** alternativet tooinstall hello ASCS och primära server-instanser i en virtuell dator i Azure och SAP HANA som hello databassystemet i en annan virtuell dator i Azure.

![ASCS och primär server programinstanser installeras med hjälp av alternativet för hello distribuerade System](./media/hana-get-started/image012.jpg)

När hello ASCS-instansen är installerad på hello app-servern VM och är måste inställd för ”grön” i hello SAP-hanteringskonsolen (visas i följande skärmbild hello) hello /sapmnt katalogen (inklusive hello SAP profil directory) delas med hello SAP HANA DB server VM. hello DB installationssteget behöver komma åt toothis information. hello bästa sätt tooprovide åtkomst är toouse NFS, som kan konfigureras med hjälp av YaST.

![SAP-hanteringskonsolen som visar hello ASCS-instans installerad på hello app-servern VM och ange för ”grön”](./media/hana-get-started/image016.jpg)

På hello app-servern VM hello/sapmnt directory bör delas via NFS med hjälp av hello **rw** och **no_root_squash** alternativ. hello standardvärdena är **ro** och **root_squash**, vilket kan innebära tooproblems när du installerar hello-databasinstansen.

![Dela hello /sapmnt katalog via NFS med hjälp av hello rw och no_root_squash alternativ](./media/hana-get-started/image017b.jpg)

Som visar hello nästa skärmbild hello /sapmnt resursen från VM hello app-servern måste konfigureras på hello SAP HANA DB server VM med hjälp av **NFS-klienten** (och YaST).

![Hej /sapmnt resursen konfigureras med hjälp av NFS-klient](./media/hana-get-started/image018b.jpg)

tooperform en distribuerad NetWeaver 7.5 installation (**databasinstans**), som visas i hello följande skärmbild, logga in toohello SAP HANA-databasserver VM och starta SWPM.

![Installera en databasinstans genom att logga in toohello SAP HANA-databasserver VM och starta SWPM](./media/hana-get-started/image019.jpg)

När du har valt **vanliga** installation och hello sökvägen toohello installationsmediet ange DB SID, hello värdnamn, hello instansnummer och hello DB lösenord.

![sidan hello SAP HANA-databas system administratören logga in](./media/hana-get-started/image035b.jpg)

Ange hello lösenord för hello DBACOCKPIT schema:

![hello lösenord-rutan för hello DBACOCKPIT schema](./media/hana-get-started/image036b.jpg)

Ange en fråga för hello SAPABAP1 schemat lösenord:

![Ange en fråga för hello SAPABAP1 schemat lösenord](./media/hana-get-started/image037b.jpg)

Efter varje aktivitet har slutförts visas en grön bock nästa tooeach fas i hello DB installationsprocessen. hello-meddelande ”körning av... Databasen instans har slutförts ”visas.

![Slutföra uppgiften fönstret bekräftelsemeddelande](./media/hana-get-started/image023.jpg)

Efter installationen bör hello SAP-hanteringskonsolen också visa hello DB-instans som ”grönt” och visa hello fullständig lista över processer för SAP HANA (hdbindexserver, hdbcompileserver och så vidare).

![SAP-hanteringskonsolen fönster med listan över SAP HANA-processer](./media/hana-get-started/image024.jpg)

hello visar följande skärmbild hello delar av hello filstruktur under hello /hana/shared katalog som SWPM skapade under hello HANA installation. Eftersom det finns inga alternativ toospecify en annan sökväg, är viktiga toomount ytterligare diskutrymme under hello /hana katalogen innan hello SAP HANA-installation med hjälp av SWPM. Detta förhindrar att hello rot filsystemet utrymmet tar slut.

![Hej /hana/shared filen katalogstruktur skapas under installationen av HANA](./media/hana-get-started/image025.jpg)

Den här skärmbilden visar hello filstruktur för hello /usr/sap katalog:

![Hej /usr/sap directory filstruktur](./media/hana-get-started/image026.jpg)

hello sista steget i hello distribuerade ABAP installation är tooinstall hello primära server-instans:

![ABAP installationen visar primära server-instans som hello sista steget](./media/hana-get-started/image027b.jpg)

När du har installerat hello primära server-instans och SAP GUI använda hello **DBA Cockpit** transaktion tooconfirm som hello SAP HANA-installationen har slutförts korrekt:

![DBA Cockpit fönstret bekräftar att installationen lyckades](./media/hana-get-started/image028b.jpg)

Som ett sista steg kan du vill installera toofirst HANA Studio i hello SAP app-servern VM och ansluta toohello SAP HANA-instans som körs på hello DB server VM:

![Installera SAP HANA Studio i hello SAP applikationsserver VM](./media/hana-get-started/image038b.jpg)

## <a name="manual-installation-of-sap-hana-hdblcm"></a>Manuell installation av SAP HANA: HDBLCM
I tillägg tooinstalling SAP HANA som en del av en distribuerad installation med hjälp av SWPM, kan du installera hello HANA fristående först med hjälp av HDBLCM. Du kan sedan installera SAP NetWeaver 7.5, t.ex. hello skärmdumpar i det här avsnittet visar hur den här processen fungerar.

Mer information om hello HANA HDBLCM verktyget finns på:

* [Att välja hello korrigera SAP HANA HDBLCM för din aktivitet](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)
* [Hanteringsverktyg för SAP HANA livscykel](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)
* [Guide för uppdatering och SAP HANA-serverinstallation](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)

tooavoid problem med en standard gruppinställningen ID för hello `\<HANA SID\>adm user` (som skapats av hello HDBLCM tool) definiera en ny grupp som kallas `sapsys` med hjälp av grupp-ID `1001` innan du installerar SAP HANA via HDBLCM:

![Ny grupp ”sapsys” definierats med hjälp av grupp-ID 1001](./media/hana-get-started/image030.jpg)

När du startar HDBLCM hello första gången visas ett enkelt start-menyn. Välj objekt 1, **installera nya**som visas i följande skärmbild hello:

![”Installera nya system” alternativ i hello HDBLCM starta fönstret](./media/hana-get-started/image031.jpg)

hello visar följande skärmbild alla viktiga hello-alternativ som du valde tidigare.

> [!IMPORTANT]
> Kataloger som heter för HANA loggen och datavolymer, samt hello installationssökvägen (hana/delade i det här exemplet) och /usr/sap, får inte vara en del av hello rot-filsystemet. De här katalogerna hör toohello Azure datadiskar som var anslutna toohello VM (beskrivs i hello ”Disk installation”). Den här metoden förhindrar hello rot filsystemet utrymmet tar slut. I följande skärmbild hello, ser du att hello HANA systemadministratören har användar-ID `1005` och är en del av hello `sapsys` grupp (ID `1001`) som har definierats innan hello-installationen.

![Lista över alla nyckelkomponenterna för SAP HANA som tidigare markerade certifikatmallen](./media/hana-get-started/image032.jpg)

Du kan kontrollera hello `\<HANA SID\>adm user` (`azdadm` i följande skärmbild hello) information i hello/etc/passwd directory:

![HANA \<HANA SID\>adm användarinformation som anges i hello/etc/passwd directory](./media/hana-get-started/image033.jpg)

När du har installerat SAP HANA med hjälp av HDBLCM ser hello filstruktur i SAP HANA Studio som visas i följande skärmbild hello. Hej SAPABAP1 schema, som innehåller alla hello SAP NetWeaver tabeller, är inte tillgänglig ännu.

![Filstruktur för SAP HANA i SAP HANA Studio](./media/hana-get-started/image034.jpg)

När du har installerat SAP HANA kan du installera SAP NetWeaver ovanpå den. Som visas i hello följande skärmbild, utfördes hello installationen som en distribuerad installation med hjälp av SWPM (enligt beskrivningen i föregående avsnitt i hello). När du installerar hello-databasinstans med hjälp av SWPM kan du ange hello samma data med hjälp av HDBLCM (till exempel värdnamn, HANA SID och instansnummer). SWPM sedan använder hello befintlig HANA installation och lägger till flera scheman.

![En distribuerad installation utförs med hjälp av SWPM](./media/hana-get-started/image035b.jpg)

hello följande skärmbild visar hello SWPM installationssteget där du kan ange information om hello DBACOCKPIT schema:

![Hej SWPM installationssteget där DBACOCKPIT schemadata anges](./media/hana-get-started/image036b.jpg)

Ange information om hello SAPABAP1 schema:

![Ange data om hello SAPABAP1 schemat](./media/hana-get-started/image037b.jpg)

När hello SWPM instans installationen har slutförts kan se du hello SAPABAP1 schemat i SAP HANA Studio:

![Hej SAPABAP1 schemat i SAP HANA Studio](./media/hana-get-started/image038b.jpg)

Slutligen när hello SAP app-servern och SAP GUI-installationer har slutförts, du kan verifiera hello HANA DB-instans med hjälp av hello **DBA Cockpit** transaktion:

![hello HANA DB-instans som verifieras med hello DBA Cockpit transaktion](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>Hämtar för SAP-program
Du kan hämta programvara från hello SAP Service Marketplace enligt hello följande skärmdumpar.

Hämta NetWeaver 7.5 för Linux/HANA:

 ![SAP Service Installation och uppgradering fönster för att ladda ned NetWeaver 7.5](./media/hana-get-started/image001.jpg)

Ladda ner HANA SP12 plattform versionen:

 ![SAP Service Installation och uppgradering fönster för att ladda ned HANA SP12 plattform Edition](./media/hana-get-started/image002.jpg)

