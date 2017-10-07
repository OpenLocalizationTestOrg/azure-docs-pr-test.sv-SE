---
title: aaaConnect tooAzure Analysis Services med Power BI | Microsoft Docs
description: "Lär dig hur tooconnect tooan Azure Analysis Services-servern med hjälp av Power BI."
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
ms.openlocfilehash: f6c4cdec6edb92900ad2e552e23a4d9172ba9b84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-power-bi"></a><span data-ttu-id="6c833-103">Ansluta med Powerbi</span><span class="sxs-lookup"><span data-stu-id="6c833-103">Connect with Power BI</span></span>

<span data-ttu-id="6c833-104">När du har skapat en server i Azure och distribuerat en tabellmodell tooit, användare i din organisation är klar tooconnect och utforska data.</span><span class="sxs-lookup"><span data-stu-id="6c833-104">Once you've created a server in Azure, and deployed a tabular model tooit, users in your organization are ready tooconnect and begin exploring data.</span></span> 

> [!TIP]
> <span data-ttu-id="6c833-105">Vara säker på att toouse hello senaste versionen av [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="6c833-105">Be sure toouse hello latest version of [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span></span>
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a><span data-ttu-id="6c833-106">Ansluta i Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="6c833-106">Connect in Power BI Desktop</span></span>

1. <span data-ttu-id="6c833-107">Klicka på Power BI Desktop **hämta Data** > **Azure** > **Azure Analysis Services-databasen**.</span><span class="sxs-lookup"><span data-stu-id="6c833-107">In Power BI Desktop, click **Get Data** > **Azure** > **Azure Analysis Services database**.</span></span>

2. <span data-ttu-id="6c833-108">I **Server**, ange hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="6c833-108">In **Server**, enter hello server name.</span></span> 
    
    <span data-ttu-id="6c833-109">Vara säker på att tooinclude hello fullständiga URL: en.</span><span class="sxs-lookup"><span data-stu-id="6c833-109">Be sure tooinclude hello full URL.</span></span> <span data-ttu-id="6c833-110">Till exempel asazure://westcentralus.asazure.windows.net/advworks.</span><span class="sxs-lookup"><span data-stu-id="6c833-110">For example, asazure://westcentralus.asazure.windows.net/advworks.</span></span>

3. <span data-ttu-id="6c833-111">I **databasen**, om du känner till hello tabellmodelldatabas eller perspektiv som du vill tooconnect och klistra in den här hello namn.</span><span class="sxs-lookup"><span data-stu-id="6c833-111">In **Database**, if you know hello name of hello tabular model database or perspective you want tooconnect to, paste it here.</span></span> <span data-ttu-id="6c833-112">Annars kan du lämna fältet tomt och välja en databas eller perspektiv senare.</span><span class="sxs-lookup"><span data-stu-id="6c833-112">Otherwise, you can leave this field blank and select a database or perspective later.</span></span>

4. <span data-ttu-id="6c833-113">Lämna hello standard **Anslut live** alternativet, och tryck sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="6c833-113">Leave hello default **Connect live** option, then press **Connect**.</span></span> 

5. <span data-ttu-id="6c833-114">Om du uppmanas ange dina inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="6c833-114">If prompted, enter your login credentials.</span></span> 

6. <span data-ttu-id="6c833-115">I **Navigator**, expandera hello server och markerar sedan hello modell eller perspektiv som du vill tooconnect till, klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="6c833-115">In **Navigator**, expand hello server, then select hello model or perspective you want tooconnect to, then click **Connect**.</span></span> <span data-ttu-id="6c833-116">Klicka på en modell- och perspektivparametrarna tooshow alla hello-objekt för att visa.</span><span class="sxs-lookup"><span data-stu-id="6c833-116">Click  a model or perspective tooshow all hello objects for that view.</span></span>

    <span data-ttu-id="6c833-117">hello modellen öppnas i Power BI Desktop med en tom rapport i rapportvyn.</span><span class="sxs-lookup"><span data-stu-id="6c833-117">hello model opens in Power BI Desktop with a blank report in Report view.</span></span> <span data-ttu-id="6c833-118">hello fältlistan visas alla icke-dolda modellobjekt.</span><span class="sxs-lookup"><span data-stu-id="6c833-118">hello Fields list displays all non-hidden model objects.</span></span> <span data-ttu-id="6c833-119">Anslutningsstatus visas i hello nedre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="6c833-119">Connection status is displayed in hello lower-right corner.</span></span>

## <a name="connect-in-power-bi-service"></a><span data-ttu-id="6c833-120">Ansluta i Power BI (service)</span><span class="sxs-lookup"><span data-stu-id="6c833-120">Connect in Power BI (service)</span></span>

1. <span data-ttu-id="6c833-121">Skapa en Power BI Desktop-fil som har en aktiv anslutning tooyour modell på servern.</span><span class="sxs-lookup"><span data-stu-id="6c833-121">Create a Power BI Desktop file that has a live connection tooyour model on your server.</span></span>
2. <span data-ttu-id="6c833-122">I [Power BI](https://powerbi.microsoft.com), klickar du på **hämta Data** > **filer**.</span><span class="sxs-lookup"><span data-stu-id="6c833-122">In [Power BI](https://powerbi.microsoft.com), click **Get Data** > **Files**.</span></span> <span data-ttu-id="6c833-123">Leta upp och markera filen.</span><span class="sxs-lookup"><span data-stu-id="6c833-123">Locate and select your file.</span></span>



## <a name="see-also"></a><span data-ttu-id="6c833-124">Se även</span><span class="sxs-lookup"><span data-stu-id="6c833-124">See also</span></span>
<span data-ttu-id="6c833-125">[Ansluta tooAzure Analysis Services](analysis-services-connect.md) </span><span class="sxs-lookup"><span data-stu-id="6c833-125">[Connect tooAzure Analysis Services](analysis-services-connect.md) </span></span>  
[<span data-ttu-id="6c833-126">Klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="6c833-126">Client libraries</span></span>](analysis-services-data-providers.md)

