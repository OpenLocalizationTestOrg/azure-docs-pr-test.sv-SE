---
title: "aaaInstall från tredje part Hadoop-program på Azure HDInsight | Microsoft Docs"
description: "Lär dig hur tooinstall från tredje part Hadoop-program på Azure HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: eaf5904d-41e2-4a5f-8bec-9dde069039c2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/16/2017
ms.author: jgao
ms.openlocfilehash: 00071517c81a17c01dccedf9e8dd5d0cabb38567
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-third-party-hadoop-applications-on-azure-hdinsight"></a>Installera Hadoop-program från tredje part i Azure HDInsight

I den här artikeln får du lära dig hur tooinstall ett redan publicerade från tredje part Hadoop-program på Azure HDInsight. Anvisningar om hur du installerar ett eget program finns i [Installera anpassade HDInsight-program](hdinsight-apps-install-custom-applications.md).

Ett HDInsight-program är ett program som användarna kan installera på ett Linux-baserat HDInsight-kluster. Dessa program kan utvecklas av Microsoft, oberoende programvaruleverantörer och av dig själv.  

För närvarande finns det fyra publicerade program:

* **DATAIKU DDS på HDInsight**: Dataiku DSS (datavetenskap Studio) är en programvara som gör att data tekniker (datavetare, affärsanalytiker, utvecklare...) tooprototype, skapa och distribuera hög specifika tjänster som omvandlar rådata till slagkraftig business förutsägelser.
* **Datameer**: [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft) erbjuder analytiker ett interaktivt sätt toodiscover, analysera och visualisera hello resultat på Big Data. Dra in ytterligare datakällor enkelt toodiscover nya relationer och få hello svar snabbt.
* **Streamsets datainsamlaren för HDnsight** tillhandahåller en komplett integrerad utvecklingsmiljö (IDE) att kan du utforma, testa, distribuera och hantera alla-till-alla infognings-pipeline-nät dataströmmen och batch data och omfattar en mängd dataströmmen transformationer – alla utan toowrite anpassad kod. 
* **Cask CDAP för HDInsight** ger hello först enhetlig integrationsplattform för stordata som minskar hello tid tooproduction för program och data sjöar med 80%. Det här programmet stöder endast Standard HBase 3.4-kluster.
* **H2O styrs av datorn för HDInsight (Beta)** H2O mousserande vattenstämplar stöder följande algoritmer för distribuerad hello: GLM, Naïve Bayes, distribuerade slumpmässiga skog, toning förstärkning datorn, djupa Neurala nätverk, djup learning K-means PCA, Generaliserad låg Rank modeller, Avvikelseidentifiering och Autoencoders.
* **Kyligence Analytics Platform** Kyligence Analytics Platform (KAP) är en enterprise-redo data warehouse drivs av Apache Kylin och Apache Hadoop, den ger sekund svarstid i massiv skala dataset och förenklar dataanalys för företagsanvändare och analytiker. 
* **SnapLogic Hadooplex** hello SnapLogic Hadooplex körs på HDInsight gör att kunder tooget toobusiness insikter snabbare genom att tillhandahålla självbetjäning datapåfyllning och förberedelse av från valfri datakälla toohello Microsoft Azure-molnet plattform.
* **Spark-jobbserver för KNIME Spark utföraren** Spark-jobbserver för KNIME Spark utföraren har använt tooconnect hello KNIME Analytics Platform tooHDInsight kluster.

hello anvisningarna i den här artikeln använder Azure-portalen. Du kan också exportera hello Azure Resource Manager-mall från hello-portalen eller skaffa en kopia av hello Resource Manager-mall från leverantörer och använda Azure PowerShell och Azure CLI toodeploy hello mallen.  Mer information finns i [Skapa Linux-baserade Hadoop-kluster i HDInsight med hjälp av Resource Manager-mallar](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Krav
Om du vill tooinstall HDInsight-program på ett befintligt HDInsight-kluster, måste du ha ett HDInsight-kluster. toocreate, se [Skapa kluster](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). Du kan även installera HDInsight-program när du skapar ett HDInsight-kluster.

## <a name="install-applications-tooexisting-clusters"></a>Installera program tooexisting kluster
hello följande procedur visar hur tooinstall HDInsight program tooan befintligt HDInsight-kluster.

**tooinstall ett HDInsight-program**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **HDInsight-kluster** hello vänstra menyn.  Om du inte ser det klickar du på **Fler tjänster** och sedan på **HDInsight-kluster**.
3. Klicka på ett HDInsight-kluster.  Om du inte har något måste du skapa ett först.  Mer information finns i [Skapa kluster](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Klicka på **program** under hello **konfigurationer** kategori. Du kan se en lista över installerade program. Om du inte hittar program, som innebär att inga program för den här versionen av hello HDInsight-kluster.
   
    ![HDInsight-program – meny på portalen](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Klicka på **Lägg till** hello bladet-menyn. 
   
    ![HDInsight-program – installerade appar](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)
   
    Du ser en lista över befintliga HDInsight-program.
   
    ![HDInsight-program – tillgängliga program](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Klicka på någon av hello program godkänner hello juridiska villkoren och klicka sedan på **Välj**.

Du kan se hello installationsstatus från portalen hello-meddelanden (klicka på hello klockikonen överst hello hello portalen). Efter hello installeras program, hello program visas på bladet för hello installerade appar.

## <a name="install-applications-during-cluster-creation"></a>Installera program när du skapar ett kluster
Du har hello alternativet tooinstall HDInsight-program när du skapar ett kluster. HDInsight-program installeras under hello efter hello klustret har skapats och är i hello körs. hello följande procedur visar hur tooinstall HDInsight-program när du skapar ett kluster.

**tooinstall ett HDInsight-program**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **Nytt**, klicka på **Data + Analys** och sedan på **HDInsight**.
3. Ange **klusternamnet**: Det här namnet måste vara globalt unikt.
4. Klicka på **prenumeration** tooselect hello Azure-prenumeration som används för hello klustret.
5. Klicka på **Välj typ av kluster** och välj sedan:
   
   * **Klustret typen**: Välj om du inte vet vilka toochoose **Hadoop**. Det är hello populäraste typ av kluster.
   * **Operativsystem**: Välj **Linux**.
   * **Version**: Använd hello standardversionen om du inte vet vilka toochoose. Mer information finns i [HDInsight-klusterversioner](hdinsight-component-versioning.md).
   * **Klustret nivå**: Azure HDInsight tillhandahåller hello molntjänster för stordata i två kategorier: standardnivån och Premium-nivån. Mer information finns i [Klusternivåer](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).
6. Klicka på **program**, klicka på en av hello publicerade program och klicka sedan på **Välj**.
7. Klicka på **autentiseringsuppgifter** och ange ett lösenord för hello administratörsanvändaren. Du måste också tooenter en **SSH-användarnamn** och antingen en **lösenord** eller **offentliga nyckel**, vilket är att använda tooauthenticate hello SSH-användare. Hello rekommenderad metod är att använda en offentlig nyckel. Klicka på **Välj** vid konfiguration av hello nedre toosave hello autentiseringsuppgifter.
8. Klicka på **datakällan**, väljer du något av de befintliga lagringskontona hello eller skapa en ny lagring konto toobe används som hello standardkontot för lagring för hello klustret.
9. Klicka på **resursgruppen** tooselect en befintlig resurs grupp eller klicka på **ny** toocreate en ny resursgrupp
10. På hello **nya HDInsight-kluster** bladet, se till att **PIN-kod tooStartboard** är markerad och klicka sedan på **skapa**. 

## <a name="list-installed-hdinsight-apps-and-properties"></a>Visa en lista med installerade HDInsight-appar och deras egenskaper
hello portal visar en lista över hello installerat HDInsight-program för ett kluster och hello egenskaper för varje installerat program.

**toolist HDInsight-program och visa egenskaper**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **HDInsight-kluster** hello vänstra menyn.  Om du inte ser det klickar du på **Bläddra** och sedan på **HDInsight-kluster**.
3. Klicka på ett HDInsight-kluster.
4. Från hello **inställningar** bladet, klickar du på **program** under hello **allmänna** kategori. hello installerade appar bladet visar alla hello installerade program. 
   
    ![HDInsight-program – installerade appar](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Klicka på en hello installerat program tooshow hello egenskapen. Hej bladet egenskapslistor:
   
   * Programnamn: programnamnet.
   * Status: programmets status. 
   * Webbsida: hello URL för hello webbprogram som du har distribuerat toohello kantnod. hello autentiseringsuppgifterna är hello samma som hello HTTP autentiseringsuppgifter som du har konfigurerat för hello klustret.
   * HTTP-slutpunkten: hello autentiseringsuppgifter är hello samma som hello HTTP autentiseringsuppgifter som du har konfigurerat för hello klustret. 
   * SSH-slutpunkten: du kan använda SSH tooconnect toohello kantnoden. hello SSH-autentiseringsuppgifter är hello samma som hello SSH autentiseringsuppgifter som du har konfigurerat för hello klustret. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).
6. toodelete ett program, högerklicka på hello program och klicka sedan på **ta bort** hello snabbmenyn.

## <a name="connect-toohello-edge-node"></a>Ansluta toohello kantnod
Du kan ansluta toohello kantnod med HTTP och SSH. hello slutpunktsinformation hittar från hello [portal](#list-installed-hdinsight-apps-and-properties). Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

hello http-slutpunkt autentiseringsuppgifterna är hello http-autentiseringsuppgifterna som du har konfigurerat för hello HDInsight-klustret. hello SSH endpoint autentiseringsuppgifterna är hello SSH-autentiseringsuppgifter som du har konfigurerat för hello HDInsight-kluster.

## <a name="troubleshoot"></a>Felsöka
Se [felsöka hello installationen](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Nästa steg
* [Installera anpassade HDInsight-program](hdinsight-apps-install-custom-applications.md): Lär dig hur toodeploy tooHDInsight för ett Opublicerat HDInsight-program.
* [Publicera HDInsight-program](hdinsight-apps-publish-applications.md): Lär dig hur toopublish din anpassade HDInsight-program tooAzure Marketplace.
* [MSDN: Installera ett HDInsight-program](https://msdn.microsoft.com/library/mt706515.aspx): Lär dig hur toodefine HDInsight-program.
* [Anpassa Linux-baserat HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md): Lär dig hur toouse skriptåtgärd tooinstall fler program.
* [Skapa Linux-baserade Hadoop-kluster i HDInsight med hjälp av Resource Manager-mallar](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Lär dig hur toocall Resource Manager-mallar toocreate HDInsight-kluster.
* [Använd tom edge-noder i HDInsight](hdinsight-apps-use-edge-node.md): Lär dig hur toouse en tom kant för att få åtkomst till HDInsight-kluster, testa HDInsight-program och värd för HDInsight-program.

