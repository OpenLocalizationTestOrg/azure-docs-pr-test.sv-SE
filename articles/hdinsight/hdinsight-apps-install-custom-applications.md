---
title: "aaaInstall egna anpassade Hadoop-program på Azure HDInsight | Microsoft Docs"
description: "Lär dig hur tooinstall HDInsight-program på HDInsight-program."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e556b29c-8176-4bc5-a90b-aa01abfd3aee
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: ed3148f2c4d4d2b568d84e44fa6d76bb5a001902
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-custom-hadoop-applications-on-azure-hdinsight"></a>Installera anpassade Hadoop-program i Azure HDInsight

I den här artikeln får du lära dig hur tooinstall ett Hadoop-program på Azure HDInsight, som inte har publicerats toohello Azure-portalen. hello-programmet som du ska installera på den här artikeln är [Hue](http://gethue.com/).

Ett HDInsight-program är ett program som användarna kan installera på ett Linux-baserat HDInsight-kluster.  Dessa program kan utvecklas av Microsoft, oberoende programvaruleverantörer och av dig själv.  

Andra relaterade artiklar:

* [Installera HDInsight-program](hdinsight-apps-install-applications.md): Lär dig hur tooinstall tooyour för programmet ett HDInsight-kluster.
* [Publicera HDInsight-program](hdinsight-apps-publish-applications.md): Lär dig hur toopublish din anpassade HDInsight-program tooAzure Marketplace.
* [MSDN: Installera ett HDInsight-program](https://msdn.microsoft.com/library/mt706515.aspx): Lär dig hur toodefine HDInsight-program.

## <a name="prerequisites"></a>Krav
Om du vill tooinstall HDInsight-program på ett befintligt HDInsight-kluster, måste du ha ett HDInsight-kluster. toocreate, se [Skapa kluster](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). Du kan även installera HDInsight-program när du skapar ett HDInsight-kluster.

## <a name="install-hdinsight-applications"></a>Installera HDInsight-program
HDInsight-program kan installeras när du skapar ett kluster eller tooan befintliga HDInsight-kluster. Information om hur du definierar Azure Resource Manager-mallar finns i [MSDN: Installera ett HDInsight-program](https://msdn.microsoft.com/library/mt706515.aspx).

hello-filer som behövs för att distribuera programmet (Hue):

* [azuredeploy.JSON](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/azuredeploy.json): hello Resource Manager-mall för att installera HDInsight-program. Information om hur du utvecklar en egen Resource Manager-mall finns i [MSDN: Installera ett HDInsight-program](https://msdn.microsoft.com/library/mt706515.aspx).
* [Hue-install_v0.sh](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/scripts/Hue-install_v0.sh): hello skriptåtgärd som anropas av hello Resource Manager-mall för att konfigurera hello kantnod.
* [Hue-binaries.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): hello hue-binärfilen som anropas från hui-install_v0.sh.
* [Hue-binärfiler-14-04.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): hello hue-binärfilen som anropas från hui-install_v0.sh.
* [webwasb-tomcat.tar.gz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/webwasb-tomcat.tar.gz): En exempelwebbapp (Tomcat) som anropas från hui-install_v0.sh.

**tooinstall-Hue tooan befintligt HDInsight-kluster**

1. Klicka på hello efter bilden toosign i tooAzure och öppna hello Resource Manager-mall i hello Azure-portalen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FHue%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/deploy-to-azure.png" alt="Deploy tooAzure"></a>

    Den här knappen öppnar en Resource Manager-mall på hello Azure-portalen.  hello Resource Manager-mall finns i [https://github.com/hdinsight/Iaas-Applications/tree/master/Hue](https://github.com/hdinsight/Iaas-Applications/tree/master/Hue).  toolearn hur toowrite Resource Manager-mallen, se [MSDN: installera ett HDInsight-program](https://msdn.microsoft.com/library/mt706515.aspx).
2. Från hello **parametrar** bladet anger hello följande:

   * **Klusternamn**: Ange hello namnet på hello klustret där du vill tooinstall hello program. Det här klustret måste vara ett befintligt kluster.
3. Klicka på **OK** toosave hello parametrar.
4. Från hello **anpassad distribution** bladet ange **resursgruppen**.  hello resursgrupp är en behållare som grupperar klustret hello, hello beroende lagringskontot och andra resurser. Det krävs toouse hello samma resursgrupp som hello kluster.
5. Klicka på **Juridiska villkor** och sedan på **Skapa**.
6. Kontrollera hello **PIN-kod toodashboard** kryssrutan är markerad och klicka sedan på **skapa**. Du kan se hello installationsstatus från hello panelen Fäst toohello portalens instrumentpanel och hello portalmeddelandet (klicka på hello klockikonen överst hello hello portalen).  Det tar cirka 10 minuter tooinstall hello program.

**tooinstall Hue när du skapar ett kluster**

1. Klicka på hello efter bilden toosign i tooAzure och öppna hello Resource Manager-mall i hello Azure-portalen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhdinsightapps%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/deploy-to-azure.png" alt="Deploy tooAzure"></a>

    Den här knappen öppnar en Resource Manager-mall på hello Azure-portalen.  hello Resource Manager-mall finns i [https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json).  toolearn hur toowrite Resource Manager-mallen, se [MSDN: installera ett HDInsight-program](https://msdn.microsoft.com/library/mt706515.aspx).
2. Följ hello instruktion toocreate kluster och installera Hue. Mer information om hur du skapar HDInsight-kluster finns i [Skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

Dessutom toohello Azure-portalen, du kan också använda [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-powershell) och [Azure CLI](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-cli) toocall Resource Manager-mallar.

## <a name="validate-hello-installation"></a>Verifiera hello installationen
Du kan kontrollera status för användning av hello på hello Azure portal toovalidate hello programinstallation. Dessutom kan du verifiera att alla HTTP-slutpunkter visats som förväntat och webbsidan hello om det finns en:

**tooopen hello Hue-portalen**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **HDInsight-kluster** hello vänstra menyn.  Om du inte ser det klickar du på **Bläddra** och sedan på **HDInsight-kluster**.
3. Klicka på hello klustret där du installerade programmet hello.
4. Från hello **inställningar** bladet, klickar du på **program** under hello **allmänna** kategori. Du bör se **hue** som anges i hello **installerade appar** bladet.
5. Klicka på **hue** från hello toolist hello egenskaper.  
6. Klicka på hello webbsidan länken toovalidate hello webbplats; Öppna hello HTTP-slutpunkt i en webbläsare toovalidate hello Hue-webbgränssnittet, öppna hello SSH-slutpunkten med hjälp av SSH. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

## <a name="troubleshoot-hello-installation"></a>Felsöka hello-installationen
Du kan kontrollera hello programmets installationsstatus i hello portalmeddelandet (klicka på hello klockikonen överst hello hello portalen).

Om en programinstallation misslyckas, kan du se hello felmeddelanden och felsökningsinformation på 3 platser:

* HDInsight-program: allmän felinformation.

    Öppna hello kluster från hello-portalen och på program från hello inställningsbladet:

    ![hdinsight-program programinstallationsfel](./media/hdinsight-apps-install-applications/hdinsight-apps-error.png)
* HDInsight-skriptåtgärder: om hello HDInsight-programmets felmeddelande anger ett skript har misslyckats, mer information om fel för hello skript kommer att visas i åtgärdsfönstret för hello skript.

    Klicka på skriptåtgärd i bladet hello. Historiken för skriptåtgärder visas felmeddelanden hello

    ![hdinsight-program skriptåtgärdsfel](./media/hdinsight-apps-install-applications/hdinsight-apps-script-action-error.png)
* Ambari-webbgränssnitt: Om hello installationsskriptet hello felorsaken med hello, använda Ambari-Webbgränssnittet toocheck fullständig loggarna om installationsskripten hello.

    Mer information finns i [Felsökning](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting).

## <a name="remove-hdinsight-applications"></a>Ta bort HDInsight-program
Det finns flera sätt toodelete HDInsight-program.

### <a name="use-portal"></a>Använda portalen
**tooremove ett program med hjälp av hello portal**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **HDInsight-kluster** hello vänstra menyn.  Om du inte ser det klickar du på **Bläddra** och sedan på **HDInsight-kluster**.
3. Klicka på hello klustret där du installerade programmet hello.
4. Från hello **inställningar** bladet, klickar du på **program** under hello **allmänna** kategori. En lista med installerade program visas. Den här självstudien **hue** som anges i hello **installerade appar** bladet.
5. Högerklicka på hello program du vill tooremove och klicka sedan på **ta bort**.
6. Klicka på **Ja** tooconfirm.

Från hello-portalen kan du också ta bort hello klustret eller ta bort hello resursgruppen som innehåller programmet hello.

### <a name="use-azure-powershell"></a>Använda Azure PowerShell
Med Azure PowerShell kan du ta bort hello klustret eller ta bort hello resursgruppen. Se [Ta bort kluster med hjälp av Azure PowerShell](hdinsight-administer-use-powershell.md#delete-clusters).

### <a name="use-azure-cli"></a>Använda Azure CLI
Använder Azure CLI, kan du ta bort hello klustret eller ta bort hello resursgruppen. Se [Ta bort kluster med hjälp av Azure CLI](hdinsight-administer-use-command-line.md#delete-clusters).

## <a name="next-steps"></a>Nästa steg
* [MSDN: Installera ett HDInsight-program](https://msdn.microsoft.com/library/mt706515.aspx): Lär dig hur toodevelop Resource Manager-mallar för distribution av HDInsight-program.
* [Installera HDInsight-program](hdinsight-apps-install-applications.md): Lär dig hur tooinstall tooyour för programmet ett HDInsight-kluster.
* [Publicera HDInsight-program](hdinsight-apps-publish-applications.md): Lär dig hur toopublish din anpassade HDInsight-program tooAzure Marketplace.
* [Anpassa Linux-baserat HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md): Lär dig hur toouse skriptåtgärd tooinstall fler program.
* [Skapa Linux-baserade Hadoop-kluster i HDInsight med hjälp av Resource Manager-mallar](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Lär dig hur toocall Resource Manager-mallar toocreate HDInsight-kluster.
* [Använd tom edge-noder i HDInsight](hdinsight-apps-use-edge-node.md): Lär dig hur toouse en tom kant för att få åtkomst till HDInsight-kluster, testa HDInsight-program och värd för HDInsight-program.
