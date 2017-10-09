---
title: aaaAlerts verifiering i Azure Security Center | Microsoft Docs
description: "Det här dokumentet hjälper dig att toovalidate hello säkerhetsaviseringar i Azure Security Center."
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
ms.openlocfilehash: 030e9e74303758192eedaf517f1cb0d2e4a7852e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="alerts-validation-in-azure-security-center"></a><span data-ttu-id="ae8e6-103">Aviseringsverifiering i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="ae8e6-103">Alerts Validation in Azure Security Center</span></span>
<span data-ttu-id="ae8e6-104">Det här dokumentet hjälper dig att lära dig hur tooverify om systemet har konfigurerats korrekt för Azure Security Center-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-104">This document helps you learn how tooverify if your system is properly configured for Azure Security Center alerts.</span></span>

## <a name="what-are-security-alerts"></a><span data-ttu-id="ae8e6-105">Vad är säkerhetsaviseringar?</span><span class="sxs-lookup"><span data-stu-id="ae8e6-105">What are security alerts?</span></span>
<span data-ttu-id="ae8e6-106">Security Center automatiskt samlar in, analyseras och integreras loggdata från din Azure-resurser och hello nätverk anslutna partnerlösningar som brandväggen och endpoint protection-lösningar, toodetect och aviseringen du toothreats.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-106">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, hello network, and connected partner solutions, like firewall and endpoint protection solutions, toodetect and alert you toothreats.</span></span> <span data-ttu-id="ae8e6-107">Läs [hantering och svarar toosecurity aviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) för mer information om säkerhetsaviseringar och Läs [förstå säkerhetsaviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) toolearn mer Om hello olika typer av aviseringar.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-107">Read [Managing and responding toosecurity alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) for more information about security alerts, and read [Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) toolearn more about hello different types of alerts.</span></span>

## <a name="alert-validation"></a><span data-ttu-id="ae8e6-108">Aviseringsverifiering</span><span class="sxs-lookup"><span data-stu-id="ae8e6-108">Alert validation</span></span>
<span data-ttu-id="ae8e6-109">Security Center-agenten är installerad på datorn gör när hello nedan från hello dator där du vill att toobe hello angripna resursen hello avisering:</span><span class="sxs-lookup"><span data-stu-id="ae8e6-109">After Security Center agent is installed on your computer, follow hello steps below from hello computer where you want toobe hello attacked resource of hello alert:</span></span>

1. <span data-ttu-id="ae8e6-110">Kopiera ett körbart (till exempel calc.exe) toohello datorns skrivbord eller andra katalogen för din bekvämlighet.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-110">Copy an executable (for example calc.exe) toohello computer’s desktop, or other directory of your convenience.</span></span>
2. <span data-ttu-id="ae8e6-111">Byt namn på den här filen för**ASC_AlertTest_662jfi039N.exe**.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-111">Rename this file too**ASC_AlertTest_662jfi039N.exe**.</span></span>
3. <span data-ttu-id="ae8e6-112">Öppna hello-kommandotolk och kör den här filen med ett argument (bara en falsk argumentnamnet), exempelvis: *ASC_AlertTest_662jfi039N.exe - foo*</span><span class="sxs-lookup"><span data-stu-id="ae8e6-112">Open hello command prompt and execute this file with an argument (just a fake argument name), such as: *ASC_AlertTest_662jfi039N.exe -foo*</span></span>
4. <span data-ttu-id="ae8e6-113">Vänta 5 minuter för too10 och öppna Security Center-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-113">Wait 5 too10 minutes and open Security Center Alerts.</span></span> <span data-ttu-id="ae8e6-114">Det bör du hitta en avisering liknande toofollowing en:</span><span class="sxs-lookup"><span data-stu-id="ae8e6-114">There you should find an alert similar toofollowing one:</span></span>

    ![Aviseringsverifiering](./media/security-center-alert-validation/security-center-alert-validation-fig1.png)

<span data-ttu-id="ae8e6-116">När du granskar den här aviseringen, kontrollera hello fältet argument granskning aktiverats visas som true.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-116">When reviewing this alert, make sure hello field Arguments Auditing Enabled appears as true.</span></span> <span data-ttu-id="ae8e6-117">Om det är false, måste tooenable kommandoradsargument granskning.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-117">If it shows false, you need tooenable command-line arguments auditing.</span></span> <span data-ttu-id="ae8e6-118">Du kan aktivera det här alternativet använder hello följande kommandorad:</span><span class="sxs-lookup"><span data-stu-id="ae8e6-118">You can enable this option using hello following command line:</span></span>

<span data-ttu-id="ae8e6-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span><span class="sxs-lookup"><span data-stu-id="ae8e6-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span></span>


## <a name="see-also"></a><span data-ttu-id="ae8e6-120">Se även</span><span class="sxs-lookup"><span data-stu-id="ae8e6-120">See also</span></span>
<span data-ttu-id="ae8e6-121">Den här artikeln introduceras toohello aviseringar verifieringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-121">This article introduced you toohello alerts validation process.</span></span> <span data-ttu-id="ae8e6-122">Nu när du är bekant med den här verifieringen försök hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="ae8e6-122">Now that you're familiar with this validation, try hello following articles:</span></span>

* <span data-ttu-id="ae8e6-123">[Hantera och svarar toosecurity aviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span><span class="sxs-lookup"><span data-stu-id="ae8e6-123">[Managing and responding toosecurity alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span></span> <span data-ttu-id="ae8e6-124">Lär dig hur toomanage aviseringar och svara toosecurity incidenter i Security Center.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-124">Learn how toomanage alerts, and respond toosecurity incidents in Security Center.</span></span>
* <span data-ttu-id="ae8e6-125">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="ae8e6-125">[Security health monitoring in Azure Security Center](security-center-monitoring.md).</span></span> <span data-ttu-id="ae8e6-126">Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-126">Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="ae8e6-127">[Förstå säkerhetsaviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span><span class="sxs-lookup"><span data-stu-id="ae8e6-127">[Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span></span> <span data-ttu-id="ae8e6-128">Läs mer om hello olika typer av säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-128">Learn about hello different types of security alerts.</span></span>
* <span data-ttu-id="ae8e6-129">[Felsökningsguide för Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span><span class="sxs-lookup"><span data-stu-id="ae8e6-129">[Azure Security Center Troubleshooting Guide](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span></span> <span data-ttu-id="ae8e6-130">Lär dig hur tootroubleshoot vanliga problem i Security Center.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-130">Learn how tootroubleshoot common issues in Security Center.</span></span> 
* <span data-ttu-id="ae8e6-131">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md).</span><span class="sxs-lookup"><span data-stu-id="ae8e6-131">[Azure Security Center FAQ](security-center-faq.md).</span></span> <span data-ttu-id="ae8e6-132">Sök efter vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-132">Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="ae8e6-133">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/).</span><span class="sxs-lookup"><span data-stu-id="ae8e6-133">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="ae8e6-134">Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.</span><span class="sxs-lookup"><span data-stu-id="ae8e6-134">Find blog posts about Azure security and compliance.</span></span>

