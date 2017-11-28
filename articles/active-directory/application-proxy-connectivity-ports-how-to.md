---
title: "Så här öppnar du portar i brandväggen krävs för ett program med Application Proxy | Microsoft Docs"
description: "Ta reda på vilka portar som bör öppnas för Azure AD Application Proxy ska fungera korrekt"
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
ms.openlocfilehash: 8ecd6d7e666d362194126a4abba7a65f2c7b8b6b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a><span data-ttu-id="1a12b-103">Så här öppnar du portar i brandväggen krävs för ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="1a12b-103">How to open the firewall ports required for an Application Proxy application</span></span>

<span data-ttu-id="1a12b-104">En fullständig lista över portarna som krävs och funktionerna i varje port finns i avsnittet förutsättningar i den [Application Proxy dokumentationen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable).</span><span class="sxs-lookup"><span data-stu-id="1a12b-104">To see a full list of the required ports and the function of each port, see the prerequisites section of the [Application Proxy documentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable).</span></span> <span data-ttu-id="1a12b-105">Observera att Application Proxy endast använder utgående portar.</span><span class="sxs-lookup"><span data-stu-id="1a12b-105">note that Application Proxy only uses outbound ports.</span></span>

<span data-ttu-id="1a12b-106">Du kan också kontrollera om du har alla de nödvändiga portarna öppna genom att öppna den [anslutningsverktyget portar Test](https://aadap-portcheck.connectorporttest.msappproxy.net/) från ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="1a12b-106">You can also check whether you have all the required ports open by opening the [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/) from your on-prem network.</span></span> <span data-ttu-id="1a12b-107">Flera gröna bockmarkeringarna innebär större flexibilitet.</span><span class="sxs-lookup"><span data-stu-id="1a12b-107">More green checkmarks means greater resiliency.</span></span> 

## <a name="app-proxy-regions"></a><span data-ttu-id="1a12b-108">Appen Proxy regioner</span><span class="sxs-lookup"><span data-stu-id="1a12b-108">App Proxy regions</span></span>

<span data-ttu-id="1a12b-109">Vi arbetar på ett sätt så att du vet vilken av dessa regioner måste vara grön för dig.</span><span class="sxs-lookup"><span data-stu-id="1a12b-109">We are working on a way to let you know which of these regions needs to be green for you.</span></span> <span data-ttu-id="1a12b-110">Kontrollera att alla är för tillfället.</span><span class="sxs-lookup"><span data-stu-id="1a12b-110">For now, make sure they all are.</span></span> <span data-ttu-id="1a12b-111">Centrala USA krävs oavsett vilken region som du befinner dig i.</span><span class="sxs-lookup"><span data-stu-id="1a12b-111">Central US is also required regardless of which region you are in.</span></span>

<span data-ttu-id="1a12b-112">Om du vill se till att verktyget ger rätt resultat, måste du kontrollera att:</span><span class="sxs-lookup"><span data-stu-id="1a12b-112">To make sure the tool gives you the right results, be sure to:</span></span>

-   <span data-ttu-id="1a12b-113">Öppna verktyget i en webbläsare från den server där du har installerat kopplingen.</span><span class="sxs-lookup"><span data-stu-id="1a12b-113">Open the tool on a browser from the server where you have installed the Connector.</span></span>

-   <span data-ttu-id="1a12b-114">Se till att alla proxyservrar och brandväggar gäller för din anslutningstjänst också används för den här sidan.</span><span class="sxs-lookup"><span data-stu-id="1a12b-114">Ensure that any proxies or firewalls applicable to your Connector are also applied to this page.</span></span> <span data-ttu-id="1a12b-115">Detta kan göras i Internet Explorer genom att gå till **inställningar**  - &gt; **Internetalternativ**  - &gt; **anslutningar**  - &gt; **Lan-inställningar**.</span><span class="sxs-lookup"><span data-stu-id="1a12b-115">This can be done in Internet Explorer by going to **Settings** -&gt; **Internet Options** -&gt; **Connections** -&gt; **Lan Settings**.</span></span> <span data-ttu-id="1a12b-116">Fältet ”Använd en Proxy-Server för ditt LAN” visas på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="1a12b-116">On this page, you see the field “Use a Proxy Server for your LAN”.</span></span> <span data-ttu-id="1a12b-117">Välj den här rutan och placera Proxyadressen i fältet ”adress”.</span><span class="sxs-lookup"><span data-stu-id="1a12b-117">Select this box, and put the proxy address into the “Address” field.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a12b-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a12b-118">Next steps</span></span>
[<span data-ttu-id="1a12b-119">Förstå Azure AD Application Proxy-kopplingar</span><span class="sxs-lookup"><span data-stu-id="1a12b-119">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
