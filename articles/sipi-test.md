---
title: Sipi Testfilen | Microsoft Docs
description: Testa filberoenden toocheck ReadyForTest
services: active-directory-b2c
documentationcenter: 
author: Sipi
manager: Sipi
editor: Sipi
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f68
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: Sipi
ms.openlocfilehash: afd3dc94dfb30926b316256fb06a768a391004f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sipi-test-file"></a><span data-ttu-id="ef0d5-103">Sipi Testfilen</span><span class="sxs-lookup"><span data-stu-id="ef0d5-103">Sipi test file</span></span>

<span data-ttu-id="ef0d5-104">Med hjälp av den här snabbstarten registrerar du ett program i en Microsoft Azure Active Directory (Azure AD) B2C-klientorganisation på några få minuter.</span><span class="sxs-lookup"><span data-stu-id="ef0d5-104">This Quickstart helps you register an application in a Microsoft Azure Active Directory (Azure AD) B2C tenant in a few minutes.</span></span> <span data-ttu-id="ef0d5-105">När du är klar registreras ditt program för användning i hello Azure B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="ef0d5-105">When you're finished, your application is registered for use in hello Azure B2C tenant.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef0d5-106">Krav</span><span class="sxs-lookup"><span data-stu-id="ef0d5-106">Prerequisites</span></span>

<span data-ttu-id="ef0d5-107">toobuild ett program som accepterar konsumenten registrering och inloggning, måste du först tooregister hello program med en Azure Active Directory B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="ef0d5-107">toobuild an application that accepts consumer sign-up and sign-in, you first need tooregister hello application with an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="ef0d5-108">Skaffa en egen klient genom att använda hello steg som beskrivs i [skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ef0d5-108">Get your own tenant by using hello steps outlined in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

<span data-ttu-id="ef0d5-109">Program som har skapats från hello Azure AD B2C-bladet i hello Azure-portalen måste hanteras från hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="ef0d5-109">Applications created from hello Azure AD B2C blade in hello Azure portal must be managed from hello same location.</span></span> <span data-ttu-id="ef0d5-110">Om du redigerar hello B2C-program med hjälp av PowerShell eller en annan portal, blir stöds inte och fungerar inte med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="ef0d5-110">If you edit hello B2C applications using PowerShell or another portal, they become unsupported and do not work with Azure AD B2C.</span></span> <span data-ttu-id="ef0d5-111">Mer information finns i hello [fel appar](#faulted-apps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ef0d5-111">See details in hello [faulted apps](#faulted-apps) section.</span></span> 

## <a name="navigate-toob2c-settings"></a><span data-ttu-id="ef0d5-112">Navigera tooB2C inställningar</span><span class="sxs-lookup"><span data-stu-id="ef0d5-112">Navigate tooB2C settings</span></span>

<span data-ttu-id="ef0d5-113">Logga in toohello [Azure-portalen](https://portal.azure.com/) som hello Global administratör för hello B2C-klienten.</span><span class="sxs-lookup"><span data-stu-id="ef0d5-113">Log in toohello [Azure portal](https://portal.azure.com/) as hello Global Administrator of hello B2C tenant.</span></span> 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../includes/active-directory-b2c-switch-b2c-tenant.md)]

<span data-ttu-id="ef0d5-114">Välj nästa steg baserat på hello programtyp som du registrerar:</span><span class="sxs-lookup"><span data-stu-id="ef0d5-114">Choose next steps based on hello application type you are registering:</span></span>
