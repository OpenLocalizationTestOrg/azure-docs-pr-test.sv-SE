---
title: "aaaLearn med Hadoop-sandbox - emulatorn – Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du använder toostart Hej Hadoop-ekosystemet, du kan ställa in en Hadoop sandbox från Hortonworks på en virtuell Azure-dator. "
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
ms.openlocfilehash: 91e74f0823fd02e9bb812155a7d09357a77b0736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a><span data-ttu-id="9ca8d-104">Kom igång med Hadoop-sandbox, en emulator på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9ca8d-104">Get started with a Hadoop sandbox, an emulator on a virtual machine</span></span>

<span data-ttu-id="9ca8d-105">Lär dig hur tooinstall hello Hadoop sandbox från Hortonworks på en virtuell dator toolearn om hello Hadoop-ekosystemet.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-105">Learn how tooinstall hello Hadoop sandbox from Hortonworks on a virtual machine toolearn about hello Hadoop ecosystem.</span></span> <span data-ttu-id="9ca8d-106">hello sandbox tillhandahåller en lokal utveckling miljö toolearn om Hadoop, Hadoop Distributed File System (HDFS) och skicka jobbet.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-106">hello sandbox provides a local development environment toolearn about Hadoop, Hadoop Distributed File System (HDFS), and job submission.</span></span> <span data-ttu-id="9ca8d-107">När du är bekant med Hadoop, kan du börja använda Hadoop i Azure genom att skapa ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-107">Once you are familiar with Hadoop, you can start using Hadoop on Azure by creating an HDInsight cluster.</span></span> <span data-ttu-id="9ca8d-108">Mer information om hur tooget igång finns [Kom igång med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9ca8d-108">For more information on how tooget started, see [Get started with Hadoop on HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ca8d-109">Krav</span><span class="sxs-lookup"><span data-stu-id="9ca8d-109">Prerequisites</span></span>
* <span data-ttu-id="9ca8d-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span><span class="sxs-lookup"><span data-stu-id="9ca8d-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span></span> <span data-ttu-id="9ca8d-111">Hämta och installera den från [här](https://www.virtualbox.org/wiki/Downloads).</span><span class="sxs-lookup"><span data-stu-id="9ca8d-111">Download and install it from [here](https://www.virtualbox.org/wiki/Downloads).</span></span>



## <a name="download-and-install-hello-virtual-machine"></a><span data-ttu-id="9ca8d-112">Hämta och installera hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9ca8d-112">Download and install hello virtual machine</span></span>
1. <span data-ttu-id="9ca8d-113">Bläddra toohello [Hortonworks hämtar](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="9ca8d-113">Browse toohello [Hortonworks downloads](http://hortonworks.com/downloads/#sandbox).</span></span>

2. <span data-ttu-id="9ca8d-114">Klicka på **hämta VIRTUALBOX** toodownload hello senaste Hortonworks Sandbox på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-114">Click **DOWNLOAD FOR VIRTUALBOX** toodownload hello latest Hortonworks Sandbox on a VM.</span></span> <span data-ttu-id="9ca8d-115">Du kan ange tooregister med Hortonworks innan hello hämtningen påbörjas.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-115">You are prompted tooregister with Hortonworks before hello download begins.</span></span> <span data-ttu-id="9ca8d-116">Det tar en tootwo timmar toodownload beroende på nätverkets hastighet.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-116">It takes one tootwo hours toodownload depending on your network speed.</span></span>
   
    ![Länka bild för att ladda ned Hortonworks Sandbox för VirtualBox](./media/hdinsight-hadoop-emulator-get-started/download-sandbox.png)
3. <span data-ttu-id="9ca8d-118">Från Hej samma webbsida, klicka på hello **Import på virtuella** länka toodownload en PDF-fil som innehåller instruktioner för installation för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-118">From hello same web page, click hello **Import on Virtual Box** link toodownload a PDF containing installation instructions for hello virtual machine.</span></span>

<span data-ttu-id="9ca8d-119">toodownload ett äldre HDP version sandbox Expandera hello Arkiv:</span><span class="sxs-lookup"><span data-stu-id="9ca8d-119">toodownload an older HDP version sandbox, expand hello archive:</span></span>

![Hortonworks Sandbox-Arkiv](./media/hdinsight-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-hello-virtual-machine"></a><span data-ttu-id="9ca8d-121">Starta hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9ca8d-121">Start hello virtual machine</span></span>

1. <span data-ttu-id="9ca8d-122">Öppna Oracle VM VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-122">Open Oracle VM VirtualBox.</span></span>
2. <span data-ttu-id="9ca8d-123">Från hello **filen** -menyn klickar du på **importera installation**, och sedan ange hello Hortonworks Sandbox avbildningen.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-123">From hello **File** menu, click **Import Appliance**, and then specify hello Hortonworks Sandbox image.</span></span>
1. <span data-ttu-id="9ca8d-124">Klicka på Välj hello Hortonworks Sandbox **starta**, och sedan **Normal Start**.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-124">Select hello Hortonworks Sandbox, click **Start**, and then **Normal Start**.</span></span> <span data-ttu-id="9ca8d-125">När hello virtuell dator har slutförts hello startprocessen visar instruktioner för inloggning.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-125">Once hello virtual machine has finished hello boot process, it displays login instructions.</span></span>
   
    ![Normal start](./media/hdinsight-hadoop-emulator-get-started/normal-start.png)
2. <span data-ttu-id="9ca8d-127">Öppna en webbläsare och gå toohello URL visas (vanligtvis http://127.0.0.1:8888).</span><span class="sxs-lookup"><span data-stu-id="9ca8d-127">Open a web browser and navigate toohello URL displayed (usually http://127.0.0.1:8888).</span></span>

## <a name="set-sandbox-passwords"></a><span data-ttu-id="9ca8d-128">Ange Sandbox lösenord</span><span class="sxs-lookup"><span data-stu-id="9ca8d-128">Set Sandbox passwords</span></span>

1. <span data-ttu-id="9ca8d-129">Från hello **Kom igång** steg i hello Hortonworks Sandbox-sidan, Välj **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-129">From hello **get started** step of hello Hortonworks Sandbox page, select **View Advanced Options**.</span></span> <span data-ttu-id="9ca8d-130">Använd hello information på den här sidan toolog i toohello sandbox via SSH.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-130">Use hello information on this page toolog in toohello sandbox using SSH.</span></span> <span data-ttu-id="9ca8d-131">Använd hello namn och lösenord angavs.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-131">Use hello name and password provided.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9ca8d-132">Om du inte har en SSH-klienten installerad kan du använda hello webbaserade SSH anges i hello virtuell dator på **http://localhost:4200 /**.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-132">If you do not have an SSH client installed, you can use hello web-based SSH provided at by hello virtual machine at **http://localhost:4200/**.</span></span>
   > 
   
    <span data-ttu-id="9ca8d-133">hello kan första gången du ansluter med SSH, du ange toochange hello lösenordet för rotkontot hello.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-133">hello first time you connect using SSH, you are prompted toochange hello password for hello root account.</span></span> <span data-ttu-id="9ca8d-134">Ange ett nytt lösenord som du använder när du loggar in via SSH.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-134">Enter a new password, which you use when you log in using SSH.</span></span>

2. <span data-ttu-id="9ca8d-135">Efter loggat in kan du ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ca8d-135">Once logged in, enter hello following command:</span></span>
   
        ambari-admin-password-reset
   
    <span data-ttu-id="9ca8d-136">När du uppmanas ange ett lösenord för hello Ambari-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-136">When prompted, provide a password for hello Ambari admin account.</span></span> <span data-ttu-id="9ca8d-137">Det här används när du använder hello Ambari-Webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="9ca8d-137">This is used when you access hello Ambari Web UI.</span></span>

## <a name="use-hive-commands"></a><span data-ttu-id="9ca8d-138">Använda Hive-kommandon</span><span class="sxs-lookup"><span data-stu-id="9ca8d-138">Use Hive commands</span></span>

1. <span data-ttu-id="9ca8d-139">Använd hello följande toostart hello Hive-kommandogränssnittet från en SSH-anslutning toohello sandbox:</span><span class="sxs-lookup"><span data-stu-id="9ca8d-139">From an SSH connection toohello sandbox, use hello following command toostart hello Hive shell:</span></span>
   
        hive
2. <span data-ttu-id="9ca8d-140">När hello shell har börjat använda hello följande tooview hello tabeller som tillhandahålls med hello sandbox:</span><span class="sxs-lookup"><span data-stu-id="9ca8d-140">Once hello shell has started, use hello following tooview hello tables that are provided with hello sandbox:</span></span>
   
        show tables;
3. <span data-ttu-id="9ca8d-141">Använd hello följande tooretrieve 10 rader från hello `sample_07` tabell:</span><span class="sxs-lookup"><span data-stu-id="9ca8d-141">Use hello following tooretrieve 10 rows from hello `sample_07` table:</span></span>
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a><span data-ttu-id="9ca8d-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9ca8d-142">Next steps</span></span>
* [<span data-ttu-id="9ca8d-143">Lär dig hur toouse Visual Studio med hello Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="9ca8d-143">Learn how toouse Visual Studio with hello Hortonworks Sandbox</span></span>](hdinsight-hadoop-emulator-visual-studio.md)
* [<span data-ttu-id="9ca8d-144">Learning hello linor av hello Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="9ca8d-144">Learning hello ropes of hello Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="9ca8d-145">Hadoop-vägledning - komma igång med HDP</span><span class="sxs-lookup"><span data-stu-id="9ca8d-145">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

