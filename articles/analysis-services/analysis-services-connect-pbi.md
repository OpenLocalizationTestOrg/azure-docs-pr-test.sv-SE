---
title: Anslut till Azure Analysis Services med Powerbi | Microsoft Docs
description: "Lär dig hur du ansluter till en Azure Analysis Services-server med hjälp av Power BI."
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
ms.openlocfilehash: 3a2af043feddb4a1d6d63f50e838c8a39035449f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connect-with-power-bi"></a><span data-ttu-id="9502d-103">Ansluta med Powerbi</span><span class="sxs-lookup"><span data-stu-id="9502d-103">Connect with Power BI</span></span>

<span data-ttu-id="9502d-104">När du har skapat en server i Azure och distribuerat en tabellmodell till den, är användare i din organisation redo att ansluta och börja utforska data.</span><span class="sxs-lookup"><span data-stu-id="9502d-104">Once you've created a server in Azure, and deployed a tabular model to it, users in your organization are ready to connect and begin exploring data.</span></span> 

> [!TIP]
> <span data-ttu-id="9502d-105">Se till att använda den senaste versionen av [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="9502d-105">Be sure to use the latest version of [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span></span>
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a><span data-ttu-id="9502d-106">Ansluta i Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="9502d-106">Connect in Power BI Desktop</span></span>

1. <span data-ttu-id="9502d-107">Klicka på Power BI Desktop **hämta Data** > **Azure** > **Azure Analysis Services-databasen**.</span><span class="sxs-lookup"><span data-stu-id="9502d-107">In Power BI Desktop, click **Get Data** > **Azure** > **Azure Analysis Services database**.</span></span>

2. <span data-ttu-id="9502d-108">I **Server**, ange namnet på servern.</span><span class="sxs-lookup"><span data-stu-id="9502d-108">In **Server**, enter the server name.</span></span> 
    
    <span data-ttu-id="9502d-109">Se till att inkludera den fullständiga URL: en.</span><span class="sxs-lookup"><span data-stu-id="9502d-109">Be sure to include the full URL.</span></span> <span data-ttu-id="9502d-110">Till exempel asazure://westcentralus.asazure.windows.net/advworks.</span><span class="sxs-lookup"><span data-stu-id="9502d-110">For example, asazure://westcentralus.asazure.windows.net/advworks.</span></span>

3. <span data-ttu-id="9502d-111">I **databasen**, om du känner till namnet på tabellmodelldatabas eller perspektiv som du vill ansluta till, klistra in den här.</span><span class="sxs-lookup"><span data-stu-id="9502d-111">In **Database**, if you know the name of the tabular model database or perspective you want to connect to, paste it here.</span></span> <span data-ttu-id="9502d-112">Annars kan du lämna fältet tomt och välja en databas eller perspektiv senare.</span><span class="sxs-lookup"><span data-stu-id="9502d-112">Otherwise, you can leave this field blank and select a database or perspective later.</span></span>

4. <span data-ttu-id="9502d-113">Låt standardvärdet **Anslut live** alternativet, och tryck sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="9502d-113">Leave the default **Connect live** option, then press **Connect**.</span></span> 

5. <span data-ttu-id="9502d-114">Om du uppmanas ange dina inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="9502d-114">If prompted, enter your login credentials.</span></span> 

6. <span data-ttu-id="9502d-115">I **Navigator**, expandera servern och välj sedan modellen eller perspektiv som du vill ansluta till, klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="9502d-115">In **Navigator**, expand the server, then select the model or perspective you want to connect to, then click **Connect**.</span></span> <span data-ttu-id="9502d-116">Klicka på en modell eller perspektiv för att visa alla objekt i vyn.</span><span class="sxs-lookup"><span data-stu-id="9502d-116">Click  a model or perspective to show all the objects for that view.</span></span>

    <span data-ttu-id="9502d-117">Modellen öppnas i Power BI Desktop med en tom rapport i rapportvyn.</span><span class="sxs-lookup"><span data-stu-id="9502d-117">The model opens in Power BI Desktop with a blank report in Report view.</span></span> <span data-ttu-id="9502d-118">Listan över fält visar alla icke-dolda modellobjekt.</span><span class="sxs-lookup"><span data-stu-id="9502d-118">The Fields list displays all non-hidden model objects.</span></span> <span data-ttu-id="9502d-119">Anslutningsstatus visas i det nedre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="9502d-119">Connection status is displayed in the lower-right corner.</span></span>

## <a name="connect-in-power-bi-service"></a><span data-ttu-id="9502d-120">Ansluta i Power BI (service)</span><span class="sxs-lookup"><span data-stu-id="9502d-120">Connect in Power BI (service)</span></span>

1. <span data-ttu-id="9502d-121">Skapa en Power BI Desktop-fil som har en aktiv anslutning till din modell på servern.</span><span class="sxs-lookup"><span data-stu-id="9502d-121">Create a Power BI Desktop file that has a live connection to your model on your server.</span></span>
2. <span data-ttu-id="9502d-122">I [Power BI](https://powerbi.microsoft.com), klickar du på **hämta Data** > **filer**.</span><span class="sxs-lookup"><span data-stu-id="9502d-122">In [Power BI](https://powerbi.microsoft.com), click **Get Data** > **Files**.</span></span> <span data-ttu-id="9502d-123">Leta upp och markera filen.</span><span class="sxs-lookup"><span data-stu-id="9502d-123">Locate and select your file.</span></span>



## <a name="see-also"></a><span data-ttu-id="9502d-124">Se även</span><span class="sxs-lookup"><span data-stu-id="9502d-124">See also</span></span>
<span data-ttu-id="9502d-125">[Anslut till Azure Analysis Services](analysis-services-connect.md) </span><span class="sxs-lookup"><span data-stu-id="9502d-125">[Connect to Azure Analysis Services](analysis-services-connect.md) </span></span>  
[<span data-ttu-id="9502d-126">Klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="9502d-126">Client libraries</span></span>](analysis-services-data-providers.md)

