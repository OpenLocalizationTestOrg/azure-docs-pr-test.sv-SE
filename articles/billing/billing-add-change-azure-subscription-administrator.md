---
title: "aaaAdd eller ändra Azure-prenumeration administratörsroller | Microsoft Docs"
description: "Beskriver hur tooadd eller ändra Medadministratör för Azure, tjänstadministratören och kontoadministratör"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: genli
ms.openlocfilehash: 14eaecf2dbfd7152775ac3552bf3a7ae3db596b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-change-azure-administrator-roles-that-manage-hello-subscription-or-services"></a>Lägg till eller ändra Azure-administratörsroller som hanterar hello prenumeration eller tjänster
Du kan ändra hello Azure administratören som hanterar din Azure-prenumeration eller hanterar hello Azure-tjänster används i din prenumeration. tooview Azure faktureringsinformation och hantera prenumerationer måste du logga in toohello [Kontocenter](https://account.windowsazure.com/Home/Index) som hello kontoadministratör. 

## <a name="add-an-admin-for-a-subscription"></a>Lägg till en administratör för en prenumeration
Du kan lägga till en Azure-administratör i hello Azure-portalen eller hello klassiska Azure-portalen.

**Azure Portal**

tooadd någon som administratör för en prenumeration i hello Azure-portalen, du ge dem hello ägarrollen. hello ägarrollen kan endast hantera hello resurser i hello-prenumeration som du tilldelade. Den har inte åtkomst privilegium tooother prenumerationer. Hej ägare som du lägger till via hello [Azure-portalen](https://portal.azure.com) kan inte hantera resurs i hello [klassiska Azure-portalen](https://manage.windowsazure.com).

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. På navmenyn hello väljer **prenumeration** > *hello prenumeration som du vill hello admin tooaccess*.

    ![Skärmbild som visar den valda prenumerationen](./media/billing-add-change-azure-subscription-administrator/newselectsub.png)

3. Välj i hello prenumerationsbladet **åtkomstkontroll (IAM)**.
4. Välj **lägga till** > **rollen** > **ägare**. Skriv hello hello användarens tooadd som ägare, Välj hello användare, och sedan markera e-postadress **spara**.

    ![Skärmbild som visar hello ägarrollen markerat](./media/billing-add-change-azure-subscription-administrator/add-role.png)

5. Om du vill tooadd hello ägare kontot som medadministratör i hello **åtkomstkontroll (IAM)** sidan, högerklicka på hello användare och välj sedan **Lägg till som medadministratör**. Den här funktionen finns nu i [Azure preview portal](https://preview.portal.azure.com/). 

     ![Skärmbild som lägger till medadministratör](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    >[!TIP]
    >Du behöver tooadd hello ”ägare” användare som medadministratör om hello användare behöver toomanage hello Azure-tjänster i [klassiska Azure-portalen](https://manage.windowsazure.com/).

    tooremove hello medadministratör behörighet, högerklicka på hello ”medadministratör” användare och välj sedan **ta bort medadministratör**.

    ![Skärmbild som tar bort medadministratör](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)


**Klassisk Azure-portal**

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/).
2. Hello navigeringsfönstret och välj **inställningar**> **administratörer**> **Lägg till**. </br>

    ![Skärmbild som visar hur tooget toohello knappen Lägg till](./media/billing-add-change-azure-subscription-administrator/addcoadmin.png)
3. Ange hello hello person som du vill tooadd som medadministratör och välj sedan hello-prenumeration som du vill hello medadministratör tooaccess e-postadress.</br>

    ![Skärmbild som visar en prenumeration som har valts ](./media/billing-add-change-azure-subscription-administrator/addcoadmin2.png)</br>

hello följande e-postadress kan läggas till som Medadministratör:

* **Account** (tidigare Windows Live ID) </br>
  Du kan använda en Account toosign i tooall konsumentinriktade Microsoft-produkter och molntjänster, till exempel Outlook (Hotmail), Skype MSN, OneDrive, Windows Phone och Xbox LIVE.
* **Organisationskonto**</br>
  Ett organisationskonto är ett konto som har skapats under Azure Active Directory. Hej organisationskonto adress har följande format:

    användare @&lt;din domän&gt;. onmicrosoft.com

## <a name="change-service-administrator-for-a-subscription"></a>Ändra tjänstadministratör för en prenumeration
Endast hello kontoadministratör kan ändra hello tjänstadministratör för en prenumeration.

1. Logga in för[Azure Kontocenter](https://account.windowsazure.com/subscriptions) med hjälp av hello kontoadministratör.
2. Välj hello-prenumeration som du vill toochange.
3. Välj hello höger **redigera prenumeration** information. </br>

    ![editsub](./media/billing-add-change-azure-subscription-administrator/editsub.png)
4. I hello **TJÄNSTADMINISTRATÖR** ange hello e-postadress hello nya tjänstadministratören. </br>

    ![changeSA](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

## <a name="change-hello-account-administrator"></a>Ändra hello kontoadministratör
tootransfer ägarskap för hello Azure-konto tooanother konto, se [överföra ägarskap för en Azure-prenumeration](billing-subscription-transfer.md).

Vi rekommenderar starkt att du inte ta bort eller byta namn på hello konto administratörens e-postadress. Du kan se oväntad och oönskat beteende med hello Azure-konto. Du kan inte vara kan logga in tooAzure med det kontot, ändrar toohello konto eller hantera resurser med det kontot. 

## <a name="check-hello-account-administrator-of-hello-subscription"></a>Kontrollera hello kontoadministratör för hello prenumerationen
Om du inte vet som hello kontoadministratör för din prenumeration, använder du följande steg toofind ut hello.

  1. Logga in toohello [Azure-portalen](https://portal.azure.com).
  2. På navmenyn hello väljer **prenumeration**.
  3. Välj hello prenumeration du vill använda toocheck och tittar du under **inställningar**.
  4. Välj **egenskaper**. hello kontoadministratör för hello prenumerationen visas i hello **kontoadministratören** rutan.  

## <a name="types-of-azure-admin-accounts"></a>Typer av Azure administratörskonton
 Kontoadministratören, tjänstadministratören och medadministratör är hello tre typer av administratörsroller i Microsoft Azure. hello beskrivs följande tabell hello skillnaden mellan dessa administrativa roller.

| Administrativ roll | Gräns | Beskrivning |
| --- | --- | --- |
| Kontoadministratör (AA) |1 per Azure-konto |Detta är hello person som registrerade sig för eller har köpt Azure-prenumerationer och är auktoriserade tooaccess hello [Kontocenter](https://account.windowsazure.com/Home/Index) och utföra olika hanteringsuppgifter. Dessa inkluderar att kan toocreate prenumerationer, avbryta prenumerationer, ändra hello fakturering för en prenumeration och ändra hello tjänstadministratör. |
| Tjänsten systemadministratörens (SA) |1 per Azure-prenumeration |Den här rollen är auktoriserade toomanage tjänster i hello [Azure-portalen](https://portal.azure.com). Som standard för en ny prenumeration är hello kontoadministratör också hello tjänstadministratör. |
| Medadministratör (CA) i hello [klassiska Azure-portalen](https://manage.windowsazure.com) |200 per prenumeration |Den här rollen har hello samma behörighet som hello tjänstadministratör, men kan inte ändra hello associationen mellan prenumerationer tooAzure kataloger. |

Azure Active Directory-rollbaserad åtkomstkontroll (RBAC) kan användare toobe läggs toomultiple roller. Mer information finns i [Azure Active Directory-rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).

## <a name="limitations-and-restrictions-for-admin-accounts"></a>Begränsningar och restriktioner för administratörskonton
* Varje prenumeration är associerad med en Azure AD-katalog (även kallat hello standardkatalog). toofind hello standardkatalog hello prenumeration är associerad med gå toohello [klassiska Azure-portalen](https://manage.windowsazure.com/)väljer **inställningar** > **prenumerationer**. Kontrollera hello prenumerations-ID toofind hello standardkatalog.
* Du är inloggad med ett Account kan du endast lägga till andra Microsoft-Accounts eller användare i hello standardkatalog som Medadministratör.
* Om du har loggat in med ett organisationskonto kan du lägga till andra organisationskonton i din organisation som Medadministratör. Till exempel abby@contoso.com kan lägga till bob@contoso.com som administratör eller Medadministratör, men det går inte att lägga till john@notcontoso.com om john@notcontoso.com är i katalogen. Användare som har loggat in med organisationskonton kan fortsätta tooadd Account användare som administratör eller Medadministratör.
* Nu när det är möjligt toosign i tooAzure med ett organisationskonto är här hello ändringar tooService administratör och medadministratör krav:

  | Logga in metod | Lägga till Account eller användare i standardkatalog som CA: N eller SA? | Lägg till organisationens konto i hello samma organisation som Certifikatutfärdare eller SA? | Lägg till organisationens konto i en annan organisation som Certifikatutfärdare eller SA? |
  | --- | --- | --- | --- |
  |  Microsoft-konto |Ja |Nej |Nej |
  |  Organisationskonto |Ja |Ja |Nej |

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Mer information om åtkomstkontroll till resurser och Active Directory
* toolearn mer information om hur resursåtkomsten hanteras i Microsoft Azure finns [förstå resursåtkomst i Azure](../active-directory/active-directory-understanding-resource-access.md).
* Läs mer om Azure Active Directory [hur Azure-prenumerationer är associerade med Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) och [Tilldela administratörsroller i Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.
