---
title: Azure AD v2.0 universell Windows-App | Microsoft Docs
description: "Hur du skapar en universella Windows-app som loggar användarna in med både personliga Account och arbets-eller skolkonton."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: d2c92b65-3c1d-46d1-81c8-88f32f6b2c4b
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.date: 02/20/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 369802f1a42b8720aa730d5ac7e5576ed20eeddf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-a-windows-universal-app-using-the-v20-endpoint"></a><span data-ttu-id="84a95-103">Lägga till inloggning till en universell Windows-app med v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="84a95-103">Add sign-in to a Windows Universal app using the v2.0 endpoint</span></span>
  <span data-ttu-id="84a95-104">Snabbkurs i hur för universella Windows-appar inte är helt klar... Kom tillbaka snart & titta efter uppdateringar från @AzureAD på Twitter.</span><span class="sxs-lookup"><span data-stu-id="84a95-104">The quick-start tutorial for Windows Universal apps isn't quite ready... Check back soon & look for updates from @AzureAD on Twitter.</span></span>

> [!NOTE]
> <span data-ttu-id="84a95-105">Inte alla Azure Active Directory-scenarier och funktioner som stöds av v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="84a95-105">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="84a95-106">Läs mer om för att avgöra om du ska använda v2.0-slutpunkten [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="84a95-106">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

    ## Get security updates for our products

<span data-ttu-id="84a95-107">Vi rekommenderar att du aktiverar aviseringar om säkerhetsincidenter genom att gå till [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera på Microsoft Security Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="84a95-107">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

