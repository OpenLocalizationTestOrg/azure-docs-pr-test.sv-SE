---
title: Anslut en personlig enhet till din organisation | Microsoft Docs
description: "Beskriver hur användarna kan registrera sina personliga enheter för Windows 10 till företagsnätverket och innehåller stegen för distributionen för en BYOD-scenario."
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
ms.openlocfilehash: 9418365ea18b065551448742b21c8b17a1749fc8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-personal-device-to-your-organization"></a><span data-ttu-id="a2afc-103">Anslut en personlig enhet till din organisation</span><span class="sxs-lookup"><span data-stu-id="a2afc-103">Join a personal device to your organization</span></span>
## <a name="to-join-a-windows-10-device-to-your-organization"></a><span data-ttu-id="a2afc-104">Att ansluta till en Windows 10-enhet till din organisation</span><span class="sxs-lookup"><span data-stu-id="a2afc-104">To join a Windows 10 device to your organization</span></span>
1. <span data-ttu-id="a2afc-105">Från den **starta** väljer du **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="a2afc-105">From the **Start** menu, select **Settings**.</span></span>
2. <span data-ttu-id="a2afc-106">Välj **konton**, och klicka sedan på **ditt konto**.</span><span class="sxs-lookup"><span data-stu-id="a2afc-106">Select **Accounts**, and then click **Your account**.</span></span>
3. <span data-ttu-id="a2afc-107">Klicka på **lägga till arbetet eller skolan konto**, och skriv sedan i ditt organisationskonto.</span><span class="sxs-lookup"><span data-stu-id="a2afc-107">Click **Add Work or School account**, and then type in your organizational account.</span></span>
4. <span data-ttu-id="a2afc-108">Ange ditt användarnamn och lösenord på sidan logga in för din organisation och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2afc-108">On the sign-in page for your organization, enter your user name and password, and then click **OK**.</span></span>
5. <span data-ttu-id="a2afc-109">Du uppmanas att en utmaning för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="a2afc-109">You will be prompted for a multi-factor authentication challenge.</span></span> <span data-ttu-id="a2afc-110">(Den här utmaningen kan konfigureras av en IT-administratör.)</span><span class="sxs-lookup"><span data-stu-id="a2afc-110">(This challenge is configurable by an IT administrator.)</span></span>
6. <span data-ttu-id="a2afc-111">Azure Active Directory (AD Azure) kontrollerar om enheten kräver registrering för hantering av mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="a2afc-111">Azure Active Directory (Azure AD) checks whether the device requires mobile device management enrollment.</span></span>
7. <span data-ttu-id="a2afc-112">Windows registrerar enheten i organisationens katalog i Azure AD och registrerar den i hantering av mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="a2afc-112">Windows registers the device in the organization’s directory in Azure AD and enrolls it in mobile device management, if appropriate.</span></span>
8. <span data-ttu-id="a2afc-113">Om du använder en hanterad går Windows du till skrivbordet via automatisk inloggning.</span><span class="sxs-lookup"><span data-stu-id="a2afc-113">If you are a managed user, Windows takes you to the desktop through the automatic sign-in.</span></span>
9. <span data-ttu-id="a2afc-114">Om du är en federerad användare kommer du tas till en Windows-inloggning skärm att ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a2afc-114">If you are a federated user, you will be taken to a Windows sign-in screen to enter your credentials.</span></span>

## <a name="additional-information"></a><span data-ttu-id="a2afc-115">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="a2afc-115">Additional information</span></span>
* [<span data-ttu-id="a2afc-116">Windows 10 för företaget: Sätt att använda enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="a2afc-116">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="a2afc-117">Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="a2afc-117">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="a2afc-118">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="a2afc-118">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="a2afc-119">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="a2afc-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="a2afc-120">Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10</span><span class="sxs-lookup"><span data-stu-id="a2afc-120">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="a2afc-121">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="a2afc-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

