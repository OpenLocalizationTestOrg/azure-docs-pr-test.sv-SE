---
title: "aaaAzure Linux VM-agenten översikt | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera den virtuella datorns interaktion med Azure-Infrastrukturkontrollanten för Linux-agenten (waagent) toomanage."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: szark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4e08c84d9205f4db7aae6fd1568ec1f15fba395c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-and-using-hello-azure-linux-agent"></a>Förstå och använda hello Azure Linux-agenten
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Introduktion
hello Microsoft Azure Linux-agenten hanterar (waagent) Linux och FreeBSD etablering och VM-interaktion med hello Azure-Infrastrukturkontrollanten. Det innehåller följande funktioner för distributioner av Linux och FreeBSD IaaS hello:

> [!NOTE]
> Se hello Azure Linux-agenten [viktigt](https://github.com/Azure/WALinuxAgent/blob/master/README.md) för ytterligare information.
> 
> 

* **Bild etablering**
  
  * Skapa ett användarkonto
  * Konfigurera typer av SSH-autentisering
  * Distribution av offentliga SSH-nycklar och nyckelpar
  * Inställningen hello värdnamn
  * Publishing hello namn toohello värdplattform DNS
  * Rapportering SSH nyckel toohello plattform
  * Disk resurshantering
  * Formatering och monterar hello resurs disk
  * Konfigurera växlingsutrymme
* **Nätverk**
  
  * Hanterar vägar tooimprove kompatibilitet med plattformar DHCP-servrar
  * Garanterar hello stabiliteten i hello nätverksgränssnittets namn
* **Kernel**
  
  * Konfigurerar virtuell NUMA (inaktivera för kernel < 2.6.37)
  * Förbrukar Hyper-V entropi för /dev/random
  * Konfigurerar SCSI tidsgränser för hello rotenheten (som kan vara fjärråtkomst)
* **Diagnostik**
  
  * Konsolen omdirigering toohello seriell port
* **SCVMM-distributioner**
  
  * Identifierar och startar hello VMM-agenten för Linux vid körning i en miljö med System Center Virtual Machine Manager 2012 R2
* **Tillägg för virtuell dator**
  
  * Mata in komponenten som skapats av Microsoft och Partners i Linux VM (IaaS) tooenable program och konfiguration automation
  * VM-tillägget referensimplementering på [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)

## <a name="communication"></a>Kommunikation
hello informationsflödet från hello plattform toohello agent görs via två kanaler:

* En starten kopplade DVD för IaaS-distributioner. DVD-skivan innehåller ett OVF-kompatibel konfigurationsfil som innehåller alla Etableringsinformation än hello faktiska SSH keypairs.
* En TCP-slutpunkt som Exponerar en REST-API används tooobtain distribution och konfiguration av topologi.

## <a name="requirements"></a>Krav
hello följande system har testats och är känd toowork med hello Azure Linux-agenten:

> [!NOTE]
> Den här listan kan skilja sig från hello officiella lista över operativsystem som stöds på hello Microsoft Azure-plattformen, som beskrivs här: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)
> 
> 

* CoreOS
* CentOS 6.3 +
* Red Hat Enterprise Linux 6.7 +
* Debian 7.0 +
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP3 +
* Oracle Linux 6.4 +

Andra operativsystem som stöds:

* FreeBSD 10 + (Azure Linux-agenten v2.0.10 +)

hello Linux-agenten beror på vissa Systempaket i ordning toofunction korrekt:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Verktyg för filsystem: sfdisk datorn mkfs, åtskilda
* Lösenord verktyg: chpasswd sudo
* Text som bearbetar verktyg: sed grep
* Verktyg: IP-väg
* Kernel stöd för att montera UDF-filsystem.

## <a name="installation"></a>Installation
Installation med en RPM eller ett DEB-paket från din distribution paketdatabasen är hello önskad metod för installation och uppgradering hello Azure Linux-agenten. Alla hello [godkända distribution providers](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) integrera hello Azure Linux-agenten i sina avbildningar och databaser.

Se dokumentationen för toohello i hello [lagringsplatsen för Azure Linux-agenten på GitHub](https://github.com/Azure/WALinuxAgent) för avancerade installationsalternativ, till exempel installera från käll- eller toocustom platser eller prefix.

## <a name="command-line-options"></a>Kommandoradsalternativ
### <a name="flags"></a>Flaggor
* utförlig: öka mängden information i angivna kommandot
* tvinga: hoppa över interaktiva bekräftelse för vissa kommandon

### <a name="commands"></a>Kommandon
* hjälp: Visar hello stöds kommandon och flaggor.
* avetablering: försök tooclean hello system och gör den lämplig för etablering igen. Den här åtgärden tas bort hello följande:
  
  * Alla SSH-värdnycklar (om Provisioning.RegenerateSshHostKeyPair är ”y” i konfigurationsfilen för hello)
  * NameServer konfigurationen i /etc/resolv.conf
  * Rotlösenordet från/etc/Shadow (om Provisioning.DeleteRootPassword är ”y” i konfigurationsfilen för hello)
  * Cachelagrade DHCP-klientlån
  * Återställer värd namn toolocalhost.localdomain

> [!WARNING]
> Avetablering garanterar inte hello avbildningen är avmarkerad av all känslig information och lämpar sig för omfördelning.
> 
> 

* avetablering + användare: utför allt under - deprovision (ovan) och även tar bort hello senaste etablerade användarkonto (hämtades från /var/lib/waagent) och tillhörande data. Den här parametern är att etablera en avbildning som tidigare etablering i Azure så att den kan samlas in och används igen.
* version: Visar hello version av waagent
* serialconsole: konfigurerar GRUB toomark ttyS0 (hello första serieport) som hello Start konsol. Detta säkerställer att kernel Autostart loggar skickas toothe seriell port och tillgängliga för felsökning.
* Daemon: kört waagent som en daemon toomanage interaktion med hello-plattformen. Det här argumentet är angivna toowaagent i hello waagent init-skriptet.
* Starta: kört waagent i bakgrunden

## <a name="configuration"></a>Konfiguration
En konfigurationsfil (/ etc/waagent.conf) kontroller hello åtgärder för waagent. En exempelkonfigurationsfilen visas nedan:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

hello olika konfigurationsalternativ beskrivs i detalj nedan. Konfigurationsalternativ som är av tre typer. Boolesk, sträng eller heltal. hello booleskt konfigurationsalternativ kan anges som ”y” eller ”n”. Hej särskilda nyckelordet ”None” kan användas för vissa sträng typen konfigurationsposter enligt anvisningarna nedan.

**Provisioning.Enabled:**  
Typ: booleskt  
Standard: y

Detta gör att hello användaren tooenable eller inaktivera hello etablering funktioner i hello agent. Giltiga värden är ”y” eller ”n”. Om etablering inaktiveras SSH-värden och användaren nycklar hello bild bevaras och en konfiguration som angetts i hello Azure etablering API ignoreras.

> [!NOTE]
> Hej `Provisioning.Enabled` som standard för ”n” på Ubuntu molnet bilder som använder molnet init för etablering.
> 
> 

**Provisioning.DeleteRootPassword:**  
Typ: booleskt  
Standard: n

Om mängd hello rotlösenordet i filen hello/etc/shadow raderas under hello etableringsprocessen.

**Provisioning.RegenerateSshHostKeyPair:**  
Typ: booleskt  
Standard: y

Om mängd alla SSH värden nyckelpar (ecdsa, dsa och rsa) tas bort under hello etableringsprocessen från/etc/ssh /. Och en enda ny nyckelpar genereras.

hello krypteringstyp för hello ny nyckelpar kan konfigureras av hello Provisioning.SshHostKeyPairType post. Observera att vissa distributioner återskapas SSH-nyckelpar för valfri krypteringstyp som saknas när hello SSH-daemon startas (till exempel vid en omstart).

**Provisioning.SshHostKeyPairType:**  
Typ: Sträng  
Standard: rsa

Detta kan ställas in tooan algoritmen krypteringstyp som stöds av hello SSH-daemon på hello virtuella datorn. hello stöds vanligtvis värden är ”rsa”, ”dsa” och ”ecdsa”. Observera att ”putty.exe” i Windows inte har stöd för ”ecdsa”. Så om du planerar toouse putty.exe på Windows tooconnect tooa Linux-distribution, Använd ”rsa” eller ”dsa”.

**Provisioning.MonitorHostName:**  
Typ: booleskt  
Standard: y

Om waagent ska övervaka hello virtuell Linux-dator för hostname ändringar (som returneras av kommandot för hello ”hostname”) och automatiskt uppdatera hello nätverkskonfigurationen i hello avbildningen tooreflect hello ändra. I ordning toopush hello namn ändra toohello DNS-servrar, nätverk kommer att startas om i hello virtuell dator. Detta resulterar i korthet förlust av Internet-anslutning.

**Provisioning.DecodeCustomData**  
Typ: booleskt  
Standard: n

Om har angetts waagent avkodar CustomData från Base64.

**Provisioning.ExecuteCustomData**  
Typ: booleskt  
Standard: n

Om det har angetts waagent körs CustomData efter etableringen.

**Provisioning.PasswordCryptId**  
Typ: sträng  
Standard: 6

Algoritm som används av crypt vid generering av lösenords-hash.  
 1 - MD5  
 2a - Blowfish  
 5 - SHA-256  
 6 - SHA-512  

**Provisioning.PasswordCryptSaltLength**  
Typ: sträng  
Standard: 10

Längden på slumpmässiga salt som används vid generering av lösenords-hash.

**ResourceDisk.Format:**  
Typ: booleskt  
Standard: y

Om har angetts hello resurs disketten hello plattform formateras och monteras med waagent om hello filesystem typen som begärts av hello användare i ”ResourceDisk.Filesystem” är något annat än ”ntfs”. En partition av typen Linux (83) görs tillgänglig hello disk. Observera att den här partitionen inte formateras om den monteras har.

**ResourceDisk.Filesystem:**  
Typ: Sträng  
Standard: ext4

Detta anger hello filesystem hello resurs disk. Värden som stöds beror på Linux-distribution. Om hello X, sedan mkfs. X ska finnas på hello Linux avbildningen. SLES 11 bilder vanligtvis använda 'ext3'. FreeBSD bilder ska använda 'ufs2' här.

**ResourceDisk.MountPoint:**  
Typ: Sträng  
Standard: / mnt/resurs 

Detta anger Hej då hello resurs disken är monterad. Observera att hello resurs disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade.

**ResourceDisk.MountOptions**  
Typ: Sträng  
Standard: ingen

Anger disk montera toobe skickades toohello mount -o alternativ. Detta är en kommaavgränsad lista med värden, t.ex. 'nodev nosuid'. Se mount(8) mer information.

**ResourceDisk.EnableSwap:**  
Typ: booleskt  
Standard: n

Om ange en växlingsfil (/ swapfile) skapas på disken för hello resurs och läggs toohello system växlingsutrymme.

**ResourceDisk.SwapSizeMB:**  
Typ: heltal  
Standard: 0

hello storleken på växlingsfilen för hello i megabyte.

**Logs.Verbose:**  
Typ: booleskt  
Standard: n

Om mängd loggen detaljnivå förstärks. Waagent loggar too/var/log/waagent.log och utnyttjar hello logrotate funktioner toorotate systemloggar.

**OS. EnableRDMA**  
Typ: booleskt  
Standard: n

Om hello agent kommer försök tooinstall och läsa in en RDMA kernel-drivrutinen som matchar hello-versionen av den inbyggda programvaran hello på hello underliggande maskinvara.

**OS. RootDeviceScsiTimeout:**  
Typ: heltal  
Standard: 300

Detta konfigurerar hello SCSI-tidsgräns i sekunder för hello OS-disk- och enheter. Om inte ange hello system som standard.

**OS. OpensslPath:**  
Typ: Sträng  
Standard: ingen

Detta kan vara används toospecify en alternativ sökväg för hello openssl binära toouse för kryptografiska åtgärder.

**HttpProxy.Host HttpProxy.Port**  
Typ: Sträng  
Standard: ingen

Om har angetts hello agenten ska använda denna proxy server tooaccess hello internet. 

## <a name="ubuntu-cloud-images"></a>Ubuntu molnet bilder
Observera att använda Ubuntu molnet bilder [moln init](https://launchpad.net/ubuntu/+source/cloud-init) tooperform många konfigurationsåtgärder som annars skulle hanteras av hello Azure Linux-agenten.  Observera hello följande skillnader:

* **Provisioning.Enabled** standardvärden för ”n” på Ubuntu molnet bilder som använder molnet init tooperform etablering uppgifter.
* hello följande konfigurationsparametrar som påverkar inte Ubuntu molnet bilder som använder molnet init toomanage hello resurs disk och växla utrymme:
  
  * **ResourceDisk.Format**
  * **ResourceDisk.Filesystem**
  * **ResourceDisk.MountPoint**
  * **ResourceDisk.EnableSwap**
  * **ResourceDisk.SwapSizeMB**
* Se följande resurser tooconfigure hello resurs disk monteringspunkt hello och växlingsutrymme på Ubuntu molnet bilder under etablering:
  
  * [Ubuntu Wiki: Konfigurera växlingen partitioner](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [Placera anpassade Data till en virtuell Azure-dator](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

