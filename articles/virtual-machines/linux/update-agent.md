---
title: "aaaUpdate hello Azure Linux-agenten från GitHub | Microsoft Docs"
description: "Lär dig hur tooupdate Azure Linux-agenten för dina Linux VM i Azure toohello senaste versionen från GitHub"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: f1f19300-987d-4f29-9393-9aba866f049c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: mingzhan
ms.openlocfilehash: 4ce7c56efc1e6563e6415f7687573f9fb9e7b4c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-hello-azure-linux-agent-on-a-vm"></a>Hur tooupdate hello Azure Linux-agenten på en virtuell dator

tooupdate din [Azure Linux-agenten](https://github.com/Azure/WALinuxAgent) på en Linux-VM i Azure måste du ha:

- En körande Linux VM i Azure.
- En anslutning toothat Linux VM via SSH.

Du bör alltid kontrollera för ett paket i hello Linux distro databasen först. Det är möjligt hello paketet finns kanske inte hello senaste versionen, men att aktivera automatisk uppdatering säkerställer hello Linux-Agent kommer alltid att hämta hello senaste uppdateringen. Om du har problem med att installera från hello paketet chefer, bör du söka stöd från hello distro leverantör.

## <a name="updating-hello-azure-linux-agent"></a>Uppdatera hello Azure Linux-agenten

## <a name="ubuntu"></a>Ubuntu

#### <a name="check-your-current-package-version"></a>Kontrollera din nuvarande Paketversion

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a>Uppdatera paketcachen

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a>Installera hello senaste Paketversion

```bash
sudo apt-get install walinuxagent
```

#### <a name="ensure-auto-update-is-enabled"></a>Kontrollera att automatiska uppdateringar är aktiverat

Kontrollera först toosee om det är aktiverat:

```bash
cat /etc/waagent.conf
```

Hitta 'AutoUpdate.Enabled'. Om du ser den här utdatan aktiveras:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable kör:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Starta om hello waagent-tjänsten

#### <a name="restart-agent-for-1404"></a>Starta om agenten för 14.04

```bash
initctl restart walinuxagent
```

#### <a name="restart-agent-for-1604--1704"></a>Starta om agenten för 16.04 / 17.04

```bash
systemctl restart walinuxagent.service
```

## <a name="debian"></a>Debian

### <a name="debian-7-wheezy"></a>Debian 7 ”Wheezy”

#### <a name="check-your-current-package-version"></a>Kontrollera din nuvarande Paketversion

```bash
dpkg -l | grep waagent
```

#### <a name="update-package-cache"></a>Uppdatera paketcachen

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a>Installera hello senaste Paketversion

```bash
sudo apt-get install waagent
```

#### <a name="enable-agent-auto-update"></a>Aktivera agenten för automatisk uppdatering
Den här versionen av Debian inte har en version > = 2.0.16, automatisk uppdatering är därför inte tillgänglig för den. hello utdata från hello ovanstående kommando visar om hello paketet är uppdaterad.

### <a name="debian-8-jessie--debian-9-stretch"></a>Debian 8 ”Jessie” / Debian 9 ”Stretch”

#### <a name="check-your-current-package-version"></a>Kontrollera din nuvarande Paketversion

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a>Uppdatera paketcachen

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a>Installera hello senaste Paketversion

```bash
sudo apt-get install waagent
```
#### <a name="ensure-auto-update-is-enabled"></a>Kontrollera att automatiska uppdateringar är aktiverat 

Kontrollera först toosee om det är aktiverat:

```bash
cat /etc/waagent.conf
```

Hitta 'AutoUpdate.Enabled'. Om du ser den här utdatan aktiveras:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable kör:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Starta om hello waagent-tjänsten

```
sudo systemctl restart walinuxagent.service
```

## <a name="redhat--centos"></a>Redhat / CentOS

### <a name="rhelcentos-6"></a>RHEL/CentOS 6

#### <a name="check-your-current-package-version"></a>Kontrollera din nuvarande Paketversion

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Kontrollera tillgängliga uppdateringar

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a>Installera hello senaste Paketversion

```bash
sudo yum install WALinuxAgent
```

#### <a name="ensure-auto-update-is-enabled"></a>Kontrollera att automatiska uppdateringar är aktiverat 

Kontrollera först toosee om det är aktiverat:

```bash
cat /etc/waagent.conf
```

Hitta 'AutoUpdate.Enabled'. Om du ser den här utdatan aktiveras:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable kör:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Starta om hello waagent-tjänsten

```
sudo service waagent restart
```

### <a name="rhelcentos-7"></a>RHEL/CentOS 7

#### <a name="check-your-current-package-version"></a>Kontrollera din nuvarande Paketversion

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Kontrollera tillgängliga uppdateringar

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a>Installera hello senaste Paketversion

```bash
sudo yum install WALinuxAgent  
```

#### <a name="ensure-auto-update-is-enabled"></a>Kontrollera att automatiska uppdateringar är aktiverat 

Kontrollera först toosee om det är aktiverat:

```bash
cat /etc/waagent.conf
```

Hitta 'AutoUpdate.Enabled'. Om du ser den här utdatan aktiveras:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable kör:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Starta om hello waagent-tjänsten

```bash
sudo systemctl restart waagent.service
```

## <a name="suse-sles"></a>SUSE SLES

### <a name="suse-sles-11-sp4"></a>SUSE SLES 11 SP4

#### <a name="check-your-current-package-version"></a>Kontrollera din nuvarande Paketversion

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Kontrollera tillgängliga uppdateringar

hello ovan utdata visar om hello paketet är in toodate.

#### <a name="install-hello-latest-package-version"></a>Installera hello senaste Paketversion

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Kontrollera att automatiska uppdateringar är aktiverat 

Kontrollera först toosee om det är aktiverat:

```bash
cat /etc/waagent.conf
```

Hitta 'AutoUpdate.Enabled'. Om du ser den här utdatan aktiveras:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable kör:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Starta om hello waagent-tjänsten

```bash
sudo /etc/init.d/waagent restart
```

### <a name="suse-sles-12-sp2"></a>SUSE SLES 12 SP2

#### <a name="check-your-current-package-version"></a>Kontrollera din nuvarande Paketversion

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Kontrollera tillgängliga uppdateringar

Hello utdata från hello ovan och visar detta om hello paketet är upp till datum.

#### <a name="install-hello-latest-package-version"></a>Installera hello senaste Paketversion

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Kontrollera att automatiska uppdateringar är aktiverat 

Kontrollera först toosee om det är aktiverat:

```bash
cat /etc/waagent.conf
```

Hitta 'AutoUpdate.Enabled'. Om du ser den här utdatan aktiveras:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable kör:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Starta om hello waagent-tjänsten

```bash
sudo systemctl restart waagent.service
```

## <a name="oracle-6-and-7"></a>Oracle 6 och 7

För Oracle Linux, kontrollera att hello `Addons` databasen är aktiverad. Välj tooedit hello fil `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) eller `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), och ändra hello rad `enabled=0` för`enabled=1` under **[ol6_addons]** eller **[ol7_addons]** i den här filen.

Sedan tooinstall hello senaste versionen av hello Azure Linux-agenten, typ:

```bash
sudo yum install WALinuxAgent
```

Om du inte hittar hello tillägg databasen kan du bara lägga till dessa rader hello slutet av filen .repo enligt tooyour Oracle Linux-versionen:

För Oracle Linux 6 virtuella datorer:

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

För Oracle Linux 7 virtuella datorer:

```sh
[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0
```

Sedan skriver du:

```bash
sudo yum update WALinuxAgent
```

Detta är vanligtvis allt du behöver, men om du av någon anledning som du behöver tooinstall från https://github.com direkt, Använd hello följande steg.


## <a name="update-hello-linux-agent-when-no-agent-package-exists-for-distribution"></a>Uppdatera hello Linux-agenten när det finns ingen agentpaket för distribution

Installera wget (det finns vissa distributioner som inte installeras som standard, till exempel Redhat, CentOS och Oracle Linux-version 6.4 och 6.5) genom att skriva `sudo yum install wget` på hello-kommandoraden.

### <a name="1-download-hello-latest-version"></a>1. Hämta hello senaste versionen
Öppna [hello versionen av Azure Linux-agenten i GitHub](https://github.com/Azure/WALinuxAgent/releases) i en webbsida och ta reda på hello senaste versionsnumret. (Du hittar din nuvarande version genom att skriva `waagent --version`.)

#### <a name="for-version-22x-or-later-type"></a>För version 2.2.x eller senare, skriver du:
```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.x.zip
unzip v2.2.x.zip.zip
cd WALinuxAgent-2.2.x
```

hello använder följande rad version 2.2.0 som exempel:

```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.14.zip
unzip v2.2.14.zip  
cd WALinuxAgent-2.2.14
```

### <a name="2-install-hello-azure-linux-agent"></a>2. Installera hello Azure Linux-agenten

#### <a name="for-version-22x-use"></a>Version för 2.2.x, Använd:
Du kan behöva tooinstall hello paketet `setuptools` först-- [här](https://pypi.python.org/pypi/setuptools). Kör sedan:

```bash
sudo python setup.py install
```

#### <a name="ensure-auto-update-is-enabled"></a>Kontrollera att automatiska uppdateringar är aktiverat

Kontrollera först toosee om det är aktiverat:

```bash
cat /etc/waagent.conf
```

Hitta 'AutoUpdate.Enabled'. Om du ser den här utdatan aktiveras:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable kör:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="3-restart-hello-waagent-service"></a>3. Starta om hello waagent-tjänsten
För de flesta distributioner för Linux:

```bash
sudo service waagent restart
```

Ubuntu, Använd:

```bash
sudo service walinuxagent restart
```

För virtuell CoreOS, använder du:

```bash
sudo systemctl restart waagent
```

### <a name="4-confirm-hello-azure-linux-agent-version"></a>4. Bekräfta hello Azure Linux-agentens version
    
```bash
waagent -version
```

För virtuell CoreOS fungerar inte hello ovanstående kommando.

Du ser att hello Azure Linux-agentens version har uppdaterats toohello ny version.

Mer information om hello Azure Linux-agenten finns [Azure Linux-agenten viktigt](https://github.com/Azure/WALinuxAgent).
