---
title: "aaaGet igång med Azure AD i Visual Studio-WebApi projekt | Microsoft Docs"
description: "Hur tooget igång med Azure Active Directory i WebApi projekt efter anslutning tooor skapa en Azure AD med hjälp av Visual Studio anslutna tjänster"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/19/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 21413a71a2fd61f31268bf6d5e4d86b8be5bd16a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a><span data-ttu-id="e27ae-103">Komma igång med Azure Active Directory och Visual Studio anslutna tjänster (WebApi-projekt)</span><span class="sxs-lookup"><span data-stu-id="e27ae-103">Get Started with Azure Active Directory and Visual Studio connected services (WebApi projects)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e27ae-104">Komma igång</span><span class="sxs-lookup"><span data-stu-id="e27ae-104">Getting Started</span></span>](vs-active-directory-webapi-getting-started.md)
> * [<span data-ttu-id="e27ae-105">Vad hände</span><span class="sxs-lookup"><span data-stu-id="e27ae-105">What Happened</span></span>](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="requiring-authentication-tooaccess-controllers"></a><span data-ttu-id="e27ae-106">Kräver autentisering tooaccess domänkontrollanter</span><span class="sxs-lookup"><span data-stu-id="e27ae-106">Requiring authentication tooaccess controllers</span></span>
<span data-ttu-id="e27ae-107">Alla domänkontrollanter i ditt projekt har adorned med hello **auktorisera** attribut.</span><span class="sxs-lookup"><span data-stu-id="e27ae-107">All controllers in your project were adorned with hello **Authorize** attribute.</span></span> <span data-ttu-id="e27ae-108">Det här attributet kräver hello användaren toobe autentiserad åtkomst till hello API: er definieras av dessa domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="e27ae-108">This attribute requires hello user toobe authenticated before accessing hello APIs defined by these controllers.</span></span> <span data-ttu-id="e27ae-109">tooallow hello controller toobe ansluta anonymt, ta bort skrivskyddsattributet från hello domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="e27ae-109">tooallow hello controller toobe accessed anonymously, remove this attribute from hello controller.</span></span> <span data-ttu-id="e27ae-110">Om du vill tooset hello behörigheter på en mer detaljerad nivå gäller hello attributet tooeach metoden som kräver tillstånd i stället för att tillämpa den toohello kontrollantklassen.</span><span class="sxs-lookup"><span data-stu-id="e27ae-110">If you want tooset hello permissions at a more granular level, apply hello attribute tooeach method that requires authorization instead of applying it toohello controller class.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e27ae-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e27ae-111">Next steps</span></span>
[<span data-ttu-id="e27ae-112">Lär dig mer om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e27ae-112">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

