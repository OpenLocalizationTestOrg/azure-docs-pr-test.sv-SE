---
title: "aaaMonitoring och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösningen | Microsoft Docs"
description: "Det här dokumentet hjälper du toouse hello hot intelligence alternativ som finns i OMS-säkerhet och granska toomonitor och svara toosecurity aviseringar."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 7d45a32b-1341-4bb5-a436-1f42a8a2590a
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2017
ms.author: yurid
ms.openlocfilehash: 3d92b6809b7bd934c889afc119e5e34ff2b85f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-and-responding-toosecurity-alerts-in-operations-management-suite-security-and-audit-solution"></a><span data-ttu-id="0ef44-103">Övervaka och svara toosecurity varningar i Operations Management Suite säkerhet och granska lösningen</span><span class="sxs-lookup"><span data-stu-id="0ef44-103">Monitoring and responding toosecurity alerts in Operations Management Suite Security and Audit Solution</span></span>
<span data-ttu-id="0ef44-104">Det här dokumentet hjälper dig att använda hello hot intelligence alternativ som finns i OMS-säkerhet och granska toomonitor och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="0ef44-104">This document helps you use hello threat intelligence option available in OMS Security and Audit toomonitor and respond toosecurity alerts.</span></span>

## <a name="what-is-oms"></a><span data-ttu-id="0ef44-105">Vad är OMS?</span><span class="sxs-lookup"><span data-stu-id="0ef44-105">What is OMS?</span></span>
<span data-ttu-id="0ef44-106">Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="0ef44-106">Microsoft Operations Management Suite (OMS) is Microsoft's cloud based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="0ef44-107">Mer information om OMS artikeln hello [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ef44-107">For more information about OMS, read hello article [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).</span></span>

## <a name="threat-intelligence"></a><span data-ttu-id="0ef44-108">Hotinformation</span><span class="sxs-lookup"><span data-stu-id="0ef44-108">Threat intelligence</span></span>
<span data-ttu-id="0ef44-109">I en företagsmiljö där användarna har bred åtkomst toohello nätverk och använda en mängd olika enheter tooconnect toocorporate data, är det viktigt att du kan övervaka dina resurser aktivt och snabbt svara toosecurity incidenter.</span><span class="sxs-lookup"><span data-stu-id="0ef44-109">In an enterprise environment where users have broad access toohello network and use a variety of devices tooconnect toocorporate data, it is imperative that you can actively monitor your resources and quickly respond toosecurity incidents.</span></span> <span data-ttu-id="0ef44-110">Detta är särskilt viktigt från hello säkerhetsperspektiv livscykel eftersom vissa cybersecurity hot inte kan generera aviseringar eller misstänkta aktiviteter som kan identifieras av traditionella tekniska säkerhetsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="0ef44-110">This is particular important from hello security lifecycle perspective because some cybersecurity threats may not raise alerts or suspicious activities that can be identified by traditional security technical controls.</span></span> 

<span data-ttu-id="0ef44-111">Med hjälp av hello **Hotinformation** alternativet finns i OMS-säkerhet och granskning, IT-administratörer kan identifiera säkerhetshot mot hello-miljön, till exempel, identifiera om en viss dator är en del av en [ botnät](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection).</span><span class="sxs-lookup"><span data-stu-id="0ef44-111">By using hello **Threat Intelligence** option available in OMS Security and Audit, IT administrators can identify security threats against hello environment, for example, identify if a particular computer is part of a [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection).</span></span> <span data-ttu-id="0ef44-112">Datorer kan bli noder i ett botnät när angripare otillåtet sätt installera skadlig kod som någon gång ansluter den här datorn toohello kommando och kontroll.</span><span class="sxs-lookup"><span data-stu-id="0ef44-112">Computers can become nodes in a botnet when attackers illicitly install malware that secretly connects this computer toohello command and control.</span></span> <span data-ttu-id="0ef44-113">Det kan även identifiera potentiella hot som kommer från lagringsutrymmen kommunikationskanaler som [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents).</span><span class="sxs-lookup"><span data-stu-id="0ef44-113">It can also identify potential threats coming from underground communication channels, such as [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents).</span></span> 

<span data-ttu-id="0ef44-114">I ordning toobuild använder den här hotinformation OMS säkerhet och granska data som kommer från olika källor i Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0ef44-114">In order toobuild this threat intelligence, OMS Security and Audit use data coming from multiple sources within Microsoft.</span></span> <span data-ttu-id="0ef44-115">OMS säkerhet och granska använder den här data tooidentify potentiella hot mot din miljö.</span><span class="sxs-lookup"><span data-stu-id="0ef44-115">OMS Security and Audit will leverage this data tooidentify potential threats against your environment.</span></span>

<span data-ttu-id="0ef44-116">Hej Hotinformation rutan består av tre huvudsakliga alternativ:</span><span class="sxs-lookup"><span data-stu-id="0ef44-116">hello Threat Intelligence pane is composed by three major options:</span></span>

* <span data-ttu-id="0ef44-117">Servrar med utgående skadlig trafik</span><span class="sxs-lookup"><span data-stu-id="0ef44-117">Servers with outbound malicious traffic</span></span>
* <span data-ttu-id="0ef44-118">Typer av identifierade hot</span><span class="sxs-lookup"><span data-stu-id="0ef44-118">Detected threats types</span></span>
* <span data-ttu-id="0ef44-119">Karta för hotinformation</span><span class="sxs-lookup"><span data-stu-id="0ef44-119">Threat intelligence map</span></span>

> [!NOTE]
> <span data-ttu-id="0ef44-120">en översikt över alla dessa alternativ, läsa [komma igång med Operations Management Suite säkerhet och granska lösningen](oms-security-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="0ef44-120">for an overview of all these options, read [Getting started with Operations Management Suite Security and Audit Solution](oms-security-getting-started.md).</span></span>
> 
> 

### <a name="responding-toosecurity-alerts"></a><span data-ttu-id="0ef44-121">Svara toosecurity aviseringar</span><span class="sxs-lookup"><span data-stu-id="0ef44-121">Responding toosecurity alerts</span></span>
<span data-ttu-id="0ef44-122">En av hello stegen i en [säkerhet incidenter](https://technet.microsoft.com/library/cc512623.aspx) processen är tooidentify hello allvarlighetsgraden hello röjande system.</span><span class="sxs-lookup"><span data-stu-id="0ef44-122">One of hello steps of a [security incident response](https://technet.microsoft.com/library/cc512623.aspx) process is tooidentify hello severity of hello compromise system(s).</span></span> <span data-ttu-id="0ef44-123">I det här steget ska du utföra hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="0ef44-123">In this phase you should perform hello following tasks:</span></span>

* <span data-ttu-id="0ef44-124">Fastställa hello uppbyggnad hello-attack</span><span class="sxs-lookup"><span data-stu-id="0ef44-124">Determine hello nature of hello attack</span></span>
* <span data-ttu-id="0ef44-125">Fastställa hello angreppspunkt för ursprung</span><span class="sxs-lookup"><span data-stu-id="0ef44-125">Determine hello attack point of origin</span></span>
* <span data-ttu-id="0ef44-126">Fastställa hello syftet med hello-attack.</span><span class="sxs-lookup"><span data-stu-id="0ef44-126">Determine hello intent of hello attack.</span></span> <span data-ttu-id="0ef44-127">Var hello attack specifikt riktat mot din organisation tooacquire information eller var den slumpmässiga?</span><span class="sxs-lookup"><span data-stu-id="0ef44-127">Was hello attack specifically directed at your organization tooacquire specific information, or was it random?</span></span>
* <span data-ttu-id="0ef44-128">Identifiera hello-datorer som har komprometterats</span><span class="sxs-lookup"><span data-stu-id="0ef44-128">Identify hello systems that have been compromised</span></span>
* <span data-ttu-id="0ef44-129">Hello-filer som har öppnats och fastställa hello känslighet av dessa filer</span><span class="sxs-lookup"><span data-stu-id="0ef44-129">Identify hello files that have been accessed and determine hello sensitivity of those files</span></span>

<span data-ttu-id="0ef44-130">Du kan utnyttja **Hotinformation** information i OMS-säkerhet och granska lösningen toohelp med dessa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="0ef44-130">You can leverage **Threat Intelligence** information in OMS Security and Audit solution toohelp with these tasks.</span></span> <span data-ttu-id="0ef44-131">Gör hello nedan tooaccess detta **Hotinformation** alternativ:</span><span class="sxs-lookup"><span data-stu-id="0ef44-131">Follow hello steps below tooaccess this **Threat Intelligence** options:</span></span>

1. <span data-ttu-id="0ef44-132">I hello **Microsoft Operations Management Suite** huvudsakliga instrumentpanelen klickar du på **säkerhet och granska** panelen.</span><span class="sxs-lookup"><span data-stu-id="0ef44-132">In hello **Microsoft Operations Management Suite** main dashboard click **Security and Audit** tile.</span></span>
   
    ![Säkerhet och granskning](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. <span data-ttu-id="0ef44-134">I hello **säkerhet och granska** instrumentpanelen visas hello **Hotinformation** alternativ i hello höger, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="0ef44-134">In hello **Security and Audit** dashboard, you will see hello **Threat Intelligence** options in hello right, as shown below:</span></span>
   
    ![Hot intel](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

<span data-ttu-id="0ef44-136">Dessa tre paneler ger en översikt över hello aktuella hot.</span><span class="sxs-lookup"><span data-stu-id="0ef44-136">These three tiles will give you an overview of hello current threats.</span></span> <span data-ttu-id="0ef44-137">I hello **Server med utgående skadlig trafik** du kommer att kunna tooidentify om det är en dator som du övervakar (inom eller utanför ditt nätverk) som skickar skadlig trafik toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="0ef44-137">In hello **Server with outbound malicious traffic** you will be able tooidentify if there is any computer that you are monitoring (inside or outside of your network) that is sending malicious traffic toohello Internet.</span></span> 

<span data-ttu-id="0ef44-138">Hej **upptäckte hot typer** panelen visas en sammanfattning av hello hot som är aktuella ”i vilda hello”, om du klickar på den här panelen visas mer information om dessa hot som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="0ef44-138">hello **Detected threat types** tile shows a summary of hello threats that are current “in hello wild”, if you click on this tile you will see more details about these threats as show below:</span></span>

![Identifierade hottyper](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

<span data-ttu-id="0ef44-140">Du kan extrahera mer information om varje hot genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="0ef44-140">You can extract more information about each threat by clicking on it.</span></span> <span data-ttu-id="0ef44-141">hello exemplet nedan visar mer information om Botnät:</span><span class="sxs-lookup"><span data-stu-id="0ef44-141">hello example below shows more details about Botnet:</span></span>

![Mer information om ett hot](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

<span data-ttu-id="0ef44-143">Enligt hello början av det här avsnittet kan kan den här informationen vara väldigt användbar vid ett ärende för incidenter.</span><span class="sxs-lookup"><span data-stu-id="0ef44-143">As described in hello beginning of this section, this information can be very useful during an incident response case.</span></span> <span data-ttu-id="0ef44-144">Det kan också vara viktigt vid kriminaltekniska undersökningar, där du behöver toofind hello källan för hello angrepp, vilket system som har drabbats och hello tidslinje.</span><span class="sxs-lookup"><span data-stu-id="0ef44-144">It can also be important during a forensic investigation, where you need toofind hello source of hello attack, which system was compromised and hello timeline.</span></span> <span data-ttu-id="0ef44-145">I den här rapporten som du lätt kan identifiera vissa viktiga uppgifter om hello angrepp, t.ex: hello källan för hello attack, hello lokala IP-adress som har drabbats och hello aktuella sessionstillstånd hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="0ef44-145">In this report you can easily identify some key details about hello attack, such as: hello source of hello attack, hello local IP that was compromised and hello current session state of hello connection.</span></span> 

<span data-ttu-id="0ef44-146">Hej **hot intelligence kartan** hjälper dig att tooidentify hello aktuell platser runt hello världen som har skadlig trafik.</span><span class="sxs-lookup"><span data-stu-id="0ef44-146">hello **threat intelligence map** will help you tooidentify hello current locations around hello globe that have malicious traffic.</span></span> <span data-ttu-id="0ef44-147">Det finns orange (inkommande) och röda (utgående) pilar i den här kartan som identifierar hello riktning på nätverkstrafik, om du klickar på någon av dessa pilar, visas hello typ av hot och hello riktning på nätverkstrafik som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="0ef44-147">There are orange (incoming) and red (outgoing) arrows in this map that identify hello traffic direction, if you click in one of these arrows, it will show hello type of threat and hello traffic direction as shown below:</span></span>

![hotinformationkarta](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [!NOTE]
> <span data-ttu-id="0ef44-149">Du kan se en demonstration på hur toouse funktionen under en incidenter bearbeta genom att titta på hello presentation [minimera datacenter säkerhetshot interaktiv undersökningen med hjälp av Operations Management Suite](https://myignite.microsoft.com/videos/5000) leverera på Microsoft Ignite.</span><span class="sxs-lookup"><span data-stu-id="0ef44-149">You can see a demonstration on how toouse this capability during an incident response process by watching hello presentation [Mitigate datacenter security threats with guided investigation using Operations Management Suite](https://myignite.microsoft.com/videos/5000) delivered at Microsoft Ignite.</span></span>
> 

### <a name="responding-toodistinct-malicious-ip-accessed"></a><span data-ttu-id="0ef44-150">Svara toodistinct skadliga IP nås</span><span class="sxs-lookup"><span data-stu-id="0ef44-150">Responding toodistinct malicious IP accessed</span></span>
<span data-ttu-id="0ef44-151">I vissa situationer kan finnas det en potentiellt skadlig IP-adress som öppnas från en övervakad dator:</span><span class="sxs-lookup"><span data-stu-id="0ef44-151">In some scenarios, you may notice a potential malicious IP that was accessed from one monitored computer:</span></span>

![hotinformationkarta](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.png)

<span data-ttu-id="0ef44-153">Den här aviseringen och andra inom Hej samma kategori, genereras via OMS säkerhet genom att utnyttja [Microsoft Hotinformation](https://youtu.be/O4WtxgUrDc8).</span><span class="sxs-lookup"><span data-stu-id="0ef44-153">This alert and others within hello same category, are generated via OMS Security by leveraging [Microsoft Threat Intelligence](https://youtu.be/O4WtxgUrDc8).</span></span> <span data-ttu-id="0ef44-154">Hej Hotinformation data är samlas in av Microsoft som köpts från inledande hot intelligence providers.</span><span class="sxs-lookup"><span data-stu-id="0ef44-154">hello Threat Intelligence data is collected by Microsoft as well as purchased from leading threat intelligence providers.</span></span> <span data-ttu-id="0ef44-155">Dessa data uppdateras ofta och anpassade toofast flytta hot.</span><span class="sxs-lookup"><span data-stu-id="0ef44-155">This data is updated frequently and adapted toofast-moving threats.</span></span> <span data-ttu-id="0ef44-156">På grund av tooits karaktär, bör det kombineras med andra informationskällor säkerhet vid [undersöker](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) en säkerhetsavisering.</span><span class="sxs-lookup"><span data-stu-id="0ef44-156">Due tooits nature, it should be combined with other sources of security information while [investigating](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) a security alert.</span></span> 

## <a name="customize-alerts-received-via-e-mail"></a><span data-ttu-id="0ef44-157">Anpassa aviseringar via e-post</span><span class="sxs-lookup"><span data-stu-id="0ef44-157">Customize alerts received via e-mail</span></span>

<span data-ttu-id="0ef44-158">Du kan anpassa vilka användare i din organisation ska meddelas när säkerhetsvarningar utlöses av OMS-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="0ef44-158">You can customize which users in your organization will be notified when security alerts are triggered by OMS Security.</span></span> <span data-ttu-id="0ef44-159">Det här alternativet är tillgängligt under översikt / inställningar på hello OMS instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="0ef44-159">This option is available under Overview / Settings at hello OMS dashboard:</span></span>

![E-post](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig7.png)

## <a name="see-also"></a><span data-ttu-id="0ef44-161">Se även</span><span class="sxs-lookup"><span data-stu-id="0ef44-161">See also</span></span>
<span data-ttu-id="0ef44-162">I det här dokumentet du lärt dig hur toouse hello **Hotinformation** alternativ i OMS-säkerhet och granska lösningen toorespond toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="0ef44-162">In this document, you learned how toouse hello **Threat Intelligence** option in OMS Security and Audit solution toorespond toosecurity alerts.</span></span> <span data-ttu-id="0ef44-163">toolearn mer om OMS-säkerhet finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="0ef44-163">toolearn more about OMS Security, see hello following articles:</span></span>

* [<span data-ttu-id="0ef44-164">Översikt över Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="0ef44-164">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="0ef44-165">Komma igång med Operations Management Suite säkerhet och granska lösningen</span><span class="sxs-lookup"><span data-stu-id="0ef44-165">Getting started with Operations Management Suite Security and Audit Solution</span></span>](oms-security-getting-started.md)
* [<span data-ttu-id="0ef44-166">Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="0ef44-166">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

