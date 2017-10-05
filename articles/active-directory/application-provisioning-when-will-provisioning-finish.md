---
title: "Användaretablering till ett program för Azure AD-galleriet är ta timmar eller mer | Microsoft Docs"
description: "Ta reda på varför etablering i tillämpningsprogrammet kan ta längre tid än väntat"
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
ms.openlocfilehash: 183d60cbbbc8d02f7cd3cacc160453c45717ef0d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a><span data-ttu-id="79645-103">Användaretablering till ett program för Azure AD-galleriet är ta timmar eller mer</span><span class="sxs-lookup"><span data-stu-id="79645-103">User provisioning to an Azure AD Gallery application is taking hours or more</span></span>

<span data-ttu-id="79645-104">När du först aktiverar automatisk etablering för ett program, kan den första synkroniseringen ta allt från 20 minuter till flera timmar, beroende på storleken på Azure AD-katalog och antalet användare i omfånget för etablering.</span><span class="sxs-lookup"><span data-stu-id="79645-104">When first enabling automatic provisioning for an application, the initial sync can take anywhere from 20 minutes to several hours, depending on the size of the Azure AD directory and the number of users in scope for provisioning.</span></span> 

<span data-ttu-id="79645-105">Efterföljande synkroniseringar efter den inledande synkroniseringen att snabbare, eftersom etablering tjänsten lagrar vattenstämplar som representerar tillståndet för båda systemen efter den första synkroniseringen, förbättra prestanda för efterföljande synkroniseringar.</span><span class="sxs-lookup"><span data-stu-id="79645-105">Subsequent syncs after the initial sync be faster, as the provisioning service stores watermarks that represent the state of both systems after the initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-to-improve-provisioning-performance"></a><span data-ttu-id="79645-106">Hur vi kan förbättra prestanda för etablering</span><span class="sxs-lookup"><span data-stu-id="79645-106">How to improve provisioning performance</span></span>

<span data-ttu-id="79645-107">Om den första synkroniseringen tar flera timmar, finns det en sak som du kan göra för att förbättra prestanda:</span><span class="sxs-lookup"><span data-stu-id="79645-107">If the initial sync is taking more than a few hours, there is one thing you can do to improve performance:</span></span>

-   <span data-ttu-id="79645-108">**Målgrupp Användarfilter.**</span><span class="sxs-lookup"><span data-stu-id="79645-108">**User scoping filters.**</span></span> <span data-ttu-id="79645-109">Målgrupp filter kan du fininställa de data som tjänsten etablering extraheras från Azure AD genom att filtrera bort användare baserat på specifika attributvärden.</span><span class="sxs-lookup"><span data-stu-id="79645-109">Scoping filters allow you to fine tune the data that the provisioning service extracts from Azure AD by filtering out users based on specific attribute values.</span></span> <span data-ttu-id="79645-110">Mer information om Omfångsfilter finns [attributbaserad programmet etablering med Omfångsfilter](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span><span class="sxs-lookup"><span data-stu-id="79645-110">For more information on scoping filters, see [Attribute-based application provisioning with scoping filters](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="79645-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79645-111">Next steps</span></span>
[<span data-ttu-id="79645-112">Automatisera användaren etablering och avetablering för SaaS-program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="79645-112">Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory</span></span>](active-directory-saas-app-provisioning.md)

