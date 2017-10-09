---
title: "aaaOperations Management Suite säkerhets- och granska lösningen Web baslinjen | Microsoft Docs"
description: "Det här dokumentet förklarar hur toouse OMS säkerhet och granska lösningen tooperform en web baslinjen bedömning av alla övervakade webbservrar för efterlevnad och säkerhet ändamål."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ROBOTS: NOINDEX
redirect_url: https://www.microsoft.com/cloud-platform/security-and-compliance
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: 8aa87fa404ff97ab549dda3f9bebb75766055963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a><span data-ttu-id="b3b87-103">Utvärdering av säkerhetswebbaslinjen i säkerhets- och granskningslösningen i Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="b3b87-103">Web Baseline Assessment in Operations Management Suite Security and Audit Solution</span></span>
<span data-ttu-id="b3b87-104">Det här dokumentet hjälper dig att toouse [Operations Management Suite (OMS) säkerhets- och granska lösningen](operations-management-suite-overview.md) web baslinjen assessment funktioner tooaccess hello säkerhetsstatus för dina övervakade resurser.</span><span class="sxs-lookup"><span data-stu-id="b3b87-104">This document helps you toouse [Operations Management Suite (OMS) Security and Audit Solution](operations-management-suite-overview.md) web baseline assessment capabilities tooaccess hello secure state of your monitored resources.</span></span>

## <a name="what-is-web-baseline-assessment"></a><span data-ttu-id="b3b87-105">Vad är utvärdering av webbaslinje?</span><span class="sxs-lookup"><span data-stu-id="b3b87-105">What is Web Baseline Assessment?</span></span>
<span data-ttu-id="b3b87-106">För närvarande tillhandahåller säkerheten i OMS utvärdering av säkerhetsbaslinje för operativsystem.</span><span class="sxs-lookup"><span data-stu-id="b3b87-106">Currently OMS Security provides security baseline assessment for operating systems.</span></span> <span data-ttu-id="b3b87-107">Söks igenom hello operativsysteminställningar servrar per dygn och ger en överblick över potentiellt känsliga inställningar.</span><span class="sxs-lookup"><span data-stu-id="b3b87-107">It scans hello operating system settings of your servers every 24 hours and provides a view into potentially vulnerable settings.</span></span> <span data-ttu-id="b3b87-108">Läs [bedömning av baslinjen i Operations Management Suite säkerhet och granska lösningen](oms-security-baseline.md) för mer information om det här.</span><span class="sxs-lookup"><span data-stu-id="b3b87-108">Read [Baseline Assessment in Operations Management Suite Security and Audit Solution](oms-security-baseline.md) for more information on this.</span></span>

<span data-ttu-id="b3b87-109">hello målet med hello web baslinjen assessment är toofind sårbara web server-inställningar.</span><span class="sxs-lookup"><span data-stu-id="b3b87-109">hello goal of hello web baseline assessment is toofind potentially vulnerable web server settings.</span></span> <span data-ttu-id="b3b87-110">Hej tre primära källor för hello web grundläggande konfigurationer är: .NET, ASP.NET och IIS-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b3b87-110">hello three primary sources for hello web baseline configurations are: .NET, ASP.NET, and IIS configuration.</span></span>  <span data-ttu-id="b3b87-111">Precis som hello fungerar system baslinjen assessment OMS säkerhet kommer tooscan din webbservrar varje 24hrs och ger en överblick över säkerhetsläget för dem.</span><span class="sxs-lookup"><span data-stu-id="b3b87-111">Just like hello operating system baseline assessment, OMS Security is going tooscan your web servers every 24hrs and provide a view into security state of them.</span></span>  <span data-ttu-id="b3b87-112">I Internet Information Service (IIS), är konfigurationer mycket anpassningsbara, vilket gör att olika webbplats- och nivåer toobe-åsidosätts.</span><span class="sxs-lookup"><span data-stu-id="b3b87-112">In Internet Information Service (IIS), configurations are highly customizable, which allows various site and application levels toobe overridden.</span></span> <span data-ttu-id="b3b87-113">hello skanner kontrollerar hello inställningar på varje nivå för program/på platsen i tillägg toohello Standardnivå rot.</span><span class="sxs-lookup"><span data-stu-id="b3b87-113">hello scanner checks hello settings at each application/site level in addition toohello default root level.</span></span> <span data-ttu-id="b3b87-114">Detta hjälper dig att tooidentify potentiella säkerhetsproblem inställningar platser och reparera snabbt.</span><span class="sxs-lookup"><span data-stu-id="b3b87-114">This helps you tooidentify potential vulnerability settings locations and quickly remediate.</span></span>


## <a name="web-security-baseline-assessment"></a><span data-ttu-id="b3b87-115">Utvärdering av säkerhetswebb-baslinje</span><span class="sxs-lookup"><span data-stu-id="b3b87-115">Web Security Baseline Assessment</span></span>
<span data-ttu-id="b3b87-116">För den här förhandsgranskningen kommer den här funktionen toobe som nås med hello OMS sökalternativ.</span><span class="sxs-lookup"><span data-stu-id="b3b87-116">For this preview this feature is going toobe accessed using hello OMS Search option.</span></span> <span data-ttu-id="b3b87-117">Så hello nedan tooperform hello används fråga:</span><span class="sxs-lookup"><span data-stu-id="b3b87-117">Follow hello steps below tooperform hello appropriated query:</span></span>

1. <span data-ttu-id="b3b87-118">I hello **Microsoft Operations Management Suite** huvudsakliga instrumentpanelen klickar du på **säkerhet och granska** panelen.</span><span class="sxs-lookup"><span data-stu-id="b3b87-118">In hello **Microsoft Operations Management Suite** main dashboard click **Security and Audit** tile.</span></span>
2. <span data-ttu-id="b3b87-119">I hello **säkerhet och granska** instrumentpanelen, klickar du på **loggen Sök** knappen.</span><span class="sxs-lookup"><span data-stu-id="b3b87-119">In hello **Security and Audit** dashboard, click **Log Search** button.</span></span>
3. <span data-ttu-id="b3b87-120">hello första frågan som du kan använda är hello **Web baslinjen Assessment sammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="b3b87-120">hello first query that you can use is hello **Web Baseline Assessment Summary**.</span></span> <span data-ttu-id="b3b87-121">I hello **Begin Sök här** anger den här frågan: typen*= SecurityBaselineSummary BaselineType = web*.</span><span class="sxs-lookup"><span data-stu-id="b3b87-121">In hello **Begin search here** field, type this query: Type*=SecurityBaselineSummary BaselineType=web*.</span></span> <span data-ttu-id="b3b87-122">hello efter skärmen har ett exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="b3b87-122">hello following screen has an output sample:</span></span>

![Sammanfattning av utvärdering av webbaslinje](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> <span data-ttu-id="b3b87-124">I den här fråga indikerar varje post en utvärderingssammanfattning på en enskild server.</span><span class="sxs-lookup"><span data-stu-id="b3b87-124">In this query, each record indicates assessment summary on a single server.</span></span>

<span data-ttu-id="b3b87-125">När du är i hello **loggen Sök**, du kan skriva olika frågor tooobtain mer information om hello web baslinjen assessment.</span><span class="sxs-lookup"><span data-stu-id="b3b87-125">Once you are in hello **Log Search**, you can type different queries tooobtain more information about hello web baseline assessment.</span></span> <span data-ttu-id="b3b87-126">Dessutom toohello föregående fråga, du kan också använda hello följande i den här förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="b3b87-126">In addition toohello previous query, you can also use hello following ones in this preview.</span></span>

<span data-ttu-id="b3b87-127">**Utvärdering av webbaslinje**: Varje post motsvarar en utvärdering av en webb-baslinjeregel på en enskild server.</span><span class="sxs-lookup"><span data-stu-id="b3b87-127">**Web Baseline Rule Assessment**: Each record represents a single web baseline rule evaluation on a single server.</span></span> <span data-ttu-id="b3b87-128">Den omfattar alla data för hello regeln, plats, hello förväntat resultat och hello faktiska resultat.</span><span class="sxs-lookup"><span data-stu-id="b3b87-128">It includes all data for hello rule, location, hello expected result, and hello actual result.</span></span>

<span data-ttu-id="b3b87-129">**Fråga**: Type*=SecurityBaseline BaselineType=web*</span><span class="sxs-lookup"><span data-stu-id="b3b87-129">**Query**: Type*=SecurityBaseline BaselineType=web*</span></span>

![Utvärdering av webbaslinje](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

<span data-ttu-id="b3b87-131">**Visa alla resultat för en specifik server**: frågan visar hur toosee resulterar i en specifik server.</span><span class="sxs-lookup"><span data-stu-id="b3b87-131">**Show all results for a specific server**: This query shows how toosee results of a specific server.</span></span>

<span data-ttu-id="b3b87-132">**Fråga**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*</span><span class="sxs-lookup"><span data-stu-id="b3b87-132">**Query**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*</span></span>

![Alla resultat](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

<span data-ttu-id="b3b87-134">Du kan också använda dessa poster/frågor toocreate egna instrumentpaneler, rapporter och aviseringar.</span><span class="sxs-lookup"><span data-stu-id="b3b87-134">You can also use these records/queries toocreate your own dashboards, reports, or alerts.</span></span> <span data-ttu-id="b3b87-135">hello-skärmen nedan har en exempel UI-kontroll som du lägger till tooyour instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b3b87-135">hello screen below has a sample UI control that you can add tooyour dashboard.</span></span> <span data-ttu-id="b3b87-136">Du kan lära dig hur toovisualize dina data med hjälp av OMS Vydesigner [här](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/).</span><span class="sxs-lookup"><span data-stu-id="b3b87-136">You can learn how toovisualize your data using OMS View Designer [here](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/).</span></span> <span data-ttu-id="b3b87-137">hello-skärmen nedan är ett exempel på hur hello panelen ser ut när du har gjort den här anpassningen.</span><span class="sxs-lookup"><span data-stu-id="b3b87-137">hello screen below is an example of how hello tile will look like once you make this customization.</span></span>

![Exempel-UI](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> <span data-ttu-id="b3b87-139">Om du vill ha tooknow hello inställningar som är markerade för hello baslinje, kan du hämta [Excel-kalkylblad](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) som innehåller dessa inställningar.</span><span class="sxs-lookup"><span data-stu-id="b3b87-139">If you would like tooknow hello settings that are checked for hello baseline assessment, you can download [this Excel spreadsheet](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) that contains these settings.</span></span>

## <a name="see-also"></a><span data-ttu-id="b3b87-140">Se även</span><span class="sxs-lookup"><span data-stu-id="b3b87-140">See also</span></span>
<span data-ttu-id="b3b87-141">I det här dokumentet har du lärt dig om Utvärdering av säkerhetswebbaslinje i säkerhets- och granskningslösningen i OMS.</span><span class="sxs-lookup"><span data-stu-id="b3b87-141">In this document, you learned about OMS Security and Audit web baseline assessment.</span></span> <span data-ttu-id="b3b87-142">toolearn mer om OMS-säkerhet finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="b3b87-142">toolearn more about OMS Security, see hello following articles:</span></span>

* [<span data-ttu-id="b3b87-143">Översikt över Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="b3b87-143">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="b3b87-144">Övervakning och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösning</span><span class="sxs-lookup"><span data-stu-id="b3b87-144">Monitoring and Responding tooSecurity Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="b3b87-145">Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="b3b87-145">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

