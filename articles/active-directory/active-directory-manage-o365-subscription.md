---
title: "aaaManage hello katalogen för din Office 365-prenumeration i Azure | Microsoft Docs"
description: Hantera en Office 365-prenumerationskatalog med Azure Active Directory och hello klassiska Azure-portalen
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 746987b7-2dfd-4b35-819d-37c1b65c5c81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 4faf9936d7e94b85356a18adfcf3d2a48e5b025f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-directory-for-your-office-365-subscription-in-azure"></a>Hantera hello katalogen för din Office 365-prenumeration i Azure
Den här artikeln beskriver hur toomanage en katalog som har skapats för en prenumeration på Office 365 med hello klassiska Azure-portalen. Du måste vara antingen hello tjänstadministratör eller en medadministratör för en Azure-prenumeration toosign i toohello klassiska Azure-portalen. Om du ännu inte har någon Azure-prenumeration kan du registrera dig för en [kostnadsfria 30-dagars utvärderingsversion](https://azure.microsoft.com/trial/get-started-active-directory/) och redan idag distribuera din första molnlösning inom 5 minuter med hjälp av den här länken. Vara säker på att toouse hello arbete eller skolkonto som du använder toosign i tooOffice 365.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.

När du har slutfört hello Azure-prenumeration kan du logga in toohello klassiska Azure-portalen och komma åt Azure-tjänster. Klicka på hello Active Directory-tillägget toomanage hello samma katalog som autentiserar dina Office 365-användare.

Om du redan har en Azure-prenumeration, är hello processen toomanage en ytterligare katalog också enkelt. Anta till exempel att Michael Smith har en prenumeration på Office 365 för Contoso.com. Han har också en Azure-prenumeration som han har registrerat sig för med sitt Microsoft-konto, msmith@hotmail.com. I detta fall hanterar han två kataloger.

| Prenumeration | Office 365 | Azure |
| --- | --- | --- |
|   Visningsnamn |Contoso |Azure Active Directory-standardkatalog (Azure AD) |
|   Domännamn |contoso.com |msmithhotmail.onmicrosoft.com |

Han vill toomanage hello användaridentiteter i hello Contoso-katalogen när han är inloggad i tooAzure med sitt Microsoft-konto så att han kan använda Azure AD-funktioner, till exempel flerfunktionsautentisering. följande diagram hello kan hjälpa tooillustrate hello processen.

![Diagram över toomanage två oberoende kataloger](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

I det här fallet är hello två kataloger oberoende av varandra.

## <a name="toomanage-two-independent-directories"></a>toomanage två oberoende kataloger
I ordning för Michael Smith toomanage båda katalogerna när han är inloggad i tooAzure som msmith@hotmail.com, han utföra hello följande steg:

> [!NOTE]
> Dessa steg kan endast utföras när en användare är inloggad med ett Microsoft-konto. Om hello användare är inloggad med ett konto för arbetet eller skolan, hello alternativet för**Använd befintlig katalog** är inte tillgänglig. Ett arbets- eller skolkonto konto kan endast autentiseras av sin hemkatalog (dvs hello katalogen där hello arbets-eller skolkontot lagras och som hello företag eller din skola äger).
>
>

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) som msmith@hotmail.com.
2. Klicka på **Nytt** > **Apptjänster** > **Active Directory** > **Katalog** > **Skapa anpassade**.
3. Klicka på Använd befintlig katalog och markera hello **jag är redo toobe utloggad nu** kryssrutan.
4. Logga in toohello klassiska Azure-portalen som global administratör för Contoso.onmicrosoft.com (till exempel msmith@contoso.com).
5. När du uppmanas för**Använd hello Contoso-katalogen med Azure?**, klickar du på **Fortsätt**.
6. Klicka på **Logga ut nu**.
7. Logga in toohello klassiska Azure-portalen som msmith@hotmail.com. hello Contoso-katalogen och standardkatalogen hello visas i hello Active Directory-tillägget.

När du har slutfört de här stegen msmith@hotmail.com är en global administratör i hello Contoso-katalogen.

## <a name="tooadminister-resources-as-hello-global-admin"></a>tooadminister resurser som hello global administratör
Nu ska vi anta att Jane Doe behöver administrera webbplatser och databasresurser som är associerade med hello Azure-prenumeration för msmith@hotmail.com. Innan hon kan göra det måste Michael Smith toocomplete dessa ytterligare steg:

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) med hello tjänstadministratörskontot för hello Azure-prenumeration (i det här exemplet msmith@hotmail.com).
2. Överför hello prenumeration toohello Contoso-katalogen: Klicka på **inställningar** > **prenumerationer** > Välj hello prenumeration > **redigera katalog** > Välj **Contoso (Contoso.com)**. Som en del av hello överföringen eventuella arbets- eller Skol tas-konton som är medadministratörer i hello prenumeration bort.
3. Lägga till Jane Doe som medadministratör för prenumerationen hello: Klicka på **inställningar** > **administratörer** > Välj hello prenumeration > **Lägg till** > typ **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Nästa steg
Mer information om hello relationen mellan prenumerationer och kataloger finns [hur en prenumeration är associerad med en katalog](active-directory-how-subscriptions-associated-directory.md).
