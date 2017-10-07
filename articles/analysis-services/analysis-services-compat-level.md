---
title: "kompatibilitetsnivån för aaaData modellen i Azure Analysis Services | Microsoft Docs"
description: "Förstå tabelldata modellen kompatibilitetsnivå."
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
ms.date: 08/16/2017
ms.author: owend
ms.openlocfilehash: bfaf0c60666729d1e6e0baf082c046ea9faa4e86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a><span data-ttu-id="fca85-103">Kompatibilitetsnivån för Analysis Services-tabellmodeller</span><span class="sxs-lookup"><span data-stu-id="fca85-103">Compatibility level for Analysis Services tabular models</span></span>

<span data-ttu-id="fca85-104">*Kompatibilitetsnivån* refererar toorelease-specifika funktioner i hello Analysis Services-motorn.</span><span class="sxs-lookup"><span data-stu-id="fca85-104">*Compatibility level* refers toorelease-specific behaviors in hello Analysis Services engine.</span></span> <span data-ttu-id="fca85-105">Ändringar toohello kompatibilitetsnivå sammanfaller normalt med större versioner av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fca85-105">Changes toohello compatibility level typically coincide with major releases of SQL Server.</span></span> <span data-ttu-id="fca85-106">De här ändringarna implementeras också i Azure Analysis Services toomaintain paritet mellan båda plattformar.</span><span class="sxs-lookup"><span data-stu-id="fca85-106">These changes are also implemented in Azure Analysis Services toomaintain parity between both platforms.</span></span> <span data-ttu-id="fca85-107">Kompatibilitet ändras även påverka funktionerna i din tabellmodeller.</span><span class="sxs-lookup"><span data-stu-id="fca85-107">Compatibility level changes also affect features available in your tabular models.</span></span> <span data-ttu-id="fca85-108">Till exempel har DirectQuery och tabular objektmetadata olika implementeringar beroende på hello kompatibilitetsnivå.</span><span class="sxs-lookup"><span data-stu-id="fca85-108">For example, DirectQuery and tabular object metadata have different implementations depending on hello compatibility level.</span></span> 

<span data-ttu-id="fca85-109">Azure Analysis Services har stöd för tabellmodeller på hello 1200 och 1400 kompatibilitetsnivåer.</span><span class="sxs-lookup"><span data-stu-id="fca85-109">Azure Analysis Services supports tabular models at hello 1200 and 1400 compatibility levels.</span></span>

<span data-ttu-id="fca85-110">hello senaste kompatibilitetsnivå är 1400.</span><span class="sxs-lookup"><span data-stu-id="fca85-110">hello latest compatibility level is 1400.</span></span> <span data-ttu-id="fca85-111">Den här nivån sammanfaller med SQL Server 2017 Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="fca85-111">This level coincides with SQL Server 2017 Analysis Services.</span></span> <span data-ttu-id="fca85-112">Viktiga funktioner i hello 1400 kompatibilitetsnivå omfattar:</span><span class="sxs-lookup"><span data-stu-id="fca85-112">Major features in hello 1400 compatibility level include:</span></span>

*  <span data-ttu-id="fca85-113">Ny infrastruktur för dataanslutning och importeras till tabellmodeller med stöd för TOM APIs och TMSL skript.</span><span class="sxs-lookup"><span data-stu-id="fca85-113">New infrastructure for data connectivity and import into tabular models with support for TOM APIs and TMSL scripting.</span></span> <span data-ttu-id="fca85-114">Den här nya funktionen aktiverar stöd för andra datakällor, till exempel Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="fca85-114">This new feature enables support for additional data sources such as Azure Blob storage.</span></span>
*  <span data-ttu-id="fca85-115">Dataomvandling av och data mashup-funktioner med hjälp av hämta Data och M uttryck.</span><span class="sxs-lookup"><span data-stu-id="fca85-115">Data transformation and data mashup capabilities by using Get Data and M expressions.</span></span>
*  <span data-ttu-id="fca85-116">Åtgärder som stöd för en detaljrader egenskap med ett DAX-uttryck.</span><span class="sxs-lookup"><span data-stu-id="fca85-116">Measures support a Detail Rows property with a DAX expression.</span></span> <span data-ttu-id="fca85-117">Den här egenskapen gör klientverktyg som Microsoft Excel toodrill toodetailed data från en sammansatt rapport.</span><span class="sxs-lookup"><span data-stu-id="fca85-117">This property enables client tools like Microsoft Excel toodrill down toodetailed data from an aggregated report.</span></span> <span data-ttu-id="fca85-118">När användare visar total försäljning för en region och månad, kan de visa hello associerade orderinformationen.</span><span class="sxs-lookup"><span data-stu-id="fca85-118">For example, when users view total sales for a region and month, they can view hello associated order details.</span></span> 
*  <span data-ttu-id="fca85-119">Säkerhet på objektnivå för tabell- och namn, dessutom toohello data i dem.</span><span class="sxs-lookup"><span data-stu-id="fca85-119">Object-level security for table and column names, in addition toohello data within them.</span></span>
*  <span data-ttu-id="fca85-120">Förbättrat stöd för ojämna hierarkier.</span><span class="sxs-lookup"><span data-stu-id="fca85-120">Enhanced support for ragged hierarchies.</span></span>
*  <span data-ttu-id="fca85-121">Prestanda och förbättringar i övervakning.</span><span class="sxs-lookup"><span data-stu-id="fca85-121">Performance and monitoring improvements.</span></span>
  
## <a name="set-compatibility-level"></a><span data-ttu-id="fca85-122">Ange kompatibilitetsnivån</span><span class="sxs-lookup"><span data-stu-id="fca85-122">Set compatibility level</span></span> 
 <span data-ttu-id="fca85-123">När du skapar ett nytt tabellmodell-projekt i SSDT, kan du ange hello kompatibilitetsnivån på hello **tabellmodell designer** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fca85-123">When creating a new tabular model project in SSDT, you can specify hello compatibility level on hello **Tabular model designer** dialog.</span></span> 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 <span data-ttu-id="fca85-125">Om du väljer hello **Visa inte detta meddelande igen** , alla efterföljande projekt använder hello kompatibilitetsnivå som du angav som hello standard.</span><span class="sxs-lookup"><span data-stu-id="fca85-125">If you select hello **Do not show this message again** option, all subsequent projects use hello compatibility level you specified as hello default.</span></span> <span data-ttu-id="fca85-126">Du kan ändra hello standard kompatibilitetsnivån i SSDT i **verktyg** > **alternativ**.</span><span class="sxs-lookup"><span data-stu-id="fca85-126">You can change hello default compatibility level in SSDT in **Tools** > **Options**.</span></span>  
  
 <span data-ttu-id="fca85-127">tooupgrade ett befintligt tabellmodell-projekt i SSDT set hello **kompatibilitetsnivå** egenskap i hello modellen **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="fca85-127">tooupgrade an existing tabular model project in SSDT, set  hello **Compatibility Level** property in hello model **Properties** window.</span></span> <span data-ttu-id="fca85-128">Håll i åtanke, uppgradera hello kompatibilitetsnivån går inte att ångra.</span><span class="sxs-lookup"><span data-stu-id="fca85-128">Keep in-mind, upgrading hello compatibility level is irreversible.</span></span>
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a><span data-ttu-id="fca85-129">Kontrollera kompatibilitetsnivån för en tabellmodelldatabas i SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="fca85-129">Check compatibility level for a tabular model database in SQL Server Management Studio</span></span> 
 <span data-ttu-id="fca85-130">I SSMS, högerklickar du på hello-databasnamn > **egenskaper** > **kompatibilitetsnivå**.</span><span class="sxs-lookup"><span data-stu-id="fca85-130">In SSMS, right-click hello database name > **Properties** > **Compatibility Level**.</span></span>  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a><span data-ttu-id="fca85-131">Kontrollera stöds kompatibilitetsnivån för en server i SSMS</span><span class="sxs-lookup"><span data-stu-id="fca85-131">Check supported compatibility level for a server in SSMS</span></span>  
 <span data-ttu-id="fca85-132">I SSMS, högerklickar du på hello-servernamn > **egenskaper** > **stöds kompatibilitetsnivå**.</span><span class="sxs-lookup"><span data-stu-id="fca85-132">In SSMS, right-click hello server name>  **Properties** > **Supported Compatibility Level**.</span></span>  
  
 <span data-ttu-id="fca85-133">Den här egenskapen anger hello högsta kompatibilitetsnivån för en databas som ska köras på hello-server (exklusive preview).</span><span class="sxs-lookup"><span data-stu-id="fca85-133">This property specifies hello highest compatibility level of a database that will run on hello server (excluding preview).</span></span> <span data-ttu-id="fca85-134">hello stöds kompatibilitetsnivån kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="fca85-134">hello supported compatibility level cannot be changed.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="fca85-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fca85-135">Next steps</span></span>
  <span data-ttu-id="fca85-136">[Skapa en modell i Azure-portalen](analysis-services-create-model-portal.md) </span><span class="sxs-lookup"><span data-stu-id="fca85-136">[Create a model in Azure portal](analysis-services-create-model-portal.md) </span></span>  
  [<span data-ttu-id="fca85-137">Hantera Analysis Services</span><span class="sxs-lookup"><span data-stu-id="fca85-137">Manage Analysis Services</span></span>](analysis-services-manage.md)  
