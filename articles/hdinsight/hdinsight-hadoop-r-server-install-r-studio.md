---
title: "aaaInstall RStudio med R Server på HDInsight - Azure | Microsoft Docs"
description: "Hur tooinstall RStudio med R Server på HDInsight."
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 918abb0d-8248-4bc5-98dc-089c0e007d49
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: b3a23021fcf99217e8f551f8b2e89bf1f1e5b967
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a><span data-ttu-id="4500e-103">Installera RStudio med R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="4500e-103">Installing RStudio with R Server on HDInsight</span></span>

<span data-ttu-id="4500e-104">Den här artikeln beskriver hur tooinstall hello community (kostnadsfritt) version av [RStudio Server](https://www.rstudio.com/products/rstudio-server/) på hello edge nod i ett kluster med ett anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="4500e-104">This article describes how tooinstall hello community (free) version of [RStudio Server](https://www.rstudio.com/products/rstudio-server/) on hello edge node of a cluster using a custom script.</span></span> <span data-ttu-id="4500e-105">RStudio Server innehåller en webbläsarbaserad IDE för användning av fjärranslutna klienter och som används ofta i Linux.</span><span class="sxs-lookup"><span data-stu-id="4500e-105">RStudio Server provides a browser-based IDE for use by remote clients and is widely used on Linux.</span></span> <span data-ttu-id="4500e-106">Det finns flera integrerad utvecklingsmiljöer (IDEs) för R idag, inklusive:</span><span class="sxs-lookup"><span data-stu-id="4500e-106">There are multiple integrated development environments (IDEs) available for R today, including:</span></span>

- <span data-ttu-id="4500e-107">Microsofts [R Tools för Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span><span class="sxs-lookup"><span data-stu-id="4500e-107">Microsoft’s  [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span></span> 
- [<span data-ttu-id="4500e-108">RStudio Server</span><span class="sxs-lookup"><span data-stu-id="4500e-108">RStudio Server</span></span>](https://www.rstudio.com/products/rstudio-server/) 
- <span data-ttu-id="4500e-109">Walware har Eclipse-baserade [StatET](http://www.walware.de/goto/statet).</span><span class="sxs-lookup"><span data-stu-id="4500e-109">Walware’s Eclipse-based [StatET](http://www.walware.de/goto/statet).</span></span>

<span data-ttu-id="4500e-110">hello fördelen att installera RStudio på hello kantnod för ett HDInsight-kluster är att det ger en fullständig IDE-upplevelse för hello utveckling och körning av R-skript med R Server på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="4500e-110">hello advantage of installing RStudio Server on hello edge node of an HDInsight cluster is that it provides a full IDE experience for hello development and execution of R scripts with R Server on hello cluster.</span></span> <span data-ttu-id="4500e-111">Den här konfigurationen kan vara betydligt mer effektiva än standardinställningen för användning av hello R-konsolen.</span><span class="sxs-lookup"><span data-stu-id="4500e-111">This configuration can be considerably more productive than default use of hello R Console.</span></span>

> [!NOTE]
> <span data-ttu-id="4500e-112">hello proceduren som beskrivs i den här artikeln gäller endast om du inte valde tooinstall RStudio Server community edition vid etablering av klustret.</span><span class="sxs-lookup"><span data-stu-id="4500e-112">hello procedure described in this article is only relevant if you did not select tooinstall RStudio Server community edition when provisioning your cluster.</span></span> <span data-ttu-id="4500e-113">Om du lade till den under etableringen, sedan tooaccess det du klickar på hello **R Server instrumentpaneler** panelen i hello Azure portal post för klustret och sedan på hello **R Studio Server** panelen.</span><span class="sxs-lookup"><span data-stu-id="4500e-113">If you added it during provisioning, then tooaccess it you click on hello **R Server Dashboards** tile in hello Azure portal entry for your cluster, then on hello **R Studio Server** tile.</span></span> 

<span data-ttu-id="4500e-114">Om du föredrar toouse hello kommersiellt licensierad Pro version av RStudio Server, måste du följa hello Installationsinstruktioner från [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).</span><span class="sxs-lookup"><span data-stu-id="4500e-114">If you prefer toouse hello commercially licensed Pro version of RStudio Server, you must follow hello installation instructions from [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).</span></span>

> [!NOTE]
> <span data-ttu-id="4500e-115">Om du använder ett HDInsight-kluster som R har installerats med hello [installera R skriptåtgärd](hdinsight-hadoop-r-scripts-linux.md), hello stegen i det här dokumentet fungerar inte korrekt eftersom de kräver en R Server på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4500e-115">If you are using an HDInsight cluster for which R was installed using hello [Install R Script Action](hdinsight-hadoop-r-scripts-linux.md), hello steps in this document will not work correctly as they require an R Server on hello HDInsight cluster.</span></span>
>
> 

## <a name="prerequisites"></a><span data-ttu-id="4500e-116">Krav</span><span class="sxs-lookup"><span data-stu-id="4500e-116">Prerequisites</span></span>

* <span data-ttu-id="4500e-117">Ett Azure HDInsight-kluster med R Server installerat.</span><span class="sxs-lookup"><span data-stu-id="4500e-117">An Azure HDInsight cluster with R Server installed.</span></span> <span data-ttu-id="4500e-118">Instruktioner finns i [komma igång med R Server på HDInsight-kluster](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4500e-118">For instructions, see [Get started with R Server on HDInsight clusters](hdinsight-hadoop-r-server-get-started.md).</span></span>
* <span data-ttu-id="4500e-119">En SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="4500e-119">An SSH client.</span></span> <span data-ttu-id="4500e-120">För Linux och Unix-distributioner eller Mac OS X, hello `ssh` kommando kan användas med hello-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="4500e-120">For Linux and Unix distributions or Macintosh OS X, hello `ssh` command is provided with hello operating system.</span></span> <span data-ttu-id="4500e-121">För Windows, rekommenderar vi [Cygwin](http://www.redhat.com/services/custom/cygwin/) med hello [OpenSSH alternativet](https://www.youtube.com/watch?v=CwYSvvGaiWU), eller [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="4500e-121">For Windows, we recommend [Cygwin](http://www.redhat.com/services/custom/cygwin/) with hello [OpenSSH option](https://www.youtube.com/watch?v=CwYSvvGaiWU), or [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>  

## <a name="install-rstudio-on-hello-cluster-using-a-custom-script"></a><span data-ttu-id="4500e-122">Installera RStudio på hello-kluster med ett anpassat skript</span><span class="sxs-lookup"><span data-stu-id="4500e-122">Install RStudio on hello cluster using a custom script</span></span>

<span data-ttu-id="4500e-123">Här är hello steg:</span><span class="sxs-lookup"><span data-stu-id="4500e-123">Here are hello steps:</span></span>

1. <span data-ttu-id="4500e-124">Identifiera hello kantnod hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="4500e-124">Identify hello edge node of hello cluster.</span></span> <span data-ttu-id="4500e-125">Följande är ett HDInsight-kluster med R Server hello namngivningskonvention huvudnod och kantnod.</span><span class="sxs-lookup"><span data-stu-id="4500e-125">For an HDInsight cluster with R Server, following is hello naming convention for head node and edge node.</span></span>
   * <span data-ttu-id="4500e-126">Huvudnod`CLUSTERNAME-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="4500e-126">Head node `CLUSTERNAME-ssh.azurehdinsight.net`</span></span>
   * <span data-ttu-id="4500e-127">Kantnod`CLUSTERNAME-ed-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="4500e-127">Edge node `CLUSTERNAME-ed-ssh.azurehdinsight.net`</span></span> 

2. <span data-ttu-id="4500e-128">SSH i hello kantnod av hello-kluster med hjälp av hello mönstret i steg 1.</span><span class="sxs-lookup"><span data-stu-id="4500e-128">SSH into hello edge node of hello cluster using hello naming pattern provided in step 1.</span></span> <span data-ttu-id="4500e-129">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="4500e-129">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="4500e-130">När du är ansluten bli rotanvändare på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="4500e-130">Once you are connected, become a root user on hello cluster.</span></span> <span data-ttu-id="4500e-131">Använd hello följande kommando i hello SSH-sessionen:</span><span class="sxs-lookup"><span data-stu-id="4500e-131">In hello SSH session, use hello following command:</span></span>

        sudo su -

4. <span data-ttu-id="4500e-132">Hämta hello anpassat skript tooinstall RStudio.</span><span class="sxs-lookup"><span data-stu-id="4500e-132">Download hello custom script tooinstall RStudio.</span></span> <span data-ttu-id="4500e-133">Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4500e-133">Use hello following command:</span></span>

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. <span data-ttu-id="4500e-134">Ändra hello behörigheter på hello anpassade skriptfilen och kör hello-skript.</span><span class="sxs-lookup"><span data-stu-id="4500e-134">Change hello permissions on hello custom script file and run hello script.</span></span> <span data-ttu-id="4500e-135">Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="4500e-135">Use hello following commands:</span></span>

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. <span data-ttu-id="4500e-136">Om du använder ett SSH-lösenord när du skapar ett HDInsight-kluster med R Server, kan du hoppa över detta steg och fortsätta toohello nästa.</span><span class="sxs-lookup"><span data-stu-id="4500e-136">If you used an SSH password while creating an HDInsight cluster with R Server, you can skip this step and proceed toohello next.</span></span> <span data-ttu-id="4500e-137">Om du använder en SSH-nyckel i stället toocreate hello kluster, måste du ange ett lösenord för SSH-användare.</span><span class="sxs-lookup"><span data-stu-id="4500e-137">If you used an SSH key instead toocreate hello cluster, you must set a password for your SSH user.</span></span> <span data-ttu-id="4500e-138">Du behöver det här lösenordet när du ansluter tooRStudio.</span><span class="sxs-lookup"><span data-stu-id="4500e-138">You need this password when connecting tooRStudio.</span></span> <span data-ttu-id="4500e-139">Kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="4500e-139">Run hello following commands:</span></span>

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. <span data-ttu-id="4500e-140">När du tillfrågas om **aktuella Kerberos lösenord**, tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="4500e-140">When prompted for **Current Kerberos password**, press **ENTER**.</span></span>  <span data-ttu-id="4500e-141">Observera att du måste ersätta `USERNAME` med en SSH-användare för din HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4500e-141">Note that you must replace `USERNAME` with an SSH user for your HDInsight cluster.</span></span> <span data-ttu-id="4500e-142">Om lösenordet är korrekt, bör du se hello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="4500e-142">If your password is successfully set, you should see hello following message:</span></span>

        passwd: password updated successfully

    <span data-ttu-id="4500e-143">Avsluta hello SSH-session.</span><span class="sxs-lookup"><span data-stu-id="4500e-143">Exit hello SSH session.</span></span>

8. <span data-ttu-id="4500e-144">Skapa en SSH-tunnel toohello kluster genom att mappa `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` på hello HDInsight-kluster toohello klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="4500e-144">Create an SSH tunnel toohello cluster by mapping `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` on hello HDInsight cluster toohello client machine.</span></span> <span data-ttu-id="4500e-145">Du måste skapa en SSH-tunnel innan du öppnar en ny webbläsarsession.</span><span class="sxs-lookup"><span data-stu-id="4500e-145">You must create an SSH tunnel before opening a new browser session.</span></span>

   * <span data-ttu-id="4500e-146">På en Linux-klient eller en Windows-klient med [Cygwin](http://www.redhat.com/services/custom/cygwin/), öppna en terminalsession och Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4500e-146">On a Linux client or a Windows client with [Cygwin](http://www.redhat.com/services/custom/cygwin/), open a terminal session and use hello following command:</span></span>

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       <span data-ttu-id="4500e-147">Ersätt **användarnamn** med en SSH-användare för din HDInsight-kluster och Ersätt **KLUSTERNAMN** med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4500e-147">Replace **USERNAME** with an SSH user for your HDInsight cluster, and replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>
       <span data-ttu-id="4500e-148">Du kan också använda en SSH-nyckel i stället för ett lösenord genom att lägga till `-i id_rsa_key`.</span><span class="sxs-lookup"><span data-stu-id="4500e-148">You can also use an SSH key rather than a password by adding `-i id_rsa_key`.</span></span>        
   * <span data-ttu-id="4500e-149">Om du använder en Windows-klienter med PuTTY sedan</span><span class="sxs-lookup"><span data-stu-id="4500e-149">If using a Windows client with PuTTY then</span></span>

     1. <span data-ttu-id="4500e-150">Öppna PuTTY och ange anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="4500e-150">Open PuTTY, and enter your connection information.</span></span>
     2. <span data-ttu-id="4500e-151">I hello **kategori** avsnitt toohello till vänster i dialogrutan hello, expandera **anslutning**, expandera **SSH**, och välj sedan **tunnlar**.</span><span class="sxs-lookup"><span data-stu-id="4500e-151">In hello **Category** section toohello left of hello dialog, expand **Connection**, expand **SSH**, and then select **Tunnels**.</span></span>
     3. <span data-ttu-id="4500e-152">Ange följande information på hello hello **alternativ som styr SSH-port vidarebefordran** formulär:</span><span class="sxs-lookup"><span data-stu-id="4500e-152">Provide hello following information on hello **Options controlling SSH port forwarding** form:</span></span>

        * <span data-ttu-id="4500e-153">**Källport** -hello port på hello-klient som du vill tooforward.</span><span class="sxs-lookup"><span data-stu-id="4500e-153">**Source port** - hello port on hello client that you wish tooforward.</span></span> <span data-ttu-id="4500e-154">Till exempel **8787**.</span><span class="sxs-lookup"><span data-stu-id="4500e-154">For example, **8787**.</span></span>
        * <span data-ttu-id="4500e-155">**Mål** - hello mål som måste vara mappat toohello lokala klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="4500e-155">**Destination** - hello destination that must be mapped toohello local client machine.</span></span> <span data-ttu-id="4500e-156">Till exempel **localhost:8787**.</span><span class="sxs-lookup"><span data-stu-id="4500e-156">For example, **localhost:8787**.</span></span>

            <span data-ttu-id="4500e-157">![Skapa en SSH-tunnel](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "skapa en SSH-tunnel")</span><span class="sxs-lookup"><span data-stu-id="4500e-157">![Create an SSH tunnel](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Create an SSH tunnel")</span></span>

     4. <span data-ttu-id="4500e-158">Klicka på **Lägg till** tooadd hello inställningar och klickar sedan på **öppna** tooopen en SSH-anslutning.</span><span class="sxs-lookup"><span data-stu-id="4500e-158">Click **Add** tooadd hello settings, and then click **Open** tooopen an SSH connection.</span></span>
     5. <span data-ttu-id="4500e-159">När du uppmanas att logga in toohello server tooestablish en SSH-session och aktivera hello-tunnel.</span><span class="sxs-lookup"><span data-stu-id="4500e-159">When prompted, log in toohello server tooestablish an SSH session and enable hello tunnel.</span></span>

9. <span data-ttu-id="4500e-160">Öppna en webbläsare och ange följande URL-Adressen baserat på hello-port som du angav för hello tunnel hello:</span><span class="sxs-lookup"><span data-stu-id="4500e-160">Open a web browser and enter hello following URL based on hello port you entered for hello tunnel:</span></span>

        http://localhost:8787/ 

10. <span data-ttu-id="4500e-161">Du kan ange tooenter hello SSH användarnamn och lösenord tooconnect toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="4500e-161">You are prompted tooenter hello SSH username and password tooconnect toohello cluster.</span></span> <span data-ttu-id="4500e-162">Om du har använt en SSH-nyckel när du skapar hello kluster, måste du ange hello lösenordet du skapade i steg 5.</span><span class="sxs-lookup"><span data-stu-id="4500e-162">If you used an SSH key while creating hello cluster, you must enter hello password you created in step 5.</span></span>

    <span data-ttu-id="4500e-163">![Ansluta tooR Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "skapa en SSH-tunnel")</span><span class="sxs-lookup"><span data-stu-id="4500e-163">![Connect tooR Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Create an SSH tunnel")</span></span>

11. <span data-ttu-id="4500e-164">tootest om hello RStudio installationen lyckades, kör du ett testskript som körs R-baserade MapReduce och Spark jobb på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="4500e-164">tootest whether hello RStudio installation was successful, you can run a test script that executes R-based MapReduce and Spark jobs on hello cluster.</span></span> <span data-ttu-id="4500e-165">toodownload hello test skriptet toorun i RStudio, gå tillbaka toohello SSH-konsolen och ange hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="4500e-165">toodownload hello test script toorun in RStudio, go back toohello SSH console and enter hello following commands:</span></span>

    *    <span data-ttu-id="4500e-166">Om du har skapat ett Hadoop-kluster med R, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4500e-166">If you created a Hadoop cluster with R, use this command:</span></span>

            <span data-ttu-id="4500e-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span><span class="sxs-lookup"><span data-stu-id="4500e-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span></span>
    *    <span data-ttu-id="4500e-168">Om du har skapat ett Spark-kluster med R, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4500e-168">If you created a Spark cluster with R, use this command:</span></span>

            <span data-ttu-id="4500e-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span><span class="sxs-lookup"><span data-stu-id="4500e-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span></span>

12. <span data-ttu-id="4500e-170">I RStudio ser du hello testa skript som du hämtat.</span><span class="sxs-lookup"><span data-stu-id="4500e-170">In RStudio, you see hello test script you downloaded.</span></span> <span data-ttu-id="4500e-171">Dubbelklicka på filen hello tooopen, Välj hello innehållet i hello-filen och klicka sedan på **kör**.</span><span class="sxs-lookup"><span data-stu-id="4500e-171">Double-click hello file tooopen it, select hello contents of hello file, and then click **Run**.</span></span> <span data-ttu-id="4500e-172">Du bör se utdata hello i hello **konsolen** fönstret:</span><span class="sxs-lookup"><span data-stu-id="4500e-172">You should see hello output in hello **Console** pane:</span></span>

   <span data-ttu-id="4500e-173">![Testa installationen av hello](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "testa hello-installation")</span><span class="sxs-lookup"><span data-stu-id="4500e-173">![Test hello installation](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Test hello installation")</span></span>

<span data-ttu-id="4500e-174">Ett annat alternativ är tootype `source(testhdi.r)` eller `source(testhdi_spark.r)` tooexecute hello skript.</span><span class="sxs-lookup"><span data-stu-id="4500e-174">Another option would be tootype `source(testhdi.r)` or `source(testhdi_spark.r)` tooexecute hello script.</span></span>

## <a name="see-also"></a><span data-ttu-id="4500e-175">Se även</span><span class="sxs-lookup"><span data-stu-id="4500e-175">See also</span></span>

* [<span data-ttu-id="4500e-176">Compute-kontexten alternativ för R Server på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="4500e-176">Compute context options for R Server on HDInsight clusters</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)
* [<span data-ttu-id="4500e-177">Alternativ för Azure Storage för R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="4500e-177">Azure Storage options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-storage.md)

