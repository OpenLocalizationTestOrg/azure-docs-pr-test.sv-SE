---
title: "aaaConfigure MPIO på StorSimple Linux-värd | Microsoft Docs"
description: "Konfigurera MPIO på StorSimple anslutna tooa Linux-värd som kör CentOS 6.6"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: tysonn
ms.assetid: ca289eed-12b7-4e2e-9117-adf7e2034f2f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: alkohli
ms.openlocfilehash: d9f7e02903243494c909313fb2c33ac690764274
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a>Konfigurera MPIO på en StorSimple-värd som kör CentOS
Den här artikeln förklarar hello steg krävs tooconfigure flera sökvägar I/O (MPIO) på värdservern Centos 6.6. hello-värdservern är anslutna tooyour Microsoft Azure StorSimple-enhet för hög tillgänglighet via iSCSI-initierare. Det beskrivs i detalj hello automatisk identifiering av multipath enheter och hello specifika inställningarna endast för StorSimple-volymer.

Den här proceduren är tillämpliga tooall hello modeller av StorSimple 8000-serien enheter.

> [!NOTE]
> Den här proceduren kan inte användas för en virtuell StorSimple-enhet. Mer information finns i hur tooconfigure värdservrar för din virtuella enhet.
> 
> 

## <a name="about-multipathing"></a>Om flera sökvägar
hello MPIO-funktionen kan du tooconfigure flera i/o-sökvägar mellan en värdserver och en lagringsenhet. Dessa i/o-sökvägar är fysiska SAN-anslutningar som kan innehålla olika kablar, växlar, nätverksgränssnitt och domänkontrollanter. Multipathing aggregerar hello i/o-sökvägar, tooconfigure en ny enhet som är associerad med alla hello samman sökvägar.

hello syftet med flera sökvägar är dubbla:

* **Hög tillgänglighet**: den ger en alternativ sökväg om någon del av hello i/o-sökväg (t.ex en kabel växel, nätverksgränssnittet eller domänkontrollant) misslyckas.
* **Belastningsutjämning**: beroende på hello konfiguration av lagringsenheten, förbättrar den hello prestanda genom att identifiera belastningar hello i/o-sökvägar och dynamiskt ombalansering dessa belastning.

### <a name="about-multipathing-components"></a>Om MPIO-komponenter
Flera sökvägar i Linux består av kernel-komponenter och användaren utrymme komponenter enligt tabellen nedan.

* **Kernel**: hello huvudkomponenten är hello *enheten mapper* som dras om i/o och stöd för växling vid fel för sökvägar och sökvägen grupper.

* **Användaren kan**: dessa är *multipath verktyg* som hanterar multipathed enheter genom att uppmana hello enhet – multipath mappningsmodulen vilka toodo. hello-verktygen består av:
   
   * **Multipath**: Visar och konfigurerar multipathed enheterna.
   * **Multipathd**: daemon som kör multipath och Övervakare hello sökvägar.
   * **Devmap namn**: ger en beskrivande enhetsnamn tooudev för devmaps.
   * **Kpartx**: mappar linjär devmaps toodevice partitioner toomake multipath maps partitionable.
   * **Multipath.conf**: konfigurationsfilen för flera sökvägar daemon som används toooverwrite hello inbyggda configuration tabell.

### <a name="about-hello-multipathconf-configuration-file"></a>Om hello multipath.conf konfigurationsfilen
hello konfigurationsfilen `/etc/multipath.conf` gör många hello MPIO-funktioner kan konfigureras av användaren. Hej `multipath` kommandot och hello kernel-daemon `multipathd` använda informationen i den här filen. hello filen samråd endast under hello konfigurationen av hello multipath enheter. Kontrollera att alla ändringar innan du kör hello `multipath` kommando. Om du ändrar hello filen efteråt du behöver toostop och starta multipathd igen för hello ändringar tootake effekt.

Hej multipath.conf har fem avsnitt:

- **Nivån systemstandard** *(standardinställningen)*: du kan åsidosätta standardvärdena för nivån.
- **Övriga enheter** *(svartlistat)*: du kan ange hello lista över enheter som inte kan styras av enheten mapper.
- **Svartlista undantag** *(blacklist_exceptions)*: du kan identifiera specifika enheter toobe behandlas som multipath enheter även om anges i hello svartlistat.
- **Specifika inställningar för lagring controller** *(enheter)*: du kan ange inställningar som kommer att tillämpas toodevices som innehåller information om leverantör och produkt.
- **Specifika Enhetsinställningar** *(multipaths)*: du kan använda det här avsnittet toofine finjustera hello konfigurationsinställningar för enskilda LUN.

## <a name="configure-multipathing-on-storsimple-connected-toolinux-host"></a>Konfigurera MPIO på StorSimple anslutna tooLinux värd
En StorSimple enhet ansluten tooa Linux-värd kan konfigureras för hög tillgänglighet och belastningsutjämning. Till exempel om hello Linux-värd har två gränssnitt som är anslutna toohello SAN och hello enhet har två gränssnitt anslutna toohello SAN-nätverk så att dessa gränssnitt finns på hello samma undernät och det ska finnas 4 sökvägar. Men om varje gränssnitt för DATA i hello gränssnitt för enheten och värden är i ett annat IP-undernät (och inte dirigerbara) kan blir endast 2 sökvägar tillgängliga. Du kan konfigurera flera sökvägar tooautomatically identifiera alla hello tillgängliga sökvägar, välja en algoritm för belastningsutjämning för dessa sökvägar, tillämpa specifika konfigurationsinställningar för endast StorSimple-volymer, och sedan aktivera och kontrollera MPIO.

hello beskrivs nedan hur tooconfigure MPIO när en StorSimple-enhet med två nätverksgränssnitt är anslutna tooa värd med två nätverksgränssnitt.

## <a name="prerequisites"></a>Krav
Det här avsnittet beskrivs hello konfigurationskrav för CentOS server och din StorSimple-enhet.

### <a name="on-centos-host"></a>På CentOS värden
1. Kontrollera att värddatorn CentOS har 2 nätverksgränssnitt som är aktiverad. Ange:
   
    `ifconfig`
   
    hello följande exempel visar hello utdata när två nätverksgränssnitt (`eth0` och `eth1`) finns på hello värden.
   
        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
          inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)
   
        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
          inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)
   
        loLink encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)
2. Installera *verktyg för iSCSI-initieraren-webbplatsuppgradering* på CentOS servern. Utför följande steg tooinstall hello *verktyg för iSCSI-initieraren-webbplatsuppgradering*.
   
   1. Logga in som `root` till CentOS-värden.
   2. Installera hello *verktyg för iSCSI-initieraren-webbplatsuppgradering*. Ange:
      
       `yum install iscsi-initiator-utils`
   3. Efter hello *verktyg för iSCSI-initieraren-webbplatsuppgradering* är korrekt installerat, starta hello iSCSI-tjänsten. Ange:
      
       `service iscsid start`
      
       Vid tillfällen `iscsid` får inte börja och hello `--force` alternativet kan behövas
   4. tooensure att iSCSI-initieraren är aktiverad under starten, Använd hello `chkconfig` kommandot tooenable hello-tjänsten.
      
       `chkconfig iscsi on`
   5. tooverify att det var korrekt installation, kör hello-kommando:
      
       `chkconfig --list | grep iscsi`
      
       Ett exempel på utdata visas nedan.
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       Du kan se att iSCSI-miljö ska köras på starttiden på Kör nivå 2, 3, 4 och 5 från hello ovan exempel.
3. Installera *enhet-mapper-multipath*. Ange:
   
    `yum install device-mapper-multipath`
   
    hello installationen startar. Typen **Y** toocontinue när du uppmanas att bekräfta.

### <a name="on-storsimple-device"></a>På StorSimple-enhet
Din StorSimple-enhet bör ha:

* Minst två gränssnitt som är aktiverade för iSCSI. tooverify att två gränssnitt som är iSCSI-aktiverade på din StorSimple-enhet utför hello följa stegen i hello Azure klassiska portal för din StorSimple-enhet:
  
  1. Logga in hello klassiska portalen för din StorSimple-enhet.
  2. Välj din StorSimple Manager-tjänsten, klicka på **enheter** och välj hello specifika StorSimple-enhet. Klicka på **konfigurera** och kontrollera hello inställningar för nätverksgränssnitt. En skärmbild med två iSCSI-aktiverade nätverkskort visas nedan. Här DATA 2 och DATA 3, både 10 GbE gränssnitt har aktiverats för iSCSI.
     
      ![MPIO StorsSimple DATA 2-konfiguration](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![MPIO StorSimple DATA 3 Config](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      I hello **konfigurera** sidan
     
     1. Se till att båda nätverksgränssnitt är iSCSI-aktiverade. Hej **iSCSI-aktiverade** fält ska anges för**Ja**.
     2. Se till att hello nätverksgränssnitt har hello samma hastighet båda vara 1 GbE- eller 10 GbE.
     3. Notera hello IPv4-adresserna till hello iSCSI-aktiverade gränssnitt och spara för senare användning på hello värden.
* hello iSCSI-gränssnitt på StorSimple-enheten ska kunna nås från hello CentOS server.
      tooverify, du behöver tooprovide hello IP-adresser för din StorSimple iSCSI-aktiverade nätverkskort på värdservern. Hej kommandon som används och hello motsvarande utdata med DATA2 (10.126.162.25) och DATA3 (10.126.162.26) visas nedan:
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a>Maskinvarukonfiguration
Vi rekommenderar att du ansluter hello två iSCSI nätverksgränssnitt på separata sökvägar för redundans. hello bilden nedan visar hello rekommenderade maskinvarukonfigurationen för hög tillgänglighet och belastningsutjämning MPIO för din CentOS server och StorSimple-enhet.

![MPIO maskinvarukonfiguration för StorSimple tooLinux värden](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

Som visas i föregående bild hello:

* StorSimple-enheten är i ett aktivt-passivt konfiguration med två domänkontrollanter.
* Två SAN-switchar är anslutna tooyour styrenheter.
* Två iSCSI-initierarna har aktiverats på din StorSimple-enhet.
* Två nätverksgränssnitt är aktiverade på CentOS-värden.

hello ovan konfiguration kommer att ge 4 separat sökvägar mellan enheten och hello värden om hello gränssnitt för data från värdar och är dirigerbart.

> [!IMPORTANT]
> * Vi rekommenderar att du inte blandar 1 GbE och 10 GbE-nätverkskort för flera sökvägar. När du använder två nätverksgränssnitt, ska båda gränssnitten hello vara hello identiska.
> * På din StorSimple-enhet DATA0, fil1, DATA4 och DATA5 är 1 GbE-gränssnitt DATA2 och DATA3 är 10 GbE-nätverkskort. |
> 
> 

## <a name="configuration-steps"></a>Konfigurationssteg
hello konfigurationssteg för flera sökvägar innebär konfigurering hello tillgängliga sökvägar för automatisk identifiering, ange hello belastningsutjämning algoritmen toouse, aktivera MPIO och slutligen verifiera hello konfiguration. Var och en av dessa steg beskrivs i detalj i följande avsnitt hello.

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a>Steg 1: Konfigurera MPIO för automatisk identifiering
hello stöds multipath enheter kan identifieras och konfigureras automatiskt.

1. Initiera `/etc/multipath.conf` fil. Ange:
   
     `mpathconf --enable`
   
    hello ovanstående kommando skapar en `sample/etc/multipath.conf` fil.
2. Starta flera sökvägar. Ange:
   
    `service multipathd start`
   
    Hello följande utdata visas:
   
    `Starting multipathd daemon:`
3. Aktivera automatisk identifiering av multipaths. Ange:
   
    `mpathconf --find_multipaths y`
   
    Detta kommer att ändra hello-standardinställningar för din `multipath.conf` enligt nedan:
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a>Steg 2: Konfigurera MPIO för StorSimple-volymer
Som standard alla enheter är svart som anges i hello multipath.conf fil och ska kringgås. Du behöver toocreate svartlistat undantag tooallow MPIO för volymer från StorSimple-enheter.

1. Redigera hello `/etc/mulitpath.conf` fil. Ange:
   
    `vi /etc/multipath.conf`
2. Leta upp hello blacklist_exceptions avsnitt i hello multipath.conf fil. Din StorSimple-enhet måste toobe anges som ett svartlistat undantag i det här avsnittet. Du kan ta bort kommentarerna relevanta rader i den här filen toomodify det som visas nedan (Använd endast hello specifika modell för hello-enhet du använder):
   
        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a>Steg 3: Konfigurera MPIO för resursallokering
Belastningsutjämning algoritmen använder alla hello tillgängliga multipaths toohello aktiva styrenheten i ett balanserat, resursallokering sätt.

1. Redigera hello `/etc/multipath.conf` fil. Ange:
   
    `vi /etc/multipath.conf`
2. Under hello `defaults` avsnitt, ange hello `path_grouping_policy` för`multibus`. Hej `path_grouping_policy` anger hello standard sökväg gruppering princip tooapply toounspecified multipaths. hello-standardinställningar ser ut som nedan.
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> Hej vanligaste värdena för `path_grouping_policy` omfattar:
> 
> * redundans = 1 sökväg per prioritet grupp
> * multibus = alla giltiga sökvägar i grupp 1 prioritet
> 
> 

### <a name="step-4-enable-multipathing"></a>Steg 4: Aktivera MPIO
1. Starta om hello `multipathd` daemon. Ange:
   
    `service multipathd restart`
2. hello utdata blir enligt nedan:
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a>Steg 5: Kontrollera MPIO
1. Kontrollera att iSCSI-anslutning upprättas med hello StorSimple-enheten på följande sätt:
   
   a. Identifiera din StorSimple-enhet. Ange:
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on hello device>:<iSCSI port on StorSimple device>
    ```
    
    hello utdata när IP-adress för DATA0 är 10.126.162.25 och port 3260 öppnas på hello StorSimple-enhet för utgående iSCSI-trafik är enligt nedan:
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    Kopiera hello IQN för din StorSimple-enhet `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, från hello föregående utdata.

   b. Anslut med hjälp av mål IQN toohello-enhet. Hej StorSimple-enhet är hello iSCSI-målet här. Ange:

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    hello följande exempel visas med ett mål IQN av `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`. hello utdata visar att du har anslutit toohello två iSCSI-aktiverade nätverksgränssnitt på enheten.

    ```
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    ```

    Om du ser en enda värd-gränssnittet och här två sökvägar måste tooenable båda hello gränssnitten på värden för iSCSI. Du kan följa hello [detaljerade instruktioner i dokumentationen för Linux](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).

2. En volym är utsatta toohello CentOS servern från hello StorSimple-enhet. Mer information finns i [steg 6: skapa en volym](storsimple-deployment-walkthrough.md#step-6-create-a-volume) via hello klassiska Azure-portalen på StorSimple-enheten.

3. Kontrollera hello tillgängliga sökvägar. Ange:

      ```
      multipath –l
      ```

      hello som följande exempel visar hello utdata för två nätverkskort på en StorSimple-enhet ansluten tooa värddator nätverksgränssnitt med två tillgängliga sökvägar.

        ```
        mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 7:0:0:1 sdc 8:32 active undef running
        `- 6:0:0:1 sdd 8:48 active undef running
        ```

        hello following example shows hello output for two network interfaces on a StorSimple device connected tootwo host network interfaces with four available paths.

        ```
        mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 17:0:0:0 sdb 8:16 active undef running
        |- 15:0:0:0 sdd 8:48 active undef running
        |- 14:0:0:0 sdc 8:32 active undef running
        `- 16:0:0:0 sde 8:64 active undef running
        ```

        After hello paths are configured, refer toohello specific instructions on your host operating system (Centos 6.6) toomount and format this volume.

## <a name="troubleshoot-multipathing"></a>Felsöka MPIO
Det här avsnittet innehåller några nyttiga tips om du stöter på problem vid konfiguration av MPIO.

FRÅGOR. Visas inte hello ändringar i `multipath.conf` fil börjar gälla.

A. Om du har gjort några ändringar toohello `multipath.conf` filen, behöver du toorestart hello multipathing tjänst. Skriv följande kommando hello:

    service multipathd restart

FRÅGOR. Jag har aktiverat två nätverksgränssnitt på hello StorSimple-enhet och två nätverksgränssnitt på hello värden. När jag ange hello tillgängliga sökvägar, visas bara två sökvägar. Det förväntade toosee fyra tillgängliga sökvägar.

A. Kontrollera att hello två sökvägar finns på hello samma undernät och inbördes. Om hello nätverksgränssnitt finns på olika VLAN och inte dirigeras visas bara två sökvägar. Enkelriktade tooverify är toomake till att du kan nå båda gränssnitten med hello värden från ett nätverksgränssnitt på hello StorSimple-enhet. Du behöver för[kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) som den här kontrollen kan bara göras via en supportsession.

FRÅGOR. När jag Listar tillgängliga sökvägar syns inte några utdata.

A. Normalt inte ser några multipathed sökvägar tyder på problem med hello MPIO daemon och det är troligen att alla problem här ligger i hello `multipath.conf` fil.

Det kan även vara värt att kontrollera att du verkligen kan se några diskar efter anslutning toohello mål, eftersom inget svar från hello multipath listor kan också innebära du inte har några diskar.

* Använd följande kommando toorescan hello SCSI-bussen hello:
  
    `$ rescan-scsi-bus.sh `(en del av sg3_utils paketet)
* Skriv följande kommandon hello:
  
    `$ dmesg | grep sd*`
     
     Eller
  
    `$ fdisk –l`
  
    Dessa kommer att returnera information om nyligen tillagda diskar.
* toodetermine om det är en StorSimple-disk, Använd hello följande kommandon:
  
    `cat /sys/block/<DISK>/device/model`
  
    Detta returnerar en sträng som avgör om det är en StorSimple-disk.

En mindre troligt men möjlig orsak kan också vara inaktuella iscsid pid. Använd följande kommando toolog ut från iSCSI-sessioner för hello hello:

    iscsiadm -m node --logout -p <Target_IP>

Upprepa det här kommandot för alla hello anslutna nätverksgränssnitt på hello iSCSI-mål, vilket är din StorSimple-enhet. När du har loggat från alla hello iSCSI-sessioner, Använd hello iSCSI target IQN tooreestablish hello iSCSI-session. Skriv följande kommando hello:

    iscsiadm -m node --login -T <TARGET_IQN>


FRÅGOR. Jag är inte säker på om enheten är godkända.

A. tooverify om enheten är godkända, Använd följande felsökning interaktiva kommandot hello:

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


Mer information finns för[använda felsökning interaktiva kommandot för flera sökvägar](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).

## <a name="list-of-useful-commands"></a>Lista över användbara kommandon
| Typ | Kommando | Beskrivning |
| --- | --- | --- |
| **iSCSI** |`service iscsid start` |Starta iSCSI-tjänsten |
| &nbsp; |`service iscsid stop` |Stoppa tjänsten iSCSI |
| &nbsp; |`service iscsid restart` |Starta om iSCSI-tjänsten |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |Identifiera tillgängliga mål på hello angetts adress |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |Logga in toohello iSCSI-mål |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |Logga ut från hello iSCSI-mål |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |Namn på iSCSI-initierare |
| &nbsp; |`iscsiadm –m session –s <sessionid> -P 3` |Kontrollera hello tillstånd på hello iSCSI-session och volym som identifieras på hello värden |
| &nbsp; |`iscsi –m session` |Visar alla hello iSCSI-sessioner mellan hello värden och hello StorSimple-enhet |
|  | | |
| **Flera sökvägar** |`service multipathd start` |Starta daemon för flera sökvägar |
| &nbsp; |`service multipathd stop` |Stoppa multipath daemon |
| &nbsp; |`service multipathd restart` |Starta om multipath daemon |
| &nbsp; |`chkconfig multipathd on` </br> ELLER </br> `mpathconf –with_chkconfig y` |Aktivera multipath daemon toostart vid start |
| &nbsp; |`multipathd –k` |Starta hello interaktiva konsolen för felsökning |
| &nbsp; |`multipath –l` |Lista över multipath anslutningar och enheter |
| &nbsp; |`mpathconf --enable` |Skapa en exempelfil mulitpath.conf i`/etc/mulitpath.conf` |
|  | | |

## <a name="next-steps"></a>Nästa steg
Du kanske också måste toorefer toohello följande CentoS 6.6 dokument som du konfigurerar MPIO på Linux-värd:

* [Hur du konfigurerar MPIO på CentOS](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
* [Guide för Linux-utbildning](http://linux-training.be/files/books/LinuxAdm.pdf)

