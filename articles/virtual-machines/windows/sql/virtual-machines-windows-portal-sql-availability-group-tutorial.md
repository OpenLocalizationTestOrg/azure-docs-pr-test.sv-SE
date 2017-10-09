---
title: "aaaSQL Server-Tillgänglighetsgrupper - virtuella datorer i Azure - kursen | Microsoft Docs"
description: "Den här kursen visar hur toocreate en SQL Server alltid på tillgänglighetsgrupp på Azure Virtual Machines."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 08a00342-fee2-4afe-8824-0db1ed4b8fca
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: 65b4210b0f851828a32a02053b03e4b8d469ba4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-group-in-azure-vm-manually"></a><span data-ttu-id="2f0a1-103">Konfigurera alltid på Tillgänglighetsgruppen i Azure VM manuellt</span><span class="sxs-lookup"><span data-stu-id="2f0a1-103">Configure Always On Availability Group in Azure VM manually</span></span>

<span data-ttu-id="2f0a1-104">Den här kursen visar hur toocreate en SQL Server alltid på tillgänglighetsgrupp på Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-104">This tutorial shows how toocreate a SQL Server Always On Availability Group on Azure Virtual Machines.</span></span> <span data-ttu-id="2f0a1-105">hello fullständig självstudiekursen skapar en tillgänglighetsgrupp med en databasreplik på två SQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-105">hello complete tutorial creates an Availability Group with a database replica on two SQL Servers.</span></span>

<span data-ttu-id="2f0a1-106">**Tid uppskattning**: tar cirka 30 minuter toocomplete när hello krav är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-106">**Time estimate**: Takes about 30 minutes toocomplete once hello prerequisites are met.</span></span>

<span data-ttu-id="2f0a1-107">hello diagram visar vad du bygga hello kursen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-107">hello diagram illustrates what you build in hello tutorial.</span></span>

![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a><span data-ttu-id="2f0a1-109">Krav</span><span class="sxs-lookup"><span data-stu-id="2f0a1-109">Prerequisites</span></span>

<span data-ttu-id="2f0a1-110">hello kursen förutsätter att du har en grundläggande förståelse för SQL Server alltid på Tillgänglighetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-110">hello tutorial assumes you have a basic understanding of SQL Server Always On Availability Groups.</span></span> <span data-ttu-id="2f0a1-111">Om du behöver mer information, se [översikt över alltid på Tillgänglighetsgrupper (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-111">If you need more information, see [Overview of Always On Availability Groups (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).</span></span>

<span data-ttu-id="2f0a1-112">hello visas följande tabell hello krav att du behöver toocomplete innan du påbörjar den här kursen:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-112">hello following table lists hello prerequisites that you need toocomplete before starting this tutorial:</span></span>

|  |<span data-ttu-id="2f0a1-113">Krav</span><span class="sxs-lookup"><span data-stu-id="2f0a1-113">Requirement</span></span> |<span data-ttu-id="2f0a1-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-114">Description</span></span> |
|----- |----- |----- |
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | <span data-ttu-id="2f0a1-116">Två SQL-servrar</span><span class="sxs-lookup"><span data-stu-id="2f0a1-116">Two SQL Servers</span></span> | <span data-ttu-id="2f0a1-117">– I en Azure tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-117">- In an Azure availability set</span></span> <br/> <span data-ttu-id="2f0a1-118">– I en domän</span><span class="sxs-lookup"><span data-stu-id="2f0a1-118">- In a single domain</span></span> <br/> <span data-ttu-id="2f0a1-119">-Med redundanskluster installerat</span><span class="sxs-lookup"><span data-stu-id="2f0a1-119">- With Failover Clustering feature installed</span></span> |
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| <span data-ttu-id="2f0a1-121">Windows Server</span><span class="sxs-lookup"><span data-stu-id="2f0a1-121">Windows Server</span></span> | <span data-ttu-id="2f0a1-122">Filresurs för klustret vittne</span><span class="sxs-lookup"><span data-stu-id="2f0a1-122">File share for cluster witness</span></span> |  
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="2f0a1-124">SQL Server-tjänstkontot</span><span class="sxs-lookup"><span data-stu-id="2f0a1-124">SQL Server service account</span></span> | <span data-ttu-id="2f0a1-125">Domänkonto</span><span class="sxs-lookup"><span data-stu-id="2f0a1-125">Domain account</span></span> |
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="2f0a1-127">Tjänstkontot för SQL Server Agent</span><span class="sxs-lookup"><span data-stu-id="2f0a1-127">SQL Server Agent service account</span></span> | <span data-ttu-id="2f0a1-128">Domänkonto</span><span class="sxs-lookup"><span data-stu-id="2f0a1-128">Domain account</span></span> |  
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="2f0a1-130">Öppna portar i brandväggen</span><span class="sxs-lookup"><span data-stu-id="2f0a1-130">Firewall ports open</span></span> | <span data-ttu-id="2f0a1-131">-SQL Server: **1433** för standardinstans</span><span class="sxs-lookup"><span data-stu-id="2f0a1-131">- SQL Server: **1433** for default instance</span></span> <br/> <span data-ttu-id="2f0a1-132">-Slutpunkten för databasspegling: **5022** eller alla tillgängliga portar</span><span class="sxs-lookup"><span data-stu-id="2f0a1-132">- Database mirroring endpoint: **5022** or any available port</span></span> <br/> <span data-ttu-id="2f0a1-133">-Azure belastningsutjämningsavsökning: **59999** eller alla tillgängliga portar</span><span class="sxs-lookup"><span data-stu-id="2f0a1-133">- Azure load balancer probe: **59999** or any available port</span></span> |
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="2f0a1-135">Lägga till Redundansklusterfunktionen</span><span class="sxs-lookup"><span data-stu-id="2f0a1-135">Add Failover Clustering Feature</span></span> | <span data-ttu-id="2f0a1-136">Både SQL-servrar kräver den här funktionen</span><span class="sxs-lookup"><span data-stu-id="2f0a1-136">Both SQL Servers require this feature</span></span> |
|![Ruta](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="2f0a1-138">Domänkonto för installation</span><span class="sxs-lookup"><span data-stu-id="2f0a1-138">Installation domain account</span></span> | <span data-ttu-id="2f0a1-139">-Lokal administratör på varje SQL Server</span><span class="sxs-lookup"><span data-stu-id="2f0a1-139">- Local administrator on each SQL Server</span></span> <br/> <span data-ttu-id="2f0a1-140">-Medlem i SQL Server fasta serverrollen sysadmin för varje instans av SQL Server</span><span class="sxs-lookup"><span data-stu-id="2f0a1-140">- Member of SQL Server sysadmin fixed server role for each instance of SQL Server</span></span>  |


<span data-ttu-id="2f0a1-141">Innan du påbörjar hello självstudien måste för[slutföra förutsättningar för att skapa Always On-Tillgänglighetsgrupper i Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-141">Before you begin hello tutorial, you need too[Complete prerequisites for creating Always On Availability Groups in Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md).</span></span> <span data-ttu-id="2f0a1-142">Om dessa krav har redan slutförts, kan du hoppa för[Skapa kluster](#CreateCluster).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-142">If these prerequisites are completed already, you can jump too[Create Cluster](#CreateCluster).</span></span>


<span data-ttu-id="2f0a1-143"><!--**Procedure**: *This is hello first “step”. Make titles H2’s and short and clear – H2’s appear in hello right pane on hello web page and are important for navigation.*-->

<a name="CreateCluster"></a>
##Skapa hello-kluster</span><span class="sxs-lookup"><span data-stu-id="2f0a1-143"><!--**Procedure**: *This is hello first “step”. Make titles H2’s and short and clear – H2’s appear in hello right pane on hello web page and are important for navigation.*-->

<a name="CreateCluster"></a>
## Create hello cluster</span></span>

<span data-ttu-id="2f0a1-144">Efter hello förutsättningar har slutförts, är hello första steget toocreate ett redundanskluster för Windows-Server som innehåller två SQL-servrar och en vittnesserver som.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-144">After hello prerequisites are completed, hello first step is toocreate a Windows Server Failover Cluster that includes two SQL Severs and a witness server.</span></span>  

1. <span data-ttu-id="2f0a1-145">RDP-toohello första SQL-Server med ett domänkonto som har administratörsbehörighet på SQL Server- och hello vittnesserver.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-145">RDP toohello first SQL Server using a domain account that is an administrator on both SQL Servers and hello witness server.</span></span>

   >[!TIP]
   ><span data-ttu-id="2f0a1-146">Om du har följt hello [förutsättningsdokumentet](virtual-machines-windows-portal-sql-availability-group-prereq.md), du har skapat ett konto som heter **CORP\Install**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-146">If you followed hello [prerequisites document](virtual-machines-windows-portal-sql-availability-group-prereq.md), you created an account called **CORP\Install**.</span></span> <span data-ttu-id="2f0a1-147">Använd det här kontot.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-147">Use this account.</span></span>

2. <span data-ttu-id="2f0a1-148">I hello **Serverhanteraren** instrumentpanelen, väljer **verktyg**, och klicka sedan på **Klusterhanteraren**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-148">In hello **Server Manager** dashboard, select **Tools**, and then click **Failover Cluster Manager**.</span></span>
3. <span data-ttu-id="2f0a1-149">I hello vänstra fönstret högerklickar du på **Klusterhanteraren**, och klicka sedan på **skapa ett kluster**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-149">In hello left pane, right-click **Failover Cluster Manager**, and then click **Create a Cluster**.</span></span>
   <span data-ttu-id="2f0a1-150">![Skapa kluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)</span><span class="sxs-lookup"><span data-stu-id="2f0a1-150">![Create Cluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)</span></span>
4. <span data-ttu-id="2f0a1-151">I guiden Skapa kluster hello, skapa ett kluster med en nod genom att gå igenom hello sidor med hello inställningarna i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-151">In hello Create Cluster Wizard, create a one-node cluster by stepping through hello pages with hello settings in hello following table:</span></span>

   | <span data-ttu-id="2f0a1-152">Sidan</span><span class="sxs-lookup"><span data-stu-id="2f0a1-152">Page</span></span> | <span data-ttu-id="2f0a1-153">Inställningar</span><span class="sxs-lookup"><span data-stu-id="2f0a1-153">Settings</span></span> |
   | --- | --- |
   | <span data-ttu-id="2f0a1-154">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="2f0a1-154">Before You Begin</span></span> |<span data-ttu-id="2f0a1-155">Använd standardvärden</span><span class="sxs-lookup"><span data-stu-id="2f0a1-155">Use defaults</span></span> |
   | <span data-ttu-id="2f0a1-156">Välj servrar</span><span class="sxs-lookup"><span data-stu-id="2f0a1-156">Select Servers</span></span> |<span data-ttu-id="2f0a1-157">Typen hello första SQL Server-namnet i **ange servernamnet** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-157">Type hello first SQL Server name in **Enter server name** and click **Add**.</span></span> |
   | <span data-ttu-id="2f0a1-158">Verifieringsvarning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-158">Validation Warning</span></span> |<span data-ttu-id="2f0a1-159">Välj **testar Nej jag inte behöver support från Microsoft för det här klustret och därför inte vill toorun hello validering. När jag klickar på nästa fortsätta skapa hello klustret**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-159">Select **No. I do not require support from Microsoft for this cluster, and therefore do not want toorun hello validation tests. When I click Next, continue Creating hello cluster**.</span></span> |
   | <span data-ttu-id="2f0a1-160">Åtkomstpunkt för administration hello kluster</span><span class="sxs-lookup"><span data-stu-id="2f0a1-160">Access Point for Administering hello Cluster</span></span> |<span data-ttu-id="2f0a1-161">Skriv ett namn för klustret, till exempel **SQLAGCluster1** i **klusternamnet**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-161">Type a cluster name, for example **SQLAGCluster1** in **Cluster Name**.</span></span>|
   | <span data-ttu-id="2f0a1-162">Bekräftelse</span><span class="sxs-lookup"><span data-stu-id="2f0a1-162">Confirmation</span></span> |<span data-ttu-id="2f0a1-163">Använd standardvärden om du inte använder lagringsutrymmen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-163">Use defaults unless you are using Storage Spaces.</span></span> <span data-ttu-id="2f0a1-164">Se hello fotnoten efter denna tabell.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-164">See hello note following this table.</span></span> |

### <a name="set-hello-cluster-ip-address"></a><span data-ttu-id="2f0a1-165">Ange hello klustrets IP-adress</span><span class="sxs-lookup"><span data-stu-id="2f0a1-165">Set hello cluster IP address</span></span>

1. <span data-ttu-id="2f0a1-166">I **Klusterhanteraren**, rulla nedåt för**klustrets kärnresurser** och expandera hello klusterinformation.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-166">In **Failover Cluster Manager**, scroll down too**Cluster Core Resources** and expand hello cluster details.</span></span> <span data-ttu-id="2f0a1-167">Du bör se både hello **namn** och hello **IP-adress** resurser i hello **misslyckades** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-167">You should see both hello **Name** and hello **IP Address** resources in hello **Failed** state.</span></span> <span data-ttu-id="2f0a1-168">hello IP-adressresurs kunde inte försättas online eftersom hello-klustret tilldelas hello samma IP-adress som dator för hello själva, därför är det en dubblett-adress.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-168">hello IP address resource cannot be brought online because hello cluster is assigned hello same IP address as hello machine itself, therefore it is a duplicate address.</span></span>

2. <span data-ttu-id="2f0a1-169">Högerklicka på hello misslyckades **IP-adress** resursen och klickar sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-169">Right-click hello failed **IP Address** resource, and then click **Properties**.</span></span>

   ![Egenskaper för klustret](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. <span data-ttu-id="2f0a1-171">Välj **statisk IP-adress** och ange en tillgänglig adress från undernätet där hello SQL Server finns i textrutan för hello-adress.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-171">Select **Static IP Address** and specify an available address from subnet where hello SQL Server is in hello Address text box.</span></span> <span data-ttu-id="2f0a1-172">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-172">Then, click **OK**.</span></span>
4. <span data-ttu-id="2f0a1-173">I hello **klustrets kärnresurser** avsnittet, högerklickar du på klusternamnet och klickar på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-173">In hello **Cluster Core Resources** section, right-click cluster name and click **Bring Online**.</span></span> <span data-ttu-id="2f0a1-174">Vänta tills båda resurser är online.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-174">Then, wait until both resources are online.</span></span> <span data-ttu-id="2f0a1-175">När hello klusterresurs namn är online uppdaterar hello DC-servern med ett nytt AD-datorkonto.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-175">When hello cluster name resource comes online, it updates hello DC server with a new AD computer account.</span></span> <span data-ttu-id="2f0a1-176">Använd den här AD-kontot toorun hello Availability Group klustrade tjänsten senare.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-176">Use this AD account toorun hello Availability Group clustered service later.</span></span>

### <span data-ttu-id="2f0a1-177"><a name="addNode"></a>Lägg till hello andra SQL Server-toocluster</span><span class="sxs-lookup"><span data-stu-id="2f0a1-177"><a name="addNode"></a>Add hello other SQL Server toocluster</span></span>

<span data-ttu-id="2f0a1-178">Lägg till hello andra toohello SQL Server-kluster.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-178">Add hello other SQL Server toohello cluster.</span></span>

1. <span data-ttu-id="2f0a1-179">Högerklicka på hello klustret i trädet för hello webbläsare och på **Lägg till nod**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-179">In hello browser tree, right-click hello cluster and click **Add Node**.</span></span>

    ![Lägg till nod toohello kluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. <span data-ttu-id="2f0a1-181">I hello **guiden Lägg till nod**, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-181">In hello **Add Node Wizard**, click **Next**.</span></span> <span data-ttu-id="2f0a1-182">I hello **Välj servrar** lägger du till hello andra SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-182">In hello **Select Servers** page, add hello second SQL Server.</span></span> <span data-ttu-id="2f0a1-183">Typen hello-servernamnet i **ange servernamnet** och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-183">Type hello server name in **Enter server name** and then click **Add**.</span></span> <span data-ttu-id="2f0a1-184">När du är klar klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-184">When you are done, click **Next**.</span></span>

1. <span data-ttu-id="2f0a1-185">I hello **verifieringsvarning** klickar du på **nr** (i en form av produktionsscenario du bör utföra hello verifieringstester).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-185">In hello **Validation Warning** page, click **No** (in a production scenario you should perform hello validation tests).</span></span> <span data-ttu-id="2f0a1-186">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-186">Then, click **Next**.</span></span>

8. <span data-ttu-id="2f0a1-187">I hello **bekräftelse** om du använder lagringsutrymmen, rensa hello kryssrutan **lägga till alla tillgängliga lagringsenheter toohello klustret.**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-187">In hello **Confirmation** page if you are using Storage Spaces, clear hello checkbox labeled **Add all eligible storage toohello cluster.**</span></span>

   ![Lägg till nod bekräftelse](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   ><span data-ttu-id="2f0a1-189">Om du använder lagringsutrymmen och inte avmarkera **lägga till alla tillgängliga lagringsenheter toohello klustret**, Windows tar bort hello virtuella diskar under hello clustering processen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-189">If you are using Storage Spaces and do not uncheck **Add all eligible storage toohello cluster**, Windows detaches hello virtual disks during hello clustering process.</span></span> <span data-ttu-id="2f0a1-190">Därför kan de visas inte i Diskhantering eller Explorer tills hello lagringsutrymmen tas bort från hello kluster och anbringas på nytt med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-190">As a result, they do not appear in Disk Manager or Explorer until hello storage spaces are removed from hello cluster and reattached using PowerShell.</span></span> <span data-ttu-id="2f0a1-191">Lagringsutrymmen grupper flera diskar i toostorage pooler.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-191">Storage Spaces groups multiple disks in toostorage pools.</span></span> <span data-ttu-id="2f0a1-192">Mer information finns i [lagringsutrymmen](https://technet.microsoft.com/library/hh831739).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-192">For more information, see [Storage Spaces](https://technet.microsoft.com/library/hh831739).</span></span>

1. <span data-ttu-id="2f0a1-193">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-193">Click **Next**.</span></span>

1. <span data-ttu-id="2f0a1-194">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-194">Click **Finish**.</span></span>

   <span data-ttu-id="2f0a1-195">Hanteraren för redundanskluster visar att klustret har en ny nod och listor i hello **noder** behållare.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-195">Failover Cluster Manager shows that your cluster has a new node and lists it in hello **Nodes** container.</span></span>

10. <span data-ttu-id="2f0a1-196">Logga ut från hello fjärrskrivbords-sessionen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-196">Log out of hello remote desktop session.</span></span>

### <a name="add-a-cluster-quorum-file-share"></a><span data-ttu-id="2f0a1-197">Lägg till en filresurs för klustrets kvorum</span><span class="sxs-lookup"><span data-stu-id="2f0a1-197">Add a cluster quorum file share</span></span>

<span data-ttu-id="2f0a1-198">I det här exemplet använder hello Windows klustret en filresursen toocreate en klustrets kvorum.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-198">In this example hello Windows cluster uses a file share toocreate a cluster quorum.</span></span> <span data-ttu-id="2f0a1-199">Den här kursen använder en nod och filresursmajoritet kvorum.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-199">This tutorial uses a Node and File Share Majority quorum.</span></span> <span data-ttu-id="2f0a1-200">Mer information finns i [Om kvorumkonfigurationer i ett Failover-kluster](http://technet.microsoft.com/library/cc731739.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-200">For more information, see [Understanding Quorum Configurations in a Failover Cluster](http://technet.microsoft.com/library/cc731739.aspx).</span></span>

1. <span data-ttu-id="2f0a1-201">Ansluta toohello filresurs vittne medlem filserver med en fjärrskrivbordssession.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-201">Connect toohello file share witness member server with a remote desktop session.</span></span>

1. <span data-ttu-id="2f0a1-202">På **Serverhanteraren**, klickar du på **verktyg**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-202">On **Server Manager**, click **Tools**.</span></span> <span data-ttu-id="2f0a1-203">Öppna **Datorhantering**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-203">Open **Computer Management**.</span></span>

1. <span data-ttu-id="2f0a1-204">Klicka på **delade mappar**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-204">Click **Shared Folders**.</span></span>

1. <span data-ttu-id="2f0a1-205">Högerklicka på **resurser**, och klicka på **ny resurs...** .</span><span class="sxs-lookup"><span data-stu-id="2f0a1-205">Right-click **Shares**, and click **New Share...**.</span></span>

   ![Ny resurs](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   <span data-ttu-id="2f0a1-207">Använd **guiden Skapa en delad mapp** toocreate en resurs.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-207">Use **Create a Shared Folder Wizard** toocreate a share.</span></span>

1. <span data-ttu-id="2f0a1-208">På **mappsökväg**, klickar du på **Bläddra** och hitta eller skapa en sökväg för hello delad mapp.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-208">On **Folder Path**, click **Browse** and locate or create a path for hello shared folder.</span></span> <span data-ttu-id="2f0a1-209">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-209">Click **Next**.</span></span>

1. <span data-ttu-id="2f0a1-210">I **namn, beskrivning och inställningar** Kontrollera hello namn och sökväg.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-210">In **Name, Description, and Settings** verify hello share name and path.</span></span> <span data-ttu-id="2f0a1-211">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-211">Click **Next**.</span></span>

1. <span data-ttu-id="2f0a1-212">På **delade mappbehörigheter** ange **anpassa behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-212">On **Shared Folder Permissions** set **Customize permissions**.</span></span> <span data-ttu-id="2f0a1-213">Klicka på **anpassad...** .</span><span class="sxs-lookup"><span data-stu-id="2f0a1-213">Click **Custom...**.</span></span>

1. <span data-ttu-id="2f0a1-214">På **anpassa behörigheter**, klickar du på **Lägg till...** .</span><span class="sxs-lookup"><span data-stu-id="2f0a1-214">On **Customize Permissions**, click **Add...**.</span></span>

1. <span data-ttu-id="2f0a1-215">Kontrollera att hello konto som används för toocreate hello kluster har fullständig kontroll.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-215">Make sure that hello account used toocreate hello cluster has full control.</span></span>

   ![Ny resurs](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. <span data-ttu-id="2f0a1-217">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-217">Click **OK**.</span></span>

1. <span data-ttu-id="2f0a1-218">I **delade mappbehörigheter**, klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-218">In **Shared Folder Permissions**, click **Finish**.</span></span> <span data-ttu-id="2f0a1-219">Klicka på **Slutför** igen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-219">Click **Finish** again.</span></span>  

1. <span data-ttu-id="2f0a1-220">Logga ut från hello-server</span><span class="sxs-lookup"><span data-stu-id="2f0a1-220">Log out of hello server</span></span>

### <a name="configure-cluster-quorum"></a><span data-ttu-id="2f0a1-221">Konfigurera klustrets kvorumdisk</span><span class="sxs-lookup"><span data-stu-id="2f0a1-221">Configure cluster quorum</span></span>

<span data-ttu-id="2f0a1-222">Ange därefter hello klustrets kvorum.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-222">Next, set hello cluster quorum.</span></span>

1. <span data-ttu-id="2f0a1-223">Ansluta toohello första klusternoden med fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-223">Connect toohello first cluster node with remote desktop.</span></span>

1. <span data-ttu-id="2f0a1-224">I **Klusterhanteraren**, högerklicka på klustret hello, peka för**fler åtgärder**, och klicka på **konfigurera inställningarna för klusterkvorum...** .</span><span class="sxs-lookup"><span data-stu-id="2f0a1-224">In **Failover Cluster Manager**, right-click hello cluster, point too**More Actions**, and click **Configure Cluster Quorum Settings...**.</span></span>

   ![Ny resurs](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. <span data-ttu-id="2f0a1-226">I **guiden Konfigurera klusterkvorum**, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-226">In **Configure Cluster Quorum Wizard**, click **Next**.</span></span>

1. <span data-ttu-id="2f0a1-227">I **Välj alternativ för kvorumkonfiguration**, Välj **Välj kvorumvittne hello**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-227">In **Select Quorum Configuration Option**, choose **Select hello quorum witness**, and click **Next**.</span></span>

1. <span data-ttu-id="2f0a1-228">På **Välj Kvorumvittne**, klickar du på **konfigurera ett filresursvittne**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-228">On **Select Quorum Witness**, click **Configure a file share witness**.</span></span>

   >[!TIP]
   ><span data-ttu-id="2f0a1-229">Windows Server 2016 stöder ett vittne i molnet.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-229">Windows Server 2016 supports a cloud witness.</span></span> <span data-ttu-id="2f0a1-230">Om du väljer den här typen av vittne du behöver inte en fil dela vittne.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-230">If you choose this type of witness, you do not need a file share witness.</span></span> <span data-ttu-id="2f0a1-231">Mer information finns i [distribuera ett moln vittne för ett redundanskluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-231">For more information, see [Deploy a cloud witness for a Failover Cluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span></span> <span data-ttu-id="2f0a1-232">Den här kursen använder ett filresursvittne som stöds av tidigare operativsystem.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-232">This tutorial uses a file share witness, which is supported by previous operating systems.</span></span>

1. <span data-ttu-id="2f0a1-233">På **konfigurera filresursvittne**, ange hello sökväg för hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-233">On **Configure File Share Witness**, type hello path for hello share you created.</span></span> <span data-ttu-id="2f0a1-234">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-234">Click **Next**.</span></span>

1. <span data-ttu-id="2f0a1-235">Kontrollera inställningarna för hello på **bekräftelse**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-235">Verify hello settings on **Confirmation**.</span></span> <span data-ttu-id="2f0a1-236">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-236">Click **Next**.</span></span>

1. <span data-ttu-id="2f0a1-237">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-237">Click **Finish**.</span></span>

<span data-ttu-id="2f0a1-238">hello klustrets kärnresurser är konfigurerade med ett filresursvittne.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-238">hello cluster core resources are configured with a file share witness.</span></span>

## <a name="enable-availability-groups"></a><span data-ttu-id="2f0a1-239">Aktivera Tillgänglighetsgrupper</span><span class="sxs-lookup"><span data-stu-id="2f0a1-239">Enable Availability Groups</span></span>

<span data-ttu-id="2f0a1-240">Aktivera sedan hello **AlwaysOn Availability Groups** funktion.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-240">Next, enable hello **AlwaysOn Availability Groups** feature.</span></span> <span data-ttu-id="2f0a1-241">Utföra de här stegen på både SQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-241">Do these steps on both SQL Servers.</span></span>

1. <span data-ttu-id="2f0a1-242">Från hello **starta** skärmen, starta **SQL Server Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-242">From hello **Start** screen, launch **SQL Server Configuration Manager**.</span></span>
2. <span data-ttu-id="2f0a1-243">I trädet för hello webbläsaren klickar du på **SQL Server Services**, högerklicka på hello **SQL Server (MSSQLSERVER)** tjänsten och klickar på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-243">In hello browser tree, click **SQL Server Services**, then right-click hello **SQL Server (MSSQLSERVER)** service and click **Properties**.</span></span>
3. <span data-ttu-id="2f0a1-244">Klicka på hello **AlwaysOn hög tillgänglighet** fliken och markera sedan **aktivera AlwaysOn Availability Groups**enligt följande:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-244">Click hello **AlwaysOn High Availability** tab, then select **Enable AlwaysOn Availability Groups**, as follows:</span></span>

    ![Aktivera AlwaysOn-Tillgänglighetsgrupper](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. <span data-ttu-id="2f0a1-246">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-246">Click **Apply**.</span></span> <span data-ttu-id="2f0a1-247">Klicka på **OK** i hello popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-247">Click **OK** in hello pop-up dialog.</span></span>

5. <span data-ttu-id="2f0a1-248">Starta om hello SQL Server-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-248">Restart hello SQL Server service.</span></span>

<span data-ttu-id="2f0a1-249">Upprepa dessa steg på hello andra SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-249">Repeat these steps on hello other SQL Server.</span></span>

<!-----------------
## <a name="endpoint-firewall"></a>Open firewall for hello database mirroring endpoint

Each instance of SQL Server that participates in an Availability Group requires a database mirroring endpoint. This endpoint is a TCP port for hello instance of SQL Server that is used toosynchronize hello database replicas in hello Availability Groups on that instance.

On both SQL Servers, open hello firewall for hello TCP port for hello database mirroring endpoint.

1. On hello first SQL Server **Start** screen, launch **Windows Firewall with Advanced Security**.
2. In hello left pane, select **Inbound Rules**. On hello right pane, click **New Rule**.
3. For **Rule Type**, choose **Port**.
1. For hello port, specify TCP and choose an unused TCP port number. For example, type *5022* and click **Next**.

   >[!NOTE]
   >For this example, we're using TCP port 5022. You can use any available port.

5. In hello **Action** page, keep **Allow hello connection** selected and click **Next**.
6. In hello **Profile** page, accept hello default settings and click **Next**.
7. In hello **Name** page, specify a rule name, such as **Default Instance Mirroring Endpoint** in hello **Name** text box, then click **Finish**.

Repeat these steps on hello second SQL Server.
-------------------------->

## <a name="create-a-database-on-hello-first-sql-server"></a><span data-ttu-id="2f0a1-250">Skapa en databas på hello första SQL-Server</span><span class="sxs-lookup"><span data-stu-id="2f0a1-250">Create a database on hello first SQL Server</span></span>

1. <span data-ttu-id="2f0a1-251">Starta hello RDP-filen toohello första SQL-Server med en domän konto som är medlem i sysadmin fasta serverrollen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-251">Launch hello RDP file toohello first SQL Server with a domain account that is a member of sysadmin fixed server role.</span></span>
1. <span data-ttu-id="2f0a1-252">Öppna SQL Server Management Studio och Anslut toohello första SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-252">Open SQL Server Management Studio and connect toohello first SQL Server.</span></span>
7. <span data-ttu-id="2f0a1-253">I **Object Explorer**, högerklicka på **databaser** och på **ny databas**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-253">In **Object Explorer**, right-click **Databases** and click **New Database**.</span></span>
8. <span data-ttu-id="2f0a1-254">I **databasnamnet**, typen **MyDB1**, klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-254">In **Database name**, type **MyDB1**, then click **OK**.</span></span>

### <span data-ttu-id="2f0a1-255"><a name="backupshare"></a>Skapa en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="2f0a1-255"><a name="backupshare"></a> Create a backup share</span></span>

1. <span data-ttu-id="2f0a1-256">Hej första SQL-servern på **Serverhanteraren**, klickar du på **verktyg**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-256">On hello first SQL Server in **Server Manager**, click **Tools**.</span></span> <span data-ttu-id="2f0a1-257">Öppna **Datorhantering**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-257">Open **Computer Management**.</span></span>

1. <span data-ttu-id="2f0a1-258">Klicka på **delade mappar**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-258">Click **Shared Folders**.</span></span>

1. <span data-ttu-id="2f0a1-259">Högerklicka på **resurser**, och klicka på **ny resurs...** .</span><span class="sxs-lookup"><span data-stu-id="2f0a1-259">Right-click **Shares**, and click **New Share...**.</span></span>

   ![Ny resurs](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   <span data-ttu-id="2f0a1-261">Använd **guiden Skapa en delad mapp** toocreate en resurs.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-261">Use **Create a Shared Folder Wizard** toocreate a share.</span></span>

1. <span data-ttu-id="2f0a1-262">På **mappsökväg**, klickar du på **Bläddra** och hitta eller skapa en sökväg för hello delad mapp för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-262">On **Folder Path**, click **Browse** and locate or create a path for hello database backup shared folder.</span></span> <span data-ttu-id="2f0a1-263">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-263">Click **Next**.</span></span>

1. <span data-ttu-id="2f0a1-264">I **namn, beskrivning och inställningar** Kontrollera hello namn och sökväg.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-264">In **Name, Description, and Settings** verify hello share name and path.</span></span> <span data-ttu-id="2f0a1-265">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-265">Click **Next**.</span></span>

1. <span data-ttu-id="2f0a1-266">På **delade mappbehörigheter** ange **anpassa behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-266">On **Shared Folder Permissions** set **Customize permissions**.</span></span> <span data-ttu-id="2f0a1-267">Klicka på **anpassad...** .</span><span class="sxs-lookup"><span data-stu-id="2f0a1-267">Click **Custom...**.</span></span>

1. <span data-ttu-id="2f0a1-268">På **anpassa behörigheter**, klickar du på **Lägg till...** .</span><span class="sxs-lookup"><span data-stu-id="2f0a1-268">On **Customize Permissions**, click **Add...**.</span></span>

1. <span data-ttu-id="2f0a1-269">Se till att hello SQL Server och SQL Server Agent-tjänstkontona för båda servrarna har fullständig behörighet.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-269">Make sure that hello SQL Server and SQL Server Agent service accounts for both servers have full control.</span></span>

   ![Ny resurs](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. <span data-ttu-id="2f0a1-271">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-271">Click **OK**.</span></span>

1. <span data-ttu-id="2f0a1-272">I **delade mappbehörigheter**, klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-272">In **Shared Folder Permissions**, click **Finish**.</span></span> <span data-ttu-id="2f0a1-273">Klicka på **Slutför** igen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-273">Click **Finish** again.</span></span>  

### <a name="take-a-full-backup-of-hello-database"></a><span data-ttu-id="2f0a1-274">Ta en fullständig säkerhetskopiering av databasen hello</span><span class="sxs-lookup"><span data-stu-id="2f0a1-274">Take a full backup of hello database</span></span>

<span data-ttu-id="2f0a1-275">Du måste tooback in hello ny databas tooinitialize hello loggkedjan.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-275">You need tooback up hello new database tooinitialize hello log chain.</span></span> <span data-ttu-id="2f0a1-276">Om du inte gör en säkerhetskopia av den nya hello-databasen kan inte ingå i en tillgänglighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-276">If you do not take a backup of hello new database, it cannot be included in an Availability Group.</span></span>

1. <span data-ttu-id="2f0a1-277">I **Object Explorer**, högerklicka på hello-databasen, peka för**aktiviteter...** , klickar du på **säkerhetskopiera**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-277">In **Object Explorer**, right-click hello database, point too**Tasks...**, click **Back Up**.</span></span>

1. <span data-ttu-id="2f0a1-278">Klicka på **OK** tootake en fullständig säkerhetskopiering toohello standardplatsen för säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-278">Click **OK** tootake a full backup toohello default backup location.</span></span>

## <a name="create-hello-availability-group"></a><span data-ttu-id="2f0a1-279">Skapa hello Tillgänglighetsgruppen</span><span class="sxs-lookup"><span data-stu-id="2f0a1-279">Create hello Availability Group</span></span>
<span data-ttu-id="2f0a1-280">Du är nu redo tooconfigure en tillgänglighetsgrupp med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-280">You are now ready tooconfigure an Availability Group using hello following steps:</span></span>

* <span data-ttu-id="2f0a1-281">Skapa en databas på hello första SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-281">Create a database on hello first SQL Server.</span></span>
* <span data-ttu-id="2f0a1-282">Ta både en fullständig säkerhetskopia och en säkerhetskopia av transaktionsloggen för databasen hello</span><span class="sxs-lookup"><span data-stu-id="2f0a1-282">Take both a full backup and a transaction log backup of hello database</span></span>
* <span data-ttu-id="2f0a1-283">Återställ hello fullständig och logg säkerhetskopieringar toohello andra SQL Server med hello **NORECOVERY** alternativet</span><span class="sxs-lookup"><span data-stu-id="2f0a1-283">Restore hello full and log backups toohello second SQL Server with hello **NORECOVERY** option</span></span>
* <span data-ttu-id="2f0a1-284">Skapa hello Availability Group (**AG1**) med synkront genomförande och automatisk redundans läsbara sekundära repliker</span><span class="sxs-lookup"><span data-stu-id="2f0a1-284">Create hello Availability Group (**AG1**) with synchronous commit, automatic failover, and readable secondary replicas</span></span>

### <a name="create-hello-availability-group"></a><span data-ttu-id="2f0a1-285">Skapa hello Tillgänglighetsgruppen:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-285">Create hello Availability Group:</span></span>

1. <span data-ttu-id="2f0a1-286">På toohello fjärrskrivbords-sessionen första SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-286">On remote desktop session toohello first SQL Server.</span></span> <span data-ttu-id="2f0a1-287">I **Object Explorer** i SSMS, högerklickar du på **AlwaysOn hög tillgänglighet** och på **guiden Ny tillgänglighetsgrupp**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-287">In **Object Explorer** in SSMS, right-click **AlwaysOn High Availability** and click **New Availability Group Wizard**.</span></span>

    ![Starta guiden Ny tillgänglighetsgrupp](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. <span data-ttu-id="2f0a1-289">I hello **introduktion** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-289">In hello **Introduction** page, click **Next**.</span></span> <span data-ttu-id="2f0a1-290">I hello **ange tillgänglighetsgruppens namn** , ange ett namn för hello Availability Group, till exempel **AG1**i **tillgänglighetsgruppens namn**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-290">In hello **Specify Availability Group Name** page, type a name for hello Availability Group, for example **AG1**, in **Availability group name**.</span></span> <span data-ttu-id="2f0a1-291">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-291">Click **Next**.</span></span>

    ![Guiden Ny AG, ange AG-namn](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. <span data-ttu-id="2f0a1-293">I hello **Välj databaser** väljer du databasen och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-293">In hello **Select Databases** page, select your database and click **Next**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="2f0a1-294">hello databasen uppfyller hello krav för en tillgänglighetsgrupp eftersom du har utfört en fullständig säkerhetskopiering på hello avsedda primära repliken.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-294">hello database meets hello prerequisites for an Availability Group because you have taken at least one full backup on hello intended primary replica.</span></span>

   ![Guiden Ny AG, Välj databaser](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. <span data-ttu-id="2f0a1-296">I hello **ange repliker** klickar du på **lägga till replik**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-296">In hello **Specify Replicas** page, click **Add Replica**.</span></span>

   ![Guiden Ny AG, ange repliker](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. <span data-ttu-id="2f0a1-298">Hej **ansluta tooServer** dialogrutan som öppnas.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-298">hello **Connect tooServer** dialog pops up.</span></span> <span data-ttu-id="2f0a1-299">Hello-typnamn för hello andra servern i **servernamn**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-299">Type hello name of hello second server in **Server name**.</span></span> <span data-ttu-id="2f0a1-300">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-300">Click **Connect**.</span></span>

   <span data-ttu-id="2f0a1-301">Tillbaka i hello **ange repliker** sidan du bör nu se hello andra server som anges i **Tillgänglighetsrepliker**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-301">Back in hello **Specify Replicas** page, you should now see hello second server listed in **Availability Replicas**.</span></span> <span data-ttu-id="2f0a1-302">Konfigurera hello repliker på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-302">Configure hello replicas as follows.</span></span>

   ![Guiden Ny AG, ange repliker (fullständig)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. <span data-ttu-id="2f0a1-304">Klicka på **slutpunkter** toosee hello slutpunkten för databasspegling för den här Tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-304">Click **Endpoints** toosee hello database mirroring endpoint for this Availability Group.</span></span> <span data-ttu-id="2f0a1-305">Använd hello samma port som du använde när du ställer in hello [brandväggsregel för slutpunkter för databasspegling](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-305">Use hello same port that you used when you set hello [firewall rule for database mirroring endpoints](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span></span>

    ![Guiden Ny AG, Välj inledande datasynkronisering](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. <span data-ttu-id="2f0a1-307">I hello **Välj inledande datasynkronisering** väljer **fullständig** och ange en plats i nätverket.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-307">In hello **Select Initial Data Synchronization** page, select **Full** and specify a shared network location.</span></span> <span data-ttu-id="2f0a1-308">För hello plats, använder du hello [säkerhetskopieringsresursen du skapade](#backupshare).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-308">For hello location, use hello [backup share that you created](#backupshare).</span></span> <span data-ttu-id="2f0a1-309">I hello exempel det var **\\\\\<första SQL Server\>\Backup\**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-309">In hello example it was, **\\\\\<First SQL Server\>\Backup\**.</span></span> <span data-ttu-id="2f0a1-310">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-310">Click **Next**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="2f0a1-311">Fullständig synkronisering tar en fullständig säkerhetskopiering av databasen hello på hello första instansen av SQL Server och återställer det andra toohello-instans.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-311">Full synchronization takes a full backup of hello database on hello first instance of SQL Server and restores it toohello second instance.</span></span> <span data-ttu-id="2f0a1-312">För stora databaser rekommenderas fullständig synkronisering inte eftersom det kan ta lång tid.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-312">For large databases, full synchronization is not recommended because it may take a long time.</span></span> <span data-ttu-id="2f0a1-313">Du kan minska nu genom att manuellt ta en säkerhetskopia av databasen hello och återställning med `NO RECOVERY`.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-313">You can reduce this time by manually taking a backup of hello database and restoring it with `NO RECOVERY`.</span></span> <span data-ttu-id="2f0a1-314">Om hello databasen är redan återställd med `NO RECOVERY` hello på andra SQL Server innan du konfigurerar hello Availability Group, Välj **Anslut endast**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-314">If hello database is already restored with `NO RECOVERY` on hello second SQL Server before configuring hello Availability Group, choose **Join only**.</span></span> <span data-ttu-id="2f0a1-315">Om du vill tootake hello säkerhetskopia när du har konfigurerat hello Availability Group väljer **hoppa över inledande datasynkronisering**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-315">If you want tootake hello backup after configuring hello Availability Group, choose **Skip initial data synchronization**.</span></span>

    ![Guiden Ny AG, Välj inledande datasynkronisering](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. <span data-ttu-id="2f0a1-317">I hello **validering** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-317">In hello **Validation** page, click **Next**.</span></span> <span data-ttu-id="2f0a1-318">Den här sidan bör se ut ungefär toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-318">This page should look similar toohello following image:</span></span>

    ![Guiden Ny AG, validering](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    ><span data-ttu-id="2f0a1-320">Det finns en varning för hello Lyssnarkonfigurationen eftersom du inte har konfigurerat en tillgänglighetsgruppens lyssnare.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-320">There is a warning for hello listener configuration because you have not configured an Availability Group listener.</span></span> <span data-ttu-id="2f0a1-321">Du kan ignorera den här varningen eftersom på Azure virtual machines hello lyssnaren skapa när du har skapat hello Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-321">You can ignore this warning because on Azure virtual machines you create hello listener after creating hello Azure load balancer.</span></span>

10. <span data-ttu-id="2f0a1-322">I hello **sammanfattning** klickar du på **Slutför**, och sedan vänta medan hello guiden konfigurerar hello nya Tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-322">In hello **Summary** page, click **Finish**, then wait while hello wizard configures hello new Availability Group.</span></span> <span data-ttu-id="2f0a1-323">I hello **förlopp** sidan som du kan klicka på **mer** tooview hello detaljerad pågår.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-323">In hello **Progress** page, you can click **More details** tooview hello detailed progress.</span></span> <span data-ttu-id="2f0a1-324">När hello-guiden har slutförts kan du granska hello **resultat** sidan tooverify som hello tillgänglighetsgrupp har skapats.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-324">Once hello wizard is finished, inspect hello **Results** page tooverify that hello Availability Group is successfully created.</span></span>

     ![Guiden för ny AG resultat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. <span data-ttu-id="2f0a1-326">Klicka på **Stäng** tooexit hello guiden.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-326">Click **Close** tooexit hello wizard.</span></span>

### <a name="check-hello-availability-group"></a><span data-ttu-id="2f0a1-327">Kontrollera hello Tillgänglighetsgruppen</span><span class="sxs-lookup"><span data-stu-id="2f0a1-327">Check hello Availability Group</span></span>

1. <span data-ttu-id="2f0a1-328">I **Object Explorer**, expandera **AlwaysOn hög tillgänglighet**, expandera **Tillgänglighetsgrupper**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-328">In **Object Explorer**, expand **AlwaysOn High Availability**, then expand **Availability Groups**.</span></span> <span data-ttu-id="2f0a1-329">Du bör nu se hello nya Tillgänglighetsgruppen i den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-329">You should now see hello new Availability Group in this container.</span></span> <span data-ttu-id="2f0a1-330">Högerklicka på hello Tillgänglighetsgruppen och på **visa instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-330">Right-click hello Availability Group and click **Show Dashboard**.</span></span>

   ![Visa AG-instrumentpanelen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   <span data-ttu-id="2f0a1-332">Din **AlwaysOn instrumentpanelen** bör se ut ungefär toothis.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-332">Your **AlwaysOn Dashboard** should look similar toothis.</span></span>

   ![AG-instrumentpanelen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   <span data-ttu-id="2f0a1-334">Du kan se hello repliker, hello redundansläge av varje replik och hello synkroniseringstillståndet.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-334">You can see hello replicas, hello failover mode of each replica and hello synchronization state.</span></span>

2. <span data-ttu-id="2f0a1-335">I **Klusterhanteraren**, klicka på ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-335">In **Failover Cluster Manager**, click your cluster.</span></span> <span data-ttu-id="2f0a1-336">Välj **roller**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-336">Select **Roles**.</span></span> <span data-ttu-id="2f0a1-337">hello tillgänglighetsgruppens namn som du använde är en roll på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-337">hello Availability Group name you used is a role on hello cluster.</span></span> <span data-ttu-id="2f0a1-338">Att Tillgänglighetsgruppen inte har en IP-adress för klientanslutningar, eftersom du inte konfigurerar en lyssnare.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-338">That Availability Group does not have an IP address for client connections, because you did not configure a listener.</span></span> <span data-ttu-id="2f0a1-339">Du konfigurerar hello lyssnare när du har skapat en Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-339">You will configure hello listener after you create an Azure load balancer.</span></span>

   ![AG i hanteraren för redundanskluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > <span data-ttu-id="2f0a1-341">Försök inte toofail över hello Availability Group från hello hanteraren för redundanskluster.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-341">Do not try toofail over hello Availability Group from hello Failover Cluster Manager.</span></span> <span data-ttu-id="2f0a1-342">Alla redundansåtgärder ska utföras inifrån **AlwaysOn instrumentpanelen** i SSMS.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-342">All failover operations should be performed from within **AlwaysOn Dashboard** in SSMS.</span></span> <span data-ttu-id="2f0a1-343">Mer information finns i [begränsningar med hjälp av hello Klusterhanterare med Tillgänglighetsgrupper](https://msdn.microsoft.com/library/ff929171.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-343">For more information, see [Restrictions on Using hello Failover Cluster Manager with Availability Groups](https://msdn.microsoft.com/library/ff929171.aspx).</span></span>
    >

<span data-ttu-id="2f0a1-344">Du har nu en tillgänglighetsgrupp med repliker på två instanser av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-344">At this point, you have an Availability Group with replicas on two instances of SQL Server.</span></span> <span data-ttu-id="2f0a1-345">Du kan flytta hello Availability Group mellan instanser.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-345">You can move hello Availability Group between instances.</span></span> <span data-ttu-id="2f0a1-346">Du kan inte ansluta toohello Availability Group ännu eftersom du inte har en lyssnare.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-346">You cannot connect toohello Availability Group yet because you do not have a listener.</span></span> <span data-ttu-id="2f0a1-347">I Azure-datorer kräver hello lyssnare en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-347">In Azure virtual machines, hello listener requires a load balancer.</span></span> <span data-ttu-id="2f0a1-348">hello nästa steg är toocreate hello belastningsutjämnare i Azure.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-348">hello next step is toocreate hello load balancer in Azure.</span></span>

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a><span data-ttu-id="2f0a1-349">Skapa en Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="2f0a1-349">Create an Azure load balancer</span></span>

<span data-ttu-id="2f0a1-350">På Azure virtual machines kräver en belastningsutjämnare i en SQL Server-tillgänglighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-350">On Azure virtual machines, a SQL Server Availability Group requires a load balancer.</span></span> <span data-ttu-id="2f0a1-351">hello belastningsutjämnaren innehåller hello IP-adress för hello tillgänglighetsgruppens lyssnare.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-351">hello load balancer holds hello IP address for hello Availability Group listener.</span></span> <span data-ttu-id="2f0a1-352">Det här avsnittet beskrivs hur toocreate hello belastningsutjämnare i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-352">This section summarizes how toocreate hello load balancer in hello Azure portal.</span></span>

1. <span data-ttu-id="2f0a1-353">I hello Azure-portalen, går toohello resursgruppen där din SQL-servrar är och på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-353">In hello Azure portal, go toohello resource group where your SQL Servers are and click **+ Add**.</span></span>
2. <span data-ttu-id="2f0a1-354">Sök efter **belastningsutjämnare**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-354">Search for **Load Balancer**.</span></span> <span data-ttu-id="2f0a1-355">Välj hello belastningsutjämnare som publicerats av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-355">Choose hello load balancer published by Microsoft.</span></span>

   ![AG i hanteraren för redundanskluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1.  <span data-ttu-id="2f0a1-357">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-357">Click **Create**.</span></span>
3. <span data-ttu-id="2f0a1-358">Konfigurera hello följande parametrar för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-358">Configure hello following parameters for hello load balancer.</span></span>

   | <span data-ttu-id="2f0a1-359">Inställning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-359">Setting</span></span> | <span data-ttu-id="2f0a1-360">Fält</span><span class="sxs-lookup"><span data-stu-id="2f0a1-360">Field</span></span> |
   | --- | --- |
   | <span data-ttu-id="2f0a1-361">**Namn**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-361">**Name**</span></span> |<span data-ttu-id="2f0a1-362">Använda ett namn för hello belastningsutjämnare, till exempel **sqlLB**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-362">Use a text name for hello load balancer, for example **sqlLB**.</span></span> |
   | <span data-ttu-id="2f0a1-363">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-363">**Type**</span></span> |<span data-ttu-id="2f0a1-364">internt</span><span class="sxs-lookup"><span data-stu-id="2f0a1-364">Internal</span></span> |
   | <span data-ttu-id="2f0a1-365">**Virtuellt nätverk**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-365">**Virtual network**</span></span> |<span data-ttu-id="2f0a1-366">Använda hello namnet på hello virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-366">Use hello name of hello Azure virtual network.</span></span> |
   | <span data-ttu-id="2f0a1-367">**Undernät**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-367">**Subnet**</span></span> |<span data-ttu-id="2f0a1-368">Använd hello namnet på hello undernät som hello virtuella datorn är på.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-368">Use hello name of hello subnet that hello virtual machine is in.</span></span>  |
   | <span data-ttu-id="2f0a1-369">**IP-adresstilldelning**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-369">**IP address assignment**</span></span> |<span data-ttu-id="2f0a1-370">Statisk</span><span class="sxs-lookup"><span data-stu-id="2f0a1-370">Static</span></span> |
   | <span data-ttu-id="2f0a1-371">**IP-adress**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-371">**IP address**</span></span> |<span data-ttu-id="2f0a1-372">Använd en tillgänglig adress från undernätet.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-372">Use an available address from subnet.</span></span> |
   | <span data-ttu-id="2f0a1-373">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-373">**Subscription**</span></span> |<span data-ttu-id="2f0a1-374">Använd hello samma prenumeration som hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-374">Use hello same subscription as hello virtual machine.</span></span> |
   | <span data-ttu-id="2f0a1-375">**Plats**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-375">**Location**</span></span> |<span data-ttu-id="2f0a1-376">Använd hello samma plats som hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-376">Use hello same location as hello virtual machine.</span></span> |

   <span data-ttu-id="2f0a1-377">hello Azure portal, blad bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-377">hello Azure portal blade should look like this:</span></span>

   ![Skapa belastningsutjämnare](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. <span data-ttu-id="2f0a1-379">Klicka på **skapa**, toocreate hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-379">Click **Create**, toocreate hello load balancer.</span></span>

<span data-ttu-id="2f0a1-380">tooconfigure hello belastningsutjämnare, måste toocreate en serverdelspool en avsökning och ange hello belastningsutjämningsregler.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-380">tooconfigure hello load balancer, you need toocreate a backend pool, a probe, and set hello load balancing rules.</span></span> <span data-ttu-id="2f0a1-381">Gör du följande i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-381">Do these in hello Azure portal.</span></span>

### <a name="add-backend-pool"></a><span data-ttu-id="2f0a1-382">Lägg till serverdelspoolen</span><span class="sxs-lookup"><span data-stu-id="2f0a1-382">Add backend pool</span></span>

1. <span data-ttu-id="2f0a1-383">Gå tooyour tillgänglighetsgruppen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-383">In hello Azure portal, go tooyour availability group.</span></span> <span data-ttu-id="2f0a1-384">Du kan behöva toorefresh hello visa toosee hello nyskapad belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-384">You might need toorefresh hello view toosee hello newly created load balancer.</span></span>

   ![Hitta belastningsutjämnare i resursgruppen.](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. <span data-ttu-id="2f0a1-386">Klicka på hello belastningsutjämnare, **serverdelspooler**, och klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-386">Click hello load balancer, click **Backend pools**, and click **+Add**.</span></span> <span data-ttu-id="2f0a1-387">Ange hello serverdelspool på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-387">Set hello backend pool as follows:</span></span>

   | <span data-ttu-id="2f0a1-388">Inställning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-388">Setting</span></span> | <span data-ttu-id="2f0a1-389">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-389">Description</span></span> | <span data-ttu-id="2f0a1-390">Exempel</span><span class="sxs-lookup"><span data-stu-id="2f0a1-390">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="2f0a1-391">**Namn**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-391">**Name**</span></span> | <span data-ttu-id="2f0a1-392">Skriv ett namn</span><span class="sxs-lookup"><span data-stu-id="2f0a1-392">Type a text name</span></span> | <span data-ttu-id="2f0a1-393">SQLLBBE</span><span class="sxs-lookup"><span data-stu-id="2f0a1-393">SQLLBBE</span></span>
   | <span data-ttu-id="2f0a1-394">**Tillhör**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-394">**Associated to**</span></span> | <span data-ttu-id="2f0a1-395">Välj från listan</span><span class="sxs-lookup"><span data-stu-id="2f0a1-395">Pick from list</span></span> | <span data-ttu-id="2f0a1-396">Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-396">Availability set</span></span>
   | <span data-ttu-id="2f0a1-397">**Tillgänglighetsuppsättning**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-397">**Availability set**</span></span> | <span data-ttu-id="2f0a1-398">Använd ett namn på hello tillgänglighetsuppsättningen som din SQL Server-datorer finns i</span><span class="sxs-lookup"><span data-stu-id="2f0a1-398">Use a name of hello availability set that your SQL Server VMs are in</span></span> | <span data-ttu-id="2f0a1-399">sqlAvailabilitySet</span><span class="sxs-lookup"><span data-stu-id="2f0a1-399">sqlAvailabilitySet</span></span> |
   | <span data-ttu-id="2f0a1-400">**Virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-400">**Virtual machines**</span></span> |<span data-ttu-id="2f0a1-401">Hej två Azure SQL Server-VM-namn</span><span class="sxs-lookup"><span data-stu-id="2f0a1-401">hello two Azure SQL Server VM names</span></span> | <span data-ttu-id="2f0a1-402">SQLServer-0, sqlserver-1</span><span class="sxs-lookup"><span data-stu-id="2f0a1-402">sqlserver-0, sqlserver-1</span></span>

1. <span data-ttu-id="2f0a1-403">Hello typnamn för hello serverdelspool.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-403">Type hello name for hello back end pool.</span></span>

1. <span data-ttu-id="2f0a1-404">Klicka på **+ Lägg till en virtuell dator**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-404">Click **+ Add a virtual machine**.</span></span>

1. <span data-ttu-id="2f0a1-405">För hello tillgänglighetsuppsättning Välj hello tillgänglighet som hello SQL-servrar finns i.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-405">For hello availability set, choose hello availability set that hello SQL Servers are in.</span></span>

1. <span data-ttu-id="2f0a1-406">För virtuella datorer, ta med både hello SQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-406">For virtual machines, include both of hello SQL Servers.</span></span> <span data-ttu-id="2f0a1-407">Inkludera inte hello filserver filresurs vittne.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-407">Do not include hello file share witness server.</span></span>

1. <span data-ttu-id="2f0a1-408">Klicka på **OK** toocreate hello serverdelspool.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-408">Click **OK** toocreate hello backend pool.</span></span>

### <a name="set-hello-probe"></a><span data-ttu-id="2f0a1-409">Ange hello avsökning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-409">Set hello probe</span></span>

1. <span data-ttu-id="2f0a1-410">Klicka på hello belastningsutjämnare, **hälsoavsökning**, och klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-410">Click hello load balancer, click **Health probes**, and click **+Add**.</span></span>

1. <span data-ttu-id="2f0a1-411">Ange hello hälsoavsökningen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-411">Set hello health probe as follows:</span></span>

   | <span data-ttu-id="2f0a1-412">Inställning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-412">Setting</span></span> | <span data-ttu-id="2f0a1-413">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-413">Description</span></span> | <span data-ttu-id="2f0a1-414">Exempel</span><span class="sxs-lookup"><span data-stu-id="2f0a1-414">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="2f0a1-415">**Namn**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-415">**Name**</span></span> | <span data-ttu-id="2f0a1-416">Text</span><span class="sxs-lookup"><span data-stu-id="2f0a1-416">Text</span></span> | <span data-ttu-id="2f0a1-417">SQLAlwaysOnEndPointProbe</span><span class="sxs-lookup"><span data-stu-id="2f0a1-417">SQLAlwaysOnEndPointProbe</span></span> |
   | <span data-ttu-id="2f0a1-418">**Protokoll**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-418">**Protocol**</span></span> | <span data-ttu-id="2f0a1-419">Välj TCP</span><span class="sxs-lookup"><span data-stu-id="2f0a1-419">Choose TCP</span></span> | <span data-ttu-id="2f0a1-420">TCP</span><span class="sxs-lookup"><span data-stu-id="2f0a1-420">TCP</span></span> |
   | <span data-ttu-id="2f0a1-421">**Port**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-421">**Port**</span></span> | <span data-ttu-id="2f0a1-422">Alla oanvända portar</span><span class="sxs-lookup"><span data-stu-id="2f0a1-422">Any unused port</span></span> | <span data-ttu-id="2f0a1-423">59999</span><span class="sxs-lookup"><span data-stu-id="2f0a1-423">59999</span></span> |
   | <span data-ttu-id="2f0a1-424">**Intervall**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-424">**Interval**</span></span>  | <span data-ttu-id="2f0a1-425">hello tidslängd mellan avsökningen försök i sekunder</span><span class="sxs-lookup"><span data-stu-id="2f0a1-425">hello amount of time between probe attempts in seconds</span></span> |<span data-ttu-id="2f0a1-426">5</span><span class="sxs-lookup"><span data-stu-id="2f0a1-426">5</span></span> |
   | <span data-ttu-id="2f0a1-427">**Tröskelvärde för ohälsosamt värde**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-427">**Unhealthy threshold**</span></span> | <span data-ttu-id="2f0a1-428">Hej antalet avsökningsfel måste ske för en virtuell dator toobe bedöms som dåligt</span><span class="sxs-lookup"><span data-stu-id="2f0a1-428">hello number of consecutive probe failures that must occur for a virtual machine toobe considered unhealthy</span></span>  | <span data-ttu-id="2f0a1-429">2</span><span class="sxs-lookup"><span data-stu-id="2f0a1-429">2</span></span> |

1. <span data-ttu-id="2f0a1-430">Klicka på **OK** tooset hello hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-430">Click **OK** tooset hello health probe.</span></span>

### <a name="set-hello-load-balancing-rules"></a><span data-ttu-id="2f0a1-431">Ange hello regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-431">Set hello load balancing rules</span></span>

1. <span data-ttu-id="2f0a1-432">Klicka på hello belastningsutjämnare, **belastningsutjämningsregler**, och klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-432">Click hello load balancer, click **Load balancing rules**, and click **+Add**.</span></span>

1. <span data-ttu-id="2f0a1-433">Ange hello belastningsutjämningsregler på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-433">Set hello load balancing rules as follows.</span></span>
   | <span data-ttu-id="2f0a1-434">Inställning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-434">Setting</span></span> | <span data-ttu-id="2f0a1-435">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-435">Description</span></span> | <span data-ttu-id="2f0a1-436">Exempel</span><span class="sxs-lookup"><span data-stu-id="2f0a1-436">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="2f0a1-437">**Namn**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-437">**Name**</span></span> | <span data-ttu-id="2f0a1-438">Text</span><span class="sxs-lookup"><span data-stu-id="2f0a1-438">Text</span></span> | <span data-ttu-id="2f0a1-439">SQLAlwaysOnEndPointListener</span><span class="sxs-lookup"><span data-stu-id="2f0a1-439">SQLAlwaysOnEndPointListener</span></span> |
   | <span data-ttu-id="2f0a1-440">**Frontend-IP-adress**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-440">**Frontend IP address**</span></span> | <span data-ttu-id="2f0a1-441">Välj en adress</span><span class="sxs-lookup"><span data-stu-id="2f0a1-441">Choose an address</span></span> |<span data-ttu-id="2f0a1-442">Använda hello-adress som du skapade när du skapade hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-442">Use hello address that you created when you created hello load balancer.</span></span> |
   | <span data-ttu-id="2f0a1-443">**Protokoll**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-443">**Protocol**</span></span> | <span data-ttu-id="2f0a1-444">Välj TCP</span><span class="sxs-lookup"><span data-stu-id="2f0a1-444">Choose TCP</span></span> |<span data-ttu-id="2f0a1-445">TCP</span><span class="sxs-lookup"><span data-stu-id="2f0a1-445">TCP</span></span> |
   | <span data-ttu-id="2f0a1-446">**Port**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-446">**Port**</span></span> | <span data-ttu-id="2f0a1-447">Använda hello port för SQL Server-instans för hello</span><span class="sxs-lookup"><span data-stu-id="2f0a1-447">Use hello port for hello SQL Server instance</span></span> | <span data-ttu-id="2f0a1-448">1433</span><span class="sxs-lookup"><span data-stu-id="2f0a1-448">1433</span></span> |
   | <span data-ttu-id="2f0a1-449">**Backend-Port**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-449">**Backend Port**</span></span> | <span data-ttu-id="2f0a1-450">Det här fältet används inte när flytande IP är inställd för direkta servern returnerade</span><span class="sxs-lookup"><span data-stu-id="2f0a1-450">This field is not used when Floating IP is set for direct server return</span></span> | <span data-ttu-id="2f0a1-451">1433</span><span class="sxs-lookup"><span data-stu-id="2f0a1-451">1433</span></span> |
   | <span data-ttu-id="2f0a1-452">**Avsökningen**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-452">**Probe**</span></span> |<span data-ttu-id="2f0a1-453">hello-namn du angav för hello avsökningen</span><span class="sxs-lookup"><span data-stu-id="2f0a1-453">hello name you specified for hello probe</span></span> | <span data-ttu-id="2f0a1-454">SQLAlwaysOnEndPointProbe</span><span class="sxs-lookup"><span data-stu-id="2f0a1-454">SQLAlwaysOnEndPointProbe</span></span> |
   | <span data-ttu-id="2f0a1-455">**Persistence för session**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-455">**Session Persistence**</span></span> | <span data-ttu-id="2f0a1-456">Listrutan</span><span class="sxs-lookup"><span data-stu-id="2f0a1-456">Drop down list</span></span> | <span data-ttu-id="2f0a1-457">**Ingen**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-457">**None**</span></span> |
   | <span data-ttu-id="2f0a1-458">**Inaktivitetstid**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-458">**Idle Timeout**</span></span> | <span data-ttu-id="2f0a1-459">Öppna minuter tookeep en TCP-anslutning</span><span class="sxs-lookup"><span data-stu-id="2f0a1-459">Minutes tookeep a TCP connection open</span></span> | <span data-ttu-id="2f0a1-460">4</span><span class="sxs-lookup"><span data-stu-id="2f0a1-460">4</span></span> |
   | <span data-ttu-id="2f0a1-461">**Flytande IP (direkt serverreturnering)**</span><span class="sxs-lookup"><span data-stu-id="2f0a1-461">**Floating IP (direct server return)**</span></span> | |<span data-ttu-id="2f0a1-462">Enabled</span><span class="sxs-lookup"><span data-stu-id="2f0a1-462">Enabled</span></span> |

   > [!WARNING]
   > <span data-ttu-id="2f0a1-463">Direkt serverreturnering anges när du skapar.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-463">Direct server return is set during creation.</span></span> <span data-ttu-id="2f0a1-464">Det går inte att ändra.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-464">It cannot be changed.</span></span>

1. <span data-ttu-id="2f0a1-465">Klicka på **OK** tooset hello belastningsutjämningsregler.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-465">Click **OK** tooset hello load balancing rules.</span></span>

## <span data-ttu-id="2f0a1-466"><a name="configure-listener"></a>Konfigurera hello-lyssnare</span><span class="sxs-lookup"><span data-stu-id="2f0a1-466"><a name="configure-listener"></a> Configure hello listener</span></span>

<span data-ttu-id="2f0a1-467">hello nästa sak toodo är tooconfigure en tillgänglighetsgruppens lyssnare på hello failover-kluster.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-467">hello next thing toodo is tooconfigure an Availability Group listener on hello failover cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="2f0a1-468">Den här kursen visar hur toocreate en enda lyssnare - med en ILB IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-468">This tutorial shows how toocreate a single listener - with one ILB IP address.</span></span> <span data-ttu-id="2f0a1-469">toocreate lyssnare med hjälp av en eller flera IP-adresser, se [Create Availability Group listener och belastningsutjämnare | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-469">toocreate one or more listeners using one or more IP addresses, see [Create Availability Group listener and load balancer | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a><span data-ttu-id="2f0a1-470">Ange lyssningsport</span><span class="sxs-lookup"><span data-stu-id="2f0a1-470">Set listener port</span></span>

<span data-ttu-id="2f0a1-471">Ange hello lyssningsport i SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-471">In SQL Server Management Studio, set hello listener port.</span></span>

1. <span data-ttu-id="2f0a1-472">Starta SQL Server Management Studio och Anslut toohello primära repliken.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-472">Launch SQL Server Management Studio and connect toohello primary replica.</span></span>

1. <span data-ttu-id="2f0a1-473">Navigera för**AlwaysOn hög tillgänglighet** | **Tillgänglighetsgrupper** | **tillgänglighet Tillgänglighetsgruppslyssnarnas**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-473">Navigate too**AlwaysOn High Availability** | **Availability Groups** | **Availability Group Listeners**.</span></span>

1. <span data-ttu-id="2f0a1-474">Du bör nu se hello grupplyssnarens namn som du skapade i hanteraren för redundanskluster.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-474">You should now see hello listener name that you created in Failover Cluster Manager.</span></span> <span data-ttu-id="2f0a1-475">Högerklicka på hello grupplyssnarens namn och på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-475">Right-click hello listener name and click **Properties**.</span></span>

1. <span data-ttu-id="2f0a1-476">I hello **Port** ange hello portnumret för hello tillgänglighetsgruppens lyssnare med hjälp av hello $EndpointPort du använde tidigare (1433 var hello standard), klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-476">In hello **Port** box, specify hello port number for hello Availability Group listener by using hello $EndpointPort you used earlier (1433 was hello default), then click **OK**.</span></span>

<span data-ttu-id="2f0a1-477">Nu har du en SQL Server-tillgänglighetsgrupp i Azure virtuella datorer som körs i läget Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-477">You now have a SQL Server Availability Group in Azure virtual machines running in Resource Manager mode.</span></span>

## <a name="test-connection-toolistener"></a><span data-ttu-id="2f0a1-478">Testa anslutning toolistener</span><span class="sxs-lookup"><span data-stu-id="2f0a1-478">Test connection toolistener</span></span>

<span data-ttu-id="2f0a1-479">tootest hello anslutning:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-479">tootest hello connection:</span></span>

1. <span data-ttu-id="2f0a1-480">RDP-tooa SQL Server som är i hello samma virtuella nätverk, men inte egna hello replik.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-480">RDP tooa SQL Server that is in hello same virtual network, but does not own hello replica.</span></span> <span data-ttu-id="2f0a1-481">Du kan använda hello andra SQL Server i hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-481">You can use hello other SQL Server in hello cluster.</span></span>

1. <span data-ttu-id="2f0a1-482">Använd **sqlcmd** verktyget tootest hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-482">Use **sqlcmd** utility tootest hello connection.</span></span> <span data-ttu-id="2f0a1-483">Till exempel följande skript hello upprättar en **sqlcmd** anslutning toohello primära repliken via hello lyssnare med Windows-autentisering:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-483">For example, hello following script establishes a **sqlcmd** connection toohello primary replica through hello listener with Windows authentication:</span></span>

    ```
    sqlcmd -S <listenerName> -E
    ```

    <span data-ttu-id="2f0a1-484">Om hello lyssnare använder en port än hello standard port (1433), ange hello port i hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-484">If hello listener is using a port other than hello default port (1433), specify hello port in hello connection string.</span></span> <span data-ttu-id="2f0a1-485">Till exempel ansluter hello följande kommando med sqlcmd tooa lyssnaren på port 1435:</span><span class="sxs-lookup"><span data-stu-id="2f0a1-485">For example, hello following sqlcmd command connects tooa listener at port 1435:</span></span>

    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

<span data-ttu-id="2f0a1-486">hello SQLCMD anslutning ansluter automatiskt toowhichever instans av SQL Server-värdar hello primära repliken.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-486">hello SQLCMD connection automatically connects toowhichever instance of SQL Server hosts hello primary replica.</span></span>

> [!TIP]
> <span data-ttu-id="2f0a1-487">Kontrollera att hello-port som du anger är öppen hello-brandväggen för både SQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-487">Make sure that hello port you specify is open on hello firewall of both SQL Servers.</span></span> <span data-ttu-id="2f0a1-488">Båda servrarna kräver en inkommande regel för hello TCP-port som du använder.</span><span class="sxs-lookup"><span data-stu-id="2f0a1-488">Both servers require an inbound rule for hello TCP port that you use.</span></span> <span data-ttu-id="2f0a1-489">Mer information finns i [Lägg till eller redigera brandväggsregel](http://technet.microsoft.com/library/cc753558.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-489">For more information, see [Add or Edit Firewall Rule](http://technet.microsoft.com/library/cc753558.aspx).</span></span>
>
>



<!--**Notes**: *Notes provide just-in-time info: A Note is “by hello way” info, an Important is info users need toocomplete a task, Tip is for shortcuts. Don’t overdo*.-->


<!--**Procedures**: *This is hello second “step." They often include substeps. Again, use a short title that tells users what they’ll do*. *("Configure a new web project.")*-->

<!--**UI**: *Note hello format for documenting hello UI: bold for UI elements and arrow keys for sequence. (Ex. Click **File > New > Project**.)*-->

<!--**Screenshot**: *Screenshots really help users. But don’t include too many since they’re difficult toomaintain. Highlight areas you are referring tooin red.*-->

<!--**No. of steps**: *Make sure hello number of steps within a procedure is 10 or fewer. Seven steps is ideal. Break up long procedure logically.*-->


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want toogo on.*-->

## <a name="next-steps"></a><span data-ttu-id="2f0a1-490">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2f0a1-490">Next steps</span></span>

- <span data-ttu-id="2f0a1-491">[Lägg till en IP-adress tooa belastningsutjämnare för en andra tillgänglighetsgruppen](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).</span><span class="sxs-lookup"><span data-stu-id="2f0a1-491">[Add an IP address tooa load balancer for a second availability group](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).</span></span>
