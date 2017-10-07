---
title: aaaDomain anslutna Azure HDInsight-arkitektur | Microsoft Docs
description: "Lär dig hur tooplan domänanslutna HDInsight."
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
ms.date: 02/03/2017
ms.author: saurinsh
ms.openlocfilehash: 1c3ecedf3739b4f8fa54160225be9c1d6e2ca6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planera Azure-domänanslutna Hadoop-kluster i HDInsight

hello är traditionella Hadoop ett kluster med en användare. Det passar för de flesta företag som har små applikationsteam som bygger stordataarbetsbelastningar. Hadoop ökar i popularitet och många företag går mot en modell där kluster hanteras av IT-team, och flera applikationsteam delar kluster. Funktioner som rör fleranvändar-kluster är bland hello därför mest begärda funktionaliteten i Azure HDInsight.

I stället för att skapa flera användare autentisering och auktorisering, använder HDInsight hello populäraste identitetsleverantör--Active Directory (AD). hello kraftfulla säkerhetsfunktionerna i AD kan vara används toomanage fleranvändar auktorisering i HDInsight. Genom att integrera HDInsight med AD kan du kommunicera med hello kluster med hjälp av AD-autentiseringsuppgifter. HDInsight mappar en AD användaren tooa lokala Hadoop-användare, så alla hello tjänster som körs på HDInsight (Ambari, Hive server Ranger Spark thrift-servern och andra) fungerar sömlöst för hello autentiserade användare.

## <a name="integrate-hdinsight-with-ad-and-ad-on-iaas-vm"></a>Integrera HDInsight med AD och AD på IaaS-VM

Genom att integrera HDInsight med Azure AD eller AD på Iaas-VM är hello HDInsight-klusternoder domänanslutna tooa domän. HDInsight skapar tjänstens huvudnamn för hello Hadoop-tjänster som körs på klustret hello och placerar dem i en angiven organisationsenhet (OU) i Azure AD eller AD på IaaS-VM. HDInsight skapas också omvänd DNS-mappningar i hello domän för hello IP-adresser för hello-noder som är kopplade toohello domän.

Det finns flera olika arkitekturer som du kan använda för att uppnå den här konfigurationen. Du kan välja mellan följande arkitekturer hello.

**HDInsight integrerad med AD som körs på Azure IaaS**

Detta är hello enklaste arkitektur för att integrera HDInsight med Active Directory. hello AD-domänkontrollant som körs på en (eller flera) virtuella datorer (VM) i Azure. Dessa virtuella datorer finns vanligtvis i ett virtuellt nätverk. Du kan konfigurera ett annat virtuellt nätverk för hello HDInsight-kluster. För HDInsight toohave en fri tooActive Directory måste toopeer dessa virtuella nätverk med hjälp av [VNet-till-VNet-peering](../virtual-network/virtual-network-create-peering.md). Om du skapar hello Active Directory i ARM, och du kan skapa hello Active Directory och HDInsight i hello samma virtuella nätverk och du behöver inte toodo peering. 

![Domänansluten HDInsight-klustertopologi](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> Du kan inte använda Azure Data Lake Store med hello HDInsight-kluster i den här arkitekturen.


Krav för Active Directory:

* En [organisationsenhet](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) måste skapas inom du placera hello HDInsight-klustrets virtuella datorer och hello tjänstens huvudnamn som används av hello klustret.
* [Protokoll för Lightweight Directory Access](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPs) måste ställas in för att kommunicera med AD. hello certifikatet som används för tooset in LDAPS måste vara en verklig certifikat (inte ett självsignerat certifikat).
* Omvänd DNS-zoner måste skapas på hello domän för hello IP-adressintervall hello HDInsight undernät (till exempel 10.2.0.0/24 i hello föregående bild).
* Ett tjänstkonto eller ett användarkonto krävs. Använd det här kontot toocreate hello HDInsight-kluster. Det här kontot måste ha hello följande behörigheter:

    - Behörigheter toocreate service principal och datorobjekt inom hello organisationsenhet
    - Behörigheter toocreate omvänd DNS-proxy regler
    - Behörigheter toojoin datorer toohello Active Directory-domän

**HDInsight integrerad med endast molnbaserad Azure AD**

För endast molnbaserad Azure AD måste du konfigurera en domänkontrollant så att HDInsight kan integreras med Azure AD. Detta uppnås med hjälp av [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS). Azure AD DS skapar domän domänkontroller-datorer på hello molnet och tillhandahåller IP-adresser för dessa. Två domänkontrollanter skapas för hög tillgänglighet.

Azure AD DS finns för närvarande bara i klassiska virtuella nätverk. Det är bara tillgänglig med hello klassiska Azure-portalen. Hej HDInsight virtuella nätverk finns i hello Azure-portalen som måste toobe peerkopplat med hello klassiskt virtuellt nätverk med hjälp av VNet-till-VNet-peering.

> [!NOTE]
> Peering mellan ett klassiskt virtuellt nätverk och en Azure Resource Manager-virtuella nätverk som kräver att båda virtuella nätverken är i samma region hello och hello under samma Azure-prenumeration.

![Domänansluten HDInsight-klustertopologi](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Krav för Azure AD:

* En [organisationsenhet](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) måste skapas inom du placera hello HDInsight-klustrets virtuella datorer och hello tjänstens huvudnamn som används av hello klustret.
* [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) måste konfigureras när du konfigurerar Azure AD DS. hello certifikatet som används för tooset in LDAPS måste vara en verklig certifikat (inte ett självsignerat certifikat).
* Omvänd DNS-zoner måste skapas på hello domän för hello IP-adressintervall hello HDInsight undernät (till exempel 10.2.0.0/24 i hello föregående bild).
* [Lösenordshashvärden](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) måste synkroniseras från Azure AD tooAzure AD DS.
* Ett tjänstkonto eller ett användarkonto krävs. Använd det här kontot toocreate hello HDInsight-kluster. Det här kontot måste ha hello följande behörigheter:

    - Behörigheter toocreate service principal och datorobjekt inom hello organisationsenhet
    - Behörigheter toocreate omvänd DNS-proxy regler
    - Behörigheter toojoin datorer toohello Azure AD-domän

## <a name="next-steps"></a>Nästa steg
* tooconfigure ett domänanslutna HDInsight-kluster, se [konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md).
* toomanage domänanslutna HDInsight-kluster, se [hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-manage.md).
* tooconfigure Hive och kör Hive-frågor, se [konfigurera Hive principer för domänanslutna HDInsight-kluster](hdinsight-domain-joined-run-hive.md).
* toorun Hive-frågor med hjälp av SSH på domänanslutna HDInsight-kluster, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
