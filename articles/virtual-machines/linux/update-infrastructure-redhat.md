---
title: aaaRed Hat Update infrastruktur (RHUI) | Microsoft Docs
description: "Lär dig mer om Red Hat Update infrastruktur (RHUI) för på begäran Red Hat Enterprise Linux-instanser i Microsoft Azure"
services: virtual-machines-linux
documentationcenter: 
author: BorisB2015
manager: timlt
editor: 
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/13/2017
ms.author: borisb
ms.openlocfilehash: cc244857104b25e4e61862c518db77e915e137ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat Update infrastruktur (RHUI) för att på begäran Red Hat Enterprise Linux virtuella datorer i Azure
Virtuella datorer skapas från hello på begäran Red Hat Enterprise Linux (RHEL) bilder som finns i Azure Marketplace är registrerade tooaccess hello Red Hat Update infrastruktur (RHUI) distribuerade i Azure.  hello på begäran RHEL instanser ha åtkomst tooa regionala yum databasen och kan tooreceive inkrementella uppdateringar.

Hej yum listan som hanteras av RHUI har konfigurerats i din instans av RHEL under etableringen. Du behöver inte toodo någon ytterligare konfiguration – kör `yum update` när RHEL-instans är klar tooget hello senaste uppdateringarna.

> [!NOTE]
> I September 2016 distribuerade vi en uppdaterad Azure RHUI och i januari 2017 startade vi stegvis avstängning av hello äldre Azure RHUI. Om du har använt hello RHEL bilder (eller deras ögonblicksbilder) från September 2016 eller senare - troligen krävs ingen åtgärd. Om du har dock äldre ögonblicksbilder/virtuella datorer, måste deras konfiguration uppdateras för oavbruten tillgång toohello Azure RHUI toobe.
> 

## <a name="rhui-azure-infrastructure-update"></a>Uppdatering av RHUI Azure-infrastrukturen
Från och med September 2016 har Azure en ny uppsättning Red Hat Update infrastruktur (RHUI) servrar. Dessa servrar distribueras med [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) så att en enda slutpunkt (rhui 1.microsoft.com) kan användas av alla VM oavsett region. Hej nya RHEL betala per användning (PAYG) bilder i hello Azure Marketplace (versioner datum September 2016 eller senare) punkt toohello nya Azure RHUI servrar och inte kräver någon ytterligare åtgärd.

### <a name="determine-if-action-is-required"></a>Avgöra om åtgärd krävs
Följ anvisningarna nedan om du har problem med att ansluta tooAzure RHUI från din Azure RHEL PAYG VM
1. Kontrollera konfigurationen för virtuell dator för Azure RHUI slutpunkt

    Kontrollera om `/etc/yum.repos.d/rh-cloud.repo` filen innehåller referens för`rhui-[1-3].microsoft.com` i baseurl av `[rhui-microsoft-azure-rhel*]` i hello-filen. Om det är – du använder hello nya Azure RHUI.

    Om den peka tooa plats med hello enligt `mirrorlist.*cds[1-4].cloudapp.net` -hello konfiguration måste uppdateras.

    Om du använder hello ny konfiguration och fortfarande inte kan ansluta tooAzure RHUI - filen som ett supportärende med Microsoft eller Red Hat.

    > [!NOTE]
    > Åtkomst tooAzure värd RHUI är begränsad toohello VM: ar inom [Microsoft Azure Datacenter IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653).
    > 

2. Om hello gamla Azure RHUI fortfarande är tillgänglig när du gör den här kontrollen och du vill att tooautomatically uppdateringskonfiguration hello, kör du följande kommando hello:

    `sudo yum update RHEL6`eller `sudo yum update RHEL7` beroende på hello RHEL-versionen.

3. Om du inte längre kan ansluta toohello gamla Azure RHUI, följ hello manuella stegen som beskrivs i nästa avsnitt om hello.

4. Kontrollera att tooupdate hello konfiguration på hello källan/ögonblicksbilden påverkas VM har etablerats från.

### <a name="phased-shutdown-of-hello-old-azure-rhui"></a>Stegvis avstängning av hello gamla Azure RHUI
Under hello avstängning av hello gamla Azure RHUI vi begränsa åtkomst till tooit på följande sätt:

1. Ytterligare begränsa åtkomst (ACL) tooset IP-adresser som redan ansluter tooit. Möjliga sidoeffekter: Om du fortsätter med hello gamla Azure RHUI – din nya virtuella datorer kanske inte kan tooconnect tooit. RHEL virtuella datorer med dynamiska IP-adresser som avstängning/frigöra/starta rad kan få nya IP och därför också kunde starta misslyckas tooconnect toohello gamla Azure RHUI

2. Stäng av spegling innehållsleverans servrar. Möjliga sidoeffekter: som vi Stäng mer CDSes kan du se längre `yum update` behandlingen tid mer tidsgränser fram till hello peka när du inte längre kan ansluta toohello gamla Azure RHUI.

### <a name="hello-ips-for-hello-new-rhui-content-delivery-servers-are"></a>hello IP-adresser för hello nya RHUI innehållsleverans servrar är
Om du använder network configuration toofurther begränsa åtkomst från RHEL PAYG virtuella datorer, se till att hello efter IP-adresser tillåts för `yum update` toowork beroende på hello-miljö i. 

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193

# Azure Germany
51.5.243.77
51.4.228.145
```

### <a name="manual-update-procedure-toouse-hello-new-azure-rhui-servers"></a>Manuell uppdatering proceduren toouse hello nya Azure RHUI servrar
Hämta (via curl) hello offentliga nyckel signatur

```bash
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Verifiera hello ned nyckeln

```bash
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Kontrollera hello utdata, kontrollera `keyid` och `user ID packet`:

```bash
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

Installera hello offentlig nyckel

```bash
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Hämta, kontrollera och installera klienten RPM

Hämta: För RHEL 6

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

För RHEL 7

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Kontrollera:

```bash
rpm -Kv azureclient.rpm
```

Kontrollera i utdata signaturen för hello paketet är OK

```bash
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Installera hello RPM

```bash
sudo rpm -U azureclient.rpm
```

Kontrollera att du har åtkomst till Azure RHUI formuläret hello VM när åtgärden har slutförts

### <a name="all-in-one-script-for-automating-hello-preceding-task"></a>Allt i ett skript för att automatisera hello föregående aktivitet
Använd följande skript som behövs tooautomate hello aktivitet för att uppdatera berörda VMs toohello nya Azure RHUI servrar hello.

```sh
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>Översikt över RHUI
[Infrastrukturen för Red Hat](https://access.redhat.com/products/red-hat-update-infrastructure) erbjuder en mycket skalbar lösning toomanage yum databasinnehåll för Red Hat Enterprise Linux moln-instanser som hanteras av Red Hat-certifierad molntjänstleverantörer. Baserat på hello överordnade massa projektet RHUI gör att molntjänstleverantörer toolocally spegling Red Hat-värdbaserad databasinnehåll, skapa anpassade databaser med sitt eget innehåll och gör dessa databaser tillgängliga tooa stor grupp med användare via en Utjämning av nätverksbelastning Content delivery system.

## <a name="regions-where-rhui-is-available"></a>Regioner där RHUI är tillgängligt
RHUI är tillgänglig i alla regioner där RHEL på begäran bilder är tillgängliga. För närvarande innehåller alla offentliga områden på hello [Azure status instrumentpanelen](https://azure.microsoft.com/status/) sidan Azure som tillhör amerikanska myndigheter och Azure Tyskland regioner. RHUI åtkomst för virtuella datorer som etablerats från RHEL på begäran avbildningar finns i deras pris. Ytterligare regionala/nationella molnet tillgänglighet kommer att uppdateras när vi Expandera RHEL på begäran tillgänglighet i hello framtida.

> [!NOTE]
> Åtkomst tooAzure värd RHUI är begränsad toohello VM: ar inom [Microsoft Azure Datacenter IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653).
> 
> 

## <a name="get-updates-from-another-update-repository"></a>Hämta uppdateringar från en annan uppdatering databas
Om du behöver tooget uppdateringar från en annan uppdatering databas (i stället för Azure-baserad RHUI), måste du först toounregister dina instanser från RHUI. Du måste registrera toore dem med hello önskade infrastrukturen (till exempel Red Hat satellit eller Red Hat kundens Portal CDN). Du behöver lämpliga Red Hat prenumerationer för dessa tjänster och registrering för [Red Hat Molnåtkomst i Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).

Så här toounregister RHUI och registrera om tooyour uppdatering infrastruktur:

1. Redigera /etc/yum.repos.d/rh-cloud.repo och ändra alla `enabled=1` för`enabled=0`. Exempel:
   
   ```bash
   sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo
   ```
   
2. Redigera /etc/yum/pluginconf.d/rhnplugin.conf och ändra `enabled=0` för`enabled=1`.
3. Registrera sedan med hello önskade infrastrukturen, till exempel Red Hat-kundportalen. Följ Red Hat solution guide på [hur tooregister och prenumerera på system-toohello Red Hat-kundportalen](https://access.redhat.com/solutions/253273).

> [!NOTE]
> Åtkomst toohello Azure-baserad RHUI ingår i hello RHEL betala per användning (PAYG) avbildningen pris. Avregistrerar en virtuell dator RHEL PAYG från hello Azure-baserad RHUI konverterar inte hello virtuella datorn till Bring-Your-äger-licens (BYOL) typ VM. Om du registrerar hello samma virtuella dator med en annan källa för uppdateringar kan du drabbas dubbla avgifter: första gången för Azure RHEL programvara avgift och hello gång för Red Hat-abonnemang. 
> 
> Om du behöver konsekvent toouse en annan infrastruktur än Azure-baserad RHUI du skapa och distribuera dina egna avbildningar (BYOL-typ) enligt beskrivningen i [skapa och ladda upp Red Hat-baserad virtuell dator för Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikel.
> 

## <a name="next-steps"></a>Nästa steg
toocreate en Red Hat Enterprise Linux virtuell dator från Azure Marketplace betala per användning avbildningen och utnyttjar Azure-baserad RHUI gå för[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). Du kommer att kunna toouse `yum update` i din RHEL instans utan några ytterligare inställningar.

