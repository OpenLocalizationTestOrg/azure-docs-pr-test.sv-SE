---
title: "Kom igång Azure MFA i molnet | Microsoft Docs"
description: "Det här är sidan om Microsoft Azure Multi-Factor Authentication som beskriver hur du kommer igång med Azure MFA i molnet."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6b2e6549-1a26-4666-9c4a-cbe5d64c4e66
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/24/2017
ms.author: kgremban
ms.openlocfilehash: 19f3228b874fc4e37bf83388dae4341428226482
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a><span data-ttu-id="7a1e5-103">Komma igång med Azure Multi-Factor Authentication i molnet</span><span class="sxs-lookup"><span data-stu-id="7a1e5-103">Getting started with Azure Multi-Factor Authentication in the cloud</span></span>
<span data-ttu-id="7a1e5-104">I den här artikeln beskrivs hur du kommer igång med Azure Multi-Factor Authentication i molnet.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-104">This article walks through how to get started using Azure Multi-Factor Authentication in the cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="7a1e5-105">Följande dokumentation innehåller information om hur du aktiverar användare med hjälp av **den klassiska Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-105">The following documentation provides information on how to enable users using the **Azure Classic Portal**.</span></span> <span data-ttu-id="7a1e5-106">Information om hur du konfigurerar Azure Multi-Factor Authentication för O365-användare finns i [Konfigurera Multi-Factor Authentication för Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)</span><span class="sxs-lookup"><span data-stu-id="7a1e5-106">If you are looking for information on how to set up Azure Multi-Factor Authentication for O365 users, see [Set up multi-factor authentication for Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)</span></span>

![MFA i molnet](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisite"></a><span data-ttu-id="7a1e5-108">Krav</span><span class="sxs-lookup"><span data-stu-id="7a1e5-108">Prerequisite</span></span>
<span data-ttu-id="7a1e5-109">[Registrera dig för en Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/) – Om du inte redan har en Azure-prenumeration måste du registrera dig för en.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-109">[Sign up for an Azure subscription](https://azure.microsoft.com/pricing/free-trial/) - If you do not already have an Azure subscription, you need to sign-up for one.</span></span> <span data-ttu-id="7a1e5-110">Om du precis börjat använda Azure MFA kan du använda en utvärderingsprenumeration.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-110">If you are just starting out and using Azure MFA you can use a trial subscription</span></span>

## <a name="enable-azure-multi-factor-authentication"></a><span data-ttu-id="7a1e5-111">Aktivera Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="7a1e5-111">Enable Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="7a1e5-112">Så länge användarna har licenser som inkluderar Azure Multi-Factor Authentication behöver du inte göra något för att aktivera Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-112">As long as your users have licenses that include Azure Multi-Factor Authentication, there's nothing that you need to do to turn on Azure MFA.</span></span> <span data-ttu-id="7a1e5-113">Du kan börja med att kräva tvåstegsverifiering för enskilda användare.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-113">You can start requiring two-step verification on an individual user basis.</span></span> <span data-ttu-id="7a1e5-114">Följande licenser aktiverar Azure MFA:</span><span class="sxs-lookup"><span data-stu-id="7a1e5-114">The licenses that enable Azure MFA are:</span></span>
- <span data-ttu-id="7a1e5-115">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="7a1e5-115">Azure Multi-Factor Authentication</span></span>
- <span data-ttu-id="7a1e5-116">Azure Active Directory Premium</span><span class="sxs-lookup"><span data-stu-id="7a1e5-116">Azure Active Directory Premium</span></span>
- <span data-ttu-id="7a1e5-117">Enterprise Mobility + Security</span><span class="sxs-lookup"><span data-stu-id="7a1e5-117">Enterprise Mobility + Security</span></span>

<span data-ttu-id="7a1e5-118">Om du saknar någon av dessa tre licenser, eller om du inte har tillräckligt med licenser för alla användare, är det också ok.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-118">If you don't have one of these three licenses, or you don't have enough licenses to cover all of your users, that's ok too.</span></span> <span data-ttu-id="7a1e5-119">Du behöver bara utföra ett extra steg och [Skapa en Multi-Factor Authentication-provider](multi-factor-authentication-get-started-auth-provider.md) i din katalog.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-119">You just have to do an extra step and [Create a Multi-Factor Auth Provider](multi-factor-authentication-get-started-auth-provider.md) in your directory.</span></span>

## <a name="turn-on-two-step-verification-for-users"></a><span data-ttu-id="7a1e5-120">Aktivera tvåstegsverifiering för användare</span><span class="sxs-lookup"><span data-stu-id="7a1e5-120">Turn on two-step verification for users</span></span>

<span data-ttu-id="7a1e5-121">Använd något av tillvägagångssätten som beskrivs i avsnittet om [hur du kräver tvåstegsverifiering för en användare eller grupp](multi-factor-authentication-get-started-user-states.md) för att börja använda Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-121">Use one of the procedures listed in [How to require two-step verification for a user or group](multi-factor-authentication-get-started-user-states.md) to start using Azure MFA.</span></span> <span data-ttu-id="7a1e5-122">Du kan välja att tillämpa tvåstegsverifiering för alla inloggningar eller du kan skapa principer för villkorlig åtkomst för att kräva tvåstegsverifiering endast när det är viktigt för dig.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-122">You can choose to enforce two-step verification for all sign-ins, or you can create conditional access policies to require two-step verification only when it matters to you.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a1e5-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a1e5-123">Next Steps</span></span>
<span data-ttu-id="7a1e5-124">Nu när du har konfigurerat Multi-Factor Authentication i molnet kan du konfigurera din distribution.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-124">Now that you have set up Azure Multi-Factor Authentication in the cloud, you can configure and set up your deployment.</span></span> <span data-ttu-id="7a1e5-125">Se [Konfigurera Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="7a1e5-125">See [Configuring Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md) for more details.</span></span>

