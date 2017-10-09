---
title: "aaaHow toogrant behörigheter tooa anpassad utvecklat program | Microsoft Docs"
description: "Hur toogrant behörigheter tooyour anpassad utvecklat program med hjälp av hello Azure AD-portalen eller en URL-parameter"
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
ms.openlocfilehash: e43a105fff60fbf912bdf4f60260f86ee289328d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogrant-permissions-tooa-custom-developed-application"></a><span data-ttu-id="c7640-103">Hur toogrant behörigheter tooa anpassad utvecklade program</span><span class="sxs-lookup"><span data-stu-id="c7640-103">How toogrant permissions tooa custom-developed application</span></span>

<span data-ttu-id="c7640-104">Om du vill toogrant medgivande förebyggande syfte på din app eller kör i ett fel som du inte har samtyckt tooan app, kan du försöka stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="c7640-104">If you want toogrant consent preemptively on your app or are running into an error that you have not consented tooan app, try these steps below.</span></span>

## <a name="how-tooperform-admin-consent-for-your-application"></a><span data-ttu-id="c7640-105">Hur tooperform Admin medgivande för ditt program</span><span class="sxs-lookup"><span data-stu-id="c7640-105">How tooperform Admin Consent for your application</span></span>

<span data-ttu-id="c7640-106">Detta påverkar hello att bevilja tillstånd toohello program för alla användare i din organisation.</span><span class="sxs-lookup"><span data-stu-id="c7640-106">This has hello effect of granting consent toohello application for all users in your organization.</span></span>

1. <span data-ttu-id="c7640-107">Navigera toohello **App registreringar** bladet som en **global administratör**och välj hello app.</span><span class="sxs-lookup"><span data-stu-id="c7640-107">Navigate toohello **App Registrations** blade as a **global administrator**, then select hello app.</span></span>

2. <span data-ttu-id="c7640-108">Välj **nödvändiga behörigheter**, och slutligen tryck hello **bevilja med** knappen hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="c7640-108">Select **Required Permissions**, and finally hit hello **Grant Permissions** button at hello top of hello blade.</span></span>

<span data-ttu-id="c7640-109">Alternativt kan du skapa en begäran för*login.microsoftonline.com* med konfigurationerna för appen och Lägg till på *& fråga = admin\_medgivande*.</span><span class="sxs-lookup"><span data-stu-id="c7640-109">Alternatively, you can construct a request too*login.microsoftonline.com* with your app configs and append on *&prompt=admin\_consent*.</span></span> <span data-ttu-id="c7640-110">Hello app har beviljats godkännande för alla användare för när de har loggat in med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c7640-110">After signing in with admin credentials, hello app has been granted consent for all users.</span></span>

## <a name="how-tooforce-user-consent-for-your-application"></a><span data-ttu-id="c7640-111">Hur tooforce användarens medgivande för ditt program</span><span class="sxs-lookup"><span data-stu-id="c7640-111">How tooforce User Consent for your application</span></span>

* <span data-ttu-id="c7640-112">Lägga till auth-begäranden *& fråga = medgivande* som kräver att slutanvändarna tooconsent varje gång de autentiserar.</span><span class="sxs-lookup"><span data-stu-id="c7640-112">Append onto auth requests *&prompt=consent* which require end users tooconsent each time they authenticate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c7640-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c7640-113">Next steps</span></span>

[<span data-ttu-id="c7640-114">Medgivande och integrera appar tooAzureAD</span><span class="sxs-lookup"><span data-stu-id="c7640-114">Consent and Integrating Apps tooAzureAD</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[<span data-ttu-id="c7640-115">Medgivande och Permissioning för AzureAD v2.0 konvergerat appar</span><span class="sxs-lookup"><span data-stu-id="c7640-115">Consent and Permissioning for AzureAD v2.0 converged Apps</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="c7640-116">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="c7640-116">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
