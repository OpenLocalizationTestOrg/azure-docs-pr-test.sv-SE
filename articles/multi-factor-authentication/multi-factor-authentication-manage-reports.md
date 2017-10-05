---
title: "Åtkomst- och rapporter för Azure MFA | Microsoft Docs"
description: "Här beskrivs hur du använder funktionen Azure Multi-Factor Authentication - rapporter."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: f76e726c6a67de4b0472c0e97f9e72c31c14c4f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a><span data-ttu-id="6d00d-103">Rapporter i Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="6d00d-103">Reports in Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="6d00d-104">Azure Multi-Factor Authentication innehåller flera rapporter som kan användas av du och din organisation.</span><span class="sxs-lookup"><span data-stu-id="6d00d-104">Azure Multi-Factor Authentication provides several reports that can be used by you and your organization.</span></span> <span data-ttu-id="6d00d-105">De här rapporterna kan nås via Multi-Factor Authentication-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="6d00d-105">These reports can be accessed through the Multi-Factor Authentication Management Portal.</span></span> <span data-ttu-id="6d00d-106">Följande är en lista över tillgängliga rapporter:</span><span class="sxs-lookup"><span data-stu-id="6d00d-106">The following is a list of the available reports:</span></span>

| <span data-ttu-id="6d00d-107">Rapport</span><span class="sxs-lookup"><span data-stu-id="6d00d-107">Report</span></span> | <span data-ttu-id="6d00d-108">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6d00d-108">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="6d00d-109">Användning</span><span class="sxs-lookup"><span data-stu-id="6d00d-109">Usage</span></span> |<span data-ttu-id="6d00d-110">Användningen rapporter visar information om övergripande användning, Användarsammanfattning och användarinformation.</span><span class="sxs-lookup"><span data-stu-id="6d00d-110">The usage reports display information on overall usage, user summary, and user details.</span></span> |
| <span data-ttu-id="6d00d-111">Serverstatus</span><span class="sxs-lookup"><span data-stu-id="6d00d-111">Server Status</span></span> |<span data-ttu-id="6d00d-112">Den här rapporten visar statusen för multi-Factor Authentication-servrar som är kopplad till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="6d00d-112">This report displays the status of Multi-Factor Authentication Servers associated with your account.</span></span> |
| <span data-ttu-id="6d00d-113">Historik över blockerad användare</span><span class="sxs-lookup"><span data-stu-id="6d00d-113">Blocked User History</span></span> |<span data-ttu-id="6d00d-114">Rapporterna Visar historik över förfrågningar om att blockera eller avblockera användare.</span><span class="sxs-lookup"><span data-stu-id="6d00d-114">These reports show the history of requests to block or unblock users.</span></span> |
| <span data-ttu-id="6d00d-115">Historik över åsidosatt användare</span><span class="sxs-lookup"><span data-stu-id="6d00d-115">Bypassed User History</span></span> |<span data-ttu-id="6d00d-116">Visar historiken över förfrågningar om att koppla förbi Multi-Factor Authentication för en användares telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="6d00d-116">Shows the history of requests to bypass Multi-Factor Authentication for a user's phone number.</span></span> |
| <span data-ttu-id="6d00d-117">Bedrägerivarning</span><span class="sxs-lookup"><span data-stu-id="6d00d-117">Fraud Alert</span></span> |<span data-ttu-id="6d00d-118">Visar en historik över bedrägerivarningar som skickats inom det angivna datumintervallet.</span><span class="sxs-lookup"><span data-stu-id="6d00d-118">Shows a history of fraud alerts submitted during the date range you specified.</span></span> |
| <span data-ttu-id="6d00d-119">I kö</span><span class="sxs-lookup"><span data-stu-id="6d00d-119">Queued</span></span> |<span data-ttu-id="6d00d-120">Visar rapporter i kö för bearbetning och deras status.</span><span class="sxs-lookup"><span data-stu-id="6d00d-120">Lists reports queued for processing and their status.</span></span> <span data-ttu-id="6d00d-121">En länk för att hämta eller visa rapporten tillhandahålls när rapporten är färdig.</span><span class="sxs-lookup"><span data-stu-id="6d00d-121">A link to download or view the report is provided when the report is complete.</span></span> |

## <a name="view-reports"></a><span data-ttu-id="6d00d-122">Visa rapporter</span><span class="sxs-lookup"><span data-stu-id="6d00d-122">View reports</span></span>
1. <span data-ttu-id="6d00d-123">Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="6d00d-123">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="6d00d-124">Välj Active Directory till vänster.</span><span class="sxs-lookup"><span data-stu-id="6d00d-124">On the left, select Active Directory.</span></span>
3. <span data-ttu-id="6d00d-125">Gör något av följande två alternativ, beroende på om du använder Autentiseringsleverantörer:</span><span class="sxs-lookup"><span data-stu-id="6d00d-125">Follow one of these two options, depending on whether you use Auth Providers:</span></span>
   * <span data-ttu-id="6d00d-126">**Alternativ 1**: Klicka på fliken Flerfunktionsautentiseringsleverantörer.</span><span class="sxs-lookup"><span data-stu-id="6d00d-126">**Option 1**: Click the Multi-Factor Auth Providers tab.</span></span> <span data-ttu-id="6d00d-127">Välj din MFA-leverantör och klicka på den **hantera** längst ned.</span><span class="sxs-lookup"><span data-stu-id="6d00d-127">Select your MFA provider and click the **Manage** button at the bottom.</span></span>
   * <span data-ttu-id="6d00d-128">**Alternativ 2**: Välj din katalog och gå till den **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="6d00d-128">**Option 2**: Select your directory and go to the **Configure** tab.</span></span> <span data-ttu-id="6d00d-129">Välj **Hantera tjänstinställningar** i avsnittet för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="6d00d-129">Under the multi-factor authentication section, select **Manage service settings**.</span></span> <span data-ttu-id="6d00d-130">Klicka på Gå till portal-länken längst ned på sidan för MFA inställningar.</span><span class="sxs-lookup"><span data-stu-id="6d00d-130">At the bottom of the MFA Service Settings page, click the Go to the portal link.</span></span>
4. <span data-ttu-id="6d00d-131">Välj typ av rapport som du vill använda från i Azure Multi-Factor Authentication-hanteringsportalen på **visa en rapport** avsnitt i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="6d00d-131">In the Azure Multi-Factor Authentication Management Portal, select the type of report you want from the **View a Report** section in the left navigation.</span></span>

<span data-ttu-id="6d00d-132"><center>![Moln](./media/multi-factor-authentication-manage-reports/report.png)</center></span><span class="sxs-lookup"><span data-stu-id="6d00d-132"><center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center></span></span>


<span data-ttu-id="6d00d-133">**Ytterligare resurser**</span><span class="sxs-lookup"><span data-stu-id="6d00d-133">**Additional Resources**</span></span>

* [<span data-ttu-id="6d00d-134">För användare</span><span class="sxs-lookup"><span data-stu-id="6d00d-134">For Users</span></span>](end-user/multi-factor-authentication-end-user.md)
* [<span data-ttu-id="6d00d-135">Azure Multi-Factor Authentication på MSDN</span><span class="sxs-lookup"><span data-stu-id="6d00d-135">Azure Multi-Factor Authentication on MSDN</span></span>](https://msdn.microsoft.com/library/azure/dn249471.aspx)
