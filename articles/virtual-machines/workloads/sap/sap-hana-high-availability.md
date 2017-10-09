---
title: "aaaHigh tillgänglighet för SAP HANA på Azure Virtual Machines (virtuella datorer) | Microsoft Docs"
description: "Skapa hög tillgänglighet för SAP HANA på virtuella Azure-datorer (VM)."
services: virtual-machines-linux
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: sedusch
ms.openlocfilehash: dcb9bb70594f9d97f8a888cec76300bcbe0bf1ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a>Hög tillgänglighet för SAP HANA på virtuella Azure-datorer (VM)

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

Lokalt, kan du använda antingen HANA System replikering eller använda delad lagring tooestablish hög tillgänglighet för SAP HANA.
Vi stöds för närvarande bara ställa in HANA System replikering på Azure. SAP HANA replikering består av en nod och minst en slav nod. Ändringar toohello data på hello huvudnoden replikerade toohello underordnade noder synkront eller asynkront.

Den här artikeln beskriver hur toodeploy hello virtuella datorer, konfigurera hello virtuella datorer, installera hello klustret framework, installera och konfigurera replikering för SAP HANA-System.
Installationen kommandon etc. instansnummer 03 i hello Exempelkonfigurationer och HANA System-ID HDB används.

Läs hello först efter SAP anteckningar och dokument

* SAP-kommentar [1928533], som innehåller:
  * Lista över Azure VM-storlekar som stöds för hello distributionen av program
  * Viktiga kapacitetsinformation för Azure VM-storlekar
  * Stöds SAP-programvara och operativsystem (OS) och kombinationer av databasen
  * SAP kernel version som krävs för Windows och Linux i Microsoft Azure
* SAP-kommentar [2015553] listar kraven för stöd för SAP SAP programvarudistributioner i Azure.
* SAP-kommentar [2205917] har rekommenderat OS-inställningar för SUSE Linux Enterprise Server för SAP-program
* SAP-kommentar [1944799] har SAP HANA riktlinjer för SUSE Linux Enterprise Server för SAP-program
* SAP-kommentar [2178632] innehåller detaljerad information om all övervakning mått som rapporterats för SAP i Azure.
* SAP-kommentar [2191498] hello krävs SAP värden Agent-version för Linux i Azure.
* SAP-kommentar [2243692] har licensieringsinformation SAP på Linux i Azure.
* SAP-kommentar [1984787] har allmän information om SUSE Linux Enterprise Server 12.
* SAP-kommentar [1999351] innehåller ytterligare felsökningsinformation för hello Azure förbättrad övervakning av tillägget för SAP.
* [SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) har alla nödvändiga SAP anteckningar för Linux.
* [Azure virtuella datorer planering och implementering för SAP på Linux][planning-guide]
* [Distribution av Azure virtuella datorer för SAP på Linux (den här artikeln)][deployment-guide]
* [Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide]
* [Scenario för SAP HANA SR prestanda optimerade] [ suse-hana-ha-guide] hello guiden innehåller all nödvändig information tooset SAP HANA System replikering på lokalt. Använd den här guiden som utgångspunkt.

## <a name="deploying-linux"></a>Distribuera Linux

hello resurs agent för SAP HANA ingår i SUSE Linux Enterprise Server för SAP-program.
hello Azure Marketplace innehåller en bild för SUSE Linux Enterprise Server för SAP program 12 med BYOS (ta med din egen prenumeration) som du kan använda toodeploy nya virtuella datorer.

### <a name="manual-deployment"></a>Manuell distribution

1. Skapa en resursgrupp
1. Skapa ett virtuellt nätverk
1. Skapa två Storage-konton
1. Skapa en Tillgänglighetsuppsättning  
   Ange högsta uppdateringsdomän
1. Skapa en belastningsutjämnare (internt)  
   Välj VNET i steget ovan
1. Skapa virtuell dator 1  
   https://Portal.Azure.com/#Create/SUSE-byos.SLES-for-SAP-byos12-SP1  
   SLES för SAP program 12 SP1 (BYOS)  
   Välj Lagringskonto 1  
   Välj Tillgänglighetsuppsättning  
1. Skapa virtuell dator 2  
   https://Portal.Azure.com/#Create/SUSE-byos.SLES-for-SAP-byos12-SP1  
   SLES för SAP program 12 SP1 (BYOS)  
   Välj Lagringskonto 2   
   Välj Tillgänglighetsuppsättning  
1. Lägg till Datadiskar
1. Konfigurera hello belastningsutjämnare
    1. Skapa en klientdel IP-adresspool
        1. Öppna hello belastningsutjämnare, Välj klientdelens IP-pool och klicka på Lägg till
        1. Ange hello namnet på hello ny klientdelens IP-pool (till exempel hana-klientdel)
       1. Klicka på OK
        1. När du har skapat hello ny klientdelens IP-pool Skriv ned dess IP-adress
    1. Skapa en serverdelspool
        1. Öppna hello belastningsutjämnare, Välj serverdelspooler och klicka på Lägg till
        1. Ange hello namnet på hello nya serverdelspool (till exempel hana-serverdel)
        1. Klicka på Lägg till en virtuell dator
        1. Välj hello Tillgänglighetsuppsättning du skapade tidigare
        1. Välj hello virtuella datorer i hello SAP HANA-kluster
        1. Klicka på OK
    1. Skapa en hälsoavsökningen
       1. Öppna hello belastningsutjämnare, Välj hälsoavsökningar och klicka på Lägg till
        1. Ange hello namnet på hello nya hälsoavsökningen (till exempel hana-hp)
        1. Välj TCP som protokoll, port 625**03**, hålla intervallet 5 och tröskelvärde för ohälsosamt värde 2
        1. Klicka på OK
    1. Skapa regler för belastningsutjämning
        1. Öppna hello belastningsutjämnare, Välj regler för belastningsutjämning och klicka på Lägg till
        1. Ange hello namnet på hello ny regel för belastningsutjämnare (till exempel hana-lb-3**03**15)
        1. Välj hello klientdelens IP-adress, serverdelspool och hälsa avsökning du skapade tidigare (till exempel hana-klientdel)
        1. Håll protokollet TCP, ange port 3**03**15
        1. Öka tidsgränsen för inaktivitet too30 minuter
       1. **Se till att tooenable flytande IP**
        1. Klicka på OK
        1. Upprepa hello stegen ovan för port 3**03**17

### <a name="deploy-with-template"></a>Distribuera med mall
Du kan använda någon av hello Snabbstart mallar på github toodeploy alla nödvändiga resurser. hello mallen distribuerar hello virtuella datorer, hello belastningsutjämnare, tillgänglighetsuppsättning osv. Följ dessa steg toodeploy hello mallen:

1. Öppna hello [databasen mallen] [ template-multisid-db] eller hello [konvergerat mallen] [ template-converged] på hello Azure-portalen skapar hello databasen mallen endast hello regler för belastningsutjämning för en databas hello medan hello konvergerade mallen skapar också regler för belastningsutjämning för en ASCS/SCS och ÄNDARE (endast Linux)-instans. Om du planerar tooinstall ett SAP NetWeaver baserat system och du bör också tooinstall hello ASCS/SCS-instans på hello samma datorer, Använd hello [konvergerat mallen][template-converged].
1. Ange följande parametrar hello
    1. SAP System-Id  
       Ange hello SAP system Id hello SAP-system som du vill tooinstall. hello Id ska användas som ett prefix för hello resurser som distribueras.
    1. Stacken typ (gäller endast om du använder hello konvergerade mall)  
       Ange hello SAP NetWeaver stack
    1. OS-typen  
       Välj en av hello Linux-distributioner. Det här exemplet väljer du SLES 12 BYOS
    1. DB-typ  
       Välj HANA
    1. Storlek för SAP-System  
       hello mängden SAP hello nya system ger. Om du inte är säker på hur många SAP hello system kräver be din SAP-teknikpartner eller systemintegreraren
    1. Systemets tillgänglighet  
       Välj hög tillgänglighet
    1. Användarnamn och lösenord för Admin administratör  
       En ny användare skapas som kan vara används toolog på toohello datorn.
    1. Ny eller befintlig undernät  
       Avgör om ett nytt virtuellt nätverk och undernät som ska skapas eller ett befintligt undernät som ska användas. Om du redan har ett virtuellt nätverk som är anslutna tooyour lokala nätverk väljer du befintliga.
    1. Undernät-Id  
    hello-ID för virtuella datorer i hello undernät toowhich hello ska anslutas till. Välj hello undernätet för din VPN eller Expressroute virtuellt nätverk tooconnect hello tooyour lokalt nätverk för virtuella datorer. hello ID ser oftast ut så /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

## <a name="setting-up-linux-ha"></a>Konfigurera Linux hög tillgänglighet

med antingen [A] - tillämpliga tooall noder, [1] – gäller endast toonode 1 eller [2] - gäller endast toonode 2 prefixet hello följande objekt.

1. [A] SLES SAP BYOS endast - registrera SLES toobe kan toouse hello databaser
1. [A] SLES för SAP BYOS endast - Lägg till offentliga moln modul
1. [A] uppdatera SLES
    ```bash
    sudo zypper update

    ```

1. [1] aktivera ssh-åtkomst
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. [2] aktivera ssh-åtkomst
    ```bash
    sudo ssh-keygen -tdsa

    # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. [1] aktivera ssh-åtkomst
    ```bash
    # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. [A] installera tillägg för hög tillgänglighet
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. [A] disklayouten för installationsprogrammet för
    1. LVM  
    Vi rekommenderar vanligtvis toouse LVM för volymer som lagrar data och loggfiler. hello exemplet nedan förutsätter att hello virtuella datorer har fyra datadiskar som är anslutna som ska använda toocreate två volymer.
        * Skapa fysiska volymer för alla diskar som du vill toouse.
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * Skapa en volym grupp för hello-datafiler, en volym grupp för hello loggfiler och en för hello delade katalogen för SAP HANA
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * Skapa hello logiska volymer
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * Skapa hello montera kataloger och kopiera hello UUID för alla logiska volymer
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down hello id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * Skapa fstab poster för hello tre logiska volymer
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    Infoga den här raden för/etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * Montera hello nya volymer
    <pre><code>
    sudo mount -a
    </code></pre>
    1. Vanlig diskar  
       För små eller demonstrera system, du kan placera HANA data och loggfilen filer på en disk. hello följande kommandon skapar en partition på /dev/sdc och formatera den med xfs.
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-hello-id-of-devsdc1"></a>Skriv ned hello-id för /dev/sdc1
    sudo/sbin/blkid sudo vi/etc/fstab
    ```

    Insert this line too/etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create hello target directory and mount hello disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. [A] konfigurera namnmatchning för alla värdar  
    Du kan använda en DNS-server, eller så kan du ändra hello/etc/hosts på alla noder. Det här exemplet visar hur toouse hello/etc/hosts-filen.
   Ersätt hello IP-adress och hello värdnamn i hello följande kommandon
    ```bash
    sudo vi /etc/hosts
    ```
    Infoga hello följande rader för/etc/hosts. Ändra hello IP-adress och värddatornamn toomatch din miljö    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. [1] installera kluster
    ```bash
    sudo ha-cluster-init
    
    # Do you want toocontinue anyway? [y/N] -> y
    # Network address toobind too(e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish toouse SBD? [y/N] -> N
    # Do you wish tooconfigure an administration IP? [y/N] -> N
    ```
        
1. [2] Lägg till nod toocluster
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured toostart at system boot.
    # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
    # Do you want toocontinue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. [A] ändra hacluster lösenord toohello samma lösenord
    ```bash
    sudo passwd hacluster
    
    ```

1. [A] konfigurerar corosync toouse andra transport och lägger till nodelist. Klustret fungerar inte på annat sätt.
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

    Lägg till följande fetstil innehåll toohello hello.
    
    <pre><code> 
    [...]
      interface { 
          [...] 
      }
      <b>transport:      udpu</b>
    } 
    <b>nodelist {
      node {
        ring0_addr:     < ip address of node 1 >
      }
      node {
        ring0_addr:     < ip address of node 2 > 
      } 
    }</b>
    logging {
      [...]
    </code></pre>

    Starta om tjänsten för hello corosync

    ```bash
    sudo service corosync restart
    
    ```

1. [A] Installationspaketen HANA hög tillgänglighet  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a>Installera SAP HANA

Följ kapitel 4 i hello [SAP HANA SR prestanda optimerade scenariot guiden] [ suse-hana-ha-guide] tooinstall SAP HANA System replikering.

1. [A] kör hdblcm från hello HANA DVD
    * Välj installation -> 1
    * Välj ytterligare komponenter för installation -> 1
    * Ange installationssökväg [/ hana/delade]: RETUR ->
    * Ange lokala värdnamn [.]: -> RETUR
    * Vill du tooadd ytterligare värdar toohello system? (j/n) [n]: RETUR ->
    * Ange SAP HANA System-ID:<SID of HANA e.g. HDB>
    * Ange instansnummer [00]:   
  HANA instansnummer. Använd 03 om du har använt hello Azure-mall eller följt hello-exemplet ovan
    * Välj läge för databasen / ange Index [1]: -> RETUR
    * Välj systemanvändning / ange Index [4]:  
  Välj hello system användning
    * Ange platsen för datavolymer [/ hana/data/HDB]: RETUR ->
    * Ange platsen för Loggvolymer [/ hana/log/HDB]: RETUR ->
    * Begränsa maximal minnesallokering? [n]: RETUR ->
    * Ange värdnamnet för certifikat för värden ”...” [...]: RETUR ->
    * Ange SAP värden Agent användare (sapadm) lösenord:
    * Bekräfta SAP värden Agent användare (sapadm) lösenord:
    * Ange systemadministratören (hdbadm) lösenord:
    * Bekräfta systemadministratören (hdbadm) lösenord:
    * Ange System administratör arbetskatalog [/ usr/sap/HDB/hem]: RETUR ->
    * Ange System inloggningsgränssnitt för administratör [/ bin/del]: RETUR ->
    * Ange systemadministratören användar-ID [1001]: -> RETUR
    * Ange ID för användargrupp (sapsys) [79]: RETUR ->
    * Ange lösenord för användare (SYSTEM) av databasen:
    * Bekräfta databas användarlösenord (SYSTEM):
    * Starta om systemet efter omstart av datorn? [n]: RETUR ->
    * Vill du toocontinue? (j/n):  
  Kontrollera hello sammanfattning och ange y toocontinue
1. [A] uppgradera SAP Värdagenten  
  Hämta hello senaste SAP Värdagenten Arkiv från hello [SAP Softwarecenter] [ sap-swcenter] och kör hello följande kommando tooupgrade hello agent. Ersätt hello sökvägen toohello toopoint toohello arkivfilen du laddade ned.
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path tooSAP Host Agent SAR>
    ```

1. [1] Skapa HANA replikering (som rot)  
    Kör följande kommando hello. Kontrollera att tooreplace fetstil strängar (HANA System-ID HDB och instans antalet 03) med hello värdena för SAP HANA-installationen.
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN too<b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. [A] Skapa keystore-post (som rot)
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. [1] backup database (som rot)
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. [1] växla toohello sapsid användare (till exempel hdbadm) och skapa hello primär plats.
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. [2] växla toohello sapsid användare (till exempel hdbadm) och skapa hello sekundär plats.
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a>Konfigurera klustret Framework

Ändra standardinställningarna för hello

<pre>
sudo vi crm-defaults.txt
# enter hello following toocrm-defaults.txt
<code>
property $id="cib-bootstrap-options" \
  no-quorum-policy="ignore" \
  stonith-enabled="true" \
  stonith-action="reboot" \
  stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
  resource-stickiness="1000" \
  migration-threshold="5000"
op_defaults $id="op-options" \
  timeout="600"
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>Vi kan nu läsa in hello toohello kluster
sudo crm konfigurera belastningen update crm-defaults.txt
</pre>

### <a name="create-stonith-device"></a>Skapa STONITH enhet

Hej STONITH enhet använder ett huvudnamn för tjänsten tooauthorize mot Microsoft Azure. Följ dessa steg toocreate ett huvudnamn för tjänsten.

1. Gå för<https://portal.azure.com>
1. Öppna hello Azure Active Directory-bladet  
   Gå tooProperties och anteckna hello Directory-Id. Detta är hello **klient-id**.
1. Klicka på appen registreringar
1. Klicka på Lägg till
1. Ange ett namn, Välj typ av program ”Web app/API”, ange en inloggnings-URL (till exempel http://localhost) och klicka på Skapa
1. hello inloggnings-URL används inte och kan vara en giltig URL
1. Välj hello nya App och klicka på nycklarna i hello på fliken Inställningar
1. Ange en beskrivning för en ny nyckel, Välj ”upphör aldrig att gälla” och klicka på Spara
1. Skriv ned hello värde. Den används som hello **lösenord** för hello tjänstens huvudnamn
1. Skriv ned hello program-Id. Den används som hello användarnamn (**inloggnings-id** i hello stegen nedan) för hello tjänstens huvudnamn

hello tjänstens huvudnamn har inte behörigheter tooaccess Azure-resurser som standard. Du behöver toogive hello tjänstens huvudnamn behörigheter toostart och stoppa (frigöra) alla virtuella datorer i hello klustret.

1. Gå toohttps://portal.azure.com
1. Öppna hello bladet för alla resurser
1. Välj hello virtuell dator
1. Klicka på åtkomstkontroll (IAM)
1. Klicka på Lägg till
1. Välj hello roll ägare
1. Ange hello namnet på hello-program som du skapade ovan
1. Klicka på OK

När du har redigerat hello behörigheter för hello virtuella datorer kan du konfigurera hello STONITH enheter i hello kluster.

<pre>
sudo vi crm-fencing.txt
# enter hello following toocrm-fencing.txt
# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>Vi kan nu läsa in hello toohello kluster
sudo crm konfigurera belastningen update crm-fencing.txt
</pre>

### <a name="create-sap-hana-resources"></a>Skapa SAP HANA-resurser

<pre>
sudo vi crm-saphanatop.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number and HANA system id
<code>
primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
    operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
    op monitor interval="10" timeout="600" \
    op start interval="0" timeout="600" \
    op stop interval="0" timeout="300" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"

clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>Vi kan nu läsa in hello toohello kluster
sudo crm konfigurera belastningen update crm-saphanatop.txt
</pre>

<pre>
sudo vi crm-saphana.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number, HANA system id and hello frontend IP address of hello Azure load balancer. 
<code>
primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
    operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
    op start interval="0" timeout="3600" \
    op stop interval="0" timeout="3600" \
    op promote interval="0" timeout="3600" \
    op monitor interval="60" role="Master" timeout="700" \
    op monitor interval="61" role="Slave" timeout="700" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
    DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"

ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
    target-role="Started" interleave="true"

primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
    meta target-role="Started" is-managed="true" \ 
    operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
    op monitor interval="10s" timeout="20s" \ 
    params ip="<b>10.0.0.21</b>" 
primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
    params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
    op monitor timeout=20s interval=10 depth=0 
group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
 
colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  
order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>Vi kan nu läsa in hello toohello kluster
sudo crm konfigurera belastningen update crm-saphana.txt
</pre>

### <a name="test-cluster-setup"></a>Inställningar för kluster
hello följande kapitel beskrivs hur du kan testa din konfiguration. Varje test förutsätter att du är rot och hello SAP HANA master körs på hello virtuella saphanavm1.

#### <a name="fencing-test"></a>Avgränsningar Test

Du kan testa hello installationen av hello avgränsningar agenten genom att inaktivera hello nätverksgränssnittet på noden saphanavm1.

<pre><code>
sudo ifdown eth0
</code></pre>

hello virtuella datorn bör nu hämta startas om eller stoppas beroende på klusterkonfigurationen.
Om du ställer in hello stonith åtgärd toooff hello virtuella datorn kommer att stoppas och hello resurser är migrerade toohello som kör virtuella datorn.

När du startar hello virtuella datorn igen hello SAP HANA resursen misslyckas toostart som sekundär om du ställer in AUTOMATED_REGISTER = ”false”. I det här fallet behöver du tooconfigure hello HANA instans som sekundär genom att köra följande kommando hello:

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a>Testa en manuell växling

Du kan testa en manuell växling av hello pacemaker tjänsten stoppas på noden saphanavm1.
<pre><code>
service pacemaker stop
</code></pre>

Efter hello redundans kan du starta hello tjänsten igen. hello SAP HANA-resursen på saphanavm1 misslyckas toostart som sekundär om du ställer in AUTOMATED_REGISTER = ”false”. I det här fallet behöver du tooconfigure hello HANA instans som sekundär genom att köra följande kommando hello:

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a>Testa en migrering

Du kan migrera hello SAP HANA-huvudnoden genom att köra följande kommando hello
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

Detta bör migrera hello SAP HANA-huvudnod och hello-grupp som innehåller hello virtuella IP-adress toosaphanavm2.
hello SAP HANA-resursen på saphanavm1 misslyckas toostart som sekundär om du ställer in AUTOMATED_REGISTER = ”false”. I det här fallet behöver du tooconfigure hello HANA instans som sekundär genom att köra följande kommando hello:

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

hello migrering skapar plats begränsningar som behöver toobe tas bort.

<pre><code>
crm configure edited

# delete location contraints that are named like hello following contraint. You should have two contraints, one for hello SAP HANA resource and one for hello IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

Du måste också toocleanup hello tillstånd hello sekundära noden resurs

<pre><code>
# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a>Nästa steg
* [Azure virtuella datorer planering och implementering för SAP][planning-guide]
* [Distribution av Azure virtuella datorer för SAP][deployment-guide]
* [Azure virtuella datorer DBMS-distribution för SAP][dbms-guide]
* hur tooestablish hög tillgänglighet och planera för katastrofåterställning för SAP HANA i Azure (stora instanser), se toolearn [SAP HANA (stora instanser) hög tillgänglighet och katastrofåterställning recovery på Azure](hana-overview-high-availability-disaster-recovery.md). 
