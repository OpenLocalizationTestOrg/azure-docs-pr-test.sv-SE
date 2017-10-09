---
title: "aaaManage domänanslutna HDInsight-kluster - Azure | Microsoft Docs"
description: "Lär dig hur toomanage domänanslutna HDInsight-kluster"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 233ddf0953e981f9a24b77d9dde194d590e5e6d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a>Hantera domänanslutna HDInsight-kluster (förhandsgranskning)
Lär dig hello användare och hello roller i domänanslutna HDInsight och hur toomanage domänanslutna HDInsight-kluster.

## <a name="users-of-domain-joined-hdinsight-clusters"></a>Användare av domänanslutna HDInsight-kluster
Ett HDInsight-kluster som inte är ansluten till domänen har två konton som skapas när hello klustret skapas:

* **Ambari admin**: det här kontot kallas även *Hadoop användare* eller *HTTP användaren*. Det här kontot kan vara används toolog på tooAmbari på https://&lt;klusternamn >. azurehdinsight.net. Det kan också använda toorun frågor på Ambari-vyer, köra jobb via externa verktyg (d.v.s. PowerShell, Templeton, Visual Studio) och autentisera med hello ODBC-drivrutinen och BI-verktyg (d.v.s. Excel, PowerBI eller Tableau).
* **SSH-användare**: det här kontot kan användas med SSH och köra sudo-kommandon. Den har rot privilegier toohello virtuella Linux-datorer.

En domänansluten HDInsight-kluster har tre nya användare i tillägg tooAmbari Admin och SSH-användare.

* **Ranger admin**: det här kontot är hello lokala Apache Ranger-administratörskonto. Det är inte en användare för active directory-domän. Det här kontot kan använda toosetup principer och göra andra användare, Administratörer eller delegerade administratörer (så att användarna kan hantera principer). Som standard hello användarnamn är *admin* och hello lösenord är hello samma som hello Ambari administratörslösenord. hello lösenord kan uppdateras från hello inställningssidan i Ranger.
* **Klustret admin domänanvändare**: det här kontot är en active directory-domänanvändare som angetts som hello Hadoop-kluster admin inklusive Ambari och Ranger. Du måste ange den här användarens autentiseringsuppgifter när klustret skapas. Den här användaren har hello följande behörigheter:

  * Ansluta datorer toohello domänen och placera dem i hello Organisationsenhet som du anger när klustret skapas.
  * Skapa tjänstens huvudnamn i hello Organisationsenhet som du anger när klustret skapas.
  * Skapa omvänd DNS-poster.

    Obs hello andra AD-användare har även dessa behörigheter.

    Det finns vissa slutpunkter inom hello kluster (till exempel Templeton) som inte hanteras av Ranger och därför inte är säker. De här slutpunkterna är låsta för alla användare utom hello klustret admin domänanvändare.
* **Vanliga**: du kan ange flera active directory-grupper när klustret skapas. hello användare i dessa grupper kommer att synkroniserade tooRanger och Ambari. Dessa användare har åtkomst tooonly Ranger-hanterade slutpunkter (till exempel Hiveserver2) är domänanvändare. Alla hello RBAC principer och granskning är tillämpliga toothese användare.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Roller för domänanslutna HDInsight-kluster
Domänanslutna HDInsight har hello följande roller:

* Klusteradministratören
* Kluster-operatorn
* Tjänstadministratör
* Tjänsten Operator
* Kluster-användare

**toosee hello behörigheter för dessa roller**

1. Öppna hello Ambari Management Användargränssnittet.  Se [öppna hello Ambari Management UI](#open-the-ambari-management-ui).
2. Hello vänstra menyn klickar du på **roller**.
3. Klicka på hello blå frågetecken toosee hello behörigheter:

    ![Behörigheter för domänanslutna HDInsight-roller](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-hello-ambari-management-ui"></a>Öppna hello Ambari Management UI
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Öppna ditt HDInsight-kluster i ett blad. Se [listan och visa](hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Klicka på **instrumentpanelen** från hello huvudmenyn tooopen Ambari.
4. Logga in tooAmbari med hello klustret administratör domänanvändarnamn och lösenord.
5. Klicka på hello **Admin** listrutan från hello övre högra hörnet och klicka sedan på **hantera Ambari**.

    ![Domänanslutna HDInsight hantera Ambari](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    Det ser ut så hello UI:

    ![Domänanslutna HDInsight Ambari hanteringsgränssnittet](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-hello-domain-users-synchronized-from-your-active-directory"></a>Visa en lista över hello domänanvändare som synkroniseras från din Active Directory
1. Öppna hello Ambari Management Användargränssnittet.  Se [öppna hello Ambari Management UI](#open-the-ambari-management-ui).
2. Hello vänstra menyn klickar du på **användare**. Du bör se alla hello användare som synkroniseras från Active Directory toohello HDInsight-kluster.

    ![Domänanslutna HDInsight Ambari management UI listan användare](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-hello-domain-groups-synchronized-from-your-active-directory"></a>Visa en lista över hello domängrupper som synkroniseras från din Active Directory
1. Öppna hello Ambari Management Användargränssnittet.  Se [öppna hello Ambari Management UI](#open-the-ambari-management-ui).
2. Hello vänstra menyn klickar du på **grupper**. Du bör se alla hello grupper som synkroniseras från Active Directory toohello HDInsight-kluster.

    ![Domänanslutna HDInsight Ambari UI lista hanteringsgrupper](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Konfigurera behörigheter för Hive-vyer
1. Öppna hello Ambari Management Användargränssnittet.  Se [öppna hello Ambari Management UI](#open-the-ambari-management-ui).
2. Hello vänstra menyn klickar du på **vyer**.
3. Klicka på **HIVE** tooshow hello information.

    ![Domänanslutna HDInsight Ambari management UI Hive-vyer](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Klicka på hello **Hive-vy** länka tooconfigure Hive-vyer.
5. Bläddra nedåt toohello **behörigheter** avsnitt.

    ![Konfigurera behörigheter för domänanslutna HDInsight Ambari management UI Hive-vyer](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Klicka på **Lägg till användare** eller **Lägg till grupp**, och sedan ange hello användare eller grupper som kan använda Hive vyer.

## <a name="configure-users-for-hello-roles"></a>Konfigurera användare för hello roller
 toosee en lista över roller och deras behörigheter finns [roller för domänanslutna HDInsight-kluster](#roles-of-domain---joined-hdinsight-clusters).

1. Öppna hello Ambari Management Användargränssnittet.  Se [öppna hello Ambari Management UI](#open-the-ambari-management-ui).
2. Hello vänstra menyn klickar du på **roller**.
3. Klicka på **Lägg till användare** eller **Lägg till grupp** tooassign användare och grupper toodifferent roller.

## <a name="next-steps"></a>Nästa steg
* Om du vill konfigurera ett domänanslutet HDInsight-kluster kan du läsa i [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md).
* Om du vill konfigurera Hive-principer och köra Hive-frågor kan du läsa i [Konfigurera Hive-principer för domänanslutna HDInsight-kluster](hdinsight-domain-joined-run-hive.md).
* För att köra Hive-frågor med SSH på domänanslutna HDInsight-kluster, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
