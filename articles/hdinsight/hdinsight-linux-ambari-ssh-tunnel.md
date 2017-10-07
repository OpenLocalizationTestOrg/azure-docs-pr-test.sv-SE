---
title: aaaUse SSH tunnlar tooaccess Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse en SSH-tunnel toosecurely Bläddra webbresurser finns på Linux-baserat HDInsight-noder."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 879834a4-52d0-499c-a3ae-8d28863abf65
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 8a315c53751bbe6950a182674f4108c67c2f2f8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-ssh-tunneling-tooaccess-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a><span data-ttu-id="4a758-103">Använda SSH-tunnlar tooaccess Ambari-webbgränssnittet, jobbhistorik, NameNode, Oozie och andra webb-användargränssnitt</span><span class="sxs-lookup"><span data-stu-id="4a758-103">Use SSH Tunneling tooaccess Ambari web UI, JobHistory, NameNode, Oozie, and other web UIs</span></span>

<span data-ttu-id="4a758-104">Linux-baserade HDInsight-kluster ger åtkomst tooAmbari webbgränssnittet över hello Internet, men vissa funktioner i hello Användargränssnittet är inte.</span><span class="sxs-lookup"><span data-stu-id="4a758-104">Linux-based HDInsight clusters provide access tooAmbari web UI over hello Internet, but some features of hello UI are not.</span></span> <span data-ttu-id="4a758-105">Till exempel hello webbgränssnittet för andra tjänster som är anslutna via Ambari.</span><span class="sxs-lookup"><span data-stu-id="4a758-105">For example, hello web UI for other services that are surfaced through Ambari.</span></span> <span data-ttu-id="4a758-106">Du måste använda en SSH-tunnel toohello klustret head full funktionalitet för hello Ambari-webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="4a758-106">For full functionality of hello Ambari web UI, you must use an SSH tunnel toohello cluster head.</span></span>

## <a name="why-use-an-ssh-tunnel"></a><span data-ttu-id="4a758-107">Varför ska jag använda en SSH-tunnel</span><span class="sxs-lookup"><span data-stu-id="4a758-107">Why use an SSH tunnel</span></span>

<span data-ttu-id="4a758-108">Flera hello menyer i Ambari fungerar bara med en SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="4a758-108">Several of hello menus in Ambari only work through an SSH tunnel.</span></span> <span data-ttu-id="4a758-109">Dessa menyer förlitar sig på webbplatser och tjänster som körs på andra nodtyper, till exempel arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="4a758-109">These menus rely on web sites and services running on other node types, such as worker nodes.</span></span> <span data-ttu-id="4a758-110">Ofta webbplatserna skyddas inte, så det inte är säker toodirectly exponera hello dem på internet.</span><span class="sxs-lookup"><span data-stu-id="4a758-110">Often, these web sites are not secured, so it is not safe toodirectly expose them on hello internet.</span></span>

<span data-ttu-id="4a758-111">hello följande Web användargränssnitt kräver en SSH-tunnel:</span><span class="sxs-lookup"><span data-stu-id="4a758-111">hello following Web UIs require an SSH tunnel:</span></span>

* <span data-ttu-id="4a758-112">Jobbhistorik</span><span class="sxs-lookup"><span data-stu-id="4a758-112">JobHistory</span></span>
* <span data-ttu-id="4a758-113">NameNode</span><span class="sxs-lookup"><span data-stu-id="4a758-113">NameNode</span></span>
* <span data-ttu-id="4a758-114">Trådstackar</span><span class="sxs-lookup"><span data-stu-id="4a758-114">Thread Stacks</span></span>
* <span data-ttu-id="4a758-115">Oozie webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="4a758-115">Oozie web UI</span></span>
* <span data-ttu-id="4a758-116">HBase-huvud- och loggar UI</span><span class="sxs-lookup"><span data-stu-id="4a758-116">HBase Master and Logs UI</span></span>

<span data-ttu-id="4a758-117">Om du använder skriptåtgärder toocustomize klustret kräver någon tjänst eller ett verktyg som du installerar som Exponerar en webbgränssnittet en SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="4a758-117">If you use Script Actions toocustomize your cluster, any services or utilities that you install that expose a web UI require an SSH tunnel.</span></span> <span data-ttu-id="4a758-118">Om du installerar Hue med hjälp av en skriptåtgärd, måste du använda en SSH-tunnel tooaccess hello Hue-webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="4a758-118">For example, if you install Hue using a Script Action, you must use an SSH tunnel tooaccess hello Hue web UI.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a758-119">Om du har direktåtkomst tooHDInsight via ett virtuellt nätverk kan behöver du inte toouse SSH-tunnlar.</span><span class="sxs-lookup"><span data-stu-id="4a758-119">If you have direct access tooHDInsight through a virtual network, you do not need toouse SSH tunnels.</span></span> <span data-ttu-id="4a758-120">Ett exempel på direkt åtkomst till HDInsight via ett virtuellt nätverk, se hello [ansluta HDInsight tooyour lokalt nätverk](connect-on-premises-network.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="4a758-120">For an example of directly accessing HDInsight through a virtual network, see hello [Connect HDInsight tooyour on-premises network](connect-on-premises-network.md) document.</span></span>

## <a name="what-is-an-ssh-tunnel"></a><span data-ttu-id="4a758-121">Vad är en SSH-tunnel</span><span class="sxs-lookup"><span data-stu-id="4a758-121">What is an SSH tunnel</span></span>

<span data-ttu-id="4a758-122">[Secure Shell (SSH) tunneling](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) dirigerar trafik som skickas tooa port på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="4a758-122">[Secure Shell (SSH) tunneling](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) routes traffic sent tooa port on your local workstation.</span></span> <span data-ttu-id="4a758-123">hello trafik dirigeras via en SSH-anslutning tooyour HDInsight-klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4a758-123">hello traffic is routed through an SSH connection tooyour HDInsight cluster head node.</span></span> <span data-ttu-id="4a758-124">hello-begäran har lösts som om den har sitt ursprung i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4a758-124">hello request is resolved as if it originated on hello head node.</span></span> <span data-ttu-id="4a758-125">hello svar dirigeras sedan tillbaka via hello tunnel tooyour arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="4a758-125">hello response is then routed back through hello tunnel tooyour workstation.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a758-126">Krav</span><span class="sxs-lookup"><span data-stu-id="4a758-126">Prerequisites</span></span>

* <span data-ttu-id="4a758-127">En SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="4a758-127">An SSH client.</span></span> <span data-ttu-id="4a758-128">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="4a758-128">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="4a758-129">En webbläsare som kan vara konfigurerad toouse SOCKS5 proxy.</span><span class="sxs-lookup"><span data-stu-id="4a758-129">A web browser that can be configured toouse a SOCKS5 proxy.</span></span>

    > [!WARNING]
    > <span data-ttu-id="4a758-130">inbyggd i Windows hello SOCKS proxy stöd stöder inte SOCKS5 och fungerar inte med hello stegen i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="4a758-130">hello SOCKS proxy support built into Windows does not support SOCKS5, and does not work with hello steps in this document.</span></span> <span data-ttu-id="4a758-131">hello följande webbläsare förlitar sig på Windows-proxyinställningar och inte för närvarande fungerar med hello stegen i det här dokumentet:</span><span class="sxs-lookup"><span data-stu-id="4a758-131">hello following browsers rely on Windows proxy settings, and do not currently work with hello steps in this document:</span></span>
    >
    > * <span data-ttu-id="4a758-132">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="4a758-132">Microsoft Edge</span></span>
    > * <span data-ttu-id="4a758-133">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="4a758-133">Microsoft Internet Explorer</span></span>
    >
    > <span data-ttu-id="4a758-134">Google Chrome använder också Windows hello för proxy-inställningar.</span><span class="sxs-lookup"><span data-stu-id="4a758-134">Google Chrome also relies on hello Windows proxy settings.</span></span> <span data-ttu-id="4a758-135">Du kan dock installera tillägg som stöder SOCKS5.</span><span class="sxs-lookup"><span data-stu-id="4a758-135">However, you can install extensions that support SOCKS5.</span></span> <span data-ttu-id="4a758-136">Vi rekommenderar [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).</span><span class="sxs-lookup"><span data-stu-id="4a758-136">We recommend [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).</span></span>

## <span data-ttu-id="4a758-137"><a name="usessh"></a>Skapa en tunnel med hello SSH-kommando</span><span class="sxs-lookup"><span data-stu-id="4a758-137"><a name="usessh"></a>Create a tunnel using hello SSH command</span></span>

<span data-ttu-id="4a758-138">Använd hello följande kommando toocreate en SSH-tunnel med hello `ssh` kommando.</span><span class="sxs-lookup"><span data-stu-id="4a758-138">Use hello following command toocreate an SSH tunnel using hello `ssh` command.</span></span> <span data-ttu-id="4a758-139">Ersätt **användarnamn** med en SSH-användare för din HDInsight-kluster och Ersätt **KLUSTERNAMN** med hello namnet på ditt HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="4a758-139">Replace **USERNAME** with an SSH user for your HDInsight cluster, and replace **CLUSTERNAME** with hello name of your HDInsight cluster:</span></span>

```bash
ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
```

<span data-ttu-id="4a758-140">Detta kommando skapar en anslutning som dirigerar trafik toolocal port 9876 toohello klustret via SSH.</span><span class="sxs-lookup"><span data-stu-id="4a758-140">This command creates a connection that routes traffic toolocal port 9876 toohello cluster over SSH.</span></span> <span data-ttu-id="4a758-141">hello alternativ är:</span><span class="sxs-lookup"><span data-stu-id="4a758-141">hello options are:</span></span>

* <span data-ttu-id="4a758-142">**D 9876** -hello lokal port som dirigerar trafik via hello-tunnel.</span><span class="sxs-lookup"><span data-stu-id="4a758-142">**D 9876** - hello local port that routes traffic through hello tunnel.</span></span>
* <span data-ttu-id="4a758-143">**C** -Komprimera alla data eftersom webbtrafik är främst text.</span><span class="sxs-lookup"><span data-stu-id="4a758-143">**C** - Compress all data, because web traffic is mostly text.</span></span>
* <span data-ttu-id="4a758-144">**2** -force SSH tootry-protokollversion 2 endast.</span><span class="sxs-lookup"><span data-stu-id="4a758-144">**2** - Force SSH tootry protocol version 2 only.</span></span>
* <span data-ttu-id="4a758-145">**q** -tyst läge.</span><span class="sxs-lookup"><span data-stu-id="4a758-145">**q** - Quiet mode.</span></span>
* <span data-ttu-id="4a758-146">**T** -inaktivera pseudokolumner tty-allokering, eftersom vi bara vidarebefordrar en port.</span><span class="sxs-lookup"><span data-stu-id="4a758-146">**T** - Disable pseudo-tty allocation, since we are just forwarding a port.</span></span>
* <span data-ttu-id="4a758-147">**n**-Förhindra läsningen av STDIN, eftersom vi bara vidarebefordrar en port.</span><span class="sxs-lookup"><span data-stu-id="4a758-147">**n** - Prevent reading of STDIN, since we are just forwarding a port.</span></span>
* <span data-ttu-id="4a758-148">**N** -inte köra fjärrkommandon, eftersom vi bara vidarebefordrar en port.</span><span class="sxs-lookup"><span data-stu-id="4a758-148">**N** - Do not execute a remote command, since we are just forwarding a port.</span></span>
* <span data-ttu-id="4a758-149">**f** -körs i bakgrunden hello.</span><span class="sxs-lookup"><span data-stu-id="4a758-149">**f** - Run in hello background.</span></span>

<span data-ttu-id="4a758-150">När hello-kommandot har slutförts, är trafik som skickas tooport 9876 på hello lokal dator routade toohello klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4a758-150">Once hello command finishes, traffic sent tooport 9876 on hello local computer is routed toohello cluster head node.</span></span>

## <span data-ttu-id="4a758-151"><a name="useputty"></a>Skapa en tunnel med PuTTY</span><span class="sxs-lookup"><span data-stu-id="4a758-151"><a name="useputty"></a>Create a tunnel using PuTTY</span></span>

<span data-ttu-id="4a758-152">[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) är en grafisk SSH-klient för Windows.</span><span class="sxs-lookup"><span data-stu-id="4a758-152">[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) is a graphical SSH client for Windows.</span></span> <span data-ttu-id="4a758-153">Använd hello följande steg toocreate en SSH-tunnel med PuTTY:</span><span class="sxs-lookup"><span data-stu-id="4a758-153">Use hello following steps toocreate an SSH tunnel using PuTTY:</span></span>

1. <span data-ttu-id="4a758-154">Öppna PuTTY och ange anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="4a758-154">Open PuTTY, and enter your connection information.</span></span> <span data-ttu-id="4a758-155">Om du inte är bekant med PuTTY finns hello [PuTTY-dokumentation (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).</span><span class="sxs-lookup"><span data-stu-id="4a758-155">If you are not familiar with PuTTY, see hello [PuTTY documentation (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).</span></span>

2. <span data-ttu-id="4a758-156">I hello **kategori** avsnitt toohello till vänster i dialogrutan hello, expandera **anslutning**, expandera **SSH**, och välj sedan **tunnlar**.</span><span class="sxs-lookup"><span data-stu-id="4a758-156">In hello **Category** section toohello left of hello dialog, expand **Connection**, expand **SSH**, and then select **Tunnels**.</span></span>

3. <span data-ttu-id="4a758-157">Ange följande information på hello hello **alternativ som styr SSH-port vidarebefordran** formulär:</span><span class="sxs-lookup"><span data-stu-id="4a758-157">Provide hello following information on hello **Options controlling SSH port forwarding** form:</span></span>
   
   * <span data-ttu-id="4a758-158">**Källport** -hello port på hello-klient som du vill tooforward.</span><span class="sxs-lookup"><span data-stu-id="4a758-158">**Source port** - hello port on hello client that you wish tooforward.</span></span> <span data-ttu-id="4a758-159">Till exempel **9876**.</span><span class="sxs-lookup"><span data-stu-id="4a758-159">For example, **9876**.</span></span>

   * <span data-ttu-id="4a758-160">**Mål** -hello SSH-adressen för hello Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4a758-160">**Destination** - hello SSH address for hello Linux-based HDInsight cluster.</span></span> <span data-ttu-id="4a758-161">Till exempel **mycluster-ssh.azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="4a758-161">For example, **mycluster-ssh.azurehdinsight.net**.</span></span>

   * <span data-ttu-id="4a758-162">**Dynamisk** – Aktiverar dynamisk SOCKS-proxyroutning.</span><span class="sxs-lookup"><span data-stu-id="4a758-162">**Dynamic** - Enables dynamic SOCKS proxy routing.</span></span>
     
     ![Bild av tunneling alternativ](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. <span data-ttu-id="4a758-164">Klicka på **Lägg till** tooadd hello inställningar och klickar sedan på **öppna** tooopen en SSH-anslutning.</span><span class="sxs-lookup"><span data-stu-id="4a758-164">Click **Add** tooadd hello settings, and then click **Open** tooopen an SSH connection.</span></span>

5. <span data-ttu-id="4a758-165">När du uppmanas att logga in toohello server.</span><span class="sxs-lookup"><span data-stu-id="4a758-165">When prompted, log in toohello server.</span></span>

## <a name="use-hello-tunnel-from-your-browser"></a><span data-ttu-id="4a758-166">Använd hello tunnel från din webbläsare</span><span class="sxs-lookup"><span data-stu-id="4a758-166">Use hello tunnel from your browser</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a758-167">hello stegen i det här avsnittet Använd hello Mozilla FireFox webbläsaren, eftersom det ger hello samma proxyinställningar på alla plattformar.</span><span class="sxs-lookup"><span data-stu-id="4a758-167">hello steps in this section use hello Mozilla FireFox browser, as it provides hello same proxy settings across all platforms.</span></span> <span data-ttu-id="4a758-168">Andra moderna webbläsare, till exempel Google Chrome kan kräva ett tillägg, till exempel FoxyProxy toowork med hello-tunnel.</span><span class="sxs-lookup"><span data-stu-id="4a758-168">Other modern browsers, such as Google Chrome, may require an extension such as FoxyProxy toowork with hello tunnel.</span></span>

1. <span data-ttu-id="4a758-169">Konfigurera hello webbläsare toouse **localhost** och hello-port som du använde när du skapar hello tunnel som en **SOCKS v5** proxy.</span><span class="sxs-lookup"><span data-stu-id="4a758-169">Configure hello browser toouse **localhost** and hello port you used when creating hello tunnel as a **SOCKS v5** proxy.</span></span> <span data-ttu-id="4a758-170">Här är hur hello Firefox inställningar se ut.</span><span class="sxs-lookup"><span data-stu-id="4a758-170">Here's what hello Firefox settings look like.</span></span> <span data-ttu-id="4a758-171">Om du använder en annan port än 9876, ändra hello port toohello du används:</span><span class="sxs-lookup"><span data-stu-id="4a758-171">If you used a different port than 9876, change hello port toohello one you used:</span></span>
   
    ![Bild av Firefox inställningar](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > <span data-ttu-id="4a758-173">Att välja **fjärr-DNS** löser Domain Name System (DNS)-begäranden med hjälp av hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4a758-173">Selecting **Remote DNS** resolves Domain Name System (DNS) requests by using hello HDInsight cluster.</span></span> <span data-ttu-id="4a758-174">Den här inställningen löser DNS med hello huvudnod hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="4a758-174">This setting resolves DNS using hello head node of hello cluster.</span></span>

2. <span data-ttu-id="4a758-175">Kontrollera att hello-tunnel fungerar genom att gå till en plats som [http://www.whatismyip.com/](http://www.whatismyip.com/).</span><span class="sxs-lookup"><span data-stu-id="4a758-175">Verify that hello tunnel works by visiting a site such as [http://www.whatismyip.com/](http://www.whatismyip.com/).</span></span> <span data-ttu-id="4a758-176">hello IP returnerade något som användas av hello Microsoft Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="4a758-176">hello IP returned should be one used by hello Microsoft Azure datacenter.</span></span>

## <a name="verify-with-ambari-web-ui"></a><span data-ttu-id="4a758-177">Kontrollera med Ambari-webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="4a758-177">Verify with Ambari web UI</span></span>

<span data-ttu-id="4a758-178">När hello kluster har upprättats använder du följande steg tooverify att du kan komma åt tjänsten web användargränssnitt från hello Ambari Web hello:</span><span class="sxs-lookup"><span data-stu-id="4a758-178">Once hello cluster has been established, use hello following steps tooverify that you can access service web UIs from hello Ambari Web:</span></span>

1. <span data-ttu-id="4a758-179">Gå toohttp://headnodehost:8080 i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="4a758-179">In your browser, go toohttp://headnodehost:8080.</span></span> <span data-ttu-id="4a758-180">Hej `headnodehost` adress skickas över hello tunnel toohello klustret och lösa toohello headnode som Ambari körs på.</span><span class="sxs-lookup"><span data-stu-id="4a758-180">hello `headnodehost` address is sent over hello tunnel toohello cluster and resolve toohello headnode that Ambari is running on.</span></span> <span data-ttu-id="4a758-181">När du uppmanas ange hello administratörsanvändarnamnet (admin) och lösenord för klustret.</span><span class="sxs-lookup"><span data-stu-id="4a758-181">When prompted, enter hello admin user name (admin) and password for your cluster.</span></span> <span data-ttu-id="4a758-182">Du kan uppmanas en gång av hello Ambari-webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="4a758-182">You may be prompted a second time by hello Ambari web UI.</span></span> <span data-ttu-id="4a758-183">I så fall, ange hello information.</span><span class="sxs-lookup"><span data-stu-id="4a758-183">If so, reenter hello information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4a758-184">När du använder hello http://headnodehost:8080 adress tooconnect toohello klustret måste ansluter du via hello-tunnel.</span><span class="sxs-lookup"><span data-stu-id="4a758-184">When using hello http://headnodehost:8080 address tooconnect toohello cluster, you are connecting through hello tunnel.</span></span> <span data-ttu-id="4a758-185">Kommunikationen säkras med hello SSH-tunnel i stället för HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4a758-185">Communication is secured using hello SSH tunnel instead of HTTPS.</span></span> <span data-ttu-id="4a758-186">tooconnect över Hej internet med hjälp av HTTPS, använder https://CLUSTERNAME.azurehdinsight.net, där **KLUSTERNAMN** är hello hello klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="4a758-186">tooconnect over hello internet using HTTPS, use https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of hello cluster.</span></span>

2. <span data-ttu-id="4a758-187">Välj HDFS hello listan hello vänster på sidan hello hello Ambari-Webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="4a758-187">From hello Ambari Web UI, select HDFS from hello list on hello left of hello page.</span></span>

    ![Bild med HDFS som valts](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. <span data-ttu-id="4a758-189">När hello HDFS tjänstinformation visas, väljer du **snabblänkar**.</span><span class="sxs-lookup"><span data-stu-id="4a758-189">When hello HDFS service information is displayed, select **Quick Links**.</span></span> <span data-ttu-id="4a758-190">En lista över hello-klustrets huvudnoder visas.</span><span class="sxs-lookup"><span data-stu-id="4a758-190">A list of hello cluster head nodes appears.</span></span> <span data-ttu-id="4a758-191">Välj en av hello huvudnoderna och välj sedan **NameNode UI**.</span><span class="sxs-lookup"><span data-stu-id="4a758-191">Select one of hello head nodes, and then select **NameNode UI**.</span></span>

    ![Bild med hello snabblänkar menyn expanderas](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]
   > <span data-ttu-id="4a758-193">När du väljer __snabblänkar__, kan du få en indikator för vänta.</span><span class="sxs-lookup"><span data-stu-id="4a758-193">When you select __Quick Links__, you may get a wait indicator.</span></span> <span data-ttu-id="4a758-194">Det här tillståndet kan uppstå om du har en långsam Internetanslutning.</span><span class="sxs-lookup"><span data-stu-id="4a758-194">This condition can occur if you have a slow internet connection.</span></span> <span data-ttu-id="4a758-195">Vänta en minut eller två för hello data toobe togs emot från hello-servern och försök sedan hello listan igen.</span><span class="sxs-lookup"><span data-stu-id="4a758-195">Wait a minute or two for hello data toobe received from hello server, then try hello list again.</span></span>
   >
   > <span data-ttu-id="4a758-196">Vissa poster i hello **snabblänkar** menyn kan vara kapa av hello höger sida av hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="4a758-196">Some entries in hello **Quick Links** menu may be cut off by hello right side of hello screen.</span></span> <span data-ttu-id="4a758-197">I så fall, expandera hello-menyn med hjälp av musen och använda hello HÖGERPIL viktiga tooscroll hello skärmen toohello rätt toosee hello resten av hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="4a758-197">If so, expand hello menu using your mouse and use hello right arrow key tooscroll hello screen toohello right toosee hello rest of hello menu.</span></span>

4. <span data-ttu-id="4a758-198">En sida liknande toohello följande bild visas:</span><span class="sxs-lookup"><span data-stu-id="4a758-198">A page similar toohello following image is displayed:</span></span>

    ![Bild av hello NameNode UI](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]
   > <span data-ttu-id="4a758-200">Lägg märke till hello URL för den här sidan. Det bör likna för**http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/kluster**.</span><span class="sxs-lookup"><span data-stu-id="4a758-200">Notice hello URL for this page; it should be similar too**http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/cluster**.</span></span> <span data-ttu-id="4a758-201">URI: N använder hello interna fullständigt kvalificerade domännamnet (FQDN) för hello nod och är bara tillgänglig när du använder en SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="4a758-201">This URI is using hello internal fully qualified domain name (FQDN) of hello node, and is only accessible when using an SSH tunnel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a758-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a758-202">Next steps</span></span>

<span data-ttu-id="4a758-203">Nu när du har lärt dig hur toocreate och använder en SSH-tunnel finns i följande dokument för andra sätt toouse Ambari hello:</span><span class="sxs-lookup"><span data-stu-id="4a758-203">Now that you have learned how toocreate and use an SSH tunnel, see hello following document for other ways toouse Ambari:</span></span>

* [<span data-ttu-id="4a758-204">Hantera HDInsight-kluster med Ambari</span><span class="sxs-lookup"><span data-stu-id="4a758-204">Manage HDInsight clusters by using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)

<span data-ttu-id="4a758-205">Mer information om hur du använder SSH med HDInsight finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="4a758-205">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

