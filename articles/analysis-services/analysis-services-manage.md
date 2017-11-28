---
title: aaaManage Azure Analysis Services | Microsoft Docs
description: "Lär dig hur toomanage en Analysis Services-server i Azure."
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
ms.openlocfilehash: b03bc440801a68162039e28cdb4f863da374014e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-analysis-services"></a><span data-ttu-id="70f35-103">Hantera Analysis Services</span><span class="sxs-lookup"><span data-stu-id="70f35-103">Manage Analysis Services</span></span>
<span data-ttu-id="70f35-104">När du har skapat en Analysis Services-server i Azure, kanske vissa administrations- och uppgifter som du behöver tooperform direkt eller senare på hello väg.</span><span class="sxs-lookup"><span data-stu-id="70f35-104">Once you've created an Analysis Services server in Azure, there may be some administration and management tasks you need tooperform right away or sometime down hello road.</span></span> <span data-ttu-id="70f35-105">Till exempel toohello uppdatera data, styra vem som kan komma åt hello modeller på servern eller övervaka ditt Servertillstånd bearbetas.</span><span class="sxs-lookup"><span data-stu-id="70f35-105">For example, run processing toohello refresh data, control who can access hello models on your server, or monitor your server's health.</span></span> <span data-ttu-id="70f35-106">Vissa hanteringsuppgifter kan endast utföras i Azure-portalen andra i SQL Server Management Studio (SSMS), och vissa aktiviteter kan göras antingen i.</span><span class="sxs-lookup"><span data-stu-id="70f35-106">Some management tasks can only be performed in Azure portal, others in SQL Server Management Studio (SSMS), and some tasks can be done in either.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="70f35-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="70f35-107">Azure portal</span></span>
<span data-ttu-id="70f35-108">[Azure-portalen](http://portal.azure.com/) är där du kan skapa och ta bort servrar, övervaka serverresurser, ändra storlek och hantera vem som har åtkomst tooyour servrar.</span><span class="sxs-lookup"><span data-stu-id="70f35-108">[Azure portal](http://portal.azure.com/) is where you can create and delete servers, monitor server resources, change size, and manage who has access tooyour servers.</span></span>  <span data-ttu-id="70f35-109">Om du har några problem, kan du även skicka en supportförfrågan.</span><span class="sxs-lookup"><span data-stu-id="70f35-109">If you're having some problems, you can also submit a support request.</span></span>

![Hämta servernamnet i Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a><span data-ttu-id="70f35-111">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="70f35-111">SQL Server Management Studio</span></span>
<span data-ttu-id="70f35-112">Ansluter tooyour server i Azure är precis som ansluter tooa server-instans i din organisation.</span><span class="sxs-lookup"><span data-stu-id="70f35-112">Connecting tooyour server in Azure is just like connecting tooa server instance in your own organization.</span></span> <span data-ttu-id="70f35-113">Du kan utföra många av hello samma uppgifter, till exempel processdata eller skapa ett skript för bearbetning, hantera roller och använda PowerShell från SSMS.</span><span class="sxs-lookup"><span data-stu-id="70f35-113">From SSMS, you can perform many of hello same tasks such as process data or create a processing script, manage roles, and use PowerShell.</span></span>
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a><span data-ttu-id="70f35-115">Hämta och installera SSMS</span><span class="sxs-lookup"><span data-stu-id="70f35-115">Download and install SSMS</span></span>
<span data-ttu-id="70f35-116">tooget alla hello senaste funktionerna och hello jämnaste upplevelse när de ansluter tooyour Azure Analysis Services-servern måste du använder hello senaste versionen av SSMS.</span><span class="sxs-lookup"><span data-stu-id="70f35-116">tooget all hello latest features, and hello smoothest experience when connecting tooyour Azure Analysis Services server, be sure you're using hello latest version of SSMS.</span></span> 

<span data-ttu-id="70f35-117">[Hämta SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="70f35-117">[Download SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>


### <a name="tooconnect-with-ssms"></a><span data-ttu-id="70f35-118">tooconnect med SSMS</span><span class="sxs-lookup"><span data-stu-id="70f35-118">tooconnect with SSMS</span></span>
 <span data-ttu-id="70f35-119">Kontrollera ditt användarnamn ingår i administratörsgruppen för hello analys när med hjälp av SSMS, innan du ansluter tooyour serverns hälsning första gången.</span><span class="sxs-lookup"><span data-stu-id="70f35-119">When using SSMS, before connecting tooyour server hello first time, make sure your username is included in hello Analysis Services Admins group.</span></span> <span data-ttu-id="70f35-120">Det finns fler toolearn [serveradministratörer](#server-administrators) senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="70f35-120">toolearn more, see [Server administrators](#server-administrators) later in this article.</span></span>

1. <span data-ttu-id="70f35-121">Innan du ansluter måste tooget hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="70f35-121">Before you connect, you need tooget hello server name.</span></span> <span data-ttu-id="70f35-122">I **Azure-portalen** > server > **översikt** > **servernamn**, kopiera hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="70f35-122">In **Azure portal** > server > **Overview** > **Server name**, copy hello server name.</span></span>
   
    ![Hämta servernamnet i Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. <span data-ttu-id="70f35-124">I SSMS > **Object Explorer**, klickar du på **Anslut** > **Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="70f35-124">In SSMS > **Object Explorer**, click **Connect** > **Analysis Services**.</span></span>
3. <span data-ttu-id="70f35-125">I hello **ansluta tooServer** klistra in i hello-servernamn, sedan i dialogrutan **autentisering**, väljer du något av följande typer av autentisering hello:</span><span class="sxs-lookup"><span data-stu-id="70f35-125">In hello **Connect tooServer** dialog box, paste in hello server name, then in **Authentication**, choose one of hello following authentication types:</span></span>
   
    <span data-ttu-id="70f35-126">**Windows-autentisering** toouse dina Windows-autentiseringsuppgifter domän\användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="70f35-126">**Windows Authentication** toouse your Windows domain\username and password credentials.</span></span>

    <span data-ttu-id="70f35-127">**Active Directory-lösenordsautentisering** toouse ett organisationskonto.</span><span class="sxs-lookup"><span data-stu-id="70f35-127">**Active Directory Password Authentication** toouse an organizational account.</span></span> <span data-ttu-id="70f35-128">När ansluter från en icke-domän exempelvis anslutna datorn.</span><span class="sxs-lookup"><span data-stu-id="70f35-128">For example, when connecting from a non-domain joined computer.</span></span>

    <span data-ttu-id="70f35-129">**Active Directory Universal autentisering** toouse [icke-interaktiv eller Multi-Factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="70f35-129">**Active Directory Universal Authentication** toouse [non-interactive or multi-factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md).</span></span> 
   
    ![Ansluta i SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a><span data-ttu-id="70f35-131">Server-administratörer och användare</span><span class="sxs-lookup"><span data-stu-id="70f35-131">Server administrators and database users</span></span>
<span data-ttu-id="70f35-132">Det finns två typer av användare, administratörer och användare i Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="70f35-132">In Azure Analysis Services, there are two types of users, server administrators and database users.</span></span> <span data-ttu-id="70f35-133">Båda typerna av användare måste finnas i Azure Active Directory och måste anges med organisationens e-postadress eller UPN.</span><span class="sxs-lookup"><span data-stu-id="70f35-133">Both types of users must be in your Azure Active Directory and must be specified by organizational email address or UPN.</span></span> <span data-ttu-id="70f35-134">Det finns fler toolearn [autentisering och användarbehörigheter](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="70f35-134">toolearn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>


## <a name="troubleshooting-connection-problems"></a><span data-ttu-id="70f35-135">Felsökning av anslutningsproblem</span><span class="sxs-lookup"><span data-stu-id="70f35-135">Troubleshooting connection problems</span></span>
<span data-ttu-id="70f35-136">När du ansluter med hjälp av SSMS, om du stöter på problem kan behöva tooclear hello inloggningen cache.</span><span class="sxs-lookup"><span data-stu-id="70f35-136">When connecting using SSMS, if you run into problems, you may need tooclear hello login cache.</span></span> <span data-ttu-id="70f35-137">Inget är cachelagrade toodisc.</span><span class="sxs-lookup"><span data-stu-id="70f35-137">Nothing is cached toodisc.</span></span> <span data-ttu-id="70f35-138">tooclear hello cache, Stäng och starta om hello ansluta processen.</span><span class="sxs-lookup"><span data-stu-id="70f35-138">tooclear hello cache, close and restart hello connect process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="70f35-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70f35-139">Next steps</span></span>
<span data-ttu-id="70f35-140">Om du inte redan har distribuerat en tabellmodell tooyour ny server, nu är ett bra tillfälle.</span><span class="sxs-lookup"><span data-stu-id="70f35-140">If you haven't already deployed a tabular model tooyour new server, now is a good time.</span></span> <span data-ttu-id="70f35-141">Det finns fler toolearn [distribuera tooAzure Analysis Services](analysis-services-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="70f35-141">toolearn more, see [Deploy tooAzure Analysis Services](analysis-services-deploy.md).</span></span>

<span data-ttu-id="70f35-142">Om du har distribuerat en modell tooyour server, är du redo tooconnect tooit med hjälp av en klient eller en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="70f35-142">If you've deployed a model tooyour server, you're ready tooconnect tooit using a client or browser.</span></span> <span data-ttu-id="70f35-143">Det finns fler toolearn [hämta data från Azure Analysis Services-servern](analysis-services-connect.md).</span><span class="sxs-lookup"><span data-stu-id="70f35-143">toolearn more, see [Get data from Azure Analysis Services server](analysis-services-connect.md).</span></span>

