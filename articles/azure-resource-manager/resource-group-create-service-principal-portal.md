---
title: "aaaCreate identitet för Azure-app i portalen | Microsoft Docs"
description: "Beskriver hur toocreate ett nytt Azure Active Directory-program och tjänstens huvudnamn som kan användas med hello rollbaserad åtkomstkontroll i Azure Resource Manager toomanage åt tooresources."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 9624715ac612f42df6f9e9e67b8233bd4b914174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-portal-toocreate-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Använda portalen toocreate ett Azure Active Directory-program och tjänstens huvudnamn som har åtkomst till resurser

När du har ett program som behöver tooaccess eller ändra resurser, måste du konfigurera ett program för Azure Active Directory (AD) och tilldelar hello krävs behörigheter tooit. Den här metoden är bättre toorunning hello app enligt dina autentiseringsuppgifter eftersom:

* Du kan tilldela behörigheter toohello app identitet som skiljer sig från din egen behörighet. Dessa behörigheter normalt begränsade tooexactly vilka hello program behöver toodo.
* Toochange hello app autentiseringsuppgifter har inte ändrar dina ansvarsområden. 
* Du kan använda en tooautomate certifikatautentisering vid körning av ett oövervakat skript.

Det här avsnittet visar hur tooperform de går igenom hello-portalen. Den fokuserar på en enskild klient-program där programmet hello är avsedda toorun i en enda organisation. Du använder vanligtvis stöd för en innehavare program för line-of-business-program som körs i din organisation.
 
## <a name="required-permissions"></a>Behörigheter som krävs
toocomplete det här avsnittet, du måste ha tillräckliga behörigheter tooregister ett program med Azure AD-klienten och tilldela hello programmet tooa roll i din Azure-prenumeration. Vi behöver kontrollera att du har hello rätt behörigheter tooperform de här stegen.

### <a name="check-azure-active-directory-permissions"></a>Kontrollera behörigheter för Azure Active Directory
1. Logga in tooyour Azure-konto via hello [Azure-portalen](https://portal.azure.com).
2. Välj **Azure Active Directory**.

     ![Välj azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)
3. Välj i Azure Active Directory, **användarinställningar**.

     ![Välj användarinställningar](./media/resource-group-create-service-principal-portal/select-user-settings.png)
4. Kontrollera hello **App registreringar** inställningen. Om anges för**Ja**, icke-administratörer kan registrera AD-appar. Den här inställningen innebär att alla användare i hello Azure AD-klient kan registrera en app. Du kan fortsätta för[Kontrollera Azure-Prenumerationsbehörigheter](#check-azure-subscription-permissions).

     ![Visa appen registreringar](./media/resource-group-create-service-principal-portal/view-app-registrations.png)
5. Om hello app registreringar inställning har angetts för**nr**, men endast administrativa användare kan registrera appar. Du måste toocheck om ditt konto är administratör för hello Azure AD-klient. Välj **översikt** och **söka efter en användare** från snabb uppgifter.

     ![Sök efter användare](./media/resource-group-create-service-principal-portal/find-user.png)
6. Sök efter ditt konto och markera den när du har hittat.

     ![Sök användare](./media/resource-group-create-service-principal-portal/show-user.png)
7. Ditt konto, Välj **Directory rollen**. 

     ![Directory roll](./media/resource-group-create-service-principal-portal/select-directory-role.png)
8. Visa tilldelade directory-rollen i Azure AD. Om ditt konto har tilldelats användarrollen toohello, men hello registrering appinställning (från föregående steg hello) är begränsad tooadmin användare, kan du be din administratör tooeither tilldela tooan administratörsroll eller tooenable användare tooregister appar.

     ![Visa roll](./media/resource-group-create-service-principal-portal/view-role.png)

### <a name="check-azure-subscription-permissions"></a>Kontrollera behörigheter för Azure-prenumeration
Kontot måste ha i din Azure-prenumeration `Microsoft.Authorization/*/Write` komma åt tooassign en AD app tooa roll. Den här åtgärden beviljas genom hello [ägare](../active-directory/role-based-access-built-in-roles.md#owner) roll eller [administratör för användaråtkomst](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) roll. Om ditt konto har tilldelats toohello **deltagare** roll, du har inte tillräcklig behörighet. Du får ett fel vid försök tooassign hello service principal tooa roll. 

toocheck din Prenumerationsbehörighet:

1. Om du inte redan tittar på Azure AD-kontot från hello föregående steg, väljer **Azure Active Directory** från hello till vänster.

2. Hitta din Azure AD-kontot. Välj **översikt** och **söka efter en användare** från snabb uppgifter.

     ![Sök efter användare](./media/resource-group-create-service-principal-portal/find-user.png)
2. Sök efter ditt konto och markera den när du har hittat.

     ![Sök användare](./media/resource-group-create-service-principal-portal/show-user.png) 
     
3. Välj **Azure-resurser**.

     ![Välj resurser](./media/resource-group-create-service-principal-portal/select-azure-resources.png) 
3. Visa tilldelade rollerna och kontrollera om du har tillräcklig behörighet tooassign en AD app tooa roll. Om inte, ber du din prenumeration administratören tooadd du tooUser åtkomst rollen Administratör. I följande bild hello, är hello användare tilldelade toohello ägarrollen för två prenumerationer, vilket innebär att användaren har tillräcklig behörighet. 

     ![Visa behörighet](./media/resource-group-create-service-principal-portal/view-assigned-roles.png)

## <a name="create-an-azure-active-directory-application"></a>Skapa ett Azure Active Directory-program
1. Logga in tooyour Azure-konto via hello [Azure-portalen](https://portal.azure.com).
2. Välj **Azure Active Directory**.

     ![Välj azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

4. Välj **App registreringar**.   

     ![Välj app-registreringar](./media/resource-group-create-service-principal-portal/select-app-registrations.png)
5. Välj **Lägg till**.

     ![Lägg till app](./media/resource-group-create-service-principal-portal/select-add-app.png)

6. Ange ett namn och URL: en för programmet hello. Välj antingen **webbapp / API** eller **interna** hello typen av program som du vill ha toocreate. När du har angett hello värden, Välj **skapa**.

     ![namn på program](./media/resource-group-create-service-principal-portal/create-app.png)

Du har skapat ditt program.

## <a name="get-application-id-and-authentication-key"></a>Hämta program-ID och autentisering nyckel
När du programmässigt loggar in, behöver du hello-ID för ditt program och en autentiseringsnyckel. tooget dessa värden används hello följande steg:

1. Från **App registreringar** i Azure Active Directory, väljer du ditt program.

     ![Välj program](./media/resource-group-create-service-principal-portal/select-app.png)
2. Kopiera hello **program-ID** och lagra den i din programkod. Hej program i hello [programexempel](#sample-applications) avsnittet finns toothis värde som hello klient-id.

     ![klient-id](./media/resource-group-create-service-principal-portal/copy-app-id.png)
3. Välj toogenerate en autentiseringsnyckel **nycklar**.

     ![Välj nycklar](./media/resource-group-create-service-principal-portal/select-keys.png)
4. Ange en beskrivning av hello nyckel och en varaktighet för hello nyckeln. När du är klar väljer **spara**.

     ![Spara nyckel](./media/resource-group-create-service-principal-portal/save-key.png)

     När du har sparat hello nyckeln visas hello värdet för hello nyckeln. Kopiera det här värdet eftersom du inte kan tooretrieve hello nyckeln senare. Du kan ange hello nyckel/värde med hello program-ID toolog i som hello program. Lagra hello nyckelvärdet där programmet kan hämta.

     ![Spara nyckel](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Hämta klient-ID
När programmässigt inloggningen måste toopass hello klient-ID med din autentiseringsbegäran. 

1. tooget hello klient-ID, Välj **egenskaper** för din Azure AD-klient. 

     ![Välj Azure AD-egenskaper](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

2. Kopiera hello **katalog-ID**. Det här värdet är klient-ID.

     ![klient-id](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-toorole"></a>Tilldela programmet toorole
tooaccess resurser i din prenumeration, måste du tilldela hello tooa programrollen. Bestäm vilken roll representerar hello rätt behörigheter för hello program. toolearn om hello tillgängliga roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md).

Du kan ange hello scope på hello nivå hello prenumerationen, resursgruppen eller resursen. Behörigheter som är ärvda toolower nivåer av omfång. Till exempel kan att lägga till en programmet toohello Reader roll för en resursgrupp innebär att den läsa hello resursgruppen och alla resurser som den innehåller.

1. Navigera toohello nivå av omfång som du vill tooassign hello programmet till. Välj till exempel tooassign en roll i hello prenumerationsomfattningen **prenumerationer**. Du kan i stället välja en resursgrupp eller resurs.

     ![Välj prenumeration](./media/resource-group-create-service-principal-portal/select-subscription.png)

2. Välj hello viss prenumeration (resursgrupp eller resurs) tooassign hello.

     ![Välj prenumerationen för tilldelning](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

3. Välj **åtkomstkontroll (IAM)**.

     ![Välj åtkomst](./media/resource-group-create-service-principal-portal/select-access-control.png)

4. Välj **Lägg till**.

     ![Välj Lägg till](./media/resource-group-create-service-principal-portal/select-add.png)
6. Välj hello-roll som du vill tooassign toohello program. hello följande bild visar hello **Reader** roll.

     ![Välj rollen](./media/resource-group-create-service-principal-portal/select-role.png)

8. Sök efter programmet och markera den.

     ![Sök efter app](./media/resource-group-create-service-principal-portal/search-app.png)
9. Välj **OK** toofinish tilldela hello roll. Du ser ditt program i hello lista över användare som tilldelats tooa roll för detta omfång.

## <a name="log-in-as-hello-application"></a>Logga in som hello program

Programmet har konfigurerats i Azure Active Directory. Du har ett ID och nyckel toouse för att logga in som hello program. hello programmet tilldelas tooa roll som ger den vissa åtgärder kan utföras. För information om att logga in som hello program via olika plattformar, se:

* [PowerShell](resource-group-authenticate-service-principal.md#provide-credentials-through-powershell)
* [Azure CLI](resource-group-authenticate-service-principal-cli.md#provide-credentials-through-azure-cli)
* [REST](/rest/api/#create-the-request)
* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Nästa steg
* tooset upp ett program med flera innehavare, se [Developer's guide tooauthorization med hello Azure Resource Manager API](resource-manager-api-authentication.md).
* toolearn om hur du anger IPSec-principer finns [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).  
* En lista över tillgängliga åtgärder som kan beviljas eller nekas toousers finns [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).
