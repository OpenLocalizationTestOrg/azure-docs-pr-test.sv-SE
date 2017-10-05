---
title: "Registrera ett program med Azure AD v2.0-slutpunkten med hjälp av portalen | Microsoft Docs"
description: "Så här registrerar du en app med Microsoft för att aktivera inloggning och åtkomst till Microsoft-tjänster med v2.0-slutpunkten"
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: e6202aa8665c906382666fe08a561421e50e0a8d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a><span data-ttu-id="bf4a9-103">Hur du registrerar en app med v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="bf4a9-103">How to register an app with the v2.0 endpoint</span></span>
<span data-ttu-id="bf4a9-104">Om du vill skapa en app som accepterar både MSA och Azure AD-inloggning, du först behöver registrera en app med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-104">To build an app that accepts both MSA & Azure AD sign-in, you'll first need to register an app with Microsoft.</span></span>  <span data-ttu-id="bf4a9-105">För närvarande kan du inte använda några befintliga appar som du kan ha med Azure AD eller MSA - du behöver skapa en helt ny.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-105">At this time, you won't be able to use any existing apps you may have with Azure AD or MSA - you'll need to create a brand new one.</span></span>

> [!NOTE]
> <span data-ttu-id="bf4a9-106">Inte alla Azure Active Directory-scenarier och funktioner som stöds av v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-106">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="bf4a9-107">Läs mer om för att avgöra om du ska använda v2.0-slutpunkten [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="bf4a9-107">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="visit-the-microsoft-app-registration-portal"></a><span data-ttu-id="bf4a9-108">Besök portalen för registrering av Microsoft-app</span><span class="sxs-lookup"><span data-stu-id="bf4a9-108">Visit the Microsoft app registration portal</span></span>
<span data-ttu-id="bf4a9-109">Första saker först - gå till [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span><span class="sxs-lookup"><span data-stu-id="bf4a9-109">First things first - navigate to [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span></span>  <span data-ttu-id="bf4a9-110">Det här är en ny app registrering portal där du kan hantera dina Microsoft-appar.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-110">This is a new app registration portal where you can manage your Microsoft apps.</span></span>

<span data-ttu-id="bf4a9-111">Logga in med antingen en personlig eller arbets- eller skolkonto för Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-111">Sign in with either a personal or work or school Microsoft account.</span></span>  <span data-ttu-id="bf4a9-112">Om du inte har antingen registrera dig för ett nytt personligt konto.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-112">If you don't have either, sign up for a new personal account.</span></span> <span data-ttu-id="bf4a9-113">Gå vidare, det tar inte lång - vi väntar här.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-113">Go ahead, it won't take long - we'll wait here.</span></span>

<span data-ttu-id="bf4a9-114">Vill du göra?</span><span class="sxs-lookup"><span data-stu-id="bf4a9-114">Done?</span></span> <span data-ttu-id="bf4a9-115">Du bör nu tittar på listan över Microsoft-appar som är förmodligen tom.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-115">You should now be looking at your list of Microsoft apps, which is probably empty.</span></span>  <span data-ttu-id="bf4a9-116">Ska vi ändra.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-116">Let's change that.</span></span>

<span data-ttu-id="bf4a9-117">Klicka på **Lägg till en app**, och ge det ett namn.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-117">Click **Add an app**, and give it a name.</span></span>  <span data-ttu-id="bf4a9-118">Portalen kommer att tilldela din app ett globalt unikt Id för programmet som du vill använda senare i koden.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-118">The portal will assign your app a globally unique  Application Id that you'll use later in your code.</span></span>  <span data-ttu-id="bf4a9-119">Om appen innehåller en komponent på serversidan som behöver åtkomst-token för anropa API: er (tror: Office, Azure eller egna webb-API), ska du skapa en **Programhemlighet** även här.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-119">If your app includes a server-side component that needs access tokens for calling APIs (think: Office, Azure, or your own web API), you'll want to create an **Application Secret** here as well.</span></span>

<span data-ttu-id="bf4a9-120">Lägg till de plattformar som ska använda för din app.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-120">Next, add the Platforms that your app will use.</span></span>

* <span data-ttu-id="bf4a9-121">Webbaserade appar, ange en **omdirigerings-URI** där inloggning meddelanden kan skickas.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-121">For web based apps, provide a **Redirect URI** where sign-in messages can be sent.</span></span>
* <span data-ttu-id="bf4a9-122">För mobila appar, kopiera fördefinierade omdirigerings-uri som skapats automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-122">For mobile apps, copy down the default redirect uri automatically created for you.</span></span>

<span data-ttu-id="bf4a9-123">Du kan också kan du anpassa utseendet och känslan av inloggningssidan under profil.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-123">Optionally, you can customize the look and feel of your sign-in page in the Profile section.</span></span>  <span data-ttu-id="bf4a9-124">Se till att klicka på **spara** innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-124">Make sure to click **Save** before moving on.</span></span>

> [!NOTE]
> <span data-ttu-id="bf4a9-125">När du skapar ett program som använder [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), programmet kommer att registreras i hem-klient för det konto som du använder för att logga in på portalen.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-125">When you create an application using [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), the application will be registered in the home tenant of the account that you use to sign into the portal.</span></span>  <span data-ttu-id="bf4a9-126">Det innebär att du inte registrera ett program i Azure AD-klienten med ett personligt microsoftkonto.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-126">This means that you can not register an application in your Azure AD tenant using a personal Microsoft account.</span></span>  <span data-ttu-id="bf4a9-127">Om du uttryckligen vill registrera ett program i en viss klient logga in med ett konto som ursprungligen skapades i den klienten.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-127">If you explicitly wish to register an application in a particular tenant, sign in with an account originally created in that tenant.</span></span>
> 
> 

## <a name="build-a-quick-start-app"></a><span data-ttu-id="bf4a9-128">Skapa en app för Snabbstart</span><span class="sxs-lookup"><span data-stu-id="bf4a9-128">Build a quick start app</span></span>
<span data-ttu-id="bf4a9-129">Nu när du har en Microsoft-app kan du slutföra en av våra självstudier för v2.0-Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="bf4a9-129">Now that you have a Microsoft app, you can complete one of our v2.0 quick start tutorials.</span></span>  <span data-ttu-id="bf4a9-130">Här är några rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="bf4a9-130">Here are a few recommendations:</span></span>

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

