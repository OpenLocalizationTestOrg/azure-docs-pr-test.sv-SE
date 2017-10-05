---
title: "Lägga till eller ändra Azure-prenumeration för administratörsroller | Microsoft Docs"
description: "Beskriver hur du lägger till eller ändrar Medadministratör för Azure, tjänstadministratören och kontoadministratör"
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
ms.openlocfilehash: da5995535d42ed52772cb09e0f4da51bbf878748
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="add-or-change-azure-administrator-roles-that-manage-the-subscription-or-services"></a>Lägg till eller ändra Azure-administratörsroller som hanterar prenumerationen eller tjänster
Du kan ändra Azure administratören som hanterar din Azure-prenumeration eller hanterar Azure-tjänster används i din prenumeration. Om du vill visa faktureringsinformation för Azure och hantera prenumerationer som du måste logga in på den [Kontocenter](https://account.windowsazure.com/Home/Index) som kontoadministratör. 

## <a name="add-an-admin-for-a-subscription"></a>Lägg till en administratör för en prenumeration
Du kan lägga till en Azure-administratör i Azure-portalen eller i den klassiska Azure-portalen.

**Azure Portal**

Om du vill lägga till någon som en administratör för en prenumeration på Azure-portalen måste ge du dem rollen som ägare. Rollen som ägare kan bara hantera resurserna i prenumerationen som du tilldelade. Den har inte behörighet att använda till andra prenumerationer. Ägare som du lägger till via den [Azure-portalen](https://portal.azure.com) kan inte hantera resurs i den [klassiska Azure-portalen](https://manage.windowsazure.com).

1. Logga in på [Azure Portal](https://portal.azure.com).
2. På navmenyn väljer **prenumeration** > *den prenumeration som du vill att administratören att komma åt*.

    ![Skärmbild som visar den valda prenumerationen](./media/billing-add-change-azure-subscription-administrator/newselectsub.png)

3. Välj i prenumerationsbladet **åtkomstkontroll (IAM)**.
4. Välj **lägga till** > **rollen** > **ägare**. Skriv e-postadressen för den användare som du vill lägga till som ägare, Välj användaren och välj sedan **spara**.

    ![Skärmbild som visar ägarrollen som valts](./media/billing-add-change-azure-subscription-administrator/add-role.png)

5. Om du vill lägga till kontot ägare som medadministratör, i den **åtkomstkontroll (IAM)** sidan, högerklicka på användaren och välj sedan **Lägg till som medadministratör**. Den här funktionen finns nu i [Azure preview portal](https://preview.portal.azure.com/). 

     ![Skärmbild som lägger till medadministratör](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    >[!TIP]
    >Du måste lägga till användaren ”ägare” som medadministratör om användaren behöver hantera Azure-tjänster i [klassiska Azure-portalen](https://manage.windowsazure.com/).

    Om du vill ta bort behörigheten medadministratör, högerklicka på användaren som ”medadministratör” och välj sedan **ta bort medadministratör**.

    ![Skärmbild som tar bort medadministratör](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)


**Klassisk Azure-portal**

1. Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com/).
2. I navigeringsfönstret väljer **inställningar**> **administratörer**> **Lägg till**. </br>

    ![Skärmbild som visar hur du kommer till knappen Lägg till](./media/billing-add-change-azure-subscription-administrator/addcoadmin.png)
3. Skriv e-postadressen för den person som du vill lägga till som medadministratör och välj den prenumeration som du vill medadministratörerna åtkomst till.</br>

    ![Skärmbild som visar en prenumeration som har valts ](./media/billing-add-change-azure-subscription-administrator/addcoadmin2.png)</br>

Följande e-postadress kan läggas till som Medadministratör:

* **Account** (tidigare Windows Live ID) </br>
  Du kan använda ett Account för att logga in på alla konsumentinriktade Microsoft-produkter och molntjänster, till exempel Outlook (Hotmail), Skype MSN, OneDrive, Windows Phone och Xbox LIVE.
* **Organisationskonto**</br>
  Ett organisationskonto är ett konto som har skapats under Azure Active Directory. Adressen organisationskonto har följande format:

    användare @&lt;din domän&gt;. onmicrosoft.com

## <a name="change-service-administrator-for-a-subscription"></a>Ändra tjänstadministratör för en prenumeration
Enbart administratörskontot kan ändra den tjänstadministratör för en prenumeration.

1. Logga in på [Azure Kontocenter](https://account.windowsazure.com/subscriptions) med hjälp av kontoadministratören.
2. Välj den prenumeration som du vill ändra.
3. Välj höger, **redigera prenumeration** information. </br>

    ![editsub](./media/billing-add-change-azure-subscription-administrator/editsub.png)
4. I den **TJÄNSTADMINISTRATÖR** ange e-postadressen för den nya tjänstadministratören. </br>

    ![changeSA](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

## <a name="change-the-account-administrator"></a>Ändra kontoadministratören
Att överföra ägarskap för Azure-konto till ett annat konto finns [överföra ägarskap för en Azure-prenumeration](billing-subscription-transfer.md).

Vi rekommenderar starkt att du inte ta bort eller byta namn på kontot administratörens e-postadress. Du kan se oväntad och oönskat beteende med Azure-konto. Du kan inte vara kan logga in på Azure med det kontot, göra ändringar i kontot eller hantera resurser med det kontot. 

## <a name="check-the-account-administrator-of-the-subscription"></a>Kontrollera kontoadministratören för prenumerationen
Om du inte vet som kontoadministratör för din prenumeration, kan du använda följande steg för att ta reda på.

  1. Logga in på [Azure Portal](https://portal.azure.com).
  2. På navmenyn väljer **prenumeration**.
  3. Välj den prenumeration som du vill kontrollera och se **inställningar**.
  4. Välj **egenskaper**. Kontoadministratören för prenumerationen visas i den **kontoadministratören** rutan.  

## <a name="types-of-azure-admin-accounts"></a>Typer av Azure administratörskonton
 Kontoadministratör tjänstadministratören och medadministratör finns tre typer av administratörsroller i Microsoft Azure. I följande tabell beskrivs skillnaden mellan dessa administrativa roller.

| Administrativ roll | Gräns | Beskrivning |
| --- | --- | --- |
| Kontoadministratör (AA) |1 per Azure-konto |Det här är den person som registrerade sig för eller har köpt Azure-prenumerationer och har behörighet att komma åt den [Kontocenter](https://account.windowsazure.com/Home/Index) och utföra olika hanteringsuppgifter. Dessa inkluderar att kunna skapa prenumerationer, avbryta prenumerationer, ändra faktureringen för en prenumeration och ändra tjänstadministratören. |
| Tjänsten systemadministratörens (SA) |1 per Azure-prenumeration |Den här rollen har behörighet att hantera tjänster i den [Azure-portalen](https://portal.azure.com). Kontoadministratören är också den tjänstadministratör som standard för en ny prenumeration. |
| Medadministratör (CA) i den [klassiska Azure-portalen](https://manage.windowsazure.com) |200 per prenumeration |Den här rollen har samma åtkomstbehörigheter som tjänstadministratören, men kan inte ändra associationen mellan prenumerationer och Azure-kataloger. |

Azure Active Directory-rollbaserad åtkomstkontroll (RBAC) kan användare som ska läggas till flera roller. Mer information finns i [Azure Active Directory-rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).

## <a name="limitations-and-restrictions-for-admin-accounts"></a>Begränsningar och restriktioner för administratörskonton
* Varje prenumeration är associerad med en Azure AD-katalog (även kallat standard katalogen). Gå till för att hitta standardkatalog prenumerationen är associerad med den [klassiska Azure-portalen](https://manage.windowsazure.com/)väljer **inställningar** > **prenumerationer**. Kontrollera prenumerations-ID för att hitta katalogen som standard.
* Du är inloggad med ett Account, kan du bara lägga till andra Microsoft-Accounts eller användare i katalogen som standard som Medadministratör.
* Om du har loggat in med ett organisationskonto kan du lägga till andra organisationskonton i din organisation som Medadministratör. Till exempel abby@contoso.com kan lägga till bob@contoso.com som administratör eller Medadministratör, men det går inte att lägga till john@notcontoso.com om john@notcontoso.com är i katalogen. Användare som har loggat in med organisationskonton kan fortsätta att lägga till Account användare som administratör eller Medadministratör.
* Nu när det är möjligt att logga in på Azure med ett organisationskonto följer krav tjänstadministratören och medadministratör ändringarna:

  | Logga in metod | Lägga till Account eller användare i standardkatalog som CA: N eller SA? | Lägg till organisationens konto i samma organisation som Certifikatutfärdare eller SA? | Lägg till organisationens konto i en annan organisation som Certifikatutfärdare eller SA? |
  | --- | --- | --- | --- |
  |  Microsoft-konto |Ja |Nej |Nej |
  |  Organisationskonto |Ja |Ja |Nej |

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Mer information om åtkomstkontroll till resurser och Active Directory
* Läs mer om hur resursåtkomsten hanteras i Microsoft Azure i [förstå resursåtkomst i Azure](../active-directory/active-directory-understanding-resource-access.md).
* Läs mer om Azure Active Directory [hur Azure-prenumerationer är associerade med Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) och [Tilldela administratörsroller i Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.
