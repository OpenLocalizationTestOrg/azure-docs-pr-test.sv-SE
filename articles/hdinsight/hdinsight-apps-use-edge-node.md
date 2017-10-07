---
title: aaaUse tom edge-noder i Hadoop-kluster i HDInsight - Azure | Microsoft Docs
description: "Hur tooadd en tom edge nod tooan HDInsight-kluster som kan användas som en klient och sedan testa/host HDInsight-program."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: cdc7d1b4-15d7-4d4d-a13f-c7d3a694b4fb
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: 9c910905b51f2fe92e6e5d47d86a32bd5247c2cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-empty-edge-nodes-on-hadoop-clusters-in-hdinsight"></a>Använda tom edge noder på Hadoop-kluster i HDInsight

Lär dig hur tooadd en tom kant nod tooan HDInsight-kluster. En tom kantnod är en virtuell Linux-dator med hello samma klientverktyg installeras och konfigureras som hello headnodes, men utan några Hadoop-tjänster som körs. Du kan använda hello kantnod för att komma åt hello kluster, testa ditt klientprogram och värd för ditt program. 

Du kan lägga till ett tomt edge-noden tooan befintligt HDInsight-kluster, tooa nytt kluster när du skapar hello kluster. Lägga till en tom kantnod görs med hjälp av Azure Resource Manager-mall.  hello följande exempel visar hur du gör med hjälp av en mall:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [ "[concat('Microsoft.HDInsight/clusters/',parameters('clusterName'))]" ],
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "Standard_D3"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "[parameters('installScriptAction')]",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Som det visas i exemplet hello, kan du om du vill anropa en [skript åtgärd](hdinsight-hadoop-customize-cluster-linux.md) tooperform ytterligare konfigurering, till exempel installera [Apache Hue](hdinsight-hadoop-hue-linux.md) i hello kantnod. hello skriptet åtgärdsskriptet måste vara allmänt tillgänglig på hello-webbplatsen.  Till exempel om hello skriptet lagras i Azure storage, använda offentliga behållare eller offentliga blobar.

hello edge nodstorlek virtuella måste uppfylla hello HDInsight-kluster worker nod vm storlek krav. Hello rekommenderade worker nod vm-storlekar, finns [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).

När du har skapat en kantnod kan du ansluta toohello kantnod med SSH och kör klienten verktyg tooaccess hello Hadoop-kluster i HDInsight.

> [!WARNING] 
> Använda en tom kantnod med HDInsight är för närvarande under förhandsgranskning. Anpassade komponenter som är installerade på hello kantnod få kommersiellt rimliga support från Microsoft. Detta kan resultera i att lösa eventuella problem. Eller du hänvisade toocommunity resurser för ytterligare hjälp. hello följande är några av de flesta aktiva platser för att få hjälp från hello community hello:
>
> * [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * [http://StackOverflow.com](http://stackoverflow.com).
>
> Om du använder en Apache-teknik kan du kanske kan toofind hjälp via hello Apache projektwebbplatser på [http://apache.org](http://apache.org), till exempel hello [Hadoop](http://hadoop.apache.org/) plats.

## <a name="add-an-edge-node-tooan-existing-cluster"></a>Lägg till ett befintligt kluster för edge noden tooan
I det här avsnittet använder du en Resource Manager mallen tooadd ett edge nod tooan befintliga HDInsight-kluster.  hello Resource Manager-mall finns i [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/). hello Resource Manager-mall anropar en skriptåtgärd på https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. hello skript utföra inte några åtgärder.  Det är toodemonstrate anropar skriptåtgärd från en Resource Manager-mall.

**tooadd ett tomt edge nod tooan befintligt kluster**

1. Skapa ett HDInsight-kluster om du inte har någon ännu.  Se [Hadoop-vägledning: komma igång med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klicka på hello efter bilden toosign i tooAzure och öppna hello Azure Resource Manager-mall i hello Azure-portalen. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. Konfigurera hello följande egenskaper:
   
   * **Prenumerationen**: Välj en Azure-prenumeration används för att skapa hello klustret.
   * **Resursgruppen**: Välj hello resursgruppens namn används för hello befintligt HDInsight-kluster.
   * **Plats**: välja hello platsen för hello befintligt HDInsight-kluster.
   * **Klusternamn**: Ange hello namnet på ett befintligt HDInsight-kluster.
   * **Kant nodstorlek**: Välj något av hello VM-storlekar. hello vm-storlek måste uppfylla hello worker nod vm storlek krav. Hello rekommenderade worker nod vm-storlekar, finns [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).
   * **Kant nod prefixet**: hello standardvärdet är **nya**.  Med standardvärdet för hello hello edge nodnamnet är **nya edgenode**.  Du kan anpassa hello prefix från hello-portalen. Du kan också anpassa hello fullständiga namn från hello mall.

4. Kontrollera **acceptera toohello villkoren ovan**, och klicka sedan på **inköp** toocreate hello kantnod.

>[!IMPORTANT]
> Se till att tooselect hello Azure-resursgrupp för hello befintliga HDInsight-kluster.  Annars felmeddelande hello meddelandet ”kan inte utföra åtgärden på kapslad resurs. Överordnade resursen '&lt;klusternamn >' hittades inte ”.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Lägg till en kantnod när du skapar ett kluster
I det här avsnittet använder du en Resource Manager mallen toocreate HDInsight-kluster med en kantnod.  hello Resource Manager-mall finns i hello [Azure QuickStart mallgalleriet](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/). hello Resource Manager-mall anropar en skriptåtgärd på https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. hello skript utföra inte några åtgärder.  Det är toodemonstrate anropar skriptåtgärd från en Resource Manager-mall.

**tooadd ett tomt edge nod tooan befintligt kluster**

1. Skapa ett HDInsight-kluster om du inte har någon ännu.  Se [komma igång med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klicka på hello efter bilden toosign i tooAzure och öppna hello Azure Resource Manager-mall i hello Azure-portalen. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. Konfigurera hello följande egenskaper:
   
   * **Prenumerationen**: Välj en Azure-prenumeration används för att skapa hello klustret.
   * **Resursgruppen**: skapa en ny resursgrupp som används för hello klustret.
   * **Plats**: Välj en plats för hello resursgrupp.
   * **Klusternamn**: Ange ett namn för hello nya kluster toocreate.
   * **Klustrets inloggningsnamn**: Ange hello Hadoop HTTP-användarnamn.  hello standardnamnet är **admin**.
   * **Klustret inloggningslösenordet**: Ange hello Hadoop HTTP användarens lösenord.
   * **SSH användarnamn**: Ange hello SSH-användarnamn. hello standardnamnet är **sshuser**.
   * **SSH lösenord**: Ange hello SSH användarens lösenord.
   * **Installera skriptåtgärd**: Behåll standardvärdet för hello för att gå igenom den här kursen.
     
     Vissa egenskaper har hårdkodad i hello mall: typ av kluster, kluster worker-nodsantalet, Edge nodstorlek och Edge nodnamn.
4. Kontrollera **acceptera toohello villkoren ovan**, och klicka sedan på **inköp** toocreate hello kluster med hello kantnod.

## <a name="access-an-edge-node"></a>Komma åt en kantnod
hello kantnod ssh-slutpunkten är &lt;EdgeNodeName >.&lt; Klusternamn >-ssh.azurehdinsight.net:22.  Till exempel nya-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

hello kantnod visas som ett program på hello Azure-portalen.  hello portal ger du hello information tooaccess hello kant noden med hjälp av SSH.

**tooverify hello edge nod SSH slutpunkt**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Öppna hello HDInsight-kluster med en kantnod.
3. Klicka på **program** från hello klusterbladet. Du bör se hello kantnod.  hello standardnamnet är **nya edgenode**.
4. Klicka på hello kantnod. Du bör se hello SSH-slutpunkten.

**toouse Hive på hello kantnod**

1. Använda SSH tooconnect toohello kantnoden. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. När du har anslutit toohello kantnod med SSH, Använd följande Hive kommandokonsolen tooopen hello hello:
   
        hive
3. Hello kör följande kommando tooshow Hive-tabeller i hello kluster:
   
        show tables;

## <a name="delete-an-edge-node"></a>Ta bort en kantnod
Du kan ta bort en kantnod hello Azure-portalen.

**tooaccess en kantnod**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Öppna hello HDInsight-kluster med en kantnod.
3. Klicka på **program** från hello klusterbladet. Visas en lista över edge noder.  
4. Högerklicka på hello kantnod du toodelete, och klickar sedan på **ta bort**.
5. Klicka på **Ja** tooconfirm.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur tooadd en kantnod och hur tooaccess hello kantnod. toolearn se fler hello följande artiklar:

* [Installera HDInsight-program](hdinsight-apps-install-applications.md): Lär dig hur tooinstall tooyour för programmet ett HDInsight-kluster.
* [Installera anpassade HDInsight-program](hdinsight-apps-install-custom-applications.md): Lär dig hur toodeploy tooHDInsight för ett Opublicerat HDInsight-program.
* [Publicera HDInsight-program](hdinsight-apps-publish-applications.md): Lär dig hur toopublish din anpassade HDInsight-program tooAzure Marketplace.
* [MSDN: Installera ett HDInsight-program](https://msdn.microsoft.com/library/mt706515.aspx): Lär dig hur toodefine HDInsight-program.
* [Anpassa Linux-baserat HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md): Lär dig hur toouse skriptåtgärd tooinstall fler program.
* [Skapa Linux-baserade Hadoop-kluster i HDInsight med hjälp av Resource Manager-mallar](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Lär dig hur toocall Resource Manager-mallar toocreate HDInsight-kluster.

