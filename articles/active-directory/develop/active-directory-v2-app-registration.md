---
title: "aaaRegister ett program med hello Azure AD v2.0-slutpunkten med hjälp av hello portal | Microsoft Docs"
description: "Hur tooregister en app med Microsoft för att aktivera inloggning och åtkomst till Microsoft tjänster som använder hello v2.0-slutpunkten"
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
ms.openlocfilehash: c56c98906656062435516e820cb318a04c03149c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooregister-an-app-with-hello-v20-endpoint"></a><span data-ttu-id="1c275-103">Hur tooregister en app med hello v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="1c275-103">How tooregister an app with hello v2.0 endpoint</span></span>
<span data-ttu-id="1c275-104">toobuild en app som accepterar både MSA och Azure AD-inloggning, behöver du tooregister en app med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1c275-104">toobuild an app that accepts both MSA & Azure AD sign-in, you'll first need tooregister an app with Microsoft.</span></span>  <span data-ttu-id="1c275-105">För tillfället inte att kunna toouse alla befintliga appar som du kan ha med Azure AD eller MSA - du behöver toocreate varumärken ny.</span><span class="sxs-lookup"><span data-stu-id="1c275-105">At this time, you won't be able toouse any existing apps you may have with Azure AD or MSA - you'll need toocreate a brand new one.</span></span>

> [!NOTE]
> <span data-ttu-id="1c275-106">Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="1c275-106">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="1c275-107">toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="1c275-107">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="visit-hello-microsoft-app-registration-portal"></a><span data-ttu-id="1c275-108">Besök portalen för registrering av hello Microsoft app</span><span class="sxs-lookup"><span data-stu-id="1c275-108">Visit hello Microsoft app registration portal</span></span>
<span data-ttu-id="1c275-109">Första saker först - navigera för[https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span><span class="sxs-lookup"><span data-stu-id="1c275-109">First things first - navigate too[https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span></span>  <span data-ttu-id="1c275-110">Det här är en ny app registrering portal där du kan hantera dina Microsoft-appar.</span><span class="sxs-lookup"><span data-stu-id="1c275-110">This is a new app registration portal where you can manage your Microsoft apps.</span></span>

<span data-ttu-id="1c275-111">Logga in med antingen en personlig eller arbets- eller skolkonto för Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1c275-111">Sign in with either a personal or work or school Microsoft account.</span></span>  <span data-ttu-id="1c275-112">Om du inte har antingen registrera dig för ett nytt personligt konto.</span><span class="sxs-lookup"><span data-stu-id="1c275-112">If you don't have either, sign up for a new personal account.</span></span> <span data-ttu-id="1c275-113">Gå vidare, det tar inte lång - vi väntar här.</span><span class="sxs-lookup"><span data-stu-id="1c275-113">Go ahead, it won't take long - we'll wait here.</span></span>

<span data-ttu-id="1c275-114">Vill du göra?</span><span class="sxs-lookup"><span data-stu-id="1c275-114">Done?</span></span> <span data-ttu-id="1c275-115">Du bör nu tittar på listan över Microsoft-appar som är förmodligen tom.</span><span class="sxs-lookup"><span data-stu-id="1c275-115">You should now be looking at your list of Microsoft apps, which is probably empty.</span></span>  <span data-ttu-id="1c275-116">Ska vi ändra.</span><span class="sxs-lookup"><span data-stu-id="1c275-116">Let's change that.</span></span>

<span data-ttu-id="1c275-117">Klicka på **Lägg till en app**, och ge det ett namn.</span><span class="sxs-lookup"><span data-stu-id="1c275-117">Click **Add an app**, and give it a name.</span></span>  <span data-ttu-id="1c275-118">hello portal tilldelar appen ett globalt unikt Id för programmet som du vill använda senare i koden.</span><span class="sxs-lookup"><span data-stu-id="1c275-118">hello portal will assign your app a globally unique  Application Id that you'll use later in your code.</span></span>  <span data-ttu-id="1c275-119">Om appen innehåller en komponent på serversidan som behöver åtkomst-token för anropa API: er (tror: Office, Azure eller egna webb-API), ska du toocreate en **Programhemlighet** även här.</span><span class="sxs-lookup"><span data-stu-id="1c275-119">If your app includes a server-side component that needs access tokens for calling APIs (think: Office, Azure, or your own web API), you'll want toocreate an **Application Secret** here as well.</span></span>

<span data-ttu-id="1c275-120">Lägg till hello plattformar som ska använda för din app.</span><span class="sxs-lookup"><span data-stu-id="1c275-120">Next, add hello Platforms that your app will use.</span></span>

* <span data-ttu-id="1c275-121">Webbaserade appar, ange en **omdirigerings-URI** där inloggning meddelanden kan skickas.</span><span class="sxs-lookup"><span data-stu-id="1c275-121">For web based apps, provide a **Redirect URI** where sign-in messages can be sent.</span></span>
* <span data-ttu-id="1c275-122">För mobila appar kopiera hello standard omdirigerings-uri som skapats automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="1c275-122">For mobile apps, copy down hello default redirect uri automatically created for you.</span></span>

<span data-ttu-id="1c275-123">Du kan också kan du anpassa hello utseendet på inloggningssidan i hello-profilen.</span><span class="sxs-lookup"><span data-stu-id="1c275-123">Optionally, you can customize hello look and feel of your sign-in page in hello Profile section.</span></span>  <span data-ttu-id="1c275-124">Se till att tooclick **spara** innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="1c275-124">Make sure tooclick **Save** before moving on.</span></span>

> [!NOTE]
> <span data-ttu-id="1c275-125">När du skapar ett program som använder [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), hello program registreras i hello hem innehavare för hello kontot du använder toosign i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="1c275-125">When you create an application using [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), hello application will be registered in hello home tenant of hello account that you use toosign into hello portal.</span></span>  <span data-ttu-id="1c275-126">Det innebär att du inte registrera ett program i Azure AD-klienten med ett personligt microsoftkonto.</span><span class="sxs-lookup"><span data-stu-id="1c275-126">This means that you can not register an application in your Azure AD tenant using a personal Microsoft account.</span></span>  <span data-ttu-id="1c275-127">Om du uttryckligen vill tooregister ett program i en viss klient måste du logga in med ett konto som ursprungligen skapades i den klienten.</span><span class="sxs-lookup"><span data-stu-id="1c275-127">If you explicitly wish tooregister an application in a particular tenant, sign in with an account originally created in that tenant.</span></span>
> 
> 

## <a name="build-a-quick-start-app"></a><span data-ttu-id="1c275-128">Skapa en app för Snabbstart</span><span class="sxs-lookup"><span data-stu-id="1c275-128">Build a quick start app</span></span>
<span data-ttu-id="1c275-129">Nu när du har en Microsoft-app kan du slutföra en av våra självstudier för v2.0-Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="1c275-129">Now that you have a Microsoft app, you can complete one of our v2.0 quick start tutorials.</span></span>  <span data-ttu-id="1c275-130">Här är några rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1c275-130">Here are a few recommendations:</span></span>

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

