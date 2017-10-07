---
title: aaaPublish HDInsight-program - Azure | Microsoft Docs
description: "Lär dig hur toocreate och publicerar HDInsight-program."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 14aef891-7a37-4cf1-8f7d-ca923565c783
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 7da0aa53828563e50ef372df901e1ba541fb40be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-hdinsight-applications-into-hello-azure-marketplace"></a>Publicera HDInsight-program hello Azure Marketplace
Ett HDInsight-program är ett program som användarna kan installera på ett Linux-baserat HDInsight-kluster. Dessa program kan utvecklas av Microsoft, oberoende programvaruleverantörer och av dig själv. I den här artikeln får du lära dig hur toopublish ett HDInsight-program i hello Azure Marketplace.  Allmän information om hur du publicerar hello Azure Marketplace finns [publicera ett erbjudande toohello Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

HDInsight-program använda hello *Bring Your Own License (BYOL)* modellen, där programmets provider ansvarar för licensiering hello tooend-programanvändare och slutanvändarna endast debiteras av Azure för hello resurser de Skapa till exempel hello HDInsight-kluster och virtuella datorer/noder. Fakturering för själva hello programmet görs för närvarande inte via Azure.

Andra HDInsight programrelaterad artikel:

* [Installera HDInsight-program](hdinsight-apps-install-applications.md): Lär dig hur tooinstall tooyour för programmet ett HDInsight-kluster.
* [Installera anpassade HDInsight-program](hdinsight-apps-install-custom-applications.md): Lär dig hur tooinstall och testa anpassade HDInsight-program.

## <a name="prerequisites"></a>Krav
toosubmit toohello marketplace för din anpassade program du har skapat och testat det anpassade programmet. Se hello följande artiklar:

* [Installera anpassade HDInsight-program](hdinsight-apps-install-custom-applications.md): Lär dig hur tooinstall och testa anpassade HDInsight-program.

Du måste också ha registrerat ett utvecklarkonto. Se [publicera ett erbjudande toohello Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) och [skapa ett konto på Microsoft Developer](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Definiera programmet
Det finns två steg för att publicera program toohello Azure Marketplace.  Först definierar du en **createUiDef.json** filen tooindicate som kluster programmet är kompatibelt med. sedan publicerar hello-mall från hello Azure-portalen. hello efter avsnittet är ett exempel createUiDef.json-fil.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


| Fält | Beskrivning | Möjliga värden |
| --- | --- | --- |
| typer |Hej klustertyper hello program är kompatibla med. |Hadoop, HBase, Storm, Spark (eller en kombination av dem) |
| nivåer |Hej klusternivåer hello program är kompatibla med. |Standard, Premium (eller båda) |
| versioner |Hej HDInsight klustertyper hello program är kompatibla med. |3.4 |

## <a name="application-install-script"></a>Installationsskriptet för programmet
När ett program har installerats på ett kluster (en befintlig eller en ny), en kantnod har skapats och hello installationsskriptet för programmet körs på den.
  > [!IMPORTANT]
  > installationsskriptet för hello programmet hello namn måste vara unikt för ett visst kluster med hello följande format.
  > 
  > name": "[concat('hue-install-v0','-' ,uniquestring(‘applicationName’)]"
  > 
  > Observera att det finns tre delar toohello skriptets namn:
  > 
  > 1. Ett skriptnamnsprefix, vilket omfattar hello programnamnet eller ett namn på relevanta toohello program.
  > 2. Ett "-" för läsbarhet.
  > 3. En unik Strängfunktion med hello programnamn som hello parametern.
  > 
  > Ett exempel är hello ovan avslutas blir: hue-install-v0-4wkahss55hlas i hello bestående skript listan över. Ett exempel på en JSON-nyttolast finns på [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).
  > 
hello installationsskriptet måste ha hello följande egenskaper:
1. Kontrollera att idempotent hello skript. Flera anrop toohello skript ska producera hello samma resultat.
2. hello skript ska vara korrekt version. Använd en annan plats för hello skript när du uppgraderar eller testa ändringar så att kunder som försöker tooinstall hello programmet inte påverkas. 
3. Lägga till lämpliga loggning toohello skript på varje plats. Vanligtvis hello skriptet loggar är hello endast sätt toodebug problem med installation av programmet.
4. Kontrollera att anrop tooexternal tjänster eller resurser har tillräcklig återförsök så att hello installation inte påverkas av tillfälliga nätverksproblem.
5. Om skriptet startar tjänsterna på hello noder, kontrollera att hello tjänster som ska övervakas och konfigurerade toostart automatiskt vid omstarter av noden.

## <a name="package-application"></a>Paketera programmet
Skapa en zip-fil som innehåller alla filer som krävs för att installera dina HDInsight-program. Du behöver hello zip-filen i [publicera program](#publish-application).

* [createUiDefinition.json](#define-application).
* mainTemplate.json. Du hittar exempel i [Installera anpassade HDInsight-program](hdinsight-apps-install-custom-applications.md).
* Alla nödvändiga skript.

> [!NOTE]
> Hej programfilerna (inklusive webbprogramfiler om sådana finns) kan finnas på valfri offentligt tillgänglig slutpunkt.
> 

## <a name="publish-application"></a>Publicera programmet
Följ hello följande steg toopublish ett HDInsight-program:

1. Logga in toohello [Azure Publishing portal](https://publish.windowsazure.com/).
2. Klicka på **lösningsmallar** från hello vänstra toocreate en ny lösningsmall.
3. Ange ett namn och klicka på **Skapa en ny lösningsmall**.
4. Klicka på **skapa Dev Center-konto och Anslut till hello Azure programmet** tooregister ditt företag om du inte gjort det.  Se [Skapa ett Microsoft-utvecklarkonto](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
5. Klicka på **definiera vissa topologier tooget igång**. En lösningsmall är en ”överordnad” tooall dess topologier. Du kan definiera flera topologier i en erbjudande-/lösningsmall. När ett erbjudande pushas toostaging pushas den med alla sina topologier. 
6. Ange ett namn på topologin och klicka på hello plustecken.
7. Ange en ny version och klicka på hello plustecknet.
8. Överför hello zip-filen som skapades i [paketera program](#package-application).  
9. Klicka på **Begär certifiering**. hello Microsofts certifieringsteam granskar hello filer och certifiera hello-topologi.

## <a name="next-steps"></a>Nästa steg
* [Installera HDInsight-program](hdinsight-apps-install-applications.md): Lär dig hur tooinstall tooyour för programmet ett HDInsight-kluster.
* [Installera anpassade HDInsight-program](hdinsight-apps-install-custom-applications.md): Lär dig hur toodeploy tooHDInsight för ett Opublicerat HDInsight-program.
* [Anpassa Linux-baserat HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md): Lär dig hur toouse skriptåtgärd tooinstall fler program.
* [Skapa Linux-baserade Hadoop-kluster i HDInsight med hjälp av Azure Resource Manager-mallar](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Lär dig hur toocall Resource Manager-mallar toocreate HDInsight-kluster.
* [Använd tom edge-noder i HDInsight](hdinsight-apps-use-edge-node.md): Lär dig hur toouse en tom kant för att få åtkomst till HDInsight-kluster, testa HDInsight-program och värd för HDInsight-program.

