---
title: "Kom igång med Azure AD i Visual Studio-WebApi projekt | Microsoft Docs"
description: "Hur du kommer igång med Azure Active Directory i WebApi projekt när du ansluter till eller skapa en Azure AD med hjälp av Visual Studio anslutna tjänster"
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
ms.openlocfilehash: a756316054dd3bb63f3b0a9f59621bb1345bc693
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a><span data-ttu-id="dc5bd-103">Komma igång med Azure Active Directory och Visual Studio anslutna tjänster (WebApi-projekt)</span><span class="sxs-lookup"><span data-stu-id="dc5bd-103">Get Started with Azure Active Directory and Visual Studio connected services (WebApi projects)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dc5bd-104">Komma igång</span><span class="sxs-lookup"><span data-stu-id="dc5bd-104">Getting Started</span></span>](vs-active-directory-webapi-getting-started.md)
> * [<span data-ttu-id="dc5bd-105">Vad hände</span><span class="sxs-lookup"><span data-stu-id="dc5bd-105">What Happened</span></span>](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="requiring-authentication-to-access-controllers"></a><span data-ttu-id="dc5bd-106">Kräva autentisering till access-domänkontrollanter</span><span class="sxs-lookup"><span data-stu-id="dc5bd-106">Requiring authentication to access controllers</span></span>
<span data-ttu-id="dc5bd-107">Alla domänkontrollanter i ditt projekt har adorned med den **auktorisera** attribut.</span><span class="sxs-lookup"><span data-stu-id="dc5bd-107">All controllers in your project were adorned with the **Authorize** attribute.</span></span> <span data-ttu-id="dc5bd-108">Det här attributet kräver att användaren autentiseras innan du använder API: er som definieras av dessa domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="dc5bd-108">This attribute requires the user to be authenticated before accessing the APIs defined by these controllers.</span></span> <span data-ttu-id="dc5bd-109">Ta bort skrivskyddsattributet från styrenheten för att tillåta att kontrollanten kan användas anonymt.</span><span class="sxs-lookup"><span data-stu-id="dc5bd-109">To allow the controller to be accessed anonymously, remove this attribute from the controller.</span></span> <span data-ttu-id="dc5bd-110">Om du vill ange behörigheter på en mer detaljerad nivå gäller attributet för varje metod som kräver tillstånd i stället för att tillämpas på klassen domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="dc5bd-110">If you want to set the permissions at a more granular level, apply the attribute to each method that requires authorization instead of applying it to the controller class.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc5bd-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc5bd-111">Next steps</span></span>
[<span data-ttu-id="dc5bd-112">Lär dig mer om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dc5bd-112">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

