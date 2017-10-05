---
title: "Azure Active Directory B2C: Region tillgänglighet & data land | Microsoft Docs"
description: "Ett ämne på vilka typer av Azure Active Directory B2C-klienter"
services: active-directory-b2c
documentationcenter: 
author: gsacavdm
manager: krassk
editor: bryanla
ms.assetid: 8a0644da-b825-4edc-8ce9-541c3c976afb
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: gsacavdm
ms.openlocfilehash: facd66f0324e382ea7609a035de8129ba433846f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a><span data-ttu-id="d0797-103">Azure Active Directory B2C: Region tillgänglighet & data land</span><span class="sxs-lookup"><span data-stu-id="d0797-103">Azure Active Directory B2C: Region availability & data residency</span></span>
<span data-ttu-id="d0797-104">Regional tillgänglighet och data land är två mycket olika begrepp som tillämpas på olika sätt att Azure AD B2C från resten av Azure.</span><span class="sxs-lookup"><span data-stu-id="d0797-104">Region availability and data residency are two very different concepts that apply differently to Azure AD B2C from the rest of Azure.</span></span> <span data-ttu-id="d0797-105">Den här artikeln beskrivs skillnaderna mellan dessa två koncept och jämföra hur de används med Azure jämfört med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d0797-105">This article will explain the differences between these two concepts and compare how they apply to Azure versus Azure AD B2C.</span></span>

## <a name="summary"></a><span data-ttu-id="d0797-106">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d0797-106">Summary</span></span>
<span data-ttu-id="d0797-107">Azure AD B2C är **allmänt tillgänglig över hela världen** med alternativet för **data land i USA eller Europa**.</span><span class="sxs-lookup"><span data-stu-id="d0797-107">Azure AD B2C is **generally available worldwide** with the option for **data residency in United States or Europe**.</span></span>

## <a name="concepts"></a><span data-ttu-id="d0797-108">Koncept</span><span class="sxs-lookup"><span data-stu-id="d0797-108">Concepts</span></span>
* <span data-ttu-id="d0797-109">**Regional tillgänglighet** avser där en tjänst är tillgänglig för användning.</span><span class="sxs-lookup"><span data-stu-id="d0797-109">**Region availability** refers to where a service is available for use.</span></span>
* <span data-ttu-id="d0797-110">**Data land** refererar till där användarinformationen är lagrad.</span><span class="sxs-lookup"><span data-stu-id="d0797-110">**Data residency** refers to where user data is stored.</span></span>

## <a name="region-availability"></a><span data-ttu-id="d0797-111">Regional tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="d0797-111">Region availability</span></span>
<span data-ttu-id="d0797-112">Azure AD B2C finns över hela världen via det offentliga Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="d0797-112">Azure AD B2C is available worldwide via the Azure public cloud.</span></span> 

<span data-ttu-id="d0797-113">Detta skiljer sig från modellen följer de flesta andra Azure-tjänster som ihop tillgänglighet med data land.</span><span class="sxs-lookup"><span data-stu-id="d0797-113">This differs from the model most other Azure services follow which couple availability with data residency.</span></span> <span data-ttu-id="d0797-114">Du kan se ett exempel på detta i både Azure [produkter tillgängliga efter Region](https://azure.microsoft.com/regions/services/) sidan och [Active Directory B2C prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="d0797-114">You can see examples of this in both Azure's [Products Available By Region](https://azure.microsoft.com/regions/services/) page and the [Active Directory B2C pricing calculator](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span></span>

## <a name="data-residency"></a><span data-ttu-id="d0797-115">Dataplacering</span><span class="sxs-lookup"><span data-stu-id="d0797-115">Data residency</span></span>
<span data-ttu-id="d0797-116">Azure AD B2C lagrar användardata i USA och Europa.</span><span class="sxs-lookup"><span data-stu-id="d0797-116">Azure AD B2C stores user data in either United States or Europe.</span></span>

<span data-ttu-id="d0797-117">Data land fastställs utifrån vilken land/region har valts när [att skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d0797-117">Data residency is determined based on which country/region is selected when [creating an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

![Skärmbild av en klient för förhandsgranskning](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

<span data-ttu-id="d0797-119">Data finns i USA för följande länder:</span><span class="sxs-lookup"><span data-stu-id="d0797-119">Data resides in the United States for the following countries/regions:</span></span>

> <span data-ttu-id="d0797-120">USA, Kanada, Costa Rica, Dominikanska republiken, Salvadoranska, Guatemala, Mexiko, Panama, registreringen och Trinidad och Tobago</span><span class="sxs-lookup"><span data-stu-id="d0797-120">United States, Canada, Costa Rica, Dominican Republic, El Salvador, Guatemala, Mexico, Panama, Puerto Rico and Trinidad & Tobago</span></span>

<span data-ttu-id="d0797-121">Data som finns i Europa för följande länder:</span><span class="sxs-lookup"><span data-stu-id="d0797-121">Data resides in Europe for the following countries/regions:</span></span>

> <span data-ttu-id="d0797-122">Algeriet, Österrike, Azerbajdzjan, Bahrain, Vitryssland, Belgien, Bulgarien, Kroatien, Cypern, Tjeckien, Danmark, Egypten, Estland, Finland, Frankrike, Tyskland, Grekland, Ungern, Islands, Irland, Israel, Italien, Jordanien, Kazakstan, Kenya, Kuwait, Lettland, Libanon Liechtenstein Lituania Luxemburg Makedonien FYROM, Malta, Montenegro, Marocko, Nederländerna, Nigeria, Norge, Oman, Pakistan, Polen, Portugal, Qatar, Rumänien, Ryssland, Saudiarabien, Serbien, Slovakien, Slovenien, Sydafrika, Spanien, Sverige, Schweiz. Tunisien, Turkiet, Ukraina, Förenade Arabemiraten och Storbritannien.</span><span class="sxs-lookup"><span data-stu-id="d0797-122">Algeria, Austria, Azerbaijan, Bahrain, Belarus, Belgium, Bulgaria, Croatia, Cyprus, Czech Republic, Denmark, Egypt, Estonia, Finland, France, Germany, Greece, Hungary, Iceland, Ireland, Israel, Italy, Jordan, Kazakhstan, Kenya, Kuwait, Lativa, Lebanon, Liechtenstein, Lituania, Luxembourg, Macedonia FYRO, Malta, Montenegro, Morocco, Netherlands, Nigeria, Norway, Oman, Pakistan, Poland, Portugal, Qatar, Romania, Russia, Saudi Arabia, Serbia, Slovakia, Slovenia, South Africa, Spain, Sweden, Switzerland, Tunisia, Turkey, Ukraine, United Arab Emirates and United Kingdom.</span></span>

<span data-ttu-id="d0797-123">Återstående länder håller läggs till i listan.</span><span class="sxs-lookup"><span data-stu-id="d0797-123">The remaining countries/regions are in the process of being added to the list.</span></span>  <span data-ttu-id="d0797-124">Du kan fortfarande använda Azure AD B2C genom att välja något av länder ovan för tillfället.</span><span class="sxs-lookup"><span data-stu-id="d0797-124">For now, you can still use Azure AD B2C by picking any of the countries/regions above.</span></span>

> <span data-ttu-id="d0797-125">Afghanistan, Argentina, Australien, Brasilien, underordnad, Colombia, Ecuador, Hongkong SAR, Indien, Indonesien, Irak, Japan, Korea, Malaysia, Nya Zeeland, Paraguay, Peru, Filippinerna, Singapore, Sri Lanka, Taiwan, Thailand, Uruguay och Venezuela.</span><span class="sxs-lookup"><span data-stu-id="d0797-125">Afghanistan, Argentina, Australia, Brazil, Chile, Colombia, Ecuador, Hong Kong SAR, India, Indonesia, Iraq, Japan, Korea, Malaysia, New Zealand, Paraguay, Peru, Philippines, Singapore, Sri Lanka, Taiwan, Thailand, Uruguay and Venezuela.</span></span>

## <a name="preview-tenant"></a><span data-ttu-id="d0797-126">Förhandsgranska klient</span><span class="sxs-lookup"><span data-stu-id="d0797-126">Preview tenant</span></span>
<span data-ttu-id="d0797-127">Om du har skapat en B2C-klient under Azure AD B2C-förhandsversionen, är förmodligen som din **klient typen** står **Preview klient**.</span><span class="sxs-lookup"><span data-stu-id="d0797-127">If you had created a B2C tenant during Azure AD B2C's preview period, it is likely that your **Tenant type** says **Preview tenant**.</span></span> <span data-ttu-id="d0797-128">Om så är fallet måste du använda din klient endast för utveckling och testning och inte för appar i produktion.</span><span class="sxs-lookup"><span data-stu-id="d0797-128">If this is the case, you MUST use your tenant only for development and testing purposes, and NOT for production apps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d0797-129">Det finns inga migreringsvägen från en förhandsgranskning B2C-klient till en produktionsskala B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="d0797-129">There is no migration path from a preview B2C tenant to a production-scale B2C tenant.</span></span> <span data-ttu-id="d0797-130">Observera att det finns kända problem när du tar bort en förhandsversion B2C-klient och återskapa en B2C-klient för produktion skala med samma domännamn.</span><span class="sxs-lookup"><span data-stu-id="d0797-130">Note that there are known issues when you delete a preview B2C tenant and re-create a production-scale B2C tenant with the same domain name.</span></span> <span data-ttu-id="d0797-131">Du måste skapa en produktionsskala B2C-klient med ett annat domännamn.</span><span class="sxs-lookup"><span data-stu-id="d0797-131">You have to create a production-scale B2C tenant with a different domain name.</span></span>


![Skärmbild av en klient för förhandsgranskning](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
