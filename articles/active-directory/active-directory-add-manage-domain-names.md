---
title: "aaaManaging anpassade domännamn i Azure Active Directory | Microsoft Docs"
description: "Management-begrepp och instruktioner för att hantera anpassade domäner i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cf3523bd-9ee0-439e-963d-ccea038867b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 4b6d06fecf3be0621be51c38a1330eafdc1b4d35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Hantera anpassade domännamn i Azure Active Directory
Ett domännamn kan vara en viktig identifierare för många directory-resurser som en del av:

* Ett användarnamn eller e-postadress för en användare
* hello-adressen för en grupp
* hello app-ID URI för ett program

En resurs i Azure Active Directory (Azure AD) kan innehålla ett domännamn som redan har verifierats toobe ägs av hello katalog som innehåller hello-resurs. Endast en global administratör kan utföra hanteringsuppgifter för domänen i Azure AD.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. Hur toomanage ditt domännamn i administrationscentret för hello Azure AD, finns i [hantera anpassade domännamn i Azure Active Directory](active-directory-domains-manage-azure-portal.md).

## <a name="set-hello-primary-domain-name-for-your-azure-ad-directory"></a>Ange namnet på hello primär domän för din Azure AD-katalog
När din katalog har skapats är också hello inledande domännamn, till exempel 'contoso.onmicrosoft.com' hello primära domännamn för din katalog. hello primär domän är hello standarddomännamnet för en ny användare när du skapar en ny användare i hello [klassiska Azure-portalen](https://manage.windowsazure.com/), eller andra portaler, till exempel hello Office 365 administrationsportal. Detta förenklar hello process för en administratör toocreate nya användare i hello-portalen.

toochange hello primära domännamn för din katalog:

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) med ett användarkonto som är en global administratör i Azure AD-katalogen.
2. Välj **Active Directory** hello vänstra navigeringsfältet.
3. Öppna din katalog.
4. Välj hello **domäner** fliken.
5. Välj hello **ändra primära** hello kommandofältet-knappen.
6. Välj hello-domän som du vill toobe hello nya primär domän för din katalog.

Du kan ändra hello primära domännamn för din katalog toobe verifierade domänen som inte är federerat. Ändra hello primär domän för din katalog ändrar inte hello användarnamn för eventuella befintliga användare.

## <a name="add-custom-domain-names-tooyour-azure-ad"></a>Lägg till anpassad domän namn tooyour Azure AD
Du kan lägga upp too900 domänen namn tooeach Azure AD-katalog. Hej processen för[lägga till ytterligare ett eget domännamn](active-directory-add-domain.md) hello samma hello första anpassade domännamn.

## <a name="add-subdomains-of-a-custom-domain"></a>Lägger till underdomäner i en anpassad domän
Om du vill tooadd ett tredje nivån domännamn, till exempel 'europe.contoso.com' tooyour directory, bör du först lägga till och kontrollera hello andranivådomän, t.ex contoso.com. hello underdomän verifieras automatiskt av Azure AD. toosee som hello underdomän som du just lagt har verifierats, uppdatera hello sida i hello webbläsare som visar hello domäner i katalogen.

## <a name="what-toodo-if-you-change-hello-dns-registrar-for-your-custom-domain-name"></a>Vilka toodo om du ändrar hello DNS-registratorns för ditt domännamn
Om du ändrar hello DNS-registratorns för ditt domännamn kan du fortsätta toouse ditt domännamn med Azure AD själva utan avbrott och utan ytterligare konfigurationsåtgärder. Om du använder ditt domännamn med Office 365, Intune eller andra tjänster som är beroende av anpassade domännamn i Azure AD läser du toohello dokumentationen för dessa tjänster.

## <a name="delete-a-custom-domain-name"></a>Ta bort ett anpassat domännamn
Du kan ta bort ett anpassat domännamn från din Azure AD, om organisationen inte längre använder domännamnet eller om du behöver toouse domännamnet med en annan Azure AD.

toodelete ett anpassat domännamn, måste du först kontrollera att inga resurser i din katalog är beroende av hello domännamn. Du kan inte ta bort ett domännamn från katalogen om:

* Alla användare har ett användarnamn, e-postadress eller proxyadress som innehåller hello domännamn.
* Valfri grupp har en e-postadress eller proxyadress som innehåller hello domännamn.
* Alla program i din Azure AD har en app-ID-URI som innehåller hello domännamn.

Du måste ändra eller ta bort alla dessa resurser i Azure AD-katalogen innan du kan ta bort hello domännamn.

## <a name="use-powershell-or-graph-api-toomanage-domain-names"></a>Använd PowerShell eller Graph API toomanage domännamn
De flesta hanteringsuppgifter för domännamn i Azure Active Directory kan också utföras med Microsoft PowerShell eller via programmering med Azure AD Graph API.

* [Med hjälp av PowerShell toomanage domännamn i Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Med hjälp av Graph API toomanage domännamn i Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Nästa steg
* [Lär dig mer om domännamn i Azure AD](active-directory-add-domain-concepts.md)
* [Hantera egna domännamn](active-directory-add-manage-domain-names.md)

