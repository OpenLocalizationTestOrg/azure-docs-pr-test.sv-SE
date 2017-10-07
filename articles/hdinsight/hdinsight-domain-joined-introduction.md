---
title: "aaaHadoop security - domänanslutna HDInsight-kluster - Azure | Microsoft Docs"
description: "Läs mer ..."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/31/2016
ms.author: saurinsh
ms.openlocfilehash: 5a9469402a61bcba4017e1ff4bd06dfba23ac963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-toohadoop-security-with-domain-joined-hdinsight-clusters-preview"></a>En introduktion tooHadoop säkerhet med domänanslutna HDInsight-kluster (förhandsgranskning)

Fram till idag hade Azure HDInsight endast stöd för en enda lokal administratörsanvändare. Det har fungerat bra för mindre programteam eller avdelningar. Klass-funktioner som active directory-baserad autentisering, stöd för flera användare och rollbaserad åtkomstkontroll blev allt viktigare som Hadoop baserade arbetsbelastningar med flera popularitet inom hello enterprise, hello måste för företag. Med domänanslutna HDInsight-kluster kan du skapa HDInsight klustret anslutna tooan Active Directory-domänen, konfigurerar en lista över anställda från hello enterprise som kan autentisera via Azure Active Directory toolog på tooHDInsight klustret. Alla utanför hello enterprise kan inte logga in eller komma åt hello HDInsight-kluster. hello företagsadministratör kan konfigurera rollbaserad åtkomstkontroll för att använda Hive-säkerhet [Apache Ranger](http://hortonworks.com/apache/ranger/), vilket begränsar åtkomst till toodata tooonly så mycket som behövs. Slutligen Hej administratör kan granska hello dataåtkomst av anställda och eventuella ändringar som gjorts tooaccess styr principer kan därmed uppnå en hög grad av styrning av företagets resurser.

> [!NOTE]
> hello nya funktioner som beskrivs i den här förhandsgranskningen är bara tillgängliga på Linux-baserade HDInsight-kluster för Hive-arbetsbelastning. Hej andra arbetsbelastningar, t.ex. HBase, Spark, Storm och Kafka, aktiveras i framtida versioner.

> [!IMPORTANT]
> Oozie har inte aktiverats på domänanslutna HDInsight.

## <a name="benefits"></a>Fördelar
Enterprise Security består av fyra huvudsakliga delar – perimetersäkerhet, autentisering, auktorisering och kryptering.

![Domänanslutna HDInsight-kluster gynnar de viktiga delarna](./media/hdinsight-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Perimetersäkerhet
Perimetersäkerhet i HDInsight uppnås med hjälp av virtuella nätverk och Gateway-tjänsten. Idag är en företagsadministratör skapar ett HDInsight-kluster i ett virtuellt nätverk och använda Nätverkssäkerhetsgrupper (inkommande eller utgående brandväggsregler) toorestrict åtkomst toohello virtuellt nätverk. Endast hello IP-adresser som definierats i hello inkommande kommer brandväggsregler att kunna toocommunicate med hello HDInsight-kluster, vilket ger säkerhet. En annan nivå av perimetersäkerhet uppnås med hjälp av Gateway-tjänsten. hello Gateway är hello-tjänst som agerar som första försvarslinje för varje inkommande begäran toohello HDInsight-kluster. Den accepterar hello begäran, validerar den och tillåter bara hello begäran toopass toohello andra noder i klustret, vilket ger säkerhet tooother namn och noderna i klustret. hello.

### <a name="authentication"></a>Autentisering
Med den här offentliga förhandsversionen kan en företagsadministratör etablera ett domänanslutet HDInsight-kluster i ett [virtuellt nätverk](https://azure.microsoft.com/services/virtual-network/). hello klusternoder hello HDInsight ska vara domänansluten toohello hanteras av hello enterprise. Detta uppnås med hjälp av [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md). Alla hello-noder i klustret hello är domänansluten tooa domän som hanterar hello enterprise. Med den här installationen kan hello företagets anställda logga in toohello noder med sina domänautentiseringsuppgifter. De kan också använda sin domän autentiseringsuppgifter tooauthenticate med andra godkända slutpunkter som Hue, Ambari-vyer, ODBC, JDBC, PowerShell och REST API: er toointeract med hello-kluster. Hej administratör har full kontroll över begränsa hello antalet användare som interagerar med hello klustret via dessa slutpunkter.

### <a name="authorization"></a>Auktorisering
Bästa praxis följt av de flesta företag har inte alla medarbetare åtkomst tooall företagsresurser. Med den här versionen på samma sätt definiera Hej administratör principer för rollbaserad åtkomstkontroll för hello klusterresurser. Hej administratör kan till exempel konfigurera [Apache Ranger](http://hortonworks.com/apache/ranger/) tooset principer för åtkomstkontroll för Hive. Den här funktionen innebär att anställda kommer att kunna tooaccess så mycket data som de behöver toobe lyckas i sitt arbete. SSH åtkomst toohello klustret är också begränsad endast toohello administratör.

### <a name="auditing"></a>Granskning
Tillsammans med skydda hello HDInsight klusterresurser från obehöriga användare och datasäkerhet hello, granska alla komma åt toohello klusterresurser och hello data är nödvändiga tootrack obehörig eller oavsiktlig åtkomst av hello resurser. Med den här förhandsversionen Hej administratör visa och rapportera alla åtkomst toohello HDInsight-kluster-resurser och data. Hej administratör kan också visa och rapporten alla ändringar toohello principer för åtkomstkontroll i Apache Ranger slutpunkter som stöds. En domänansluten HDInsight-kluster använder hello bekant Apache Ranger UI toosearch granskningsloggar. På hello serverdel Ranger använder [Apache Solr](http://hortonworks.com/apache/solr/) för att lagra och söka hello loggar.

### <a name="encryption"></a>Kryptering
Skydda data är viktigt för möte organisationens säkerhets- och efterlevnadskrav och tillsammans med att begränsa åtkomst toodata från obehörig anställda, det bör också skyddas genom att kryptera den. Både hello datalager för HDInsight-kluster, Azure Storage Blob och Azure Data Lake Storage stöder transparent serversidan [kryptering av data](../storage/common/storage-service-encryption.md) i vila. Secure HDInsight-kluster fungerar sömlöst med den här funktionen för serverdelskryptering för vilande data.

## <a name="next-steps"></a>Nästa steg
* Om du vill konfigurera ett domänanslutet HDInsight-kluster kan du läsa i [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md).
* Om du vill hantera ett domänanslutet HDInsight-kluster kan du läsa i [Hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-manage.md).
* Om du vill konfigurera Hive-principer och köra Hive-frågor kan du läsa i [Konfigurera Hive-principer för domänanslutna HDInsight-kluster](hdinsight-domain-joined-run-hive.md).
* För att köra Hive-frågor med SSH på domänanslutna HDInsight-kluster, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
