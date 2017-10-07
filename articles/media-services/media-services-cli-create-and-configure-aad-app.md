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
# <a name="use-cli-20-toocreate-an-aad-app-and-configure-it-tooaccess-azure-media-services-api"></a>Använd CLI 2.0 toocreate en AAD-app och konfigurera den tooaccess Azure Media Services API

Det här avsnittet visar hur toouse CLI 2.0 toocreate ett Azure Active Directory (Azure AD)-program och tjänstens huvudnamn tooaccess Azure Media Services resurser. 

## <a name="prerequisites"></a>Krav

- Ett Azure-konto. Mer information finns i avsnittet om [den kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Ett Media Services-konto. Mer information finns i [skapa ett Azure Media Services-konto med hello Azure-portalen](media-services-portal-create-account.md).

## <a name="use-hello-azure-cloud-shell"></a>Använd hello Azure Cloud Shell

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Starta hello molnet Shell från hello övre navigeringsfönstret på hello-portalen.

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

Mer information finns i [översikt av Azure Cloud Shell](../cloud-shell/overview.md).

## <a name="create-an-azure-ad-app-and-configure-access-toohello-media-account-with-cli-20"></a>Skapa en Azure AD-app och konfigurera åtkomstkonto toohello media med CLI 2.0
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create -- assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

Exempel:

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

I det här exemplet hello **omfång** är hello fullständig resursens sökväg för hello media services-konto. Dock hello **omfång** kan när som helst.

Det kan till exempel vara något av följande nivåer hello:
 
* Hej **prenumeration** nivå.
* Hej **resursgruppen** nivå.
* Hej **resurs** nivå (till exempel ett konto för Media).

Mer information finns i [skapa en Azure tjänstens huvudnamn med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)

Se även [Manage Role-Based åtkomstkontroll med hello Azure-kommandoradsgränssnittet](../active-directory/role-based-access-control-manage-access-azure-cli.md). 

## <a name="next-steps"></a>Nästa steg

Kom igång med [Överför filer tooyour konto](media-services-portal-upload-files.md).
