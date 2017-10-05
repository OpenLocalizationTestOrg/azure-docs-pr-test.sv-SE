---
title: "Ange säkerhet kontaktinformation i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur du ange säkerhet kontaktinformation i Azure Security Center."
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
ms.openlocfilehash: 1a6e5e915745dd3588fbc54b353daa947b1c4289
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="provide-security-contact-details-in-azure-security-center"></a><span data-ttu-id="38778-103">Ange säkerhet kontaktinformation i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="38778-103">Provide security contact details in Azure Security Center</span></span>
<span data-ttu-id="38778-104">Azure Security Center kommer rekommenderar att du förser säkerhet kontaktinformation för din Azure-prenumeration om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="38778-104">Azure Security Center will recommend that you provide security contact details for your Azure subscription if you haven’t already.</span></span> <span data-ttu-id="38778-105">Den här informationen används av Microsoft för att kontakta dig om Microsoft Security Response Center (MRSC) upptäcker att en obehörig part har kommit åt dina kunddata.</span><span class="sxs-lookup"><span data-stu-id="38778-105">This information will be used by Microsoft to contact you if the Microsoft Security Response Center (MSRC) discovers that your customer data has been accessed by an unlawful or unauthorized party.</span></span> <span data-ttu-id="38778-106">MSRC utför väljer säkerhetsövervakning av Azure-nätverk och infrastruktur och tar emot hot intelligence och missbruk klagomål från tredje part.</span><span class="sxs-lookup"><span data-stu-id="38778-106">MSRC performs select security monitoring of the Azure network and infrastructure and receives threat intelligence and abuse complaints from third parties.</span></span>

<span data-ttu-id="38778-107">Ett e-postmeddelande skickas på den första dagliga förekomsten av en avisering och endast för varningar med hög angelägenhetsgrad.</span><span class="sxs-lookup"><span data-stu-id="38778-107">An email notification is sent on the first daily occurrence of an alert and only for high severity alerts.</span></span> <span data-ttu-id="38778-108">E-postinställningar kan bara konfigureras för prenumerationsprinciper.</span><span class="sxs-lookup"><span data-stu-id="38778-108">Email preferences can only be configured for subscription policies.</span></span> <span data-ttu-id="38778-109">Resursgrupper inom en prenumeration ärver inställningarna.</span><span class="sxs-lookup"><span data-stu-id="38778-109">Resource groups within a subscription will inherit these settings.</span></span>

> [!NOTE]
> <span data-ttu-id="38778-110">I det här dokumentet beskrivs tjänsten genom en exempeldistribution.</span><span class="sxs-lookup"><span data-stu-id="38778-110">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="38778-111">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="38778-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="38778-112">Implementera rekommendationen</span><span class="sxs-lookup"><span data-stu-id="38778-112">Implement the recommendation</span></span>
1. <span data-ttu-id="38778-113">I den **rekommendationer** bladet väljer **ange säkerhet kontaktinformation**.</span><span class="sxs-lookup"><span data-stu-id="38778-113">In the **Recommendations** blade, select **Provide security contact details**.</span></span>
   <span data-ttu-id="38778-114">![Ger säkerhet kontakt][1]</span><span class="sxs-lookup"><span data-stu-id="38778-114">![Provide security contact][1]</span></span>
2. <span data-ttu-id="38778-115">Gör det öppnas bladet **ange säkerhet kontaktinformation**.</span><span class="sxs-lookup"><span data-stu-id="38778-115">This opens the blade **Provide security contact details**.</span></span> <span data-ttu-id="38778-116">Välj den Azure-prenumerationer anger du kontaktinformation på.</span><span class="sxs-lookup"><span data-stu-id="38778-116">Select the Azure subscription to provide contact information on.</span></span>
   <span data-ttu-id="38778-117">![Ange säkerhetskontaktinformation][2]</span><span class="sxs-lookup"><span data-stu-id="38778-117">![Provide security contact details][2]</span></span>
3. <span data-ttu-id="38778-118">En andra **ange säkerhet kontaktinformation** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="38778-118">A second **Provide security contact details** blade opens.</span></span>

   * <span data-ttu-id="38778-119">Ange säkerhet kontakta e-postadressen eller adresser som avgränsas med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="38778-119">Enter the security contact email address or addresses separated by commas.</span></span> <span data-ttu-id="38778-120">Det finns inte en gräns för antalet e-postadresser som du kan ange.</span><span class="sxs-lookup"><span data-stu-id="38778-120">There is not a limit to the number of email addresses that you can enter.</span></span>
   * <span data-ttu-id="38778-121">Ange en säkerhet kontakta internationella telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="38778-121">Enter one security contact international phone number.</span></span>
   * <span data-ttu-id="38778-122">Aktivera alternativet för att ta emot e-post om varningar med hög angelägenhetsgrad **Skicka mig e-postmeddelanden om aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="38778-122">To receive emails about high severity alerts, turn on the option **Send me emails about alerts**.</span></span>
   * <span data-ttu-id="38778-123">I framtiden, har du möjlighet att skicka e-postmeddelanden till prenumerationen ägare.</span><span class="sxs-lookup"><span data-stu-id="38778-123">In the future, you will have the option to send email notifications to subscription owners.</span></span> <span data-ttu-id="38778-124">Det här alternativet är för närvarande nedtonade.</span><span class="sxs-lookup"><span data-stu-id="38778-124">This option is currently grayed out.</span></span>
   * <span data-ttu-id="38778-125">Välj **OK** ska gälla säkerhet kontaktinformation för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="38778-125">Select **OK** to apply the security contact information to your subscription.</span></span>

## <a name="see-also"></a><span data-ttu-id="38778-126">Se även</span><span class="sxs-lookup"><span data-stu-id="38778-126">See also</span></span>
<span data-ttu-id="38778-127">I följande avsnitt kan du lära dig mer om Security Center:</span><span class="sxs-lookup"><span data-stu-id="38778-127">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="38778-128">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Här får du lära dig hur du ställer in säkerhetsprinciper för prenumerationer och resursgrupper i Azure.</span><span class="sxs-lookup"><span data-stu-id="38778-128">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="38778-129">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="38778-129">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="38778-130">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig att övervaka hälsotillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="38778-130">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="38778-131">[Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Här får du lära dig hur du hanterar och åtgärdar säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="38778-131">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="38778-132">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md): Här får du lära dig hur du övervakar dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="38778-132">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="38778-133">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md) – Här hittar du vanliga frågor och svar om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="38778-133">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="38778-134">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="38778-134">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
