---
title: aaaHow toouse hello granskningsloggen i Azure AD Privileged Identity Management | Microsoft Docs
description: "Lär dig hur toouse hello granskningsloggen i hello Azure Privileged Identity Management – tillägget."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 5d13a6dd-1fcb-4e76-82fb-cb2f4f0e4357
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 36987eaab9fe02c5dd7b4f4705e487299430745d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-audit-log-in-pim"></a><span data-ttu-id="93b52-103">Med hjälp av hello granskningsloggen i PIM</span><span class="sxs-lookup"><span data-stu-id="93b52-103">Using hello audit log in PIM</span></span>
<span data-ttu-id="93b52-104">Du kan använda hello Privileged Identity Management (PIM) Granska loggen toosee alla hello användartilldelningar och aktiveringar inom en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="93b52-104">You can use hello Privileged Identity Management (PIM) audit log toosee all hello user assignments and activations within a given time period.</span></span> <span data-ttu-id="93b52-105">Om du vill toosee hello fullständig granskningshistorik för aktiviteten i din klientorganisation, inklusive administratör, slutanvändare och synkroniseringsåtgärden, kan du använda hello [rapporter för åtkomst till och användning av Azure Active Directory.](active-directory-view-access-usage-reports.md)</span><span class="sxs-lookup"><span data-stu-id="93b52-105">If you want toosee hello full audit history of activity in your tenant, including administrator, end user, and synchronization activity, you can use hello [Azure Active Directory access and usage reports.](active-directory-view-access-usage-reports.md)</span></span>

## <a name="navigate-toohello-audit-log"></a><span data-ttu-id="93b52-106">Navigera toohello granskningsloggen</span><span class="sxs-lookup"><span data-stu-id="93b52-106">Navigate toohello audit log</span></span>
<span data-ttu-id="93b52-107">Från hello [Azure-portalen](https://portal.azure.com) instrumentpanelen, väljer hello **Azure AD Privileged Identity Management** app.</span><span class="sxs-lookup"><span data-stu-id="93b52-107">From hello [Azure portal](https://portal.azure.com) dashboard, select hello **Azure AD Privileged Identity Management** app.</span></span> <span data-ttu-id="93b52-108">Därifrån kan komma åt hello granskningsloggen genom att klicka på **hantera Privilegierade roller** > **granskningshistorik** i hello PIM-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="93b52-108">From there, access hello audit log by clicking **Manage privileged roles** > **Audit history** in hello PIM dashboard.</span></span>

## <a name="hello-audit-log-graph"></a><span data-ttu-id="93b52-109">hello Granska loggen diagram</span><span class="sxs-lookup"><span data-stu-id="93b52-109">hello audit log graph</span></span>
<span data-ttu-id="93b52-110">Du kan använda hello Granska loggen tooview hello Totalt antal aktiveringar, max aktiveringar per dag och genomsnittlig aktiveringar per dag i ett linjediagram.</span><span class="sxs-lookup"><span data-stu-id="93b52-110">You can use hello audit log tooview hello total activations, max activations per day, and average activations per day in a line graph.</span></span>  <span data-ttu-id="93b52-111">Du kan också filtrera hello data av rollen om det finns fler än en roll i hello granskningshistorik.</span><span class="sxs-lookup"><span data-stu-id="93b52-111">You can also filter hello data by role if there is more than one role in hello audit history.</span></span>

<span data-ttu-id="93b52-112">Använd hello **tid**, **åtgärd**, och **rollen** knappar toosort hello loggen.</span><span class="sxs-lookup"><span data-stu-id="93b52-112">Use hello **time**, **action**, and **role** buttons toosort hello log.</span></span>

## <a name="hello-audit-log-list"></a><span data-ttu-id="93b52-113">listan över hello granskning</span><span class="sxs-lookup"><span data-stu-id="93b52-113">hello audit log list</span></span>
<span data-ttu-id="93b52-114">hello kolumner i listan för hello granskning loggen är:</span><span class="sxs-lookup"><span data-stu-id="93b52-114">hello columns in hello audit log list are:</span></span>

* <span data-ttu-id="93b52-115">**Begärande** -hello användare som begärde hello rollaktivering eller ändra.</span><span class="sxs-lookup"><span data-stu-id="93b52-115">**Requestor** - hello user who requested hello role activation or change.</span></span>  <span data-ttu-id="93b52-116">Kontrollera hello Azure granskningsloggen för mer information om hello-värdet är ”Azure System”.</span><span class="sxs-lookup"><span data-stu-id="93b52-116">If hello value is "Azure System", check hello Azure audit log for more information.</span></span>
* <span data-ttu-id="93b52-117">**Användaren** -hello användare aktiveras eller tooa rollen.</span><span class="sxs-lookup"><span data-stu-id="93b52-117">**User** - hello user who is activating or assigned tooa role.</span></span>
* <span data-ttu-id="93b52-118">**Rollen** -hello rollen tilldelas eller aktiveras av hello användare.</span><span class="sxs-lookup"><span data-stu-id="93b52-118">**Role** - hello role assigned or activated by hello user.</span></span>
* <span data-ttu-id="93b52-119">**Åtgärden** - hello-åtgärder som vidtas av hello begärande.</span><span class="sxs-lookup"><span data-stu-id="93b52-119">**Action** - hello actions taken by hello requestor.</span></span> <span data-ttu-id="93b52-120">Detta kan inkludera tilldelning vid ej tilldelning, aktivering eller inaktivering.</span><span class="sxs-lookup"><span data-stu-id="93b52-120">This can include assignment, unassignment, activation, or deactivation.</span></span>
* <span data-ttu-id="93b52-121">**Tid** – när hello åtgärd har utförts.</span><span class="sxs-lookup"><span data-stu-id="93b52-121">**Time** - when hello action occurred.</span></span>
* <span data-ttu-id="93b52-122">**Skäl till** -om text har angetts i hello orsak fältet under aktiveringen, visas här.</span><span class="sxs-lookup"><span data-stu-id="93b52-122">**Reasoning** - if any text was entered into hello reason field during activation, it will show up here.</span></span>
* <span data-ttu-id="93b52-123">**Förfallodatum** – endast relevant för aktivering av roller.</span><span class="sxs-lookup"><span data-stu-id="93b52-123">**Expiration** - only relevant for activation of roles.</span></span>

## <a name="filter-hello-audit-log"></a><span data-ttu-id="93b52-124">Filtrera hello granskningsloggen</span><span class="sxs-lookup"><span data-stu-id="93b52-124">Filter hello audit log</span></span>
<span data-ttu-id="93b52-125">Du kan filtrera hello information som visas i hello granskningsloggen genom att klicka på hello **Filter** knappen.</span><span class="sxs-lookup"><span data-stu-id="93b52-125">You can filter hello information that shows up in hello audit log by clicking hello **Filter** button.</span></span>  <span data-ttu-id="93b52-126">Hej **uppdatering diagram parametrar bladet** visas.</span><span class="sxs-lookup"><span data-stu-id="93b52-126">hello **Update chart parameters blade** will appear.</span></span>

<span data-ttu-id="93b52-127">När du anger hello filter, klickar du på **uppdatering** toofilter hello data i hello-loggen.</span><span class="sxs-lookup"><span data-stu-id="93b52-127">After you set hello filters, click **Update** toofilter hello data in hello log.</span></span>  <span data-ttu-id="93b52-128">Om hello data inte visas direkt, uppdatera hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="93b52-128">If hello data doesn't appear right away, refresh hello page.</span></span>

### <a name="change-hello-date-range"></a><span data-ttu-id="93b52-129">Ändra hello datumintervall</span><span class="sxs-lookup"><span data-stu-id="93b52-129">Change hello date range</span></span>
<span data-ttu-id="93b52-130">Använd hello **idag**, **gångna veckan**, **senaste månaden**, eller **anpassad** knappar toochange hello tidsintervall för hello granskningslogg.</span><span class="sxs-lookup"><span data-stu-id="93b52-130">Use hello **Today**, **Past Week**, **Past Month**, or **Custom** buttons toochange hello time range of hello audit log.</span></span>

<span data-ttu-id="93b52-131">När du väljer hello **anpassad** knappen, får du en **från** datum och en **till** datum fältet toospecify ett datumintervall för hello logg.</span><span class="sxs-lookup"><span data-stu-id="93b52-131">When you choose hello **Custom** button, you will be given a **From** date field and a **To** date field toospecify a range of dates for hello log.</span></span>  <span data-ttu-id="93b52-132">Du kan ange hello datum i formatet ÅÅÅÅ-MM-DD eller klicka på hello **kalender** ikon och väljer en kalender hello datum.</span><span class="sxs-lookup"><span data-stu-id="93b52-132">You can either enter hello dates in MM/DD/YYYY format or click on hello **calendar** icon and choose hello date from a calendar.</span></span>

### <a name="change-hello-roles-included-in-hello-log"></a><span data-ttu-id="93b52-133">Ändra hello roller som ingår i hello-loggen</span><span class="sxs-lookup"><span data-stu-id="93b52-133">Change hello roles included in hello log</span></span>
<span data-ttu-id="93b52-134">Markera eller avmarkera hello **rollen** kryssrutan nästa tooeach rollen tooinclude eller Uteslut den från hello logga.</span><span class="sxs-lookup"><span data-stu-id="93b52-134">Check or uncheck hello **Role** checkbox next tooeach role tooinclude or exclude it from hello log.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="93b52-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93b52-135">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

