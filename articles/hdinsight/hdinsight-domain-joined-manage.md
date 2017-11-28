---
title: "Hantera domänanslutna HDInsight - kluster i Azure | Microsoft Docs"
description: "Lär dig att hantera domänanslutna HDInsight-kluster"
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
ms.openlocfilehash: 9b56ce6cc5bdd3b8d48d047751e978cad08598e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a><span data-ttu-id="1f3db-103">Hantera domänanslutna HDInsight-kluster (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="1f3db-103">Manage Domain-joined HDInsight clusters (Preview)</span></span>
<span data-ttu-id="1f3db-104">Läs om användarna och roller för domänanslutna HDInsight och hur du hanterar domänanslutna HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1f3db-104">Learn the users and the roles in Domain-joined HDInsight, and how to manage domain-joined HDInsight clusters.</span></span>

## <a name="users-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="1f3db-105">Användare av domänanslutna HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="1f3db-105">Users of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="1f3db-106">Ett HDInsight-kluster som inte är ansluten till domänen har två konton som skapas när klustret skapas:</span><span class="sxs-lookup"><span data-stu-id="1f3db-106">An HDInsight cluster that is not domain-joined has two user accounts that are created during the cluster creation:</span></span>

* <span data-ttu-id="1f3db-107">**Ambari admin**: det här kontot kallas även *Hadoop användare* eller *HTTP användaren*.</span><span class="sxs-lookup"><span data-stu-id="1f3db-107">**Ambari admin**: This account is also known as *Hadoop user* or *HTTP user*.</span></span> <span data-ttu-id="1f3db-108">Det här kontot kan användas för att logga in på Ambari när https://&lt;klusternamn >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="1f3db-108">This account can be used to log on to Ambari at https://&lt;clustername>.azurehdinsight.net.</span></span> <span data-ttu-id="1f3db-109">Det kan också användas för att köra frågor på Ambari-vyer, köra jobb via externa verktyg (d.v.s. PowerShell, Templeton, Visual Studio) och autentisera med ODBC-drivrutinen och BI-verktyg (d.v.s. Excel, PowerBI eller Tableau).</span><span class="sxs-lookup"><span data-stu-id="1f3db-109">It can also be used to run queries on Ambari views, execute jobs via external tools (i.e. PowerShell, Templeton, Visual Studio), and authenticate with the Hive ODBC driver and BI tools (i.e. Excel, PowerBI, or Tableau).</span></span>
* <span data-ttu-id="1f3db-110">**SSH-användare**: det här kontot kan användas med SSH och köra sudo-kommandon.</span><span class="sxs-lookup"><span data-stu-id="1f3db-110">**SSH user**:  This account can be used with SSH, and execute sudo commands.</span></span> <span data-ttu-id="1f3db-111">Det har rot behörigheter till den virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="1f3db-111">It has root privileges to the Linux VMs.</span></span>

<span data-ttu-id="1f3db-112">En domänansluten HDInsight-kluster har tre nya användare utöver Ambari Admin och SSH-användare.</span><span class="sxs-lookup"><span data-stu-id="1f3db-112">A domain-joined HDInsight cluster has three new users in addition to Ambari Admin and SSH user.</span></span>

* <span data-ttu-id="1f3db-113">**Ranger admin**: det här kontot är lokalt administratörskonto för Apache Ranger.</span><span class="sxs-lookup"><span data-stu-id="1f3db-113">**Ranger admin**:  This account is the local Apache Ranger admin account.</span></span> <span data-ttu-id="1f3db-114">Det är inte en användare för active directory-domän.</span><span class="sxs-lookup"><span data-stu-id="1f3db-114">It is not an active directory domain user.</span></span> <span data-ttu-id="1f3db-115">Det här kontot kan användas för att konfigurera principer och göra andra användare, Administratörer eller delegerade administratörer (så att användarna kan hantera principer).</span><span class="sxs-lookup"><span data-stu-id="1f3db-115">This account can be used to setup policies and make other users admins or delegated admins (so that those users can manage policies).</span></span> <span data-ttu-id="1f3db-116">Som standard är användarnamnet *admin* och lösenordet är detsamma som Ambari administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="1f3db-116">By default, the username is *admin* and the password is the same as the Ambari admin password.</span></span> <span data-ttu-id="1f3db-117">Lösenordet kan uppdateras från sidan Inställningar i Ranger.</span><span class="sxs-lookup"><span data-stu-id="1f3db-117">The password can be updated from the Settings page in Ranger.</span></span>
* <span data-ttu-id="1f3db-118">**Klustret admin domänanvändare**: det här kontot är en active directory-domänanvändare utses Hadoop-kluster administratören inklusive Ambari och Ranger.</span><span class="sxs-lookup"><span data-stu-id="1f3db-118">**Cluster admin domain user**: This account is an active directory domain user designated as the Hadoop cluster admin including Ambari and Ranger.</span></span> <span data-ttu-id="1f3db-119">Du måste ange den här användarens autentiseringsuppgifter när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="1f3db-119">You must provide this user’s credentials during cluster creation.</span></span> <span data-ttu-id="1f3db-120">Den här användaren har följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="1f3db-120">This user has the following privileges:</span></span>

  * <span data-ttu-id="1f3db-121">Ansluta datorer till domänen och placera dem i Organisationsenheten som du anger när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="1f3db-121">Join machines to the domain and place them within the OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="1f3db-122">Skapa tjänstens huvudnamn i Organisationsenheten som du anger när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="1f3db-122">Create service principals within the OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="1f3db-123">Skapa omvänd DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="1f3db-123">Create reverse DNS entries.</span></span>

    <span data-ttu-id="1f3db-124">Observera de AD-användarna ha dessa privilegier.</span><span class="sxs-lookup"><span data-stu-id="1f3db-124">Note the other AD users also have these privileges.</span></span>

    <span data-ttu-id="1f3db-125">Det finns några slutpunkter i klustret (till exempel Templeton) som inte hanteras av Ranger och därför inte är säker.</span><span class="sxs-lookup"><span data-stu-id="1f3db-125">There are some end points within the cluster (for example, Templeton) which are not managed by Ranger, and hence are not secure.</span></span> <span data-ttu-id="1f3db-126">De här slutpunkterna är låsta för alla användare utom klustret domän administratörsanvändare.</span><span class="sxs-lookup"><span data-stu-id="1f3db-126">These end points are locked down for all users except the cluster admin domain user.</span></span>
* <span data-ttu-id="1f3db-127">**Vanliga**: du kan ange flera active directory-grupper när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="1f3db-127">**Regular**: During cluster creation, you can provide multiple active directory groups.</span></span> <span data-ttu-id="1f3db-128">Användare i dessa grupper kommer att synkroniseras till Ranger och Ambari.</span><span class="sxs-lookup"><span data-stu-id="1f3db-128">The users in these groups will be synced to Ranger and Ambari.</span></span> <span data-ttu-id="1f3db-129">Dessa användare har åtkomst till endast hanterade Ranger slutpunkter (till exempel Hiveserver2) är domänanvändare.</span><span class="sxs-lookup"><span data-stu-id="1f3db-129">These users are domain users and will have access to only Ranger-managed endpoints (for example, Hiveserver2).</span></span> <span data-ttu-id="1f3db-130">Alla RBAC principer och granskning kommer att användas på dessa användare.</span><span class="sxs-lookup"><span data-stu-id="1f3db-130">All the RBAC policies and auditing will be applicable to these users.</span></span>

## <a name="roles-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="1f3db-131">Roller för domänanslutna HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="1f3db-131">Roles of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="1f3db-132">Domänanslutna HDInsight har följande roller:</span><span class="sxs-lookup"><span data-stu-id="1f3db-132">Domain-joined HDInsight have the following roles:</span></span>

* <span data-ttu-id="1f3db-133">Klusteradministratören</span><span class="sxs-lookup"><span data-stu-id="1f3db-133">Cluster Administrator</span></span>
* <span data-ttu-id="1f3db-134">Kluster-operatorn</span><span class="sxs-lookup"><span data-stu-id="1f3db-134">Cluster Operator</span></span>
* <span data-ttu-id="1f3db-135">Tjänstadministratör</span><span class="sxs-lookup"><span data-stu-id="1f3db-135">Service Administrator</span></span>
* <span data-ttu-id="1f3db-136">Tjänsten Operator</span><span class="sxs-lookup"><span data-stu-id="1f3db-136">Service Operator</span></span>
* <span data-ttu-id="1f3db-137">Kluster-användare</span><span class="sxs-lookup"><span data-stu-id="1f3db-137">Cluster User</span></span>

<span data-ttu-id="1f3db-138">**Se behörigheter för dessa roller**</span><span class="sxs-lookup"><span data-stu-id="1f3db-138">**To see the permissions of these roles**</span></span>

1. <span data-ttu-id="1f3db-139">Öppna hanteringsgränssnittet för Ambari.</span><span class="sxs-lookup"><span data-stu-id="1f3db-139">Open the Ambari Management UI.</span></span>  <span data-ttu-id="1f3db-140">Se [öppna Ambari hanteringsgränssnittet](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="1f3db-140">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="1f3db-141">I den vänstra menyn klickar du på **roller**.</span><span class="sxs-lookup"><span data-stu-id="1f3db-141">From the left menu, click **Roles**.</span></span>
3. <span data-ttu-id="1f3db-142">Klicka på det blå frågetecknet visas behörigheterna:</span><span class="sxs-lookup"><span data-stu-id="1f3db-142">Click the blue question mark to see the permissions:</span></span>

    ![Behörigheter för domänanslutna HDInsight-roller](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a><span data-ttu-id="1f3db-144">Öppna hanteringsgränssnittet för Ambari</span><span class="sxs-lookup"><span data-stu-id="1f3db-144">Open the Ambari Management UI</span></span>
1. <span data-ttu-id="1f3db-145">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1f3db-145">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="1f3db-146">Öppna ditt HDInsight-kluster i ett blad.</span><span class="sxs-lookup"><span data-stu-id="1f3db-146">Open your HDInsight cluster in a blade.</span></span> <span data-ttu-id="1f3db-147">Se [listan och visa](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="1f3db-147">See [List and show clusters](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span></span>
3. <span data-ttu-id="1f3db-148">Klicka på **instrumentpanelen** på den översta menyn för att öppna Ambari.</span><span class="sxs-lookup"><span data-stu-id="1f3db-148">Click **Dashboard** from the top menu to open Ambari.</span></span>
4. <span data-ttu-id="1f3db-149">Logga in på Ambari med klustrets administratör domänanvändarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="1f3db-149">Log on to Ambari using the cluster administrator domain user name and password.</span></span>
5. <span data-ttu-id="1f3db-150">Klicka på den **Admin** listrutan från längst upp till höger hörnet och klicka sedan på **hantera Ambari**.</span><span class="sxs-lookup"><span data-stu-id="1f3db-150">Click the **Admin** dropdown menu from the upper right corner, and then click **Manage Ambari**.</span></span>

    ![Domänanslutna HDInsight hantera Ambari](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    <span data-ttu-id="1f3db-152">Det ser ut så Användargränssnittet:</span><span class="sxs-lookup"><span data-stu-id="1f3db-152">The UI looks like:</span></span>

    ![Domänanslutna HDInsight Ambari hanteringsgränssnittet](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a><span data-ttu-id="1f3db-154">Visa en lista med domänanvändare som synkroniseras från din Active Directory</span><span class="sxs-lookup"><span data-stu-id="1f3db-154">List the domain users synchronized from your Active Directory</span></span>
1. <span data-ttu-id="1f3db-155">Öppna hanteringsgränssnittet för Ambari.</span><span class="sxs-lookup"><span data-stu-id="1f3db-155">Open the Ambari Management UI.</span></span>  <span data-ttu-id="1f3db-156">Se [öppna Ambari hanteringsgränssnittet](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="1f3db-156">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="1f3db-157">I den vänstra menyn klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="1f3db-157">From the left menu, click **Users**.</span></span> <span data-ttu-id="1f3db-158">Du bör se alla användare som synkroniseras från din Active Directory till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="1f3db-158">You shall see all the users synced from your Active Directory to the HDInsight cluster.</span></span>

    ![Domänanslutna HDInsight Ambari management UI listan användare](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a><span data-ttu-id="1f3db-160">Visa en lista över de domängrupperna som synkroniseras från din Active Directory</span><span class="sxs-lookup"><span data-stu-id="1f3db-160">List the domain groups synchronized from your Active Directory</span></span>
1. <span data-ttu-id="1f3db-161">Öppna hanteringsgränssnittet för Ambari.</span><span class="sxs-lookup"><span data-stu-id="1f3db-161">Open the Ambari Management UI.</span></span>  <span data-ttu-id="1f3db-162">Se [öppna Ambari hanteringsgränssnittet](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="1f3db-162">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="1f3db-163">I den vänstra menyn klickar du på **grupper**.</span><span class="sxs-lookup"><span data-stu-id="1f3db-163">From the left menu, click **Groups**.</span></span> <span data-ttu-id="1f3db-164">Du bör se alla grupper som synkroniseras från din Active Directory till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="1f3db-164">You shall see all the groups synced from your Active Directory to the HDInsight cluster.</span></span>

    ![Domänanslutna HDInsight Ambari UI lista hanteringsgrupper](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a><span data-ttu-id="1f3db-166">Konfigurera behörigheter för Hive-vyer</span><span class="sxs-lookup"><span data-stu-id="1f3db-166">Configure Hive Views permissions</span></span>
1. <span data-ttu-id="1f3db-167">Öppna hanteringsgränssnittet för Ambari.</span><span class="sxs-lookup"><span data-stu-id="1f3db-167">Open the Ambari Management UI.</span></span>  <span data-ttu-id="1f3db-168">Se [öppna Ambari hanteringsgränssnittet](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="1f3db-168">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="1f3db-169">I den vänstra menyn klickar du på **vyer**.</span><span class="sxs-lookup"><span data-stu-id="1f3db-169">From the left menu, click **Views**.</span></span>
3. <span data-ttu-id="1f3db-170">Klicka på **HIVE** att visa detaljerna.</span><span class="sxs-lookup"><span data-stu-id="1f3db-170">Click **HIVE** to show the details.</span></span>

    ![Domänanslutna HDInsight Ambari management UI Hive-vyer](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. <span data-ttu-id="1f3db-172">Klicka på den **Hive-vy** länk för att konfigurera Hive vyer.</span><span class="sxs-lookup"><span data-stu-id="1f3db-172">Click the **Hive View** link to configure Hive Views.</span></span>
5. <span data-ttu-id="1f3db-173">Rulla ned till den **behörigheter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1f3db-173">Scroll down to the **Permissions** section.</span></span>

    ![Konfigurera behörigheter för domänanslutna HDInsight Ambari management UI Hive-vyer](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. <span data-ttu-id="1f3db-175">Klicka på **Lägg till användare** eller **Lägg till grupp**, och sedan ange användare eller grupper som kan använda Hive vyer.</span><span class="sxs-lookup"><span data-stu-id="1f3db-175">Click **Add User** or **Add Group**, and then specify the users or groups that can use Hive Views.</span></span>

## <a name="configure-users-for-the-roles"></a><span data-ttu-id="1f3db-176">Konfigurera användare för roller</span><span class="sxs-lookup"><span data-stu-id="1f3db-176">Configure users for the roles</span></span>
 <span data-ttu-id="1f3db-177">Om du vill se en lista över roller och deras behörigheter finns [roller för domänanslutna HDInsight-kluster](#roles-of-domain---joined-hdinsight-clusters).</span><span class="sxs-lookup"><span data-stu-id="1f3db-177">To see a list of roles and their permissions, see [Roles of Domain-joined HDInsight clusters](#roles-of-domain---joined-hdinsight-clusters).</span></span>

1. <span data-ttu-id="1f3db-178">Öppna hanteringsgränssnittet för Ambari.</span><span class="sxs-lookup"><span data-stu-id="1f3db-178">Open the Ambari Management UI.</span></span>  <span data-ttu-id="1f3db-179">Se [öppna Ambari hanteringsgränssnittet](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="1f3db-179">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="1f3db-180">I den vänstra menyn klickar du på **roller**.</span><span class="sxs-lookup"><span data-stu-id="1f3db-180">From the left menu, click **Roles**.</span></span>
3. <span data-ttu-id="1f3db-181">Klicka på **Lägg till användare** eller **Lägg till grupp** att tilldela användare och grupper för olika roller.</span><span class="sxs-lookup"><span data-stu-id="1f3db-181">Click **Add User** or **Add Group** to assign users and groups to different roles.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f3db-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f3db-182">Next steps</span></span>
* <span data-ttu-id="1f3db-183">Om du vill konfigurera ett domänanslutet HDInsight-kluster kan du läsa i [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="1f3db-183">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="1f3db-184">Om du vill konfigurera Hive-principer och köra Hive-frågor kan du läsa i [Konfigurera Hive-principer för domänanslutna HDInsight-kluster](hdinsight-domain-joined-run-hive.md).</span><span class="sxs-lookup"><span data-stu-id="1f3db-184">For configuring Hive policies and run Hive queries, see [Configure Hive policies for Domain-joined HDInsight clusters](hdinsight-domain-joined-run-hive.md).</span></span>
* <span data-ttu-id="1f3db-185">För att köra Hive-frågor med SSH på domänanslutna HDInsight-kluster, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="1f3db-185">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
