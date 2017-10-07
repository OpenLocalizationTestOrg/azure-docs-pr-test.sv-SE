---
title: aaaProblems inloggning tooan utvecklade anpassade program | Microsoft Docs
description: Vanliga rrors som kan orsaka du toonot vara kan toosign till ett program som du har utvecklat med Azure AD
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
ms.openlocfilehash: cc302e68ae6c129b74387c6fc5ba4fb45ccb8fb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-custom-developed-application"></a><span data-ttu-id="24f63-103">Problem med att logga i tooan utvecklade anpassade program</span><span class="sxs-lookup"><span data-stu-id="24f63-103">Problems signing in tooan custom-developed application</span></span>

<span data-ttu-id="24f63-104">Det finns flera fel som kan orsaka du toonot vara kan toosign i en app.</span><span class="sxs-lookup"><span data-stu-id="24f63-104">There are several errors that could be causing you toonot be able toosign into an app.</span></span> <span data-ttu-id="24f63-105">hello största orsaken personer får det här problemet är felkonfigurerad appar.</span><span class="sxs-lookup"><span data-stu-id="24f63-105">hello biggest reason people encounter this problem is misconfigured apps.</span></span>

## <a name="errors-related-too-misconfigured-apps"></a><span data-ttu-id="24f63-106">Fel relaterade för felkonfigurerad appar</span><span class="sxs-lookup"><span data-stu-id="24f63-106">Errors related too misconfigured apps</span></span>

* <span data-ttu-id="24f63-107">Kontrollera att både hello konfigurationer i hello portal matchar vad du har i din app.</span><span class="sxs-lookup"><span data-stu-id="24f63-107">Verify both hello configurations in hello portal match what you have in your app.</span></span> <span data-ttu-id="24f63-108">Mer specifikt jämför klient/program-ID, svars-URL: er, klienten hemligheter/nycklar och App-ID-URI.</span><span class="sxs-lookup"><span data-stu-id="24f63-108">Specifically, compare Client/Application ID, Reply URLs, Client Secrets/Keys, and App ID URI.</span></span>

* <span data-ttu-id="24f63-109">Jämför hello-resurs som du ska få åtkomst till tooin kod med hello konfigurerats behörigheter i hello **krävs resurser** fliken toomake att du bara begära resurser som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="24f63-109">Compare hello resource you’re requesting access tooin code with hello configured permissions in hello **Required Resources** tab toomake sure you only request resources you’ve configured.</span></span>

* <span data-ttu-id="24f63-110">Se [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) för alla liknande fel eller problem.</span><span class="sxs-lookup"><span data-stu-id="24f63-110">See [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) for any similar errors or problems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24f63-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24f63-111">Next steps</span></span>

[<span data-ttu-id="24f63-112">Utvecklarhandbok för Azure AD</span><span class="sxs-lookup"><span data-stu-id="24f63-112">Azure AD Developer Guide</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[<span data-ttu-id="24f63-113">Medgivande och integrera appar tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="24f63-113">Consent and Integrating Apps tooAzure AD</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[<span data-ttu-id="24f63-114">Medgivande och Permissioning för Azure AD v2.0 konvergerat appar</span><span class="sxs-lookup"><span data-stu-id="24f63-114">Consent and Permissioning for Azure AD v2.0 converged Apps</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="24f63-115">Azure AD-StackOverflow</span><span class="sxs-lookup"><span data-stu-id="24f63-115">Azure AD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory>)
