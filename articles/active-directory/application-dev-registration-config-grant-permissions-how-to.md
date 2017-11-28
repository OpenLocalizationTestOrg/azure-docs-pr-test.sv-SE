---
title: "Ge behörigheter till ett anpassat utvecklade program | Microsoft Docs"
description: "Ge behörigheter till ditt anpassade utvecklade program med hjälp av Azure AD-portalen eller en URL-parameter"
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
ms.openlocfilehash: 336b945929f80e1a566f7cf71b40fd799a98c12d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a><span data-ttu-id="3c7ab-103">Ge behörigheter till ett anpassat utvecklade program</span><span class="sxs-lookup"><span data-stu-id="3c7ab-103">How to grant permissions to a custom-developed application</span></span>

<span data-ttu-id="3c7ab-104">Försök stegen nedan om du vill ge medgivande förebyggande syfte på din app eller körs i ett fel som du inte har samtyckt till en app.</span><span class="sxs-lookup"><span data-stu-id="3c7ab-104">If you want to grant consent preemptively on your app or are running into an error that you have not consented to an app, try these steps below.</span></span>

## <a name="how-to-perform-admin-consent-for-your-application"></a><span data-ttu-id="3c7ab-105">Hur du utför Admin medgivande för ditt program</span><span class="sxs-lookup"><span data-stu-id="3c7ab-105">How to perform Admin Consent for your application</span></span>

<span data-ttu-id="3c7ab-106">Effekten av att godkänna programmet för alla användare i din organisation har.</span><span class="sxs-lookup"><span data-stu-id="3c7ab-106">This has the effect of granting consent to the application for all users in your organization.</span></span>

1. <span data-ttu-id="3c7ab-107">Navigera till den **App registreringar** bladet som en **global administratör**, Välj appen.</span><span class="sxs-lookup"><span data-stu-id="3c7ab-107">Navigate to the **App Registrations** blade as a **global administrator**, then select the app.</span></span>

2. <span data-ttu-id="3c7ab-108">Välj **nödvändiga behörigheter**, och slutligen nådde den **bevilja med** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="3c7ab-108">Select **Required Permissions**, and finally hit the **Grant Permissions** button at the top of the blade.</span></span>

<span data-ttu-id="3c7ab-109">Alternativt kan du skapa en begäran om att *login.microsoftonline.com* med konfigurationerna för appen och Lägg till på *& fråga = admin\_medgivande*.</span><span class="sxs-lookup"><span data-stu-id="3c7ab-109">Alternatively, you can construct a request to *login.microsoftonline.com* with your app configs and append on *&prompt=admin\_consent*.</span></span> <span data-ttu-id="3c7ab-110">Appen har beviljats godkännande för alla användare för när de har loggat in med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3c7ab-110">After signing in with admin credentials, the app has been granted consent for all users.</span></span>

## <a name="how-to-force-user-consent-for-your-application"></a><span data-ttu-id="3c7ab-111">Tvinga användaren medgivande för ditt program</span><span class="sxs-lookup"><span data-stu-id="3c7ab-111">How to force User Consent for your application</span></span>

* <span data-ttu-id="3c7ab-112">Lägga till auth-begäranden *& fråga = medgivande* som kräver tillstånd varje gång de autentiserar användare.</span><span class="sxs-lookup"><span data-stu-id="3c7ab-112">Append onto auth requests *&prompt=consent* which require end users to consent each time they authenticate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c7ab-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c7ab-113">Next steps</span></span>

[<span data-ttu-id="3c7ab-114">Medgivande och integrera appar till AzureAD</span><span class="sxs-lookup"><span data-stu-id="3c7ab-114">Consent and Integrating Apps to AzureAD</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[<span data-ttu-id="3c7ab-115">Medgivande och Permissioning för AzureAD v2.0 konvergerat appar</span><span class="sxs-lookup"><span data-stu-id="3c7ab-115">Consent and Permissioning for AzureAD v2.0 converged Apps</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="3c7ab-116">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="3c7ab-116">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
