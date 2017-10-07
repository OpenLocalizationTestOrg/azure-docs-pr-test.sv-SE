---
title: "aaaUser etablering tooan Azure AD-galleriet program är ta timmar eller mer | Microsoft Docs"
description: "Hur toofind reda på varför etablering tooyour programmet kan tar längre tid än väntat"
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
ms.openlocfilehash: 0658f041724c91ddd1997cc7759393b46680f13a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="user-provisioning-tooan-azure-ad-gallery-application-is-taking-hours-or-more"></a><span data-ttu-id="f58fd-103">Användaretablering tooan Azure AD-galleriet program är ta timmar eller mer</span><span class="sxs-lookup"><span data-stu-id="f58fd-103">User provisioning tooan Azure AD Gallery application is taking hours or more</span></span>

<span data-ttu-id="f58fd-104">När du först aktiverar automatisk etablering för ett program, kan hello inledande synkronisering ta mellan 20 minuter tooseveral timmar, beroende på hello storleken på hello Azure AD-katalog och hello antalet användare i omfånget för etablering.</span><span class="sxs-lookup"><span data-stu-id="f58fd-104">When first enabling automatic provisioning for an application, hello initial sync can take anywhere from 20 minutes tooseveral hours, depending on hello size of hello Azure AD directory and hello number of users in scope for provisioning.</span></span> 

<span data-ttu-id="f58fd-105">Efterföljande synkroniseringar efter hello inledande synkronisering vara snabbare som hello Etablerar tjänsten lagrar vattenstämplar som representerar hello tillståndet för båda systemen efter hello inledande synkronisering, förbättra prestanda för efterföljande synkronisering.</span><span class="sxs-lookup"><span data-stu-id="f58fd-105">Subsequent syncs after hello initial sync be faster, as hello provisioning service stores watermarks that represent hello state of both systems after hello initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-tooimprove-provisioning-performance"></a><span data-ttu-id="f58fd-106">Hur tooimprove etablering prestanda</span><span class="sxs-lookup"><span data-stu-id="f58fd-106">How tooimprove provisioning performance</span></span>

<span data-ttu-id="f58fd-107">Om hello inledande synkronisering tar flera timmar, finns det en sak som du kan göra tooimprove prestanda:</span><span class="sxs-lookup"><span data-stu-id="f58fd-107">If hello initial sync is taking more than a few hours, there is one thing you can do tooimprove performance:</span></span>

-   <span data-ttu-id="f58fd-108">**Målgrupp Användarfilter.**</span><span class="sxs-lookup"><span data-stu-id="f58fd-108">**User scoping filters.**</span></span> <span data-ttu-id="f58fd-109">Målgrupp filter kan du toofine finjustera hello data som hello etablering service utdrag ur Azure AD genom att filtrera bort användare baserat på specifika attributvärden.</span><span class="sxs-lookup"><span data-stu-id="f58fd-109">Scoping filters allow you toofine tune hello data that hello provisioning service extracts from Azure AD by filtering out users based on specific attribute values.</span></span> <span data-ttu-id="f58fd-110">Mer information om Omfångsfilter finns [attributbaserad programmet etablering med Omfångsfilter](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span><span class="sxs-lookup"><span data-stu-id="f58fd-110">For more information on scoping filters, see [Attribute-based application provisioning with scoping filters](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f58fd-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f58fd-111">Next steps</span></span>
[<span data-ttu-id="f58fd-112">Automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f58fd-112">Automate User Provisioning and Deprovisioning tooSaaS Applications with Azure Active Directory</span></span>](active-directory-saas-app-provisioning.md)

