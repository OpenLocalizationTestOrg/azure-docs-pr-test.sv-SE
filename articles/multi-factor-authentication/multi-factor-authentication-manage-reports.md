---
title: "aaaAccess- och användningsdata rapporter för Azure MFA | Microsoft Docs"
description: "Här beskrivs hur toouse hello Azure Multi-Factor Authentication-funktionen - rapporter."
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
ms.openlocfilehash: ae7ccceca4968d7ec7cf0cb1cf9e041d9997c840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a><span data-ttu-id="69ca9-103">Rapporter i Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="69ca9-103">Reports in Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="69ca9-104">Azure Multi-Factor Authentication innehåller flera rapporter som kan användas av du och din organisation.</span><span class="sxs-lookup"><span data-stu-id="69ca9-104">Azure Multi-Factor Authentication provides several reports that can be used by you and your organization.</span></span> <span data-ttu-id="69ca9-105">De här rapporterna kan nås via hello Multi-Factor Authentication-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="69ca9-105">These reports can be accessed through hello Multi-Factor Authentication Management Portal.</span></span> <span data-ttu-id="69ca9-106">hello nedan följer en lista över tillgängliga hello-rapporter:</span><span class="sxs-lookup"><span data-stu-id="69ca9-106">hello following is a list of hello available reports:</span></span>

| <span data-ttu-id="69ca9-107">Rapport</span><span class="sxs-lookup"><span data-stu-id="69ca9-107">Report</span></span> | <span data-ttu-id="69ca9-108">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="69ca9-108">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="69ca9-109">Användning</span><span class="sxs-lookup"><span data-stu-id="69ca9-109">Usage</span></span> |<span data-ttu-id="69ca9-110">hello användning rapporter visar information om övergripande användning, Användarsammanfattning och användarinformation.</span><span class="sxs-lookup"><span data-stu-id="69ca9-110">hello usage reports display information on overall usage, user summary, and user details.</span></span> |
| <span data-ttu-id="69ca9-111">Serverstatus</span><span class="sxs-lookup"><span data-stu-id="69ca9-111">Server Status</span></span> |<span data-ttu-id="69ca9-112">Den här rapporten visar hello status för multi-Factor Authentication-servrar som är kopplad till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="69ca9-112">This report displays hello status of Multi-Factor Authentication Servers associated with your account.</span></span> |
| <span data-ttu-id="69ca9-113">Historik över blockerad användare</span><span class="sxs-lookup"><span data-stu-id="69ca9-113">Blocked User History</span></span> |<span data-ttu-id="69ca9-114">Rapporterna visar hello historik över förfrågningar tooblock eller avblockera användare.</span><span class="sxs-lookup"><span data-stu-id="69ca9-114">These reports show hello history of requests tooblock or unblock users.</span></span> |
| <span data-ttu-id="69ca9-115">Historik över åsidosatt användare</span><span class="sxs-lookup"><span data-stu-id="69ca9-115">Bypassed User History</span></span> |<span data-ttu-id="69ca9-116">Visar hello historik över förfrågningar toobypass Multifaktorautentisering för en användares telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="69ca9-116">Shows hello history of requests toobypass Multi-Factor Authentication for a user's phone number.</span></span> |
| <span data-ttu-id="69ca9-117">Bedrägerivarning</span><span class="sxs-lookup"><span data-stu-id="69ca9-117">Fraud Alert</span></span> |<span data-ttu-id="69ca9-118">Visar en historik över bedrägerivarningar som skickats under hello angivna datumintervallet.</span><span class="sxs-lookup"><span data-stu-id="69ca9-118">Shows a history of fraud alerts submitted during hello date range you specified.</span></span> |
| <span data-ttu-id="69ca9-119">I kö</span><span class="sxs-lookup"><span data-stu-id="69ca9-119">Queued</span></span> |<span data-ttu-id="69ca9-120">Visar rapporter i kö för bearbetning och deras status.</span><span class="sxs-lookup"><span data-stu-id="69ca9-120">Lists reports queued for processing and their status.</span></span> <span data-ttu-id="69ca9-121">En toodownload eller visa hello rapport tillhandahålls när rapporten hello är klar.</span><span class="sxs-lookup"><span data-stu-id="69ca9-121">A link toodownload or view hello report is provided when hello report is complete.</span></span> |

## <a name="view-reports"></a><span data-ttu-id="69ca9-122">Visa rapporter</span><span class="sxs-lookup"><span data-stu-id="69ca9-122">View reports</span></span>
1. <span data-ttu-id="69ca9-123">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="69ca9-123">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="69ca9-124">Välj Active Directory hello vänster.</span><span class="sxs-lookup"><span data-stu-id="69ca9-124">On hello left, select Active Directory.</span></span>
3. <span data-ttu-id="69ca9-125">Gör något av följande två alternativ, beroende på om du använder Autentiseringsleverantörer:</span><span class="sxs-lookup"><span data-stu-id="69ca9-125">Follow one of these two options, depending on whether you use Auth Providers:</span></span>
   * <span data-ttu-id="69ca9-126">**Alternativ 1**: hello Flerfunktionsautentiseringsleverantörer fliken. Välj din MFA-leverantör och klicka på hello **hantera** knappen längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="69ca9-126">**Option 1**: Click hello Multi-Factor Auth Providers tab. Select your MFA provider and click hello **Manage** button at hello bottom.</span></span>
   * <span data-ttu-id="69ca9-127">**Alternativ 2**: Välj din katalog och gå toohello **konfigurera** fliken. Markera under hello multifaktorautentisering avsnittet **hantera tjänstinställningar**.</span><span class="sxs-lookup"><span data-stu-id="69ca9-127">**Option 2**: Select your directory and go toohello **Configure** tab. Under hello multi-factor authentication section, select **Manage service settings**.</span></span> <span data-ttu-id="69ca9-128">Hello längst ned på sidan för hello MFA inställningar klickar du på hello gå toohello portal-länken.</span><span class="sxs-lookup"><span data-stu-id="69ca9-128">At hello bottom of hello MFA Service Settings page, click hello Go toohello portal link.</span></span>
4. <span data-ttu-id="69ca9-129">Välj hello typ av rapport som du vill använda från hello i hello Azure Multi-Factor Authentication-hanteringsportalen, **visa en rapport** avsnitt i hello vänstra navigeringsrutan.</span><span class="sxs-lookup"><span data-stu-id="69ca9-129">In hello Azure Multi-Factor Authentication Management Portal, select hello type of report you want from hello **View a Report** section in hello left navigation.</span></span>

<span data-ttu-id="69ca9-130"><center>![Moln](./media/multi-factor-authentication-manage-reports/report.png)</center></span><span class="sxs-lookup"><span data-stu-id="69ca9-130"><center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center></span></span>


<span data-ttu-id="69ca9-131">**Ytterligare resurser**</span><span class="sxs-lookup"><span data-stu-id="69ca9-131">**Additional Resources**</span></span>

* [<span data-ttu-id="69ca9-132">För användare</span><span class="sxs-lookup"><span data-stu-id="69ca9-132">For Users</span></span>](end-user/multi-factor-authentication-end-user.md)
* [<span data-ttu-id="69ca9-133">Azure Multi-Factor Authentication på MSDN</span><span class="sxs-lookup"><span data-stu-id="69ca9-133">Azure Multi-Factor Authentication on MSDN</span></span>](https://msdn.microsoft.com/library/azure/dn249471.aspx)
