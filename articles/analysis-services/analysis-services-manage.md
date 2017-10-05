---
title: Hantera Azure Analysis Services | Microsoft Docs
description: "Lär dig hur du hanterar en Analysis Services-server i Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 79491d0b-b00d-4e02-9ca7-adc99bc02fdb
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: b897e81351ebee11c292e67ac76ba8202a6f0108
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-analysis-services"></a><span data-ttu-id="d4ebe-103">Hantera Analysis Services</span><span class="sxs-lookup"><span data-stu-id="d4ebe-103">Manage Analysis Services</span></span>
<span data-ttu-id="d4ebe-104">När du har skapat en Analysis Services-server i Azure kan finnas det vissa administrations- och uppgifter som du behöver utföra direkt eller senare ned väg.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-104">Once you've created an Analysis Services server in Azure, there may be some administration and management tasks you need to perform right away or sometime down the road.</span></span> <span data-ttu-id="d4ebe-105">Till exempel bearbetning för att uppdatera data, styra vem som kan komma åt modeller på servern eller övervaka ditt Servertillstånd.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-105">For example, run processing to the refresh data, control who can access the models on your server, or monitor your server's health.</span></span> <span data-ttu-id="d4ebe-106">Vissa hanteringsuppgifter kan endast utföras i Azure-portalen andra i SQL Server Management Studio (SSMS), och vissa aktiviteter kan göras antingen i.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-106">Some management tasks can only be performed in Azure portal, others in SQL Server Management Studio (SSMS), and some tasks can be done in either.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="d4ebe-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d4ebe-107">Azure portal</span></span>
<span data-ttu-id="d4ebe-108">[Azure-portalen](http://portal.azure.com/) är där du kan skapa och ta bort servrar, övervaka serverresurser, ändra storlek och hantera vem som har åtkomst till dina servrar.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-108">[Azure portal](http://portal.azure.com/) is where you can create and delete servers, monitor server resources, change size, and manage who has access to your servers.</span></span>  <span data-ttu-id="d4ebe-109">Om du har några problem, kan du även skicka en supportförfrågan.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-109">If you're having some problems, you can also submit a support request.</span></span>

![Hämta servernamnet i Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a><span data-ttu-id="d4ebe-111">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="d4ebe-111">SQL Server Management Studio</span></span>
<span data-ttu-id="d4ebe-112">Ansluter till servern i Azure är precis som ansluter till en server-instans i din organisation.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-112">Connecting to your server in Azure is just like connecting to a server instance in your own organization.</span></span> <span data-ttu-id="d4ebe-113">Från SSMS, du utföra samma uppgifter, till exempel processdata eller skapa ett skript för bearbetning, hantera roller och använder PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-113">From SSMS, you can perform many of the same tasks such as process data or create a processing script, manage roles, and use PowerShell.</span></span>
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a><span data-ttu-id="d4ebe-115">Hämta och installera SSMS</span><span class="sxs-lookup"><span data-stu-id="d4ebe-115">Download and install SSMS</span></span>
<span data-ttu-id="d4ebe-116">För att få de senaste funktionerna och erfarenhet som jämnaste vid anslutning till Azure Analysis Services-server kan vara att du använder den senaste versionen av SSMS.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-116">To get all the latest features, and the smoothest experience when connecting to your Azure Analysis Services server, be sure you're using the latest version of SSMS.</span></span> 

<span data-ttu-id="d4ebe-117">[Hämta SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="d4ebe-117">[Download SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>


### <a name="to-connect-with-ssms"></a><span data-ttu-id="d4ebe-118">Att ansluta med SSMS</span><span class="sxs-lookup"><span data-stu-id="d4ebe-118">To connect with SSMS</span></span>
 <span data-ttu-id="d4ebe-119">När du använder SSMS, innan du ansluter till servern för första gången, kontrollera att ditt användarnamn ingår i gruppen Administratörer för Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-119">When using SSMS, before connecting to your server the first time, make sure your username is included in the Analysis Services Admins group.</span></span> <span data-ttu-id="d4ebe-120">Läs mer i [serveradministratörer](#server-administrators) senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-120">To learn more, see [Server administrators](#server-administrators) later in this article.</span></span>

1. <span data-ttu-id="d4ebe-121">Du måste hämta namnet på servern innan du ansluter.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-121">Before you connect, you need to get the server name.</span></span> <span data-ttu-id="d4ebe-122">Välj **Azure Portal** > server > **Översikt** > **Servernamn** och kopiera servernamnet.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-122">In **Azure portal** > server > **Overview** > **Server name**, copy the server name.</span></span>
   
    ![Hämta servernamnet i Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. <span data-ttu-id="d4ebe-124">I SSMS > **Object Explorer**, klickar du på **Anslut** > **Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-124">In SSMS > **Object Explorer**, click **Connect** > **Analysis Services**.</span></span>
3. <span data-ttu-id="d4ebe-125">I den **Anslut till Server** klistra in i namnet på server och sedan i dialogrutan **autentisering**, väljer du något av följande autentiseringstyper av:</span><span class="sxs-lookup"><span data-stu-id="d4ebe-125">In the **Connect to Server** dialog box, paste in the server name, then in **Authentication**, choose one of the following authentication types:</span></span>
   
    <span data-ttu-id="d4ebe-126">**Windows-autentisering** att använda dina Windows-autentiseringsuppgifter domän\användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-126">**Windows Authentication** to use your Windows domain\username and password credentials.</span></span>

    <span data-ttu-id="d4ebe-127">**Active Directory-lösenordsautentisering** att använda ett organisationskonto.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-127">**Active Directory Password Authentication** to use an organizational account.</span></span> <span data-ttu-id="d4ebe-128">När ansluter från en icke-domän exempelvis anslutna datorn.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-128">For example, when connecting from a non-domain joined computer.</span></span>

    <span data-ttu-id="d4ebe-129">**Active Directory Universal autentisering** att använda [icke-interaktiv eller Multi-Factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="d4ebe-129">**Active Directory Universal Authentication** to use [non-interactive or multi-factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md).</span></span> 
   
    ![Ansluta i SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a><span data-ttu-id="d4ebe-131">Server-administratörer och användare</span><span class="sxs-lookup"><span data-stu-id="d4ebe-131">Server administrators and database users</span></span>
<span data-ttu-id="d4ebe-132">Det finns två typer av användare, administratörer och användare i Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-132">In Azure Analysis Services, there are two types of users, server administrators and database users.</span></span> <span data-ttu-id="d4ebe-133">Båda typerna av användare måste finnas i Azure Active Directory och måste anges med organisationens e-postadress eller UPN.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-133">Both types of users must be in your Azure Active Directory and must be specified by organizational email address or UPN.</span></span> <span data-ttu-id="d4ebe-134">Mer information finns i [Autentisering och användarbehörigheter](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="d4ebe-134">To learn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>


## <a name="troubleshooting-connection-problems"></a><span data-ttu-id="d4ebe-135">Felsökning av anslutningsproblem</span><span class="sxs-lookup"><span data-stu-id="d4ebe-135">Troubleshooting connection problems</span></span>
<span data-ttu-id="d4ebe-136">När du ansluter med hjälp av SSMS, om du stöter på problem kan behöva du rensa cacheminnet för inloggning.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-136">When connecting using SSMS, if you run into problems, you may need to clear the login cache.</span></span> <span data-ttu-id="d4ebe-137">Inget cachelagras-skiva. Om du vill rensa cachen, Stäng och starta om processen connect.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-137">Nothing is cached to disc. To clear the cache, close and restart the connect process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d4ebe-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d4ebe-138">Next steps</span></span>
<span data-ttu-id="d4ebe-139">Om du inte redan har distribuerat en tabellmodell till den nya servern nu är ett bra tillfälle.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-139">If you haven't already deployed a tabular model to your new server, now is a good time.</span></span> <span data-ttu-id="d4ebe-140">Läs mer i [Distribuera till Azure Analysis Services](analysis-services-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="d4ebe-140">To learn more, see [Deploy to Azure Analysis Services](analysis-services-deploy.md).</span></span>

<span data-ttu-id="d4ebe-141">Om du har distribuerat en modell till servern, är du redo att ansluta till den med hjälp av en klient eller en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d4ebe-141">If you've deployed a model to your server, you're ready to connect to it using a client or browser.</span></span> <span data-ttu-id="d4ebe-142">Läs mer i [hämta data från Azure Analysis Services-servern](analysis-services-connect.md).</span><span class="sxs-lookup"><span data-stu-id="d4ebe-142">To learn more, see [Get data from Azure Analysis Services server](analysis-services-connect.md).</span></span>

