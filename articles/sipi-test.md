---
title: Sipi Testfilen | Microsoft Docs
description: "Testa filen för att kontrollera ReadyForTest beroenden"
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
ms.openlocfilehash: 871d58818dcbaee5f7a5f07c19e2297ec6459a6f
ms.sourcegitcommit: b0af2a2cf44101a1b1ff41bd2ad795eaef29612a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/28/2017
---
# <a name="sipi-test-file"></a><span data-ttu-id="48580-103">Sipi Testfilen</span><span class="sxs-lookup"><span data-stu-id="48580-103">Sipi test file</span></span>

<span data-ttu-id="48580-104">Med hjälp av den här snabbstarten registrerar du ett program i en Microsoft Azure Active Directory (Azure AD) B2C-klientorganisation på några få minuter.</span><span class="sxs-lookup"><span data-stu-id="48580-104">This Quickstart helps you register an application in a Microsoft Azure Active Directory (Azure AD) B2C tenant in a few minutes.</span></span> <span data-ttu-id="48580-105">När du är färdig är programmet klart att användas i Azure B2C-klientorganisationen.</span><span class="sxs-lookup"><span data-stu-id="48580-105">When you're finished, your application is registered for use in the Azure B2C tenant.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48580-106">Krav</span><span class="sxs-lookup"><span data-stu-id="48580-106">Prerequisites</span></span>

<span data-ttu-id="48580-107">Om du vill skapa ett program som accepterar registrering och inloggning av konsumenter måste du först registrera programmet med en Azure Active Directory B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="48580-107">To build an application that accepts consumer sign-up and sign-in, you first need to register the application with an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="48580-108">Skaffa en egen klient genom att följa stegen i [Skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="48580-108">Get your own tenant by using the steps outlined in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

<span data-ttu-id="48580-109">Program som har skapats från Azure AD B2C-bladet i Azure Portal måste hanteras från samma plats.</span><span class="sxs-lookup"><span data-stu-id="48580-109">Applications created from the Azure AD B2C blade in the Azure portal must be managed from the same location.</span></span> <span data-ttu-id="48580-110">Om du redigerar B2C-program med hjälp av PowerShell eller någon annan portal, stöds de inte och fungerar inte med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="48580-110">If you edit the B2C applications using PowerShell or another portal, they become unsupported and do not work with Azure AD B2C.</span></span> <span data-ttu-id="48580-111">Mer information finns i avsnittet [felaktiga appar](#faulted-apps).</span><span class="sxs-lookup"><span data-stu-id="48580-111">See details in the [faulted apps](#faulted-apps) section.</span></span> 

## <a name="navigate-to-b2c-settings"></a><span data-ttu-id="48580-112">Gå till B2C-inställningar</span><span class="sxs-lookup"><span data-stu-id="48580-112">Navigate to B2C settings</span></span>

<span data-ttu-id="48580-113">Logga in på [Azure Portal](https://portal.azure.com/) som global administratör för B2C-klientorganisationen.</span><span class="sxs-lookup"><span data-stu-id="48580-113">Log in to the [Azure portal](https://portal.azure.com/) as the Global Administrator of the B2C tenant.</span></span> 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../includes/active-directory-b2c-switch-b2c-tenant.md)]

<span data-ttu-id="48580-114">Välj nästa steg baserat på vilken typ av program du registrerar:</span><span class="sxs-lookup"><span data-stu-id="48580-114">Choose next steps based on the application type you are registering:</span></span>
