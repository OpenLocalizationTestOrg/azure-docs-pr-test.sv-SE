---
title: "Lär dig använda Hadoop-sandbox - emulatorn – Azure HDInsight | Microsoft Docs"
description: "Om du vill börja lära dig om att använda Hadoop-ekosystemet, kan du ställa in en Hadoop sandbox från Hortonworks på en virtuell Azure-dator. "
keywords: hadoop-emulatorn hadoop sandbox
editor: cgronlun
manager: jhubbard
services: hdinsight
author: nitinme
documentationcenter: 
tags: azure-portal
ms.assetid: 6ad5bb58-8215-4e3d-a07f-07fcd8839cc6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: b701879464205860edd1c097651b532f87bae388
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a><span data-ttu-id="444c1-104">Kom igång med Hadoop-sandbox, en emulator på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="444c1-104">Get started with a Hadoop sandbox, an emulator on a virtual machine</span></span>

<span data-ttu-id="444c1-105">Lär dig hur du installerar sandlådan Hadoop från Hortonworks på en virtuell dator om du vill veta mer om Hadoop-ekosystemet.</span><span class="sxs-lookup"><span data-stu-id="444c1-105">Learn how to install the Hadoop sandbox from Hortonworks on a virtual machine to learn about the Hadoop ecosystem.</span></span> <span data-ttu-id="444c1-106">Sandbox tillhandahåller en lokal utvecklingsmiljö mer information om Hadoop, Hadoop Distributed File System (HDFS) och skicka jobbet.</span><span class="sxs-lookup"><span data-stu-id="444c1-106">The sandbox provides a local development environment to learn about Hadoop, Hadoop Distributed File System (HDFS), and job submission.</span></span> <span data-ttu-id="444c1-107">När du är bekant med Hadoop, kan du börja använda Hadoop i Azure genom att skapa ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="444c1-107">Once you are familiar with Hadoop, you can start using Hadoop on Azure by creating an HDInsight cluster.</span></span> <span data-ttu-id="444c1-108">Mer information om hur du kommer igång finns [Kom igång med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="444c1-108">For more information on how to get started, see [Get started with Hadoop on HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="444c1-109">Krav</span><span class="sxs-lookup"><span data-stu-id="444c1-109">Prerequisites</span></span>
* <span data-ttu-id="444c1-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span><span class="sxs-lookup"><span data-stu-id="444c1-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span></span> <span data-ttu-id="444c1-111">Hämta och installera den från [här](https://www.virtualbox.org/wiki/Downloads).</span><span class="sxs-lookup"><span data-stu-id="444c1-111">Download and install it from [here](https://www.virtualbox.org/wiki/Downloads).</span></span>



## <a name="download-and-install-the-virtual-machine"></a><span data-ttu-id="444c1-112">Hämta och installera den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="444c1-112">Download and install the virtual machine</span></span>
1. <span data-ttu-id="444c1-113">Bläddra till den [Hortonworks hämtar](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="444c1-113">Browse to the [Hortonworks downloads](http://hortonworks.com/downloads/#sandbox).</span></span>

2. <span data-ttu-id="444c1-114">Klicka på **hämta VIRTUALBOX** att ladda ned den senaste Hortonworks Sandbox på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="444c1-114">Click **DOWNLOAD FOR VIRTUALBOX** to download the latest Hortonworks Sandbox on a VM.</span></span> <span data-ttu-id="444c1-115">Du uppmanas att registrera med Hortonworks innan hämtningen påbörjas.</span><span class="sxs-lookup"><span data-stu-id="444c1-115">You are prompted to register with Hortonworks before the download begins.</span></span> <span data-ttu-id="444c1-116">Det tar en till två timmar att hämta beroende på nätverkets hastighet.</span><span class="sxs-lookup"><span data-stu-id="444c1-116">It takes one to two hours to download depending on your network speed.</span></span>
   
    ![Länka bild för att ladda ned Hortonworks Sandbox för VirtualBox](./media/hdinsight-hadoop-emulator-get-started/download-sandbox.png)
3. <span data-ttu-id="444c1-118">Samma webbsida, klicka på den **Import på virtuella** länk för att hämta en PDF-fil som innehåller instruktioner för installation för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="444c1-118">From the same web page, click the **Import on Virtual Box** link to download a PDF containing installation instructions for the virtual machine.</span></span>

<span data-ttu-id="444c1-119">Om du vill hämta en äldre HDP version sandbox Expandera arkivet:</span><span class="sxs-lookup"><span data-stu-id="444c1-119">To download an older HDP version sandbox, expand the archive:</span></span>

![Hortonworks Sandbox-Arkiv](./media/hdinsight-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-the-virtual-machine"></a><span data-ttu-id="444c1-121">Starta den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="444c1-121">Start the virtual machine</span></span>

1. <span data-ttu-id="444c1-122">Öppna Oracle VM VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="444c1-122">Open Oracle VM VirtualBox.</span></span>
2. <span data-ttu-id="444c1-123">Från den **filen** -menyn klickar du på **importera installation**, och sedan ange avbildningen som Hortonworks Sandbox.</span><span class="sxs-lookup"><span data-stu-id="444c1-123">From the **File** menu, click **Import Appliance**, and then specify the Hortonworks Sandbox image.</span></span>
1. <span data-ttu-id="444c1-124">Välj sandlådan Hortonworks, klicka på **starta**, och sedan **Normal Start**.</span><span class="sxs-lookup"><span data-stu-id="444c1-124">Select the Hortonworks Sandbox, click **Start**, and then **Normal Start**.</span></span> <span data-ttu-id="444c1-125">När den virtuella datorn har slutförts startprocessen visar instruktioner för inloggning.</span><span class="sxs-lookup"><span data-stu-id="444c1-125">Once the virtual machine has finished the boot process, it displays login instructions.</span></span>
   
    ![Normal start](./media/hdinsight-hadoop-emulator-get-started/normal-start.png)
2. <span data-ttu-id="444c1-127">Öppna en webbläsare och gå till den URL som visas (vanligtvis http://127.0.0.1:8888).</span><span class="sxs-lookup"><span data-stu-id="444c1-127">Open a web browser and navigate to the URL displayed (usually http://127.0.0.1:8888).</span></span>

## <a name="set-sandbox-passwords"></a><span data-ttu-id="444c1-128">Ange Sandbox lösenord</span><span class="sxs-lookup"><span data-stu-id="444c1-128">Set Sandbox passwords</span></span>

1. <span data-ttu-id="444c1-129">Från den **Kom igång** steg på sidan Hortonworks Sandbox väljer **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="444c1-129">From the **get started** step of the Hortonworks Sandbox page, select **View Advanced Options**.</span></span> <span data-ttu-id="444c1-130">Använd informationen på den här sidan för att logga in till sandbox via SSH.</span><span class="sxs-lookup"><span data-stu-id="444c1-130">Use the information on this page to log in to the sandbox using SSH.</span></span> <span data-ttu-id="444c1-131">Använd namn och lösenord angavs.</span><span class="sxs-lookup"><span data-stu-id="444c1-131">Use the name and password provided.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="444c1-132">Om du inte har en SSH-klienten installerad kan du använda den webbaserade SSH som anges i av den virtuella datorn på **http://localhost:4200 /**.</span><span class="sxs-lookup"><span data-stu-id="444c1-132">If you do not have an SSH client installed, you can use the web-based SSH provided at by the virtual machine at **http://localhost:4200/**.</span></span>
   > 
   
    <span data-ttu-id="444c1-133">Första gången du ansluter med SSH, uppmanas du att ändra lösenordet för rotkontot.</span><span class="sxs-lookup"><span data-stu-id="444c1-133">The first time you connect using SSH, you are prompted to change the password for the root account.</span></span> <span data-ttu-id="444c1-134">Ange ett nytt lösenord som du använder när du loggar in via SSH.</span><span class="sxs-lookup"><span data-stu-id="444c1-134">Enter a new password, which you use when you log in using SSH.</span></span>

2. <span data-ttu-id="444c1-135">Efter loggat in kan du ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="444c1-135">Once logged in, enter the following command:</span></span>
   
        ambari-admin-password-reset
   
    <span data-ttu-id="444c1-136">När du uppmanas ange ett lösenord för administratörskontot Ambari.</span><span class="sxs-lookup"><span data-stu-id="444c1-136">When prompted, provide a password for the Ambari admin account.</span></span> <span data-ttu-id="444c1-137">Det här används när du använder Ambari-Webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="444c1-137">This is used when you access the Ambari Web UI.</span></span>

## <a name="use-hive-commands"></a><span data-ttu-id="444c1-138">Använda Hive-kommandon</span><span class="sxs-lookup"><span data-stu-id="444c1-138">Use Hive commands</span></span>

1. <span data-ttu-id="444c1-139">Från en SSH-anslutning till sandbox använder du följande kommando för att starta Hive-gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="444c1-139">From an SSH connection to the sandbox, use the following command to start the Hive shell:</span></span>
   
        hive
2. <span data-ttu-id="444c1-140">När gränssnittet har börjat använda följande för att visa tabeller som tillhandahålls med sandbox:</span><span class="sxs-lookup"><span data-stu-id="444c1-140">Once the shell has started, use the following to view the tables that are provided with the sandbox:</span></span>
   
        show tables;
3. <span data-ttu-id="444c1-141">Använd följande för att hämta 10 rader från den `sample_07` tabellen:</span><span class="sxs-lookup"><span data-stu-id="444c1-141">Use the following to retrieve 10 rows from the `sample_07` table:</span></span>
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a><span data-ttu-id="444c1-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="444c1-142">Next steps</span></span>
* [<span data-ttu-id="444c1-143">Lär dig hur du använder Visual Studio med Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="444c1-143">Learn how to use Visual Studio with the Hortonworks Sandbox</span></span>](hdinsight-hadoop-emulator-visual-studio.md)
* [<span data-ttu-id="444c1-144">Learning linor av sandlådan Hortonworks</span><span class="sxs-lookup"><span data-stu-id="444c1-144">Learning the ropes of the Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="444c1-145">Hadoop-vägledning - komma igång med HDP</span><span class="sxs-lookup"><span data-stu-id="444c1-145">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

