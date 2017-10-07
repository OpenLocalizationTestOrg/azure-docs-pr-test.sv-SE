---
title: aaaUse flera HDInsight-kluster med ett Azure Data Lake Store-konto - Azure | Microsoft Docs
description: "Lär dig hur toouse mer än ett HDInsight-kluster med ett enda Data Lake Store-konto"
keywords: "hdinsight lagring, hdfs, strukturerade data, Ostrukturerade data, datasjölager"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: nitinme
ms.openlocfilehash: 3a125b91fcc1e436270a1bd349f7a2d8f27e06fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-store-account"></a>Använda flera HDInsight-kluster med ett Azure Data Lake Store-konto

Från och med HDInsight version 3.5 kan skapa du HDInsight-kluster med Azure Data Lake Store-konton som hello standard filsystem.
Data Lake Store stöder obegränsad lagring som gör det perfekta inte värd för stora mängder data. men även som värd för flera HDInsight-kluster som delar ett enda Data Lake Store-konto. Instructionson hur toocreate ett HDInsight-kluster med Data Lake Store som hello lagring, se [skapa HDInsight-kluster med Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Den här artikeln innehåller rekommendationer toohello Data Lake store-administratören för att skapa en enkel och delad lagring Datasjökontot som kan användas på flera **active** HDInsight-kluster. De här rekommendationerna gäller toohosting flera säkra som inte är säkra Hadoop-kluster på ett delade Data Lake store-konto.


## <a name="data-lake-store-file-and-folder-level-acls"></a>Data Lake Store fil- och nivå ACL: er

hello resten av den här artikeln förutsätter att du har goda kunskaper om fil- och nivå ACL: er i Azure Data Lake Store, som beskrivs i detalj i [åtkomstkontroll i Azure Data Lake Store](../data-lake-store/data-lake-store-access-control.md).

## <a name="data-lake-store-setup-for-multiple-hdinsight-clusters"></a>Data Lake Store-inställningar för flera HDInsight-kluster
Låt oss ta ett två nivåer mapphierarkin tooexplain hello rekommendationer för att använda fler HDInsight-kluster med ett Data Lake Store-konto. Överväg att du har ett Data Lake Store-konto med hello mappstruktur **/kluster/ekonomi**. Med den här strukturen kan alla hello kluster hello ekonomi organisation kräver använda /clusters/finance som hello lagringsplats. I hello framtida om en annan organisation säger marknadsföring, vill toocreate HDInsight-kluster med Hej samma Data Lake Store-konto, de kan skapa /clusters/marketing. Nu ska vi använda **/kluster/ekonomi**.

tooenable som den här mappen strukturen toobe effektivt används av HDInsight-kluster hello Data Lake Store administratören måste tilldela rätt behörighet, enligt beskrivningen i hello tabell. hello behörigheter som visas i tabellen hello motsvara tooAccess ACL: er och inte standard-ACL: er. 


|Mapp  |Behörigheter  |Ägande användare  |Ägande grupp  | Namngiven användare | Namngiven användarbehörighet | Namngiven grupp | Namngivna gruppbehörigheter |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x--x  |Admin |Admin  |Tjänstens huvudnamn |--x  |FINGRP   |r-x         |
|/Clusters | rwxr-x--x |Admin |Admin |Tjänstens huvudnamn |--x  |FINGRP |r-x         |
|/ kluster/ekonomi | rwxr-x--t |Admin |FINGRP  |Tjänstens huvudnamn |rwx  |-  |-     |

I hello tabell

- **Admin** är hello skapare och administratör i hello Data Lake Store-konto.
- **Tjänstens huvudnamn** är hello Azure Active Directory (AAD) tjänstens huvudnamn som associeras med hello-konto.
- **FINGRP** är en användargrupp som skapats i AAD som innehåller användare från hello ekonomi-organisation.

Anvisningar för hur toocreate ett AAD-program (som skapar också ett huvudnamn för tjänsten), se [skapa ett AAD-program](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application). Anvisningar för hur toocreate en användargrupp i AAD, se [hantera grupper i Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

Några viktiga punkter tooconsider.

- hello två nivåer mappstrukturen (**/kluster/ekonomi/**) måste skapas och etablerats med rätt behörighet av Hej Data Lake Store administratör **innan** använder hello storage-konto för kluster. Den här strukturen skapas inte automatiskt när du skapar kluster.
- hello-exemplet ovan rekommenderar inställningen hello äger grupp **/kluster/ekonomi** som **FINGRP** och gör det möjligt att **r-x** komma åt tooFINGRP toohello hela mapphierarki startar från hello rot. Detta säkerställer att hello medlemmar i FINGRP kan navigera hello mappstrukturen från roten.
- I hello fallet när olika AAD tjänstens huvudnamn kan skapa kluster under **/kluster/ekonomi**, Fäst hello-bitars (på hello **ekonomi** mapp) så att mappar skapas en tjänst Säkerhetsobjekt kan inte tas bort av hello andra.
- När hello mappstrukturen och behörigheterna är på plats, HDInsight klusterskapandeprocessen skapar en klusterspecifika lagring loaction under **/kluster/ekonomi/**. Hello lagring för ett kluster med hello namnet fincluster01 kan till exempel vara **/clusters/finance/fincluster01**. hello ägarskap och behörigheter för hello mappar som skapas av HDInsight-klustret visas i här hello-tabellen.

    |Mapp  |Behörigheter  |Ägande användare  |Ägande grupp  | Namngiven användare | Namngiven användarbehörighet | Namngiven grupp | Namngivna gruppbehörigheter |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/Clusters/finanace/fincluster01 | rwxr x---  |Tjänstens huvudnamn |FINGRP  |- |-  |-   |-  | 
   


## <a name="recommendations-for-job-input-and-output-data"></a>Rekommendationer för jobbet indata och utdata

Vi rekommenderar att ange data tooa jobb och hello utdata från ett jobb lagras i en mapp utanför **/kluster**. Detta säkerställer att även om hello klusterspecifika mappen har tagits bort tooreclaim vissa lagringsutrymme, hello jobbet indata och utdata är fortfarande tillgänglig för framtida användning. Kontrollera att hello mapphierarkin för att lagra hello jobbet in- och utdataenheter tillåter rätt åtkomstnivå för hello tjänstens huvudnamn i så fall.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>Begränsa på kluster som delar ett enda storage-konto

hello gränsen för hello antalet kluster som kan dela ett enda Data Lake Store-konto är beroende av hello arbetsbelastningen som körs på dessa kluster. Med för många kluster eller väldigt tunga arbetsbelastningar om hello-kluster som delar ett lagringskonto kan orsaka hello storage-konto ingång-/ utgång tooget begränsas.

## <a name="support-for-default-acls"></a>Stöd för standard-ACL: er

När du skapar ett huvudnamn för tjänsten med namnet användaråtkomst (som visas i hello tabellen ovan), rekommenderar vi **inte** att lägga till hello med namnet användare med en standard-ACL. Etablering med namnet användaren åtkomst med standard-ACL: er resultat hello tilldelning av 770 behörigheter för ägande användare och äger gruppen. När det här standardvärdet 770 inte tar bort behörigheter från det ägande användare (7) eller äger-grupp (7), men det tar bort alla behörigheter för andra (0). Detta resulterar i ett känt problem med en viss användningsfall som diskuteras i detalj i hello [kända problem och lösningar](#known-issues-and-workarounds) avsnitt.

## <a name="known-issues-and-workarounds"></a>Kända problem och lösningar

Det här avsnittet innehåller hello kända problem för att använda HDInsight med Data Lake Store och sina lösningar.

### <a name="publicly-visible-localized-yarn-resources"></a>Synligt offentligt lokaliserade YARN-resurser

När ett nytt Azure Data Lake store-konto har skapats kan etableras automatiskt hello rotkatalog med ACL Access behörighet bits set too770. hello Rotmappens äger är uppsättningen toohello användaren som skapade hello konto (hello Data Lake Store) och toohello primär grupp för hello-användaren som skapade hello konto anges hello ägande grupp. Ingen åtkomst har angetts för ”övriga”.

De här inställningarna är kända tooaffect specifika HDInsight-användningsfall i [YARN 247](https://hwxmonarch.atlassian.net/browse/YARN-247). Jobbet bidrag kan misslyckas med ett fel meddelande liknande toothis:

    Resource XXXX is not publicly accessible and as such cannot be part of hello public cache.

Enligt informationen i hello YARN JIRA länkade tidigare under lokaliseringen offentliga resurser hello localizer verifierar att alla hello är begärda resurserna verkligen offentliga genom att kontrollera deras behörigheter för hello fjärr-filsystemet. Alla LocalResource som inte uppfyller villkoret avvisas för lokalisering. Hej Kontrollera behörigheter, innehåller läsbehörighet toohello-fil för ”övriga”. Det här scenariot fungerar inte out box när värd HDInsight-kluster i Azure Data Lake eftersom Azure Data Lake nekar åtkomst för ”andra” på rotnivå för mappen.

#### <a name="workaround"></a>Lösning
Ange Läs-körningsbehörighet **andra** genom hello-hierarkin, till exempel på  **/** , **/kluster** och   **/kluster/ekonomi**  enligt hello tabellen ovan.

## <a name="see-also"></a>Se även

* [Skapa ett HDInsight-kluster med Data Lake Store som lagring](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)


