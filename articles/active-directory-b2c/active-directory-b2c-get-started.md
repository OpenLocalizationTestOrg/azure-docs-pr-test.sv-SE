---
title: 'Azure Active Directory B2C: Skapa en Azure Active Directory B2C-klient | Microsoft Docs'
description: Ett avsnitt om hur du skapar en Azure Active Directory B2C-klient
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: patricka
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/07/2017
ms.author: swkrish
ms.openlocfilehash: 1a7eb94e3c74aa0dc187a6d203ba0cf885b97c4d
ms.sourcegitcommit: b0af2a2cf44101a1b1ff41bd2ad795eaef29612a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/28/2017
---
# <a name="create-an-azure-active-directory-b2c-tenant-in-the-azure-portal"></a><span data-ttu-id="ab713-103">Skapa en Azure Active Directory B2C-klient i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ab713-103">Create an Azure Active Directory B2C tenant in the Azure portal</span></span>

<span data-ttu-id="ab713-104">Redigera Sipi.</span><span class="sxs-lookup"><span data-stu-id="ab713-104">Edited by Sipi.</span></span>

<span data-ttu-id="ab713-105">Den här snabbstarten hjälper dig att skapa en Microsoft Azure Active Directory (AD Azure) B2C-klient på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="ab713-105">This Quickstart helps you create a Microsoft Azure Active Directory (Azure AD) B2C tenant in just a few minutes.</span></span> <span data-ttu-id="ab713-106">När du är klar har du en B2C-klient ska användas för att registrera B2C-program.</span><span class="sxs-lookup"><span data-stu-id="ab713-106">When you're finished, you have a B2C tenant to use for registering B2C applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab713-107">Krav</span><span class="sxs-lookup"><span data-stu-id="ab713-107">Prerequisites</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

##  <a name="log-in-to-azure"></a><span data-ttu-id="ab713-108">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="ab713-108">Log in to Azure</span></span>

<span data-ttu-id="ab713-109">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ab713-109">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="ab713-110">Skapa en Azure AD B2C-klient</span><span class="sxs-lookup"><span data-stu-id="ab713-110">Create an Azure AD B2C tenant</span></span>

<span data-ttu-id="ab713-111">B2C-funktioner kan inte aktiveras i din befintliga klienter.</span><span class="sxs-lookup"><span data-stu-id="ab713-111">B2C features can't be enabled in your existing tenants.</span></span> <span data-ttu-id="ab713-112">Du måste skapa en Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="ab713-112">You need to create an Azure AD B2C tenant.</span></span>

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

<span data-ttu-id="ab713-113">Grattis, du har skapat en Azure Active Directory B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="ab713-113">Congratulations, you have created an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="ab713-114">Du är en Global administratör för klienten.</span><span class="sxs-lookup"><span data-stu-id="ab713-114">You are a Global Administrator of the tenant.</span></span> <span data-ttu-id="ab713-115">Du kan lägga till andra globala administratörer om det behövs.</span><span class="sxs-lookup"><span data-stu-id="ab713-115">You can add other Global Administrators as required.</span></span> <span data-ttu-id="ab713-116">Växla till den nya innehavaren, klicka på den *hantera nya klient länken*.</span><span class="sxs-lookup"><span data-stu-id="ab713-116">To switch to your new tenant, click the *manage your new tenant link*.</span></span>

![Hantera din ny klient-länk](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> <span data-ttu-id="ab713-118">Om du planerar att använda en B2C-klient för en produktionsapp artikeln om [produktionsskala kontra preview B2C hyresgäster](active-directory-b2c-reference-tenant-type.md).</span><span class="sxs-lookup"><span data-stu-id="ab713-118">If you are planning to use a B2C tenant for a production app, read the article on [production-scale vs. preview B2C tenants](active-directory-b2c-reference-tenant-type.md).</span></span> <span data-ttu-id="ab713-119">Det finns kända problem när du tar bort en befintlig B2C-klient och skapa den igen med samma domännamn.</span><span class="sxs-lookup"><span data-stu-id="ab713-119">There are known issues when you delete an existing B2C tenant and re-create it with the same domain name.</span></span> <span data-ttu-id="ab713-120">Du måste skapa en B2C-klient med ett annat domännamn.</span><span class="sxs-lookup"><span data-stu-id="ab713-120">You need to create a B2C tenant with a different domain name.</span></span>
>
>

## <a name="link-your-tenant-to-your-subscription"></a><span data-ttu-id="ab713-121">Länka din klient till din prenumeration</span><span class="sxs-lookup"><span data-stu-id="ab713-121">Link your tenant to your subscription</span></span>

<span data-ttu-id="ab713-122">Du måste länka din Azure AD B2C-klient till din Azure-prenumeration att aktivera alla B2C-funktioner och betalar avgifter för användning.</span><span class="sxs-lookup"><span data-stu-id="ab713-122">You need to link your Azure AD B2C tenant to your Azure subscription to enable all B2C functionality and pay for usage charges.</span></span> <span data-ttu-id="ab713-123">Mer information, [i den här artikeln](active-directory-b2c-how-to-enable-billing.md).</span><span class="sxs-lookup"><span data-stu-id="ab713-123">To learn more, read [this article](active-directory-b2c-how-to-enable-billing.md).</span></span> <span data-ttu-id="ab713-124">Om du inte länka din Azure AD B2C-klient till din Azure-prenumeration, vissa funktioner är blockerad och visas ett varningsmeddelande (”ingen prenumeration länkad till den här B2C-klient eller prenumeration måste kontrolleras”.) i B2C-inställningar.</span><span class="sxs-lookup"><span data-stu-id="ab713-124">If you don't link your Azure AD B2C tenant to your Azure subscription, some functionality is blocked and, you see a warning message ("No Subscription linked to this B2C tenant or the Subscription needs your attention.") in the B2C settings.</span></span> <span data-ttu-id="ab713-125">Det är viktigt att du vidtar åtgärden innan du levererar dina appar i produktionen.</span><span class="sxs-lookup"><span data-stu-id="ab713-125">It is important that you take this step before you ship your apps into production.</span></span>

## <a name="easy-access-to-settings"></a><span data-ttu-id="ab713-126">Enkel åtkomst till inställningar</span><span class="sxs-lookup"><span data-stu-id="ab713-126">Easy access to settings</span></span>

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

<span data-ttu-id="ab713-127">Du kan också komma åt bladet genom att ange `Azure AD B2C` i **söka resurser** överst i portalen.</span><span class="sxs-lookup"><span data-stu-id="ab713-127">You can also access the blade by entering `Azure AD B2C` in **Search resources** at the top of the portal.</span></span> <span data-ttu-id="ab713-128">Välj i resultatlistan **Azure AD B2C** att komma åt bladet B2C-inställningar.</span><span class="sxs-lookup"><span data-stu-id="ab713-128">In the results list, select **Azure AD B2C** to access the B2C settings blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab713-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ab713-129">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ab713-130">Registrera din B2C-program i en B2C-klient</span><span class="sxs-lookup"><span data-stu-id="ab713-130">Register your B2C application in a B2C tenant</span></span>](active-directory-b2c-app-registration.md)
