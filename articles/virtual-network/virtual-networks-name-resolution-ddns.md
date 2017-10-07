---
title: "aaaUsing dynamisk DNS-tooregister värdnamn"
description: "Den här sidan ger information om hur tooset in dynamisk DNS-tooregister värdnamn i DNS-servrarna."
services: dns
documentationcenter: na
author: GarethBradshawMSFT
manager: timlt
editor: 
ms.assetid: c315961a-fa33-45cf-82b9-4551e70d32dd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2017
ms.author: garbrad
ms.openlocfilehash: 8d4b44265714e6976f26bfb3446e8101aa70996a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-dynamic-dns-tooregister-hostnames-in-your-own-dns-server"></a>Med dynamisk DNS-tooregister värdnamn i DNS-servern
[Azure tillhandahåller namnmatchning](virtual-networks-name-resolution-for-vms-and-role-instances.md) för virtuella datorer (VM) och rollinstanser. När din namnmatchning måste utöver de som tillhandahålls av Azure, kan du ange DNS-servrarna. Det här ger du hello power tootailor din DNS-lösningen toosuit dina egna behov. Till exempel behöva tooaccess lokala resurser via Active Directory-domänkontrollant.

När det finns anpassade DNS-servrarna som Azure virtuella datorer som du kan vidarebefordra hostname frågar om hello samma virtuella nätverk tooAzure tooresolve värdnamn. Om du inte vill att toouse vägen måste registrera du VM-värdnamn i DNS-servern med hjälp av dynamisk DNS.  Azure har inte hello möjlighet (t.ex. autentiseringsuppgifter) toodirectly skapa poster i DNS-servrar så att andra åtgärder krävs ofta. Här följer några vanliga scenarier med andra alternativ.

## <a name="windows-clients"></a>Windows-klienter
Icke-domänanslutna Windows-klienter försöker oskyddat dynamisk DNS (DDNS) uppdateringar när de startar eller när IP-adresser ändras. hello DNS-namn är hello hostname plus hello primärt DNS-suffix. Azure låter hello primärt DNS-suffix som är tom, men du kan ange detta i hello VM via hello [UI](https://technet.microsoft.com/library/cc794784.aspx) eller [med hjälp av automation](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).

Domänanslutna Windows-klienter för att registrera sina IP-adresser med hello domänkontrollant med hjälp av säkra dynamiska DNS. hello domänanslutning processen anger hello primärt DNS-suffix på klienten hello och skapar och underhåller hello förtroenderelation.

## <a name="linux-clients"></a>Linux-klienter
Linux-klienter normalt registrera inte sig själva med hello DNS-server på Start, de förutsätter hello DHCP-servern fungerar. Azures DHCP-servrar har inte möjlighet hello eller autentiseringsuppgifter tooregister poster i DNS-servern.  Du kan använda ett verktyg som kallas *nsupdate*, som ingår i hello Bind paketet, toosend dynamiska DNS-uppdateringar. Eftersom hello dynamisk DNS-protokollet är standardiserad, kan du använda *nsupdate* även när du inte använder Bind på hello DNS-server.

Du kan använda hello hook som tillhandahålls av hello DHCP-klient toocreate och underhålla hello värdnamnsposten i hello DNS-server. Under hello DHCP cykel hello klienten kör hello skript i */etc/dhcp/dhclient-exit-hooks.d/*. Detta kan vara används tooregister hello nya IP-adressen med hjälp av *nsupdate*. Exempel:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on hello primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        
        

Du kan också använda hello *nsupdate* kommandot tooperform säkra dynamiska DNS-uppdateringar. När du använder en Bind DNS-server, ett privat-offentligt nyckelpar är exempelvis [genereras](http://linux.yyz.us/nsupdate/).  hello DNS-servern är [konfigurerats](http://linux.yyz.us/dns/ddns-server.html) med hello offentliga del av hello nyckel så att den kan kontrollera hello signaturen på hello-begäran. Du måste använda hello *-k* alternativet tooprovide hello nyckel för*nsupdate* för hello dynamisk DNS-uppdatering begäran toobe signerade.

När du använder en Windows DNS-server kan du använda Kerberos-autentisering med hello *-g* parameter i *nsupdate* (inte tillgängligt i hello Windows-versionen av *nsupdate* ). toodo denna, Använd *kinit* tooload hello autentiseringsuppgifter (t.ex. från en [keytab filen](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)). Sedan *nsupdate -g* ska hämta hello autentiseringsuppgifter från hello cache.

Om det behövs kan du lägga till en DNS-sökning suffix tooyour virtuella datorer. hello DNS-suffix anges i hello */etc/resolv.conf* fil. De flesta distributioner av Linux hantera automatiskt hello innehållet för den här filen, så ofta du kan inte redigera den. Du kan dock åsidosätta hello suffix med hello DHCP-klient *ersätter* kommando. toodo detta i */etc/dhcp/dhclient.conf*, lägga till:

        supersede domain-name <required-dns-suffix>;

