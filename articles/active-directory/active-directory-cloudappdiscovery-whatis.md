---
title: Hitta ohanterade molnprogram med Cloud App Discovery | Microsoft Docs
description: "Innehåller information om att söka efter och hantera program med Cloud App Discovery, vilka är fördelarna och hur det fungerar."
services: active-directory
keywords: cloud app discovery, hantera program
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 6284ff5bac8edbc19561d0916adef153526dfbe3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="finding-unmanaged-cloud-applications-with-cloud-app-discovery"></a><span data-ttu-id="29bb2-104">Hitta ohanterad molnprogram med Cloud App Discovery</span><span class="sxs-lookup"><span data-stu-id="29bb2-104">Finding unmanaged cloud applications with Cloud App Discovery</span></span>
## <a name="overview"></a><span data-ttu-id="29bb2-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="29bb2-105">Overview</span></span>
<span data-ttu-id="29bb2-106">Moderna företag, IT-avdelningar ofta inte är medveten om alla molnprogram som medlemmar i organisationen använder för att utföra sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="29bb2-106">In modern enterprises, IT departments are often not aware of all the cloud applications that members of their organization use to do their work.</span></span> <span data-ttu-id="29bb2-107">Det är enkelt att se varför administratörer skulle oroar obehörig åtkomst till företagsdata, möjliga dataläckage och andra säkerhetsrisker.</span><span class="sxs-lookup"><span data-stu-id="29bb2-107">It is easy to see why administrators would have concerns about unauthorized access to corporate data, possible data leakage and other security risks.</span></span> <span data-ttu-id="29bb2-108">Den här bristen på medvetenhet kan göra genom att skapa en plan för hantering av dessa säkerhetsrisker verka avskräckande.</span><span class="sxs-lookup"><span data-stu-id="29bb2-108">This lack of awareness can make creating a plan for dealing with these security risks seem daunting.</span></span>

<span data-ttu-id="29bb2-109">Cloud App Discovery är en funktion i Azure Active Directory (AD) Premium där du kan identifiera molnprogram som används av personer i din organisation.</span><span class="sxs-lookup"><span data-stu-id="29bb2-109">Cloud App Discovery is a feature of Azure Active Directory (AD) Premium that enables you to discover cloud applications being used by the people in your organization.</span></span>

<span data-ttu-id="29bb2-110">**Med Cloud App Discovery kan du:**</span><span class="sxs-lookup"><span data-stu-id="29bb2-110">**With Cloud App Discovery, you can:**</span></span>

* <span data-ttu-id="29bb2-111">Hitta molnprogram som används och mäta denna användning av antalet användare, datavolym trafik eller antalet webbegäranden till programmet.</span><span class="sxs-lookup"><span data-stu-id="29bb2-111">Find the cloud applications being used and measure that usage by number of users, volume of traffic or number of web requests to the application.</span></span>
* <span data-ttu-id="29bb2-112">Identifiera de användare som använder ett program.</span><span class="sxs-lookup"><span data-stu-id="29bb2-112">Identify the users that are using an application.</span></span>
* <span data-ttu-id="29bb2-113">Exportera data för offlineanalys.</span><span class="sxs-lookup"><span data-stu-id="29bb2-113">Export data for offline analysis.</span></span>
* <span data-ttu-id="29bb2-114">Se dessa program under IT-kontroll och aktivera enkel inloggning på för användarhantering.</span><span class="sxs-lookup"><span data-stu-id="29bb2-114">Bring these applications under IT control and enable single sign on for user management.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="29bb2-115">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="29bb2-115">How it works</span></span>
1. <span data-ttu-id="29bb2-116">Programmet användning agenter är installerade på användarens dator.</span><span class="sxs-lookup"><span data-stu-id="29bb2-116">Application usage agents are installed on user's computers.</span></span>
2. <span data-ttu-id="29bb2-117">Programmet användningsinformation som avbildas av agenterna skickas via en säker och krypterad kanal till cloud app discovery-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="29bb2-117">The application usage information captured by the agents is sent over a secure, encrypted channel to the cloud app discovery service.</span></span>
3. <span data-ttu-id="29bb2-118">Cloud App Discovery-tjänsten utvärderar data och genererar rapporter.</span><span class="sxs-lookup"><span data-stu-id="29bb2-118">The Cloud App Discovery service evaluates the data and generates reports.</span></span>

![Cloud App Discovery-diagram](./media/active-directory-cloudappdiscovery/cad01.png)

<span data-ttu-id="29bb2-120">Om du vill komma igång med Cloud App Discovery finns [komma igång med Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)</span><span class="sxs-lookup"><span data-stu-id="29bb2-120">To get started with Cloud App Discovery, see [Getting Started With Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)</span></span>

## <a name="related-articles"></a><span data-ttu-id="29bb2-121">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="29bb2-121">Related articles</span></span>
* [<span data-ttu-id="29bb2-122">Cloud App Discovery säkerhets- och överväganden för sekretess</span><span class="sxs-lookup"><span data-stu-id="29bb2-122">Cloud App Discovery Security and Privacy Considerations</span></span>](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [<span data-ttu-id="29bb2-123">Distributionsguide för cloud App Discovery grupp princip</span><span class="sxs-lookup"><span data-stu-id="29bb2-123">Cloud App Discovery Group Policy Deployment Guide</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)
* [<span data-ttu-id="29bb2-124">Distributionsguide för cloud App Discovery System Center</span><span class="sxs-lookup"><span data-stu-id="29bb2-124">Cloud App Discovery System Center Deployment Guide</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)
* [<span data-ttu-id="29bb2-125">Cloud App Discovery-registerinställningar för proxyservrar med anpassade portar</span><span class="sxs-lookup"><span data-stu-id="29bb2-125">Cloud App Discovery Registry Settings for Proxy Servers with Custom Ports</span></span>](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [<span data-ttu-id="29bb2-126">Cloud App Discovery-agenten Changelog</span><span class="sxs-lookup"><span data-stu-id="29bb2-126">Cloud App Discovery Agent Changelog </span></span>](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [<span data-ttu-id="29bb2-127">Vanliga och frågor svar om cloud App Discovery</span><span class="sxs-lookup"><span data-stu-id="29bb2-127">Cloud App Discovery Frequently Asked Questions</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)
* [<span data-ttu-id="29bb2-128">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="29bb2-128">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)

