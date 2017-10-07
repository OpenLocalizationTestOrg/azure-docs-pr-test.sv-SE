---
title: "aaaGet igång Azure MFA i molnet hello | Microsoft Docs"
description: "Det här är hello Microsoft Azure Multi-Factor authentication sida som beskriver hur tooget igång med Azure MFA i molnet hello."
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
ms.openlocfilehash: a4aaf44bf08d96f2baad155072fdd6e0e727ce8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-in-hello-cloud"></a><span data-ttu-id="4d472-103">Komma igång med Azure Multi-Factor Authentication i molnet hello</span><span class="sxs-lookup"><span data-stu-id="4d472-103">Getting started with Azure Multi-Factor Authentication in hello cloud</span></span>
<span data-ttu-id="4d472-104">Den här artikeln beskrivs hur tooget igång med Azure Multi-Factor Authentication i molnet hello.</span><span class="sxs-lookup"><span data-stu-id="4d472-104">This article walks through how tooget started using Azure Multi-Factor Authentication in hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="4d472-105">hello följande dokumentation innehåller information om hur tooenable användare som använder hello **klassiska Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="4d472-105">hello following documentation provides information on how tooenable users using hello **Azure Classic Portal**.</span></span> <span data-ttu-id="4d472-106">Om du letar efter information om hur tooset in Azure Multi-Factor Authentication för O365-användare kan se [ställer in multifaktorautentisering för Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)</span><span class="sxs-lookup"><span data-stu-id="4d472-106">If you are looking for information on how tooset up Azure Multi-Factor Authentication for O365 users, see [Set up multi-factor authentication for Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)</span></span>

![MFA i hello moln](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisite"></a><span data-ttu-id="4d472-108">Krav</span><span class="sxs-lookup"><span data-stu-id="4d472-108">Prerequisite</span></span>
<span data-ttu-id="4d472-109">[Registrera dig för en Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/) -om du inte redan har en Azure-prenumeration behöver du toosign upp för en.</span><span class="sxs-lookup"><span data-stu-id="4d472-109">[Sign up for an Azure subscription](https://azure.microsoft.com/pricing/free-trial/) - If you do not already have an Azure subscription, you need toosign-up for one.</span></span> <span data-ttu-id="4d472-110">Om du precis börjat använda Azure MFA kan du använda en utvärderingsprenumeration.</span><span class="sxs-lookup"><span data-stu-id="4d472-110">If you are just starting out and using Azure MFA you can use a trial subscription</span></span>

## <a name="enable-azure-multi-factor-authentication"></a><span data-ttu-id="4d472-111">Aktivera Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="4d472-111">Enable Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="4d472-112">Så länge som användarna har licenser som innehåller Azure Multi-Factor Authentication, finns det inget att du behöver toodo tooturn på Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="4d472-112">As long as your users have licenses that include Azure Multi-Factor Authentication, there's nothing that you need toodo tooturn on Azure MFA.</span></span> <span data-ttu-id="4d472-113">Du kan börja med att kräva tvåstegsverifiering för enskilda användare.</span><span class="sxs-lookup"><span data-stu-id="4d472-113">You can start requiring two-step verification on an individual user basis.</span></span> <span data-ttu-id="4d472-114">hello licenser som gör att Azure MFA är:</span><span class="sxs-lookup"><span data-stu-id="4d472-114">hello licenses that enable Azure MFA are:</span></span>
- <span data-ttu-id="4d472-115">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="4d472-115">Azure Multi-Factor Authentication</span></span>
- <span data-ttu-id="4d472-116">Azure Active Directory Premium</span><span class="sxs-lookup"><span data-stu-id="4d472-116">Azure Active Directory Premium</span></span>
- <span data-ttu-id="4d472-117">Enterprise Mobility + Security</span><span class="sxs-lookup"><span data-stu-id="4d472-117">Enterprise Mobility + Security</span></span>

<span data-ttu-id="4d472-118">Om du inte har någon av dessa tre licenser, eller om du inte har tillräckligt med licenser toocover alla användare, det är ok för.</span><span class="sxs-lookup"><span data-stu-id="4d472-118">If you don't have one of these three licenses, or you don't have enough licenses toocover all of your users, that's ok too.</span></span> <span data-ttu-id="4d472-119">Du precis har toodo ett extra steg och [skapa en leverantör av Multifaktorautent](multi-factor-authentication-get-started-auth-provider.md) i din katalog.</span><span class="sxs-lookup"><span data-stu-id="4d472-119">You just have toodo an extra step and [Create a Multi-Factor Auth Provider](multi-factor-authentication-get-started-auth-provider.md) in your directory.</span></span>

## <a name="turn-on-two-step-verification-for-users"></a><span data-ttu-id="4d472-120">Aktivera tvåstegsverifiering för användare</span><span class="sxs-lookup"><span data-stu-id="4d472-120">Turn on two-step verification for users</span></span>

<span data-ttu-id="4d472-121">Använd någon av hello procedurer som anges i [hur toorequire tvåstegsverifiering för en användare eller grupp](multi-factor-authentication-get-started-user-states.md) toostart med hjälp av Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="4d472-121">Use one of hello procedures listed in [How toorequire two-step verification for a user or group](multi-factor-authentication-get-started-user-states.md) toostart using Azure MFA.</span></span> <span data-ttu-id="4d472-122">Du kan välja tooenforce tvåstegsverifiering för alla inloggningar eller du kan skapa tvåstegsverifiering för villkorlig åtkomst principer toorequire endast när det gäller tooyou.</span><span class="sxs-lookup"><span data-stu-id="4d472-122">You can choose tooenforce two-step verification for all sign-ins, or you can create conditional access policies toorequire two-step verification only when it matters tooyou.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d472-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d472-123">Next Steps</span></span>
<span data-ttu-id="4d472-124">Nu när du har konfigurerat Azure Multi-Factor Authentication i molnet hello, konfigurerar och konfigurera din distribution.</span><span class="sxs-lookup"><span data-stu-id="4d472-124">Now that you have set up Azure Multi-Factor Authentication in hello cloud, you can configure and set up your deployment.</span></span> <span data-ttu-id="4d472-125">Se [Konfigurera Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="4d472-125">See [Configuring Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md) for more details.</span></span>

