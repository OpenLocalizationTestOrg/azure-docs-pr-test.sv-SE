---
title: "Dataflödet i aaaTesting Virtuella Azure-nätverket | Microsoft Docs"
description: "Lär dig hur tootest Azure virtuella nätverk genomflöde."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/21/2017
ms.author: steveesp
ms.openlocfilehash: 2da85c27bc8d16a443b215891f4cd0460f41926f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bandwidththroughput-testing-ntttcp"></a><span data-ttu-id="61690-103">Bandbredd/dataflöde testning (NTTTCP)</span><span class="sxs-lookup"><span data-stu-id="61690-103">Bandwidth/Throughput testing (NTTTCP)</span></span>

<span data-ttu-id="61690-104">När du testar nätverksprestanda genomflöde i Azure, är det bästa toouse ett verktyg som riktar sig till hello nätverket för testning och minimerar hello användning av andra resurser som kan påverka prestanda.</span><span class="sxs-lookup"><span data-stu-id="61690-104">When testing network throughput performance in Azure, it's best toouse a tool that targets hello network for testing and minimizes hello use of other resources that could impact performance.</span></span> <span data-ttu-id="61690-105">NTTTCP rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="61690-105">NTTTCP is recommended.</span></span>

<span data-ttu-id="61690-106">Kopiera hello verktyget tootwo Azure virtuella datorer i hello samma storlek.</span><span class="sxs-lookup"><span data-stu-id="61690-106">Copy hello tool tootwo Azure VMs of hello same size.</span></span> <span data-ttu-id="61690-107">En virtuell dator fungerar som avsändare och hello andra som mottagare.</span><span class="sxs-lookup"><span data-stu-id="61690-107">One VM functions as SENDER and hello other as RECEIVER.</span></span>

#### <a name="deploying-vms-for-testing"></a><span data-ttu-id="61690-108">Distribuera virtuella datorer för testning</span><span class="sxs-lookup"><span data-stu-id="61690-108">Deploying VMs for testing</span></span>
<span data-ttu-id="61690-109">Hello enligt det här testet hello två virtuella datorer måste vara i hello samma molntjänst eller hello samma Tillgänglighetsuppsättning så att vi kan använda sina interna IP-adresser och undanta hello belastningsutjämnare från hello test.</span><span class="sxs-lookup"><span data-stu-id="61690-109">For hello purposes of this test, hello two VMs should be in either hello same Cloud Service or hello same Availability Set so that we can use their internal IPs and exclude hello Load Balancers from hello test.</span></span> <span data-ttu-id="61690-110">Det är möjligt tootest med hello VIP men den här typen av testning är utanför hello omfånget för det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="61690-110">It is possible tootest with hello VIP but this kind of testing is outside hello scope of this document.</span></span>
 
<span data-ttu-id="61690-111">Anteckna hello MOTTAGARENS IP-adress.</span><span class="sxs-lookup"><span data-stu-id="61690-111">Make a note of hello RECEIVER's IP address.</span></span> <span data-ttu-id="61690-112">Vi ringer som IP ”a.b.c.r”</span><span class="sxs-lookup"><span data-stu-id="61690-112">Let's call that IP "a.b.c.r"</span></span>

<span data-ttu-id="61690-113">Anteckna hello antal kärnor på hello VM.</span><span class="sxs-lookup"><span data-stu-id="61690-113">Make a note of hello number of cores on hello VM.</span></span> <span data-ttu-id="61690-114">Vi ska anropa detta ”\#num\_kärnor”</span><span class="sxs-lookup"><span data-stu-id="61690-114">Let's call this "\#num\_cores"</span></span>
 
<span data-ttu-id="61690-115">Kör hello NTTTCP testa för 300 sekunder (eller 5 minuter) på hello avsändaren VM och mottagaren VM.</span><span class="sxs-lookup"><span data-stu-id="61690-115">Run hello NTTTCP test for 300 seconds (or 5 minutes) on hello sender VM and receiver VM.</span></span>

<span data-ttu-id="61690-116">Tips: När du ställer in det här testet för hello första gången, kan du en kortare test period tooget feedback snabbare.</span><span class="sxs-lookup"><span data-stu-id="61690-116">Tip: When setting up this test for hello first time, you might try a shorter test period tooget feedback sooner.</span></span> <span data-ttu-id="61690-117">När verktyget hello fungerar som förväntat, utöka hello test period too300 sekunder för hello mest korrekta resultat.</span><span class="sxs-lookup"><span data-stu-id="61690-117">Once hello tool is working as expected, extend hello test period too300 seconds for hello most accurate results.</span></span>

> [!NOTE]
> <span data-ttu-id="61690-118">hello avsändaren **och** mottagare måste ange **hello samma** testa varaktighet parameter (-t).</span><span class="sxs-lookup"><span data-stu-id="61690-118">hello sender **and** receiver must specify **hello same** test duration parameter (-t).</span></span>

<span data-ttu-id="61690-119">tootest en TCP-ström för 10 sekunder:</span><span class="sxs-lookup"><span data-stu-id="61690-119">tootest a single TCP stream for 10 seconds:</span></span>

<span data-ttu-id="61690-120">Mottagaren parametrar: ntttcp - r -t 10 - P 1</span><span class="sxs-lookup"><span data-stu-id="61690-120">Receiver parameters: ntttcp -r -t 10 -P 1</span></span>

<span data-ttu-id="61690-121">Avsändaren parametrar: ntttcp-s10.27.33.7 -t 10 - n 1 P - 1</span><span class="sxs-lookup"><span data-stu-id="61690-121">Sender parameters: ntttcp -s10.27.33.7 -t 10 -n 1 -P 1</span></span>

> [!NOTE]
> <span data-ttu-id="61690-122">föregående exempel hello bara ska använda tooconfirm din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="61690-122">hello preceding sample should only be used tooconfirm your configuration.</span></span> <span data-ttu-id="61690-123">Giltiga exempel tester beskrivs senare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="61690-123">Valid examples of testing are covered later in this document.</span></span>

## <a name="testing-vms-running-windows"></a><span data-ttu-id="61690-124">Testa virtuella datorer som kör WINDOWS:</span><span class="sxs-lookup"><span data-stu-id="61690-124">Testing VMs running WINDOWS:</span></span>

#### <a name="get-ntttcp-onto-hello-vms"></a><span data-ttu-id="61690-125">Hämta NTTTCP till hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="61690-125">Get NTTTCP onto hello VMs.</span></span>

<span data-ttu-id="61690-126">Hämta senaste versionen av hello: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span><span class="sxs-lookup"><span data-stu-id="61690-126">Download hello latest version: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span></span>

<span data-ttu-id="61690-127">Eller söka efter den om flyttas: <https://www.bing.com/search?q=ntttcp+download> \< --bör först träffar</span><span class="sxs-lookup"><span data-stu-id="61690-127">Or search for it if moved: <https://www.bing.com/search?q=ntttcp+download>\< -- should be first hit</span></span>

<span data-ttu-id="61690-128">Du kan lägga NTTTCP i separat mapp som c:\\verktyg</span><span class="sxs-lookup"><span data-stu-id="61690-128">Consider putting NTTTCP in separate folder, like c:\\tools</span></span>

#### <a name="allow-ntttcp-through-hello-windows-firewall"></a><span data-ttu-id="61690-129">Tillåt NTTTCP hello Windows-brandväggen</span><span class="sxs-lookup"><span data-stu-id="61690-129">Allow NTTTCP through hello Windows firewall</span></span>
<span data-ttu-id="61690-130">Skapa en Tillåt-regel på hello Windows-brandväggen tooallow NTTTCP trafik tooarrive på hello mottagare.</span><span class="sxs-lookup"><span data-stu-id="61690-130">On hello RECEIVER, create an Allow rule on hello Windows Firewall tooallow the NTTTCP traffic tooarrive.</span></span> <span data-ttu-id="61690-131">I stället för att inkommande tooallow specifika TCP-portar är det enklaste tooallow hello hela NTTTCP programmet efter namn.</span><span class="sxs-lookup"><span data-stu-id="61690-131">It's easiest tooallow hello entire NTTTCP program by name rather than tooallow specific TCP ports inbound.</span></span>

<span data-ttu-id="61690-132">Tillåt ntttcp via hello Windows-brandväggen så här:</span><span class="sxs-lookup"><span data-stu-id="61690-132">Allow ntttcp through hello Windows Firewall like this:</span></span>

<span data-ttu-id="61690-133">netsh advfirewall firewall Lägg till regel för programmet =\<sökväg\>\\ntttcp.exe namn = ”ntttcp”-protokollet = alla dir = i praktiken = Tillåt aktivera = yes profil = alla</span><span class="sxs-lookup"><span data-stu-id="61690-133">netsh advfirewall firewall add rule program=\<PATH\>\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

<span data-ttu-id="61690-134">Till exempel om du har kopierat ntttcp.exe toohello ”c:\\verktyg” mapp, skulle detta hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="61690-134">For example, if you copied ntttcp.exe toohello "c:\\tools" folder, this would be hello command:</span></span> 

<span data-ttu-id="61690-135">netsh advfirewall firewall Lägg till regel för programmet = c:\\verktyg\\ntttcp.exe namn = ”ntttcp”-protokollet = alla dir = i praktiken = Tillåt aktivera = yes profil = alla</span><span class="sxs-lookup"><span data-stu-id="61690-135">netsh advfirewall firewall add rule program=c:\\tools\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

#### <a name="running-ntttcp-tests"></a><span data-ttu-id="61690-136">Kör NTTTCP</span><span class="sxs-lookup"><span data-stu-id="61690-136">Running NTTTCP tests</span></span>

<span data-ttu-id="61690-137">Starta NTTTCP på hello mottagare (**kör från CMD**, inte från PowerShell):</span><span class="sxs-lookup"><span data-stu-id="61690-137">Start NTTTCP on hello RECEIVER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="61690-138">ntttcp - r-m [2\*\#num\_kärnor],\*, a.b.c.r -t 300</span><span class="sxs-lookup"><span data-stu-id="61690-138">ntttcp -r –m [2\*\#num\_cores],\*,a.b.c.r -t 300</span></span>

<span data-ttu-id="61690-139">Om hello VM har fyra kärnor och IP-adressen 10.0.0.4, skulle det se ut så här:</span><span class="sxs-lookup"><span data-stu-id="61690-139">If hello VM has four cores and an IP address of 10.0.0.4, it would look like this:</span></span>

<span data-ttu-id="61690-140">ntttcp - r-m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="61690-140">ntttcp -r –m 8,\*,10.0.0.4 -t 300</span></span>


<span data-ttu-id="61690-141">Starta NTTTCP på hello AVSÄNDAREN (**kör från CMD**, inte från PowerShell):</span><span class="sxs-lookup"><span data-stu-id="61690-141">Start NTTTCP on hello SENDER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="61690-142">ntttcp -s-m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="61690-142">ntttcp -s –m 8,\*,10.0.0.4 -t 300</span></span> 

<span data-ttu-id="61690-143">Vänta tills hello resultat.</span><span class="sxs-lookup"><span data-stu-id="61690-143">Wait for hello results.</span></span>


## <a name="testing-vms-running-linux"></a><span data-ttu-id="61690-144">Testa virtuella datorer som kör LINUX:</span><span class="sxs-lookup"><span data-stu-id="61690-144">Testing VMs running LINUX:</span></span>

<span data-ttu-id="61690-145">Använd nttcp för linux.</span><span class="sxs-lookup"><span data-stu-id="61690-145">Use nttcp-for-linux.</span></span> <span data-ttu-id="61690-146">Den är tillgänglig från <https://github.com/Microsoft/ntttcp-for-linux></span><span class="sxs-lookup"><span data-stu-id="61690-146">It is available from <https://github.com/Microsoft/ntttcp-for-linux></span></span>

<span data-ttu-id="61690-147">Kör följande kommandon för att förbereda ntttcp för linux på dina virtuella datorer på hello virtuella Linux-datorer (både AVSÄNDAREN och mottagaren):</span><span class="sxs-lookup"><span data-stu-id="61690-147">On hello Linux VMs (both SENDER and RECEIVER), run these commands to prepare ntttcp-for-linux on your VMs:</span></span>

<span data-ttu-id="61690-148">CentOS - Install Git:</span><span class="sxs-lookup"><span data-stu-id="61690-148">CentOS - Install Git:</span></span>
``` bash
  yum install gcc -y  
  yum install git -y
```
<span data-ttu-id="61690-149">Ubuntu - Install Git:</span><span class="sxs-lookup"><span data-stu-id="61690-149">Ubuntu - Install Git:</span></span>
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
<span data-ttu-id="61690-150">Kontrollera och installera både:</span><span class="sxs-lookup"><span data-stu-id="61690-150">Make and Install on both:</span></span>
``` bash
 git clone <https://github.com/Microsoft/ntttcp-for-linux>
 cd ntttcp-for-linux/src
 make && make install
```

<span data-ttu-id="61690-151">Som hello Windows exempel förutsätter vi att hello Linux MOTTAGARENS IP är 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="61690-151">As in hello Windows example, we assume hello Linux RECEIVER's IP is 10.0.0.4</span></span>

<span data-ttu-id="61690-152">Starta NTTTCP för Linux hello mottagare:</span><span class="sxs-lookup"><span data-stu-id="61690-152">Start NTTTCP-for-Linux on hello RECEIVER:</span></span>

``` bash
ntttcp -r -t 300
```

<span data-ttu-id="61690-153">Och på hello AVSÄNDAREN, kör:</span><span class="sxs-lookup"><span data-stu-id="61690-153">And on hello SENDER, run:</span></span>

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
<span data-ttu-id="61690-154">Testa längd standardvärden too60 sekunder om ingen tid parameter anges</span><span class="sxs-lookup"><span data-stu-id="61690-154">Test length defaults too60 seconds if no time parameter is given</span></span>

## <a name="testing-between-vms-running-windows-and-linux"></a><span data-ttu-id="61690-155">Testa mellan virtuella datorer som kör Windows och LINUX:</span><span class="sxs-lookup"><span data-stu-id="61690-155">Testing between VMs running Windows and LINUX:</span></span>

<span data-ttu-id="61690-156">På den här scenarier bör vi aktivera hello Nej-Synkroniseringsläge hello test kan köras.</span><span class="sxs-lookup"><span data-stu-id="61690-156">On this scenarios we should enable hello no-sync mode so hello test can run.</span></span> <span data-ttu-id="61690-157">Detta görs med hjälp av hello **-N flaggan** för Linux och **-ns flaggan** för Windows.</span><span class="sxs-lookup"><span data-stu-id="61690-157">This is done by using hello **-N flag** for Linux, and **-ns flag** for Windows.</span></span>

#### <a name="from-linux-toowindows"></a><span data-ttu-id="61690-158">Från Linux tooWindows:</span><span class="sxs-lookup"><span data-stu-id="61690-158">From Linux tooWindows:</span></span>

<span data-ttu-id="61690-159">Mottagaren <Windows>:</span><span class="sxs-lookup"><span data-stu-id="61690-159">Receiver <Windows>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

<span data-ttu-id="61690-160">Avsändaren <Linux> :</span><span class="sxs-lookup"><span data-stu-id="61690-160">Sender <Linux> :</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-toolinux"></a><span data-ttu-id="61690-161">Från Windows tooLinux:</span><span class="sxs-lookup"><span data-stu-id="61690-161">From Windows tooLinux:</span></span>

<span data-ttu-id="61690-162">Mottagaren <Linux>:</span><span class="sxs-lookup"><span data-stu-id="61690-162">Receiver <Linux>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

<span data-ttu-id="61690-163">Avsändaren <Windows>:</span><span class="sxs-lookup"><span data-stu-id="61690-163">Sender <Windows>:</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```

## <a name="next-steps"></a><span data-ttu-id="61690-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="61690-164">Next steps</span></span>
* <span data-ttu-id="61690-165">Beroende på resultatet, det kan finnas utrymme för[optimera genomflödet datorer](virtual-network-optimize-network-bandwidth.md) för ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="61690-165">Depending on results, there may be room too[Optimize network throughput machines](virtual-network-optimize-network-bandwidth.md) for your scenario.</span></span>
* <span data-ttu-id="61690-166">Lär dig mer i [Azure Virtual Network vanliga frågor (FAQ)](virtual-networks-faq.md)</span><span class="sxs-lookup"><span data-stu-id="61690-166">Learn more wtih [Azure Virtual Network frequently asked questions (FAQ)](virtual-networks-faq.md)</span></span>
