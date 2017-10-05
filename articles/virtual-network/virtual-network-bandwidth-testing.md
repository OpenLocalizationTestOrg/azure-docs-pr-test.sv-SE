---
title: "Testa Azure VM dataflödet i nätverket | Microsoft Docs"
description: "Lär dig mer om att testa virtuell dator i Azure dataflödet i nätverket."
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
ms.openlocfilehash: ccebc722386a19014674d7a59757a3685bd50793
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="bandwidththroughput-testing-ntttcp"></a><span data-ttu-id="af8a5-103">Bandbredd/dataflöde testning (NTTTCP)</span><span class="sxs-lookup"><span data-stu-id="af8a5-103">Bandwidth/Throughput testing (NTTTCP)</span></span>

<span data-ttu-id="af8a5-104">När du testar nätverksprestanda genomflöde i Azure, är det bäst att använda ett verktyg som riktar sig till nätverket för testning och minimerar användningen av andra resurser som kan påverka prestanda.</span><span class="sxs-lookup"><span data-stu-id="af8a5-104">When testing network throughput performance in Azure, it's best to use a tool that targets the network for testing and minimizes the use of other resources that could impact performance.</span></span> <span data-ttu-id="af8a5-105">NTTTCP rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="af8a5-105">NTTTCP is recommended.</span></span>

<span data-ttu-id="af8a5-106">Kopiera verktyget till två virtuella Azure-datorer med samma storlek.</span><span class="sxs-lookup"><span data-stu-id="af8a5-106">Copy the tool to two Azure VMs of the same size.</span></span> <span data-ttu-id="af8a5-107">En virtuell dator fungerar som avsändare och andra som mottagare.</span><span class="sxs-lookup"><span data-stu-id="af8a5-107">One VM functions as SENDER and the other as RECEIVER.</span></span>

#### <a name="deploying-vms-for-testing"></a><span data-ttu-id="af8a5-108">Distribuera virtuella datorer för testning</span><span class="sxs-lookup"><span data-stu-id="af8a5-108">Deploying VMs for testing</span></span>
<span data-ttu-id="af8a5-109">Enligt det här testet ska två virtuella datorer i samma molntjänst eller i samma Tillgänglighetsuppsättning, så att vi kan använda sina interna IP-adresser och undanta belastningsutjämnare från testet.</span><span class="sxs-lookup"><span data-stu-id="af8a5-109">For the purposes of this test, the two VMs should be in either the same Cloud Service or the same Availability Set so that we can use their internal IPs and exclude the Load Balancers from the test.</span></span> <span data-ttu-id="af8a5-110">Det är möjligt att testa med VIP-Adressen men den här typen av testning ligger utanför omfånget för det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="af8a5-110">It is possible to test with the VIP but this kind of testing is outside the scope of this document.</span></span>
 
<span data-ttu-id="af8a5-111">Anteckna MOTTAGARENS IP-adress.</span><span class="sxs-lookup"><span data-stu-id="af8a5-111">Make a note of the RECEIVER's IP address.</span></span> <span data-ttu-id="af8a5-112">Vi ringer som IP ”a.b.c.r”</span><span class="sxs-lookup"><span data-stu-id="af8a5-112">Let's call that IP "a.b.c.r"</span></span>

<span data-ttu-id="af8a5-113">Notera antal kärnor på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="af8a5-113">Make a note of the number of cores on the VM.</span></span> <span data-ttu-id="af8a5-114">Vi ska anropa detta ”\#num\_kärnor”</span><span class="sxs-lookup"><span data-stu-id="af8a5-114">Let's call this "\#num\_cores"</span></span>
 
<span data-ttu-id="af8a5-115">Kör testet NTTTCP för 300 sekunder (eller 5 minuter) på VM-avsändare och mottagare VM.</span><span class="sxs-lookup"><span data-stu-id="af8a5-115">Run the NTTTCP test for 300 seconds (or 5 minutes) on the sender VM and receiver VM.</span></span>

<span data-ttu-id="af8a5-116">Tips: När du ställer in det här testet för första gången du försöka med en kortare period test om du vill ha feedback snabbare.</span><span class="sxs-lookup"><span data-stu-id="af8a5-116">Tip: When setting up this test for the first time, you might try a shorter test period to get feedback sooner.</span></span> <span data-ttu-id="af8a5-117">När verktyget fungerar som förväntat, utöka testperioden med 300 sekunder för de mest korrekta resultat.</span><span class="sxs-lookup"><span data-stu-id="af8a5-117">Once the tool is working as expected, extend the test period to 300 seconds for the most accurate results.</span></span>

> [!NOTE]
> <span data-ttu-id="af8a5-118">Avsändaren **och** mottagare måste ange **samma** testa varaktighet parameter (-t).</span><span class="sxs-lookup"><span data-stu-id="af8a5-118">The sender **and** receiver must specify **the same** test duration parameter (-t).</span></span>

<span data-ttu-id="af8a5-119">Så här testar du en enda TCP-ström för 10 sekunder:</span><span class="sxs-lookup"><span data-stu-id="af8a5-119">To test a single TCP stream for 10 seconds:</span></span>

<span data-ttu-id="af8a5-120">Mottagaren parametrar: ntttcp - r -t 10 - P 1</span><span class="sxs-lookup"><span data-stu-id="af8a5-120">Receiver parameters: ntttcp -r -t 10 -P 1</span></span>

<span data-ttu-id="af8a5-121">Avsändaren parametrar: ntttcp-s10.27.33.7 -t 10 - n 1 P - 1</span><span class="sxs-lookup"><span data-stu-id="af8a5-121">Sender parameters: ntttcp -s10.27.33.7 -t 10 -n 1 -P 1</span></span>

> [!NOTE]
> <span data-ttu-id="af8a5-122">Föregående exempel ska bara användas för att bekräfta din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="af8a5-122">The preceding sample should only be used to confirm your configuration.</span></span> <span data-ttu-id="af8a5-123">Giltiga exempel tester beskrivs senare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="af8a5-123">Valid examples of testing are covered later in this document.</span></span>

## <a name="testing-vms-running-windows"></a><span data-ttu-id="af8a5-124">Testa virtuella datorer som kör WINDOWS:</span><span class="sxs-lookup"><span data-stu-id="af8a5-124">Testing VMs running WINDOWS:</span></span>

#### <a name="get-ntttcp-onto-the-vms"></a><span data-ttu-id="af8a5-125">Hämta NTTTCP till de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="af8a5-125">Get NTTTCP onto the VMs.</span></span>

<span data-ttu-id="af8a5-126">Hämta den senaste versionen: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span><span class="sxs-lookup"><span data-stu-id="af8a5-126">Download the latest version: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span></span>

<span data-ttu-id="af8a5-127">Eller söka efter den om flyttas: <https://www.bing.com/search?q=ntttcp+download> \< --bör först träffar</span><span class="sxs-lookup"><span data-stu-id="af8a5-127">Or search for it if moved: <https://www.bing.com/search?q=ntttcp+download>\< -- should be first hit</span></span>

<span data-ttu-id="af8a5-128">Du kan lägga NTTTCP i separat mapp som c:\\verktyg</span><span class="sxs-lookup"><span data-stu-id="af8a5-128">Consider putting NTTTCP in separate folder, like c:\\tools</span></span>

#### <a name="allow-ntttcp-through-the-windows-firewall"></a><span data-ttu-id="af8a5-129">Tillåt NTTTCP via Windows-brandväggen</span><span class="sxs-lookup"><span data-stu-id="af8a5-129">Allow NTTTCP through the Windows firewall</span></span>
<span data-ttu-id="af8a5-130">Skapa en Tillåt-regel i Windows-brandväggen så att den NTTTCP trafiken som tas emot på mottagaren..</span><span class="sxs-lookup"><span data-stu-id="af8a5-130">On the RECEIVER, create an Allow rule on the Windows Firewall to allow the NTTTCP traffic to arrive.</span></span> <span data-ttu-id="af8a5-131">Det enklast att hela NTTTCP programmet efter namn snarare än för att tillåta inkommande specifika TCP-portar.</span><span class="sxs-lookup"><span data-stu-id="af8a5-131">It's easiest to allow the entire NTTTCP program by name rather than to allow specific TCP ports inbound.</span></span>

<span data-ttu-id="af8a5-132">Tillåt ntttcp via Windows-brandväggen så här:</span><span class="sxs-lookup"><span data-stu-id="af8a5-132">Allow ntttcp through the Windows Firewall like this:</span></span>

<span data-ttu-id="af8a5-133">netsh advfirewall firewall Lägg till regel för programmet =\<sökväg\>\\ntttcp.exe namn = ”ntttcp”-protokollet = alla dir = i praktiken = Tillåt aktivera = yes profil = alla</span><span class="sxs-lookup"><span data-stu-id="af8a5-133">netsh advfirewall firewall add rule program=\<PATH\>\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

<span data-ttu-id="af8a5-134">Om du kopierade ntttcp.exe till exempelvis den ”c:\\verktyg” mapp, skulle detta kommandot:</span><span class="sxs-lookup"><span data-stu-id="af8a5-134">For example, if you copied ntttcp.exe to the "c:\\tools" folder, this would be the command:</span></span> 

<span data-ttu-id="af8a5-135">netsh advfirewall firewall Lägg till regel för programmet = c:\\verktyg\\ntttcp.exe namn = ”ntttcp”-protokollet = alla dir = i praktiken = Tillåt aktivera = yes profil = alla</span><span class="sxs-lookup"><span data-stu-id="af8a5-135">netsh advfirewall firewall add rule program=c:\\tools\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

#### <a name="running-ntttcp-tests"></a><span data-ttu-id="af8a5-136">Kör NTTTCP</span><span class="sxs-lookup"><span data-stu-id="af8a5-136">Running NTTTCP tests</span></span>

<span data-ttu-id="af8a5-137">Starta NTTTCP på mottagaren (**kör från CMD**, inte från PowerShell):</span><span class="sxs-lookup"><span data-stu-id="af8a5-137">Start NTTTCP on the RECEIVER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="af8a5-138">ntttcp - r-m [2\*\#num\_kärnor],\*, a.b.c.r -t 300</span><span class="sxs-lookup"><span data-stu-id="af8a5-138">ntttcp -r –m [2\*\#num\_cores],\*,a.b.c.r -t 300</span></span>

<span data-ttu-id="af8a5-139">Om den virtuella datorn har fyra kärnor och IP-adressen 10.0.0.4, skulle det se ut så här:</span><span class="sxs-lookup"><span data-stu-id="af8a5-139">If the VM has four cores and an IP address of 10.0.0.4, it would look like this:</span></span>

<span data-ttu-id="af8a5-140">ntttcp - r-m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="af8a5-140">ntttcp -r –m 8,\*,10.0.0.4 -t 300</span></span>


<span data-ttu-id="af8a5-141">Starta NTTTCP på AVSÄNDAREN (**kör från CMD**, inte från PowerShell):</span><span class="sxs-lookup"><span data-stu-id="af8a5-141">Start NTTTCP on the SENDER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="af8a5-142">ntttcp -s-m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="af8a5-142">ntttcp -s –m 8,\*,10.0.0.4 -t 300</span></span> 

<span data-ttu-id="af8a5-143">Vänta tills resultaten.</span><span class="sxs-lookup"><span data-stu-id="af8a5-143">Wait for the results.</span></span>


## <a name="testing-vms-running-linux"></a><span data-ttu-id="af8a5-144">Testa virtuella datorer som kör LINUX:</span><span class="sxs-lookup"><span data-stu-id="af8a5-144">Testing VMs running LINUX:</span></span>

<span data-ttu-id="af8a5-145">Använd nttcp för linux.</span><span class="sxs-lookup"><span data-stu-id="af8a5-145">Use nttcp-for-linux.</span></span> <span data-ttu-id="af8a5-146">Den är tillgänglig från <https://github.com/Microsoft/ntttcp-for-linux></span><span class="sxs-lookup"><span data-stu-id="af8a5-146">It is available from <https://github.com/Microsoft/ntttcp-for-linux></span></span>

<span data-ttu-id="af8a5-147">Kör följande kommandon för att förbereda ntttcp för linux på dina virtuella datorer på Linux virtuella datorer (både AVSÄNDAREN och mottagaren):</span><span class="sxs-lookup"><span data-stu-id="af8a5-147">On the Linux VMs (both SENDER and RECEIVER), run these commands to prepare ntttcp-for-linux on your VMs:</span></span>

<span data-ttu-id="af8a5-148">CentOS - Install Git:</span><span class="sxs-lookup"><span data-stu-id="af8a5-148">CentOS - Install Git:</span></span>
``` bash
  yum install gcc -y  
  yum install git -y
```
<span data-ttu-id="af8a5-149">Ubuntu - Install Git:</span><span class="sxs-lookup"><span data-stu-id="af8a5-149">Ubuntu - Install Git:</span></span>
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
<span data-ttu-id="af8a5-150">Kontrollera och installera både:</span><span class="sxs-lookup"><span data-stu-id="af8a5-150">Make and Install on both:</span></span>
``` bash
 git clone <https://github.com/Microsoft/ntttcp-for-linux>
 cd ntttcp-for-linux/src
 make && make install
```

<span data-ttu-id="af8a5-151">Som i Windows-exemplet förutsätter vi att mottagaren Linux IP är 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="af8a5-151">As in the Windows example, we assume the Linux RECEIVER's IP is 10.0.0.4</span></span>

<span data-ttu-id="af8a5-152">Starta NTTTCP för Linux mottagaren:</span><span class="sxs-lookup"><span data-stu-id="af8a5-152">Start NTTTCP-for-Linux on the RECEIVER:</span></span>

``` bash
ntttcp -r -t 300
```

<span data-ttu-id="af8a5-153">Och på AVSÄNDAREN, kör:</span><span class="sxs-lookup"><span data-stu-id="af8a5-153">And on the SENDER, run:</span></span>

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
<span data-ttu-id="af8a5-154">Test längd standardvärdet är 60 sekunder om ingen tid parameter anges</span><span class="sxs-lookup"><span data-stu-id="af8a5-154">Test length defaults to 60 seconds if no time parameter is given</span></span>

## <a name="testing-between-vms-running-windows-and-linux"></a><span data-ttu-id="af8a5-155">Testa mellan virtuella datorer som kör Windows och LINUX:</span><span class="sxs-lookup"><span data-stu-id="af8a5-155">Testing between VMs running Windows and LINUX:</span></span>

<span data-ttu-id="af8a5-156">På den här scenarier bör vi aktivera läget no sync testet kan köras.</span><span class="sxs-lookup"><span data-stu-id="af8a5-156">On this scenarios we should enable the no-sync mode so the test can run.</span></span> <span data-ttu-id="af8a5-157">Detta görs med hjälp av den **-N flaggan** för Linux och **-ns flaggan** för Windows.</span><span class="sxs-lookup"><span data-stu-id="af8a5-157">This is done by using the **-N flag** for Linux, and **-ns flag** for Windows.</span></span>

#### <a name="from-linux-to-windows"></a><span data-ttu-id="af8a5-158">Från Linux i Windows:</span><span class="sxs-lookup"><span data-stu-id="af8a5-158">From Linux to Windows:</span></span>

<span data-ttu-id="af8a5-159">Mottagaren <Windows>:</span><span class="sxs-lookup"><span data-stu-id="af8a5-159">Receiver <Windows>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

<span data-ttu-id="af8a5-160">Avsändaren <Linux> :</span><span class="sxs-lookup"><span data-stu-id="af8a5-160">Sender <Linux> :</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-to-linux"></a><span data-ttu-id="af8a5-161">Från Windows till Linux:</span><span class="sxs-lookup"><span data-stu-id="af8a5-161">From Windows to Linux:</span></span>

<span data-ttu-id="af8a5-162">Mottagaren <Linux>:</span><span class="sxs-lookup"><span data-stu-id="af8a5-162">Receiver <Linux>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

<span data-ttu-id="af8a5-163">Avsändaren <Windows>:</span><span class="sxs-lookup"><span data-stu-id="af8a5-163">Sender <Windows>:</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```

## <a name="next-steps"></a><span data-ttu-id="af8a5-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af8a5-164">Next steps</span></span>
* <span data-ttu-id="af8a5-165">Beroende på resultatet, det kan finnas utrymme att [optimera genomflödet datorer](virtual-network-optimize-network-bandwidth.md) för ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="af8a5-165">Depending on results, there may be room to [Optimize network throughput machines](virtual-network-optimize-network-bandwidth.md) for your scenario.</span></span>
* <span data-ttu-id="af8a5-166">Lär dig mer i [Azure Virtual Network vanliga frågor (FAQ)](virtual-networks-faq.md)</span><span class="sxs-lookup"><span data-stu-id="af8a5-166">Learn more wtih [Azure Virtual Network frequently asked questions (FAQ)](virtual-networks-faq.md)</span></span>
