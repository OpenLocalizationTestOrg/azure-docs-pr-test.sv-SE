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
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a>Installera RStudio med R Server på HDInsight

Den här artikeln beskriver hur tooinstall hello community (kostnadsfritt) version av [RStudio Server](https://www.rstudio.com/products/rstudio-server/) på hello edge nod i ett kluster med ett anpassat skript. RStudio Server innehåller en webbläsarbaserad IDE för användning av fjärranslutna klienter och som används ofta i Linux. Det finns flera integrerad utvecklingsmiljöer (IDEs) för R idag, inklusive:

- Microsofts [R Tools för Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) 
- [RStudio Server](https://www.rstudio.com/products/rstudio-server/) 
- Walware har Eclipse-baserade [StatET](http://www.walware.de/goto/statet).

hello fördelen att installera RStudio på hello kantnod för ett HDInsight-kluster är att det ger en fullständig IDE-upplevelse för hello utveckling och körning av R-skript med R Server på hello klustret. Den här konfigurationen kan vara betydligt mer effektiva än standardinställningen för användning av hello R-konsolen.

> [!NOTE]
> hello proceduren som beskrivs i den här artikeln gäller endast om du inte valde tooinstall RStudio Server community edition vid etablering av klustret. Om du lade till den under etableringen, sedan tooaccess det du klickar på hello **R Server instrumentpaneler** panelen i hello Azure portal post för klustret och sedan på hello **R Studio Server** panelen. 

Om du föredrar toouse hello kommersiellt licensierad Pro version av RStudio Server, måste du följa hello Installationsinstruktioner från [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).

> [!NOTE]
> Om du använder ett HDInsight-kluster som R har installerats med hello [installera R skriptåtgärd](hdinsight-hadoop-r-scripts-linux.md), hello stegen i det här dokumentet fungerar inte korrekt eftersom de kräver en R Server på hello HDInsight-kluster.
>
> 

## <a name="prerequisites"></a>Krav

* Ett Azure HDInsight-kluster med R Server installerat. Instruktioner finns i [komma igång med R Server på HDInsight-kluster](hdinsight-hadoop-r-server-get-started.md).
* En SSH-klient. För Linux och Unix-distributioner eller Mac OS X, hello `ssh` kommando kan användas med hello-operativsystem. För Windows, rekommenderar vi [Cygwin](http://www.redhat.com/services/custom/cygwin/) med hello [OpenSSH alternativet](https://www.youtube.com/watch?v=CwYSvvGaiWU), eller [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  

## <a name="install-rstudio-on-hello-cluster-using-a-custom-script"></a>Installera RStudio på hello-kluster med ett anpassat skript

Här är hello steg:

1. Identifiera hello kantnod hello-klustret. Följande är ett HDInsight-kluster med R Server hello namngivningskonvention huvudnod och kantnod.
   * Huvudnod`CLUSTERNAME-ssh.azurehdinsight.net`
   * Kantnod`CLUSTERNAME-ed-ssh.azurehdinsight.net` 

2. SSH i hello kantnod av hello-kluster med hjälp av hello mönstret i steg 1. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

3. När du är ansluten bli rotanvändare på hello klustret. Använd hello följande kommando i hello SSH-sessionen:

        sudo su -

4. Hämta hello anpassat skript tooinstall RStudio. Använd hello följande kommando:

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Ändra hello behörigheter på hello anpassade skriptfilen och kör hello-skript. Använd hello följande kommandon:

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Om du använder ett SSH-lösenord när du skapar ett HDInsight-kluster med R Server, kan du hoppa över detta steg och fortsätta toohello nästa. Om du använder en SSH-nyckel i stället toocreate hello kluster, måste du ange ett lösenord för SSH-användare. Du behöver det här lösenordet när du ansluter tooRStudio. Kör följande kommandon hello:

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. När du tillfrågas om **aktuella Kerberos lösenord**, tryck på **RETUR**.  Observera att du måste ersätta `USERNAME` med en SSH-användare för din HDInsight-kluster. Om lösenordet är korrekt, bör du se hello följande meddelande:

        passwd: password updated successfully

    Avsluta hello SSH-session.

8. Skapa en SSH-tunnel toohello kluster genom att mappa `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` på hello HDInsight-kluster toohello klientdatorn. Du måste skapa en SSH-tunnel innan du öppnar en ny webbläsarsession.

   * På en Linux-klient eller en Windows-klient med [Cygwin](http://www.redhat.com/services/custom/cygwin/), öppna en terminalsession och Använd hello följande kommando:

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       Ersätt **användarnamn** med en SSH-användare för din HDInsight-kluster och Ersätt **KLUSTERNAMN** med hello namnet på ditt HDInsight-kluster.
       Du kan också använda en SSH-nyckel i stället för ett lösenord genom att lägga till `-i id_rsa_key`.        
   * Om du använder en Windows-klienter med PuTTY sedan

     1. Öppna PuTTY och ange anslutningsinformationen.
     2. I hello **kategori** avsnitt toohello till vänster i dialogrutan hello, expandera **anslutning**, expandera **SSH**, och välj sedan **tunnlar**.
     3. Ange följande information på hello hello **alternativ som styr SSH-port vidarebefordran** formulär:

        * **Källport** -hello port på hello-klient som du vill tooforward. Till exempel **8787**.
        * **Mål** - hello mål som måste vara mappat toohello lokala klientdatorn. Till exempel **localhost:8787**.

            ![Skapa en SSH-tunnel](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "skapa en SSH-tunnel")

     4. Klicka på **Lägg till** tooadd hello inställningar och klickar sedan på **öppna** tooopen en SSH-anslutning.
     5. När du uppmanas att logga in toohello server tooestablish en SSH-session och aktivera hello-tunnel.

9. Öppna en webbläsare och ange följande URL-Adressen baserat på hello-port som du angav för hello tunnel hello:

        http://localhost:8787/ 

10. Du kan ange tooenter hello SSH användarnamn och lösenord tooconnect toohello klustret. Om du har använt en SSH-nyckel när du skapar hello kluster, måste du ange hello lösenordet du skapade i steg 5.

    ![Ansluta tooR Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "skapa en SSH-tunnel")

11. tootest om hello RStudio installationen lyckades, kör du ett testskript som körs R-baserade MapReduce och Spark jobb på hello klustret. toodownload hello test skriptet toorun i RStudio, gå tillbaka toohello SSH-konsolen och ange hello följande kommandon:

    *    Om du har skapat ett Hadoop-kluster med R, Använd följande kommando:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r
    *    Om du har skapat ett Spark-kluster med R, Använd följande kommando:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

12. I RStudio ser du hello testa skript som du hämtat. Dubbelklicka på filen hello tooopen, Välj hello innehållet i hello-filen och klicka sedan på **kör**. Du bör se utdata hello i hello **konsolen** fönstret:

   ![Testa installationen av hello](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "testa hello-installation")

Ett annat alternativ är tootype `source(testhdi.r)` eller `source(testhdi_spark.r)` tooexecute hello skript.

## <a name="see-also"></a>Se även

* [Compute-kontexten alternativ för R Server på HDInsight-kluster](hdinsight-hadoop-r-server-compute-contexts.md)
* [Alternativ för Azure Storage för R Server på HDInsight](hdinsight-hadoop-r-server-storage.md)

