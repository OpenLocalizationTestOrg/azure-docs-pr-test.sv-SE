---
title: "Azure Active Directory B2C: Region tillgänglighet & data land | Microsoft Docs"
description: "Ett ämne på hello typer av Azure Active Directory B2C-klienter"
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
ms.openlocfilehash: d382921fe9d62183b6d52c47d62f952dabd116c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a><span data-ttu-id="241c8-103">Azure Active Directory B2C: Region tillgänglighet & data land</span><span class="sxs-lookup"><span data-stu-id="241c8-103">Azure Active Directory B2C: Region availability & data residency</span></span>
<span data-ttu-id="241c8-104">Regional tillgänglighet och data land är två mycket olika begrepp som tillämpas på olika sätt tooAzure AD B2C från hello resten av Azure.</span><span class="sxs-lookup"><span data-stu-id="241c8-104">Region availability and data residency are two very different concepts that apply differently tooAzure AD B2C from hello rest of Azure.</span></span> <span data-ttu-id="241c8-105">Den här artikeln förklarar hello skillnaderna mellan dessa två koncept och jämföra hur de används tooAzure jämfört med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="241c8-105">This article will explain hello differences between these two concepts and compare how they apply tooAzure versus Azure AD B2C.</span></span>

## <a name="summary"></a><span data-ttu-id="241c8-106">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="241c8-106">Summary</span></span>
<span data-ttu-id="241c8-107">Azure AD B2C är **allmänt tillgänglig över hela världen** med hello alternativet för **data land i USA eller Europa**.</span><span class="sxs-lookup"><span data-stu-id="241c8-107">Azure AD B2C is **generally available worldwide** with hello option for **data residency in United States or Europe**.</span></span>

## <a name="concepts"></a><span data-ttu-id="241c8-108">Koncept</span><span class="sxs-lookup"><span data-stu-id="241c8-108">Concepts</span></span>
* <span data-ttu-id="241c8-109">**Regional tillgänglighet** refererar toowhere en tjänst är tillgänglig för användning.</span><span class="sxs-lookup"><span data-stu-id="241c8-109">**Region availability** refers toowhere a service is available for use.</span></span>
* <span data-ttu-id="241c8-110">**Data land** refererar toowhere användare data lagras.</span><span class="sxs-lookup"><span data-stu-id="241c8-110">**Data residency** refers toowhere user data is stored.</span></span>

## <a name="region-availability"></a><span data-ttu-id="241c8-111">Regional tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="241c8-111">Region availability</span></span>
<span data-ttu-id="241c8-112">Azure AD B2C finns över hela världen via hello offentliga Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="241c8-112">Azure AD B2C is available worldwide via hello Azure public cloud.</span></span> 

<span data-ttu-id="241c8-113">Detta skiljer sig från hello modell följer de flesta andra Azure-tjänster som ihop tillgänglighet med data land.</span><span class="sxs-lookup"><span data-stu-id="241c8-113">This differs from hello model most other Azure services follow which couple availability with data residency.</span></span> <span data-ttu-id="241c8-114">Du kan se ett exempel på detta i både Azure [produkter tillgängliga efter Region](https://azure.microsoft.com/regions/services/) sida och hello [Active Directory B2C prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="241c8-114">You can see examples of this in both Azure's [Products Available By Region](https://azure.microsoft.com/regions/services/) page and hello [Active Directory B2C pricing calculator](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span></span>

## <a name="data-residency"></a><span data-ttu-id="241c8-115">Dataplacering</span><span class="sxs-lookup"><span data-stu-id="241c8-115">Data residency</span></span>
<span data-ttu-id="241c8-116">Azure AD B2C lagrar användardata i USA och Europa.</span><span class="sxs-lookup"><span data-stu-id="241c8-116">Azure AD B2C stores user data in either United States or Europe.</span></span>

<span data-ttu-id="241c8-117">Data land fastställs utifrån vilken land/region har valts när [att skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="241c8-117">Data residency is determined based on which country/region is selected when [creating an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

![Skärmbild av en klient för förhandsgranskning](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

<span data-ttu-id="241c8-119">Data finns i hello USA för hello följande länder:</span><span class="sxs-lookup"><span data-stu-id="241c8-119">Data resides in hello United States for hello following countries/regions:</span></span>

> <span data-ttu-id="241c8-120">USA, Kanada, Costa Rica, Dominikanska republiken, Salvadoranska, Guatemala, Mexiko, Panama, registreringen och Trinidad och Tobago</span><span class="sxs-lookup"><span data-stu-id="241c8-120">United States, Canada, Costa Rica, Dominican Republic, El Salvador, Guatemala, Mexico, Panama, Puerto Rico and Trinidad & Tobago</span></span>

<span data-ttu-id="241c8-121">Data som finns i Europa för hello följande länder:</span><span class="sxs-lookup"><span data-stu-id="241c8-121">Data resides in Europe for hello following countries/regions:</span></span>

> <span data-ttu-id="241c8-122">Algeriet, Österrike, Azerbajdzjan, Bahrain, Vitryssland, Belgien, Bulgarien, Kroatien, Cypern, Tjeckien, Danmark, Egypten, Estland, Finland, Frankrike, Tyskland, Grekland, Ungern, Islands, Irland, Israel, Italien, Jordanien, Kazakstan, Kenya, Kuwait, Lettland, Libanon Liechtenstein Lituania Luxemburg Makedonien FYROM, Malta, Montenegro, Marocko, Nederländerna, Nigeria, Norge, Oman, Pakistan, Polen, Portugal, Qatar, Rumänien, Ryssland, Saudiarabien, Serbien, Slovakien, Slovenien, Sydafrika, Spanien, Sverige, Schweiz. Tunisien, Turkiet, Ukraina, Förenade Arabemiraten och Storbritannien.</span><span class="sxs-lookup"><span data-stu-id="241c8-122">Algeria, Austria, Azerbaijan, Bahrain, Belarus, Belgium, Bulgaria, Croatia, Cyprus, Czech Republic, Denmark, Egypt, Estonia, Finland, France, Germany, Greece, Hungary, Iceland, Ireland, Israel, Italy, Jordan, Kazakhstan, Kenya, Kuwait, Lativa, Lebanon, Liechtenstein, Lituania, Luxembourg, Macedonia FYRO, Malta, Montenegro, Morocco, Netherlands, Nigeria, Norway, Oman, Pakistan, Poland, Portugal, Qatar, Romania, Russia, Saudi Arabia, Serbia, Slovakia, Slovenia, South Africa, Spain, Sweden, Switzerland, Tunisia, Turkey, Ukraine, United Arab Emirates and United Kingdom.</span></span>

<span data-ttu-id="241c8-123">hello återstående länder finns i hello processen att läggs toohello lista.</span><span class="sxs-lookup"><span data-stu-id="241c8-123">hello remaining countries/regions are in hello process of being added toohello list.</span></span>  <span data-ttu-id="241c8-124">Du kan fortfarande använda Azure AD B2C genom att välja någon av hello länder ovan för tillfället.</span><span class="sxs-lookup"><span data-stu-id="241c8-124">For now, you can still use Azure AD B2C by picking any of hello countries/regions above.</span></span>

> <span data-ttu-id="241c8-125">Afghanistan, Argentina, Australien, Brasilien, underordnad, Colombia, Ecuador, Hongkong SAR, Indien, Indonesien, Irak, Japan, Korea, Malaysia, Nya Zeeland, Paraguay, Peru, Filippinerna, Singapore, Sri Lanka, Taiwan, Thailand, Uruguay och Venezuela.</span><span class="sxs-lookup"><span data-stu-id="241c8-125">Afghanistan, Argentina, Australia, Brazil, Chile, Colombia, Ecuador, Hong Kong SAR, India, Indonesia, Iraq, Japan, Korea, Malaysia, New Zealand, Paraguay, Peru, Philippines, Singapore, Sri Lanka, Taiwan, Thailand, Uruguay and Venezuela.</span></span>

## <a name="preview-tenant"></a><span data-ttu-id="241c8-126">Förhandsgranska klient</span><span class="sxs-lookup"><span data-stu-id="241c8-126">Preview tenant</span></span>
<span data-ttu-id="241c8-127">Om du har skapat en B2C-klient under Azure AD B2C-förhandsversionen, är förmodligen som din **klient typen** står **Preview klient**.</span><span class="sxs-lookup"><span data-stu-id="241c8-127">If you had created a B2C tenant during Azure AD B2C's preview period, it is likely that your **Tenant type** says **Preview tenant**.</span></span> <span data-ttu-id="241c8-128">Om så är fallet hello måste du använda din klient endast för utveckling och testning och inte för appar i produktion.</span><span class="sxs-lookup"><span data-stu-id="241c8-128">If this is hello case, you MUST use your tenant only for development and testing purposes, and NOT for production apps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="241c8-129">Det finns ingen migreringssökväg från en förhandsgranskning B2C-klient tooa produktionsskala B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="241c8-129">There is no migration path from a preview B2C tenant tooa production-scale B2C tenant.</span></span> <span data-ttu-id="241c8-130">Observera att det finns kända problem när du tar bort en förhandsversion B2C-klient och skapa en produktionsskala B2C-klienten med hello samma domännamn.</span><span class="sxs-lookup"><span data-stu-id="241c8-130">Note that there are known issues when you delete a preview B2C tenant and re-create a production-scale B2C tenant with hello same domain name.</span></span> <span data-ttu-id="241c8-131">Du har toocreate en produktionsskala B2C-klient med ett annat domännamn.</span><span class="sxs-lookup"><span data-stu-id="241c8-131">You have toocreate a production-scale B2C tenant with a different domain name.</span></span>


![Skärmbild av en klient för förhandsgranskning](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
