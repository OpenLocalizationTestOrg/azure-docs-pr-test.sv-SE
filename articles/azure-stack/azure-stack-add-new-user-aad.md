---
title: "Lägg till ett nytt Azure Stack klient konto i Azure Active Directory | Microsoft Docs"
description: "När du har distribuerat Microsoft Azure-stacken Development Kit måste du skapa minst en klient användarkonto så du kan utforska klientportalen."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a75d5c88-5b9e-4e9a-a6e3-48bbfa7069a7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: b7fd3c36825746a009c01c97fb8664e04278159f
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/08/2017
---
*Gäller för: Azure stacken Development Kit*

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Lägg till ett nytt Azure Stack klient konto i Azure Active Directory
Efter [distribuerar Azure Stack Development Kit](azure-stack-run-powershell-script.md), du behöver ett användarkonto för innehavare så att du kan utforska klientportalen och testa dina erbjudanden och planer. Du kan skapa ett innehavarkonto av [med hjälp av Azure portal](#create-an-azure-stack-tenant-account-using-the-azure-portal) eller [med hjälp av PowerShell](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Skapa ett konto för innehavare av Azure-stacken med Azure-portalen
Du måste ha en Azure-prenumeration att använda Azure-portalen.

1. Logga in på [Azure](https://portal.azure.com).
2. I Microsoft Azure vänstra navigeringsfältet klickar du på **Active Directory**.
3. Klicka på den katalog som du vill använda för Azure-stacken i kataloglistan eller skapa en ny.
4. Klicka på den här sidan directory **användare**.
5. Klicka på **Lägg till användare**.
6. I den **Lägg till användare** guiden i den **typ av användare** Välj **ny användare i din organisation**.
7. I den **användarnamn** Skriv ett namn för användaren.
8. I den  **@**  väljer du ett lämpligt alternativ.
9. Klicka på pilen Nästa.
10. I den **användarprofil** sidan i guiden och ange ett **Förnamn**, **efternamn**, och **visningsnamn**.
11. I den **rollen** Välj **användaren**.
12. Klicka på pilen Nästa.
13. På den **skaffa tillfälligt lösenord** klickar du på **skapa**.
14. Kopiera den **nytt lösenord**.
15. Logga in på Microsoft Azure med det nya kontot. Ändra lösenord när du uppmanas.
16. Logga in på `https://portal.local.azurestack.external` med ett nytt konto för att se klientportalen.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Skapa ett konto för innehavare av Azure-stacken med hjälp av PowerShell
Om du inte har en Azure-prenumeration kan du inte använda Azure-portalen för att lägga till ett användarkonto för innehavare. I det här fallet kan du använda Azure Active Directory-modulen för Windows PowerShell i stället.

> [!NOTE]
> Om du använder Account (Live ID) för att distribuera Azure Stack Development Kit, kan du inte använda AAD PowerShell för att skapa innehavarkonto. 
> 
> 

1. Installera den [Microsoft Online Services-inloggningsassistent för IT-proffs RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).
2. Installera den [Azure Active Directory-modulen för Windows PowerShell (64-bitars version)](http://go.microsoft.com/fwlink/p/?linkid=236297) och öppna den.
3. Kör följande cmdlets:

    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack Development Kit

            $msolcred = get-credential

    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".

            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId

    ```

1. Logga in på Microsoft Azure med det nya kontot. Ändra lösenord när du uppmanas.
2. Logga in på `https://portal.local.azurestack.external` med ett nytt konto för att se klientportalen.

