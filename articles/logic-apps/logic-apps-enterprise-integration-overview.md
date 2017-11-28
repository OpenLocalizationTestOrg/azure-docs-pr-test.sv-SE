---
title: "Enterprise-integrering för B2B - Azure Logikappar | Microsoft Docs"
description: "Skapa B2B-arbetsflöden och stöder enterprise integrationsscenarier för logic apps med Enterprise-Integrationspaket"
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
ms.openlocfilehash: 9462707db03ecfcc3d5186ce7ded8655ad3bdcc9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="overview-b2b-scenarios-and-communication-with-the-enterprise-integration-pack"></a><span data-ttu-id="edd71-103">Översikt: B2B-scenarier och kommunikation med Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="edd71-103">Overview: B2B scenarios and communication with the Enterprise Integration Pack</span></span>

<span data-ttu-id="edd71-104">Du kan aktivera företagsscenarier integration med Microsofts molnbaserad lösning, Enterprise-Integrationspaket för business-to-business (B2B) arbetsflöden och sömlös kommunikation med Azure Logikappar.</span><span class="sxs-lookup"><span data-stu-id="edd71-104">For business-to-business (B2B) workflows and seamless communication with Azure Logic Apps, you can enable enterprise integration scenarios with Microsoft's cloud-based solution, the Enterprise Integration Pack.</span></span> <span data-ttu-id="edd71-105">Organisationer kan utbyta meddelanden elektroniskt, även om de använder olika protokoll och format.</span><span class="sxs-lookup"><span data-stu-id="edd71-105">Organizations can exchange messages electronically, even if they use different protocols and formats.</span></span> <span data-ttu-id="edd71-106">Packet transformerar olika format till ett format som organisationers system kan tolka och bearbeta.</span><span class="sxs-lookup"><span data-stu-id="edd71-106">The pack transforms different formats into a format that organizations' systems can interpret and process.</span></span> <span data-ttu-id="edd71-107">Organisationer kan utbyta meddelanden via standardiserade protokoll, inklusive [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), och [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md).</span><span class="sxs-lookup"><span data-stu-id="edd71-107">Organizations can exchange messages through industry-standard protocols, including [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), and [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md).</span></span> <span data-ttu-id="edd71-108">Du kan även skydda meddelanden med både kryptering och digitala signaturer.</span><span class="sxs-lookup"><span data-stu-id="edd71-108">You can also secure messages with both encryption and digital signatures.</span></span>

<span data-ttu-id="edd71-109">Om du är bekant med BizTalk Server eller BizTalk-tjänst för Microsoft Azure är Enterprise Integration-funktioner enkla att använda eftersom de flesta principerna är liknande.</span><span class="sxs-lookup"><span data-stu-id="edd71-109">If you are familiar with BizTalk Server or Microsoft Azure BizTalk Services, the Enterprise Integration features are easy to use because most concepts are similar.</span></span> <span data-ttu-id="edd71-110">En största skillnaden är att Enterprise Integration använder integrationskonton för att förenkla lagring och hantering av artefakter som används i B2B-kommunikation.</span><span class="sxs-lookup"><span data-stu-id="edd71-110">One major difference is that Enterprise Integration uses integration accounts to simplify the storage and management of artifacts used in B2B communications.</span></span> 

<span data-ttu-id="edd71-111">Enterprise-Integrationspaket är arkitektoniskt sett likadant baserat på ”integrationskonton”.</span><span class="sxs-lookup"><span data-stu-id="edd71-111">Architecturally, the Enterprise Integration Pack is based on "integration accounts".</span></span> <span data-ttu-id="edd71-112">Dessa konton är molnbaserad behållare som lagrar alla dina artefakter som scheman, partner, certifikat, kartor och avtal.</span><span class="sxs-lookup"><span data-stu-id="edd71-112">These accounts are cloud-based containers that store all your artifacts, like schemas, partners, certificates, maps, and agreements.</span></span> <span data-ttu-id="edd71-113">Du kan använda dessa artefakter att utforma, distribuera och underhålla dina B2B-appar och även skapa B2B arbetsflöden för logic apps.</span><span class="sxs-lookup"><span data-stu-id="edd71-113">You can use these artifacts to design, deploy, and maintain your B2B apps and also to build B2B workflows for logic apps.</span></span> <span data-ttu-id="edd71-114">Men innan du kan använda dessa artefakter, måste du först länka ditt integration konto till din logikapp.</span><span class="sxs-lookup"><span data-stu-id="edd71-114">But before you can use these artifacts, you must first link your integration account to your logic app.</span></span> <span data-ttu-id="edd71-115">Efter det logikappen kan komma åt ditt konto integration artefakter.</span><span class="sxs-lookup"><span data-stu-id="edd71-115">After that, your logic app can access your integration account's artifacts.</span></span>

## <a name="why-should-you-use-enterprise-integration"></a><span data-ttu-id="edd71-116">Varför ska du använda enterprise integration?</span><span class="sxs-lookup"><span data-stu-id="edd71-116">Why should you use enterprise integration?</span></span>

* <span data-ttu-id="edd71-117">Du kan lagra alla artefakter på ett ställe – kontot för integrering med enterprise-integration.</span><span class="sxs-lookup"><span data-stu-id="edd71-117">With enterprise integration, you can store all your artifacts in one place -- your integration account.</span></span>
* <span data-ttu-id="edd71-118">Du kan skapa B2B-arbetsflöden och integrera med tredje parts programvara som tjänst (SaaS)-appar, lokala appar och anpassade program med Azure Logikappar-motorn och alla kopplingar.</span><span class="sxs-lookup"><span data-stu-id="edd71-118">You can build B2B workflows and integrate with third-party software-as-service (SaaS) apps, on-premises apps, and custom apps by using the Azure Logic Apps engine and all its connectors.</span></span>
* <span data-ttu-id="edd71-119">Du kan skapa anpassade kod för dina logic apps med Azure functions.</span><span class="sxs-lookup"><span data-stu-id="edd71-119">You can create custom code for your logic apps with Azure functions.</span></span>

## <a name="how-to-get-started-with-enterprise-integration"></a><span data-ttu-id="edd71-120">Hur du kommer igång med enterprise integration?</span><span class="sxs-lookup"><span data-stu-id="edd71-120">How to get started with enterprise integration?</span></span>

<span data-ttu-id="edd71-121">Du kan skapa och hantera B2B-appar med Enterprise-Integrationspaket via logik App Designer i den **Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="edd71-121">You can build and manage B2B apps with the Enterprise Integration Pack through the Logic App Designer in the **Azure portal**.</span></span> <span data-ttu-id="edd71-122">Du kan också hantera dina logic apps med [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logic apps PowerShell-artiklar").</span><span class="sxs-lookup"><span data-stu-id="edd71-122">You can also manage your logic apps with [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logic apps PowerShell topics").</span></span>

<span data-ttu-id="edd71-123">Här följer de övergripande stegen som du måste vidta innan du kan skapa appar i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="edd71-123">Here are the high-level steps you must take before you can create apps in the Azure portal:</span></span>

![Översikt över bild](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a><span data-ttu-id="edd71-125">Vilka är några vanliga scenarier?</span><span class="sxs-lookup"><span data-stu-id="edd71-125">What are some common scenarios?</span></span>

<span data-ttu-id="edd71-126">Enterprise Integration stöder dessa branschstandarder:</span><span class="sxs-lookup"><span data-stu-id="edd71-126">Enterprise Integration supports these industry standards:</span></span>

* <span data-ttu-id="edd71-127">EDI - elektroniskt datautbyte</span><span class="sxs-lookup"><span data-stu-id="edd71-127">EDI - Electronic Data Interchange</span></span>
* <span data-ttu-id="edd71-128">EAI - Integration av företagsprogram</span><span class="sxs-lookup"><span data-stu-id="edd71-128">EAI - Enterprise Application Integration</span></span>

## <a name="heres-what-you-need-to-get-started"></a><span data-ttu-id="edd71-129">Här är vad du behöver att komma igång</span><span class="sxs-lookup"><span data-stu-id="edd71-129">Here's what you need to get started</span></span>

* <span data-ttu-id="edd71-130">En Azure-prenumeration med ett konto för integrering</span><span class="sxs-lookup"><span data-stu-id="edd71-130">An Azure subscription with an integration account</span></span>
* <span data-ttu-id="edd71-131">Visual Studio 2015 att skapa mappningar och scheman</span><span class="sxs-lookup"><span data-stu-id="edd71-131">Visual Studio 2015 to create maps and schemas</span></span>
* [<span data-ttu-id="edd71-132">Microsoft Azure Logic Apps Enterprise Integration Tools för Visual Studio 2015 2.0</span><span class="sxs-lookup"><span data-stu-id="edd71-132">Microsoft Azure Logic Apps Enterprise Integration Tools for Visual Studio 2015 2.0</span></span>](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a><span data-ttu-id="edd71-133">Prova nu</span><span class="sxs-lookup"><span data-stu-id="edd71-133">Try it now</span></span>

<span data-ttu-id="edd71-134">[Distribuera en fullt fungerande exempel AS2 skicka och ta emot logikapp](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) som använder B2B-funktioner för Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="edd71-134">[Deploy a fully operational sample AS2 send & receive logic app](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) that uses the B2B features for Azure Logic Apps.</span></span>

## <a name="learn-more"></a><span data-ttu-id="edd71-135">Läs mer</span><span class="sxs-lookup"><span data-stu-id="edd71-135">Learn more</span></span>
* [<span data-ttu-id="edd71-136">Avtal</span><span class="sxs-lookup"><span data-stu-id="edd71-136">Agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")
* [<span data-ttu-id="edd71-137">Scenarier för Business to Business (B2B)</span><span class="sxs-lookup"><span data-stu-id="edd71-137">Business to Business (B2B) scenarios</span></span>](../logic-apps/logic-apps-enterprise-integration-b2b.md "Lär dig att skapa logikappar med B2B-funktioner")  
* [<span data-ttu-id="edd71-138">Certifikat</span><span class="sxs-lookup"><span data-stu-id="edd71-138">Certificates</span></span>](logic-apps-enterprise-integration-certificates.md "Lär dig mer om integrering av företagscertifikat")
* [<span data-ttu-id="edd71-139">Flat fil kodning/avkodning</span><span class="sxs-lookup"><span data-stu-id="edd71-139">Flat file encoding/decoding</span></span>](logic-apps-enterprise-integration-flatfile.md "Lär dig hur du kodar och avkodar flat filinnehållet")  
* [<span data-ttu-id="edd71-140">Integrationskonton</span><span class="sxs-lookup"><span data-stu-id="edd71-140">Integration accounts</span></span>](../logic-apps/logic-apps-enterprise-integration-accounts.md "Lär dig mer om integrationskonton")
* [<span data-ttu-id="edd71-141">Mappar</span><span class="sxs-lookup"><span data-stu-id="edd71-141">Maps</span></span>](../logic-apps/logic-apps-enterprise-integration-maps.md "Lär dig mer om enterprise integration maps")
* [<span data-ttu-id="edd71-142">Partners</span><span class="sxs-lookup"><span data-stu-id="edd71-142">Partners</span></span>](logic-apps-enterprise-integration-partners.md "Lär dig mer om enterprise integration-partner")
* [<span data-ttu-id="edd71-143">Scheman</span><span class="sxs-lookup"><span data-stu-id="edd71-143">Schemas</span></span>](logic-apps-enterprise-integration-schemas.md "Lär dig mer om enterprise integration-scheman")
* [<span data-ttu-id="edd71-144">XML-meddelandevalidering</span><span class="sxs-lookup"><span data-stu-id="edd71-144">XML message validation</span></span>](logic-apps-enterprise-integration-xml.md "Lär dig hur du verifierar XML-meddelanden med Logic apps")
* [<span data-ttu-id="edd71-145">XML-transformeringen</span><span class="sxs-lookup"><span data-stu-id="edd71-145">XML transform</span></span>](logic-apps-enterprise-integration-transform.md "Lär dig mer om enterprise integration maps")
* [<span data-ttu-id="edd71-146">Enterprise Integration-kopplingar</span><span class="sxs-lookup"><span data-stu-id="edd71-146">Enterprise Integration Connectors</span></span>](../connectors/apis-list.md "Lär dig mer om enterprise integration pack-kopplingar")
* [<span data-ttu-id="edd71-147">Integration konto Metadata</span><span class="sxs-lookup"><span data-stu-id="edd71-147">Integration Account Metadata</span></span>](../logic-apps/logic-apps-enterprise-integration-metadata.md "Lär dig mer om integrering konto metadata")
* [<span data-ttu-id="edd71-148">Övervaka B2B-meddelanden</span><span class="sxs-lookup"><span data-stu-id="edd71-148">Monitor B2B messages</span></span>](logic-apps-monitor-b2b-message.md "Lär dig mer om hur du övervakar B2B-meddelanden")
* [<span data-ttu-id="edd71-149">Spåra B2B-meddelanden i OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="edd71-149">Tracking B2B messages in OMS portal</span></span>](logic-apps-track-b2b-messages-omsportal.md "Lär dig mer om hur du spårar B2B-meddelanden i OMS-portalen")

