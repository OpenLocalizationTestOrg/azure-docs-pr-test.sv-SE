---
title: "aaaHow toofind specifika API: et behövs för ett anpassat utvecklade program | Microsoft Docs"
description: "Hur tooconfigure hello behörigheter som du behöver tooaccess ett visst API i dina anpassade utvecklade Azure AD-program"
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
ms.openlocfilehash: 7331129204d8b34b4ef9671749bd702f893768ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-a-specific-api-needed-for-a-custom-developed-application"></a><span data-ttu-id="e2839-103">Hur toofind en specifika API: et behövs för ett anpassat utvecklade program</span><span class="sxs-lookup"><span data-stu-id="e2839-103">How toofind a specific API needed for a custom-developed application</span></span>

<span data-ttu-id="e2839-104">Åtkomst tooAPIs kräver konfiguration av åtkomstscope och roller.</span><span class="sxs-lookup"><span data-stu-id="e2839-104">Access tooAPIs require configuration of access scopes and roles.</span></span> <span data-ttu-id="e2839-105">Om du vill tooexpose resurs programmet API: er tooclient webbprogrammen, behöver du tooconfigure åtkomstscope och roller för hello API.</span><span class="sxs-lookup"><span data-stu-id="e2839-105">If you want tooexpose your resource application web APIs tooclient applications, you need tooconfigure access scopes and roles for hello API.</span></span> <span data-ttu-id="e2839-106">Om du vill att en klient programmet tooaccess ett webb-API, måste tooconfigure behörigheter tooaccess hello API i hello app registreringen.</span><span class="sxs-lookup"><span data-stu-id="e2839-106">If you want a client application tooaccess a web API, you need tooconfigure permissions tooaccess hello API in hello app registration.</span></span>

## <a name="configuring-a-resource-application-tooexpose-web-apis"></a><span data-ttu-id="e2839-107">Konfigurera en resurs programmet tooexpose-webb-API: er</span><span class="sxs-lookup"><span data-stu-id="e2839-107">Configuring a resource application tooexpose web APIs</span></span>

<span data-ttu-id="e2839-108">När du exponera ditt webb-API, hello API visas i hello **väljer en API** listan när du lägger till behörigheter tooan appregistrering.</span><span class="sxs-lookup"><span data-stu-id="e2839-108">When you expose your web API, hello API be displayed in hello **Select an API** list when adding permissions tooan app registration.</span></span> <span data-ttu-id="e2839-109">tooadd åtkomstscope följer hello steg som beskrivs i [att lägga till access scope tooyour resursprogram](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).</span><span class="sxs-lookup"><span data-stu-id="e2839-109">tooadd access scopes, follow hello steps outlined in [adding access scopes tooyour resource application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).</span></span>

## <a name="configuring-a-client-application-tooaccess-web-apis"></a><span data-ttu-id="e2839-110">Konfigurera en klient programmet tooaccess-webb-API: er</span><span class="sxs-lookup"><span data-stu-id="e2839-110">Configuring a client application tooaccess web APIs</span></span>

<span data-ttu-id="e2839-111">När du lägger till behörigheter tooyour appregistrering, kan du **lägga till API-åtkomst** tooexposed web API: er.</span><span class="sxs-lookup"><span data-stu-id="e2839-111">When you add permissions tooyour app registration, you can **add API access** tooexposed web APIs.</span></span> <span data-ttu-id="e2839-112">tooaccess webb-API: er, Följ stegen i hello [lägga till autentiseringsuppgifter eller behörigheter tooaccess webb-API: er](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).</span><span class="sxs-lookup"><span data-stu-id="e2839-112">tooaccess web APIs, follow hello steps outlined in [add credentials or permissions tooaccess web APIs](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2839-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e2839-113">Next steps</span></span>

-   [<span data-ttu-id="e2839-114">Integrera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e2839-114">Integrating applications with Azure Active Directory</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [<span data-ttu-id="e2839-115">Förstå hello Azure Active Directory-programmanifestet</span><span class="sxs-lookup"><span data-stu-id="e2839-115">Understanding hello Azure Active Directory application manifest</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


