---
title: "krav för aaaAzure Data Catalog | Microsoft Docs"
description: "Läs mer om hello krav som du behöver tooget igång med Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 0c8e768e5846c61b542b746d7ad80121725a9ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-prerequisites"></a><span data-ttu-id="19104-103">Förutsättningar för Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="19104-103">Azure Data Catalog prerequisites</span></span>

<span data-ttu-id="19104-104">Du måste tootake vård några saker innan du kan ställa in Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="19104-104">You need tootake care of a few things before you can set up Azure Data Catalog.</span></span> <span data-ttu-id="19104-105">Oroa dig inte, den här processen tar inte lång tid.</span><span class="sxs-lookup"><span data-stu-id="19104-105">Don’t worry, this process does not take long.</span></span>

## <a name="azure-subscription"></a><span data-ttu-id="19104-106">Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="19104-106">Azure subscription</span></span>
<span data-ttu-id="19104-107">tooset upp Data Catalog, måste du vara hello ägare eller Medägare för Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="19104-107">tooset up Data Catalog, you must be hello owner or co-owner of an Azure subscription.</span></span>

<span data-ttu-id="19104-108">Azure-prenumerationer kan du organisera toocloud-tjänst komma åt resurser, till exempel Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="19104-108">Azure subscriptions help you organize access toocloud-service resources such as Data Catalog.</span></span> <span data-ttu-id="19104-109">Prenumerationer hjälpa dig att styra hur Resursanvändning rapporterade, debiteras och betalat för.</span><span class="sxs-lookup"><span data-stu-id="19104-109">Subscriptions also help you control how resource usage is reported, billed, and paid for.</span></span> <span data-ttu-id="19104-110">Varje prenumeration kan ha separata fakturerings- och inställningar, så du kan ha prenumerationer och scheman som varierar beroende på avdelning, projekt, lokalkontor och så vidare.</span><span class="sxs-lookup"><span data-stu-id="19104-110">Each subscription can have a separate billing and payment setup, so you can have subscriptions and plans that vary by department, project, regional office, and so on.</span></span> <span data-ttu-id="19104-111">Varje tjänst i molnet tillhör tooa prenumeration och du behöver toohave en prenumeration innan du konfigurerar Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="19104-111">Every cloud service belongs tooa subscription, and you need toohave a subscription before you set up Data Catalog.</span></span> <span data-ttu-id="19104-112">Det finns fler toolearn [hantera konton, prenumerationer och administrativa roller](../active-directory/active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="19104-112">toolearn more, see [Manage accounts, subscriptions, and administrative roles](../active-directory/active-directory-assign-admin-roles.md).</span></span>

## <a name="azure-active-directory"></a><span data-ttu-id="19104-113">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="19104-113">Azure Active Directory</span></span>
<span data-ttu-id="19104-114">tooset upp Data Catalog, måste du vara inloggad med ett användarkonto i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="19104-114">tooset up Data Catalog, you must be signed in with an Azure Active Directory (Azure AD) user account.</span></span>

<span data-ttu-id="19104-115">Azure AD innehåller ett enkelt sätt för ditt företag toomanage identitets- och, både i hello molnet och lokalt.</span><span class="sxs-lookup"><span data-stu-id="19104-115">Azure AD provides an easy way for your business toomanage identity and access, both in hello cloud and on-premises.</span></span> <span data-ttu-id="19104-116">Användarna kan använda en enda arbets- eller skolkonto för enkel inloggning tooany molnet och lokala webbprogram.</span><span class="sxs-lookup"><span data-stu-id="19104-116">Users can use a single work or school account for single sign-in tooany cloud and on-premises web application.</span></span> <span data-ttu-id="19104-117">Data Catalog använder Azure AD tooauthenticate inloggning.</span><span class="sxs-lookup"><span data-stu-id="19104-117">Data Catalog uses Azure AD tooauthenticate sign-in.</span></span> <span data-ttu-id="19104-118">Det finns fler toolearn [vad är Azure Active Directory?](../active-directory/active-directory-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="19104-118">toolearn more, see [What is Azure Active Directory?](../active-directory/active-directory-whatis.md).</span></span>

> [!NOTE]
> <span data-ttu-id="19104-119">Med hjälp av hello [Azure-portalen](http://portal.azure.com/), du kan logga in med ett personligt microsoftkonto eller ett Azure Active Directory arbets- eller skolkonto.</span><span class="sxs-lookup"><span data-stu-id="19104-119">By using hello [Azure portal](http://portal.azure.com/), you can sign in with either a personal Microsoft account or an Azure Active Directory work or school account.</span></span> <span data-ttu-id="19104-120">tooset upp Data Catalog genom att använda antingen hello Azure-portalen eller hello [Data Catalog-portalen](http://www.azuredatacatalog.com), du måste logga in med ett Azure Active Directory-konto, inte ett personligt konto.</span><span class="sxs-lookup"><span data-stu-id="19104-120">tooset up Data Catalog by using either hello Azure portal or hello [Data Catalog portal](http://www.azuredatacatalog.com), you must sign in with an Azure Active Directory account, not a personal account.</span></span>
>
>

## <a name="active-directory-policy-configuration"></a><span data-ttu-id="19104-121">Active Directory-principkonfiguration</span><span class="sxs-lookup"><span data-stu-id="19104-121">Active Directory policy configuration</span></span>
<span data-ttu-id="19104-122">Du kan hända att du kan logga in toohello Data Catalog-portalen, men när du försöker toosign i toohello registreringsverktyget för datakällor, du får ett felmeddelande som hindrar dig från att logga in.</span><span class="sxs-lookup"><span data-stu-id="19104-122">You might encounter a situation where you can sign in toohello Data Catalog portal, but when you attempt toosign in toohello data source registration tool, you encounter an error message that prevents you from signing in.</span></span> <span data-ttu-id="19104-123">Det här problemet kan inträffa när du är på hello företagets nätverk eller uppstå endast när du kopplar från hello utanför företagets nätverk.</span><span class="sxs-lookup"><span data-stu-id="19104-123">This problem behavior might occur only when you're on hello company network, or it might occur only when you're connecting from outside hello company network.</span></span>

<span data-ttu-id="19104-124">hello registreringsverktyget för datakällor använder formulärautentisering toovalidate användarautentiseringsuppgifter mot Active Directory.</span><span class="sxs-lookup"><span data-stu-id="19104-124">hello data source registration tool uses Forms Authentication toovalidate your user credentials against Active Directory.</span></span> <span data-ttu-id="19104-125">toohelp som du loggar in har ett Active Directory-administratören måste aktivera formulärautentisering i hello Global autentiseringsprincip.</span><span class="sxs-lookup"><span data-stu-id="19104-125">toohelp you sign in successfully, an Active Directory administrator must enable Forms Authentication in hello Global Authentication Policy.</span></span>

<span data-ttu-id="19104-126">I hello Global autentiseringsprincip vara autentiseringsmetoder aktiverade separat för intranät och extranät-anslutningar som visas i följande skärmbild hello.</span><span class="sxs-lookup"><span data-stu-id="19104-126">In hello Global Authentication Policy, authentication methods can be enabled separately for intranet and extranet connections, as shown in hello following screenshot.</span></span> <span data-ttu-id="19104-127">Logga in fel kan inträffa om formulär för autentisering inte har aktiverats för hello-nätverk som du vill ansluta.</span><span class="sxs-lookup"><span data-stu-id="19104-127">Sign-in errors might occur if Forms Authentication is not enabled for hello network from which you're connecting.</span></span>

 ![Globala autentiseringsprincipen i Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a><span data-ttu-id="19104-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="19104-129">Next steps</span></span>
<span data-ttu-id="19104-130">Mer information finns i [konfigurera autentiseringsprinciper](https://technet.microsoft.com/library/dn486781.aspx).</span><span class="sxs-lookup"><span data-stu-id="19104-130">For more information, see [Configuring Authentication Policies](https://technet.microsoft.com/library/dn486781.aspx).</span></span>
