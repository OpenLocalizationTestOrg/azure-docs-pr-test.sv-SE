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
# <a name="using-dynamic-dns-tooregister-hostnames-in-your-own-dns-server"></a><span data-ttu-id="cefbf-103">Med dynamisk DNS-tooregister värdnamn i DNS-servern</span><span class="sxs-lookup"><span data-stu-id="cefbf-103">Using Dynamic DNS tooregister hostnames in your own DNS server</span></span>
<span data-ttu-id="cefbf-104">[Azure tillhandahåller namnmatchning](virtual-networks-name-resolution-for-vms-and-role-instances.md) för virtuella datorer (VM) och rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="cefbf-104">[Azure provides name resolution](virtual-networks-name-resolution-for-vms-and-role-instances.md) for virtual machines (VMs) and role instances.</span></span> <span data-ttu-id="cefbf-105">När din namnmatchning måste utöver de som tillhandahålls av Azure, kan du ange DNS-servrarna.</span><span class="sxs-lookup"><span data-stu-id="cefbf-105">However, when your name resolution needs go beyond those provided by Azure, you can provide your own DNS servers.</span></span> <span data-ttu-id="cefbf-106">Det här ger du hello power tootailor din DNS-lösningen toosuit dina egna behov.</span><span class="sxs-lookup"><span data-stu-id="cefbf-106">This gives you hello power tootailor your DNS solution toosuit your own specific needs.</span></span> <span data-ttu-id="cefbf-107">Till exempel behöva tooaccess lokala resurser via Active Directory-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="cefbf-107">For example, you may need tooaccess on-premises resources via your Active Directory domain controller.</span></span>

<span data-ttu-id="cefbf-108">När det finns anpassade DNS-servrarna som Azure virtuella datorer som du kan vidarebefordra hostname frågar om hello samma virtuella nätverk tooAzure tooresolve värdnamn.</span><span class="sxs-lookup"><span data-stu-id="cefbf-108">When your custom DNS servers are hosted as Azure VMs you can forward hostname queries for hello same vnet tooAzure tooresolve hostnames.</span></span> <span data-ttu-id="cefbf-109">Om du inte vill att toouse vägen måste registrera du VM-värdnamn i DNS-servern med hjälp av dynamisk DNS.</span><span class="sxs-lookup"><span data-stu-id="cefbf-109">If you do not wish toouse this route, you can register your VM hostnames in your DNS server using Dynamic DNS.</span></span>  <span data-ttu-id="cefbf-110">Azure har inte hello möjlighet (t.ex. autentiseringsuppgifter) toodirectly skapa poster i DNS-servrar så att andra åtgärder krävs ofta.</span><span class="sxs-lookup"><span data-stu-id="cefbf-110">Azure doesn't have hello ability (e.g. credentials) toodirectly create records in your DNS servers, so alternative arrangements are often needed.</span></span> <span data-ttu-id="cefbf-111">Här följer några vanliga scenarier med andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="cefbf-111">Here are some common scenarios with alternatives.</span></span>

## <a name="windows-clients"></a><span data-ttu-id="cefbf-112">Windows-klienter</span><span class="sxs-lookup"><span data-stu-id="cefbf-112">Windows clients</span></span>
<span data-ttu-id="cefbf-113">Icke-domänanslutna Windows-klienter försöker oskyddat dynamisk DNS (DDNS) uppdateringar när de startar eller när IP-adresser ändras.</span><span class="sxs-lookup"><span data-stu-id="cefbf-113">Non-domain-joined Windows clients attempt unsecured Dynamic DNS (DDNS) updates when they boot or when their IP address changes.</span></span> <span data-ttu-id="cefbf-114">hello DNS-namn är hello hostname plus hello primärt DNS-suffix.</span><span class="sxs-lookup"><span data-stu-id="cefbf-114">hello DNS name is hello hostname plus hello primary DNS suffix.</span></span> <span data-ttu-id="cefbf-115">Azure låter hello primärt DNS-suffix som är tom, men du kan ange detta i hello VM via hello [UI](https://technet.microsoft.com/library/cc794784.aspx) eller [med hjälp av automation](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).</span><span class="sxs-lookup"><span data-stu-id="cefbf-115">Azure leaves hello primary DNS suffix blank, but you can set this in hello VM, via hello [UI](https://technet.microsoft.com/library/cc794784.aspx) or [by using automation](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).</span></span>

<span data-ttu-id="cefbf-116">Domänanslutna Windows-klienter för att registrera sina IP-adresser med hello domänkontrollant med hjälp av säkra dynamiska DNS.</span><span class="sxs-lookup"><span data-stu-id="cefbf-116">Domain-joined Windows clients register their IP addresses with hello domain controller by using secure Dynamic DNS.</span></span> <span data-ttu-id="cefbf-117">hello domänanslutning processen anger hello primärt DNS-suffix på klienten hello och skapar och underhåller hello förtroenderelation.</span><span class="sxs-lookup"><span data-stu-id="cefbf-117">hello domain-join process sets hello primary DNS suffix on hello client and creates and maintains hello trust relationship.</span></span>

## <a name="linux-clients"></a><span data-ttu-id="cefbf-118">Linux-klienter</span><span class="sxs-lookup"><span data-stu-id="cefbf-118">Linux clients</span></span>
<span data-ttu-id="cefbf-119">Linux-klienter normalt registrera inte sig själva med hello DNS-server på Start, de förutsätter hello DHCP-servern fungerar.</span><span class="sxs-lookup"><span data-stu-id="cefbf-119">Linux clients generally don't register themselves with hello DNS server on startup, they assume hello DHCP server does it.</span></span> <span data-ttu-id="cefbf-120">Azures DHCP-servrar har inte möjlighet hello eller autentiseringsuppgifter tooregister poster i DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="cefbf-120">Azure's DHCP servers do not have hello ability or credentials tooregister records in your DNS server.</span></span>  <span data-ttu-id="cefbf-121">Du kan använda ett verktyg som kallas *nsupdate*, som ingår i hello Bind paketet, toosend dynamiska DNS-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="cefbf-121">You can use a tool called *nsupdate*, which is included in hello Bind package, toosend Dynamic DNS updates.</span></span> <span data-ttu-id="cefbf-122">Eftersom hello dynamisk DNS-protokollet är standardiserad, kan du använda *nsupdate* även när du inte använder Bind på hello DNS-server.</span><span class="sxs-lookup"><span data-stu-id="cefbf-122">Because hello Dynamic DNS protocol is standardized, you can use *nsupdate* even when you're not using Bind on hello DNS server.</span></span>

<span data-ttu-id="cefbf-123">Du kan använda hello hook som tillhandahålls av hello DHCP-klient toocreate och underhålla hello värdnamnsposten i hello DNS-server.</span><span class="sxs-lookup"><span data-stu-id="cefbf-123">You can use hello hooks that are provided by hello DHCP client toocreate and maintain hello hostname entry in hello DNS server.</span></span> <span data-ttu-id="cefbf-124">Under hello DHCP cykel hello klienten kör hello skript i */etc/dhcp/dhclient-exit-hooks.d/*.</span><span class="sxs-lookup"><span data-stu-id="cefbf-124">During hello DHCP cycle, hello client executes hello scripts in */etc/dhcp/dhclient-exit-hooks.d/*.</span></span> <span data-ttu-id="cefbf-125">Detta kan vara används tooregister hello nya IP-adressen med hjälp av *nsupdate*.</span><span class="sxs-lookup"><span data-stu-id="cefbf-125">This can be used tooregister hello new IP address by using *nsupdate*.</span></span> <span data-ttu-id="cefbf-126">Exempel:</span><span class="sxs-lookup"><span data-stu-id="cefbf-126">For example:</span></span>

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

        
        

<span data-ttu-id="cefbf-127">Du kan också använda hello *nsupdate* kommandot tooperform säkra dynamiska DNS-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="cefbf-127">You can also use hello *nsupdate* command tooperform secure Dynamic DNS updates.</span></span> <span data-ttu-id="cefbf-128">När du använder en Bind DNS-server, ett privat-offentligt nyckelpar är exempelvis [genereras](http://linux.yyz.us/nsupdate/).</span><span class="sxs-lookup"><span data-stu-id="cefbf-128">For example, when you're using a Bind DNS server, a public-private key pair is [generated](http://linux.yyz.us/nsupdate/).</span></span>  <span data-ttu-id="cefbf-129">hello DNS-servern är [konfigurerats](http://linux.yyz.us/dns/ddns-server.html) med hello offentliga del av hello nyckel så att den kan kontrollera hello signaturen på hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="cefbf-129">hello DNS server is [configured](http://linux.yyz.us/dns/ddns-server.html) with hello public part of hello key so that it can verify hello signature on hello request.</span></span> <span data-ttu-id="cefbf-130">Du måste använda hello *-k* alternativet tooprovide hello nyckel för*nsupdate* för hello dynamisk DNS-uppdatering begäran toobe signerade.</span><span class="sxs-lookup"><span data-stu-id="cefbf-130">You must use hello *-k* option tooprovide hello key-pair too*nsupdate* in order for hello Dynamic DNS update request toobe signed.</span></span>

<span data-ttu-id="cefbf-131">När du använder en Windows DNS-server kan du använda Kerberos-autentisering med hello *-g* parameter i *nsupdate* (inte tillgängligt i hello Windows-versionen av *nsupdate* ).</span><span class="sxs-lookup"><span data-stu-id="cefbf-131">When you're using a Windows DNS server, you can use Kerberos authentication with hello *-g* parameter in *nsupdate* (not available in hello Windows version of *nsupdate*).</span></span> <span data-ttu-id="cefbf-132">toodo denna, Använd *kinit* tooload hello autentiseringsuppgifter (t.ex. från en [keytab filen](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)).</span><span class="sxs-lookup"><span data-stu-id="cefbf-132">toodo this, use *kinit* tooload hello credentials (e.g. from a [keytab file](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)).</span></span> <span data-ttu-id="cefbf-133">Sedan *nsupdate -g* ska hämta hello autentiseringsuppgifter från hello cache.</span><span class="sxs-lookup"><span data-stu-id="cefbf-133">Then *nsupdate -g* will pick up hello credentials from hello cache.</span></span>

<span data-ttu-id="cefbf-134">Om det behövs kan du lägga till en DNS-sökning suffix tooyour virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="cefbf-134">If needed, you can add a DNS search suffix tooyour VMs.</span></span> <span data-ttu-id="cefbf-135">hello DNS-suffix anges i hello */etc/resolv.conf* fil.</span><span class="sxs-lookup"><span data-stu-id="cefbf-135">hello DNS suffix is specified in hello */etc/resolv.conf* file.</span></span> <span data-ttu-id="cefbf-136">De flesta distributioner av Linux hantera automatiskt hello innehållet för den här filen, så ofta du kan inte redigera den.</span><span class="sxs-lookup"><span data-stu-id="cefbf-136">Most Linux distros automatically manage hello content of this file, so usually you can't edit it.</span></span> <span data-ttu-id="cefbf-137">Du kan dock åsidosätta hello suffix med hello DHCP-klient *ersätter* kommando.</span><span class="sxs-lookup"><span data-stu-id="cefbf-137">However, you can override hello suffix by using hello DHCP client's *supersede* command.</span></span> <span data-ttu-id="cefbf-138">toodo detta i */etc/dhcp/dhclient.conf*, lägga till:</span><span class="sxs-lookup"><span data-stu-id="cefbf-138">toodo this, in */etc/dhcp/dhclient.conf*, add:</span></span>

        supersede domain-name <required-dns-suffix>;

