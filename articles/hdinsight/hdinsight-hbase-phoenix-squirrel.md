---
title: aaaUse Apache Phoenix och SQuirreL med Windows-baserade Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Apache Phoenix i HDInsight, och hur tooinstall och konfigurera SQuirreL på din arbetsstation tooconnect tooan HBase-kluster i HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1a756e98-75c1-44cd-a178-c5119683b7b7
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 147ac35fa882fd1bedbc5361ac804c36a4d56de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a><span data-ttu-id="b7104-103">Använda Apache Phoenix och SQuirreL med Windows-baserade HBase-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="b7104-103">Use Apache Phoenix and SQuirreL with Windows-based HBase clusters in HDInsight</span></span>
<span data-ttu-id="b7104-104">Lär dig hur toouse [Apache Phoenix](http://phoenix.apache.org/) i HDInsight, och hur tooinstall och konfigurera SQuirreL på din arbetsstation tooconnect tooan HBase-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b7104-104">Learn how toouse [Apache Phoenix](http://phoenix.apache.org/) in HDInsight, and how tooinstall and configure SQuirreL on your workstation tooconnect tooan HBase cluster in HDInsight.</span></span> <span data-ttu-id="b7104-105">Läs mer om Phoenix [Phoenix i 15 minuter eller mindre](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span><span class="sxs-lookup"><span data-stu-id="b7104-105">For more information about Phoenix, see [Phoenix in 15 minutes or less](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span></span> <span data-ttu-id="b7104-106">Hello Phoenix grammatik, se [Phoenix grammatik](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="b7104-106">For hello Phoenix grammar, see [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

> [!NOTE]
> <span data-ttu-id="b7104-107">Hello Phoenix versionsinformation i HDInsight, se [vad är nytt i hello Hadoop-klusterversioner som tillhandahålls av HDInsight?](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="b7104-107">For hello Phoenix version information in HDInsight, see [What's new in hello Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).</span></span>
>

> [!IMPORTANT]
> <span data-ttu-id="b7104-108">hello stegen i det här dokumentet endast fungerar för Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="b7104-108">hello steps in this document only work for Windows-based HDInsight clusters.</span></span> <span data-ttu-id="b7104-109">HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="b7104-109">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="b7104-110">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b7104-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b7104-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b7104-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="b7104-112">Information om hur du använder Phoenix på Linux-baserat HDInsight finns i [använda Apache Phoenix med Linux-baserade HBase-kluster i HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).</span><span class="sxs-lookup"><span data-stu-id="b7104-112">For information on using Phoenix on Linux-based HDInsight, see [Use Apache Phoenix with Linux-based HBase clusters in HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).</span></span>
>



## <a name="use-sqlline"></a><span data-ttu-id="b7104-113">Använd SQLLine</span><span class="sxs-lookup"><span data-stu-id="b7104-113">Use SQLLine</span></span>
<span data-ttu-id="b7104-114">[SQLLine](http://sqlline.sourceforge.net/) är en kommandorad verktyget tooexecute SQL.</span><span class="sxs-lookup"><span data-stu-id="b7104-114">[SQLLine](http://sqlline.sourceforge.net/) is a command line utility tooexecute SQL.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b7104-115">Krav</span><span class="sxs-lookup"><span data-stu-id="b7104-115">Prerequisites</span></span>
<span data-ttu-id="b7104-116">Innan du kan använda SQLLine, måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="b7104-116">Before you can use SQLLine, you must have hello following:</span></span>

* <span data-ttu-id="b7104-117">**En HBase-kluster i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="b7104-117">**A HBase cluster in HDInsight**.</span></span> <span data-ttu-id="b7104-118">Mer information om etablera HBase-kluster, se [Kom igång med Apache HBase i HDInsight][hdinsight-hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="b7104-118">For information on provision HBase cluster, see [Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started].</span></span>
* <span data-ttu-id="b7104-119">**Ansluta toohello HBase-kluster via hello Fjärrskrivbordsprotokollet**.</span><span class="sxs-lookup"><span data-stu-id="b7104-119">**Connect toohello HBase cluster via hello remote desktop protocol**.</span></span> <span data-ttu-id="b7104-120">Instruktioner finns i [hantera Hadoop-kluster i HDInsight med hjälp av hello Azure klassiska Portal][hdinsight-manage-portal].</span><span class="sxs-lookup"><span data-stu-id="b7104-120">For instructions, see [Manage Hadoop clusters in HDInsight by using hello Azure Classic Portal][hdinsight-manage-portal].</span></span>

<span data-ttu-id="b7104-121">**toofind ut hello värdnamn**</span><span class="sxs-lookup"><span data-stu-id="b7104-121">**toofind out hello host name**</span></span>

1. <span data-ttu-id="b7104-122">Öppna **Hadoop kommandoraden** från hello skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="b7104-122">Open **Hadoop Command Line** from hello desktop.</span></span>
2. <span data-ttu-id="b7104-123">Kör följande kommando tooget hello DNS-suffix hello:</span><span class="sxs-lookup"><span data-stu-id="b7104-123">Run hello following command tooget hello DNS suffix:</span></span>

        ipconfig

    <span data-ttu-id="b7104-124">Skriv ned **anslutningsspecifika DNS-suffixet**.</span><span class="sxs-lookup"><span data-stu-id="b7104-124">Write down **Connection-specific DNS Suffix**.</span></span> <span data-ttu-id="b7104-125">Till exempel *myhbasecluster.f5.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="b7104-125">For example, *myhbasecluster.f5.internal.cloudapp.net*.</span></span> <span data-ttu-id="b7104-126">När du ansluter tooan HBase-kluster måste tooconnect tooone av hello Zookeepers med FQDN.</span><span class="sxs-lookup"><span data-stu-id="b7104-126">When you connect tooan HBase cluster, you will need tooconnect tooone of hello Zookeepers using FQDN.</span></span> <span data-ttu-id="b7104-127">Varje HDInsight-kluster har 3 Zookeepers.</span><span class="sxs-lookup"><span data-stu-id="b7104-127">Each HDInsight cluster has 3 Zookeepers.</span></span> <span data-ttu-id="b7104-128">De är *zookeeper0*, *zookeeper1*, och *zookeeper2*.</span><span class="sxs-lookup"><span data-stu-id="b7104-128">They are *zookeeper0*, *zookeeper1*, and *zookeeper2*.</span></span> <span data-ttu-id="b7104-129">hello FQDN blir något som liknar *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="b7104-129">hello FQDN will be something like *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.</span></span>

<span data-ttu-id="b7104-130">**toouse SQLLine**</span><span class="sxs-lookup"><span data-stu-id="b7104-130">**toouse SQLLine**</span></span>

1. <span data-ttu-id="b7104-131">Öppna **Hadoop kommandoraden** från hello skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="b7104-131">Open **Hadoop Command Line** from hello desktop.</span></span>
2. <span data-ttu-id="b7104-132">Kör följande kommandon tooopen SQLLine hello:</span><span class="sxs-lookup"><span data-stu-id="b7104-132">Run hello following commands tooopen SQLLine:</span></span>

        cd %phoenix_home%\bin
        sqlline.py [hello FQDN of one of hello Zookeepers]

    ![HDInsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    <span data-ttu-id="b7104-134">hello-kommandon som används i hello exemplet:</span><span class="sxs-lookup"><span data-stu-id="b7104-134">hello commands used in hello sample:</span></span>

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

<span data-ttu-id="b7104-135">Mer information finns i [SQLLine manuell](http://sqlline.sourceforge.net/#manual) och [Phoenix grammatik](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="b7104-135">For more information, see [SQLLine manual](http://sqlline.sourceforge.net/#manual) and [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

## <a name="use-squirrel"></a><span data-ttu-id="b7104-136">Använd SQuirreL</span><span class="sxs-lookup"><span data-stu-id="b7104-136">Use SQuirreL</span></span>
<span data-ttu-id="b7104-137">[SQuirreL SQL Client](http://squirrel-sql.sourceforge.net/) är ett grafiskt Java-program som gör att du tooview hello struktur för en JDBC-kompatibel databas, bläddra hello data i tabeller, utfärda SQL-kommandon osv. Det kan vara används tooconnect tooApache Phoenix på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b7104-137">[SQuirreL SQL Client](http://squirrel-sql.sourceforge.net/) is a graphical Java program that will allow you tooview hello structure of a JDBC compliant database, browse hello data in tables, issue SQL commands etc. It can be used tooconnect tooApache Phoenix on HDInsight.</span></span>

<span data-ttu-id="b7104-138">Det här avsnittet beskrivs hur du tooinstall och konfigurera SQuirreL på din arbetsstation tooconnect tooan HBase-kluster i HDInsight via VPN.</span><span class="sxs-lookup"><span data-stu-id="b7104-138">This section shows you how tooinstall and configure SQuirreL on your workstation tooconnect tooan HBase cluster in HDInsight via VPN.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b7104-139">Krav</span><span class="sxs-lookup"><span data-stu-id="b7104-139">Prerequisites</span></span>
<span data-ttu-id="b7104-140">Innan du följer hello procedurer, måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="b7104-140">Before following hello procedures, you must have hello following:</span></span>

* <span data-ttu-id="b7104-141">Ett HBase-kluster distribueras tooan Azure-nätverk med en virtuell dator i DNS.</span><span class="sxs-lookup"><span data-stu-id="b7104-141">An HBase cluster deployed tooan Azure virtual network with a DNS virtual machine.</span></span>  <span data-ttu-id="b7104-142">Instruktioner finns i [skapa HBase-kluster i Azure Virtual Network][hdinsight-hbase-provision-vnet].</span><span class="sxs-lookup"><span data-stu-id="b7104-142">For instructions, see [Create HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet].</span></span>

* <span data-ttu-id="b7104-143">Hämta hello HBase-kluster klustret anslutningsspecifika DNS-suffix.</span><span class="sxs-lookup"><span data-stu-id="b7104-143">Get hello HBase cluster cluster Connection-specific DNS suffix.</span></span> <span data-ttu-id="b7104-144">tooget den RDP till hello-kluster och kör sedan IPConfig.</span><span class="sxs-lookup"><span data-stu-id="b7104-144">tooget it, RDP into hello cluster, and then run IPConfig.</span></span>  <span data-ttu-id="b7104-145">hello DNS-suffix är ungefär:</span><span class="sxs-lookup"><span data-stu-id="b7104-145">hello DNS suffix is similar to:</span></span>

        myhbase.b7.internal.cloudapp.net
* <span data-ttu-id="b7104-146">Hämta och installera [Microsoft Visual Studio Express för Windows-skrivbordet](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) på arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="b7104-146">Download and install [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) on your workstation.</span></span> <span data-ttu-id="b7104-147">Du behöver makecert från hello paketet toocreate ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="b7104-147">You will need makecert from hello package toocreate your certificate.</span></span>  
* <span data-ttu-id="b7104-148">Hämta och installera [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) på arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="b7104-148">Download and install [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) on your workstation.</span></span>  <span data-ttu-id="b7104-149">SQuirreL SQL klientversion 3.0 och senare kräver JRE version 1.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b7104-149">SQuirreL SQL client version 3.0 and higher requires JRE version 1.6 or higher.</span></span>  

### <a name="configure-a-point-to-site-vpn-connection-toohello-azure-virtual-network"></a><span data-ttu-id="b7104-150">Konfigurera en punkt-till-plats VPN-anslutningen toohello virtuella Azure-nätverket</span><span class="sxs-lookup"><span data-stu-id="b7104-150">Configure a Point-to-Site VPN connection toohello Azure virtual network</span></span>
<span data-ttu-id="b7104-151">Ingår 3 steg hur du konfigurerar en punkt-till-plats VPN-anslutning:</span><span class="sxs-lookup"><span data-stu-id="b7104-151">There are 3 steps involved configuring a point-to-site VPN connection:</span></span>

1. [<span data-ttu-id="b7104-152">Konfigurera ett virtuellt nätverk och en dynamisk routning gateway</span><span class="sxs-lookup"><span data-stu-id="b7104-152">Configure a virtual network and a dynamic routing gateway</span></span>](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [<span data-ttu-id="b7104-153">Skapa certifikat</span><span class="sxs-lookup"><span data-stu-id="b7104-153">Create your certificates</span></span>](#Create-your-certificates)
3. [<span data-ttu-id="b7104-154">Konfigurera VPN-klienten</span><span class="sxs-lookup"><span data-stu-id="b7104-154">Configure your VPN client</span></span>](#Configure-your-VPN-client)

<span data-ttu-id="b7104-155">Se [konfigurerar en punkt-till-plats VPN-anslutningen tooan Azure Virtual Network](../vpn-gateway/vpn-gateway-point-to-site-create.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="b7104-155">See [Configure a Point-to-Site VPN connection tooan Azure Virtual Network](../vpn-gateway/vpn-gateway-point-to-site-create.md) for more information.</span></span>

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a><span data-ttu-id="b7104-156">Konfigurera ett virtuellt nätverk och en dynamisk routning gateway</span><span class="sxs-lookup"><span data-stu-id="b7104-156">Configure a virtual network and a dynamic routing gateway</span></span>
<span data-ttu-id="b7104-157">Garantera att du har etablerat ett HBase-kluster i Azure-nätverk (se hello krav för det här avsnittet).</span><span class="sxs-lookup"><span data-stu-id="b7104-157">Assure you have provisioned an HBase cluster in an Azure virtual network (see hello prerequisites for this section).</span></span> <span data-ttu-id="b7104-158">hello nästa steg är tooconfigure en punkt-till-plats-anslutning.</span><span class="sxs-lookup"><span data-stu-id="b7104-158">hello next step is tooconfigure a point-to-site connection.</span></span>

<span data-ttu-id="b7104-159">**tooconfigure hello punkt-till-plats-anslutning**</span><span class="sxs-lookup"><span data-stu-id="b7104-159">**tooconfigure hello point-to-site connectivity**</span></span>

1. <span data-ttu-id="b7104-160">Logga in toohello [klassiska Azure-portalen][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="b7104-160">Sign in toohello [Azure Classic Portal][azure-portal].</span></span>
2. <span data-ttu-id="b7104-161">Klicka på vänster hello **nätverk**.</span><span class="sxs-lookup"><span data-stu-id="b7104-161">On hello left, click **NETWORKS**.</span></span>
3. <span data-ttu-id="b7104-162">Klicka på hello virtuella nätverk som du har skapat (se [etablera HBase-kluster i Azure Virtual Network][hdinsight-hbase-provision-vnet]).</span><span class="sxs-lookup"><span data-stu-id="b7104-162">Click hello virtual network you have created (see [Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]).</span></span>
4. <span data-ttu-id="b7104-163">Klicka på **konfigurera** hello uppifrån.</span><span class="sxs-lookup"><span data-stu-id="b7104-163">Click **CONFIGURE** from hello top.</span></span>
5. <span data-ttu-id="b7104-164">I hello **punkt-till-plats-anslutning** väljer **konfigurera punkt-till-plats-anslutning**.</span><span class="sxs-lookup"><span data-stu-id="b7104-164">In hello **point-to-site connectivity** section, select **Configure point-to-site connectivity**.</span></span>
6. <span data-ttu-id="b7104-165">Konfigurera **första IP-** och **CIDR** toospecify hello IP-adressintervall från vilka dina VPN-klienter tar emot en IP-adress vid anslutning.</span><span class="sxs-lookup"><span data-stu-id="b7104-165">Configure **STARTING IP** and **CIDR** toospecify hello IP address range from which your VPN clients will receive an IP address when connected.</span></span> <span data-ttu-id="b7104-166">hello-intervallet får inte överlappa med någon av hello adressintervall som finns på ditt lokala nätverk och hello du ska kunna ansluta till virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="b7104-166">hello range cannot overlap with any of hello ranges located on your on-premises network and hello Azure virtual network you will be connecting to.</span></span> <span data-ttu-id="b7104-167">Till exempel.</span><span class="sxs-lookup"><span data-stu-id="b7104-167">For example.</span></span> <span data-ttu-id="b7104-168">Om du har valt 10.0.0.0/20 för hello virtuella nätverk kan du välja 10.1.0.0/24 för hello klientens adressutrymme.</span><span class="sxs-lookup"><span data-stu-id="b7104-168">if you selected 10.0.0.0/20 for hello virtual network, you can select 10.1.0.0/24 for hello client address space.</span></span> <span data-ttu-id="b7104-169">Se hello [punkt-till-plats-anslutning] [ vnet-point-to-site-connectivity] för mer information.</span><span class="sxs-lookup"><span data-stu-id="b7104-169">See hello [Point-To-Site Connectivity][vnet-point-to-site-connectivity] page for more information.</span></span>
7. <span data-ttu-id="b7104-170">På hello virtuellt nätverk adress blanksteg under **Lägg till gateway-undernätet**.</span><span class="sxs-lookup"><span data-stu-id="b7104-170">In hello virtual network address spaces section, click **add gateway subnet**.</span></span>
8. <span data-ttu-id="b7104-171">Klicka på **spara** på hello längst ned på sidan för hello.</span><span class="sxs-lookup"><span data-stu-id="b7104-171">Click **SAVE** on hello bottom of hello page.</span></span>
9. <span data-ttu-id="b7104-172">Klicka på **Ja** tooconfirm hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="b7104-172">Click **YES** tooconfirm hello change.</span></span> <span data-ttu-id="b7104-173">Vänta tills hello system är klar med dina hello ändra innan du fortsätter toohello nästa procedur.</span><span class="sxs-lookup"><span data-stu-id="b7104-173">Wait until hello system has finished making hello change before you proceed toohello next procedure.</span></span>

<span data-ttu-id="b7104-174">**toocreate en dynamisk routning gateway**</span><span class="sxs-lookup"><span data-stu-id="b7104-174">**toocreate a dynamic routing gateway**</span></span>

1. <span data-ttu-id="b7104-175">Hello klassiska Azure-portalen, klicka på **INSTRUMENTPANELEN** från hello hello överst.</span><span class="sxs-lookup"><span data-stu-id="b7104-175">From hello Azure Classic Portal, click **DASHBOARD** from hello top of hello page.</span></span>
2. <span data-ttu-id="b7104-176">Klicka på **skapa GATEWAY** från hello längst ned på sidan för hello.</span><span class="sxs-lookup"><span data-stu-id="b7104-176">Click **CREATE GATEWAY** from hello bottom of hello page.</span></span>
3. <span data-ttu-id="b7104-177">Klicka på **Ja** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="b7104-177">Click **YES** tooconfirm.</span></span> <span data-ttu-id="b7104-178">Vänta tills hello gateway har skapats.</span><span class="sxs-lookup"><span data-stu-id="b7104-178">Wait until hello gateway is created.</span></span>
4. <span data-ttu-id="b7104-179">Klicka på **INSTRUMENTPANELEN** hello uppifrån.</span><span class="sxs-lookup"><span data-stu-id="b7104-179">Click **DASHBOARD** from hello top.</span></span>  <span data-ttu-id="b7104-180">Ett visual diagram över hello virtuella nätverk visas:</span><span class="sxs-lookup"><span data-stu-id="b7104-180">You will see a visual diagram of hello virtual network:</span></span>

    ![Virtuella Azure-nätverket punkt-till-plats-diagram][img-vnet-diagram]

    <span data-ttu-id="b7104-182">hello diagram visar 0-klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="b7104-182">hello diagram shows 0 client connections.</span></span> <span data-ttu-id="b7104-183">När du har gjort ett virtuellt nätverk anslutning toohello blir hello nummer uppdaterade tooone.</span><span class="sxs-lookup"><span data-stu-id="b7104-183">After you make a connection toohello virtual network, hello number will be updated tooone.</span></span>

#### <a name="create-your-certificates"></a><span data-ttu-id="b7104-184">Skapa certifikat</span><span class="sxs-lookup"><span data-stu-id="b7104-184">Create your certificates</span></span>
<span data-ttu-id="b7104-185">Enkelriktade toocreate ett X.509-certifikat är att använda hello certifikat verktyg för att skapa (makecert.exe) som medföljer [Microsoft Visual Studio Express för Windows-skrivbordet](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="b7104-185">One way toocreate an X.509 certificate is by using hello Certificate Creation Tool (makecert.exe) that comes with [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).</span></span>

<span data-ttu-id="b7104-186">**toocreate ett självsignerat rotcertifikat**</span><span class="sxs-lookup"><span data-stu-id="b7104-186">**toocreate a self-signed root certificate**</span></span>

1. <span data-ttu-id="b7104-187">Öppna Kommandotolken från din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="b7104-187">From your workstation, open a command prompt window.</span></span>
2. <span data-ttu-id="b7104-188">Navigera toohello Visual Studio tools mapp.</span><span class="sxs-lookup"><span data-stu-id="b7104-188">Navigate toohello Visual Studio tools folder.</span></span>
3. <span data-ttu-id="b7104-189">hello följande kommando i hello exemplet nedan skapar och installera ett rotcertifikat i hello personliga certifikatarkivet på din arbetsstation och även skapa en motsvarande .cer-fil som du kommer senare överför toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7104-189">hello following command in hello example below will create and install a root certificate in hello Personal certificate store on your workstation and also create a corresponding .cer file that you’ll later upload toohello Azure Classic Portal.</span></span>

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    <span data-ttu-id="b7104-190">Ändra toohello katalog hello .cer-filen toobe finns i, där HBaseVnetVPNRootCertificate är hello namn som du vill toouse för hello certifikatet.</span><span class="sxs-lookup"><span data-stu-id="b7104-190">Change toohello directory that you want hello .cer file toobe located in, where HBaseVnetVPNRootCertificate is hello name that you want toouse for hello certificate.</span></span>

    <span data-ttu-id="b7104-191">Stäng inte hello kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="b7104-191">Don't close hello command prompt.</span></span>  <span data-ttu-id="b7104-192">Du behöver det i hello nästa procedur.</span><span class="sxs-lookup"><span data-stu-id="b7104-192">You will need it in hello next procedure.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b7104-193">Eftersom du har skapat ett rotcertifikat som klientcertifikat genereras, kanske du vill tooexport detta certifikat tillsammans med dess privata nyckel och spara den tooa säker plats där den kan återställas.</span><span class="sxs-lookup"><span data-stu-id="b7104-193">Because you have created a root certificate from which client certificates will be generated, you may want tooexport this certificate along with its private key and save it tooa safe location where it may be recovered.</span></span>
   >
   >

<span data-ttu-id="b7104-194">**toocreate ett klientcertifikat**</span><span class="sxs-lookup"><span data-stu-id="b7104-194">**toocreate a client certificate**</span></span>

* <span data-ttu-id="b7104-195">Från hello samma kommandotolk (den har toobe på hello samma dator där du skapade hello rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="b7104-195">From hello same command prompt (It has toobe on hello same computer where you created hello root certificate.</span></span> <span data-ttu-id="b7104-196">hello klientcertifikatet måste skapas från hello rotcertifikat), kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b7104-196">hello client certificate must be generated from hello root certificate), run hello following command:</span></span>

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    <span data-ttu-id="b7104-197">HBaseVnetVPNRootCertificate är hello rotnamn för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="b7104-197">HBaseVnetVPNRootCertificate is hello root certificate name.</span></span>  <span data-ttu-id="b7104-198">Den har toomatch hello rotnamn certifikat.</span><span class="sxs-lookup"><span data-stu-id="b7104-198">It has toomatch hello root certificate name.</span></span>  

    <span data-ttu-id="b7104-199">Både hello rotcertifikatet och klientcertifikatet hello lagras i ditt personliga certifikatarkiv på datorn.</span><span class="sxs-lookup"><span data-stu-id="b7104-199">Both hello root certificate and hello client certificate are stored in your Personal certificate store on your computer.</span></span> <span data-ttu-id="b7104-200">Använd certmgr.msc tooverify.</span><span class="sxs-lookup"><span data-stu-id="b7104-200">Use certmgr.msc tooverify.</span></span>

    ![Punkt-till-plats VPN-certifikat för virtuella Azure-nätverket][img-certificate]

    <span data-ttu-id="b7104-202">Ett certifikat måste installeras på varje dator som du vill tooconnect toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="b7104-202">A client certificate must be installed on each computer that you want tooconnect toohello virtual network.</span></span> <span data-ttu-id="b7104-203">Vi rekommenderar att du skapar unika certifikat för varje dator som du vill tooconnect toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="b7104-203">We recommend that you create unique client certificates for each computer that you want tooconnect toohello virtual network.</span></span> <span data-ttu-id="b7104-204">tooexport hello klientcertifikat, Använd certmgr.msc.</span><span class="sxs-lookup"><span data-stu-id="b7104-204">tooexport hello client certificates, use certmgr.msc.</span></span>

<span data-ttu-id="b7104-205">**tooupload hello root certificate toohello klassiska Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="b7104-205">**tooupload hello root certificate toohello Azure Classic Portal**</span></span>

1. <span data-ttu-id="b7104-206">Hello klassiska Azure-portalen, klicka på **nätverk** hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b7104-206">From hello Azure Classic Portal, click **NETWORK** on hello left.</span></span>
2. <span data-ttu-id="b7104-207">Klicka på hello virtuella nätverk där din HBase-kluster har distribuerats till.</span><span class="sxs-lookup"><span data-stu-id="b7104-207">Click hello virtual network where your HBase cluster is deployed to.</span></span>
3. <span data-ttu-id="b7104-208">Klicka på **certifikat** hello uppifrån.</span><span class="sxs-lookup"><span data-stu-id="b7104-208">Click **CERTIFICATES** from hello top.</span></span>
4. <span data-ttu-id="b7104-209">Klicka på **överför** från hello nederkanten och ange hello rotcertifikatfilen som du har skapat i hello proceduren före senaste.</span><span class="sxs-lookup"><span data-stu-id="b7104-209">Click **UPLOAD** from hello bottom, and specify hello root certificate file you have created in hello procedure before last.</span></span> <span data-ttu-id="b7104-210">Vänta tills hello certifikatet har importerats.</span><span class="sxs-lookup"><span data-stu-id="b7104-210">Wait until hello certificate got imported.</span></span>
5. <span data-ttu-id="b7104-211">Klicka på **INSTRUMENTPANELEN** hello längst upp.</span><span class="sxs-lookup"><span data-stu-id="b7104-211">Click **DASHBOARD** on hello top.</span></span>  <span data-ttu-id="b7104-212">hello virtuella diagram visar hello status.</span><span class="sxs-lookup"><span data-stu-id="b7104-212">hello virtual diagram shows hello status.</span></span>

#### <a name="configure-your-vpn-client"></a><span data-ttu-id="b7104-213">Konfigurera VPN-klienten</span><span class="sxs-lookup"><span data-stu-id="b7104-213">Configure your VPN client</span></span>
<span data-ttu-id="b7104-214">**toodownload och installera hello VPN-klientpaketet**</span><span class="sxs-lookup"><span data-stu-id="b7104-214">**toodownload and install hello client VPN package**</span></span>

1. <span data-ttu-id="b7104-215">Klicka på antingen från hello INSTRUMENTPANELEN i hello virtuellt nätverk under hello snabböversikten **Download hello 64-bitars VPN-klientpaketet** eller **Download hello 32-bitars VPN-klientpaketet** baserat på din arbetsstation OS-version.</span><span class="sxs-lookup"><span data-stu-id="b7104-215">From hello DASHBOARD page of hello virtual network, in hello quick glance section, click either **Download hello 64-bit Client VPN Package** or **Download hello 32-bit Client VPN Package** based on your workstation OS version.</span></span>
2. <span data-ttu-id="b7104-216">Klicka på **kör** tooinstall hello paketet.</span><span class="sxs-lookup"><span data-stu-id="b7104-216">Click **Run** tooinstall hello package.</span></span>
3. <span data-ttu-id="b7104-217">Hello säkerhetsmeddelande klickar du på **mer info**, och klicka sedan på **kör ändå**.</span><span class="sxs-lookup"><span data-stu-id="b7104-217">At hello security prompt, click **More info**, and then click **Run anyway**.</span></span>
4. <span data-ttu-id="b7104-218">Klicka på **Ja** två gånger.</span><span class="sxs-lookup"><span data-stu-id="b7104-218">Click **Yes** twice.</span></span>

<span data-ttu-id="b7104-219">**tooconnect tooVPN**</span><span class="sxs-lookup"><span data-stu-id="b7104-219">**tooconnect tooVPN**</span></span>

1. <span data-ttu-id="b7104-220">Klicka på hello nätverk-ikonen i Aktivitetsfältet hello hello skrivbordet på din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="b7104-220">On hello desktop of your workstation, click hello Networks icon on hello Task bar.</span></span> <span data-ttu-id="b7104-221">En VPN-anslutning visas med namn på din virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="b7104-221">You shall see a VPN connection with your virtual network name.</span></span>
2. <span data-ttu-id="b7104-222">Klicka på hello VPN-anslutningens namn.</span><span class="sxs-lookup"><span data-stu-id="b7104-222">Click hello VPN connection name.</span></span>
3. <span data-ttu-id="b7104-223">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="b7104-223">Click **Connect**.</span></span>

<span data-ttu-id="b7104-224">**tootest hello namnmatchning för VPN-anslutning och domän**</span><span class="sxs-lookup"><span data-stu-id="b7104-224">**tootest hello VPN connection and domain name resolution**</span></span>

* <span data-ttu-id="b7104-225">Öppna en kommandotolk från hello arbetsstation och pinga en hello följande namn tilldelat hello HBase klustret DNS-suffix är myhbase.b7.internal.cloudapp.net:</span><span class="sxs-lookup"><span data-stu-id="b7104-225">From hello workstation, open a command prompt and ping one of hello following names given hello HBase cluster's DNS suffix is myhbase.b7.internal.cloudapp.net:</span></span>

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a><span data-ttu-id="b7104-226">Installera och konfigurera SQuirreL på din arbetsstation</span><span class="sxs-lookup"><span data-stu-id="b7104-226">Install and configure SQuirreL on your workstation</span></span>
<span data-ttu-id="b7104-227">**tooinstall SQuirreL**</span><span class="sxs-lookup"><span data-stu-id="b7104-227">**tooinstall SQuirreL**</span></span>

1. <span data-ttu-id="b7104-228">Hämta hello SQuirreL SQL klienten jar-filen från [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).</span><span class="sxs-lookup"><span data-stu-id="b7104-228">Download hello SQuirreL SQL client jar file from [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).</span></span>
2. <span data-ttu-id="b7104-229">Öppna/köra hello jar-filen.</span><span class="sxs-lookup"><span data-stu-id="b7104-229">Open/run hello jar file.</span></span> <span data-ttu-id="b7104-230">Det krävs hello [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).</span><span class="sxs-lookup"><span data-stu-id="b7104-230">It requires hello [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).</span></span>
3. <span data-ttu-id="b7104-231">Klicka på **nästa** två gånger.</span><span class="sxs-lookup"><span data-stu-id="b7104-231">Click **Next** twice.</span></span>
4. <span data-ttu-id="b7104-232">Ange en sökväg där du har hello skrivbehörighet och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b7104-232">Specify a path where you have hello write permission, and then click **Next**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="b7104-233">hello standardinstallationsmappen finns i hello C:\Program Files\squirrel-sql-3,6 mapp.</span><span class="sxs-lookup"><span data-stu-id="b7104-233">hello default installation folder is in hello C:\Program Files\squirrel-sql-3.6 folder.</span></span>  <span data-ttu-id="b7104-234">Hello installer måste beviljas hello administratörsbehörighet för i ordning toowrite toothis sökväg.</span><span class="sxs-lookup"><span data-stu-id="b7104-234">In order toowrite toothis path, hello installer must be granted hello administrator privilege.</span></span> <span data-ttu-id="b7104-235">Du kan öppna en kommandotolk som administratör, gå Toojava's bin-mappen och kör sedan:</span><span class="sxs-lookup"><span data-stu-id="b7104-235">You can open a command prompt as administrator, navigate tooJava's bin folder, and then run:</span></span>
  >
  >     <span data-ttu-id="b7104-236">Java.exe-jar [hello SQuirreL jar-filen hello sökväg]</span><span class="sxs-lookup"><span data-stu-id="b7104-236">java.exe -jar [hello path of hello SQuirreL jar file]</span></span>
5. <span data-ttu-id="b7104-237">Klicka på **OK** tooconfirm skapar hello målkatalogen.</span><span class="sxs-lookup"><span data-stu-id="b7104-237">Click **OK** tooconfirm creating hello target directory.</span></span>
6. <span data-ttu-id="b7104-238">hello standardinställningen är tooinstall hello Base och Standard paket.</span><span class="sxs-lookup"><span data-stu-id="b7104-238">hello default setting is tooinstall hello Base and Standard packages.</span></span>  <span data-ttu-id="b7104-239">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b7104-239">Click **Next**.</span></span>
7. <span data-ttu-id="b7104-240">Klicka på **nästa** två gånger, och klicka sedan på **klar**.</span><span class="sxs-lookup"><span data-stu-id="b7104-240">Click **Next** twice, and then click **Done**.</span></span>

<span data-ttu-id="b7104-241">**tooinstall hello Phoenix drivrutin**</span><span class="sxs-lookup"><span data-stu-id="b7104-241">**tooinstall hello Phoenix driver**</span></span>

<span data-ttu-id="b7104-242">hello phoenix drivrutinen jar-filen finns på hello HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="b7104-242">hello phoenix driver jar file is located on hello HBase cluster.</span></span> <span data-ttu-id="b7104-243">hello sökvägen är liknande toohello följande baserat på hello versioner:</span><span class="sxs-lookup"><span data-stu-id="b7104-243">hello path is similar toohello following based on hello versions:</span></span>

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
<span data-ttu-id="b7104-244">Du behöver toocopy den tooyour arbetsstation under hello [SQuirreL installationsmappen] / lib sökväg.</span><span class="sxs-lookup"><span data-stu-id="b7104-244">You need toocopy it tooyour workstation under hello [SQuirreL installation folder]/lib path.</span></span>  <span data-ttu-id="b7104-245">hello enklast tooRDP i hello klustret och Använd fil kopiera och klistra in (CTRL + C och CTRL + V) toocopy den tooyour arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="b7104-245">hello easiest way is tooRDP into hello cluster, and then use file copy/paste (CTRL+C and CTRL+V) toocopy it tooyour workstation.</span></span>

<span data-ttu-id="b7104-246">**tooadd en Phoenix drivrutinen tooSQuirreL**</span><span class="sxs-lookup"><span data-stu-id="b7104-246">**tooadd a Phoenix driver tooSQuirreL**</span></span>

1. <span data-ttu-id="b7104-247">Öppna SQuirreL SQL-klienten från din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="b7104-247">Open SQuirreL SQL Client from your workstation.</span></span>
2. <span data-ttu-id="b7104-248">Klicka på hello **drivrutinen** fliken hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b7104-248">Click hello **Driver** tab on hello left.</span></span>
3. <span data-ttu-id="b7104-249">Från hello **drivrutiner** -menyn klickar du på **ny drivrutin**.</span><span class="sxs-lookup"><span data-stu-id="b7104-249">From hello **Drivers** menu, click **New Driver**.</span></span>
4. <span data-ttu-id="b7104-250">Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="b7104-250">Enter hello following information:</span></span>

   * <span data-ttu-id="b7104-251">**Namnet**: Phoenix</span><span class="sxs-lookup"><span data-stu-id="b7104-251">**Name**: Phoenix</span></span>
   * <span data-ttu-id="b7104-252">**Exempel-URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="b7104-252">**Example URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net</span></span>
   * <span data-ttu-id="b7104-253">**Klassnamn**: org.apache.phoenix.jdbc.PhoenixDriver</span><span class="sxs-lookup"><span data-stu-id="b7104-253">**Class Name**: org.apache.phoenix.jdbc.PhoenixDriver</span></span>

     > [!WARNING]
     > <span data-ttu-id="b7104-254">Användaren alla med gemener hello exempel-URL.</span><span class="sxs-lookup"><span data-stu-id="b7104-254">User all lower case in hello Example URL.</span></span> <span data-ttu-id="b7104-255">Du kan använda de fullständiga zookeeper kvorum om en av dem är nere.</span><span class="sxs-lookup"><span data-stu-id="b7104-255">You can use they full zookeeper quorum in case one of them is down.</span></span>  <span data-ttu-id="b7104-256">hello värdnamn är zookeeper0, zookeeper1 och zookeeper2.</span><span class="sxs-lookup"><span data-stu-id="b7104-256">hello hostnames are zookeeper0, zookeeper1, and zookeeper2.</span></span>
     >
     >

     ![HDInsight HBase Phoenix SQuirreL drivrutin][img-squirrel-driver]
5. <span data-ttu-id="b7104-258">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b7104-258">Click **OK**.</span></span>

<span data-ttu-id="b7104-259">**toocreate ett alias toohello HBase-kluster**</span><span class="sxs-lookup"><span data-stu-id="b7104-259">**toocreate an alias toohello HBase cluster**</span></span>

1. <span data-ttu-id="b7104-260">Klicka på hello SQuirreL, **alias** fliken hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b7104-260">From SQuirreL, Click hello **Aliases** tab on hello left.</span></span>
2. <span data-ttu-id="b7104-261">Från hello **alias** -menyn klickar du på **nytt Alias**.</span><span class="sxs-lookup"><span data-stu-id="b7104-261">From hello **Aliases** menu, click **New Alias**.</span></span>
3. <span data-ttu-id="b7104-262">Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="b7104-262">Enter hello following information:</span></span>

   * <span data-ttu-id="b7104-263">**Namnet**: hello namnet på hello HBase-kluster eller ett namn som du föredrar.</span><span class="sxs-lookup"><span data-stu-id="b7104-263">**Name**: hello name of hello HBase cluster or any name you prefer.</span></span>
   * <span data-ttu-id="b7104-264">**Drivrutinen**: Phoenix.</span><span class="sxs-lookup"><span data-stu-id="b7104-264">**Driver**: Phoenix.</span></span>  <span data-ttu-id="b7104-265">Detta måste matcha hello drivrutinsnamn som du skapade i föregående procedur för hello.</span><span class="sxs-lookup"><span data-stu-id="b7104-265">This must match hello driver name you created in hello last procedure.</span></span>
   * <span data-ttu-id="b7104-266">**URL: en**: hello URL kopieras från konfigurationen av drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="b7104-266">**URL**: hello URL is copied from your driver configuration.</span></span> <span data-ttu-id="b7104-267">Se till att toouser alla gemen.</span><span class="sxs-lookup"><span data-stu-id="b7104-267">Make sure toouser all lower case.</span></span>
   * <span data-ttu-id="b7104-268">**Användarnamnet**: Det kan vara text.</span><span class="sxs-lookup"><span data-stu-id="b7104-268">**User name**: It can be any text.</span></span>  <span data-ttu-id="b7104-269">Eftersom du använder VPN-anslutningar här används inte hello användarnamn alls.</span><span class="sxs-lookup"><span data-stu-id="b7104-269">Because you use VPN connectivity here, hello user name is not used at all.</span></span>
   * <span data-ttu-id="b7104-270">**Lösenordet**: Det kan vara text.</span><span class="sxs-lookup"><span data-stu-id="b7104-270">**Password**: It can be any text.</span></span>

     ![HDInsight HBase Phoenix SQuirreL drivrutin][img-squirrel-alias]
4. <span data-ttu-id="b7104-272">Klicka på **Test**.</span><span class="sxs-lookup"><span data-stu-id="b7104-272">Click **Test**.</span></span>
5. <span data-ttu-id="b7104-273">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="b7104-273">Click **Connect**.</span></span> <span data-ttu-id="b7104-274">När den gör hello anslutning SQuirreL ser ut som:</span><span class="sxs-lookup"><span data-stu-id="b7104-274">When it makes hello connection, SQuirreL looks like:</span></span>

    ![HBase Phoenix SQuirreL][img-squirrel]

<span data-ttu-id="b7104-276">**toorun ett test**</span><span class="sxs-lookup"><span data-stu-id="b7104-276">**toorun a test**</span></span>

1. <span data-ttu-id="b7104-277">Klicka på hello **SQL** fliken höger nästa toohello **objekt** fliken.</span><span class="sxs-lookup"><span data-stu-id="b7104-277">Click hello **SQL** tab right next toohello **Objects** tab.</span></span>
2. <span data-ttu-id="b7104-278">Kopiera och klistra in följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="b7104-278">Copy and paste hello following code:</span></span>

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. <span data-ttu-id="b7104-279">Klicka hello kör.</span><span class="sxs-lookup"><span data-stu-id="b7104-279">Click hello run button.</span></span>

    ![HBase Phoenix SQuirreL][img-squirrel-sql]
4. <span data-ttu-id="b7104-281">Växla tillbaka toohello **objekt** fliken.</span><span class="sxs-lookup"><span data-stu-id="b7104-281">Switch back toohello **Objects** tab.</span></span>
5. <span data-ttu-id="b7104-282">Expandera hello aliasnamn och expandera sedan **tabellen**.</span><span class="sxs-lookup"><span data-stu-id="b7104-282">Expand hello alias name, and then expand **TABLE**.</span></span>  <span data-ttu-id="b7104-283">Du bör se hello ny tabell visas under.</span><span class="sxs-lookup"><span data-stu-id="b7104-283">You shall see hello new table listed under.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7104-284">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7104-284">Next steps</span></span>
<span data-ttu-id="b7104-285">I den här artikeln har du lärt dig hur toouse Apache Phoenix i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b7104-285">In this article, you have learned how toouse Apache Phoenix in HDInsight.</span></span>  <span data-ttu-id="b7104-286">Det finns fler toolearn</span><span class="sxs-lookup"><span data-stu-id="b7104-286">toolearn more, see</span></span>

* <span data-ttu-id="b7104-287">[HDInsight HBase-översikt][hdinsight-hbase-overview]: HBase är en NoSQL-databas av Apachetyp med öppen källkod som bygger på Hadoop och som ger direktåtkomst och stark konsekvens för stora mängder ostrukturerade och semistrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="b7104-287">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>
* <span data-ttu-id="b7104-288">[Etablera HBase-kluster i Azure Virtual Network][hdinsight-hbase-provision-vnet]: med virtuell nätverksintegration HBase-kluster kan vara distribuerade toohello samma virtuella nätverk som dina program så att program kan kommunicera med HBase direkt.</span><span class="sxs-lookup"><span data-stu-id="b7104-288">[Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]: With virtual network integration, HBase clusters can be deployed toohello same virtual network as your applications so that applications can communicate with HBase directly.</span></span>
* <span data-ttu-id="b7104-289">[Konfigurera HBase-replikering i HDInsight](hdinsight-hbase-replication.md): Lär dig hur tooconfigure HBase-replikering mellan två Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="b7104-289">[Configure HBase replication in HDInsight](hdinsight-hbase-replication.md): Learn how tooconfigure HBase replication across two Azure datacenters.</span></span>
* <span data-ttu-id="b7104-290">[Analysera Twitter-åsikter med HBase i HDInsight][hbase-twitter-sentiment]: Lär dig hur toodo realtid [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) av stordata med HBase i ett Hadoop-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b7104-290">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]: Learn how toodo real-time [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) of big data by using HBase in a Hadoop cluster in HDInsight.</span></span>

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
