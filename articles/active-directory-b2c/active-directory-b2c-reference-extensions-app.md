---
title: "Tillägg app – Azure AD B2C | Microsoft Docs"
description: "Återställa hello b2c-tillägg-app"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f0392e32-0771-473c-a799-81438ca2bcff
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/17/2017
ms.author: parja
ms.openlocfilehash: b36410c18314bd893dc669b49814fdcd77fae054
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-extensions-app"></a><span data-ttu-id="32840-103">Azure AD B2C: Tillägg app</span><span class="sxs-lookup"><span data-stu-id="32840-103">Azure AD B2C: Extensions app</span></span>

<span data-ttu-id="32840-104">När en Azure AD B2C-katalog har skapats kan en app kallas `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` skapas automatiskt i hello ny katalog.</span><span class="sxs-lookup"><span data-stu-id="32840-104">When an Azure AD B2C directory is created, an app called `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` is automatically created inside hello new directory.</span></span> <span data-ttu-id="32840-105">Den här appen, enligt tooas hello **b2c-tillägg-app**, visas i *App registreringar*.</span><span class="sxs-lookup"><span data-stu-id="32840-105">This app, referred tooas hello **b2c-extensions-app**, is visible in *App registrations*.</span></span> <span data-ttu-id="32840-106">Den används av hello Azure AD B2C-tjänsten toostore information om användare och anpassade attribut.</span><span class="sxs-lookup"><span data-stu-id="32840-106">It is used by hello Azure AD B2C service toostore information about users and custom attributes.</span></span> <span data-ttu-id="32840-107">Om hello appen tas bort, Azure AD B2C kommer inte att fungera på rätt sätt och produktionsmiljön kommer att påverkas.</span><span class="sxs-lookup"><span data-stu-id="32840-107">If hello app is deleted, Azure AD B2C will not function correctly and your production environment will be affected.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32840-108">Ta inte bort hello b2c-tillägg-app om du planerar att ta bort tooimmediately din klient.</span><span class="sxs-lookup"><span data-stu-id="32840-108">Do not delete hello b2c-extensions-app unless you are planning tooimmediately delete your tenant.</span></span> <span data-ttu-id="32840-109">Hello app är borttagna mer än 30 dagar, kommer användarinformation att gå förlorade.</span><span class="sxs-lookup"><span data-stu-id="32840-109">If hello app remains deleted for more than 30 days, user information will be permanently lost.</span></span>

## <a name="verifying-that-hello-extensions-app-is-present"></a><span data-ttu-id="32840-110">Verifiera hello tillägg appen finns</span><span class="sxs-lookup"><span data-stu-id="32840-110">Verifying that hello extensions app is present</span></span>

<span data-ttu-id="32840-111">Det finns tooverify som hello b2c-tillägg-app:</span><span class="sxs-lookup"><span data-stu-id="32840-111">tooverify that hello b2c-extensions-app is present:</span></span>

1. <span data-ttu-id="32840-112">Klicka på på i din Azure AD B2C-klient **fler tjänster** i hello vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="32840-112">Inside your Azure AD B2C tenant, click on **More services** in hello left-hand navigation menu.</span></span>
1. <span data-ttu-id="32840-113">Söka efter och öppna **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="32840-113">Search for and open **App registrations**.</span></span>
1. <span data-ttu-id="32840-114">Leta efter en app som börjar med **b2c-tillägg-app**</span><span class="sxs-lookup"><span data-stu-id="32840-114">Look for an app that begins with **b2c-extensions-app**</span></span>

## <a name="recover-hello-extensions-app"></a><span data-ttu-id="32840-115">Återställa hello tillägg app</span><span class="sxs-lookup"><span data-stu-id="32840-115">Recover hello extensions app</span></span>

<span data-ttu-id="32840-116">Om du av misstag tas bort hello b2c-tillägg-app du har 30 dagar toorecover den.</span><span class="sxs-lookup"><span data-stu-id="32840-116">If you accidentally deleted hello b2c-extensions-app, you have 30 days toorecover it.</span></span> <span data-ttu-id="32840-117">Du kan återställa hello-app med hello Graph API:</span><span class="sxs-lookup"><span data-stu-id="32840-117">You can restore hello app using hello Graph API:</span></span>

1. <span data-ttu-id="32840-118">Bläddra för[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="32840-118">Browse too[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span></span>
1. <span data-ttu-id="32840-119">Logga in toohello platsen som en global administratör för hello Azure AD B2C-katalog som du vill toorestore hello bort app för.</span><span class="sxs-lookup"><span data-stu-id="32840-119">Log in toohello site as a global administrator for hello Azure AD B2C directory that you want toorestore hello deleted app for.</span></span>
1. <span data-ttu-id="32840-120">Utfärda en HTTP GET mot hello URL `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` = med api-version 1.6.</span><span class="sxs-lookup"><span data-stu-id="32840-120">Issue an HTTP GET against hello URL `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` with api-version=1.6.</span></span> <span data-ttu-id="32840-121">Se till att tooreplace `{tenantName}` med innehavarens namn.</span><span class="sxs-lookup"><span data-stu-id="32840-121">Make sure tooreplace `{tenantName}` with your tenant name.</span></span> <span data-ttu-id="32840-122">Den här åtgärden visar en lista över alla hello-program som har tagits bort inom hello senaste 30 dagarna.</span><span class="sxs-lookup"><span data-stu-id="32840-122">This operation will list all of hello applications that have been deleted within hello past 30 days.</span></span>
1. <span data-ttu-id="32840-123">Hitta hello program i hello lista där hello namn börjar med ”b2c-tillägg-app” och kopiera dess `objectid` egenskapsvärde.</span><span class="sxs-lookup"><span data-stu-id="32840-123">Find hello application in hello list where hello name begins with 'b2c-extension-app’ and copy its `objectid` property value.</span></span>
1. <span data-ttu-id="32840-124">Utfärda en HTTP POST mot hello URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`.</span><span class="sxs-lookup"><span data-stu-id="32840-124">Issue an HTTP POST against hello URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`.</span></span> <span data-ttu-id="32840-125">Ersätt hello `{OBJECTID}` del av hello URL: en med hello `objectid` hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="32840-125">Replace hello `{OBJECTID}` portion of hello URL with hello `objectid` from hello previous step.</span></span> 

<span data-ttu-id="32840-126">Du bör nu kunna för[Se hello återställts](#verifying-that-the-extensions-app-is-present) i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="32840-126">You should now be able too[see hello restored app](#verifying-that-the-extensions-app-is-present) in hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="32840-127">Ett program kan endast återställas om det har tagits bort inom hello senaste 30 dagarna.</span><span class="sxs-lookup"><span data-stu-id="32840-127">An application can only be restored if it has been deleted within hello last 30 days.</span></span> <span data-ttu-id="32840-128">Om det är mer än 30 dagar, blir data förlorade permanent.</span><span class="sxs-lookup"><span data-stu-id="32840-128">If it has been more than 30 days, data will be permanently lost.</span></span> <span data-ttu-id="32840-129">Filen ett supportärende för mer hjälp.</span><span class="sxs-lookup"><span data-stu-id="32840-129">For more assistance, file a support ticket.</span></span>
