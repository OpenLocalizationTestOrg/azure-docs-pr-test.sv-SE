---
title: aaaFinding ohanterad molnprogram med Cloud App Discovery | Microsoft Docs
description: "Innehåller information om att söka efter och hantera program med Cloud App Discovery, vilka är fördelarna med hello och hur det fungerar."
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
ms.openlocfilehash: 50c24af9bb400e4be11f4ad2d1de13d26f5467bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="finding-unmanaged-cloud-applications-with-cloud-app-discovery"></a><span data-ttu-id="0d68c-104">Hitta ohanterad molnprogram med Cloud App Discovery</span><span class="sxs-lookup"><span data-stu-id="0d68c-104">Finding unmanaged cloud applications with Cloud App Discovery</span></span>
## <a name="overview"></a><span data-ttu-id="0d68c-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="0d68c-105">Overview</span></span>
<span data-ttu-id="0d68c-106">Moderna företag, IT-avdelningar ofta inte är medveten om alla hello molnprogram att medlemmar i organisationen använder toodo sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="0d68c-106">In modern enterprises, IT departments are often not aware of all hello cloud applications that members of their organization use toodo their work.</span></span> <span data-ttu-id="0d68c-107">Det är enkelt toosee varför administratörer skulle oroar obehörig åtkomst toocorporate data, möjliga dataläckage och andra säkerhetsrisker.</span><span class="sxs-lookup"><span data-stu-id="0d68c-107">It is easy toosee why administrators would have concerns about unauthorized access toocorporate data, possible data leakage and other security risks.</span></span> <span data-ttu-id="0d68c-108">Den här bristen på medvetenhet kan göra genom att skapa en plan för hantering av dessa säkerhetsrisker verka avskräckande.</span><span class="sxs-lookup"><span data-stu-id="0d68c-108">This lack of awareness can make creating a plan for dealing with these security risks seem daunting.</span></span>

<span data-ttu-id="0d68c-109">Cloud App Discovery är en funktion i Azure Active Directory (AD) Premium som gör att du toodiscover molnprogram som används av hello personer i din organisation.</span><span class="sxs-lookup"><span data-stu-id="0d68c-109">Cloud App Discovery is a feature of Azure Active Directory (AD) Premium that enables you toodiscover cloud applications being used by hello people in your organization.</span></span>

<span data-ttu-id="0d68c-110">**Med Cloud App Discovery kan du:**</span><span class="sxs-lookup"><span data-stu-id="0d68c-110">**With Cloud App Discovery, you can:**</span></span>

* <span data-ttu-id="0d68c-111">Hitta hello molnprogram som används och mäta denna användning av antalet användare, datavolym trafik eller antalet begäranden toohello för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0d68c-111">Find hello cloud applications being used and measure that usage by number of users, volume of traffic or number of web requests toohello application.</span></span>
* <span data-ttu-id="0d68c-112">Identifiera hello-användare som använder ett program.</span><span class="sxs-lookup"><span data-stu-id="0d68c-112">Identify hello users that are using an application.</span></span>
* <span data-ttu-id="0d68c-113">Exportera data för offlineanalys.</span><span class="sxs-lookup"><span data-stu-id="0d68c-113">Export data for offline analysis.</span></span>
* <span data-ttu-id="0d68c-114">Se dessa program under IT-kontroll och aktivera enkel inloggning på för användarhantering.</span><span class="sxs-lookup"><span data-stu-id="0d68c-114">Bring these applications under IT control and enable single sign on for user management.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="0d68c-115">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="0d68c-115">How it works</span></span>
1. <span data-ttu-id="0d68c-116">Programmet användning agenter är installerade på användarens dator.</span><span class="sxs-lookup"><span data-stu-id="0d68c-116">Application usage agents are installed on user's computers.</span></span>
2. <span data-ttu-id="0d68c-117">hello programmet användningsinformation som avbildas av hello agenter skickas via en säker och krypterad kanal toohello cloud app discovery-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0d68c-117">hello application usage information captured by hello agents is sent over a secure, encrypted channel toohello cloud app discovery service.</span></span>
3. <span data-ttu-id="0d68c-118">hello Cloud App Discovery-tjänsten utvärderar hello data och genererar rapporter.</span><span class="sxs-lookup"><span data-stu-id="0d68c-118">hello Cloud App Discovery service evaluates hello data and generates reports.</span></span>

![Cloud App Discovery-diagram](./media/active-directory-cloudappdiscovery/cad01.png)

<span data-ttu-id="0d68c-120">tooget igång med Cloud App Discovery finns [komma igång med Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)</span><span class="sxs-lookup"><span data-stu-id="0d68c-120">tooget started with Cloud App Discovery, see [Getting Started With Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)</span></span>

## <a name="related-articles"></a><span data-ttu-id="0d68c-121">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="0d68c-121">Related articles</span></span>
* [<span data-ttu-id="0d68c-122">Cloud App Discovery säkerhets- och överväganden för sekretess</span><span class="sxs-lookup"><span data-stu-id="0d68c-122">Cloud App Discovery Security and Privacy Considerations</span></span>](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [<span data-ttu-id="0d68c-123">Distributionsguide för cloud App Discovery grupp princip</span><span class="sxs-lookup"><span data-stu-id="0d68c-123">Cloud App Discovery Group Policy Deployment Guide</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)
* [<span data-ttu-id="0d68c-124">Distributionsguide för cloud App Discovery System Center</span><span class="sxs-lookup"><span data-stu-id="0d68c-124">Cloud App Discovery System Center Deployment Guide</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)
* [<span data-ttu-id="0d68c-125">Cloud App Discovery-registerinställningar för proxyservrar med anpassade portar</span><span class="sxs-lookup"><span data-stu-id="0d68c-125">Cloud App Discovery Registry Settings for Proxy Servers with Custom Ports</span></span>](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [<span data-ttu-id="0d68c-126">Cloud App Discovery-agenten Changelog</span><span class="sxs-lookup"><span data-stu-id="0d68c-126">Cloud App Discovery Agent Changelog </span></span>](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [<span data-ttu-id="0d68c-127">Vanliga och frågor svar om cloud App Discovery</span><span class="sxs-lookup"><span data-stu-id="0d68c-127">Cloud App Discovery Frequently Asked Questions</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)
* [<span data-ttu-id="0d68c-128">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d68c-128">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)

