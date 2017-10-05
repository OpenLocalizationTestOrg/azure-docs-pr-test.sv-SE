---
title: Aviseringsverifiering i Azure Security Center | Microsoft Docs
description: "I det här dokumentet får du hjälp med att verifiera säkerhetsaviseringar i Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.openlocfilehash: 121b5d8f023a9b663d0e7af26dce8f81db27672c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="alerts-validation-in-azure-security-center"></a><span data-ttu-id="13e8a-103">Aviseringsverifiering i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="13e8a-103">Alerts Validation in Azure Security Center</span></span>
<span data-ttu-id="13e8a-104">I det här dokumentet får du hjälp med att verifiera systemet är rätt konfigurerat för aviseringar från Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="13e8a-104">This document helps you learn how to verify if your system is properly configured for Azure Security Center alerts.</span></span>

## <a name="what-are-security-alerts"></a><span data-ttu-id="13e8a-105">Vad är säkerhetsaviseringar?</span><span class="sxs-lookup"><span data-stu-id="13e8a-105">What are security alerts?</span></span>
<span data-ttu-id="13e8a-106">Security Center samlar automatiskt in, analyserar och integrerar loggdata från Azure-resurser, nätverket och anslutna partnerlösningar som brandväggs- och slutpunktsskyddslösningar för att identifiera och uppmärksamma dig på hot.</span><span class="sxs-lookup"><span data-stu-id="13e8a-106">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, the network, and connected partner solutions, like firewall and endpoint protection solutions, to detect and alert you to threats.</span></span> <span data-ttu-id="13e8a-107">I [Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) kan du läsa mer om säkerhetsaviseringar, och i [Förstå säkerhetsaviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) får du veta mer om olika typer av aviseringar.</span><span class="sxs-lookup"><span data-stu-id="13e8a-107">Read [Managing and responding to security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) for more information about security alerts, and read [Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) to learn more about the different types of alerts.</span></span>

## <a name="alert-validation"></a><span data-ttu-id="13e8a-108">Aviseringsverifiering</span><span class="sxs-lookup"><span data-stu-id="13e8a-108">Alert validation</span></span>
<span data-ttu-id="13e8a-109">När Security Center-agenten är installerad på datorn följer du stegen nedan från den dator där du vill ha den angripna resursen som aviseras:</span><span class="sxs-lookup"><span data-stu-id="13e8a-109">After Security Center agent is installed on your computer, follow the steps below from the computer where you want to be the attacked resource of the alert:</span></span>

1. <span data-ttu-id="13e8a-110">Kopiera en körbar fil (till exempel calc.exe) till datorns skrivbord eller en annan katalog som är lätt att hitta.</span><span class="sxs-lookup"><span data-stu-id="13e8a-110">Copy an executable (for example calc.exe) to the computer’s desktop, or other directory of your convenience.</span></span>
2. <span data-ttu-id="13e8a-111">Byt namn på filen till **ASC_AlertTest_662jfi039N.exe**.</span><span class="sxs-lookup"><span data-stu-id="13e8a-111">Rename this file to **ASC_AlertTest_662jfi039N.exe**.</span></span>
3. <span data-ttu-id="13e8a-112">Öppna kommandotolken och kör den här filen med ett argument (bara ett falskt argumentnamnet), exempelvis: *ASC_AlertTest_662jfi039N.exe -foo*</span><span class="sxs-lookup"><span data-stu-id="13e8a-112">Open the command prompt and execute this file with an argument (just a fake argument name), such as: *ASC_AlertTest_662jfi039N.exe -foo*</span></span>
4. <span data-ttu-id="13e8a-113">Vänta 5–10 minuter och öppna sedan Security Center-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="13e8a-113">Wait 5 to 10 minutes and open Security Center Alerts.</span></span> <span data-ttu-id="13e8a-114">Där bör du hitta en avisering liknande denna:</span><span class="sxs-lookup"><span data-stu-id="13e8a-114">There you should find an alert similar to following one:</span></span>

    ![Aviseringsverifiering](./media/security-center-alert-validation/security-center-alert-validation-fig1.png)

<span data-ttu-id="13e8a-116">När du granskar den här aviseringen ska du se till att fältet Argument Auditing Enabled (argumentgranskning aktiverat) visas som true (sant).</span><span class="sxs-lookup"><span data-stu-id="13e8a-116">When reviewing this alert, make sure the field Arguments Auditing Enabled appears as true.</span></span> <span data-ttu-id="13e8a-117">Om det visas som false (falskt) måste du aktivera granskning av kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="13e8a-117">If it shows false, you need to enable command-line arguments auditing.</span></span> <span data-ttu-id="13e8a-118">Du kan aktivera det här alternativet med följande kommandorad:</span><span class="sxs-lookup"><span data-stu-id="13e8a-118">You can enable this option using the following command line:</span></span>

<span data-ttu-id="13e8a-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span><span class="sxs-lookup"><span data-stu-id="13e8a-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span></span>


## <a name="see-also"></a><span data-ttu-id="13e8a-120">Se även</span><span class="sxs-lookup"><span data-stu-id="13e8a-120">See also</span></span>
<span data-ttu-id="13e8a-121">I den här artikeln förklaras processen för aviseringsverifiering.</span><span class="sxs-lookup"><span data-stu-id="13e8a-121">This article introduced you to the alerts validation process.</span></span> <span data-ttu-id="13e8a-122">Nu när du är bekant med den här verifieringen kan du titta på följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="13e8a-122">Now that you're familiar with this validation, try the following articles:</span></span>

* <span data-ttu-id="13e8a-123">[Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span><span class="sxs-lookup"><span data-stu-id="13e8a-123">[Managing and responding to security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span></span> <span data-ttu-id="13e8a-124">Lär dig hur du hanterar aviseringar och åtgärdar säkerhetsincidenter i Security Center.</span><span class="sxs-lookup"><span data-stu-id="13e8a-124">Learn how to manage alerts, and respond to security incidents in Security Center.</span></span>
* <span data-ttu-id="13e8a-125">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="13e8a-125">[Security health monitoring in Azure Security Center](security-center-monitoring.md).</span></span> <span data-ttu-id="13e8a-126">Lär dig att övervaka hälsotillståndet för dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="13e8a-126">Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="13e8a-127">[Förstå säkerhetsaviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span><span class="sxs-lookup"><span data-stu-id="13e8a-127">[Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span></span> <span data-ttu-id="13e8a-128">Läs mer om de olika typerna av säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="13e8a-128">Learn about the different types of security alerts.</span></span>
* <span data-ttu-id="13e8a-129">[Felsökningsguide för Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span><span class="sxs-lookup"><span data-stu-id="13e8a-129">[Azure Security Center Troubleshooting Guide](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span></span> <span data-ttu-id="13e8a-130">Lär dig hur du felsöker vanliga problem i Security Center.</span><span class="sxs-lookup"><span data-stu-id="13e8a-130">Learn how to troubleshoot common issues in Security Center.</span></span> 
* <span data-ttu-id="13e8a-131">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md).</span><span class="sxs-lookup"><span data-stu-id="13e8a-131">[Azure Security Center FAQ](security-center-faq.md).</span></span> <span data-ttu-id="13e8a-132">Här finns vanliga frågor om att använda tjänsten.</span><span class="sxs-lookup"><span data-stu-id="13e8a-132">Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="13e8a-133">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/).</span><span class="sxs-lookup"><span data-stu-id="13e8a-133">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="13e8a-134">Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.</span><span class="sxs-lookup"><span data-stu-id="13e8a-134">Find blog posts about Azure security and compliance.</span></span>

