---
title: "Felsökning av Azure Active Directory-aktivitet loggar Innehållspaketet fel | Microsoft Docs"
description: "Ger dig en lista över felmeddelanden i Azure Active Directory-aktivitet Innehållspaketet och steg för att åtgärda dem."
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
ms.openlocfilehash: c880e9eb6d48bd1e38075fbd867d3906ec67b547
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a><span data-ttu-id="dbcd6-103">Felsökning av Azure Active Directory-aktivitet loggar Innehållspaketet fel</span><span class="sxs-lookup"><span data-stu-id="dbcd6-103">Troubleshooting Azure Active Directory Activity logs content pack errors</span></span> 


<span data-ttu-id="dbcd6-104">När du arbetar med Power BI-Innehållspaketet för Azure Active Directory-förhandsgranskning, är det möjligt att du får följande fel:</span><span class="sxs-lookup"><span data-stu-id="dbcd6-104">When working with the Power BI Content Pack for Azure Active Directory Preview, it is possible that you run into the following errors:</span></span> 

- [<span data-ttu-id="dbcd6-105">Uppdateringen misslyckades</span><span class="sxs-lookup"><span data-stu-id="dbcd6-105">Refresh failed</span></span>](active-directory-reporting-troubleshoot-content-pack.md#refresh-failed) 
- [<span data-ttu-id="dbcd6-106">Det gick inte att uppdatera datakällans autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="dbcd6-106">Failed to update data source credentials</span></span>](active-directory-reporting-troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [<span data-ttu-id="dbcd6-107">Importen av data tar för lång</span><span class="sxs-lookup"><span data-stu-id="dbcd6-107">Importing of data is taking too long</span></span>](active-directory-reporting-troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
<span data-ttu-id="dbcd6-108">Det här avsnittet ger information om möjliga orsaker och hur du korrigera felen.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-108">This topic provides you with information about the possible causes and how to fix these errors.</span></span>
 
## <a name="refresh-failed"></a><span data-ttu-id="dbcd6-109">Uppdateringen misslyckades</span><span class="sxs-lookup"><span data-stu-id="dbcd6-109">Refresh failed</span></span> 
 
<span data-ttu-id="dbcd6-110">**Hur det här felet är anslutna**: e-post från Power BI- eller felstatus i uppdateringshistoriken.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-110">**How this error is surfaced**: Email from Power BI or failed status in the refresh history.</span></span> 


| <span data-ttu-id="dbcd6-111">Orsak</span><span class="sxs-lookup"><span data-stu-id="dbcd6-111">Cause</span></span> | <span data-ttu-id="dbcd6-112">Hur du löser</span><span class="sxs-lookup"><span data-stu-id="dbcd6-112">How to fix</span></span> |
| ---   | ---        |
| <span data-ttu-id="dbcd6-113">Uppdatera fel kan bero på fel när autentiseringsuppgifterna för användare som ansluter till Innehållspaketet har återställts men inte uppdateras i anslutningsinställningarna för den av Innehållspaketet.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-113">Refresh failure errors can be caused when the credentials of the users connecting to the content pack have been reset but not updated in the connection settings of the of the content pack.</span></span> | <span data-ttu-id="dbcd6-114">Leta reda på datauppsättningen motsvarar infopanelen loggar aktivitet för Azure Active Directory (Azure Active Directory aktivitetsloggar), Välj schemalägga en uppdatering ange dina autentiseringsuppgifter för Azure AD i Power BI.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-114">In Power BI, locate the dataset corresponding to the Azure Active Directory Activity logs dashboard (Azure Active Directory Activity logs), choose schedule refresh, and then enter your Azure AD credentials.</span></span> |
| <span data-ttu-id="dbcd6-115">En uppdatering kan misslyckas på grund av dataproblem med i det underliggande Innehållspaketet.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-115">A refresh can fail due to data issues in the underlying content pack.</span></span> | <span data-ttu-id="dbcd6-116">Filen ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-116">File a support ticket.</span></span> <span data-ttu-id="dbcd6-117">Mer information finns i [få support för Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="dbcd6-117">For more details, see [How to get support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 
 
## <a name="failed-to-update-data-source-credentials"></a><span data-ttu-id="dbcd6-118">Det gick inte att uppdatera datakällans autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="dbcd6-118">Failed to update data source credentials</span></span> 
 
<span data-ttu-id="dbcd6-119">**Hur det här felet är anslutna**: I Power BI, när du ansluter till Innehållspaketet Azure Active Directory-aktivitet loggar (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="dbcd6-119">**How this error is surfaced**: In Power BI, when you are connecting to the Azure Active Directory Activity logs (preview) content pack.</span></span> 

| <span data-ttu-id="dbcd6-120">Orsak</span><span class="sxs-lookup"><span data-stu-id="dbcd6-120">Cause</span></span> | <span data-ttu-id="dbcd6-121">Hur du löser</span><span class="sxs-lookup"><span data-stu-id="dbcd6-121">How to fix</span></span> |
| ---   | ---        |
| <span data-ttu-id="dbcd6-122">Den anslutande användaren är varken en global administratör eller en läsare säkerhet eller säkerhet-administratör.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-122">The connecting user is neither a global admin nor a security reader or a security admin.</span></span> | <span data-ttu-id="dbcd6-123">Använd ett konto som är antingen en global administratör eller en läsare säkerhet eller en administratör för säkerhet för att komma åt innehållspaket.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-123">Use an account that is either a global admin or a security reader or a security admin to access the content packs.</span></span> |
| <span data-ttu-id="dbcd6-124">Din klient är inte en Premium-klientorganisation eller har inte minst en användare med Premium-licens filen.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-124">Your tenant is not a Premium tenant or doesn't have at least one user with Premium license File.</span></span> | <span data-ttu-id="dbcd6-125">Filen ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-125">File a support ticket.</span></span> <span data-ttu-id="dbcd6-126">Mer information finns i [få support för Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="dbcd6-126">For more details, see [How to get support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 

 

## <a name="importing-of-data-is-taking-too-long"></a><span data-ttu-id="dbcd6-127">Importen av data tar för lång</span><span class="sxs-lookup"><span data-stu-id="dbcd6-127">Importing of data is taking too long</span></span> 
 
<span data-ttu-id="dbcd6-128">**Hur det här felet är anslutna**: I Power BI när du har anslutit till ditt innehållspaket dataimporten börjar förbereda din instrumentpanel för Azure Active Directory aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-128">**How this error is surfaced**: In Power BI, once you have connected your content pack, the data import process starts to prepare your dashboard for Azure Active Directory Activity log.</span></span> <span data-ttu-id="dbcd6-129">Du ser meddelandet ”:*importerar data...* ”</span><span class="sxs-lookup"><span data-stu-id="dbcd6-129">You see the message: “*Importing data...*”</span></span>  

| <span data-ttu-id="dbcd6-130">Orsak</span><span class="sxs-lookup"><span data-stu-id="dbcd6-130">Cause</span></span> | <span data-ttu-id="dbcd6-131">Hur du löser</span><span class="sxs-lookup"><span data-stu-id="dbcd6-131">How to fix</span></span> |
| ---   | ---        |
| <span data-ttu-id="dbcd6-132">Beroende på storleken på din klient kan kan det här steget ta allt från några minuter till 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-132">Depending on the size of your tenant, this step could take anywhere from a few mins to 30 minutes.</span></span> | <span data-ttu-id="dbcd6-133">Bara vara öppen.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-133">Just be patient.</span></span> <span data-ttu-id="dbcd6-134">Om meddelandet inte ändras till visar instrumentpanelen inom en timme, kontrollera filen ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="dbcd6-134">If the message does not change to showing your dashboard within an hour, please file a support ticket.</span></span> <span data-ttu-id="dbcd6-135">Mer information finns i [få support för Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="dbcd6-135">For more details, see [How to get support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|

## <a name="next-steps"></a><span data-ttu-id="dbcd6-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dbcd6-136">Next steps</span></span>

<span data-ttu-id="dbcd6-137">Om du vill installera Power BI-Innehållspaketet för Azure Active Directory-förhandsgranskning klickar du på [här](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span><span class="sxs-lookup"><span data-stu-id="dbcd6-137">To install the Power BI Content Pack for Azure Active Directory Preview, click [here](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span></span>


