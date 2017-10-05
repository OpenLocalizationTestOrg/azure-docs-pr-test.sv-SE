---
title: "Så här använder du granskningsloggen i Azure AD Privileged Identity Management | Microsoft Docs"
description: "Lär dig hur du använder du granskningsloggen i Azure Privileged Identity Management-tillägget."
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
ms.openlocfilehash: 7d9a5255a64d46c1388d328a606b3f297d61262b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-audit-log-in-pim"></a><span data-ttu-id="8088d-103">Använda granskningsloggen i PIM</span><span class="sxs-lookup"><span data-stu-id="8088d-103">Using the audit log in PIM</span></span>
<span data-ttu-id="8088d-104">Du kan använda granskningsloggen Privileged Identity Management (PIM) om du vill visa alla användare och aktiveringar inom en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="8088d-104">You can use the Privileged Identity Management (PIM) audit log to see all the user assignments and activations within a given time period.</span></span> <span data-ttu-id="8088d-105">Om du vill visa fullständig granskningshistorik för aktivitet i din klient, inklusive administratör, slutanvändare och synkroniseringsåtgärden, kan du använda den [rapporter för åtkomst till och användning av Azure Active Directory.](active-directory-view-access-usage-reports.md)</span><span class="sxs-lookup"><span data-stu-id="8088d-105">If you want to see the full audit history of activity in your tenant, including administrator, end user, and synchronization activity, you can use the [Azure Active Directory access and usage reports.](active-directory-view-access-usage-reports.md)</span></span>

## <a name="navigate-to-the-audit-log"></a><span data-ttu-id="8088d-106">Navigera till granskningslogg</span><span class="sxs-lookup"><span data-stu-id="8088d-106">Navigate to the audit log</span></span>
<span data-ttu-id="8088d-107">Från den [Azure-portalen](https://portal.azure.com) instrumentpanelen, väljer den **Azure AD Privileged Identity Management** app.</span><span class="sxs-lookup"><span data-stu-id="8088d-107">From the [Azure portal](https://portal.azure.com) dashboard, select the **Azure AD Privileged Identity Management** app.</span></span> <span data-ttu-id="8088d-108">Därifrån kan komma åt granskningsloggen genom att klicka på **hantera Privilegierade roller** > **granskningshistorik** PIM-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="8088d-108">From there, access the audit log by clicking **Manage privileged roles** > **Audit history** in the PIM dashboard.</span></span>

## <a name="the-audit-log-graph"></a><span data-ttu-id="8088d-109">Granska loggen diagrammet</span><span class="sxs-lookup"><span data-stu-id="8088d-109">The audit log graph</span></span>
<span data-ttu-id="8088d-110">Du kan använda granskningsloggen för att visa totalt antal aktiveringar, max aktiveringar per dag och genomsnittlig aktiveringar per dag i ett linjediagram.</span><span class="sxs-lookup"><span data-stu-id="8088d-110">You can use the audit log to view the total activations, max activations per day, and average activations per day in a line graph.</span></span>  <span data-ttu-id="8088d-111">Du kan filtrera informationen efter roll om det finns fler än en roll i granskningshistoriken.</span><span class="sxs-lookup"><span data-stu-id="8088d-111">You can also filter the data by role if there is more than one role in the audit history.</span></span>

<span data-ttu-id="8088d-112">Använd den **tid**, **åtgärd**, och **rollen** knappar för att sortera loggen.</span><span class="sxs-lookup"><span data-stu-id="8088d-112">Use the **time**, **action**, and **role** buttons to sort the log.</span></span>

## <a name="the-audit-log-list"></a><span data-ttu-id="8088d-113">Listan Granska loggen</span><span class="sxs-lookup"><span data-stu-id="8088d-113">The audit log list</span></span>
<span data-ttu-id="8088d-114">Kolumner i listan Granska loggen är:</span><span class="sxs-lookup"><span data-stu-id="8088d-114">The columns in the audit log list are:</span></span>

* <span data-ttu-id="8088d-115">**Begärande** -användaren som begärde rollaktivering eller ändra.</span><span class="sxs-lookup"><span data-stu-id="8088d-115">**Requestor** - the user who requested the role activation or change.</span></span>  <span data-ttu-id="8088d-116">Kontrollera Azure granskningsloggen för mer information om värdet är ”Azure System”.</span><span class="sxs-lookup"><span data-stu-id="8088d-116">If the value is "Azure System", check the Azure audit log for more information.</span></span>
* <span data-ttu-id="8088d-117">**Användaren** -användaren som är genom att aktivera eller tilldelade till en roll.</span><span class="sxs-lookup"><span data-stu-id="8088d-117">**User** - the user who is activating or assigned to a role.</span></span>
* <span data-ttu-id="8088d-118">**Rollen** -rollen tilldelas eller aktiveras av användaren.</span><span class="sxs-lookup"><span data-stu-id="8088d-118">**Role** - the role assigned or activated by the user.</span></span>
* <span data-ttu-id="8088d-119">**Åtgärden** – de åtgärder som vidtas av förfrågaren.</span><span class="sxs-lookup"><span data-stu-id="8088d-119">**Action** - the actions taken by the requestor.</span></span> <span data-ttu-id="8088d-120">Detta kan inkludera tilldelning vid ej tilldelning, aktivering eller inaktivering.</span><span class="sxs-lookup"><span data-stu-id="8088d-120">This can include assignment, unassignment, activation, or deactivation.</span></span>
* <span data-ttu-id="8088d-121">**Tid** – när åtgärden utfördes.</span><span class="sxs-lookup"><span data-stu-id="8088d-121">**Time** - when the action occurred.</span></span>
* <span data-ttu-id="8088d-122">**Skäl till** -om text har angetts i fältet Orsak under aktivering, visas här.</span><span class="sxs-lookup"><span data-stu-id="8088d-122">**Reasoning** - if any text was entered into the reason field during activation, it will show up here.</span></span>
* <span data-ttu-id="8088d-123">**Förfallodatum** – endast relevant för aktivering av roller.</span><span class="sxs-lookup"><span data-stu-id="8088d-123">**Expiration** - only relevant for activation of roles.</span></span>

## <a name="filter-the-audit-log"></a><span data-ttu-id="8088d-124">Filtrera granskningsloggen</span><span class="sxs-lookup"><span data-stu-id="8088d-124">Filter the audit log</span></span>
<span data-ttu-id="8088d-125">Du kan filtrera den information som visas i granskningsloggen genom att klicka på den **Filter** knappen.</span><span class="sxs-lookup"><span data-stu-id="8088d-125">You can filter the information that shows up in the audit log by clicking the **Filter** button.</span></span>  <span data-ttu-id="8088d-126">Den **uppdatering diagram parametrar bladet** visas.</span><span class="sxs-lookup"><span data-stu-id="8088d-126">The **Update chart parameters blade** will appear.</span></span>

<span data-ttu-id="8088d-127">När du har angett filtren klickar du på **uppdatering** att filtrera data i loggen.</span><span class="sxs-lookup"><span data-stu-id="8088d-127">After you set the filters, click **Update** to filter the data in the log.</span></span>  <span data-ttu-id="8088d-128">Om data inte visas direkt, uppdatera sidan.</span><span class="sxs-lookup"><span data-stu-id="8088d-128">If the data doesn't appear right away, refresh the page.</span></span>

### <a name="change-the-date-range"></a><span data-ttu-id="8088d-129">Ändra datumintervallet</span><span class="sxs-lookup"><span data-stu-id="8088d-129">Change the date range</span></span>
<span data-ttu-id="8088d-130">Använd den **idag**, **gångna veckan**, **senaste månaden**, eller **anpassad** knappar för att ändra tidsintervallet på granskningsloggen.</span><span class="sxs-lookup"><span data-stu-id="8088d-130">Use the **Today**, **Past Week**, **Past Month**, or **Custom** buttons to change the time range of the audit log.</span></span>

<span data-ttu-id="8088d-131">När du väljer den **anpassad** knappen, får du en **från** datum och en **till** datumfält att ange ett datumintervall för loggen.</span><span class="sxs-lookup"><span data-stu-id="8088d-131">When you choose the **Custom** button, you will be given a **From** date field and a **To** date field to specify a range of dates for the log.</span></span>  <span data-ttu-id="8088d-132">Du kan ange datum i formatet ÅÅÅÅ-MM-DD eller klicka på den **kalender** ikonen och Välj datum i en kalender.</span><span class="sxs-lookup"><span data-stu-id="8088d-132">You can either enter the dates in MM/DD/YYYY format or click on the **calendar** icon and choose the date from a calendar.</span></span>

### <a name="change-the-roles-included-in-the-log"></a><span data-ttu-id="8088d-133">Ändra de roller som ingår i loggen</span><span class="sxs-lookup"><span data-stu-id="8088d-133">Change the roles included in the log</span></span>
<span data-ttu-id="8088d-134">Markera eller avmarkera den **rollen** kryssrutan bredvid varje roll för att inkludera eller exkludera från loggen.</span><span class="sxs-lookup"><span data-stu-id="8088d-134">Check or uncheck the **Role** checkbox next to each role to include or exclude it from the log.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="8088d-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8088d-135">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

