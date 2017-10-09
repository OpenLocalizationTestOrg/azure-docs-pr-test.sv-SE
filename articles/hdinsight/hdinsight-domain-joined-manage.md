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
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a><span data-ttu-id="eaaec-103">Hantera domänanslutna HDInsight-kluster (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="eaaec-103">Manage Domain-joined HDInsight clusters (Preview)</span></span>
<span data-ttu-id="eaaec-104">Lär dig hello användare och hello roller i domänanslutna HDInsight och hur toomanage domänanslutna HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="eaaec-104">Learn hello users and hello roles in Domain-joined HDInsight, and how toomanage domain-joined HDInsight clusters.</span></span>

## <a name="users-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="eaaec-105">Användare av domänanslutna HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="eaaec-105">Users of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="eaaec-106">Ett HDInsight-kluster som inte är ansluten till domänen har två konton som skapas när hello klustret skapas:</span><span class="sxs-lookup"><span data-stu-id="eaaec-106">An HDInsight cluster that is not domain-joined has two user accounts that are created during hello cluster creation:</span></span>

* <span data-ttu-id="eaaec-107">**Ambari admin**: det här kontot kallas även *Hadoop användare* eller *HTTP användaren*.</span><span class="sxs-lookup"><span data-stu-id="eaaec-107">**Ambari admin**: This account is also known as *Hadoop user* or *HTTP user*.</span></span> <span data-ttu-id="eaaec-108">Det här kontot kan vara används toolog på tooAmbari på https://&lt;klusternamn >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="eaaec-108">This account can be used toolog on tooAmbari at https://&lt;clustername>.azurehdinsight.net.</span></span> <span data-ttu-id="eaaec-109">Det kan också använda toorun frågor på Ambari-vyer, köra jobb via externa verktyg (d.v.s. PowerShell, Templeton, Visual Studio) och autentisera med hello ODBC-drivrutinen och BI-verktyg (d.v.s. Excel, PowerBI eller Tableau).</span><span class="sxs-lookup"><span data-stu-id="eaaec-109">It can also be used toorun queries on Ambari views, execute jobs via external tools (i.e. PowerShell, Templeton, Visual Studio), and authenticate with hello Hive ODBC driver and BI tools (i.e. Excel, PowerBI, or Tableau).</span></span>
* <span data-ttu-id="eaaec-110">**SSH-användare**: det här kontot kan användas med SSH och köra sudo-kommandon.</span><span class="sxs-lookup"><span data-stu-id="eaaec-110">**SSH user**:  This account can be used with SSH, and execute sudo commands.</span></span> <span data-ttu-id="eaaec-111">Den har rot privilegier toohello virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="eaaec-111">It has root privileges toohello Linux VMs.</span></span>

<span data-ttu-id="eaaec-112">En domänansluten HDInsight-kluster har tre nya användare i tillägg tooAmbari Admin och SSH-användare.</span><span class="sxs-lookup"><span data-stu-id="eaaec-112">A domain-joined HDInsight cluster has three new users in addition tooAmbari Admin and SSH user.</span></span>

* <span data-ttu-id="eaaec-113">**Ranger admin**: det här kontot är hello lokala Apache Ranger-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="eaaec-113">**Ranger admin**:  This account is hello local Apache Ranger admin account.</span></span> <span data-ttu-id="eaaec-114">Det är inte en användare för active directory-domän.</span><span class="sxs-lookup"><span data-stu-id="eaaec-114">It is not an active directory domain user.</span></span> <span data-ttu-id="eaaec-115">Det här kontot kan använda toosetup principer och göra andra användare, Administratörer eller delegerade administratörer (så att användarna kan hantera principer).</span><span class="sxs-lookup"><span data-stu-id="eaaec-115">This account can be used toosetup policies and make other users admins or delegated admins (so that those users can manage policies).</span></span> <span data-ttu-id="eaaec-116">Som standard hello användarnamn är *admin* och hello lösenord är hello samma som hello Ambari administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="eaaec-116">By default, hello username is *admin* and hello password is hello same as hello Ambari admin password.</span></span> <span data-ttu-id="eaaec-117">hello lösenord kan uppdateras från hello inställningssidan i Ranger.</span><span class="sxs-lookup"><span data-stu-id="eaaec-117">hello password can be updated from hello Settings page in Ranger.</span></span>
* <span data-ttu-id="eaaec-118">**Klustret admin domänanvändare**: det här kontot är en active directory-domänanvändare som angetts som hello Hadoop-kluster admin inklusive Ambari och Ranger.</span><span class="sxs-lookup"><span data-stu-id="eaaec-118">**Cluster admin domain user**: This account is an active directory domain user designated as hello Hadoop cluster admin including Ambari and Ranger.</span></span> <span data-ttu-id="eaaec-119">Du måste ange den här användarens autentiseringsuppgifter när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="eaaec-119">You must provide this user’s credentials during cluster creation.</span></span> <span data-ttu-id="eaaec-120">Den här användaren har hello följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="eaaec-120">This user has hello following privileges:</span></span>

  * <span data-ttu-id="eaaec-121">Ansluta datorer toohello domänen och placera dem i hello Organisationsenhet som du anger när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="eaaec-121">Join machines toohello domain and place them within hello OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="eaaec-122">Skapa tjänstens huvudnamn i hello Organisationsenhet som du anger när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="eaaec-122">Create service principals within hello OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="eaaec-123">Skapa omvänd DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="eaaec-123">Create reverse DNS entries.</span></span>

    <span data-ttu-id="eaaec-124">Obs hello andra AD-användare har även dessa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="eaaec-124">Note hello other AD users also have these privileges.</span></span>

    <span data-ttu-id="eaaec-125">Det finns vissa slutpunkter inom hello kluster (till exempel Templeton) som inte hanteras av Ranger och därför inte är säker.</span><span class="sxs-lookup"><span data-stu-id="eaaec-125">There are some end points within hello cluster (for example, Templeton) which are not managed by Ranger, and hence are not secure.</span></span> <span data-ttu-id="eaaec-126">De här slutpunkterna är låsta för alla användare utom hello klustret admin domänanvändare.</span><span class="sxs-lookup"><span data-stu-id="eaaec-126">These end points are locked down for all users except hello cluster admin domain user.</span></span>
* <span data-ttu-id="eaaec-127">**Vanliga**: du kan ange flera active directory-grupper när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="eaaec-127">**Regular**: During cluster creation, you can provide multiple active directory groups.</span></span> <span data-ttu-id="eaaec-128">hello användare i dessa grupper kommer att synkroniserade tooRanger och Ambari.</span><span class="sxs-lookup"><span data-stu-id="eaaec-128">hello users in these groups will be synced tooRanger and Ambari.</span></span> <span data-ttu-id="eaaec-129">Dessa användare har åtkomst tooonly Ranger-hanterade slutpunkter (till exempel Hiveserver2) är domänanvändare.</span><span class="sxs-lookup"><span data-stu-id="eaaec-129">These users are domain users and will have access tooonly Ranger-managed endpoints (for example, Hiveserver2).</span></span> <span data-ttu-id="eaaec-130">Alla hello RBAC principer och granskning är tillämpliga toothese användare.</span><span class="sxs-lookup"><span data-stu-id="eaaec-130">All hello RBAC policies and auditing will be applicable toothese users.</span></span>

## <a name="roles-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="eaaec-131">Roller för domänanslutna HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="eaaec-131">Roles of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="eaaec-132">Domänanslutna HDInsight har hello följande roller:</span><span class="sxs-lookup"><span data-stu-id="eaaec-132">Domain-joined HDInsight have hello following roles:</span></span>

* <span data-ttu-id="eaaec-133">Klusteradministratören</span><span class="sxs-lookup"><span data-stu-id="eaaec-133">Cluster Administrator</span></span>
* <span data-ttu-id="eaaec-134">Kluster-operatorn</span><span class="sxs-lookup"><span data-stu-id="eaaec-134">Cluster Operator</span></span>
* <span data-ttu-id="eaaec-135">Tjänstadministratör</span><span class="sxs-lookup"><span data-stu-id="eaaec-135">Service Administrator</span></span>
* <span data-ttu-id="eaaec-136">Tjänsten Operator</span><span class="sxs-lookup"><span data-stu-id="eaaec-136">Service Operator</span></span>
* <span data-ttu-id="eaaec-137">Kluster-användare</span><span class="sxs-lookup"><span data-stu-id="eaaec-137">Cluster User</span></span>

<span data-ttu-id="eaaec-138">**toosee hello behörigheter för dessa roller**</span><span class="sxs-lookup"><span data-stu-id="eaaec-138">**toosee hello permissions of these roles**</span></span>

1. <span data-ttu-id="eaaec-139">Öppna hello Ambari Management Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="eaaec-139">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="eaaec-140">Se [öppna hello Ambari Management UI](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="eaaec-140">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="eaaec-141">Hello vänstra menyn klickar du på **roller**.</span><span class="sxs-lookup"><span data-stu-id="eaaec-141">From hello left menu, click **Roles**.</span></span>
3. <span data-ttu-id="eaaec-142">Klicka på hello blå frågetecken toosee hello behörigheter:</span><span class="sxs-lookup"><span data-stu-id="eaaec-142">Click hello blue question mark toosee hello permissions:</span></span>

    ![Behörigheter för domänanslutna HDInsight-roller](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-hello-ambari-management-ui"></a><span data-ttu-id="eaaec-144">Öppna hello Ambari Management UI</span><span class="sxs-lookup"><span data-stu-id="eaaec-144">Open hello Ambari Management UI</span></span>
1. <span data-ttu-id="eaaec-145">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="eaaec-145">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="eaaec-146">Öppna ditt HDInsight-kluster i ett blad.</span><span class="sxs-lookup"><span data-stu-id="eaaec-146">Open your HDInsight cluster in a blade.</span></span> <span data-ttu-id="eaaec-147">Se [listan och visa](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="eaaec-147">See [List and show clusters](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span></span>
3. <span data-ttu-id="eaaec-148">Klicka på **instrumentpanelen** från hello huvudmenyn tooopen Ambari.</span><span class="sxs-lookup"><span data-stu-id="eaaec-148">Click **Dashboard** from hello top menu tooopen Ambari.</span></span>
4. <span data-ttu-id="eaaec-149">Logga in tooAmbari med hello klustret administratör domänanvändarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="eaaec-149">Log on tooAmbari using hello cluster administrator domain user name and password.</span></span>
5. <span data-ttu-id="eaaec-150">Klicka på hello **Admin** listrutan från hello övre högra hörnet och klicka sedan på **hantera Ambari**.</span><span class="sxs-lookup"><span data-stu-id="eaaec-150">Click hello **Admin** dropdown menu from hello upper right corner, and then click **Manage Ambari**.</span></span>

    ![Domänanslutna HDInsight hantera Ambari](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    <span data-ttu-id="eaaec-152">Det ser ut så hello UI:</span><span class="sxs-lookup"><span data-stu-id="eaaec-152">hello UI looks like:</span></span>

    ![Domänanslutna HDInsight Ambari hanteringsgränssnittet](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-hello-domain-users-synchronized-from-your-active-directory"></a><span data-ttu-id="eaaec-154">Visa en lista över hello domänanvändare som synkroniseras från din Active Directory</span><span class="sxs-lookup"><span data-stu-id="eaaec-154">List hello domain users synchronized from your Active Directory</span></span>
1. <span data-ttu-id="eaaec-155">Öppna hello Ambari Management Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="eaaec-155">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="eaaec-156">Se [öppna hello Ambari Management UI](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="eaaec-156">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="eaaec-157">Hello vänstra menyn klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="eaaec-157">From hello left menu, click **Users**.</span></span> <span data-ttu-id="eaaec-158">Du bör se alla hello användare som synkroniseras från Active Directory toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="eaaec-158">You shall see all hello users synced from your Active Directory toohello HDInsight cluster.</span></span>

    ![Domänanslutna HDInsight Ambari management UI listan användare](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-hello-domain-groups-synchronized-from-your-active-directory"></a><span data-ttu-id="eaaec-160">Visa en lista över hello domängrupper som synkroniseras från din Active Directory</span><span class="sxs-lookup"><span data-stu-id="eaaec-160">List hello domain groups synchronized from your Active Directory</span></span>
1. <span data-ttu-id="eaaec-161">Öppna hello Ambari Management Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="eaaec-161">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="eaaec-162">Se [öppna hello Ambari Management UI](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="eaaec-162">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="eaaec-163">Hello vänstra menyn klickar du på **grupper**.</span><span class="sxs-lookup"><span data-stu-id="eaaec-163">From hello left menu, click **Groups**.</span></span> <span data-ttu-id="eaaec-164">Du bör se alla hello grupper som synkroniseras från Active Directory toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="eaaec-164">You shall see all hello groups synced from your Active Directory toohello HDInsight cluster.</span></span>

    ![Domänanslutna HDInsight Ambari UI lista hanteringsgrupper](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a><span data-ttu-id="eaaec-166">Konfigurera behörigheter för Hive-vyer</span><span class="sxs-lookup"><span data-stu-id="eaaec-166">Configure Hive Views permissions</span></span>
1. <span data-ttu-id="eaaec-167">Öppna hello Ambari Management Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="eaaec-167">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="eaaec-168">Se [öppna hello Ambari Management UI](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="eaaec-168">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="eaaec-169">Hello vänstra menyn klickar du på **vyer**.</span><span class="sxs-lookup"><span data-stu-id="eaaec-169">From hello left menu, click **Views**.</span></span>
3. <span data-ttu-id="eaaec-170">Klicka på **HIVE** tooshow hello information.</span><span class="sxs-lookup"><span data-stu-id="eaaec-170">Click **HIVE** tooshow hello details.</span></span>

    ![Domänanslutna HDInsight Ambari management UI Hive-vyer](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. <span data-ttu-id="eaaec-172">Klicka på hello **Hive-vy** länka tooconfigure Hive-vyer.</span><span class="sxs-lookup"><span data-stu-id="eaaec-172">Click hello **Hive View** link tooconfigure Hive Views.</span></span>
5. <span data-ttu-id="eaaec-173">Bläddra nedåt toohello **behörigheter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="eaaec-173">Scroll down toohello **Permissions** section.</span></span>

    ![Konfigurera behörigheter för domänanslutna HDInsight Ambari management UI Hive-vyer](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. <span data-ttu-id="eaaec-175">Klicka på **Lägg till användare** eller **Lägg till grupp**, och sedan ange hello användare eller grupper som kan använda Hive vyer.</span><span class="sxs-lookup"><span data-stu-id="eaaec-175">Click **Add User** or **Add Group**, and then specify hello users or groups that can use Hive Views.</span></span>

## <a name="configure-users-for-hello-roles"></a><span data-ttu-id="eaaec-176">Konfigurera användare för hello roller</span><span class="sxs-lookup"><span data-stu-id="eaaec-176">Configure users for hello roles</span></span>
 <span data-ttu-id="eaaec-177">toosee en lista över roller och deras behörigheter finns [roller för domänanslutna HDInsight-kluster](#roles-of-domain---joined-hdinsight-clusters).</span><span class="sxs-lookup"><span data-stu-id="eaaec-177">toosee a list of roles and their permissions, see [Roles of Domain-joined HDInsight clusters](#roles-of-domain---joined-hdinsight-clusters).</span></span>

1. <span data-ttu-id="eaaec-178">Öppna hello Ambari Management Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="eaaec-178">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="eaaec-179">Se [öppna hello Ambari Management UI](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="eaaec-179">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="eaaec-180">Hello vänstra menyn klickar du på **roller**.</span><span class="sxs-lookup"><span data-stu-id="eaaec-180">From hello left menu, click **Roles**.</span></span>
3. <span data-ttu-id="eaaec-181">Klicka på **Lägg till användare** eller **Lägg till grupp** tooassign användare och grupper toodifferent roller.</span><span class="sxs-lookup"><span data-stu-id="eaaec-181">Click **Add User** or **Add Group** tooassign users and groups toodifferent roles.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eaaec-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eaaec-182">Next steps</span></span>
* <span data-ttu-id="eaaec-183">Om du vill konfigurera ett domänanslutet HDInsight-kluster kan du läsa i [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="eaaec-183">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="eaaec-184">Om du vill konfigurera Hive-principer och köra Hive-frågor kan du läsa i [Konfigurera Hive-principer för domänanslutna HDInsight-kluster](hdinsight-domain-joined-run-hive.md).</span><span class="sxs-lookup"><span data-stu-id="eaaec-184">For configuring Hive policies and run Hive queries, see [Configure Hive policies for Domain-joined HDInsight clusters](hdinsight-domain-joined-run-hive.md).</span></span>
* <span data-ttu-id="eaaec-185">För att köra Hive-frågor med SSH på domänanslutna HDInsight-kluster, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="eaaec-185">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
