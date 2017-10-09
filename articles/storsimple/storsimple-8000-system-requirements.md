---
title: "Systemkrav för aaaStorSimple 8000-serien | Microsoft Docs"
description: "Beskriver program, nätverk, och hög tillgänglighet krav och bästa praxis för en Microsoft Azure StorSimple-lösning."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/10/2017
ms.author: alkohli
ms.openlocfilehash: f13ccdb7cb317d72c60e9c2fe49937764d10b43e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-software-high-availability-and-networking-requirements"></a>StorSimple 8000-serien programvara, hög tillgänglighet och nätverkskrav

## <a name="overview"></a>Översikt

Välkommen tooMicrosoft Azure StorSimple. Den här artikeln beskriver viktiga krav och bästa praxis för din StorSimple-enhet och hello lagring klienterna kommer åt hello enhet. Vi rekommenderar att du granskar hello informationen noggrant innan du distribuerar din StorSimple-system och sedan refererar tillbaka tooit som behövs under distributionen och efterföljande igen.

hello systemkraven är:

* **Programvarukrav för lagring klienter** -beskriver hello stöds operativsystem och eventuella ytterligare krav för dessa operativsystem.
* **Nätverkskrav för hello StorSimple-enhet** -ger information om hello portar som måste toobe öppen i brandväggen-tooallow för iSCSI-, molnet eller hantering av trafik.
* **Krav för hög tillgänglighet för StorSimple** – beskriver krav för hög tillgänglighet och bästa praxis för din StorSimple-enheten och värden dator.

## <a name="software-requirements-for-storage-clients"></a>Programvarukrav för klienter för lagring

hello följande programvarukrav finns för hello lagring klienter som har åtkomst till din StorSimple-enhet.

| Operativsystem som stöds | Version som krävs | Ytterligare krav/anteckningar |
| --- | --- | --- |
| Windows Server |2008 R2 SP1, 2012, 2012 R2, 2016 |StorSimple iSCSI-volymer stöds för användning på endast hello följande Windows-disktyper:<ul><li>Enkel volym på en grundläggande disk</li><li>Enkel och speglad volym på en dynamisk disk</li></ul>Endast hello programvara iSCSI-initierare finns inbyggt i operativsystemet hello stöds. Maskinvara iSCSI-initierarna stöds inte.<br></br>Windows Server 2012 och 2016 tunn allokering och ODX funktioner stöds om du använder en iSCSI-StorSimple-volym.<br><br>StorSimple skapa tunt allokerade och helt allokerade volymer. Det går inte att skapa delvis allokerade volymer.<br><br>Formatera om en tunt etablerad volym kan det ta lång tid. Vi rekommenderar att hello volymen och sedan skapa en ny i stället för att formatera om. Men om du fortfarande vill tooreformat en volym:<ul><li>Kör följande kommando innan hello formatera om tooavoid utrymme återvinning av fördröjningar hello: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Efter hello formateringen, kommando Använd hello följande toore aktivera utrymmesåtertagning:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Hello Windows Server 2012 snabbkorrigeringen som beskrivs i [KB 2878635](https://support.microsoft.com/kb/2870270) tooyour Windows Server-datorn.</li></ul></li></ul></ul> Om du konfigurerar StorSimple Snapshot Manager eller en StorSimple-kortet för SharePoint går för[programvarukrav för valfria komponenter](#software-requirements-for-optional-components). |
| VMware ESX |5.5 och 6.0 |Stöds med VMware vSphere som iSCSI-klient. VAAI-block stöds med VMware vSphere på StorSimple-enheter. |
| Linux RHEL/CentOS |5, 6 och 7 |Stöd för Linux iSCSI-klienter med öppna iSCSI-initieraren versioner 5, 6 och 7. |
| Linux |SUSE Linux 11 | |

> [!NOTE]
> IBM AIX stöds för närvarande inte med StorSimple.


## <a name="software-requirements-for-optional-components"></a>Programvarukrav för valfria komponenter

hello följande programvarukrav finns för hello StorSimple tillvalskomponenter (StorSimple Snapshot Manager och StorSimple-kortet för SharePoint).

| Komponent | Plattform | Ytterligare krav/anteckningar |
| --- | --- | --- |
| StorSimple Snapshot Manager |Windows Server 2008 R2 SP1, 2012, 2012 R2 |Användning av StorSimple Snapshot Manager på Windows Server krävs för säkerhetskopiering/återställning av speglade dynamiska diskar och programkonsekventa säkerhetskopior.<br> StorSimple Snapshot Manager stöds endast på Windows Server 2008 R2 SP1 (64-bitars), Windows Server 2012 R2 och Windows Server 2012.<ul><li>Om du använder Windows Server 2012 måste du installera .NET 3.5 – 4.5 innan du installerar StorSimple Snapshot Manager.</li><li>Om du använder Windows Server 2008 R2 SP1, måste du installera Windows Management Framework 3.0 innan du installerar StorSimple Snapshot Manager.</li></ul> |
| StorSimple-adapter för SharePoint |Windows Server 2008 R2 SP1, 2012, 2012 R2 |<ul><li>StorSimple-kortet för SharePoint stöds bara på SharePoint 2010 och SharePoint 2013.</li><li>RBS kräver SQL Server Enterprise Edition version 2008 R2 eller 2012.</li></ul> |

## <a name="networking-requirements-for-your-storsimple-device"></a>Nätverkskrav för din StorSimple-enhet

Din StorSimple-enhet är en låst enhet. Portarna måste dock toobe öppnas i din brandvägg tooallow för iSCSI-, moln och hantering av trafik. hello visas följande tabell hello-portar som behövs toobe öppnas i brandväggen. I den här tabellen *i* eller *inkommande* refererar toohello riktning som inkommande klientbegäranden åtkomst till din enhet. *Ut* eller *utgående* refererar toohello riktning din StorSimple-enhet skickar data externt, utöver hello distribution: till exempel utgående toohello Internet.

| Nej. port<sup>1,2</sup> | In eller ut | Port omfång | Krävs | Anteckningar |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP)<sup>3</sup> |Ut |WAN |Nej |<ul><li>Utgående port används för Internet access tooretrieve uppdateringar.</li><li>hello utgående webbproxy kan konfigureras av användaren.</li><li>tooallow systemuppdateringar den här porten måste också öppnas för hello styrenhets-fästa IP-adresser.</li></ul> |
| TCP 443 (HTTPS)<sup>3</sup> |Ut |WAN |Ja |<ul><li>Utgående port används för att komma åt data i hello molnet.</li><li>hello utgående webbproxy kan konfigureras av användaren.</li><li>tooallow systemuppdateringar den här porten måste också öppnas för hello styrenhets-fästa IP-adresser.</li><li>Den här porten används också på båda hello-domänkontrollanter för skräpinsamling.</li></ul> |
| UDP 53 (DNS) |Ut |WAN |I vissa fall. Du hittar information. |Den här porten krävs bara om du använder en Internet-baserad DNS-server. |
| UDP 123 (NTP) |Ut |WAN |I vissa fall. Du hittar information. |Den här porten krävs bara om du använder en Internetbaserad NTP-server. |
| TCP 9354 |Ut |WAN |Ja |hello utgående porten används av hello StorSimple enheten toocommunicate med hello StorSimple enheten Manager-tjänsten. |
| 3260 (iSCSI) |i |LAN |Nej |Den här porten är används tooaccess data via iSCSI. |
| 5985 |i |LAN |Nej |Inkommande porten används av StorSimple Snapshot Manager toocommunicate med hello StorSimple-enhet.<br>Den här porten används också när du fjärransluta tooWindows PowerShell för StorSimple via HTTP. |
| 5986 |i |LAN |Nej |Den här porten används när du fjärransluta tooWindows PowerShell för StorSimple via HTTPS. |

<sup>1</sup> några inkommande portar måste toobe öppnas på hello offentliga Internet.

<sup>2</sup> om flera portar har en gateway-konfiguration, hello utgående routade trafik ordning bestäms utifrån hello port routning ordning som beskrivs i [Port routning](#routing-metric), nedan.

<sup>3</sup> hello styrenhets-fästa IP-adresser på StorSimple-enheten måste vara dirigerbara och kunna tooconnect toohello Internet direkt eller via hello konfigurerade webbproxy. hello fasta IP-adresser används för att underhålla hello uppdateringar toohello enhet. Om hello styrenheter inte kan ansluta toohello Internet via hello statiska IP-adresser kan du inte kan tooupdate din StorSimple-enhet.

> [!IMPORTANT]
> Kontrollera att brandväggen hello inte ändra eller dekryptera alla SSL-trafik mellan hello StorSimple-enhet och Azure.


### <a name="url-patterns-for-firewall-rules"></a>URL-mönster för brandväggsregler

Nätverksadministratörer kan ofta konfigurera avancerad brandvägg regler baserat på hello URL mönster toofilter hello inkommande och utgående trafik hello. Din StorSimple-enhet och hello StorSimple enheten Manager-tjänsten är beroende av andra Microsoft-program, till exempel Azure Service Bus, Azure Active Directory-åtkomstkontroll, storage-konton och Microsoft Update-servrar. hello URL-mönster som associeras med dessa program kan använda tooconfigure brandväggsregler. Det är viktigt toounderstand som hello URL-mönster som associeras med dessa program kan ändra. Detta i sin tur kräver hello nätverket administratören toomonitor och uppdatera brandväggsreglerna för din StorSimple som och vid behov.

Vi rekommenderar att du ställer in brandväggsreglerna för utgående trafik, baserat på StorSimple fasta IP-adresser, liberally i de flesta fall. Du kan dock använda hello informationen under tooset avancerade brandväggsregler som är nödvändiga toocreate säkra miljöer.

> [!NOTE]
> hello-enhet (källa) IP-adresser ska alltid anges tooall hello aktiverade nätverksgränssnitt. hello mål-IP-adresser ska anges för[IP-adressintervall för Azure-datacenter](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


#### <a name="url-patterns-for-azure-portal"></a>URL-mönster för Azure-portalen

| URL-mönster | Komponenten/funktioner | IP-adresser för enhet |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`<br>`https://login.windows.net` |StorSimple enheten Manager-tjänsten<br>Access Control Service<br>Azure Service Bus<br>Autentiseringstjänst |Moln-aktiverat nätverksgränssnitt |
| `https://*.backup.windowsazure.com` |Enhetsregistrering |DATA 0 |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Återkallade certifikat |Moln-aktiverat nätverksgränssnitt |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure storage-konton och övervakning |Moln-aktiverat nätverksgränssnitt |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Microsoft Update-servrar<br> |Styrenhets-fästa IP-adresser endast |
| `http://*.deploy.akamaitechnologies.com` |CDN med Akamai |Styrenhets-fästa IP-adresser endast |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Support-paket |Moln-aktiverat nätverksgränssnitt |

#### <a name="url-patterns-for-azure-government-portal"></a>URL-mönster för Azure Government-portalen

| URL-mönster | Komponenten/funktioner | IP-adresser för enhet |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`<br>`https://login-us.microsoftonline.com` |StorSimple enheten Manager-tjänsten<br>Access Control Service<br>Azure Service Bus<br>Autentiseringstjänst |Moln-aktiverat nätverksgränssnitt |
| `https://*.backup.windowsazure.us` |Enhetsregistrering |DATA 0 |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Återkallade certifikat |Moln-aktiverat nätverksgränssnitt |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure storage-konton och övervakning |Moln-aktiverat nätverksgränssnitt |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Microsoft Update-servrar<br> |Styrenhets-fästa IP-adresser endast |
| `http://*.deploy.akamaitechnologies.com` |CDN med Akamai |Styrenhets-fästa IP-adresser endast |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Support-paket |Moln-aktiverat nätverksgränssnitt |

### <a name="routing-metric"></a>Routning mått

Routning mått är associerad med hello gränssnitt och hello-gateway som dirigerar hello data toohello angivna nätverk. Routning måttet används av hello routing protocol toocalculate hello bästa sökvägen tooa givet mål, om den lär sig flera sökvägar finns toohello samma mål. hello lägre hello routning mått, hello högre hello prioritet.

I hello kontexten för StorSimple, om flera nätverksgränssnitt och gateway är konfigurerad toochannel trafik, hello routning mått träda i play toodetermine hello relativa ordning i vilken hello gränssnitt får användas. hello routning mått ändras inte av hello användare. Du kan dock använda hello `Get-HcsRoutingTable` cmdlet tooprint ut hello-routningstabell (och mått) på din StorSimple-enhet. Mer information om cmdlet Get-HcsRoutingTable i [felsökning StorSimple distribution](storsimple-troubleshoot-deployment.md).

hello routning mått-algoritm som används för uppdatering 2 och senare versioner kan beskrivas på följande sätt.

* En uppsättning förinställda värden har tilldelats toonetwork gränssnitt.
* Överväg att en exempeltabell nedan med värden som har tilldelats toohello olika nätverksgränssnitt när de moln-aktiverat eller moln-inaktiverad, men med en konfigurerad gateway. Obs värdena hello angetts endast exempelvärden.

    | Nätverksgränssnitt | Moln aktiverat- | Moln-inaktiverad med gateway |
    |-----|---------------|---------------------------|
    | Data 0  | 1            | -                        |
    | Data 1  | 2            | 20                       |
    | Data 2  | 3            | 30                       |
    | Data 3  | 4            | 40                       |
    | Data 4  | 5            | 50                       |
    | Data 5  | 6            | 60                       |


* Hej är där hello molnet trafik dirigeras via hello nätverksgränssnitt:
  
    *Data 0 > Data 1 > datum 2 > Data 3 > Data 4 > Data 5*
  
    Detta kan förklaras med hello följande exempel.
  
    Överväg att en StorSimple-enhet med två moln-aktiverat nätverksgränssnitt, Data 0 och 5 Data. Data 1 till 4 Data är moln-inaktiverad men har en konfigurerad gateway. hello ordning i vilken trafik vidarebefordras för den här enheten kommer att:
  
    *Data 0 (1) > Data 5 (6) > Data 1 (20) > Data 2 (30) > Data 3 (40) > Data 4 (50)*
  
    *hello nummer inom parentes visar hello respektive Routning mått.*
  
    Om Data 0 misslyckas hämta hello molntrafiken dirigeras till Data 5. Med hänsyn till att en gateway har konfigurerats på alla andra nätverk, om både Data 0 och Data 5 toofail, går hello molnet trafik igenom Data 1.
* Om ett moln-aktiverat nätverksgränssnitt misslyckas, kan du 3 försök med en 30 andra fördröjning tooconnect toohello gränssnitt. Om alla hello försök misslyckas är hello trafik routade toohello nästa tillgängliga moln-aktiverat gränssnitt utifrån hello-routningstabell. Om alla hello moln-aktiverat nätverksgränssnitt misslyckas kommer hello enheten växlar över toohello andra styrenheten (ingen omstart i detta fall).
* Om det finns ett VIP-fel för en iSCSI-aktiverade nätverksgränssnitt måste finnas det 3 försök med en fördröjning på 2 sekunder. Det här beteendet har svaret inte hello samma från hello tidigare versioner. Om alla hello iSCSI nätverksgränssnitt misslyckas, kommer en domänkontrollant växling vid fel inträffa (åtföljs av en omstart).
* En avisering utlöses också på din StorSimple-enhet när det finns en VIP-fel. Mer information finns för[Varna Snabbreferens](storsimple-8000-manage-alerts.md).
* Vad gäller omförsök företräde iSCSI framför molnet.
  
    Överväg att hello följande exempel: en StorSimple-enhet har två nätverksgränssnitt som är aktiverad, Data 0 och 1 för Data. Data 0 är moln-aktiverat Data 1 är både molntjänster och iSCSI-aktiverade. Inga andra nätverksgränssnitt på den här enheten har aktiverats för molntjänster eller iSCSI.
  
    Om Data 1 misslyckas, anges det hello senaste iSCSI nätverksgränssnittet resulterar detta i en domänkontrollant redundans tooData 1 på hello andra styrenheten.

### <a name="networking-best-practices"></a>Metodtips för nätverk

Dessutom uppfylla toohello ovan nätverkskraven för hello optimala prestanda av din StorSimple-lösning du toohello följande metodtips:

* Kontrollera att din StorSimple-enhet har en dedikerad 40 Mbit/s bandbredd (eller mer) vid alla tidpunkter. Den här bandbredden inte ska delas (eller allokering bör garanteras genom hello användning av QoS-principer) med andra program.
* Se till att network connectivity toohello Internet är tillgänglig vid alla tidpunkter. Sporadiska eller otillförlitliga Internet-anslutningar toohello enheter, inklusive utan Internet-anslutning som helst, resulterar i en konfiguration som inte stöds.
* Isolera hello iSCSI och molntrafik genom att ha dedikerade nätverksgränssnitt på enheten för iSCSI- och åtkomst. Mer information finns i hur för[ändra nätverksgränssnitt](storsimple-8000-modify-device-config.md#modify-network-interfaces) på StorSimple-enheten.
* Använd inte en Link Aggregation Control Protocol (LACP) konfiguration för nätverksgränssnitten. Det här är en konfiguration som inte stöds.

## <a name="high-availability-requirements-for-storsimple"></a>Krav för hög tillgänglighet för StorSimple

hello maskinvaruplattform som ingår i hello StorSimple-lösningen har funktioner för tillgänglighet och tillförlitlighet som utgör en grund för hög tillgänglighet, feltolerant lagringsinfrastruktur i ditt datacenter. Men det finns krav och bästa praxis som du bör följa toohelp garantera hello i StorSimple-lösningen. Innan du distribuerar StorSimple granska noggrant hello följande krav och bästa praxis för hello StorSimple-enhet och anslutna värddatorer.

Mer information om övervakning och underhåll hello maskinvarukomponenter för din StorSimple-enhet går för[använda hello StorSimple Enhetshanteraren service toomonitor maskinvarukomponenter och status](storsimple-8000-monitor-hardware-status.md) och [ Ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Krav för hög tillgänglighet och procedurer för din StorSimple-enhet

Granska hello följande information noggrant tooensure hello hög tillgänglighet för din StorSimple-enhet.

#### <a name="pcms"></a>PCMs

StorSimple-enheter är redundant kan bytas ström och kylning moduler (PCMs). Varje PCM har tillräckligt med kapacitet tooprovide service för hello hela chassi. tooensure hög tillgänglighet, både PCMs måste vara installerad.

* Anslut din PCMs toodifferent power källor tooprovide tillgänglighet om ett eluttag misslyckas.
* Om en PCM misslyckas kan du begära en ersättning omedelbart.
* Ta bort en misslyckad PCM bara när du har hello ersättning och är redo tooinstall den.
* Ta inte bort båda PCMs samtidigt. hello PCM modulen innehåller hello säkerhetskopiering batterimodulen. Ta bort både av hello PCMs resulterar i en avstängning utan batteri skydd och hello enhetstillstånd sparas inte. Mer information om hello batteri gå för[Underhåll hello säkerhetskopiering batterimodulen](storsimple-8000-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Domänkontrollanten moduler

StorSimple-enheter är redundant kan bytas controller moduler. hello controller moduler fungerar i ett aktivt-passivt. Vid en given tidpunkt en modul som domänkontrollanten är aktiv och betjänar, medan hello modulen andra domänkontrollanter är passiv. hello passiva controller modulen är påslagen och aktiveras om hello aktiva styrenheten modulen misslyckas eller tas bort. Varje domänkontrollant-modul har tillräckligt med kapacitet tooprovide service för hello hela chassi. Båda controller moduler måste vara installerade tooensure hög tillgänglighet.

* Kontrollera att båda controller moduler installeras alltid.
* Om en domänkontrollant modul inte begära en ersättning omedelbart.
* Ta bort en felaktig Styrenhetsmodul när du har hello ersättning och är redo tooinstall den. Ta bort en modul under långa perioder påverkar hello luftflödet och därför hello kylningen av hello system.
* Kontrollera att hello nätverket anslutningar tooboth domänkontrollant moduler är identiska och hello anslutna nätverksgränssnitt har en identisk nätverkskonfigurationen.
* Om en domänkontrollant modul misslyckas eller behöver bytas ut, se till att hello andra domänkontrollanter modulen är aktiv innan du ersätter hello felaktig styrenhet modulen. tooverify att en domänkontrollant är aktiv, gå för[identifiera hello aktiva styrenheten på enheten](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Ta inte bort båda controller moduler i hello samtidigt. Om en domänkontrollant växling pågår inte stänga hello vänteläge controller modulen eller ta bort den från hello chassi.
* Vänta minst fem minuter innan du tar bort antingen domänkontrollant modulen efter en domänkontrollant växling.

#### <a name="network-interfaces"></a>Nätverksgränssnitt

StorSimple-styrenhet moduler varje har fyra 1 Gigabit och två 10 Gigabit Ethernet-nätverkskort.

* Kontrollera att hello nätverket anslutningar tooboth domänkontrollant moduler är identiska och hello nätverksgränssnitt att hello controller modulgränssnitt är anslutna toohave en identisk nätverkskonfigurationen.
* När det är möjligt ska du distribuera nätverksanslutningar i olika växlar tooensure tjänsttillgänglighet hello enheten nätverksfel för händelsen.
* När du kopplar från hello endast eller hello sista återstående iSCSI-gränssnittet (med IP-adresser tilldelas), inaktivera hello gränssnittet först och sedan koppla från hello-kablar. Om hello-gränssnittet är inte ansluten först, att sedan detta orsaka hello aktiva styrenheten toofail över toohello passiva domänkontrollant. Om hello passiva domänkontrollant har även dess motsvarande gränssnitt kopplas från, kommer båda hello domänkontrollanter startas om flera gånger innan reglera på en domänkontrollant.
* Ansluta minst två gränssnitt toohello datanätverk från varje domänkontrollant modul.
* Om du har aktiverat hello två 10 GbE-gränssnitt, distribuera dessa över olika växlar.
* När det är möjligt använda MPIO på servrar tooensure att hello servrar kan tolerera länken, nätverk och fel.

Mer information om nätverk enheten för hög tillgänglighet och prestanda finns för[installerar StorSimple 8100-enhet](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) eller [installera din StorSimple-8600-enhet](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD och hårddiskar

StorSimple-enheter med transistoriserade diskar (SSD) och hårddiskar (hårddiskar) som skyddas med hjälp av speglade lagringsutrymmen. Användning av speglingsutrymmen garanterar hello enheten kan tootolerate hello fel på en eller flera SSD eller hårddiskar.

* Kontrollera att alla SSD och HDD-moduler är installerade.
* Om en SSD och HDD misslyckas kan begära en ersättning omedelbart.
* Om en SSD och HDD misslyckas eller behöver bytas ut, se till att du tar bort endast hello SSD och HDD som behöver bytas ut.
* Ta inte bort mer än en SSD och HDD från hello system när som helst i tid.
  Ett fel på 2 eller flera diskar för en viss typ (Hårddisk, SSD) eller på varandra följande fel inom en kort tidsperiod kan resultera i system-fel och potentiella dataförluster. Om detta inträffar [kontaktar Microsoft Support](storsimple-8000-contact-microsoft-support.md) om du behöver hjälp.
* Under ersättning, övervaka hello **delade komponenter** i hello **maskinvara hälsa** bladet för hello enheter i hello SSD och hårddiskar. Grön bock status anger att hello diskar är felfri eller OK, medan ett rött utropstecken peka anger en misslyckad SSD eller Hårddisk.
* Vi rekommenderar att du konfigurerar molnögonblicksbilder för alla volymer som du behöver tooprotect vid ett systemfel.

#### <a name="ebod-enclosure"></a>EBOD hölje

StorSimple enhetsmodell 8600 innehåller en utökad Bunch av diskar (EBOD) höljet i tillägg toohello primära hölje. En EBOD innehåller EBOD domänkontrollanterna och hårddiskar (hårddiskar) som skyddas med hjälp av speglade lagringsutrymmen. Använda speglingsutrymmen garanterar att hello-enheten är kan tootolerate hello fel på en eller flera hårddiskar. Hej EBOD hölje är anslutna toohello primära höljet via redundanta SAS-kablar.

* Se till att båda EBOD hölje controller moduler installeras både SAS-kablar och alla hello hårddiskar vid alla tidpunkter.
* Om en EBOD hölje controller modul inte begära en ersättning omedelbart.
* Om en EBOD hölje controller modul misslyckas, se till att hello modulen andra domänkontrollanter är aktiv innan du ersätter hello misslyckade modulen. tooverify att en domänkontrollant är aktiv, gå för[identifiera hello aktiva styrenheten på enheten](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Under en EBOD styrenhet modulen ersättning kontinuerligt övervaka hello status för hello-komponenten i hello StorSimple enheten Manager-tjänsten genom att öppna **övervakaren** > **maskinvara hälsa** .
* Om en SAS-misslyckas eller behöver bytas ut (Microsoft-supporten ska vara ingår toomake sådant fastställande), se till att du tar bort endast hello SAS kabel som behöver bytas ut.
* Ta inte samtidigt bort båda SAS-kablar från hello system när som helst i tid.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Rekommendationer för hög tillgänglighet för din värddatorer

Granska noggrant dessa bästa praxis tooensure hello hög tillgänglighet av värdar anslutna tooyour StorSimple-enhet.

* Konfigurera StorSimple med [tvånods-filen server-klusterkonfigurationer][1]. Tar bort enskilda felpunkter på felet och bygga i redundans på hello värddatorns sida, blir hello hela lösningen hög tillgänglighet.
* Använda ständigt tillgängliga (CA) resurser som är tillgängliga med Windows Server 2012 (SMB 3.0) för hög tillgänglighet under växling vid fel i hello lagringsstyrenheter. För ytterligare information för att konfigurera filserverkluster och ständigt tillgängliga resurser med Windows Server 2012, se toothis [videodemonstration](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Nästa steg

* [Lär dig mer om StorSimple system gränser](storsimple-8000-limits.md).
* [Lär dig hur toodeploy StorSimple-lösningen](storsimple-8000-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
