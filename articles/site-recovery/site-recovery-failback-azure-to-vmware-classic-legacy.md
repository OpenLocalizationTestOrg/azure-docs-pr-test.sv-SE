---
title: "aaaFail tillbaka den virtuella VMware-datorer från Azure klassiska äldre hello-portalen | Microsoft Docs"
description: "Den här artikeln beskriver hur toofail tillbaka en virtuell VMware-dator som har replikerats tooAzure med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: a63524bf-990c-42ee-8554-e017e6e67e68
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 5ef66b366dcdc43f3bc171e0ed1532216cc2ab89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-toovmware-with-azure-site-recovery-legacy"></a>Växla tillbaka virtuella VMware-datorer och fysiska servrar från Azure tooVMware med Azure Site Recovery (bakåtkompatibelt)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-failback-azure-to-vmware.md)
> * [Klassisk Azure-portal](site-recovery-failback-azure-to-vmware-classic.md)
> * [Klassiska Azure-portalen (bakåtkompatibelt)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
>
>

Den här artikeln beskriver hur toofail tillbaka VMware-datorer och Windows-/ Linux fysiska servrar från Azure tooyour lokal plats när du har replikerats från lokalerna platsen tooAzure med [Azure Site Recovery?](site-recovery-overview.md).

Den här artikeln beskriver en gammal konfiguration och bör inte användas för nya valv.

## <a name="architecture"></a>Arkitektur
Det här diagrammet visar hello redundans och återställning av scenariot. hello blå raderna är hello-anslutningar som används under växling vid fel. hello red raderna är hello-anslutningar som används vid återställning. hello rader med pilar gå igenom hello internet.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Innan du börjar
* Du bör ha misslyckats över din virtuella VMware-datorer eller fysiska servrar och de ska köras i Azure.
* Observera att du kan bara växla tillbaka virtuella VMware-datorer och Windows-/ Linux fysiska servrar från Azure tooVMware virtuella datorer i hello lokal primär plats.  Om du växlar tillbaka en fysisk dator, redundans tooAzure konvertera det tooan virtuella Azure-datorn och återställning efter fel tooVMware konvertera den tooa VMware VM.

Här är hur du ställer in återställning:

1. **Ställ in återställning komponenter**: du behöver tooset in en vContinuum-server lokalt, och peka toohello konfigurationsservern VM i Azure. Du måste också ställa in en processerver som en virtuell dator i Azure toosend data tillbaka toohello lokala huvudmålservern. Du kan registrera hello processervern med hello konfigurationsservern som hanteras hello växling vid fel. Du installerar en lokal huvudmålserver. Om du behöver en Windows-huvudmålserver ställs det in automatiskt när du installerar vContinuum. Om du behöver Linux behöver du tooset den manuellt på en separat server.
2. **Aktivera skydd och återställning efter fel**: när du har konfigurerat hello komponenter i vContinuum måste tooenable skydd för redundansväxlade virtuella Azure-datorer. Du kör en beredskapskontroll på hello virtuella datorer och köra en växling vid fel från Azure tooyour lokal plats. När återställningen är klar skyddar lokala datorer så att de startar replikerar tooAzure.

## <a name="step-1-install-vcontinuum-on-premises"></a>Steg 1: Installera vContinuum lokala
Du behöver tooinstall vContinuum-server lokalt och peka den toohello konfigurationsservern.

1. [Hämta vContinuum](http://go.microsoft.com/fwlink/?linkid=526305).
2. Hämta hello [vContinuum uppdatering](http://go.microsoft.com/fwlink/?LinkID=533813) version.
3. Installera hello senaste versionen av vContinuum. På hello **Välkommen** klickar **nästa**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4. Ange hello CX IP-adress och port för hello CX server på hello första sidan i guiden hello. Välj **använder HTTPS**.

   ![](./media/site-recovery-failback-azure-to-vmware/image3.png)
5. Hitta hello konfigurationsservern IP-adress på hello **instrumentpanelen** fliken hello konfigurationsservern VM i Azure.
   ![](./media/site-recovery-failback-azure-to-vmware/image4.png)
6. Hitta hello konfigurationsservern HTTPS offentlig port på hello **slutpunkter** fliken hello konfigurationsservern VM i Azure.

   ![](./media/site-recovery-failback-azure-to-vmware/image5.png)
7. På hello **CS lösenfras information** sidan Ange hello lösenfras som du antecknade ned när du registrerade hello konfigurationsservern. Om du inte kommer ihåg den checka in **C:\\programfiler (x86)\\InMage system\\privata\\connection.passphrase** på hello konfigurationsservern VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)
8. I hello **Välj målplats** ange där du vill tooinstall hello vContinuum-servern och på **installera**.

   ![](./media/site-recovery-failback-azure-to-vmware/image7.png)
9. När installationen är klar kan du starta vContinuum.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

## <a name="step-2-install-a-process-server-in-azure"></a>Steg 2: Installera en processerver i Azure
Du behöver tooinstall en processerver i Azure så att hello virtuella datorer i Azure kan skicka hello data tillbaka tooan lokala huvudmålservern.

1. På hello **Konfigurationsservrar** sidan i Azure, Välj tooadd en ny processerver.

   ![](./media/site-recovery-failback-azure-to-vmware/image9.png)
2. Ange namn på en server och ett namn och lösenord tooconnect toohello virtuella datorn som administratör. Välj hello configuration server toowhich du vill tooregister hello processervern. Hello bör vara samma server som du använder tooprotect och växla över dina virtuella datorer.
3. Ange hello Azure nätverk i vilka hello processervern ska distribueras. Det bör vara hello samma nätverk som hello konfigurationsservern. Ange ett unikt IP-adressen som valts undernät och påbörjar distributionen.

   ![](./media/site-recovery-failback-azure-to-vmware/image10.png)
4. Ett jobb är utlösta toodeploy hello processervern.

   ![](./media/site-recovery-failback-azure-to-vmware/image11.png)
5. När hello processervern har distribuerats i Azure kan du logga in på hello-servern med hjälp av hello autentiseringsuppgifter du angav. Registrera processervern hello i hello samma sätt som du har registrerat hello lokalt processervern.

   ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

> [!NOTE]
> hello servrar har registrerats under återställning efter fel visas under VM-egenskaper i Site Recovery inte. De är bara synliga under hello **servrar** fliken hello configuration server toowhich de är registrerade. Det kan ta ungefär 10 – 15 minuter tills de hello processen servern visas på fliken hello.
>
>

## <a name="step-3-install-a-master-target-server-on-premises"></a>Steg 3: Installera en huvudmålserver lokalt
Beroende på operativsystemet källa virtuella datorer måste tooinstall en Linux eller en Windows-huvudmålserver lokalt.

### <a name="deploy-a-windows-master-target-server"></a>Distribuera en Windows-huvudmålserver
Ett Windows-huvudmål finns redan med vContinuum-installationen. När du installerar hello vContinuum huvudserver också distribueras på hello samma dator och registrerade toohello konfigurationsservern.

1. toobegin distribution, skapa en tom datorn lokalt på hello ESX-värd där du vill toorecover hello virtuella datorer från Azure.
2. Kontrollera att det finns minst två diskar kopplade toohello VM – en används för hello operativsystem och hello andra används för hello kvarhållningsenhetens.
3. Installera hello-operativsystem.
4. Installera hello vContinuum på hello-servern. Det här slutför du installationen av hello huvudmålservern.

### <a name="deploy-a-linux-master-target-server"></a>Distribuera en Linux-huvudmålserver
1. toobegin distribution, skapa en tom datorn lokalt på hello ESX-värd där du vill toorecover hello virtuella datorer från Azure.
2. Kontrollera att det finns minst två diskar kopplade toohello VM – en används för hello operativsystem och hello andra används för hello kvarhållningsenhetens.
3. Installera hello Linux-operativsystem. hello Linux huvudmålservern-system bör inte använda LVM för rot- eller kvarhållning lagringsutrymmen. Som standard konfigurerad en Linux huvudmålservern tooavoid LVM partitioner/diskar identifiering.
4. Partitioner som du kan skapa:

   ![](./media/site-recovery-failback-azure-to-vmware/image13.png)
5. Utföra hello nedanstående steg för efter installationen innan du påbörjar installationen av hello huvudmålservern.

#### <a name="post-os-installation-steps"></a>Bokför OS installationssteg
tooget hello SCSI-ID: n för varje SCSI-hårddisk på en virtuell dator Linux aktiverar hello parameter ”disk. EnableUUID = TRUE ”enligt följande:

1. Stäng av den virtuella datorn.
2. Högerklicka på posten för hello VM hello vänstra panelen > **redigera inställningar för**.
3. Klicka på hello **alternativ** fliken. Välj **Avancerat\>allmän artikel** > **konfigurationsparametrar**. Hej **konfigurationsparametrar** alternativet är endast tillgängligt när hello datorn är avstängd.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)
4. Kontrollerar om det finns en rad med **disk. EnableUUID** finns. Om det innehåller och har angetts för**FALSKT** ställa in den också**SANT** (inte skiftlägeskänsliga). Om finns och ange tootrue, klickar du på **Avbryt** och testa hello SCSI-kommando i hello gästoperativsystemet när den har startats in. Om inte finns klickar du på **Lägg till rad**.
5. Lägg till disk. EnableUUID i hello **namn** kolumn. Ange värdet true. Lägg inte till hello ovan värden tillsammans med dubbla citattecken.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-hello-additional-packages"></a>Hämta och installera ytterligare hello-paket
Obs: Kontrollera hello system har Internetanslutning innan du hämtar och installerar hello ytterligare paket.

\#YUM installera -y xfsprogs perl lsscsi rsync wget kexec-verktyg

Detta kommando hämtar paketen 15 från CentOS 6.6 databasen och installerar dem:

BC-1.06.95-1.el6.x86\_64. rpm

busybox-1.15.1-20.el6.x86\_64. rpm

elfutils-libs-0.158-3.2.el6.x86\_64. rpm

kexec-verktyg-2.0.0-280.el6.x86\_64. rpm

lsscsi-0.23-2.el6.x86\_64. rpm

lzo-2.03-3.1.el6\_5.1.x86\_64. rpm

Perl-5.10.1-136.el6\_6.1.x86\_64. rpm

Perl-modul-plug-3.90-136.el6\_6.1.x86\_64. rpm

Perl-baljor-visar-1,04-136.el6\_6.1.x86\_64. rpm

Perl-baljor-enkel-3.13-136.el6\_6.1.x86\_64. rpm

Perl-libs-5.10.1-136.el6\_6.1.x86\_64. rpm

Perl-version-0,77-136.el6\_6.1.x86\_64. rpm

rsync-3.0.6-12.el6.x86\_64. rpm

snygga 1.1.0 1.el6.x86\_64. rpm

wget-1.12-5.el6\_6.1.x86\_64. rpm

Om källdatorn hello använder Reiser eller XFS filsystem för hello rot- eller enhet, ska sedan följande paket hämtas och installeras på Linux huvudmålservern tidigare tooprotection.

\#CD /usr/local

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#rpm - ivh kmod-reiserfs-0,0-1.el6.elrepo.x86\_64. rpm reiserfs-verktyg för webbplatsuppgradering-3.6.21-1.el6.elrepo.x86\_64. rpm

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#rpm - ivh xfsprogs-3.1.1-16.el6.x86\_64. rpm

#### <a name="apply-custom-configuration-changes"></a>Använd anpassad konfigurationsändringar
Kontrollera att du har slutfört hello föregående avsnitt och sedan följa dessa steg innan du tillämpar ändringarna:

1. Kopiera hello RHEL 6-64 Unified Agent binära toohello nyskapad OS.
2. Kör det här kommandot toountar hello binära: **tar - zxvf \<filnamn\>**
3. Kör det här kommandot toogive behörigheter: \# **chmod 755./ApplyCustomChanges.sh**
4. Kör skript hello:  **\# ./ApplyCustomChanges.sh**. Kör hello skriptet en gång på hello-servern. Starta om servern hello när hello skriptet körs.

### <a name="install-hello-linux-server"></a>Installera hello Linux-server
1. [Hämta](http://go.microsoft.com/fwlink/?LinkID=529757) hello-installationsfilen.
2. Kopiera hello filen toohello Linux virtuella huvudmåldatorn med hjälp av ett sftp klienten verktyg du väljer. Alternativt kan du logga in på huvudmålservern virtuell hello Linux-dator och använda wget toodownload hello-installationspaketet från den angivna länken
3. Logga in på hello Linux huvudmålservern servern virtuell dator med en ssh-klient av önskat
4. Om du är ansluten toohello Azure nätverk som du distribuerat Linux huvudmålservern-servern via en VPN-anslutning och sedan använda den interna IP-adressen för hello-server som du hittar i den virtuella datorn **instrumentpanelen** fliken och port 22 tooconnect toohello Linux master mål servern med hjälp av Secure Shell.
5. Om du vill ansluta toohello Linux huvudmålservern via en offentlig Internetanslutning använder hello Linux huvudmålservern serverns offentliga virtuella IP-adress (från virtuella datorer som hello **instrumentpanelen** fliken) och hello offentlig slutpunkt Skapa för ssh toologin toohello Linux servder.
6. Extrahera hello filer från hello gzippade Linux huvudmålservern Server installer tar Arkiv genom att köra: *”tar – xvzf Microsoft ASR\_UA\_8.2.0.0\_RHEL6 64\”* från hello directory som innehåller hello installer-fil.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)
7. Om du extraherade hello installer filer tooa annan katalog ändrar extraherades toohello directory toowhich hello innehållet i hello tar arkivet. Från den här sökvägen kör ”sudo. / install.sh”.

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)
8. När du väljer Ange toochoose en primär roll **2 (Huvudmålservern)**. Lämna hello andra alternativ för interaktiv installation till sina standardvärden.
9. Vänta tills installationen toocontinue och hello värden Config gränssnittet tooappear. hello värd-konfigurationsverktyget för hello Linux master sarget Server är ett kommandoradsverktyg. Ändrar inte storlek hello ssh verktygsfönstret för klienten. Använd hello pilen nycklar tooselect hello **Global** alternativet och tryck på RETUR på tangentbordet.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)
10. I fältet hello **IP** ange hello interna IP-adress för konfigurationsservern hello (du hittar i hello **instrumentpanelen** fliken hello konfigurationsservern VM) och tryck på RETUR. I **Port** ange **22** och tryck på RETUR.
11. Lämna **Använd HTTPS** som **Ja**, och tryck på RETUR.
12. Ange hello lösenfras som skapades på hello konfigurationsservern. Om du använder en PUTTY klient från en Windows datorn toossh toohello Linux-dator kan du använda SKIFT + Insert toopaste hello innehållet i Urklipp hello. Kopiera hello lösenfras toohello lokala Urklipp med Ctrl-C och använda SKIFT + Insert toopaste den. Tryck på RETUR.
13. Hello HÖGERPIL viktiga toonavigate tooquit och tryck på RETUR. Vänta tills installationen toocomplete.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Om kunde inte tooregister av någon anledning din Linux server toohello configuration huvudmålsserver kan du göra så igen genom att köra värden konfigurationsverktyget /usr/local/ASR/Vx/bin/hostconfigcli (behöver du tooset åtkomst behörigheter toothis katalogen genom att köra chmod som en superanvändare).

#### <a name="validate-master-target-registration"></a>Validera huvudmålservern registrering
Du kan verifiera att hello huvudmålservern registrerades toohello konfigurationsservern i Azure Site Recovery-valv > **konfigurationsservern** > **serverinformation**.

> [!NOTE]
> När registrera hello huvudmålservern, om du får konfigurationsfel som hello virtuell dator kan ha tagits bort från Azure eller som slutpunkter inte är korrekt konfigurerade kan beror detta på att även om hello huvudmålservern konfiguration har identifierats av hello Azure dndpoints när hello huvudmålservern har distribuerats i Azure, det är inte sant för en lokal huvudmålservern server lokalt. Detta påverkar inte återställning efter fel och du kan ignorera dessa fel.
>
>

## <a name="step-4-protect-hello-virtual-machines-toohello-on-premises-site"></a>Steg 4: Skydda hello virtuella datorer toohello lokal plats
Du måste tooprotect VMs toohello lokal plats innan du växlar tillbaka.

### <a name="before-you-begin"></a>Innan du börjar
När en virtuell dator har redundansväxlats tooAzure, läggs en extra temp enhet för filen till sidan. Detta är en extra enhet som normalt inte krävs av din redundansväxlade virtuella datorn eftersom den kanske redan har en enhet som är dedikerad för växlingsfilen. Innan du börjar omvänd skydd av hello virtuella datorer som du behöver se till att den här enheten tas offline så att den inte skyddas. Gör på följande sätt:

1. Öppna Datorhantering och välj lagringshantering så att den visar hello diskar online och ansluten toohello datorn.
2. Välj hello diskutrymme bifogade toohello datorn och välj toobring det offline.

### <a name="protect-hello-vms"></a>Skydda hello virtuella datorer
1. Kontrollera hello tillstånd för hello virtuella tooensure som är redundansväxling i hello Azure-portalen.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)
2. Starta vContinuum på din dator.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)
3. Klicka på **nytt skydd** och välj hello operativsystemtyp den
4. I hello nytt fönster som öppnas väljer hello **OS-typen** > **Hämta detaljer** hello virtuella datorer du vill ha toofail tillbaka. I **primära serverinformation**, identifiera och välj hello virtuella datorer som du vill tooprotect. Virtuella datorer visas under hello vCenter värdnamn som de befann sig i före redundans.
5. När du väljer en virtuell dator tooprotect (och det redan har redundansväxlats tooAzure) tillhandahåller två poster i ett popup-fönster för hello virtuell dator. Det beror på att hello konfigurationsservern identifierar två instanser av hello virtuella datorer som registrerats tooit. Du behöver tooremove hello post hello lokala VM så att du kan skydda hello rätt VM. tooidentify hello Azure VM posten här, kan du logga in hello Azure VM och gå tooC:\Program filer (x86) \Microsoft Azure Site Recovery\Application Data\etc. I hello filen drscout.conf identifiera hello värd-ID. Behåll hello-posten som hello värd-ID hittades på hello VM i hello vContinuum dialogrutan. Ta bort alla poster. tooselect hello korrigera VM som du kan se tooits IP-adress. hello IP-adressintervallet lokalt kommer hello lokala VM.

   ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image23.png)
6. Stoppa hello virtuella datorn på hello vCenter server. Du kan också ta bort hello VMs lokalt.
7. Ange hello lokala Huvudmålservern server toowhich du vill tooprotect hello virtuella datorer. toodo, ansluta toohello vCenter server toowhich du vill toofail tillbaka.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
8. Välj hello huvudmålservern baserat på hello värden toowhich du vill toorecover hello VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
9. Ange hello replikeringsalternativ för varje hello virtuella datorer. toodo detta behöver du tooselect hello recovery sida datastore toowhich hello virtuella datorer kommer att återställas. hello tabell sammanfattas hello olika alternativ som du behöver tooprovide för varje virtuell dator.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

   | **Alternativet** | **Alternativet Rekommenderat värde** |
   | --- | --- |
   |  Bearbeta serverns IP-adress |Välj hello process-server distribuerad i Azure |
   |  Kvarhållning storlek i MB | |
   |  Kvarhållningsvärde |1 |
   |  Dagar/timmar |Dagar |
   |  Intervall för konsekvenskontroll |1 |
   |  Välj Målet datalagret |Hej datalagret som är tillgängliga på hello återställningsplatsen. hello datalagret bör ha tillräckligt med utrymme och vara tillgängliga toohello ESX-värd som du vill toorestore hello virtuella datorn. |
10. Konfigurera hello egenskaper som hello virtuell dator får efter redundans tooon lokal plats. hello egenskaper sammanfattas i följande tabell hello.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    | **Egenskap** | **Detaljer** |
    | --- | --- |
    | Nätverkskonfiguration |För varje nätverkskort som upptäckts, markerar du den och klicka på **ändra** tooconfigure hello återställning efter fel IP-adress för hello virtuell dator. |
    | Maskinvarukonfiguration |Ange hello CPU och hello minne för hello VM. Inställningarna kan vara tillämpade tooall hello virtuella datorer som du försöker tooprotect. tooidentify hello korrekta värden för hello CPU och minne, kan du referera toohello IAAS-VM rollstorleken och finns hello antalet kärnor och tilldelat minne. |
    | Visningsnamn |Du kan byta namn hello virtuella datorer som de visas i hello vCenter lager efter återställning efter fel tooon (lokalt). hello standardnamnet är hello virtuella datorvärdnamn. tooidentify hello VM-namn, kan du referera toohello VM listan i hello skyddsgrupp. |
    | NAT-konfiguration |Beskrivs i detalj nedan |

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>Konfigurera NAT-inställningar
1. tooenable skydd av hello virtuella datorer, två kommunikationskanaler måste toobe upprättas. hello första kanalen är mellan hello virtuell dator och hello processervern. Den här kanalen samlar in hello data från hello VM och skickar den toohello processervern som sedan skickar hello data toohello huvudmålservern. Om hello processervern och hello virtuella toobe skyddade finns på hello samma virtuella Azure-nätverket och du behöver inte toouse NAT-inställningar. Ange annars NAT-inställningar. Visa hello offentliga IP-adressen för hello processerver i Azure.

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)
2. hello andra kanalen är mellan hello processervern och huvudmålservern för hello. Hej alternativet toouse NAT eller inte beror på om du använder en anslutning för VPN-baserade eller kommunicera via hello internet. Välj inte NAt om du använder VPN, men bara om du använder en internet-anslutning.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)
3. Om du inte har tagit bort hello lokala virtuella datorer som anges och om hello datastore du misslyckas tillbaka toostill innehåller hello gamla VMDK behöver du tooensure att hello återställningen hämtar skapas den virtuella datorn på en ny plats. toodo denna väljer hello **Avancerat** inställningar och ange en annan mapp toorestore tooin **namn mappinställningar**. Lämna hello andra alternativ med standardinställningarna. Tillämpa hello mappen namnet inställningar tooall hello servrar.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)
4. Kör en beredskapskontrollen skyddade tooensure att hello virtuella datorer är klar toobe tillbaka tooon lokala.

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)
5. Vänta tills den toocomplete. Om det har finns på alla virtuella datorer kan du ange ett namn för hello skyddsplan. Klicka på **skydda** toobegin.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)
6. Du kan övervaka förloppet på vContinuum.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)
7. Efter steget hello **aktiverar skydd planera** är slutförd kan du övervaka skydd av Virtuella datorer i hello Site Recovery-portalen.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)
8. Du kan se hello exakt status på hello VM och övervaka dess förlopp.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-hello-failback-plan"></a>Förbereda hello plan för återställning efter fel
Du kan förbereda en plan för återställning efter fel med vContinuum så att programmet hello kan vara felaktigt tillbaka toohello lokal plats när som helst. Dessa är mycket lik toohello återställningsplaner i Site Recovery.

1. Starta vContinuum och välj **hantera planer** > **återställa.** Du kan se i listan över alla hello planer som har använt toofail över virtuella datorer. Du kan använda hello samma planer toorecover.

   ![](./media/site-recovery-failback-azure-to-vmware/image37.png)
2. Välj hello skyddsplan och alla hello virtuella datorer du vill toorecover i den. Du kan se mer information, inklusive hello ESX målservern och hello VM källdisken när du väljer varje virtuell dator. Klicka på **nästa** toobegin hello återställa guiden och väljer hello virtuella datorer du vill toorecover.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)
3. Du kan återställa baserat på flera olika alternativ men vi rekommenderar att du använder **senaste taggen** och välj **Använd för alla virtuella datorer** tooensure som hello senaste data från hello virtuella datorn kommer att användas.
4. Kör hello **beredskapskontrollen.** Detta ska kontrollera om hello rätt parametrar är konfigurerade tooenable VM återställning. Klicka på **nästa** om alla hello kontroller lyckas. Om inte hello loggen och Lös hello-fel.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)
5. I **VM-konfiguration** se till att inställningarna för återställning av hello korrekt inställd. Du kan ändra hello VM inställningar om du behöver.

   ![](./media/site-recovery-failback-azure-to-vmware/image40.png)
6. Granska hello listan över virtuella datorer som ska återställas och anger ett. Observera att virtuella datorer visas med hjälp av hello datorvärdnamn. Det kan vara svårt toomap hello datorn namnet toohello virtuella värddatorn.
   toomap hello namn, gå toohello virtuella datorer **instrumentpanelen** i Azure och kontrollera hello virtuella datorns värdnamn.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)
7. Ange ett namn och välj **senare återställa**. Vi rekommenderar senare toorecover eftersom hello inledande skyddet kan vara ofullständiga.
8. Klicka på **återställa** toosave hello plan utlösaren hello återställning eller om du har markerat toorecover nu och senast. Du kan kontrollera hello recovery status toosee om hello plan har sparats.

   ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Återställa virtuella datorer
När hello planen har skapats, kan du återställa hello virtuella datorer. Innan du markerar den hello virtuella har datorer slutfört synkroniseringen. Om replikeringsstatusen är OK hello skyddet har slutförts och hello Återställningspunktmål tröskelvärde har uppfyllts. Du kan kontrollera hälsa i hello VM-egenskaper.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Inaktivera hello Azure virtuella datorer innan du startar hello återställning. Detta säkerställer att det finns ingen delad hjärna och att användarna bara åtkomst till en kopia av programmet hello.

1. Starta hello sparade plan. Välj i vContinuum **övervakaren** hello planer. Detta visar alla hello planer som har körts.

   ![](./media/site-recovery-failback-azure-to-vmware/image45.png)
2. Välj hello plan i **Recovery** och på **starta**. Du kan övervaka återställning. När virtuella datorer har aktiverats på kan du ansluta toothem i vCenter.

   ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-tooazure-again-after-failback"></a>Skydda tooAzure igen efter återställning efter fel
Återställning har slutförts. vill du förmodligen tooprotect hello virtuella datorer igen. Gör på följande sätt:

1. Kontrollera att hello virtuella datorer lokala fungerar och att program kan nås.
2. Välj hello virtuella datorer och ta bort dem i hello Azure Site Recovery-portalen. Välj toodisable hello skydd av hello virtuella datorer. Detta säkerställer att inga fler hello virtuella datorer är skyddade.
3. Ta bort hello redundansväxlats virtuella Azure-datorer i Azure
4. Ta bort hello gamla virtuell dator på vSpehere. Dessa är hello virtuella datorer som du tidigare redundansväxlade tooAzure.
5. Skydda hello virtuella datorer som nyligen redundansväxling i hello Site Recovery-portalen. När de är skyddade du kan lägga till dem tooa återställningsplan.

## <a name="next-steps"></a>Nästa steg
* [Läs mer om](site-recovery-vmware-to-azure-classic.md) replikera virtuella VMware-datorer och fysiska servrar tooAzure med hello förbättrad distribution.
