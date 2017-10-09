---
title: 'Azure Active Directory B2C: Skapa en Azure Active Directory B2C-klient | Microsoft Docs'
description: Ett avsnitt om hur toocreate ett Azure Active Directory B2C-klient
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
ms.openlocfilehash: e8b257d66c1f66ffb84f5d3d21b30b42eddcbac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-active-directory-b2c-tenant-in-hello-azure-portal"></a><span data-ttu-id="d2e12-103">Skapa en Azure Active Directory B2C-klient i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d2e12-103">Create an Azure Active Directory B2C tenant in hello Azure portal</span></span>

<span data-ttu-id="d2e12-104">Redigera Sipi.</span><span class="sxs-lookup"><span data-stu-id="d2e12-104">Edited by Sipi.</span></span>

<span data-ttu-id="d2e12-105">Den här snabbstarten hjälper dig att skapa en Microsoft Azure Active Directory (AD Azure) B2C-klient på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="d2e12-105">This Quickstart helps you create a Microsoft Azure Active Directory (Azure AD) B2C tenant in just a few minutes.</span></span> <span data-ttu-id="d2e12-106">När du är klar har du toouse en B2C-klient för att registrera B2C-program.</span><span class="sxs-lookup"><span data-stu-id="d2e12-106">When you're finished, you have a B2C tenant toouse for registering B2C applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2e12-107">Krav</span><span class="sxs-lookup"><span data-stu-id="d2e12-107">Prerequisites</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

##  <a name="log-in-tooazure"></a><span data-ttu-id="d2e12-108">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="d2e12-108">Log in tooAzure</span></span>

<span data-ttu-id="d2e12-109">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d2e12-109">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="d2e12-110">Skapa en Azure AD B2C-klient</span><span class="sxs-lookup"><span data-stu-id="d2e12-110">Create an Azure AD B2C tenant</span></span>

<span data-ttu-id="d2e12-111">B2C-funktioner kan inte aktiveras i din befintliga klienter.</span><span class="sxs-lookup"><span data-stu-id="d2e12-111">B2C features can't be enabled in your existing tenants.</span></span> <span data-ttu-id="d2e12-112">Du måste toocreate en Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="d2e12-112">You need toocreate an Azure AD B2C tenant.</span></span>

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

<span data-ttu-id="d2e12-113">Grattis, du har skapat en Azure Active Directory B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="d2e12-113">Congratulations, you have created an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="d2e12-114">Du är en Global administratör för hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="d2e12-114">You are a Global Administrator of hello tenant.</span></span> <span data-ttu-id="d2e12-115">Du kan lägga till andra globala administratörer om det behövs.</span><span class="sxs-lookup"><span data-stu-id="d2e12-115">You can add other Global Administrators as required.</span></span> <span data-ttu-id="d2e12-116">tooswitch tooyour ny klient klickar du på hello *hantera nya klient länken*.</span><span class="sxs-lookup"><span data-stu-id="d2e12-116">tooswitch tooyour new tenant, click hello *manage your new tenant link*.</span></span>

![Hantera din ny klient-länk](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> <span data-ttu-id="d2e12-118">Om du planerar toouse en B2C-klient för en produktionsapp artikeln hello på [produktionsskala kontra preview B2C hyresgäster](active-directory-b2c-reference-tenant-type.md).</span><span class="sxs-lookup"><span data-stu-id="d2e12-118">If you are planning toouse a B2C tenant for a production app, read hello article on [production-scale vs. preview B2C tenants](active-directory-b2c-reference-tenant-type.md).</span></span> <span data-ttu-id="d2e12-119">Det finns kända problem när du tar bort en befintlig B2C-klient och skapa den igen med hello samma domännamn.</span><span class="sxs-lookup"><span data-stu-id="d2e12-119">There are known issues when you delete an existing B2C tenant and re-create it with hello same domain name.</span></span> <span data-ttu-id="d2e12-120">Du måste toocreate en B2C-klient med ett annat domännamn.</span><span class="sxs-lookup"><span data-stu-id="d2e12-120">You need toocreate a B2C tenant with a different domain name.</span></span>
>
>

## <a name="link-your-tenant-tooyour-subscription"></a><span data-ttu-id="d2e12-121">Länka din klient tooyour prenumeration</span><span class="sxs-lookup"><span data-stu-id="d2e12-121">Link your tenant tooyour subscription</span></span>

<span data-ttu-id="d2e12-122">Du behöver toolink din Azure AD B2C-klient tooyour Azure-prenumeration tooenable alla B2C-funktioner och betala för avgifter för användning.</span><span class="sxs-lookup"><span data-stu-id="d2e12-122">You need toolink your Azure AD B2C tenant tooyour Azure subscription tooenable all B2C functionality and pay for usage charges.</span></span> <span data-ttu-id="d2e12-123">toolearn läsa fler [i den här artikeln](active-directory-b2c-how-to-enable-billing.md).</span><span class="sxs-lookup"><span data-stu-id="d2e12-123">toolearn more, read [this article](active-directory-b2c-how-to-enable-billing.md).</span></span> <span data-ttu-id="d2e12-124">Om du inte länka ditt Azure-prenumeration med Azure AD B2C-klient tooyour vissa funktioner blockeras och visas ett varningsmeddelande (”ingen prenumeration länkade toothis B2C-klient eller hello prenumeration behöver kontrolleras”.) i hello B2C-inställningar.</span><span class="sxs-lookup"><span data-stu-id="d2e12-124">If you don't link your Azure AD B2C tenant tooyour Azure subscription, some functionality is blocked and, you see a warning message ("No Subscription linked toothis B2C tenant or hello Subscription needs your attention.") in hello B2C settings.</span></span> <span data-ttu-id="d2e12-125">Det är viktigt att du vidtar åtgärden innan du levererar dina appar i produktionen.</span><span class="sxs-lookup"><span data-stu-id="d2e12-125">It is important that you take this step before you ship your apps into production.</span></span>

## <a name="easy-access-toosettings"></a><span data-ttu-id="d2e12-126">Enkel åtkomst toosettings</span><span class="sxs-lookup"><span data-stu-id="d2e12-126">Easy access toosettings</span></span>

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

<span data-ttu-id="d2e12-127">Du kan också komma åt hello bladet genom att ange `Azure AD B2C` i **söka resurser** hello överst i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="d2e12-127">You can also access hello blade by entering `Azure AD B2C` in **Search resources** at hello top of hello portal.</span></span> <span data-ttu-id="d2e12-128">Markera i hello resultat **Azure AD B2C** tooaccess hello B2C inställningsbladet.</span><span class="sxs-lookup"><span data-stu-id="d2e12-128">In hello results list, select **Azure AD B2C** tooaccess hello B2C settings blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2e12-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2e12-129">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d2e12-130">Registrera din B2C-program i en B2C-klient</span><span class="sxs-lookup"><span data-stu-id="d2e12-130">Register your B2C application in a B2C tenant</span></span>](active-directory-b2c-app-registration.md)
