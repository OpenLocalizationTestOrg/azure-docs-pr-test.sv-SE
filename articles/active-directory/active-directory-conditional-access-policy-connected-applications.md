---
title: "Konfigurera principer för Azure Active Directory enhetsbaserad villkorlig åtkomst | Microsoft Docs"
description: "Lär dig hur du konfigurerar principer för Azure Active Directory enhetsbaserad villkorlig åtkomst."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: a26c40351c6b982fd90acb4bf06220ef3f79f399
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a><span data-ttu-id="8101c-103">Konfigurera principer för Azure Active Directory enhetsbaserad villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="8101c-103">Configure Azure Active Directory device-based conditional access policies</span></span>

<span data-ttu-id="8101c-104">Med [villkorlig åtkomst i Azure Active Directory (AD Azure)](active-directory-conditional-access-azure-portal.md), du kan finjustera hur behöriga användare kan komma åt dina resurser.</span><span class="sxs-lookup"><span data-stu-id="8101c-104">With [Azure Active Directory (Azure AD) conditional access](active-directory-conditional-access-azure-portal.md), you can fine-tune how authorized users can access your resources.</span></span> <span data-ttu-id="8101c-105">Exempelvis kan du begränsa åtkomst till vissa resurser till betrodda enheter.</span><span class="sxs-lookup"><span data-stu-id="8101c-105">For example, you limit the access to certain resources to trusted devices.</span></span> <span data-ttu-id="8101c-106">En princip för villkorlig åtkomst som kräver en betrodd enhet kallas även för principen för enhetsbaserad villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8101c-106">A conditional access policy that requires a trusted device is also known as device-based conditional access policy.</span></span>

<span data-ttu-id="8101c-107">Det här avsnittet ger information om hur du konfigurerar principer för enhetsbaserad villkorlig åtkomst för Azure AD-anslutna program.</span><span class="sxs-lookup"><span data-stu-id="8101c-107">This topic provides you with information on how to configure device-based conditional access policies for Azure AD-connected applications.</span></span> 


## <a name="before-you-begin"></a><span data-ttu-id="8101c-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="8101c-108">Before you begin</span></span>

<span data-ttu-id="8101c-109">Enhetsbaserad villkorlig åtkomst ties **villkorlig åtkomst i Azure AD** och **Azure AD-enhetshantering tillsammans**.</span><span class="sxs-lookup"><span data-stu-id="8101c-109">Device-based conditional access ties **Azure AD conditional access** and **Azure AD device management together**.</span></span> <span data-ttu-id="8101c-110">Om du inte är bekant med något av dessa områden ännu, bör du först läsa i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="8101c-110">If you are not familiar with one of these areas yet, you should read the following topics, first:</span></span>

- <span data-ttu-id="8101c-111">**[Villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  -det här avsnittet ger en översikt över relaterade terminologi och villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8101c-111">**[Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)** - This topic provides you with a conceptual overview of conditional access and the related terminology.</span></span>

- <span data-ttu-id="8101c-112">**[Introduktion till hantering av enheter i Azure Active Directory](device-management-introduction.md)**  -det här avsnittet innehåller en översikt över de olika alternativ som du måste ansluta enheter med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8101c-112">**[Introduction to device management in Azure Active Directory](device-management-introduction.md)** - This topic gives you an overview of the various options you have to connect devices with Azure AD.</span></span> 


## <a name="trusted-devices"></a><span data-ttu-id="8101c-113">Betrodda enheter</span><span class="sxs-lookup"><span data-stu-id="8101c-113">Trusted devices</span></span>

<span data-ttu-id="8101c-114">I en mobile första, molnet först värld kan Azure Active Directory enkel inloggning till enheter, appar och tjänster från var som helst.</span><span class="sxs-lookup"><span data-stu-id="8101c-114">In a mobile-first, cloud-first world, Azure Active Directory enables single sign-on to devices, apps, and services from anywhere.</span></span> <span data-ttu-id="8101c-115">För vissa kanske resurser i din miljö och bevilja åtkomst till rätt användare inte tillräckligt bra.</span><span class="sxs-lookup"><span data-stu-id="8101c-115">For certain resources in your environment, granting access to the right users might not be good enough.</span></span> <span data-ttu-id="8101c-116">Förutom rätt användare kan du även kräva en betrodd enhet som används för att komma åt en resurs.</span><span class="sxs-lookup"><span data-stu-id="8101c-116">In addition to the right users, you might also require a trusted device to be used to access a resource.</span></span> <span data-ttu-id="8101c-117">I din miljö kan du definiera en betrodd enhet är baserad på följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="8101c-117">In your environment, you can define what a trusted device is based on the following components:</span></span>

- <span data-ttu-id="8101c-118">Den [enhetsplattformar](active-directory-conditional-access-azure-portal.md#device-platforms) på en enhet</span><span class="sxs-lookup"><span data-stu-id="8101c-118">The [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) on a device</span></span>
- <span data-ttu-id="8101c-119">Om en enhet är kompatibel</span><span class="sxs-lookup"><span data-stu-id="8101c-119">Whether a device is compliant</span></span>
- <span data-ttu-id="8101c-120">Om en enhet är ansluten till domänen</span><span class="sxs-lookup"><span data-stu-id="8101c-120">Whether a device is domain-joined</span></span> 

<span data-ttu-id="8101c-121">Den [enhetsplattformar](active-directory-conditional-access-azure-portal.md#device-platforms) kännetecknas av operativsystemet som körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="8101c-121">The [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) is characterized by the operating system that is running on your device.</span></span> <span data-ttu-id="8101c-122">Du kan begränsa åtkomst till vissa resurser till specifika enhetsplattformar i principen för enhetsbaserad villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8101c-122">In your device-based conditional access policy, you can limit access to certain resources to specific device platforms.</span></span>



<span data-ttu-id="8101c-123">Du kan kräva betrodda enheter har markerats som kompatibel i en princip med enhetsbaserad villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8101c-123">In a device-based conditional access policy, you can require trusted devices to be marked as compliant.</span></span>

![Molnappar](./media/active-directory-conditional-access-policy-connected-applications/24.png)

<span data-ttu-id="8101c-125">Enheter kan markeras som kompatibla i katalogen genom att:</span><span class="sxs-lookup"><span data-stu-id="8101c-125">Devices can be marked as compliant in the directory by:</span></span>

- <span data-ttu-id="8101c-126">Intune</span><span class="sxs-lookup"><span data-stu-id="8101c-126">Intune</span></span> 
- <span data-ttu-id="8101c-127">Ett system från tredje part mobila enheter som kan integreras med Azure AD</span><span class="sxs-lookup"><span data-stu-id="8101c-127">A third-party mobile device management system that integrates with Azure AD</span></span>  

<span data-ttu-id="8101c-128">Endast enheter som är anslutna till Azure AD kan markeras som kompatibel.</span><span class="sxs-lookup"><span data-stu-id="8101c-128">Only devices that are connected to Azure AD can be marked as compliant.</span></span> <span data-ttu-id="8101c-129">För att ansluta en enhet till Azure Active Directory, har du följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="8101c-129">To connect a device to Azure Active Directory, you have the following options:</span></span> 

- <span data-ttu-id="8101c-130">Azure AD som registrerade</span><span class="sxs-lookup"><span data-stu-id="8101c-130">Azure AD registered</span></span>
- <span data-ttu-id="8101c-131">Azure AD ansluten</span><span class="sxs-lookup"><span data-stu-id="8101c-131">Azure AD joined</span></span>
- <span data-ttu-id="8101c-132">Azure AD-hybridlösning ansluten</span><span class="sxs-lookup"><span data-stu-id="8101c-132">Hybrid Azure AD joined</span></span>

    ![Molnappar](./media/active-directory-conditional-access-policy-connected-applications/26.png)

<span data-ttu-id="8101c-134">Om du har en lokal Active Directory (AD) storleken kan överväga du att enheter som inte är ansluten till Azure AD, men ansluten till din AD vara betrodd.</span><span class="sxs-lookup"><span data-stu-id="8101c-134">If you have an on-premises Active Directory (AD) footprint, you might consider devices that are not connected to Azure AD but joined to your AD to be trusted.</span></span>

![Molnappar](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a><span data-ttu-id="8101c-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8101c-136">Next steps</span></span>

<span data-ttu-id="8101c-137">Innan du konfigurerar en princip för enhetsbaserad villkorlig åtkomst i din miljö bör du ta en titt på den [bästa praxis för villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="8101c-137">Before configuring a device-based conditional access policy in your environment, you should take a look at the [best practices for conditional access in Azure Active Directory](active-directory-conditional-access-best-practices.md).</span></span>

