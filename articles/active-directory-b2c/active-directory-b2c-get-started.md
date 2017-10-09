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
# <a name="create-an-azure-active-directory-b2c-tenant-in-hello-azure-portal"></a>Skapa en Azure Active Directory B2C-klient i hello Azure-portalen

Redigera Sipi.

Den här snabbstarten hjälper dig att skapa en Microsoft Azure Active Directory (AD Azure) B2C-klient på bara några minuter. När du är klar har du toouse en B2C-klient för att registrera B2C-program.

## <a name="prerequisites"></a>Krav

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

##  <a name="log-in-tooazure"></a>Logga in tooAzure

Logga in toohello [Azure-portalen](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Skapa en Azure AD B2C-klient

B2C-funktioner kan inte aktiveras i din befintliga klienter. Du måste toocreate en Azure AD B2C-klient.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

Grattis, du har skapat en Azure Active Directory B2C-klient. Du är en Global administratör för hello-klienten. Du kan lägga till andra globala administratörer om det behövs. tooswitch tooyour ny klient klickar du på hello *hantera nya klient länken*.

![Hantera din ny klient-länk](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> Om du planerar toouse en B2C-klient för en produktionsapp artikeln hello på [produktionsskala kontra preview B2C hyresgäster](active-directory-b2c-reference-tenant-type.md). Det finns kända problem när du tar bort en befintlig B2C-klient och skapa den igen med hello samma domännamn. Du måste toocreate en B2C-klient med ett annat domännamn.
>
>

## <a name="link-your-tenant-tooyour-subscription"></a>Länka din klient tooyour prenumeration

Du behöver toolink din Azure AD B2C-klient tooyour Azure-prenumeration tooenable alla B2C-funktioner och betala för avgifter för användning. toolearn läsa fler [i den här artikeln](active-directory-b2c-how-to-enable-billing.md). Om du inte länka ditt Azure-prenumeration med Azure AD B2C-klient tooyour vissa funktioner blockeras och visas ett varningsmeddelande (”ingen prenumeration länkade toothis B2C-klient eller hello prenumeration behöver kontrolleras”.) i hello B2C-inställningar. Det är viktigt att du vidtar åtgärden innan du levererar dina appar i produktionen.

## <a name="easy-access-toosettings"></a>Enkel åtkomst toosettings

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

Du kan också komma åt hello bladet genom att ange `Azure AD B2C` i **söka resurser** hello överst i hello-portalen. Markera i hello resultat **Azure AD B2C** tooaccess hello B2C inställningsbladet.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Registrera din B2C-program i en B2C-klient](active-directory-b2c-app-registration.md)
