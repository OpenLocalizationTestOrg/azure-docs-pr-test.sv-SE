---
title: "aaaHow tooinstall en Linux-huvudmålserver för redundans från Azure tooon lokala | Microsoft Docs"
description: "Innan du skydda en Linux-dator, måste en Linux-huvudmålserver. Lär dig hur tooinstall en."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: d7c55d115712b9862414979f89efb1f177c5f0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-linux-master-target-server"></a>Installera en Linux-huvudmålsserver
När du växlar över dina virtuella datorer kan du växla tillbaka hello virtuella datorer toohello lokal plats. tillbaka toofail måste tooreprotect hello virtuell dator från Azure toohello lokal plats. För den här processen behöver du en lokal huvudmålservern server tooreceive hello-trafik. 

Om den skydda virtuella datorn är en Windows-dator, måste en Windows-huvudmålserver. För en virtuell Linux-dator behöver du en Linux-huvudmålserverns. Läs hello följande steg toolearn hur toocreate och installera en Linux master-mål.

> [!IMPORTANT]
> Från och med lanseringen av hello 9.10.0 huvudmålservern kan hello senaste huvudmålservern endast installeras på en 16.04 Ubuntu server. Nya installationer är inte tillåtna på CentOS6.6 servrar. Du kan dock fortsätta tooupgrade ditt gamla huvudnyckeln målservrar med hello 9.10.0 version.

## <a name="overview"></a>Översikt
Den här artikeln innehåller anvisningar för hur tooinstall en Linux master-mål.

Skicka kommentarer eller frågor hello slutet av den här artikeln eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Krav

* toochoose hello värden på vilka toodeploy hello Huvudmålet, avgör om hello återställning kommer toobe tooan befintlig lokal virtuell dator eller tooa ny virtuell dator. 
    * Hello värden för hello huvudmålservern ska ha åtkomst toohello datalager för hello virtuell dator för en befintlig virtuell dator.
    * Om hello lokala virtuella datorn inte finns, skapas hello återställning av virtuell dator på hello samma värden som hello huvudmålservern. Du kan välja valfri ESXi-värd tooinstall hello huvudmålservern.
* Hej huvudmålservern ska vara i ett nätverk som kan kommunicera med hello processervern och konfigurationsservern hello.
* hello måste hello huvudmålservern vara lika tooor tidigare än hello versioner av hello processervern och konfigurationsservern hello. Till exempel om hello version av hello konfigurationsservern 9.4 hello versionen av hello huvudmålservern kan vara 9.4 eller 9.3 men inte 9.5.
* Hej huvudmålservern kan bara finnas en virtuell VMware-dator och inte en fysisk server.

## <a name="create-hello-master-target-according-toohello-sizing-guidelines"></a>Skapa hello huvudmålservern enligt toohello storlek riktlinjer

Skapa hello huvudmålservern i enlighet med hello följa riktlinjerna för storlek:
- **RAM-minne**: 6 GB eller mer
- **OS-diskstorlek**: 100 GB eller mer (tooinstall CentOS6.6)
- **Ytterligare diskstorleken för kvarhållningsenhetens**: 1 TB
- **CPU-kärnor**: 4 kärnor eller mer

hello följande stöd för Ubuntu kärnor stöds.


|Kernel-serien  |Stöd för upp |
|---------|---------|
|4.4      |4.4.0-81-Generic         |
|4.8      |4.8.0-56-Generic         |
|4.10     |4.10.0-24-Generic        |


## <a name="deploy-hello-master-target-server"></a>Distribuera hello huvudmålservern

### <a name="install-ubuntu-16042-minimal"></a>Installera Ubuntu 16.04.2 Minimal

Ta hello följande hello steg tooinstall hello Ubuntu 16.04.2 64-bitars operativsystem.

**Steg 1:** gå toohello [Hämta länk](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) och välj hello närmaste spegling från som hämtar en Ubuntu 16.04.2 minimal 64-bitars ISO.

Behåll en Ubuntu 16.04.2 minimal 64-bitars ISO i hello DVD-enheten och starta hello system.

**Steg 2:** Välj **engelska** som önskat språk och välj sedan **RETUR**.

![Välj ett språk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

**Steg 3:** Välj **installerar Ubuntu Server**, och välj sedan **RETUR**.

![Välj installerar Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

**Steg 4:** Välj **engelska** som önskat språk och välj sedan **RETUR**.

![Välj engelska som ditt språk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

**Steg 5:** Välj hello lämpligt alternativ från hello **tidszon** alternativ, och välj **RETUR**.

![Välj hello tidszon](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

**Steg 6:** Välj **nr** (hello standardalternativet), och välj sedan **RETUR**.


![Konfigurera hello tangentbord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

**Steg 7:** Välj **engelska (USA)** som hello land för hello tangentbord och välj sedan **RETUR**.

![Välj USA som hello land för ursprung](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

**Steg 8:** Välj **engelska (USA)** som hello tangentbordslayout och välj sedan **RETUR**.

![Välj amerikansk engelska som hello tangentbordslayout](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

**Steg 9:** ange hello värdnamn för servern i hello **värdnamn** och välj sedan **Fortsätt**.

![Ange hello värdnamn för servern](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

**Steg 10:** toocreate ett användarkonto, ange hello användarnamn och välj sedan **Fortsätt**.

![Skapa ett användarkonto](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

**Steg 11:** ange hello lösenord för hello nytt användarkonto och välj sedan **Fortsätt**.

![Ange hello lösenord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

**Steg 12:** bekräfta hello lösenord för hello nya användaren och välj sedan **Fortsätt**.

![Bekräfta hello lösenord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

**Steg 13:** Välj **nr** (hello standardalternativet), och välj sedan **RETUR**.

![Konfigurera användare och lösenord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

**Steg 14:** om hello tidszon som visas är korrekt, Välj **Ja** (hello standardalternativet), och välj sedan **RETUR**.

tooreconfigure din tidszon, Välj **nr**.

![Konfigurera hello klockan](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

**Steg 15:** hello partitionering metoden alternativ, Välj **interaktiv - använder hela disken**, och välj sedan **RETUR**.

![Välj hello partitionering metodalternativet](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

**Steg 16:** Välj hello lämplig disk från hello **Välj disk toopartition** alternativ och välj sedan **RETUR**.


![Välj hello disk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

**Steg 17:** Välj **Ja** toowrite hello ändringar toodisk och välj sedan **RETUR**.

![Skriva hello ändringar toodisk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

**Steg 18:** väljer hello standardalternativet, väljer **Fortsätt**, och välj sedan **RETUR**.

![Välj hello standardalternativet](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

**Steg 19:** Välj hello lämpligt alternativ för att hantera uppgraderingar på datorn och välj sedan **RETUR**.

![Välj hur toomanage uppgraderas](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> Eftersom Azure Site Recovery hello huvudmålservern kräver en särskild version av hello Ubuntu, behöver du tooensure som hello kernel uppgraderingar har inaktiverats för hello virtuell dator. Om de är aktiverade kan orsaka reguljära uppgraderingar hello huvudmålservern server toomalfunction. Kontrollera att du väljer hello **inga automatiska uppdateringar** alternativet.


**Steg 20:** Välj standardalternativen. Om du vill openSSH för SSH ansluta, Välj hello **OpenSSH server** alternativet och välj sedan **Fortsätt**.

![Välj program](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

**Steg 21:** Välj **Ja**, och välj sedan **RETUR**.

![Startprogram för installationen hello GRUB](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

**Steg 22:** Välj hello lämplig enhet för hello Start inläsaren installation (helst **/dev/sda**), och välj sedan **RETUR**.

![Välj en enhet för start inläsaren installation](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

**Steg 23:** Välj **Fortsätt**, och välj sedan **RETUR** toofinish hello installation.

![Slut hello installationen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

När hello installationen är klar, logga in toohello VM med hello nya autentiseringsuppgifterna för användaren. (Se för**steg 10** för mer information.)

Ta hello stegen som beskrivs i följande skärmbild tooset hello rot hello användarens lösenord. Logga in som rotanvändare.

![Ange hello rot användarlösenord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-hello-machine-for-configuration-as-a-master-target-server"></a>Förbered hello datorn för konfigurationen som en huvudmålserver
Förbered hello datorn för konfigurationen som en huvudmålserver.

tooget hello-ID för varje SCSI-hårddisk i en virtuell Linux-dator, aktivera hello **disk. EnableUUID = TRUE** parameter.

tooenable den här parametern tar hello följande steg:

1. Stäng av den virtuella datorn.

2. Högerklicka hello posten för hello virtuell dator i hello till vänster och välj sedan **redigera inställningar för**.

3. Välj hello **alternativ** fliken.

4. I hello vänster och välj **Avancerat** > **allmänna**, och välj sedan hello **konfigurationsparametrar** knappen hello nedre högra tillhör hello-skärmen.

    ![Alternativfliken](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    Hej **konfigurationsparametrar** alternativet är inte tillgängligt när hello datorn körs. toomake på den här fliken är aktiv, Stäng hello virtuella datorn.

5. Se om en rad med **disk. EnableUUID** finns redan.

    - Om värdet för hello finns och har angetts för**FALSKT**, ändrar hello-värdet för**SANT**. (hello värden är inte skiftlägeskänsligt.)

    - Om värdet för hello finns och har angetts för**SANT**väljer **Avbryt**.

    - Om hello värdet inte finns, väljer **Lägg till rad**.

    - Hello namnkolumnen lägga till **disk. EnableUUID**, och sedan ange hello värdet för**SANT**.

    ![Kontrollerar om disken. EnableUUID finns redan](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a>Inaktivera kernel-uppgraderingar

Azure Site Recovery-huvudmålservern kräver en särskild version av hello Ubuntu, se till att hello kernel uppgraderingar är inaktiverat för hello virtuell dator.

Om kernel uppgraderingar är aktiverade kan orsaka reguljära uppgraderingar hello huvudmålservern server toomalfunction.

#### <a name="download-and-install-additional-packages"></a>Hämta och installera ytterligare paket

> [!NOTE]
> Kontrollera att du har toodownload för Internet-anslutning och installera ytterligare paket. Om du inte har anslutning till Internet, måste toomanually hitta dessa RPM-paket och installera dem.

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-hello-installer-for-setup"></a>Hämta hello installer för installationen

Om din huvudmålservern har Internetanslutning, kan du använda följande steg toodownload hello installer hello. Annars kan du kopiera hello installationsprogrammet från hello processervern och installera den.

#### <a name="download-hello-master-target-installation-packages"></a>Hämta hello huvudmålservern-paket

[Hämta hello senaste Linux huvudmålservern installation bits](https://aka.ms/latestlinuxmobsvc).

toodownload den med hjälp av Linux, typ:

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

Kontrollera att du hämtar och packa hello installer i arbetskatalogen. Om du packa för**/usr/lokal**, misslyckas hello-installationen.


#### <a name="access-hello-installer-from-hello-process-server"></a>Åtkomst hello installer från hello processervern

1. Hej processervern gå för**C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

2. Kopiera hello krävs installer-fil från hello processervern och sparar den som **latestlinuxmobsvc.tar.gz** i arbetskatalogen.


### <a name="apply-custom-configuration-changes"></a>Använd anpassad konfigurationsändringar

tooapply anpassade konfigurationsändringar, Använd hello följande steg:


1. Kör hello efter kommandot toountar hello binary.
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Skärmbild av hello kommandot toorun](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. Kör följande kommando toogive behörighet hello.
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. Kör följande kommandoskript toorun hello hello.
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> Kör hello skriptet en gång på hello-servern. Stäng hello-servern. Starta sedan om hello server när du lägger till en disk, enligt beskrivningen i nästa avsnitt om hello.

### <a name="add-a-retention-disk-toohello-linux-master-target-virtual-machine"></a>Lägg till en kvarhållning disk toohello Linux huvudmålservern virtuell dator

Använd följande steg toocreate en kvarhållningsdisken hello:

1. Koppla en ny virtuella för 1 TB-disk toohello Linux huvudmålservern och starta sedan hello-datorn.

2. Använd hello **multipath -lla** kommando toolearn hello multipath-ID för hello kvarhållningsdisken.

    ```
    multipath -ll
    ```
    ![hello multipath ID hello kvarhållningsdisken](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. Formatera hello enhet och sedan skapa ett filsystem i hello ny enhet.

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Skapa ett filsystem på hello-enhet](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. När du har skapat hello filsystemet montera hello kvarhållningsdisken.
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![Montering hello kvarhållningsdisken](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. Skapa hello **fstab** post toomount hello kvarhållningsenhetens varje gång hello systemet startar.
    ```
    vi /etc/fstab
    ```
    Välj **infoga** toobegin hello Filredigering. Skapa en ny rad och infoga hello efter texten. Redigera hello disk multipath ID baserat på hello markerat multipath ID från hello föregående kommando.

     **/dev/mapper/ <Retention disks multipath id> /mnt/kvarhållning ext4 rw 0 0**

    Välj **Esc**, och skriv sedan **: wq** (skriva och avsluta) tooclose hello editor-fönstret.

### <a name="install-hello-master-target"></a>Installera hello huvudmålservern

> [!IMPORTANT]
> hello måste hello huvudmålservern vara lika tooor tidigare än hello versioner av hello processervern och konfigurationsservern hello. Om detta inte är uppfyllt, skyddar lyckas, men kan slutföras inte.


> [!NOTE]
> Kontrollera att hello innan du installerar hello huvudmålservern **/etc/hosts** filen på hello virtuell dator innehåller poster som mappar hello lokala värdnamnet toohello IP-adresser som är associerade med alla nätverkskort.

1. Kopiera hello lösenfras från **C:\ProgramData\Microsoft Azure plats Recovery\private\connection.passphrase** på hello konfigurationsservern. Spara den som **passphrase.txt** i hello hello samma lokala katalog genom att köra följande kommando:

    ```
    echo <passphrase> >passphrase.txt
    ```
    Exempel: 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. Observera hello configuration serverns IP-adress. Du behöver det i hello nästa steg.

3. Kör följande kommando tooinstall hello huvudmålservern hello och registrera hello-servern med hello konfigurationsservern.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Exempel: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    Vänta tills hello skriptet har körts. Om hello huvudmålservern registrerar konfigurationsändringarna, hello huvudmålservern visas på hello **Site Recovery-infrastruktur** sidan hello-portalen.


#### <a name="install-hello-master-target-by-using-interactive-installation"></a>Installera hello huvudmålservern genom att använda interaktiv installation

1. Kör följande kommando tooinstall hello huvudmålservern hello. Hello agent roll, Välj **Huvudmålservern**.

    ```
    ./install
    ```

2. Välj hello standardplatsen för installation och välj sedan **RETUR** toocontinue.

    ![Om du väljer en standardplatsen för installation av huvudmålservern](./media/site-recovery-how-to-install-linux-master-target/image17.png)

Registrera hello konfigurationsservern med hjälp av kommandoraden hello efter hello-installationen har slutförts.

1. Observera hello hello configuration serverns IP-adress. Du behöver det i hello nästa steg.

2. Kör följande kommando tooinstall hello huvudmålservern hello och registrera hello-servern med hello konfigurationsservern.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Exempel: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   Vänta tills hello skriptet har körts. Om hello huvudmålservern är registrerade har, hello huvudmålservern finns på hello **Site Recovery-infrastruktur** sidan hello-portalen.


### <a name="upgrade-hello-master-target"></a>Uppgradera hello huvudmålservern

Kör installationsprogrammet för hello. Den identifierar automatiskt den hello-agenten är installerad på hello Huvudmålet. tooupgrade, Välj **Y**.  När hello-installationen har slutförts, kontrollera hello version av hello huvudmålservern installeras med hjälp av följande kommando hello.

    ```
    cat /usr/local/.vx_version
    ```

Du kan se att hello **Version** fältet ger hello versionsnumret för hello huvudmålservern.

### <a name="install-vmware-tools-on-hello-master-target-server"></a>Installera VMware-verktyg på hello huvudmålservern

Du måste tooinstall VMware-verktyg på hello Huvudmålet så att den kan identifiera hello datalager. Om hello tools inte är installerat, visas skyddar hello-skärmen inte i hello datalager. Efter installationen av hello VMware-verktyg behöver du toorestart.

## <a name="next-steps"></a>Nästa steg
När hello installation och registrering av hello huvudmålservern har finsihed, kan du se hello huvudmålservern visas på hello **Huvudmålservern** i avsnittet **Site Recovery-infrastruktur**, under hello Översikt över konfigurationen.

Du kan nu fortsätta med [återaktivera skydd](site-recovery-how-to-reprotect.md), följt av återställning efter fel.

## <a name="common-issues"></a>Vanliga problem

* Kontrollera att du inte aktiverar Storage vMotion på alla management-komponenter, till exempel en huvudmålserver. Om hello huvudmålservern flyttas efter en lyckad skyddar, kan hello virtuella diskar (VMDKs) inte frånkopplas. I det här fallet misslyckas återställning efter fel.

* hello huvudmålservern ska inte ha några ögonblicksbilder på hello virtuella datorn. Om det finns ögonblicksbilder, misslyckas återställning efter fel.

* På grund av toosome anpassade NIC konfigurationer med vissa kunder hello nätverksgränssnittet inaktiveras under start och hello huvudmålservern agent kan inte initieras. Kontrollera att hello följande egenskaper är korrekt inställda. Kontrollera de här egenskaperna i hello Ethernet-kort filens /etc/sysconfig/network-scripts/ifcfg-eth *.
    * BOOTPROTO = dhcp
    * ONBOOT = Ja
