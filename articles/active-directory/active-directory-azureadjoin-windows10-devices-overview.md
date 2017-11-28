---
title: "Windows 10 för hello företaget: sätt toouse enheter för arbete | Microsoft Docs"
description: "Översikt över hur du distribuerar Windows 10-enheter för företag och hur toointegrate med Azure Active Directory för hello Windows cloud. Står i kontrast hello olika sätt en enhet kan etableras och användas i ett företag via hello Azure-portalen."
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
ms.openlocfilehash: 95b452bc5ba3937e16de769275a59c77cb821e23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-10-for-hello-enterprise-ways-toouse-devices-for-work"></a><span data-ttu-id="72b86-105">Windows 10 för hello företaget: sätt toouse enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="72b86-105">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>
<span data-ttu-id="72b86-106">Windows 10 ger du hello möjlighet tooleverage Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="72b86-106">Windows 10 gives you hello ability tooleverage Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="72b86-107">Du kan ansluta Windows 10 enheter tooAzure AD så att användarna kan logga in tooWindows med hjälp av Azure AD-konton eller lägga till Azure-ID: N toogain åtkomst toobusiness appar och resurser.</span><span class="sxs-lookup"><span data-stu-id="72b86-107">You can connect Windows 10 devices tooAzure AD so that users can sign in tooWindows by using Azure AD accounts or by adding their Azure IDs toogain access toobusiness apps and resources.</span></span>

![Azure Active Directory med Windows-moln](./media/active-directory-azureadjoin/windows10-overview.png)

## <a name="integrating-windows-10-devices-with-azure-active-directory--a-content-map"></a><span data-ttu-id="72b86-109">Integrera Windows 10-enheter med Azure Active Directory--en innehållskarta</span><span class="sxs-lookup"><span data-stu-id="72b86-109">Integrating Windows 10 devices with Azure Active Directory--a content map</span></span>
<span data-ttu-id="72b86-110">hello följande avsnitt ger insikter om olika funktioner i Windows 10-enheter i din organisation.</span><span class="sxs-lookup"><span data-stu-id="72b86-110">hello following topics provide insights into different capabilities of Windows 10 devices in your organization.</span></span>

|  | <span data-ttu-id="72b86-111">Ämnen</span><span class="sxs-lookup"><span data-stu-id="72b86-111">Topics</span></span> |
| --- | --- |
| <span data-ttu-id="72b86-112">Komma igång</span><span class="sxs-lookup"><span data-stu-id="72b86-112">Getting started</span></span> |[<span data-ttu-id="72b86-113">Med hjälp av Windows 10-enheter på din arbetsplats</span><span class="sxs-lookup"><span data-stu-id="72b86-113">Using Windows 10 devices in your workplace</span></span>](active-directory-azureadjoin-windows10-devices.md) <br> <br> [<span data-ttu-id="72b86-114">Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="72b86-114">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-overview.md) <br> <br> [<span data-ttu-id="72b86-115">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="72b86-115">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md) |
| <span data-ttu-id="72b86-116">Distribution</span><span class="sxs-lookup"><span data-stu-id="72b86-116">Deployment</span></span> |[<span data-ttu-id="72b86-117">Användningsscenarier och överväganden vid distribution för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="72b86-117">Usage scenarios and deployment considerations for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md) <br><br> [<span data-ttu-id="72b86-118">Ansluta domänanslutna enheter tooAzure AD för Windows 10 inträffar</span><span class="sxs-lookup"><span data-stu-id="72b86-118">Connecting domain-joined devices tooAzure AD, for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)<br><br>[<span data-ttu-id="72b86-119">Att aktivera Microsoft Passport for work i hello organisation</span><span class="sxs-lookup"><span data-stu-id="72b86-119">Enabling Microsoft Passport for work in hello organization</span></span>](active-directory-azureadjoin-passport-deployment.md)<br><br> [<span data-ttu-id="72b86-120">Aktivera Enterprise tillstånd Roaming för Windows 10</span><span class="sxs-lookup"><span data-stu-id="72b86-120">Enabling Enterprise State Roaming for Windows 10</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)<br><br> |
| <span data-ttu-id="72b86-121">Användaruppgifter</span><span class="sxs-lookup"><span data-stu-id="72b86-121">User tasks</span></span> |[<span data-ttu-id="72b86-122">Skapa en ny Windows 10-enhet med Azure AD under installationen</span><span class="sxs-lookup"><span data-stu-id="72b86-122">Setting up a new Windows 10 device with Azure AD during setup</span></span>](active-directory-azureadjoin-user-frx.md) <br><br> [<span data-ttu-id="72b86-123">Konfigurera en Windows 10-enhet med Azure AD hello inställningsmenyn</span><span class="sxs-lookup"><span data-stu-id="72b86-123">Setting up a Windows 10 device with Azure AD from hello settings menu</span></span>](active-directory-azureadjoin-user-upgrade.md) <br><br> [<span data-ttu-id="72b86-124">Ansluta till en personlig tooyour organisation för Windows 10-enhet</span><span class="sxs-lookup"><span data-stu-id="72b86-124">Joining a personal Windows 10 device tooyour organization</span></span>](active-directory-azureadjoin-personal-device.md) |

