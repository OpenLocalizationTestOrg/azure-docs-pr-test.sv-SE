---
title: "aaaProvide säkerhet kontaktinformation i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooprovide säkerhet kontaktinformation i Azure Security Center."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 6c378c9c84c6e4fce70b2541e4cc121018700269
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provide-security-contact-details-in-azure-security-center"></a><span data-ttu-id="54157-103">Ange säkerhet kontaktinformation i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="54157-103">Provide security contact details in Azure Security Center</span></span>
<span data-ttu-id="54157-104">Azure Security Center kommer rekommenderar att du förser säkerhet kontaktinformation för din Azure-prenumeration om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="54157-104">Azure Security Center will recommend that you provide security contact details for your Azure subscription if you haven’t already.</span></span> <span data-ttu-id="54157-105">Den här informationen kommer att användas av Microsoft toocontact om hello Microsoft Security Response Center (MSRC) upptäcker att din kundinformation används redan av en olaglig eller obehörig part.</span><span class="sxs-lookup"><span data-stu-id="54157-105">This information will be used by Microsoft toocontact you if hello Microsoft Security Response Center (MSRC) discovers that your customer data has been accessed by an unlawful or unauthorized party.</span></span> <span data-ttu-id="54157-106">MSRC utför väljer säkerhetsövervakning hello Azure-nätverk och infrastruktur och tar emot hot intelligence och missbruk klagomål från tredje part.</span><span class="sxs-lookup"><span data-stu-id="54157-106">MSRC performs select security monitoring of hello Azure network and infrastructure and receives threat intelligence and abuse complaints from third parties.</span></span>

<span data-ttu-id="54157-107">Ett e-postmeddelande skickas på hello första dagliga förekomsten av en avisering och endast för varningar med hög angelägenhetsgrad.</span><span class="sxs-lookup"><span data-stu-id="54157-107">An email notification is sent on hello first daily occurrence of an alert and only for high severity alerts.</span></span> <span data-ttu-id="54157-108">E-postinställningar kan bara konfigureras för prenumerationsprinciper.</span><span class="sxs-lookup"><span data-stu-id="54157-108">Email preferences can only be configured for subscription policies.</span></span> <span data-ttu-id="54157-109">Resursgrupper inom en prenumeration ärver inställningarna.</span><span class="sxs-lookup"><span data-stu-id="54157-109">Resource groups within a subscription will inherit these settings.</span></span>

> [!NOTE]
> <span data-ttu-id="54157-110">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="54157-110">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="54157-111">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="54157-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="54157-112">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="54157-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="54157-113">I hello **rekommendationer** bladet väljer **ange säkerhet kontaktinformation**.</span><span class="sxs-lookup"><span data-stu-id="54157-113">In hello **Recommendations** blade, select **Provide security contact details**.</span></span>
   <span data-ttu-id="54157-114">![Ger säkerhet kontakt][1]</span><span class="sxs-lookup"><span data-stu-id="54157-114">![Provide security contact][1]</span></span>
2. <span data-ttu-id="54157-115">Då öppnas bladet hello **ange säkerhet kontaktinformation**.</span><span class="sxs-lookup"><span data-stu-id="54157-115">This opens hello blade **Provide security contact details**.</span></span> <span data-ttu-id="54157-116">Välj Azure-prenumeration hello tooprovide kontaktinformation.</span><span class="sxs-lookup"><span data-stu-id="54157-116">Select hello Azure subscription tooprovide contact information on.</span></span>
   <span data-ttu-id="54157-117">![Ange säkerhetskontaktinformation][2]</span><span class="sxs-lookup"><span data-stu-id="54157-117">![Provide security contact details][2]</span></span>
3. <span data-ttu-id="54157-118">En andra **ange säkerhet kontaktinformation** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="54157-118">A second **Provide security contact details** blade opens.</span></span>

   * <span data-ttu-id="54157-119">Ange hello säkerhet kontakta e-postadress eller adresser som avgränsas med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="54157-119">Enter hello security contact email address or addresses separated by commas.</span></span> <span data-ttu-id="54157-120">Det finns inte en begränsa toohello antalet e-postadresser som du kan ange.</span><span class="sxs-lookup"><span data-stu-id="54157-120">There is not a limit toohello number of email addresses that you can enter.</span></span>
   * <span data-ttu-id="54157-121">Ange en säkerhet kontakta internationella telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="54157-121">Enter one security contact international phone number.</span></span>
   * <span data-ttu-id="54157-122">tooreceive e-post om varningar med hög angelägenhetsgrad, aktivera hello **Skicka mig e-postmeddelanden om aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="54157-122">tooreceive emails about high severity alerts, turn on hello option **Send me emails about alerts**.</span></span>
   * <span data-ttu-id="54157-123">I framtida hello har du hello alternativet toosend e-postaviseringar toosubscription ägare.</span><span class="sxs-lookup"><span data-stu-id="54157-123">In hello future, you will have hello option toosend email notifications toosubscription owners.</span></span> <span data-ttu-id="54157-124">Det här alternativet är för närvarande nedtonade.</span><span class="sxs-lookup"><span data-stu-id="54157-124">This option is currently grayed out.</span></span>
   * <span data-ttu-id="54157-125">Välj **OK** tooapply hello säkerhet kontakta information tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="54157-125">Select **OK** tooapply hello security contact information tooyour subscription.</span></span>

## <a name="see-also"></a><span data-ttu-id="54157-126">Se även</span><span class="sxs-lookup"><span data-stu-id="54157-126">See also</span></span>
<span data-ttu-id="54157-127">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="54157-127">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="54157-128">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="54157-128">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="54157-129">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="54157-129">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="54157-130">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="54157-130">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="54157-131">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="54157-131">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="54157-132">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="54157-132">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="54157-133">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="54157-133">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="54157-134">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hello Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="54157-134">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
