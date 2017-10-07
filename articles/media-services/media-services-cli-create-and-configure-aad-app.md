---
title: aaaUse CLI 2.0 toocreate en Azure AD-app och konfigurera den tooaccess Azure Media Services API | Microsoft Docs
description: "Det här avsnittet visar hur toouse CLI 2.0 toocreate en Azure AD-app och konfigurera den tooaccess Azure Media Services API."
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
ms.openlocfilehash: c865e2701722374b5dd17b0e20fa848c07065006
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-20-toocreate-an-aad-app-and-configure-it-tooaccess-azure-media-services-api"></a><span data-ttu-id="5ccdb-103">Använd CLI 2.0 toocreate en AAD-app och konfigurera den tooaccess Azure Media Services API</span><span class="sxs-lookup"><span data-stu-id="5ccdb-103">Use CLI 2.0 toocreate an AAD app and configure it tooaccess Azure Media Services API</span></span>

<span data-ttu-id="5ccdb-104">Det här avsnittet visar hur toouse CLI 2.0 toocreate ett Azure Active Directory (Azure AD)-program och tjänstens huvudnamn tooaccess Azure Media Services resurser.</span><span class="sxs-lookup"><span data-stu-id="5ccdb-104">This topic shows you how toouse CLI 2.0 toocreate an Azure Active Directory (Azure AD) application and service principal tooaccess Azure Media Services resources.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="5ccdb-105">Krav</span><span class="sxs-lookup"><span data-stu-id="5ccdb-105">Prerequisites</span></span>

- <span data-ttu-id="5ccdb-106">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="5ccdb-106">An Azure account.</span></span> <span data-ttu-id="5ccdb-107">Mer information finns i avsnittet om [den kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5ccdb-107">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="5ccdb-108">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="5ccdb-108">A Media Services account.</span></span> <span data-ttu-id="5ccdb-109">Mer information finns i [skapa ett Azure Media Services-konto med hello Azure-portalen](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="5ccdb-109">For more information, see [Create an Azure Media Services account using hello Azure portal](media-services-portal-create-account.md).</span></span>

## <a name="use-hello-azure-cloud-shell"></a><span data-ttu-id="5ccdb-110">Använd hello Azure Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="5ccdb-110">Use hello Azure Cloud Shell</span></span>

1. <span data-ttu-id="5ccdb-111">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5ccdb-111">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="5ccdb-112">Starta hello molnet Shell från hello övre navigeringsfönstret på hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="5ccdb-112">Launch hello Cloud Shell from hello upper navigation pane of hello portal.</span></span>

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

<span data-ttu-id="5ccdb-114">Mer information finns i [översikt av Azure Cloud Shell](../cloud-shell/overview.md).</span><span class="sxs-lookup"><span data-stu-id="5ccdb-114">For more information, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md).</span></span>

## <a name="create-an-azure-ad-app-and-configure-access-toohello-media-account-with-cli-20"></a><span data-ttu-id="5ccdb-115">Skapa en Azure AD-app och konfigurera åtkomstkonto toohello media med CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5ccdb-115">Create an Azure AD app and configure access toohello media account with CLI 2.0</span></span>
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create -- assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

<span data-ttu-id="5ccdb-116">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5ccdb-116">For example:</span></span>

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

<span data-ttu-id="5ccdb-117">I det här exemplet hello **omfång** är hello fullständig resursens sökväg för hello media services-konto.</span><span class="sxs-lookup"><span data-stu-id="5ccdb-117">In this example, hello **scope** is hello full resource path for hello media services account.</span></span> <span data-ttu-id="5ccdb-118">Dock hello **omfång** kan när som helst.</span><span class="sxs-lookup"><span data-stu-id="5ccdb-118">However, hello **scope** can be at any level.</span></span>

<span data-ttu-id="5ccdb-119">Det kan till exempel vara något av följande nivåer hello:</span><span class="sxs-lookup"><span data-stu-id="5ccdb-119">For example, it could be one of hello following levels:</span></span>
 
* <span data-ttu-id="5ccdb-120">Hej **prenumeration** nivå.</span><span class="sxs-lookup"><span data-stu-id="5ccdb-120">hello **subscription** level.</span></span>
* <span data-ttu-id="5ccdb-121">Hej **resursgruppen** nivå.</span><span class="sxs-lookup"><span data-stu-id="5ccdb-121">hello **resource group** level.</span></span>
* <span data-ttu-id="5ccdb-122">Hej **resurs** nivå (till exempel ett konto för Media).</span><span class="sxs-lookup"><span data-stu-id="5ccdb-122">hello **resource** level (for example, a Media account).</span></span>

<span data-ttu-id="5ccdb-123">Mer information finns i [skapa en Azure tjänstens huvudnamn med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="5ccdb-123">For more information, see [Create an Azure service principal with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

<span data-ttu-id="5ccdb-124">Se även [Manage Role-Based åtkomstkontroll med hello Azure-kommandoradsgränssnittet](../active-directory/role-based-access-control-manage-access-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="5ccdb-124">Also see [Manage Role-Based Access Control with hello Azure command-line interface](../active-directory/role-based-access-control-manage-access-azure-cli.md).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5ccdb-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5ccdb-125">Next steps</span></span>

<span data-ttu-id="5ccdb-126">Kom igång med [Överför filer tooyour konto](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="5ccdb-126">Get started with [uploading files tooyour account](media-services-portal-upload-files.md).</span></span>
