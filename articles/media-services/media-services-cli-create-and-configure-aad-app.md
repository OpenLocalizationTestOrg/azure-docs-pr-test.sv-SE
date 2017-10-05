---
title: "Använda CLI 2.0 för att skapa en Azure AD-app och konfigurera den för att komma åt Azure Media Services API | Microsoft Docs"
description: "Det här avsnittet visar hur du använder CLI 2.0 för att skapa en Azure AD-app och konfigurera den för att komma åt Azure Media Services API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 01a2bb6d99776feec936315bc882c3097ce832d4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-cli-20-to-create-an-aad-app-and-configure-it-to-access-azure-media-services-api"></a><span data-ttu-id="aaa1a-103">Använda CLI 2.0 för att skapa en AAD-app och konfigurera den för att komma åt Azure Media Services API</span><span class="sxs-lookup"><span data-stu-id="aaa1a-103">Use CLI 2.0 to create an AAD app and configure it to access Azure Media Services API</span></span>

<span data-ttu-id="aaa1a-104">Det här avsnittet visar hur du skapar ett Azure Active Directory (Azure AD)-program och tjänstens huvudnamn för att komma åt resurser i Azure Media Services med hjälp av CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="aaa1a-104">This topic shows you how to use CLI 2.0 to create an Azure Active Directory (Azure AD) application and service principal to access Azure Media Services resources.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="aaa1a-105">Krav</span><span class="sxs-lookup"><span data-stu-id="aaa1a-105">Prerequisites</span></span>

- <span data-ttu-id="aaa1a-106">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="aaa1a-106">An Azure account.</span></span> <span data-ttu-id="aaa1a-107">Mer information finns i avsnittet om [den kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="aaa1a-107">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="aaa1a-108">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="aaa1a-108">A Media Services account.</span></span> <span data-ttu-id="aaa1a-109">Mer information finns i [skapa ett Azure Media Services-konto med hjälp av Azure portal](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="aaa1a-109">For more information, see [Create an Azure Media Services account using the Azure portal](media-services-portal-create-account.md).</span></span>

## <a name="use-the-azure-cloud-shell"></a><span data-ttu-id="aaa1a-110">Använd Azure-molnet Shell</span><span class="sxs-lookup"><span data-stu-id="aaa1a-110">Use the Azure Cloud Shell</span></span>

1. <span data-ttu-id="aaa1a-111">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="aaa1a-111">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="aaa1a-112">Starta molnet Shell från det övre navigeringsfönstret på portalen.</span><span class="sxs-lookup"><span data-stu-id="aaa1a-112">Launch the Cloud Shell from the upper navigation pane of the portal.</span></span>

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

<span data-ttu-id="aaa1a-114">Mer information finns i [översikt av Azure Cloud Shell](../cloud-shell/overview.md).</span><span class="sxs-lookup"><span data-stu-id="aaa1a-114">For more information, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md).</span></span>

## <a name="create-an-azure-ad-app-and-configure-access-to-the-media-account-with-cli-20"></a><span data-ttu-id="aaa1a-115">Skapa en Azure AD-app och konfigurera åtkomst till media-konto med CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="aaa1a-115">Create an Azure AD app and configure access to the media account with CLI 2.0</span></span>
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create -- assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

<span data-ttu-id="aaa1a-116">Exempel:</span><span class="sxs-lookup"><span data-stu-id="aaa1a-116">For example:</span></span>

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

<span data-ttu-id="aaa1a-117">I det här exemplet i **omfång** är fullständig resursens sökväg för media services-konto.</span><span class="sxs-lookup"><span data-stu-id="aaa1a-117">In this example, the **scope** is the full resource path for the media services account.</span></span> <span data-ttu-id="aaa1a-118">Men den **omfång** kan när som helst.</span><span class="sxs-lookup"><span data-stu-id="aaa1a-118">However, the **scope** can be at any level.</span></span>

<span data-ttu-id="aaa1a-119">Det kan till exempel vara något av följande nivåer:</span><span class="sxs-lookup"><span data-stu-id="aaa1a-119">For example, it could be one of the following levels:</span></span>
 
* <span data-ttu-id="aaa1a-120">Den **prenumeration** nivå.</span><span class="sxs-lookup"><span data-stu-id="aaa1a-120">The **subscription** level.</span></span>
* <span data-ttu-id="aaa1a-121">Den **resursgruppen** nivå.</span><span class="sxs-lookup"><span data-stu-id="aaa1a-121">The **resource group** level.</span></span>
* <span data-ttu-id="aaa1a-122">Den **resurs** nivå (till exempel ett konto för Media).</span><span class="sxs-lookup"><span data-stu-id="aaa1a-122">The **resource** level (for example, a Media account).</span></span>

<span data-ttu-id="aaa1a-123">Mer information finns i [skapa en Azure tjänstens huvudnamn med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="aaa1a-123">For more information, see [Create an Azure service principal with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

<span data-ttu-id="aaa1a-124">Se även [Manage Role-Based åtkomstkontroll med kommandoradsgränssnittet i Azure](../active-directory/role-based-access-control-manage-access-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="aaa1a-124">Also see [Manage Role-Based Access Control with the Azure command-line interface](../active-directory/role-based-access-control-manage-access-azure-cli.md).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="aaa1a-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aaa1a-125">Next steps</span></span>

<span data-ttu-id="aaa1a-126">Kom igång med [överföringen av filer till ditt konto](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="aaa1a-126">Get started with [uploading files to your account](media-services-portal-upload-files.md).</span></span>
