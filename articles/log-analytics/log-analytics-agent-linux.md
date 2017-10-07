---
title: aaaConnect ditt Linux-datorer tooOperations Management Suite (OMS) | Microsoft Docs
description: "Den här artikeln beskriver hur tooconnect Linux-datorer finns i Azure andra molntjänster eller lokala tooOMS med hello OMS-Agent för Linux."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: magoedte
ms.openlocfilehash: cb4fc671d0678f9fadc689c6ba7d719213aa61b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-linux-computers-toooperations-management-suite-oms"></a>Ansluta din Linux-datorer tooOperations Management Suite (OMS) 

Med Microsoft Operations Management Suite (OMS), som du kan samla in och arbeta med data som genereras från Linux-datorer och lösningar för behållare som Docker, som finns i ditt lokala datacenter som fysiska servrar eller virtuella datorer, virtuella datorer i en molnbaserad tjänst som Amazon Web Services (AWS) eller Microsoft Azure. Du kan också använda hanteringslösningar som är tillgängliga i OMS, till exempel ändringsspårning, tooidentify konfigurationsändringar och uppdateringshantering toomanage programvara uppdateringar tooproactively hantera hello livscykeln för din virtuella Linux-datorer. 

hello OMS-Agent för Linux kommunicerar utgående med hello OMS-tjänsten via TCP-port 443, och om hello datorn ansluter tooa brandvägg eller proxyserver server toocommunicate via hello Internet, granska [konfigurera hello agent för användning med en HTTP-proxy servern eller Gateway OMS](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) toounderstand vilka konfigurationsändringar behöver toobe tillämpas.  Om du övervakar hello-dator med System Center 2016 - Operations Manager eller Operations Manager 2012 R2 kan vara multi-homed med hello OMS-tjänsten toocollect data och vidarebefordra toohello service och fortfarande övervakas av Operations Manager.  Linux-datorer som övervakas av en Operations Manager-hanteringsgrupp som är integrerad med OMS får inte konfigurationen för datakällor och vidarebefordra insamlade data via hello hanteringsgruppen.  hello OMS-agenten kan inte vara konfigurerade tooreport toomore än en arbetsyta.  

Om din IT-säkerhetsprinciper inte tillåter datorer på ditt nätverk tooconnect toohello Internet, kan hello agent vara konfigurerade tooconnect toohello OMS Gateway tooreceive konfigurationsinformation och skicka insamlade data beroende på hello-lösning som du har aktiverad. Mer information och anvisningar om hur tooconfigure din OMS Linux-agenten toocommunicate via en OMS-Gateway toohello OMS-tjänst, se [ansluta datorer tooOMS med hello OMS Gateway](log-analytics-oms-gateway.md).  

hello visar följande diagram hello anslutningen mellan hello hanteras med agent Linux-datorer och OMS, inklusive hello riktning och portar.

![direkt agentkommunikation med OMS-diagram](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a>Systemkrav
Granska hello följande information tooverify hello förutsättningar vara uppfyllda innan du börjar.

### <a name="supported-linux-operating-systems"></a>Linux-operativsystem som stöds
hello följande Linux-distributioner stöds officiellt.  Hello OMS-Agent för Linux kan också köra på andra distributioner som inte finns.

* Amazon Linux 2012.09 too2015.09 (x86/x64)
* CentOS Linux 5, 6 och 7 (x86/x64)
* Oracle Linux 5, 6 och 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 och 7 (x86/x64)
* Debian GNU/Linux 6, 7 och 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 och 12 (x86/x64)

### <a name="network"></a>Nätverk
hello informationen under listan hello proxy och brandväggen konfigurationsinformation som krävs för hello Linux-agenten toocommunicate med OMS. Trafiken är utgående från din nätverkstjänst toohello OMS. 

|Agentresurs| Portar |  
|------|---------|  
|*.ods.opinsights.azure.com | Port 443|   
|*.oms.opinsights.azure.com | Port 443|   
|*.BLOB.Core.Windows.NET/ | Port 443|   
|*.azure-automation.net | Port 443|  

### <a name="package-requirements"></a>Krav för paketet

 **Nödvändigt paket**   | **Beskrivning**   | **Lägsta version**
--------------------- | --------------------- | -------------------
Glibc | GNU C-bibliotek   | 2.5-12 
Openssl | OpenSSL-bibliotek | 0.9.8e eller 1.0
CURL | cURL Webbklient | 7.15.5
Python-ctypes | | 
PAM | Lösenordssynkronisering moduler | 

> [!NOTE]
>  Rsyslog eller syslog-ng är obligatoriska toocollect syslog-meddelanden. hello standard syslog-daemon på version 5 Red Hat Enterprise Linux, CentOS och Oracle Linux-version (sysklog) stöds inte för insamling av syslog-händelser. toocollect syslog-data från den här versionen av dessa distributioner hello rsyslog daemon ska vara installerat och konfigurerat tooreplace sysklog 

hello agent innehåller flera paket. hello versionen filen innehåller hello följande paket, som körs hello shell-paket med `--extract`:

**Paketet** | **Version** | **Beskrivning**
----------- | ----------- | --------------
omsagent | 1.4.0 | hello Operations Management Suite-Agent för Linux
omsconfig | 1.1.1 | Konfiguration av agenten för hello OMS-Agent
omi | 1.2.0 | Öppna Management Infrastructure (OMI) - lightweight CIM-servern
scx | 1.6.3 | OMI CIM-Providers för operativsystemet prestandamått
Apache cimprov | 1.0.1 | Apache HTTP-Server prestandaövervakning provider för OMI. Installeras om Apache HTTP-Server har identifierats.
MySQL-cimprov | 1.0.1 | MySQL-Server prestandaövervakning provider för OMI. Installeras om MySQL/MariaDB server har identifierats.
docker-cimprov | 1.0.0 | Docker-provider för OMI. Installeras om Docker har identifierats.

### <a name="compatibility-with-system-center-operations-manager"></a>Kompatibilitet med System Center Operations Manager
hello OMS-Agent för Linux delar agent binärfiler med hello System Center Operations Manager-agenten. Om du installerar hello OMS-Agent för Linux på ett system som för närvarande hanteras av Operations Manager hello OMI och SCX paket på hello datorn tooa nyare version. I den här versionen, hello OMS och System Center 2016 - är Operations Manager/Operations Manager 2012 R2-agenter för Linux kompatibla. 

> [!NOTE]
> System Center 2012 SP1 och tidigare versioner är för närvarande inte kompatibelt och stöds med hello OMS-Agent för Linux.<br>
> Om hello OMS-Agent för Linux är installerade tooa dator som för närvarande inte övervakas av Operations Manager, och du sedan toomonitor hello datorn med Operations Manager, måste du ändra hello [OMI configuration](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) tidigare toodiscovering hello-dator. **Det här steget är *inte* behövs om hello Operations Manager-agenten är installerad före hello OMS-Agent för Linux.**

### <a name="system-configuration-changes"></a>Ändringar i systemkonfigurationen
När du har installerat hello OMS-Agent för Linux-paket tillämpas hello följande ytterligare systemomfattande konfigurationsändringar. Dessa artefakter tas bort när hello omsagent paketet är avinstallerat.

* En icke-privilegierade användare med namnet: `omsagent` skapas. Detta är hello konto hello omsagent daemon körs som.
* En sudoers ”innehåller” filen skapas vid /etc/sudoers.d/omsagent. Detta ger rätt omsagent toorestart hello syslog och omsagent daemon. Om sudo ”” Includes inte stöds i hello installerade versionen av sudo, skrivs dessa poster för/etc/sudoers.
* hello syslog-konfigurationen är ändrade tooforward en delmängd av händelser toohello agent. Mer information finns i hello **konfigurera datainsamling** nedan

### <a name="upgrade-from-a-previous-release"></a>Uppgradera från en tidigare version
Uppgradera från versioner tidigare än 1.0.0-47 stöds i den här versionen. Utföra hello installation med hello `--upgrade` kommandot uppgraderar alla komponenter i hello agent toohello senaste versionen.

## <a name="installing-hello-agent"></a>Installera hello-agent

Det här avsnittet beskrivs hur tooinstall hello OMS-Agent för Linux med en bunndle som innehåller Debian och RPM-paket för varje hello agentkomponenter.  Den kan installeras direkt eller extraherade tooretrieve hello enskilda paket.  

Du måste först ditt OMS arbetsyte-ID och nyckel som du hittar genom att växla toohello [klassiska OMS-portalen](https://mms.microsoft.com).  På hello **översikt** sidan hello huvudmenyn väljer **inställningar**, och sedan gå för**anslutna Sources\Linux servrar**.  Du ser hello värdet toohello höger om **arbetsyte-ID** och **primärnyckel**.  Kopiera och klistra in både i din favorit-redigeraren.    

1. Hämta senaste hello [OMS-Agent för Linux (x64)](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) eller [OMS-Agent för Linux x86](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) från GitHub.  
2. Överför hello lämpliga paket (x86 eller x64) tooyour Linux-dator med tjänstanslutningspunkten/sftp.
3. Installera hello paket med hjälp av hello `--install` eller `--upgrade` argumentet. 

    > [!NOTE]
    > Om alla befintliga paket installeras, till exempel när hello System Center Operations Manager-agent för Linux redan är installerad, använda hello `--upgrade` argumentet. tooconnect tooOperations Management Suite under installationen, ange hello `-w <WorkspaceID>` och `-s <Shared Key>` parametrar.


#### <a name="tooinstall-and-onboard-directly"></a>tooinstall och publicera direkt
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="tooupgrade-hello-agent-package"></a>tooupgrade hello-agenten
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade
```

#### <a name="tooinstall-and-onboard-tooa-workspace-in-us-government-cloud"></a>tooinstall och publicera tooa arbetsyta i US Government-moln
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

## <a name="configuring-hello-agent-for-use-with-an-http-proxy-server-or-oms-gateway"></a>Konfigurera hello agenten för användning med en HTTP-proxyserver eller OMS-Gateway
hello OMS-Agent för Linux stöder kommunikation via en proxyserver för HTTP eller HTTPS eller OMS Gateway toohello OMS-tjänsten.  Både anonyma och grundläggande autentisering (användarnamn/lösenord) stöds.  

### <a name="proxy-configuration"></a>Proxykonfiguration
Konfigurationsvärdet för hello-proxy har hello följande syntax:

`[protocol://][user:password@]proxyhost[:port]`

Egenskap|Beskrivning
-|-
Protokoll|HTTP eller https
Användaren|Valfritt användarnamn för proxyautentisering
lösenord|Lösenord för proxyautentisering
proxyhost|Adress eller FQDN för hello proxy server/OMS Gateway
port|Valfria portnummer för hello proxy server/OMS Gateway

Exempel: `http://user01:password@proxy01.contoso.com:8080`

hello-proxyserver kan anges under installationen eller genom att ändra hello proxy.conf-konfigurationsfilen efter installationen.   

### <a name="specify-proxy-configuration-during-installation"></a>Ange proxykonfiguration under installationen
Hej `-p` eller `--proxy` argument för hello omsagent installation paket anger hello proxy configuration toouse. 

```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -p http://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-hello-proxy-configuration-in-a-file"></a>Definiera hello proxykonfiguration i en fil
hello proxykonfiguration kan anges i hello filer `/etc/opt/microsoft/omsagent/proxy.conf` och `/etc/opt/microsoft/omsagent/conf/proxy.conf `. kan skapas eller redigeras hello filer direkt, men deras behörigheter måste vara uppdaterade toogrant hello omiuser användare läsbehörighet på hello-filer. Exempel:
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:8080"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf  
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-hello-proxy-configuration"></a>Ta bort hello proxykonfiguration
tooremove en tidigare definierad proxykonfigurationen och återställa toodirect anslutning, ta bort hello proxy.conf fil:
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="onboarding-with-operations-management-suite"></a>Onboarding med Operations Management Suite
Om ett arbetsyte-ID och nyckel inte angavs under installationen av hello paket, registreras hello agent senare med Operations Management Suite.

### <a name="onboarding-using-hello-command-line"></a>Onboarding hello kommandoraden
Kör hello omsadmin.sh kommando tillhandahåller hello arbetsyte-id och nyckel för din arbetsyta. Det här kommandot måste köras som rot (med sudo-höjning):
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="onboarding-using-a-file"></a>Onboarding med hjälp av en fil
1.  Skapa hello fil `/etc/omsagent-onboard.conf`. hello-filen måste vara Läs- och skrivbara för roten.
`sudo vi /etc/omsagent-onboard.conf`
2.  Infoga följande rader i hello-filen med ditt arbetsyte-ID och delad nyckel hello:

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  
   
3.  Kör följande kommando tooOnboard tooOMS hello:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh`
4.  hello filen tas bort på lyckad onboarding.

## <a name="enable-hello-oms-agent-for-linux-tooreport-toosystem-center-operations-manager"></a>Aktivera hello OMS-Agent för Linux tooreport tooSystem Center Operations Manager
Utföra hello följande steg tooconfigure hello OMS-Agent för Linux tooreport tooa System Center Operations Manager-hanteringsgruppen.  

1. Redigera hello-filen`/etc/opt/omi/conf/omiserver.conf`
2. Se till att hello rad som börjar med **httpsport =** definierar hello port 1270. Exempelvis:`httpsport=1270`
3. Starta om hello OMI-servern:`sudo /opt/omi/bin/service_control restart`

## <a name="agent-logs"></a>Agenten loggar
Hej loggar för hello OMS-Agent för Linux finns på: `/var/opt/microsoft/omsagent/<workspace id>/log/` hello loggar för hello omsconfig (agentkonfiguration) programmet finns på: `/var/opt/microsoft/omsconfig/log/` loggar för hello OMI och SCX-komponenter (som tillhandahåller mått prestandadata) finns på:`/var/opt/omi/log/ and /var/opt/microsoft/scx/log`

### <a name="log-rotation-configuration"></a>Loggen rotation konfiguration ##
hello rotera konfigurationen av loggen för omsagent finns på:`/etc/logrotate.d/omsagent-<workspace id>`

hello standardinställningarna är: 
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-hello-oms-agent-for-linux"></a>Avinstallera hello OMS-Agent för Linux
hello-agentpaketen avinstalleras med körs hello .sh samlingsfilen med hello `--purge` argument som helt tar bort hello agent och dess konfiguration från hello-dator.   

```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

## <a name="troubleshooting"></a>Felsökning

### <a name="issue-unable-tooconnect-through-proxy-toooms"></a>Problem: Det gick inte tooconnect via proxy tooOMS

#### <a name="probable-causes"></a>Troliga orsaker
* hello-proxyn som anges under onboarding var felaktigt
* hello OMS slutpunkter är inte whitelistested i ditt datacenter 

#### <a name="resolutions"></a>Lösningar
1. Reonboard toohello OMS-tjänsten med hello OMS-Agent för Linux med hjälp av följande kommando med hello alternativet hello `-v` aktiverat. Detta gör att utförliga utdata av hello-agenten ansluter via hello proxy toohello OMS-tjänsten. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Granska hello avsnittet [konfigurera hello agent för användning med en HTTP-proxy server(#configuring the-agent-for-use-with-a-http-proxy-server) tooverify som du har konfigurerat korrekt hello agent toocommunicate via en proxyserver.    
* Kontrollera hello följande OMS-tjänstens slutpunkter är godkända:

    |Agentresurs| Portar |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Port 443|   
    |*.oms.opinsights.azure.com | Port 443|   
    |ods.systemcenteradvisor.com | Port 443|   
    |*.BLOB.Core.Windows.NET/ | Port 443|   

### <a name="issue-you-receive-a-403-error-when-trying-tooonboard"></a>Problem: Du får ett 403-fel vid försök tooonboard

#### <a name="probable-causes"></a>Troliga orsaker
* Datum och tid är felaktiga på Linux-Server 
* Arbetsyte-ID och Arbetsytenyckel som används är inte korrekt

#### <a name="resolution"></a>Lösning

1. Kontrollera hello tid på Linux-servern med hello kommandot datum. Om hello tid +/-15 minuter från aktuell tid, misslyckas onboarding. toocorrect den här uppdateringen hello datum och/eller tidszonen för Linux-servern. 
2. Kontrollera att du har installerat hello senaste versionen av hello OMS-Agent för Linux.  hello senaste versionen nu meddelar dig om tid skeva orsakar hello onboarding fel.
3. Reonboard med rätt arbetsyte-ID och Arbetsytenyckel följande hello Installationsinstruktioner tidigare i det här avsnittet.

### <a name="issue-you-see-a-500-and-404-error-in-hello-log-file-right-after-onboarding"></a>Problem: Du ser ett 500 och 404-fel i loggfilen för hello direkt efter onboarding
Detta är ett känt problem som uppstår på första uppladdning av Linux-data i en OMS-arbetsyta. Detta påverkar inte data som skickas eller tjänsten erfarenhet.

### <a name="issue--you-are-not-seeing-any-data-in-hello-oms-portal"></a>Problem: Du inte ser några data i hello OMS-portalen

#### <a name="probable-causes"></a>Troliga orsaker

- Onboarding toohello OMS-tjänsten misslyckades
- Anslutningen toohello OMS-tjänsten är blockerad
- OMS-Agent för Linux data säkerhetskopieras

#### <a name="resolutions"></a>Lösningar
1. Kontrollera om onboarding hello OMS-tjänsten lyckades genom att kontrollera om hello följande fil:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reonboard med hello `omsadmin.sh` kommandoradsinstruktioner
3. Om du använder en proxy finns toohello proxy Lösningssteg som tidigare.
4. I vissa fall när hello OMS-Agent för Linux inte kan kommunicera med hello OMS-tjänsten är data på hello agent köade toohello fullständig buffertstorleken, vilket är 50 MB. hello OMS-Agent för Linux ska startas om genom att köra följande kommando hello: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Det här problemet har lösts i agenten version 1.1.0-28 och senare.
> 