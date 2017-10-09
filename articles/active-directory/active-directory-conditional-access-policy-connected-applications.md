---
title: "principer för aaaConfigure Azure Active Directory enhetsbaserad villkorlig åtkomst | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory enhetsbaserad villkorliga åtkomstprinciper."
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
ms.openlocfilehash: cc5923b8ccee4335442c17ef63b2ee6f098e104e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a><span data-ttu-id="9e8f4-103">Konfigurera principer för Azure Active Directory enhetsbaserad villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="9e8f4-103">Configure Azure Active Directory device-based conditional access policies</span></span>

<span data-ttu-id="9e8f4-104">Med [villkorlig åtkomst i Azure Active Directory (AD Azure)](active-directory-conditional-access-azure-portal.md), du kan finjustera hur behöriga användare kan komma åt dina resurser.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-104">With [Azure Active Directory (Azure AD) conditional access](active-directory-conditional-access-azure-portal.md), you can fine-tune how authorized users can access your resources.</span></span> <span data-ttu-id="9e8f4-105">Till exempel begränsa hello åtkomst toocertain resurser tootrusted enheter.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-105">For example, you limit hello access toocertain resources tootrusted devices.</span></span> <span data-ttu-id="9e8f4-106">En princip för villkorlig åtkomst som kräver en betrodd enhet kallas även för principen för enhetsbaserad villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-106">A conditional access policy that requires a trusted device is also known as device-based conditional access policy.</span></span>

<span data-ttu-id="9e8f4-107">Det här avsnittet ger information om hur tooconfigure enhetsbaserad villkorliga åtkomstprinciper för Azure AD-anslutna program.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-107">This topic provides you with information on how tooconfigure device-based conditional access policies for Azure AD-connected applications.</span></span> 


## <a name="before-you-begin"></a><span data-ttu-id="9e8f4-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="9e8f4-108">Before you begin</span></span>

<span data-ttu-id="9e8f4-109">Enhetsbaserad villkorlig åtkomst ties **villkorlig åtkomst i Azure AD** och **Azure AD-enhetshantering tillsammans**.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-109">Device-based conditional access ties **Azure AD conditional access** and **Azure AD device management together**.</span></span> <span data-ttu-id="9e8f4-110">Om du inte är bekant med något av dessa områden ännu, bör du läsa hello följande ämnen, först:</span><span class="sxs-lookup"><span data-stu-id="9e8f4-110">If you are not familiar with one of these areas yet, you should read hello following topics, first:</span></span>

- <span data-ttu-id="9e8f4-111">**[Villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  -det här avsnittet ger dig en översikt över villkorlig åtkomst och hello relaterade termer.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-111">**[Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)** - This topic provides you with a conceptual overview of conditional access and hello related terminology.</span></span>

- <span data-ttu-id="9e8f4-112">**[Introduktion toodevice management i Azure Active Directory](device-management-introduction.md)**  -det här avsnittet ger en översikt av hello olika alternativ du har tooconnect enheter med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-112">**[Introduction toodevice management in Azure Active Directory](device-management-introduction.md)** - This topic gives you an overview of hello various options you have tooconnect devices with Azure AD.</span></span> 


## <a name="trusted-devices"></a><span data-ttu-id="9e8f4-113">Betrodda enheter</span><span class="sxs-lookup"><span data-stu-id="9e8f4-113">Trusted devices</span></span>

<span data-ttu-id="9e8f4-114">I en mobile första, molnet först värld kan Azure Active Directory enkel inloggning toodevices, appar och tjänster från var som helst.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-114">In a mobile-first, cloud-first world, Azure Active Directory enables single sign-on toodevices, apps, and services from anywhere.</span></span> <span data-ttu-id="9e8f4-115">För vissa kanske resurser i din miljö, beviljar åtkomst toohello rätt användare inte tillräckligt bra.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-115">For certain resources in your environment, granting access toohello right users might not be good enough.</span></span> <span data-ttu-id="9e8f4-116">Dessutom toohello rätt användare, du kan även kräva en betrodd enhet toobe används tooaccess en resurs.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-116">In addition toohello right users, you might also require a trusted device toobe used tooaccess a resource.</span></span> <span data-ttu-id="9e8f4-117">I din miljö kan du definiera en betrodd enhet är baserad på hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="9e8f4-117">In your environment, you can define what a trusted device is based on hello following components:</span></span>

- <span data-ttu-id="9e8f4-118">Hej [enhetsplattformar](active-directory-conditional-access-azure-portal.md#device-platforms) på en enhet</span><span class="sxs-lookup"><span data-stu-id="9e8f4-118">hello [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) on a device</span></span>
- <span data-ttu-id="9e8f4-119">Om en enhet är kompatibel</span><span class="sxs-lookup"><span data-stu-id="9e8f4-119">Whether a device is compliant</span></span>
- <span data-ttu-id="9e8f4-120">Om en enhet är ansluten till domänen</span><span class="sxs-lookup"><span data-stu-id="9e8f4-120">Whether a device is domain-joined</span></span> 

<span data-ttu-id="9e8f4-121">Hej [enhetsplattformar](active-directory-conditional-access-azure-portal.md#device-platforms) kännetecknas av hello-operativsystemet som körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-121">hello [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) is characterized by hello operating system that is running on your device.</span></span> <span data-ttu-id="9e8f4-122">Du kan begränsa åtkomst toocertain resurser toospecific enhetsplattformar i principen för enhetsbaserad villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-122">In your device-based conditional access policy, you can limit access toocertain resources toospecific device platforms.</span></span>



<span data-ttu-id="9e8f4-123">Du kan kräva betrodda enheter toobe som markerats som kompatibel i en princip med enhetsbaserad villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-123">In a device-based conditional access policy, you can require trusted devices toobe marked as compliant.</span></span>

![Molnappar](./media/active-directory-conditional-access-policy-connected-applications/24.png)

<span data-ttu-id="9e8f4-125">Enheter kan markeras som kompatibla i hello katalog genom att:</span><span class="sxs-lookup"><span data-stu-id="9e8f4-125">Devices can be marked as compliant in hello directory by:</span></span>

- <span data-ttu-id="9e8f4-126">Intune</span><span class="sxs-lookup"><span data-stu-id="9e8f4-126">Intune</span></span> 
- <span data-ttu-id="9e8f4-127">Ett system från tredje part mobila enheter som kan integreras med Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e8f4-127">A third-party mobile device management system that integrates with Azure AD</span></span>  

<span data-ttu-id="9e8f4-128">Endast enheter som är anslutna tooAzure AD kan markeras som kompatibel.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-128">Only devices that are connected tooAzure AD can be marked as compliant.</span></span> <span data-ttu-id="9e8f4-129">tooconnect enhet-tooAzure Active Directory har hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="9e8f4-129">tooconnect a device tooAzure Active Directory, you have hello following options:</span></span> 

- <span data-ttu-id="9e8f4-130">Azure AD som registrerade</span><span class="sxs-lookup"><span data-stu-id="9e8f4-130">Azure AD registered</span></span>
- <span data-ttu-id="9e8f4-131">Azure AD ansluten</span><span class="sxs-lookup"><span data-stu-id="9e8f4-131">Azure AD joined</span></span>
- <span data-ttu-id="9e8f4-132">Azure AD-hybridlösning ansluten</span><span class="sxs-lookup"><span data-stu-id="9e8f4-132">Hybrid Azure AD joined</span></span>

    ![Molnappar](./media/active-directory-conditional-access-policy-connected-applications/26.png)

<span data-ttu-id="9e8f4-134">Om du har en lokal Active Directory (AD) storleken kan överväga du att enheter som inte är anslutna tooAzure AD men kopplade tooyour AD toobe betrodda.</span><span class="sxs-lookup"><span data-stu-id="9e8f4-134">If you have an on-premises Active Directory (AD) footprint, you might consider devices that are not connected tooAzure AD but joined tooyour AD toobe trusted.</span></span>

![Molnappar](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a><span data-ttu-id="9e8f4-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9e8f4-136">Next steps</span></span>

<span data-ttu-id="9e8f4-137">Innan du konfigurerar en princip för enhetsbaserad villkorlig åtkomst i din miljö bör du ta en titt på hello [bästa praxis för villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="9e8f4-137">Before configuring a device-based conditional access policy in your environment, you should take a look at hello [best practices for conditional access in Azure Active Directory](active-directory-conditional-access-best-practices.md).</span></span>

