---
title: "aaaHow tooopen hello brandväggsportar krävs för ett program med Application Proxy | Microsoft Docs"
description: "Ta reda på vilka portar tooopen för hello Azure AD Application Proxy toowork korrekt"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: cdc7badb7c15591689a3bfd6bb26da182b00fb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-hello-firewall-ports-required-for-an-application-proxy-application"></a><span data-ttu-id="44caa-103">Hur tooopen hello brandväggsportar som krävs för ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="44caa-103">How tooopen hello firewall ports required for an Application Proxy application</span></span>

<span data-ttu-id="44caa-104">toosee en fullständig lista över hello krävs portar och hello funktionerna i varje port finns hello förutsättningar i hello [Application Proxy dokumentationen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable).</span><span class="sxs-lookup"><span data-stu-id="44caa-104">toosee a full list of hello required ports and hello function of each port, see hello prerequisites section of hello [Application Proxy documentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable).</span></span> <span data-ttu-id="44caa-105">Observera att Application Proxy endast använder utgående portar.</span><span class="sxs-lookup"><span data-stu-id="44caa-105">note that Application Proxy only uses outbound ports.</span></span>

<span data-ttu-id="44caa-106">Du kan också kontrollera om du har alla hello krävs portar öppna genom att öppna hello [anslutningsverktyget portar Test](https://aadap-portcheck.connectorporttest.msappproxy.net/) från ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="44caa-106">You can also check whether you have all hello required ports open by opening hello [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/) from your on-prem network.</span></span> <span data-ttu-id="44caa-107">Flera gröna bockmarkeringarna innebär större flexibilitet.</span><span class="sxs-lookup"><span data-stu-id="44caa-107">More green checkmarks means greater resiliency.</span></span> 

## <a name="app-proxy-regions"></a><span data-ttu-id="44caa-108">Appen Proxy regioner</span><span class="sxs-lookup"><span data-stu-id="44caa-108">App Proxy regions</span></span>

<span data-ttu-id="44caa-109">Vi arbetar på ett sätt som du vet vilken av dessa regioner toolet måste toobe grön du.</span><span class="sxs-lookup"><span data-stu-id="44caa-109">We are working on a way toolet you know which of these regions needs toobe green for you.</span></span> <span data-ttu-id="44caa-110">Kontrollera att alla är för tillfället.</span><span class="sxs-lookup"><span data-stu-id="44caa-110">For now, make sure they all are.</span></span> <span data-ttu-id="44caa-111">Centrala USA krävs oavsett vilken region som du befinner dig i.</span><span class="sxs-lookup"><span data-stu-id="44caa-111">Central US is also required regardless of which region you are in.</span></span>

<span data-ttu-id="44caa-112">toomake att hello verktyget ger du hello rätt resultat måste du kontrollera att:</span><span class="sxs-lookup"><span data-stu-id="44caa-112">toomake sure hello tool gives you hello right results, be sure to:</span></span>

-   <span data-ttu-id="44caa-113">Öppna hello-verktyget i en webbläsare från hello server där du har installerat hello Connector.</span><span class="sxs-lookup"><span data-stu-id="44caa-113">Open hello tool on a browser from hello server where you have installed hello Connector.</span></span>

-   <span data-ttu-id="44caa-114">Kontrollera att alla proxyservrar och brandväggar tillämpliga tooyour Connector är också används toothis sidan.</span><span class="sxs-lookup"><span data-stu-id="44caa-114">Ensure that any proxies or firewalls applicable tooyour Connector are also applied toothis page.</span></span> <span data-ttu-id="44caa-115">Detta kan göras i Internet Explorer genom att gå för**inställningar**  - &gt; **Internetalternativ**  - &gt; **anslutningar**  - &gt; **Lan-inställningar**.</span><span class="sxs-lookup"><span data-stu-id="44caa-115">This can be done in Internet Explorer by going too**Settings** -&gt; **Internet Options** -&gt; **Connections** -&gt; **Lan Settings**.</span></span> <span data-ttu-id="44caa-116">På den här sidan kan se du hello fältet ”Använd en Proxy-Server för ditt LAN”.</span><span class="sxs-lookup"><span data-stu-id="44caa-116">On this page, you see hello field “Use a Proxy Server for your LAN”.</span></span> <span data-ttu-id="44caa-117">Välj den här rutan och placera hello proxyadress i hello ”adress” fält.</span><span class="sxs-lookup"><span data-stu-id="44caa-117">Select this box, and put hello proxy address into hello “Address” field.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44caa-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44caa-118">Next steps</span></span>
[<span data-ttu-id="44caa-119">Förstå Azure AD Application Proxy-kopplingar</span><span class="sxs-lookup"><span data-stu-id="44caa-119">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
