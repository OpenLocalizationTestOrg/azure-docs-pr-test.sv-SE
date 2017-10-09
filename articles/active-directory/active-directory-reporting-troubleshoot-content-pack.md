---
title: "Felsökning av Azure Active Directory-aktivitet loggar Innehållspaketet fel | Microsoft Docs"
description: "Ger dig en lista över felmeddelanden i hello Azure Active Directory-aktivitet content pack och steg toofix dem."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 325de65ff1572a2f8f8319c0a52350bda03af3de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a><span data-ttu-id="c0079-103">Felsökning av Azure Active Directory-aktivitet loggar Innehållspaketet fel</span><span class="sxs-lookup"><span data-stu-id="c0079-103">Troubleshooting Azure Active Directory Activity logs content pack errors</span></span> 


<span data-ttu-id="c0079-104">När du arbetar med hello Power BI-Innehållspaketet för Azure Active Directory-förhandsgranskning, är det möjligt att du kör i hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="c0079-104">When working with hello Power BI Content Pack for Azure Active Directory Preview, it is possible that you run into hello following errors:</span></span> 

- [<span data-ttu-id="c0079-105">Uppdateringen misslyckades</span><span class="sxs-lookup"><span data-stu-id="c0079-105">Refresh failed</span></span>](active-directory-reporting-troubleshoot-content-pack.md#refresh-failed) 
- [<span data-ttu-id="c0079-106">Misslyckade tooupdate autentiseringsuppgifterna för datakällan</span><span class="sxs-lookup"><span data-stu-id="c0079-106">Failed tooupdate data source credentials</span></span>](active-directory-reporting-troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [<span data-ttu-id="c0079-107">Importen av data tar för lång</span><span class="sxs-lookup"><span data-stu-id="c0079-107">Importing of data is taking too long</span></span>](active-directory-reporting-troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
<span data-ttu-id="c0079-108">Det här avsnittet ger information om hello möjliga orsaker och hur toofix felen.</span><span class="sxs-lookup"><span data-stu-id="c0079-108">This topic provides you with information about hello possible causes and how toofix these errors.</span></span>
 
## <a name="refresh-failed"></a><span data-ttu-id="c0079-109">Uppdateringen misslyckades</span><span class="sxs-lookup"><span data-stu-id="c0079-109">Refresh failed</span></span> 
 
<span data-ttu-id="c0079-110">**Hur det här felet är anslutna**: e-post från Power BI- eller felstatus i hello uppdateringshistoriken.</span><span class="sxs-lookup"><span data-stu-id="c0079-110">**How this error is surfaced**: Email from Power BI or failed status in hello refresh history.</span></span> 


| <span data-ttu-id="c0079-111">Orsak</span><span class="sxs-lookup"><span data-stu-id="c0079-111">Cause</span></span> | <span data-ttu-id="c0079-112">Hur toofix</span><span class="sxs-lookup"><span data-stu-id="c0079-112">How toofix</span></span> |
| ---   | ---        |
| <span data-ttu-id="c0079-113">Uppdatera fel kan bero på fel när hello autentiseringsuppgifterna för hello användare som ansluter toohello Innehållspaketet har återställa men inte uppdateras i hello anslutningsinställningarna för hello av hello-Innehållspaketet.</span><span class="sxs-lookup"><span data-stu-id="c0079-113">Refresh failure errors can be caused when hello credentials of hello users connecting toohello content pack have been reset but not updated in hello connection settings of hello of hello content pack.</span></span> | <span data-ttu-id="c0079-114">Hitta hello dataset motsvarande toohello Azure Active Directory-aktivitet loggar instrumentpanelen (Azure Active Directory aktivitetsloggar) i Power BI, väljer du schemalägga en uppdatering och ange dina autentiseringsuppgifter för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c0079-114">In Power BI, locate hello dataset corresponding toohello Azure Active Directory Activity logs dashboard (Azure Active Directory Activity logs), choose schedule refresh, and then enter your Azure AD credentials.</span></span> |
| <span data-ttu-id="c0079-115">En uppdatering kan misslyckas på grund av problem med toodata i hello underliggande Innehållspaketet.</span><span class="sxs-lookup"><span data-stu-id="c0079-115">A refresh can fail due toodata issues in hello underlying content pack.</span></span> | <span data-ttu-id="c0079-116">Filen ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="c0079-116">File a support ticket.</span></span> <span data-ttu-id="c0079-117">Mer information finns i [hur tooget stöd för Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="c0079-117">For more details, see [How tooget support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 
 
## <a name="failed-tooupdate-data-source-credentials"></a><span data-ttu-id="c0079-118">Misslyckade tooupdate autentiseringsuppgifterna för datakällan</span><span class="sxs-lookup"><span data-stu-id="c0079-118">Failed tooupdate data source credentials</span></span> 
 
<span data-ttu-id="c0079-119">**Hur det här felet är anslutna**: I Power BI, när du ansluter toohello Azure Active Directory-aktivitet loggar (förhandsgranskning)-Innehållspaketet.</span><span class="sxs-lookup"><span data-stu-id="c0079-119">**How this error is surfaced**: In Power BI, when you are connecting toohello Azure Active Directory Activity logs (preview) content pack.</span></span> 

| <span data-ttu-id="c0079-120">Orsak</span><span class="sxs-lookup"><span data-stu-id="c0079-120">Cause</span></span> | <span data-ttu-id="c0079-121">Hur toofix</span><span class="sxs-lookup"><span data-stu-id="c0079-121">How toofix</span></span> |
| ---   | ---        |
| <span data-ttu-id="c0079-122">hello anslutande användaren är varken en global administratör eller en läsare säkerhet eller säkerhet-administratör.</span><span class="sxs-lookup"><span data-stu-id="c0079-122">hello connecting user is neither a global admin nor a security reader or a security admin.</span></span> | <span data-ttu-id="c0079-123">Använd ett konto som är en global administratör eller en läsare säkerhet eller en säkerhet admin tooaccess hello innehållspaket.</span><span class="sxs-lookup"><span data-stu-id="c0079-123">Use an account that is either a global admin or a security reader or a security admin tooaccess hello content packs.</span></span> |
| <span data-ttu-id="c0079-124">Din klient är inte en Premium-klientorganisation eller har inte minst en användare med Premium-licens filen.</span><span class="sxs-lookup"><span data-stu-id="c0079-124">Your tenant is not a Premium tenant or doesn't have at least one user with Premium license File.</span></span> | <span data-ttu-id="c0079-125">Filen ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="c0079-125">File a support ticket.</span></span> <span data-ttu-id="c0079-126">Mer information finns i [hur tooget stöd för Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="c0079-126">For more details, see [How tooget support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 

 

## <a name="importing-of-data-is-taking-too-long"></a><span data-ttu-id="c0079-127">Importen av data tar för lång</span><span class="sxs-lookup"><span data-stu-id="c0079-127">Importing of data is taking too long</span></span> 
 
<span data-ttu-id="c0079-128">**Hur det här felet är anslutna**: I Powerbi, när du har anslutit till ditt innehållspaket hello dataimporten startar tooprepare instrumentpanelen för Azure Active Directory aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="c0079-128">**How this error is surfaced**: In Power BI, once you have connected your content pack, hello data import process starts tooprepare your dashboard for Azure Active Directory Activity log.</span></span> <span data-ttu-id="c0079-129">Du ser hello-meddelande ”:*importerar data...* ”</span><span class="sxs-lookup"><span data-stu-id="c0079-129">You see hello message: “*Importing data...*”</span></span>  

| <span data-ttu-id="c0079-130">Orsak</span><span class="sxs-lookup"><span data-stu-id="c0079-130">Cause</span></span> | <span data-ttu-id="c0079-131">Hur toofix</span><span class="sxs-lookup"><span data-stu-id="c0079-131">How toofix</span></span> |
| ---   | ---        |
| <span data-ttu-id="c0079-132">Beroende på hello storlek för din klient, kan det här steget ta några minuter too30 minuter.</span><span class="sxs-lookup"><span data-stu-id="c0079-132">Depending on hello size of your tenant, this step could take anywhere from a few mins too30 minutes.</span></span> | <span data-ttu-id="c0079-133">Bara vara öppen.</span><span class="sxs-lookup"><span data-stu-id="c0079-133">Just be patient.</span></span> <span data-ttu-id="c0079-134">Om hello-meddelande inte ändras tooshowing instrumentpanelen inom en timme, kontrollera filen ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="c0079-134">If hello message does not change tooshowing your dashboard within an hour, please file a support ticket.</span></span> <span data-ttu-id="c0079-135">Mer information finns i [hur tooget stöd för Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="c0079-135">For more details, see [How tooget support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|

## <a name="next-steps"></a><span data-ttu-id="c0079-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c0079-136">Next steps</span></span>

<span data-ttu-id="c0079-137">tooinstall hello Power BI-Innehållspaketet för Azure Active Directory-förhandsgranskning klickar du på [här](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span><span class="sxs-lookup"><span data-stu-id="c0079-137">tooinstall hello Power BI Content Pack for Azure Active Directory Preview, click [here](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span></span>


