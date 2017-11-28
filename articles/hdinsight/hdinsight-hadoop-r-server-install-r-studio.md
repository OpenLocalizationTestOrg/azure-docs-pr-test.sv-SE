---
title: "Installera RStudio med R Server på HDInsight - Azure | Microsoft Docs"
description: "Så här installerar du RStudio med R Server på HDInsight."
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
ms.openlocfilehash: 416420d855505508735ebd8526e93efdb230ad53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a><span data-ttu-id="3c35a-103">Installera RStudio med R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="3c35a-103">Installing RStudio with R Server on HDInsight</span></span>

<span data-ttu-id="3c35a-104">Den här artikeln beskriver hur du installerar community (kostnadsfritt) version av [RStudio Server](https://www.rstudio.com/products/rstudio-server/) på edge-nod i ett kluster med ett anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="3c35a-104">This article describes how to install the community (free) version of [RStudio Server](https://www.rstudio.com/products/rstudio-server/) on the edge node of a cluster using a custom script.</span></span> <span data-ttu-id="3c35a-105">RStudio Server innehåller en webbläsarbaserad IDE för användning av fjärranslutna klienter och som används ofta i Linux.</span><span class="sxs-lookup"><span data-stu-id="3c35a-105">RStudio Server provides a browser-based IDE for use by remote clients and is widely used on Linux.</span></span> <span data-ttu-id="3c35a-106">Det finns flera integrerad utvecklingsmiljöer (IDEs) för R idag, inklusive:</span><span class="sxs-lookup"><span data-stu-id="3c35a-106">There are multiple integrated development environments (IDEs) available for R today, including:</span></span>

- <span data-ttu-id="3c35a-107">Microsofts [R Tools för Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span><span class="sxs-lookup"><span data-stu-id="3c35a-107">Microsoft’s  [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span></span> 
- [<span data-ttu-id="3c35a-108">RStudio Server</span><span class="sxs-lookup"><span data-stu-id="3c35a-108">RStudio Server</span></span>](https://www.rstudio.com/products/rstudio-server/) 
- <span data-ttu-id="3c35a-109">Walware har Eclipse-baserade [StatET](http://www.walware.de/goto/statet).</span><span class="sxs-lookup"><span data-stu-id="3c35a-109">Walware’s Eclipse-based [StatET](http://www.walware.de/goto/statet).</span></span>

<span data-ttu-id="3c35a-110">Fördelen med att installera RStudio på kantnoden för ett HDInsight-kluster är att det ger en fullständig IDE-miljö för utveckling och körning av R-skript med R Server i klustret.</span><span class="sxs-lookup"><span data-stu-id="3c35a-110">The advantage of installing RStudio Server on the edge node of an HDInsight cluster is that it provides a full IDE experience for the development and execution of R scripts with R Server on the cluster.</span></span> <span data-ttu-id="3c35a-111">Den här konfigurationen kan vara betydligt mer effektiva än standardinställningen för användning av R-konsolen.</span><span class="sxs-lookup"><span data-stu-id="3c35a-111">This configuration can be considerably more productive than default use of the R Console.</span></span>

> [!NOTE]
> <span data-ttu-id="3c35a-112">Det förfarande som beskrivs i den här artikeln gäller endast om du inte valde för att installera RStudio Server community edition vid etablering av klustret.</span><span class="sxs-lookup"><span data-stu-id="3c35a-112">The procedure described in this article is only relevant if you did not select to install RStudio Server community edition when provisioning your cluster.</span></span> <span data-ttu-id="3c35a-113">Om du lade till den under etableringen och att komma åt det. Klicka på den **R Server instrumentpaneler** panelen i Azure portal posten för klustret, sedan på den **R Studio Server** panelen.</span><span class="sxs-lookup"><span data-stu-id="3c35a-113">If you added it during provisioning, then to access it you click on the **R Server Dashboards** tile in the Azure portal entry for your cluster, then on the **R Studio Server** tile.</span></span> 

<span data-ttu-id="3c35a-114">Om du föredrar att använda kommersiellt licensierad Pro-version av RStudio Server, måste du följa installationsanvisningarna från [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).</span><span class="sxs-lookup"><span data-stu-id="3c35a-114">If you prefer to use the commercially licensed Pro version of RStudio Server, you must follow the installation instructions from [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).</span></span>

> [!NOTE]
> <span data-ttu-id="3c35a-115">Om du använder ett HDInsight-kluster som R har installerats med hjälp av den [installera R skriptåtgärd](hdinsight-hadoop-r-scripts-linux.md), stegen i det här dokumentet fungerar inte korrekt eftersom de kräver en R Server på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3c35a-115">If you are using an HDInsight cluster for which R was installed using the [Install R Script Action](hdinsight-hadoop-r-scripts-linux.md), the steps in this document will not work correctly as they require an R Server on the HDInsight cluster.</span></span>
>
> 

## <a name="prerequisites"></a><span data-ttu-id="3c35a-116">Krav</span><span class="sxs-lookup"><span data-stu-id="3c35a-116">Prerequisites</span></span>

* <span data-ttu-id="3c35a-117">Ett Azure HDInsight-kluster med R Server installerat.</span><span class="sxs-lookup"><span data-stu-id="3c35a-117">An Azure HDInsight cluster with R Server installed.</span></span> <span data-ttu-id="3c35a-118">Instruktioner finns i [komma igång med R Server på HDInsight-kluster](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3c35a-118">For instructions, see [Get started with R Server on HDInsight clusters](hdinsight-hadoop-r-server-get-started.md).</span></span>
* <span data-ttu-id="3c35a-119">En SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="3c35a-119">An SSH client.</span></span> <span data-ttu-id="3c35a-120">För Linux och Unix-distributioner eller Mac OS X i `ssh` kommandot ingår i operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="3c35a-120">For Linux and Unix distributions or Macintosh OS X, the `ssh` command is provided with the operating system.</span></span> <span data-ttu-id="3c35a-121">För Windows, rekommenderar vi [Cygwin](http://www.redhat.com/services/custom/cygwin/) med den [OpenSSH alternativet](https://www.youtube.com/watch?v=CwYSvvGaiWU), eller [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="3c35a-121">For Windows, we recommend [Cygwin](http://www.redhat.com/services/custom/cygwin/) with the [OpenSSH option](https://www.youtube.com/watch?v=CwYSvvGaiWU), or [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>  

## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a><span data-ttu-id="3c35a-122">Installera RStudio på klustret med ett anpassat skript</span><span class="sxs-lookup"><span data-stu-id="3c35a-122">Install RStudio on the cluster using a custom script</span></span>

<span data-ttu-id="3c35a-123">Här är stegen:</span><span class="sxs-lookup"><span data-stu-id="3c35a-123">Here are the steps:</span></span>

1. <span data-ttu-id="3c35a-124">Identifiera edge-nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="3c35a-124">Identify the edge node of the cluster.</span></span> <span data-ttu-id="3c35a-125">Följande är ett HDInsight-kluster med R Server namngivningskonvention huvudnod och kantnod.</span><span class="sxs-lookup"><span data-stu-id="3c35a-125">For an HDInsight cluster with R Server, following is the naming convention for head node and edge node.</span></span>
   * <span data-ttu-id="3c35a-126">Huvudnod`CLUSTERNAME-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="3c35a-126">Head node `CLUSTERNAME-ssh.azurehdinsight.net`</span></span>
   * <span data-ttu-id="3c35a-127">Kantnod`CLUSTERNAME-ed-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="3c35a-127">Edge node `CLUSTERNAME-ed-ssh.azurehdinsight.net`</span></span> 

2. <span data-ttu-id="3c35a-128">SSH i edge-nod i klustret med namngivningsmönstret i steg 1.</span><span class="sxs-lookup"><span data-stu-id="3c35a-128">SSH into the edge node of the cluster using the naming pattern provided in step 1.</span></span> <span data-ttu-id="3c35a-129">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="3c35a-129">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="3c35a-130">När du är ansluten bli rotanvändare i klustret.</span><span class="sxs-lookup"><span data-stu-id="3c35a-130">Once you are connected, become a root user on the cluster.</span></span> <span data-ttu-id="3c35a-131">I SSH-session använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3c35a-131">In the SSH session, use the following command:</span></span>

        sudo su -

4. <span data-ttu-id="3c35a-132">Hämta anpassade skript för att installera RStudio.</span><span class="sxs-lookup"><span data-stu-id="3c35a-132">Download the custom script to install RStudio.</span></span> <span data-ttu-id="3c35a-133">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3c35a-133">Use the following command:</span></span>

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. <span data-ttu-id="3c35a-134">Ändra behörigheterna för anpassat skript-fil och kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="3c35a-134">Change the permissions on the custom script file and run the script.</span></span> <span data-ttu-id="3c35a-135">Använd följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3c35a-135">Use the following commands:</span></span>

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. <span data-ttu-id="3c35a-136">Om du har använt en SSH-lösenordet när du skapar ett HDInsight-kluster med R Server kan du hoppa över detta steg och fortsätta till nästa.</span><span class="sxs-lookup"><span data-stu-id="3c35a-136">If you used an SSH password while creating an HDInsight cluster with R Server, you can skip this step and proceed to the next.</span></span> <span data-ttu-id="3c35a-137">Om du har använt en SSH-nyckel i stället för att skapa klustret, måste du ange ett lösenord för SSH-användare.</span><span class="sxs-lookup"><span data-stu-id="3c35a-137">If you used an SSH key instead to create the cluster, you must set a password for your SSH user.</span></span> <span data-ttu-id="3c35a-138">Du behöver det här lösenordet när du ansluter till RStudio.</span><span class="sxs-lookup"><span data-stu-id="3c35a-138">You need this password when connecting to RStudio.</span></span> <span data-ttu-id="3c35a-139">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3c35a-139">Run the following commands:</span></span>

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. <span data-ttu-id="3c35a-140">När du tillfrågas om **aktuella Kerberos lösenord**, tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="3c35a-140">When prompted for **Current Kerberos password**, press **ENTER**.</span></span>  <span data-ttu-id="3c35a-141">Observera att du måste ersätta `USERNAME` med en SSH-användare för din HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3c35a-141">Note that you must replace `USERNAME` with an SSH user for your HDInsight cluster.</span></span> <span data-ttu-id="3c35a-142">Om ditt lösenord har angetts ska du se följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="3c35a-142">If your password is successfully set, you should see the following message:</span></span>

        passwd: password updated successfully

    <span data-ttu-id="3c35a-143">Avsluta SSH-session.</span><span class="sxs-lookup"><span data-stu-id="3c35a-143">Exit the SSH session.</span></span>

8. <span data-ttu-id="3c35a-144">Skapa en SSH-tunnel till klustret genom att mappa `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` på HDInsight-kluster till klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="3c35a-144">Create an SSH tunnel to the cluster by mapping `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` on the HDInsight cluster to the client machine.</span></span> <span data-ttu-id="3c35a-145">Du måste skapa en SSH-tunnel innan du öppnar en ny webbläsarsession.</span><span class="sxs-lookup"><span data-stu-id="3c35a-145">You must create an SSH tunnel before opening a new browser session.</span></span>

   * <span data-ttu-id="3c35a-146">På en Linux-klient eller en Windows-klient med [Cygwin](http://www.redhat.com/services/custom/cygwin/), öppna en terminalsession och använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3c35a-146">On a Linux client or a Windows client with [Cygwin](http://www.redhat.com/services/custom/cygwin/), open a terminal session and use the following command:</span></span>

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       <span data-ttu-id="3c35a-147">Ersätt **användarnamn** med en SSH-användare för din HDInsight-kluster och Ersätt **KLUSTERNAMN** med namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3c35a-147">Replace **USERNAME** with an SSH user for your HDInsight cluster, and replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>
       <span data-ttu-id="3c35a-148">Du kan också använda en SSH-nyckel i stället för ett lösenord genom att lägga till `-i id_rsa_key`.</span><span class="sxs-lookup"><span data-stu-id="3c35a-148">You can also use an SSH key rather than a password by adding `-i id_rsa_key`.</span></span>        
   * <span data-ttu-id="3c35a-149">Om du använder en Windows-klienter med PuTTY sedan</span><span class="sxs-lookup"><span data-stu-id="3c35a-149">If using a Windows client with PuTTY then</span></span>

     1. <span data-ttu-id="3c35a-150">Öppna PuTTY och ange anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="3c35a-150">Open PuTTY, and enter your connection information.</span></span>
     2. <span data-ttu-id="3c35a-151">I den **kategori** till vänster om dialogrutan, expanderar **anslutning**, expandera **SSH**, och välj sedan **tunnlar**.</span><span class="sxs-lookup"><span data-stu-id="3c35a-151">In the **Category** section to the left of the dialog, expand **Connection**, expand **SSH**, and then select **Tunnels**.</span></span>
     3. <span data-ttu-id="3c35a-152">Ange följande information på den **alternativ som styr SSH-port vidarebefordran** formulär:</span><span class="sxs-lookup"><span data-stu-id="3c35a-152">Provide the following information on the **Options controlling SSH port forwarding** form:</span></span>

        * <span data-ttu-id="3c35a-153">**Källport** – Porten på den klient som du vill vidarebefordra.</span><span class="sxs-lookup"><span data-stu-id="3c35a-153">**Source port** - The port on the client that you wish to forward.</span></span> <span data-ttu-id="3c35a-154">Till exempel **8787**.</span><span class="sxs-lookup"><span data-stu-id="3c35a-154">For example, **8787**.</span></span>
        * <span data-ttu-id="3c35a-155">**Mål** -målet måste mappas till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="3c35a-155">**Destination** - The destination that must be mapped to the local client machine.</span></span> <span data-ttu-id="3c35a-156">Till exempel **localhost:8787**.</span><span class="sxs-lookup"><span data-stu-id="3c35a-156">For example, **localhost:8787**.</span></span>

            <span data-ttu-id="3c35a-157">![Skapa en SSH-tunnel](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "skapa en SSH-tunnel")</span><span class="sxs-lookup"><span data-stu-id="3c35a-157">![Create an SSH tunnel](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Create an SSH tunnel")</span></span>

     4. <span data-ttu-id="3c35a-158">Klicka på **Lägg till** lägga till inställningarna och klicka sedan på **öppna** att öppna en SSH-anslutning.</span><span class="sxs-lookup"><span data-stu-id="3c35a-158">Click **Add** to add the settings, and then click **Open** to open an SSH connection.</span></span>
     5. <span data-ttu-id="3c35a-159">När du uppmanas logga in på servern för att upprätta en SSH-session och aktiverar tunneln.</span><span class="sxs-lookup"><span data-stu-id="3c35a-159">When prompted, log in to the server to establish an SSH session and enable the tunnel.</span></span>

9. <span data-ttu-id="3c35a-160">Öppna en webbläsare och ange följande URL baserat på vilken port som du angav för tunneln:</span><span class="sxs-lookup"><span data-stu-id="3c35a-160">Open a web browser and enter the following URL based on the port you entered for the tunnel:</span></span>

        http://localhost:8787/ 

10. <span data-ttu-id="3c35a-161">Du uppmanas att ange SSH-användarnamn och lösenord för att ansluta till klustret.</span><span class="sxs-lookup"><span data-stu-id="3c35a-161">You are prompted to enter the SSH username and password to connect to the cluster.</span></span> <span data-ttu-id="3c35a-162">Om du har använt en SSH-nyckel när du skapar klustret måste du ange lösenordet du skapade i steg 5.</span><span class="sxs-lookup"><span data-stu-id="3c35a-162">If you used an SSH key while creating the cluster, you must enter the password you created in step 5.</span></span>

    <span data-ttu-id="3c35a-163">![Ansluta till R Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "skapa en SSH-tunnel")</span><span class="sxs-lookup"><span data-stu-id="3c35a-163">![Connect to R Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Create an SSH tunnel")</span></span>

11. <span data-ttu-id="3c35a-164">Du kan köra ett testskript som körs R-baserade MapReduce och Spark jobb i klustret för att testa om RStudio installationen lyckades.</span><span class="sxs-lookup"><span data-stu-id="3c35a-164">To test whether the RStudio installation was successful, you can run a test script that executes R-based MapReduce and Spark jobs on the cluster.</span></span> <span data-ttu-id="3c35a-165">Hämta testskriptet ska köras i RStudio gå tillbaka till konsolen SSH och ange följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3c35a-165">To download the test script to run in RStudio, go back to the SSH console and enter the following commands:</span></span>

    *    <span data-ttu-id="3c35a-166">Om du har skapat ett Hadoop-kluster med R, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3c35a-166">If you created a Hadoop cluster with R, use this command:</span></span>

            <span data-ttu-id="3c35a-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span><span class="sxs-lookup"><span data-stu-id="3c35a-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span></span>
    *    <span data-ttu-id="3c35a-168">Om du har skapat ett Spark-kluster med R, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3c35a-168">If you created a Spark cluster with R, use this command:</span></span>

            <span data-ttu-id="3c35a-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span><span class="sxs-lookup"><span data-stu-id="3c35a-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span></span>

12. <span data-ttu-id="3c35a-170">I RStudio ser du testskriptet som du hämtat.</span><span class="sxs-lookup"><span data-stu-id="3c35a-170">In RStudio, you see the test script you downloaded.</span></span> <span data-ttu-id="3c35a-171">Dubbelklicka på filen för att öppna den, markerar du innehållet i filen och klicka sedan på **kör**.</span><span class="sxs-lookup"><span data-stu-id="3c35a-171">Double-click the file to open it, select the contents of the file, and then click **Run**.</span></span> <span data-ttu-id="3c35a-172">Du bör se utdata i den **konsolen** fönstret:</span><span class="sxs-lookup"><span data-stu-id="3c35a-172">You should see the output in the **Console** pane:</span></span>

   <span data-ttu-id="3c35a-173">![Testa installationen](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "testa installationen")</span><span class="sxs-lookup"><span data-stu-id="3c35a-173">![Test the installation](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Test the installation")</span></span>

<span data-ttu-id="3c35a-174">Ett annat alternativ är att ange `source(testhdi.r)` eller `source(testhdi_spark.r)` att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="3c35a-174">Another option would be to type `source(testhdi.r)` or `source(testhdi_spark.r)` to execute the script.</span></span>

## <a name="see-also"></a><span data-ttu-id="3c35a-175">Se även</span><span class="sxs-lookup"><span data-stu-id="3c35a-175">See also</span></span>

* [<span data-ttu-id="3c35a-176">Compute-kontexten alternativ för R Server på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="3c35a-176">Compute context options for R Server on HDInsight clusters</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)
* [<span data-ttu-id="3c35a-177">Alternativ för Azure Storage för R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="3c35a-177">Azure Storage options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-storage.md)

