---
title: "Windows 10 för företaget: sätt att använda enheter för arbete | Microsoft Docs"
description: "Översikt över hur du distribuerar Windows 10-enheter för företag och hur du integrerar med Azure Active Directory för Windows-molnet. Skiljer från olika sätt en enhet kan etableras och användas i ett företag via Azure-portalen."
keywords: "Windows molnet Windows på Azure Active Directory, Windows 10-enheter på Azure, Windows Azure-enheter"
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 2cb9ab6a-55b6-4658-b7f2-6e05ae015e1b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 804156048a7596f9937098e6fe762f424526473c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="windows-10-for-the-enterprise-ways-to-use-devices-for-work"></a><span data-ttu-id="3be27-105">Windows 10 för företaget: Sätt att använda enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="3be27-105">Windows 10 for the enterprise: Ways to use devices for work</span></span>
<span data-ttu-id="3be27-106">Windows 10 ger dig möjlighet att utnyttja Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3be27-106">Windows 10 gives you the ability to leverage Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="3be27-107">Du kan ansluta Windows 10-enheter till Azure AD så att användarna kan logga in till Windows med hjälp av Azure AD-konton eller genom att lägga till sina Azure-ID: N att få åtkomst till företagsappar och resurser.</span><span class="sxs-lookup"><span data-stu-id="3be27-107">You can connect Windows 10 devices to Azure AD so that users can sign in to Windows by using Azure AD accounts or by adding their Azure IDs to gain access to business apps and resources.</span></span>

![Azure Active Directory med Windows-moln](./media/active-directory-azureadjoin/windows10-overview.png)

## <a name="integrating-windows-10-devices-with-azure-active-directory--a-content-map"></a><span data-ttu-id="3be27-109">Integrera Windows 10-enheter med Azure Active Directory--en innehållskarta</span><span class="sxs-lookup"><span data-stu-id="3be27-109">Integrating Windows 10 devices with Azure Active Directory--a content map</span></span>
<span data-ttu-id="3be27-110">Följande avsnitt innehåller insikter om olika funktioner i Windows 10-enheter i din organisation.</span><span class="sxs-lookup"><span data-stu-id="3be27-110">The following topics provide insights into different capabilities of Windows 10 devices in your organization.</span></span>

|  | <span data-ttu-id="3be27-111">Ämnen</span><span class="sxs-lookup"><span data-stu-id="3be27-111">Topics</span></span> |
| --- | --- |
| <span data-ttu-id="3be27-112">Komma igång</span><span class="sxs-lookup"><span data-stu-id="3be27-112">Getting started</span></span> |[<span data-ttu-id="3be27-113">Med hjälp av Windows 10-enheter på din arbetsplats</span><span class="sxs-lookup"><span data-stu-id="3be27-113">Using Windows 10 devices in your workplace</span></span>](active-directory-azureadjoin-windows10-devices.md) <br> <br> [<span data-ttu-id="3be27-114">Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="3be27-114">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-overview.md) <br> <br> [<span data-ttu-id="3be27-115">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="3be27-115">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md) |
| <span data-ttu-id="3be27-116">Distribution</span><span class="sxs-lookup"><span data-stu-id="3be27-116">Deployment</span></span> |[<span data-ttu-id="3be27-117">Användningsscenarier och överväganden vid distribution för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="3be27-117">Usage scenarios and deployment considerations for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md) <br><br> [<span data-ttu-id="3be27-118">Ansluta domänanslutna enheter till Azure AD för Windows 10 inträffar</span><span class="sxs-lookup"><span data-stu-id="3be27-118">Connecting domain-joined devices to Azure AD, for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)<br><br>[<span data-ttu-id="3be27-119">Att aktivera Microsoft Passport for work i organisationen</span><span class="sxs-lookup"><span data-stu-id="3be27-119">Enabling Microsoft Passport for work in the organization</span></span>](active-directory-azureadjoin-passport-deployment.md)<br><br> [<span data-ttu-id="3be27-120">Aktivera Enterprise tillstånd Roaming för Windows 10</span><span class="sxs-lookup"><span data-stu-id="3be27-120">Enabling Enterprise State Roaming for Windows 10</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)<br><br> |
| <span data-ttu-id="3be27-121">Användaruppgifter</span><span class="sxs-lookup"><span data-stu-id="3be27-121">User tasks</span></span> |[<span data-ttu-id="3be27-122">Skapa en ny Windows 10-enhet med Azure AD under installationen</span><span class="sxs-lookup"><span data-stu-id="3be27-122">Setting up a new Windows 10 device with Azure AD during setup</span></span>](active-directory-azureadjoin-user-frx.md) <br><br> [<span data-ttu-id="3be27-123">Konfigurera en Windows 10-enhet med Azure AD från inställningsmenyn</span><span class="sxs-lookup"><span data-stu-id="3be27-123">Setting up a Windows 10 device with Azure AD from the settings menu</span></span>](active-directory-azureadjoin-user-upgrade.md) <br><br> [<span data-ttu-id="3be27-124">Ansluta till en personlig enhet för Windows 10 för din organisation</span><span class="sxs-lookup"><span data-stu-id="3be27-124">Joining a personal Windows 10 device to your organization</span></span>](active-directory-azureadjoin-personal-device.md) |

