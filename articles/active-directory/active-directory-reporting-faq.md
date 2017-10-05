---
title: "Azure Active Directory Reporting vanliga frågor och svar | Microsoft Docs"
description: "Azure Active Directory reporting vanliga frågor och svar."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: accf292f70bf0eafdefc00c3feeaf8e346605401
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-reporting-faq"></a><span data-ttu-id="79dcf-103">Azure Active Directory reporting vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="79dcf-103">Azure Active Directory reporting FAQ</span></span>

<span data-ttu-id="79dcf-104">Den här artikeln innehåller svar på vanliga frågor och svar (FAQ) om Azure Active Directory-rapportering.</span><span class="sxs-lookup"><span data-stu-id="79dcf-104">This article includes answers to frequently asked questions (FAQs) about Azure Active Directory reporting.</span></span>  
<span data-ttu-id="79dcf-105">Mer information finns i [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="79dcf-105">For more details, see [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span> 

<span data-ttu-id="79dcf-106">**F: Vad är datalagring för aktivitetsloggar (granskning och inloggningar) i Azure portal?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-106">**Q: What is the data retention for activity logs (Audit and Sign-ins) in the Azure portal?**</span></span> 

<span data-ttu-id="79dcf-107">**S:** vi erbjuder 7 dagars data för kunden ledigt och genom att växla till en Azure AD Premium 1 eller 2 för Premium-licens, du kan komma åt data i upp till 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="79dcf-107">**A:** We provide 7 days of data for our free customers and by switching to an Azure AD Premium 1 or Premium 2 license, you can access data for up to 30 days.</span></span> <span data-ttu-id="79dcf-108">Mer information om kvarhållning finns [rapportkvarhållningsregler i Azure Active Directory](active-directory-reporting-retention.md)</span><span class="sxs-lookup"><span data-stu-id="79dcf-108">For more details on retention, see [Azure Active Directory report retention policies](active-directory-reporting-retention.md)</span></span>

--- 

<span data-ttu-id="79dcf-109">**F: hur lång tid tar det förrän aktivitetsdata visas när jag har slutfört min uppgiften?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-109">**Q: How long does it take until I can see the Activity data after I have completed my task?**</span></span>

<span data-ttu-id="79dcf-110">**S:** granskningsloggarna för aktivitet har en fördröjning mellan 15 minuter till en timme.</span><span class="sxs-lookup"><span data-stu-id="79dcf-110">**A:** Audit activity logs have a latency ranging from 15 mins to an hour.</span></span> <span data-ttu-id="79dcf-111">Inloggningsaktivitet loggar har en fördröjning mellan 15 minuter för de flesta poster och upp till två timmar för några poster.</span><span class="sxs-lookup"><span data-stu-id="79dcf-111">Sign-in activity logs have a latency ranging from 15 mins for most records and up to 2 hours for a few records.</span></span>

---

<span data-ttu-id="79dcf-112">**F: måste vara en global administratör för att se aktiviteten loggar i Azure-portalen eller hämta data via API?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-112">**Q: Do I need to be a global admin to see the activity logs in the Azure Portal or to get data through the API?**</span></span>

<span data-ttu-id="79dcf-113">**S:** Nej.</span><span class="sxs-lookup"><span data-stu-id="79dcf-113">**A:** No.</span></span> <span data-ttu-id="79dcf-114">Du kan antingen vara en **säkerhet Reader**, **säkerhet Admin** eller en **Global administratör** att se data i Azure-portalen eller genom att öppna den via API-rapportering.</span><span class="sxs-lookup"><span data-stu-id="79dcf-114">You can either be a **Security Reader**, a **Security Admin** or a **Global Admin** to see reporting data in Azure Portal or by accessing it through the API.</span></span>

---

<span data-ttu-id="79dcf-115">**F: kan jag få logginformation för Office 365 aktiviteten via Azure portal?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-115">**Q: Can I get Office 365 activity log information through the Azure portal?**</span></span>

<span data-ttu-id="79dcf-116">**S:** även om aktiviteten för Office 365 och Azure AD aktivitet loggar resursen mycket directory-resurser om du vill att en fullständig överblick över aktivitetsloggar Office 365 bör du gå till Office 365 Admin Center få logginformation för Office 365-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="79dcf-116">**A:** Even though Office 365 activity and Azure AD activity logs share a lot of the directory resources, if you want a full view of the Office 365 activity logs, you should go to the Office 365 Admin Center to get Office 365 Activity log information.</span></span>

---


<span data-ttu-id="79dcf-117">**F: vilken API: er används för att få information om Office 365 aktivitetsloggar?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-117">**Q: Which APIs do I use to get information about Office 365 Activity logs?**</span></span>

<span data-ttu-id="79dcf-118">**S:** använda Office 365 Management-API: er för att komma åt den [Office 365 aktivitet loggar via ett API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span><span class="sxs-lookup"><span data-stu-id="79dcf-118">**A:** Use the Office 365 Management APIs to access the [Office 365 Activity logs through an API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span></span>

---

<span data-ttu-id="79dcf-119">**F: hur många poster som jag kan hämta från Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-119">**Q: How many records I can download from Azure portal?**</span></span>

<span data-ttu-id="79dcf-120">**S:** upp till 120 K poster kan hämtas från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="79dcf-120">**A:** You can download up to 120K records from the Azure portal.</span></span> <span data-ttu-id="79dcf-121">Poster sorteras efter *senaste* och som standard får de senaste 120 K posterna.</span><span class="sxs-lookup"><span data-stu-id="79dcf-121">The records are sorted by *most recent* and by default, you get the most recent 120K records.</span></span> 

---

<span data-ttu-id="79dcf-122">**F: hur många poster kan fråga via aktiviteter API?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-122">**Q: How many records can I query through the activities API?**</span></span>

<span data-ttu-id="79dcf-123">**S:** du kan fråga efter upp till 1 miljon poster (om du inte använder operatorn top som sorterar posten efter de senaste).</span><span class="sxs-lookup"><span data-stu-id="79dcf-123">**A:** You can query up to 1 million records (if you don’t use the top operator, which sorts the record by most recent).</span></span> <span data-ttu-id="79dcf-124">Om du använder operatorn ”top”, kan du fråga upp till 500 kB poster.</span><span class="sxs-lookup"><span data-stu-id="79dcf-124">If you do use the “top” operator, you can query up to 500K records.</span></span> <span data-ttu-id="79dcf-125">Du kan hitta Exempelfrågor för hur du använder API: et här [här](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="79dcf-125">You can find sample queries on how to use the API here [here](active-directory-reporting-api-getting-started.md).</span></span>

---

<span data-ttu-id="79dcf-126">**F: hur får jag en premium-licens?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-126">**Q: How do I get a premium license?**</span></span>

<span data-ttu-id="79dcf-127">**S:** finns [komma igång med Azure Active Directory Premium](active-directory-get-started-premium.md) för ett svar på frågan.</span><span class="sxs-lookup"><span data-stu-id="79dcf-127">**A:** See [Getting started with Azure Active Directory Premium](active-directory-get-started-premium.md) for an answer to this question.</span></span>

---

<span data-ttu-id="79dcf-128">**F: hur snart bör se aktiviteter data när en premium-licens?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-128">**Q: How soon should I see activities data after getting a premium license?**</span></span>

<span data-ttu-id="79dcf-129">**S:** om du redan har data som aktiviteter som en kostnadsfri licens, så du kan se samma data.</span><span class="sxs-lookup"><span data-stu-id="79dcf-129">**A:** If you already have activities data as a free license, then you can see the same data.</span></span> <span data-ttu-id="79dcf-130">Om du inte har några data, tar en eller två dagar.</span><span class="sxs-lookup"><span data-stu-id="79dcf-130">If you don’t have any data, then it will take one or two days.</span></span>

---

<span data-ttu-id="79dcf-131">**F: kan jag se senaste månaden data när en Azure AD premium-licens?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-131">**Q: Do I see last month's data after getting an Azure AD premium license?**</span></span>

<span data-ttu-id="79dcf-132">**S:** om du har nyligen bytt till en Premium-version (inklusive en utvärderingsversion), kan du se data upp till 7 dagar från början.</span><span class="sxs-lookup"><span data-stu-id="79dcf-132">**A:** If you have recently switched to a Premium version (including a trial version), you can see data up to 7 days initially.</span></span> <span data-ttu-id="79dcf-133">När data ackumuleras visas upp till 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="79dcf-133">When data accumulates, you will see up to 30 days.</span></span>

---

<span data-ttu-id="79dcf-134">**F: det finns en risk händelse i Identity Protection men jag ser inte motsvarande inloggning i alla inloggningar. Förväntas detta?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-134">**Q: There is a risk event in Identity Protection but I’m not seeing corresponding sign-in in the all sign-ins. Is this expected?**</span></span>

<span data-ttu-id="79dcf-135">**S:** Ja, identitetsskydd utvärderar risk för alla flöden för autentisering om om vara interaktiva eller icke-interaktivt.</span><span class="sxs-lookup"><span data-stu-id="79dcf-135">**A:** Yes, Identity Protection evaluates risk for all authentication flows whether if be interactive or non-interactive.</span></span> <span data-ttu-id="79dcf-136">Alla inloggningar endast rapporten innehåller dock bara de interaktiva inloggningarna.</span><span class="sxs-lookup"><span data-stu-id="79dcf-136">However, all sign-ins only report shows only the interactive sign-ins.</span></span>

---

<span data-ttu-id="79dcf-137">**F: hur kan jag hämta rapporten ”användare som flaggats för risk” i Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-137">**Q: How can I download the “Users flagged for risk” report in Azure portal?**</span></span>

<span data-ttu-id="79dcf-138">**S:** alternativet för att hämta *användare som har flaggats för risk* rapporten läggs snart.</span><span class="sxs-lookup"><span data-stu-id="79dcf-138">**A:** The option to download *Users flagged for risk* report will be added soon.</span></span>

---

<span data-ttu-id="79dcf-139">**F: hur vet jag varför en inloggning eller en användare som har flaggats riskfyllda i Azure portal?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-139">**Q: How do I know why a sign-in or a user was flagged risky in the Azure portal?**</span></span>

<span data-ttu-id="79dcf-140">**S:** Premium edition kunder kan läsa mer om de underliggande riskhändelser genom att klicka på användare i ”användare som flaggats för risk” eller genom att klicka på de ”riskfyllda inloggningarna”.</span><span class="sxs-lookup"><span data-stu-id="79dcf-140">**A:** Premium edition customers can learn more about the underlying risk events by clicking on the user in “Users flagged for risk” or by clicking on the “Risky sign-ins”.</span></span> <span data-ttu-id="79dcf-141">Ledigt och grundläggande edition kunder få se vilka användare och inloggningar utan underliggande händelseinformation för risk.</span><span class="sxs-lookup"><span data-stu-id="79dcf-141">Free and Basic edition customers get to see the at-risk users and sign-ins without the underlying risk event information.</span></span>

---

<span data-ttu-id="79dcf-142">**F: hur beräknas IP-adresser i inloggningar och riskfyllda inloggningar rapporten?**</span><span class="sxs-lookup"><span data-stu-id="79dcf-142">**Q: How are IP addresses calculated in the sign-ins and risky sign-ins report??**</span></span>

<span data-ttu-id="79dcf-143">**S:** IP-adresser utfärdas så att det finns ingen slutgiltiga anslutning mellan en IP-adress och där datorn med adressen finns fysiskt.</span><span class="sxs-lookup"><span data-stu-id="79dcf-143">**A:** IP addresses are issued in such a way that there is no definitive connection between an IP address and where the computer with that address is physically located.</span></span> <span data-ttu-id="79dcf-144">Detta är komplicerade av faktorer, till exempel mobila providers och VPN-anslutningar utfärda IP-adresser från central pooler ofta mycket långt där klientenheten verkligen används.</span><span class="sxs-lookup"><span data-stu-id="79dcf-144">This is complicated by factors such as mobile providers and VPNs issuing IP addresses from central pools often very far from where the client device is actually used.</span></span> <span data-ttu-id="79dcf-145">Ovanstående är, konverterar IP-adress till en fysisk plats en bästa prestanda baserat på spårningar, registerdata, omvänd sökningar och annan information.</span><span class="sxs-lookup"><span data-stu-id="79dcf-145">Given the above, converting IP address to a physical location is a best effort based on traces, registry data, reverse look ups and other information.</span></span> 

---
