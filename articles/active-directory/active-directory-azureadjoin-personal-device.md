---
title: aaaJoin en personlig enhet tooyour organisation | Microsoft Docs
description: "Beskriver hur användarna kan registrera sina personliga Windows 10 enheter tootheir företagsnätverket och innehåller distributionsstegen för en BYOD-scenario."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 9f3d38f5-1cfd-43d4-97da-4fed1255a1ff
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 979e2461dd9ad0438aa402a0a3223cc84c4c625c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-personal-device-tooyour-organization"></a><span data-ttu-id="ca839-103">Ansluta till en personlig enhet tooyour organisation</span><span class="sxs-lookup"><span data-stu-id="ca839-103">Join a personal device tooyour organization</span></span>
## <a name="toojoin-a-windows-10-device-tooyour-organization"></a><span data-ttu-id="ca839-104">toojoin Windows 10-enhet tooyour organisation</span><span class="sxs-lookup"><span data-stu-id="ca839-104">toojoin a Windows 10 device tooyour organization</span></span>
1. <span data-ttu-id="ca839-105">Från hello **starta** väljer du **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="ca839-105">From hello **Start** menu, select **Settings**.</span></span>
2. <span data-ttu-id="ca839-106">Välj **konton**, och klicka sedan på **ditt konto**.</span><span class="sxs-lookup"><span data-stu-id="ca839-106">Select **Accounts**, and then click **Your account**.</span></span>
3. <span data-ttu-id="ca839-107">Klicka på **lägga till arbetet eller skolan konto**, och skriv sedan i ditt organisationskonto.</span><span class="sxs-lookup"><span data-stu-id="ca839-107">Click **Add Work or School account**, and then type in your organizational account.</span></span>
4. <span data-ttu-id="ca839-108">Ange ditt användarnamn och lösenord på hello-inloggningssida för din organisation, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca839-108">On hello sign-in page for your organization, enter your user name and password, and then click **OK**.</span></span>
5. <span data-ttu-id="ca839-109">Du uppmanas att en utmaning för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="ca839-109">You will be prompted for a multi-factor authentication challenge.</span></span> <span data-ttu-id="ca839-110">(Den här utmaningen kan konfigureras av en IT-administratör.)</span><span class="sxs-lookup"><span data-stu-id="ca839-110">(This challenge is configurable by an IT administrator.)</span></span>
6. <span data-ttu-id="ca839-111">Azure Active Directory (AD Azure) kontrollerar om hello enheten kräver registrering för hantering av mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="ca839-111">Azure Active Directory (Azure AD) checks whether hello device requires mobile device management enrollment.</span></span>
7. <span data-ttu-id="ca839-112">Windows registrerar hello enheten i hello organisations katalog i Azure AD och registrerar den i hantering av mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="ca839-112">Windows registers hello device in hello organization’s directory in Azure AD and enrolls it in mobile device management, if appropriate.</span></span>
8. <span data-ttu-id="ca839-113">Om du använder en hanterad, tar Windows toohello via hello automatisk inloggning.</span><span class="sxs-lookup"><span data-stu-id="ca839-113">If you are a managed user, Windows takes you toohello desktop through hello automatic sign-in.</span></span>
9. <span data-ttu-id="ca839-114">Om du är en federerad användare kan du kommer att vidtas tooa Windows-inloggning skärmen tooenter dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ca839-114">If you are a federated user, you will be taken tooa Windows sign-in screen tooenter your credentials.</span></span>

## <a name="additional-information"></a><span data-ttu-id="ca839-115">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="ca839-115">Additional information</span></span>
* [<span data-ttu-id="ca839-116">Windows 10 för hello företaget: sätt toouse enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="ca839-116">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="ca839-117">Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="ca839-117">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="ca839-118">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="ca839-118">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="ca839-119">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="ca839-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="ca839-120">Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser</span><span class="sxs-lookup"><span data-stu-id="ca839-120">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="ca839-121">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="ca839-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

