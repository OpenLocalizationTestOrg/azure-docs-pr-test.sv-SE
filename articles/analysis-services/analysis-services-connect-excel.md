---
title: aaaConnect tooAzure Analysis Services med Excel | Microsoft Docs
description: "Lär dig hur tooconnect tooan Azure Analysis Services-servern med hjälp av Excel."
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
ms.openlocfilehash: 175b83e7d936716a626aa4b3bf22b5598bb983d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-excel"></a><span data-ttu-id="49e3c-103">Anslut till Excel</span><span class="sxs-lookup"><span data-stu-id="49e3c-103">Connect with Excel</span></span>

<span data-ttu-id="49e3c-104">När du har skapat en server i Azure och distribuerat en tabellmodell tooit, du är klar tooconnect och utforska data.</span><span class="sxs-lookup"><span data-stu-id="49e3c-104">Once you've created a server in Azure, and deployed a tabular model tooit, you're ready tooconnect and begin exploring data.</span></span>


## <a name="connect-in-excel"></a><span data-ttu-id="49e3c-105">Ansluta i Excel</span><span class="sxs-lookup"><span data-stu-id="49e3c-105">Connect in Excel</span></span>

<span data-ttu-id="49e3c-106">Ansluter tooa server i Excel stöds med hämta Data i Excel 2016.</span><span class="sxs-lookup"><span data-stu-id="49e3c-106">Connecting tooa server in Excel is supported by using Get Data in Excel 2016.</span></span> <span data-ttu-id="49e3c-107">Anslutning med hjälp av hello av guiden Importera tabell i Power Pivot stöds inte.</span><span class="sxs-lookup"><span data-stu-id="49e3c-107">Connecting by using hello Import Table Wizard in Power Pivot is not supported.</span></span> 

<span data-ttu-id="49e3c-108">**tooconnect i Excel 2016**</span><span class="sxs-lookup"><span data-stu-id="49e3c-108">**tooconnect in Excel 2016**</span></span>

1. <span data-ttu-id="49e3c-109">I Excel 2016 på hello **Data** band klickar du på **hämta externa Data** > **från andra källor** > **från Analysis Services** .</span><span class="sxs-lookup"><span data-stu-id="49e3c-109">In Excel 2016, on hello **Data** ribbon, click **Get External Data** > **From Other Sources** > **From Analysis Services**.</span></span>

2. <span data-ttu-id="49e3c-110">I hello guiden i **servernamn**, ange hello servernamn inklusive protokoll och URI.</span><span class="sxs-lookup"><span data-stu-id="49e3c-110">In hello Data Connection Wizard, in **Server name**, enter hello server name including protocol and URI.</span></span> <span data-ttu-id="49e3c-111">I **logga in autentiseringsuppgifter**väljer **Använd hello följande användarnamn och lösenord**, och skriv sedan hello organisationens namn, till exempel nancy@adventureworks.com, och lösenord.</span><span class="sxs-lookup"><span data-stu-id="49e3c-111">Then, in **Logon credentials**, select **Use hello following User Name and Password**, and then type hello organizational user name, for example nancy@adventureworks.com, and password.</span></span>

    ![Ansluta från Excel-inloggning](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. <span data-ttu-id="49e3c-113">I **Välj databas och tabell**välja hello databas och modell eller perspektiv och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="49e3c-113">In **Select Database and Table**, select hello database and model or perspective, and then click **Finish**.</span></span>
   
    ![Ansluta från Excel väljer modellen](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a><span data-ttu-id="49e3c-115">Se även</span><span class="sxs-lookup"><span data-stu-id="49e3c-115">See also</span></span>
<span data-ttu-id="49e3c-116">[Klientbibliotek](analysis-services-data-providers.md) </span><span class="sxs-lookup"><span data-stu-id="49e3c-116">[Client libraries](analysis-services-data-providers.md) </span></span>  
[<span data-ttu-id="49e3c-117">Hantera servern</span><span class="sxs-lookup"><span data-stu-id="49e3c-117">Manage your server</span></span>](analysis-services-manage.md)     


