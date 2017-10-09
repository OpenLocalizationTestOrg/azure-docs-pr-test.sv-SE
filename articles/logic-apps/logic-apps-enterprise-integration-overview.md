---
title: "aaaEnterprise-integrering för B2B - Azure Logic Apps | Microsoft Docs"
description: "Skapa B2B-arbetsflöden och stöder enterprise integrationsscenarier för logic apps med hello Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: dd517c4d-1701-4247-b83c-183c4d8d8aae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 8dc866533110b1d07f51cf446056d2ca5ce869ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-b2b-scenarios-and-communication-with-hello-enterprise-integration-pack"></a><span data-ttu-id="0b266-103">Översikt: B2B-scenarier och kommunikation med hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="0b266-103">Overview: B2B scenarios and communication with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="0b266-104">Du kan aktivera företagsscenarier integration med Microsofts molnbaserade lösningen kan hello Enterprise-Integrationspaket för business-to-business (B2B) arbetsflöden och sömlös kommunikation med Azure Logikappar.</span><span class="sxs-lookup"><span data-stu-id="0b266-104">For business-to-business (B2B) workflows and seamless communication with Azure Logic Apps, you can enable enterprise integration scenarios with Microsoft's cloud-based solution, hello Enterprise Integration Pack.</span></span> <span data-ttu-id="0b266-105">Organisationer kan utbyta meddelanden elektroniskt, även om de använder olika protokoll och format.</span><span class="sxs-lookup"><span data-stu-id="0b266-105">Organizations can exchange messages electronically, even if they use different protocols and formats.</span></span> <span data-ttu-id="0b266-106">hello pack transformerar olika format till ett format som organisationers system kan tolka och bearbeta.</span><span class="sxs-lookup"><span data-stu-id="0b266-106">hello pack transforms different formats into a format that organizations' systems can interpret and process.</span></span> <span data-ttu-id="0b266-107">Organisationer kan utbyta meddelanden via standardiserade protokoll, inklusive [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), och [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md).</span><span class="sxs-lookup"><span data-stu-id="0b266-107">Organizations can exchange messages through industry-standard protocols, including [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), and [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md).</span></span> <span data-ttu-id="0b266-108">Du kan även skydda meddelanden med både kryptering och digitala signaturer.</span><span class="sxs-lookup"><span data-stu-id="0b266-108">You can also secure messages with both encryption and digital signatures.</span></span>

<span data-ttu-id="0b266-109">Om du är bekant med BizTalk Server eller Microsoft Azure BizTalk Services är hello Enterprise Integration funktioner enkelt toouse eftersom de flesta principerna är liknande.</span><span class="sxs-lookup"><span data-stu-id="0b266-109">If you are familiar with BizTalk Server or Microsoft Azure BizTalk Services, hello Enterprise Integration features are easy toouse because most concepts are similar.</span></span> <span data-ttu-id="0b266-110">En största skillnaden är att Enterprise Integration använder integration konton toosimplify hello lagring och hantering av artefakter som används i B2B-kommunikation.</span><span class="sxs-lookup"><span data-stu-id="0b266-110">One major difference is that Enterprise Integration uses integration accounts toosimplify hello storage and management of artifacts used in B2B communications.</span></span> 

<span data-ttu-id="0b266-111">Arkitektoniskt sett likadant hello Enterprise-Integrationspaket baseras på ”integrationskonton”.</span><span class="sxs-lookup"><span data-stu-id="0b266-111">Architecturally, hello Enterprise Integration Pack is based on "integration accounts".</span></span> <span data-ttu-id="0b266-112">Dessa konton är molnbaserad behållare som lagrar alla dina artefakter som scheman, partner, certifikat, kartor och avtal.</span><span class="sxs-lookup"><span data-stu-id="0b266-112">These accounts are cloud-based containers that store all your artifacts, like schemas, partners, certificates, maps, and agreements.</span></span> <span data-ttu-id="0b266-113">Du kan använda dessa artefakter toodesign, distribuera och underhålla dina B2B-appar och även toobuild B2B arbetsflöden för logic apps.</span><span class="sxs-lookup"><span data-stu-id="0b266-113">You can use these artifacts toodesign, deploy, and maintain your B2B apps and also toobuild B2B workflows for logic apps.</span></span> <span data-ttu-id="0b266-114">Men innan du kan använda dessa artefakter, måste du först koppla logikappen integrering konto tooyour.</span><span class="sxs-lookup"><span data-stu-id="0b266-114">But before you can use these artifacts, you must first link your integration account tooyour logic app.</span></span> <span data-ttu-id="0b266-115">Efter det logikappen kan komma åt ditt konto integration artefakter.</span><span class="sxs-lookup"><span data-stu-id="0b266-115">After that, your logic app can access your integration account's artifacts.</span></span>

## <a name="why-should-you-use-enterprise-integration"></a><span data-ttu-id="0b266-116">Varför ska du använda enterprise integration?</span><span class="sxs-lookup"><span data-stu-id="0b266-116">Why should you use enterprise integration?</span></span>

* <span data-ttu-id="0b266-117">Du kan lagra alla artefakter på ett ställe – kontot för integrering med enterprise-integration.</span><span class="sxs-lookup"><span data-stu-id="0b266-117">With enterprise integration, you can store all your artifacts in one place -- your integration account.</span></span>
* <span data-ttu-id="0b266-118">Du kan skapa B2B-arbetsflöden och integrera med tredje parts programvara som tjänst (SaaS)-appar, lokala appar och anpassade appar med hjälp av hello Azure Logic Apps-motorn och alla kopplingar.</span><span class="sxs-lookup"><span data-stu-id="0b266-118">You can build B2B workflows and integrate with third-party software-as-service (SaaS) apps, on-premises apps, and custom apps by using hello Azure Logic Apps engine and all its connectors.</span></span>
* <span data-ttu-id="0b266-119">Du kan skapa anpassade kod för dina logic apps med Azure functions.</span><span class="sxs-lookup"><span data-stu-id="0b266-119">You can create custom code for your logic apps with Azure functions.</span></span>

## <a name="how-tooget-started-with-enterprise-integration"></a><span data-ttu-id="0b266-120">Hur tooget igång med enterprise integration?</span><span class="sxs-lookup"><span data-stu-id="0b266-120">How tooget started with enterprise integration?</span></span>

<span data-ttu-id="0b266-121">Du kan skapa och hantera B2B-appar med hello Enterprise-Integrationspaket via hello logik App Designer i hello **Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="0b266-121">You can build and manage B2B apps with hello Enterprise Integration Pack through hello Logic App Designer in hello **Azure portal**.</span></span> <span data-ttu-id="0b266-122">Du kan också hantera dina logic apps med [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logic apps PowerShell-artiklar").</span><span class="sxs-lookup"><span data-stu-id="0b266-122">You can also manage your logic apps with [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logic apps PowerShell topics").</span></span>

<span data-ttu-id="0b266-123">Här följer hello övergripande steg du måste utföra innan du kan skapa appar i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="0b266-123">Here are hello high-level steps you must take before you can create apps in hello Azure portal:</span></span>

![Översikt över bild](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a><span data-ttu-id="0b266-125">Vilka är några vanliga scenarier?</span><span class="sxs-lookup"><span data-stu-id="0b266-125">What are some common scenarios?</span></span>

<span data-ttu-id="0b266-126">Enterprise Integration stöder dessa branschstandarder:</span><span class="sxs-lookup"><span data-stu-id="0b266-126">Enterprise Integration supports these industry standards:</span></span>

* <span data-ttu-id="0b266-127">EDI - elektroniskt datautbyte</span><span class="sxs-lookup"><span data-stu-id="0b266-127">EDI - Electronic Data Interchange</span></span>
* <span data-ttu-id="0b266-128">EAI - Integration av företagsprogram</span><span class="sxs-lookup"><span data-stu-id="0b266-128">EAI - Enterprise Application Integration</span></span>

## <a name="heres-what-you-need-tooget-started"></a><span data-ttu-id="0b266-129">Här är vad du behöver tooget igång</span><span class="sxs-lookup"><span data-stu-id="0b266-129">Here's what you need tooget started</span></span>

* <span data-ttu-id="0b266-130">En Azure-prenumeration med ett konto för integrering</span><span class="sxs-lookup"><span data-stu-id="0b266-130">An Azure subscription with an integration account</span></span>
* <span data-ttu-id="0b266-131">Visual Studio 2015 toocreate maps och scheman</span><span class="sxs-lookup"><span data-stu-id="0b266-131">Visual Studio 2015 toocreate maps and schemas</span></span>
* [<span data-ttu-id="0b266-132">Microsoft Azure Logic Apps Enterprise Integration Tools för Visual Studio 2015 2.0</span><span class="sxs-lookup"><span data-stu-id="0b266-132">Microsoft Azure Logic Apps Enterprise Integration Tools for Visual Studio 2015 2.0</span></span>](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a><span data-ttu-id="0b266-133">Prova nu</span><span class="sxs-lookup"><span data-stu-id="0b266-133">Try it now</span></span>

<span data-ttu-id="0b266-134">[Distribuera en fullt fungerande exempel AS2 skicka och ta emot logikapp](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) som använder hello B2B-funktioner för Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="0b266-134">[Deploy a fully operational sample AS2 send & receive logic app](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) that uses hello B2B features for Azure Logic Apps.</span></span>

## <a name="learn-more"></a><span data-ttu-id="0b266-135">Läs mer</span><span class="sxs-lookup"><span data-stu-id="0b266-135">Learn more</span></span>
* [<span data-ttu-id="0b266-136">Avtal</span><span class="sxs-lookup"><span data-stu-id="0b266-136">Agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")
* [<span data-ttu-id="0b266-137">TooBusiness (B2B) affärsscenarier</span><span class="sxs-lookup"><span data-stu-id="0b266-137">Business tooBusiness (B2B) scenarios</span></span>](../logic-apps/logic-apps-enterprise-integration-b2b.md "Lär dig hur toocreate Logic apps med B2B-funktioner")  
* [<span data-ttu-id="0b266-138">Certifikat</span><span class="sxs-lookup"><span data-stu-id="0b266-138">Certificates</span></span>](logic-apps-enterprise-integration-certificates.md "Lär dig mer om integrering av företagscertifikat")
* [<span data-ttu-id="0b266-139">Flat fil kodning/avkodning</span><span class="sxs-lookup"><span data-stu-id="0b266-139">Flat file encoding/decoding</span></span>](logic-apps-enterprise-integration-flatfile.md "Lär dig hur tooencode och avkoda flat filinnehållet")  
* [<span data-ttu-id="0b266-140">Integrationskonton</span><span class="sxs-lookup"><span data-stu-id="0b266-140">Integration accounts</span></span>](../logic-apps/logic-apps-enterprise-integration-accounts.md "Lär dig mer om integrationskonton")
* [<span data-ttu-id="0b266-141">Mappar</span><span class="sxs-lookup"><span data-stu-id="0b266-141">Maps</span></span>](../logic-apps/logic-apps-enterprise-integration-maps.md "Lär dig mer om enterprise integration maps")
* [<span data-ttu-id="0b266-142">Partners</span><span class="sxs-lookup"><span data-stu-id="0b266-142">Partners</span></span>](logic-apps-enterprise-integration-partners.md "Lär dig mer om enterprise integration-partner")
* [<span data-ttu-id="0b266-143">Scheman</span><span class="sxs-lookup"><span data-stu-id="0b266-143">Schemas</span></span>](logic-apps-enterprise-integration-schemas.md "Lär dig mer om enterprise integration-scheman")
* [<span data-ttu-id="0b266-144">XML-meddelandevalidering</span><span class="sxs-lookup"><span data-stu-id="0b266-144">XML message validation</span></span>](logic-apps-enterprise-integration-xml.md "Lär dig hur toovalidate XML meddelanden med Logic apps")
* [<span data-ttu-id="0b266-145">XML-transformeringen</span><span class="sxs-lookup"><span data-stu-id="0b266-145">XML transform</span></span>](logic-apps-enterprise-integration-transform.md "Lär dig mer om enterprise integration maps")
* [<span data-ttu-id="0b266-146">Enterprise Integration-kopplingar</span><span class="sxs-lookup"><span data-stu-id="0b266-146">Enterprise Integration Connectors</span></span>](../connectors/apis-list.md "Lär dig mer om enterprise integration pack-kopplingar")
* [<span data-ttu-id="0b266-147">Integration konto Metadata</span><span class="sxs-lookup"><span data-stu-id="0b266-147">Integration Account Metadata</span></span>](../logic-apps/logic-apps-enterprise-integration-metadata.md "Lär dig mer om integrering konto metadata")
* [<span data-ttu-id="0b266-148">Övervaka B2B-meddelanden</span><span class="sxs-lookup"><span data-stu-id="0b266-148">Monitor B2B messages</span></span>](logic-apps-monitor-b2b-message.md "Lär dig mer om hur du övervakar B2B-meddelanden")
* [<span data-ttu-id="0b266-149">Spåra B2B-meddelanden i OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="0b266-149">Tracking B2B messages in OMS portal</span></span>](logic-apps-track-b2b-messages-omsportal.md "Lär dig mer om hur du spårar B2B-meddelanden i OMS-portalen")

