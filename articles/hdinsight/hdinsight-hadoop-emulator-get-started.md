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
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a>Kom igång med Hadoop-sandbox, en emulator på en virtuell dator

Lär dig hur tooinstall hello Hadoop sandbox från Hortonworks på en virtuell dator toolearn om hello Hadoop-ekosystemet. hello sandbox tillhandahåller en lokal utveckling miljö toolearn om Hadoop, Hadoop Distributed File System (HDFS) och skicka jobbet. När du är bekant med Hadoop, kan du börja använda Hadoop i Azure genom att skapa ett HDInsight-kluster. Mer information om hur tooget igång finns [Kom igång med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

## <a name="prerequisites"></a>Krav
* [Oracle VirtualBox](https://www.virtualbox.org/). Hämta och installera den från [här](https://www.virtualbox.org/wiki/Downloads).



## <a name="download-and-install-hello-virtual-machine"></a>Hämta och installera hello virtuell dator
1. Bläddra toohello [Hortonworks hämtar](http://hortonworks.com/downloads/#sandbox).

2. Klicka på **hämta VIRTUALBOX** toodownload hello senaste Hortonworks Sandbox på en virtuell dator. Du kan ange tooregister med Hortonworks innan hello hämtningen påbörjas. Det tar en tootwo timmar toodownload beroende på nätverkets hastighet.
   
    ![Länka bild för att ladda ned Hortonworks Sandbox för VirtualBox](./media/hdinsight-hadoop-emulator-get-started/download-sandbox.png)
3. Från Hej samma webbsida, klicka på hello **Import på virtuella** länka toodownload en PDF-fil som innehåller instruktioner för installation för hello virtuella datorn.

toodownload ett äldre HDP version sandbox Expandera hello Arkiv:

![Hortonworks Sandbox-Arkiv](./media/hdinsight-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-hello-virtual-machine"></a>Starta hello virtuell dator

1. Öppna Oracle VM VirtualBox.
2. Från hello **filen** -menyn klickar du på **importera installation**, och sedan ange hello Hortonworks Sandbox avbildningen.
1. Klicka på Välj hello Hortonworks Sandbox **starta**, och sedan **Normal Start**. När hello virtuell dator har slutförts hello startprocessen visar instruktioner för inloggning.
   
    ![Normal start](./media/hdinsight-hadoop-emulator-get-started/normal-start.png)
2. Öppna en webbläsare och gå toohello URL visas (vanligtvis http://127.0.0.1:8888).

## <a name="set-sandbox-passwords"></a>Ange Sandbox lösenord

1. Från hello **Kom igång** steg i hello Hortonworks Sandbox-sidan, Välj **visa avancerade alternativ**. Använd hello information på den här sidan toolog i toohello sandbox via SSH. Använd hello namn och lösenord angavs.
   
   > [!NOTE]
   > Om du inte har en SSH-klienten installerad kan du använda hello webbaserade SSH anges i hello virtuell dator på **http://localhost:4200 /**.
   > 
   
    hello kan första gången du ansluter med SSH, du ange toochange hello lösenordet för rotkontot hello. Ange ett nytt lösenord som du använder när du loggar in via SSH.

2. Efter loggat in kan du ange hello följande kommando:
   
        ambari-admin-password-reset
   
    När du uppmanas ange ett lösenord för hello Ambari-administratörskonto. Det här används när du använder hello Ambari-Webbgränssnittet.

## <a name="use-hive-commands"></a>Använda Hive-kommandon

1. Använd hello följande toostart hello Hive-kommandogränssnittet från en SSH-anslutning toohello sandbox:
   
        hive
2. När hello shell har börjat använda hello följande tooview hello tabeller som tillhandahålls med hello sandbox:
   
        show tables;
3. Använd hello följande tooretrieve 10 rader från hello `sample_07` tabell:
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a>Nästa steg
* [Lär dig hur toouse Visual Studio med hello Hortonworks Sandbox](hdinsight-hadoop-emulator-visual-studio.md)
* [Learning hello linor av hello Hortonworks Sandbox](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop-vägledning - komma igång med HDP](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

