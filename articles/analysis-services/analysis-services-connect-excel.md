---
title: Anslut till Azure Analysis Services med Excel | Microsoft Docs
description: "Lär dig hur du ansluter till en Azure Analysis Services-server med hjälp av Excel."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: d51b6980120f1cf9bc8d053d463a73ac600b915f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connect-with-excel"></a><span data-ttu-id="d562c-103">Anslut till Excel</span><span class="sxs-lookup"><span data-stu-id="d562c-103">Connect with Excel</span></span>

<span data-ttu-id="d562c-104">När du har skapat en server i Azure och distribuerat en tabellmodell till den, är du redo att ansluta och utforska data.</span><span class="sxs-lookup"><span data-stu-id="d562c-104">Once you've created a server in Azure, and deployed a tabular model to it, you're ready to connect and begin exploring data.</span></span>


## <a name="connect-in-excel"></a><span data-ttu-id="d562c-105">Ansluta i Excel</span><span class="sxs-lookup"><span data-stu-id="d562c-105">Connect in Excel</span></span>

<span data-ttu-id="d562c-106">Ansluter till en server i Excel stöds med hämta Data i Excel 2016.</span><span class="sxs-lookup"><span data-stu-id="d562c-106">Connecting to a server in Excel is supported by using Get Data in Excel 2016.</span></span> <span data-ttu-id="d562c-107">Det går inte att ansluta med hjälp av guiden Importera tabell i Power Pivot.</span><span class="sxs-lookup"><span data-stu-id="d562c-107">Connecting by using the Import Table Wizard in Power Pivot is not supported.</span></span> 

<span data-ttu-id="d562c-108">**Ansluta Excel 2016**</span><span class="sxs-lookup"><span data-stu-id="d562c-108">**To connect in Excel 2016**</span></span>

1. <span data-ttu-id="d562c-109">I Excel 2016, på den **Data** band klickar du på **hämta externa Data** > **från andra källor** > **från Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="d562c-109">In Excel 2016, on the **Data** ribbon, click **Get External Data** > **From Other Sources** > **From Analysis Services**.</span></span>

2. <span data-ttu-id="d562c-110">I guiden Dataanslutning i **servernamn**, ange namnet på servern inklusive protokoll och URI.</span><span class="sxs-lookup"><span data-stu-id="d562c-110">In the Data Connection Wizard, in **Server name**, enter the server name including protocol and URI.</span></span> <span data-ttu-id="d562c-111">I **logga in autentiseringsuppgifter**väljer **använder följande användarnamn och lösenord**, och skriv sedan organisationens användarnamnet, till exempel nancy@adventureworks.com, och lösenord.</span><span class="sxs-lookup"><span data-stu-id="d562c-111">Then, in **Logon credentials**, select **Use the following User Name and Password**, and then type the organizational user name, for example nancy@adventureworks.com, and password.</span></span>

    ![Ansluta från Excel-inloggning](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. <span data-ttu-id="d562c-113">I **Välj databas och tabell**, väljer du databasen och modellen eller perspektiv och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="d562c-113">In **Select Database and Table**, select the database and model or perspective, and then click **Finish**.</span></span>
   
    ![Ansluta från Excel väljer modellen](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a><span data-ttu-id="d562c-115">Se även</span><span class="sxs-lookup"><span data-stu-id="d562c-115">See also</span></span>
<span data-ttu-id="d562c-116">[Klientbibliotek](analysis-services-data-providers.md) </span><span class="sxs-lookup"><span data-stu-id="d562c-116">[Client libraries](analysis-services-data-providers.md) </span></span>  
[<span data-ttu-id="d562c-117">Hantera servern</span><span class="sxs-lookup"><span data-stu-id="d562c-117">Manage your server</span></span>](analysis-services-manage.md)     


