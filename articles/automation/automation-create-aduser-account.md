---
title: "aaaCreate användarkonto i Azure AD | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate ett användarkonto i Azure AD-autentiseringen för runbooks i Azure Automation-tooauthenticate i Azure och klassiska Azure."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
keywords: azure active directory user, azure service management, azure ad user account
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 7c6ed4182dbab074d0bc5da7057f74ad321d8884
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Autentisera runbooks med den klassiska Azure-distributionen och Resource Manager
Den här artikeln beskriver hello steg du måste utföra tooconfigure ett användarkonto i Azure AD för Azure Automation-runbooks som körs mot Azure klassiska distributionsmodellen och Azure Resource Manager-resurser.  När detta fortsätter toobe en identitet för autentiseringsmetoder som stöds för din Azure Resource Manager baserad runbooks, hello bör använda ett Azure kör som-konto.       

## <a name="create-a-new-azure-active-directory-user"></a>Skapa en ny Azure Active Directory-användare
1. Logga in toohello Azure klassiska portal som tjänstadministratör för hello Azure-prenumeration du vill toomanage.
2. Välj **Active Directory**, och välj sedan hello namnet på din organisationskatalog.
3. Välj hello **användare** fliken och markera sedan under hello kommandot **Lägg till användare**.
4. På hello **berätta om den här användaren** sidan under **typ av användare**väljer **ny användare i din organisation**.
5. Ange ett användarnamn.  
6. Välj hello katalognamn som är associerad med din Azure-prenumeration på hello Active Directory-sidan.
7. På hello **användarprofil** anger du ett första och sista namn, ett användarvänligt namn och användaren från hello **roller** lista.  **Aktivera inte Multi-Factor Authentication**.
8. Observera hello användarens fullständiga namn och tillfälliga lösenord.
9. Välj **Inställningar > Administratörer > Lägg till**.
10. Ange hello fullständig hello användares användarnamn som du skapade.
11. Välj hello-prenumeration som du vill hello användaren toomanage.
12. Logga ut från Azure och sedan logga in igen med hello-konto som du nyss skapade. Du kommer att tillfrågas toochange hello användarens lösenord.

## <a name="create-an-automation-account-in-azure-classic-portal"></a>Skapa ett Automation-konto i den klassiska Azure-portalen
I det här avsnittet kan du utföra följande steg toocreate ett Azure Automation-konto i hello Azure-portalen för användning med dina runbooks hantera resurser i Azure klassiska distributionen hello.  

> [!NOTE]
> Automation-konton som skapats med hello Azure klassiska portal kan hanteras av både hello Azure klassiska Azure-portalen och antingen uppsättning cmdlets. När hello kontot skapas spelar det ingen roll hur du skapar och hanterar resurser i hello-konto. Om du planerar toocontinue toouse hello Azure klassiska portal, väljer du den i stället för hello Azure portal toocreate Automation-konton.
> 
> 

1. Logga in toohello Azure klassiska portal som tjänstadministratör för hello Azure-prenumeration du vill toomanage.
2. Välj **Automation**.
3. På hello **Automation** väljer **skapa ett Automation-konto**.
4. I hello **skapa ett Automation-konto** anger du ett namn på det nya Automation-kontot och välj en **Region** hello nedrullningsbara listan.  
5. Klicka på **OK** tooaccept dina inställningar och skapa hello-konto.
6. När den har skapats visas det på hello **Automation** sidan.
7. Klicka på hello-konto och den kommer att få toohello instrumentpanelssida.  
8. Välj på sidan för instrumentpanelen för automatisering av hello **tillgångar**.
9. På hello **tillgångar** väljer **lägga till inställningarna** finns på hello hello sidans nederkant.
10. På hello **lägger du till inställningar** väljer **lägga till autentiseringsuppgifter**.
11. På hello **definiera autentiseringsuppgifter** väljer **Windows PowerShell-autentiseringsuppgift** från hello **autentiseringstypen** nedrullningsbara listan och ange ett namn för hello autentiseringsuppgifter.
12. Hello följande **definiera autentiseringsuppgifter** sidan Ange hello användarnamn för hello AD-kontot som skapade tidigare i hello **användarnamn** fältet och hello lösenordet i hello **lösenord**och **Bekräfta lösenord** fält. Klicka på **OK** toosave ändringarna.

## <a name="create-an-automation-account-in-hello-azure-portal"></a>Skapa ett Automation-konto i hello Azure-portalen
I det här avsnittet, utföra hello följande steg toocreate ett Azure Automation-konto i hello Azure-portalen för användning med dina runbooks hantera resurser i Azure Resource Manager-läge.  

1. Logga in toohello Azure-portalen som tjänstadministratör för hello Azure-prenumeration du vill toomanage.
2. Välj **Automation-konton**.
3. I bladet för hello Automation-konton klickar du på **Lägg till**.<br><br>![Lägga till ett Automation-konto](media/automation-create-aduser-account/add-automation-acct-properties.png)
4. I hello **lägga till Automation-konto** bladet i hello **namnet** skriver ett namn på det nya Automation-kontot.
5. Om du har mer än en prenumeration kan ange hello en för hello nytt konto, samt en ny eller befintlig **resursgruppen** och ett Azure-datacenter **plats**.
6. Välj hello värde **Ja** för hello **skapa kör som-kontot Azure** alternativ och klickar på hello **skapa** knappen.  
   
    > [!NOTE]
    > Om du vill skapa toonot hello-kör som-konto genom att välja alternativet hello **nr**, visas ett varningsmeddelande i hello **lägga till Automation-konto** bladet.  Medan hello konto skapas och tilldelas toohello **deltagare** roll i hello prenumerationen har inte en motsvarande autentiseringsidentitet i katalogtjänsten prenumerationer och därför ingen åtkomst resurser i din prenumeration.  Detta förhindrar alla runbooks som refererar till det här kontot från att kunna tooauthenticate och utföra åtgärder mot resurser i Azure Resource Manager.
    > 
    >

    <br>![Varningsmeddelande för Lägg till Automation-konto](media/automation-create-aduser-account/add-automation-acct-properties-error.png)<br>  
7. Medan Azure skapar hello Automation-konto, du kan följa förloppet för hello under **meddelanden** hello-menyn.

Du måste toocreate en Autentiseringstillgång tooassociate hello Automation-konto med hello AD användarkonto skapade tidigare när hello skapandet av hello autentiseringsuppgifter har slutförts.  Kom ihåg att vi endast skapa hello Automation-kontot och är inte associerad med en autentiseringsidentitet.  Utföra hello steg som beskrivs i hello [autentiseringsuppgifter tillgångar i Azure Automation-artikel](automation-credentials.md#creating-a-new-credential-asset) och ange hello värde för **användarnamn** hello format **domän\användare**.

## <a name="use-hello-credential-in-a-runbook"></a>Använd hello autentiseringsuppgift i en runbook
Du kan hämta hello-autentiseringsuppgift i en runbook med hello [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) aktivitet och sedan använda den med [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) tooconnect tooyour Azure-prenumeration. Om hello autentiseringsuppgifter är administratör för Azure-prenumerationer och du bör också använda [Välj AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) toospecify hello korrekt. Detta visas i hello prov Windows PowerShell nedan som visas vanligtvis hello överst i de flesta Azure Automation-runbook.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Upprepa dessa rader efter eventuella [kontrollpunkter](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) i din runbook. Om hello runbook pausas och återupptas på en annan worker, behöver tooperform hello autentiseringen igen.

## <a name="next-steps"></a>Nästa steg
* Granska hello olika typer av runbookflöden och hur du skapar egna runbooks från hello följande artikel [typer för Azure Automation-runbook](automation-runbook-types.md)

