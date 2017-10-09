---
title: "aaaAzure Active Directory Reporting vanliga frågor och svar | Microsoft Docs"
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
ms.openlocfilehash: be65a05574ea3b5b190cd02a96b211c571ba70bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-faq"></a><span data-ttu-id="43009-103">Azure Active Directory reporting vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="43009-103">Azure Active Directory reporting FAQ</span></span>

<span data-ttu-id="43009-104">Den här artikeln innehåller svar toofrequently frågor (FAQ) och svar om Azure Active Directory reporting.</span><span class="sxs-lookup"><span data-stu-id="43009-104">This article includes answers toofrequently asked questions (FAQs) about Azure Active Directory reporting.</span></span>  
<span data-ttu-id="43009-105">Mer information finns i [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="43009-105">For more details, see [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span> 

<span data-ttu-id="43009-106">**F: Vad är hello datalagring för aktivitetsloggar (granskning och inloggningar) hello Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="43009-106">**Q: What is hello data retention for activity logs (Audit and Sign-ins) in hello Azure portal?**</span></span> 

<span data-ttu-id="43009-107">**S:** vi erbjuder 7 dagars data för kunden ledigt och genom att växla tooan Azure AD Premium 1 eller 2 för Premium-licens, du kan komma åt data för in too30 dagar.</span><span class="sxs-lookup"><span data-stu-id="43009-107">**A:** We provide 7 days of data for our free customers and by switching tooan Azure AD Premium 1 or Premium 2 license, you can access data for up too30 days.</span></span> <span data-ttu-id="43009-108">Mer information om kvarhållning finns [rapportkvarhållningsregler i Azure Active Directory](active-directory-reporting-retention.md)</span><span class="sxs-lookup"><span data-stu-id="43009-108">For more details on retention, see [Azure Active Directory report retention policies](active-directory-reporting-retention.md)</span></span>

--- 

<span data-ttu-id="43009-109">**F: hur lång tid tar det tills hello aktivitetsdata visas när jag har slutfört min uppgiften?**</span><span class="sxs-lookup"><span data-stu-id="43009-109">**Q: How long does it take until I can see hello Activity data after I have completed my task?**</span></span>

<span data-ttu-id="43009-110">**S:** granskningsloggarna för aktivitet har en fördröjning mellan 15 minuter tooan timme.</span><span class="sxs-lookup"><span data-stu-id="43009-110">**A:** Audit activity logs have a latency ranging from 15 mins tooan hour.</span></span> <span data-ttu-id="43009-111">Inloggningsaktivitet loggar har en fördröjning mellan 15 minuter för de flesta poster och uppåt too2 timmar innan några poster.</span><span class="sxs-lookup"><span data-stu-id="43009-111">Sign-in activity logs have a latency ranging from 15 mins for most records and up too2 hours for a few records.</span></span>

---

<span data-ttu-id="43009-112">**F: måste toobe en global administratör toosee hello aktivitet loggar hello Azure-portalen eller tooget data via hello API?**</span><span class="sxs-lookup"><span data-stu-id="43009-112">**Q: Do I need toobe a global admin toosee hello activity logs in hello Azure Portal or tooget data through hello API?**</span></span>

<span data-ttu-id="43009-113">**S:** Nej.</span><span class="sxs-lookup"><span data-stu-id="43009-113">**A:** No.</span></span> <span data-ttu-id="43009-114">Du kan antingen vara en **säkerhet Reader**, **säkerhet Admin** eller en **Global administratör** toosee rapporterar data i Azure-portalen eller genom att öppna den via hello API.</span><span class="sxs-lookup"><span data-stu-id="43009-114">You can either be a **Security Reader**, a **Security Admin** or a **Global Admin** toosee reporting data in Azure Portal or by accessing it through hello API.</span></span>

---

<span data-ttu-id="43009-115">**F: kan jag få logginformation för Office 365 aktiviteten via hello Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="43009-115">**Q: Can I get Office 365 activity log information through hello Azure portal?**</span></span>

<span data-ttu-id="43009-116">**S:** även om aktiviteten för Office 365 och Azure AD aktivitet loggar resursen mycket hello directory resurser om du vill att en fullständig överblick över Hej aktivitetsloggar för Office 365, bör du gå toohello logginformation för Office 365 Admin Center tooget Office 365-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="43009-116">**A:** Even though Office 365 activity and Azure AD activity logs share a lot of hello directory resources, if you want a full view of hello Office 365 activity logs, you should go toohello Office 365 Admin Center tooget Office 365 Activity log information.</span></span>

---


<span data-ttu-id="43009-117">**F: som API: er ska jag använda tooget information om Office 365 aktivitetsloggar?**</span><span class="sxs-lookup"><span data-stu-id="43009-117">**Q: Which APIs do I use tooget information about Office 365 Activity logs?**</span></span>

<span data-ttu-id="43009-118">**S:** Använd hello Office 365 Management API: er tooaccess hello [Office 365 aktivitet loggar via ett API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span><span class="sxs-lookup"><span data-stu-id="43009-118">**A:** Use hello Office 365 Management APIs tooaccess hello [Office 365 Activity logs through an API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span></span>

---

<span data-ttu-id="43009-119">**F: hur många poster som jag kan hämta från Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="43009-119">**Q: How many records I can download from Azure portal?**</span></span>

<span data-ttu-id="43009-120">**S:** kan du hämta too120K poster från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="43009-120">**A:** You can download up too120K records from hello Azure portal.</span></span> <span data-ttu-id="43009-121">hello-poster sorteras efter *senaste* och som standard får hello senaste 120 K poster.</span><span class="sxs-lookup"><span data-stu-id="43009-121">hello records are sorted by *most recent* and by default, you get hello most recent 120K records.</span></span> 

---

<span data-ttu-id="43009-122">**F: hur många poster kan fråga via hello aktiviteter API?**</span><span class="sxs-lookup"><span data-stu-id="43009-122">**Q: How many records can I query through hello activities API?**</span></span>

<span data-ttu-id="43009-123">**S:** du kan fråga efter too1 miljoner poster (om du inte använder hello operatorn top, som sorterar hello post efter de senaste).</span><span class="sxs-lookup"><span data-stu-id="43009-123">**A:** You can query up too1 million records (if you don’t use hello top operator, which sorts hello record by most recent).</span></span> <span data-ttu-id="43009-124">Om du använder hello ”top” operator, kan du fråga efter too500K poster.</span><span class="sxs-lookup"><span data-stu-id="43009-124">If you do use hello “top” operator, you can query up too500K records.</span></span> <span data-ttu-id="43009-125">Du kan hitta exempelfrågor på hur toouse hello API här [här](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="43009-125">You can find sample queries on how toouse hello API here [here](active-directory-reporting-api-getting-started.md).</span></span>

---

<span data-ttu-id="43009-126">**F: hur får jag en premium-licens?**</span><span class="sxs-lookup"><span data-stu-id="43009-126">**Q: How do I get a premium license?**</span></span>

<span data-ttu-id="43009-127">**S:** finns [komma igång med Azure Active Directory Premium](active-directory-get-started-premium.md) för en fråga toothis.</span><span class="sxs-lookup"><span data-stu-id="43009-127">**A:** See [Getting started with Azure Active Directory Premium](active-directory-get-started-premium.md) for an answer toothis question.</span></span>

---

<span data-ttu-id="43009-128">**F: hur snart bör se aktiviteter data när en premium-licens?**</span><span class="sxs-lookup"><span data-stu-id="43009-128">**Q: How soon should I see activities data after getting a premium license?**</span></span>

<span data-ttu-id="43009-129">**S:** om du redan har data som aktiviteter som en kostnadsfri licens, så du kan se hello samma data.</span><span class="sxs-lookup"><span data-stu-id="43009-129">**A:** If you already have activities data as a free license, then you can see hello same data.</span></span> <span data-ttu-id="43009-130">Om du inte har några data, tar en eller två dagar.</span><span class="sxs-lookup"><span data-stu-id="43009-130">If you don’t have any data, then it will take one or two days.</span></span>

---

<span data-ttu-id="43009-131">**F: kan jag se senaste månaden data när en Azure AD premium-licens?**</span><span class="sxs-lookup"><span data-stu-id="43009-131">**Q: Do I see last month's data after getting an Azure AD premium license?**</span></span>

<span data-ttu-id="43009-132">**S:** om du har nyligen har bytt tooa Premium version (inklusive en utvärderingsversion), kan du se data in too7 dagar från början.</span><span class="sxs-lookup"><span data-stu-id="43009-132">**A:** If you have recently switched tooa Premium version (including a trial version), you can see data up too7 days initially.</span></span> <span data-ttu-id="43009-133">När data ackumuleras visas in too30 dagar.</span><span class="sxs-lookup"><span data-stu-id="43009-133">When data accumulates, you will see up too30 days.</span></span>

---

<span data-ttu-id="43009-134">**F: det finns en risk händelse i Identity Protection men jag ser inte motsvarande inloggning i hello alla inloggningar. Förväntas detta?**</span><span class="sxs-lookup"><span data-stu-id="43009-134">**Q: There is a risk event in Identity Protection but I’m not seeing corresponding sign-in in hello all sign-ins. Is this expected?**</span></span>

<span data-ttu-id="43009-135">**S:** Ja, identitetsskydd utvärderar risk för alla flöden för autentisering om om vara interaktiva eller icke-interaktivt.</span><span class="sxs-lookup"><span data-stu-id="43009-135">**A:** Yes, Identity Protection evaluates risk for all authentication flows whether if be interactive or non-interactive.</span></span> <span data-ttu-id="43009-136">Alla inloggningar endast rapporten innehåller dock endast hello interaktiva inloggningar.</span><span class="sxs-lookup"><span data-stu-id="43009-136">However, all sign-ins only report shows only hello interactive sign-ins.</span></span>

---

<span data-ttu-id="43009-137">**F: hur kan jag hämta hello ”användare som flaggats för risk” rapporten i Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="43009-137">**Q: How can I download hello “Users flagged for risk” report in Azure portal?**</span></span>

<span data-ttu-id="43009-138">**S:** hello alternativet toodownload *användare som har flaggats för risk* rapporten läggs snart.</span><span class="sxs-lookup"><span data-stu-id="43009-138">**A:** hello option toodownload *Users flagged for risk* report will be added soon.</span></span>

---

<span data-ttu-id="43009-139">**F: hur vet jag varför en inloggning eller en användare som har flaggats riskfyllda i hello Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="43009-139">**Q: How do I know why a sign-in or a user was flagged risky in hello Azure portal?**</span></span>

<span data-ttu-id="43009-140">**S:** Premium edition kunder kan läsa mer om hello underliggande riskhändelser genom att klicka på hello användare i ”användare som flaggats för risk” eller genom att klicka på hello ”riskfyllda inloggningar”.</span><span class="sxs-lookup"><span data-stu-id="43009-140">**A:** Premium edition customers can learn more about hello underlying risk events by clicking on hello user in “Users flagged for risk” or by clicking on hello “Risky sign-ins”.</span></span> <span data-ttu-id="43009-141">Ledigt och grundläggande edition kunder få toosee hello vilka användare och inloggningar utan hello underliggande händelseinformation för risk.</span><span class="sxs-lookup"><span data-stu-id="43009-141">Free and Basic edition customers get toosee hello at-risk users and sign-ins without hello underlying risk event information.</span></span>

---

<span data-ttu-id="43009-142">**F: hur beräknas IP-adresser i hello inloggningar och riskfyllda inloggningar rapporten?**</span><span class="sxs-lookup"><span data-stu-id="43009-142">**Q: How are IP addresses calculated in hello sign-ins and risky sign-ins report??**</span></span>

<span data-ttu-id="43009-143">**S:** IP-adresser utfärdas så att det finns ingen slutgiltiga anslutning mellan en IP-adress och där hello datorn med adressen finns fysiskt.</span><span class="sxs-lookup"><span data-stu-id="43009-143">**A:** IP addresses are issued in such a way that there is no definitive connection between an IP address and where hello computer with that address is physically located.</span></span> <span data-ttu-id="43009-144">Detta är komplicerade av faktorer, till exempel mobila providers och VPN-anslutningar utfärda IP-adresser från central pooler ofta mycket långt där hello klientenheten verkligen används.</span><span class="sxs-lookup"><span data-stu-id="43009-144">This is complicated by factors such as mobile providers and VPNs issuing IP addresses from central pools often very far from where hello client device is actually used.</span></span> <span data-ttu-id="43009-145">Hello ovan är, konvertera IP-adress tooa fysisk plats en bästa prestanda baserat på spårningar, registerdata, omvänd sökningar och annan information.</span><span class="sxs-lookup"><span data-stu-id="43009-145">Given hello above, converting IP address tooa physical location is a best effort based on traces, registry data, reverse look ups and other information.</span></span> 

---
