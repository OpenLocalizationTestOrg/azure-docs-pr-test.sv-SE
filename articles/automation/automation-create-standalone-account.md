---
title: "aaaCreate fristående Azure Automation-konto | Microsoft Docs"
description: "Självstudiekurs som vägleder dig genom hello skapa, testa och exempel användningen av primära autentiseringsmetod för nätverkssäkerhet i Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: 1500d25d9565d4082768933834303a17c5e84234
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-standalone-azure-automation-account"></a>Skapa ett fristående Azure Automation-konto
Det här avsnittet visar hur toocreate ett Automation-konto från hello Azure-portalen om du vill tooevaluate och Läs Azure Automation utan att inkludera hello ytterligare hanteringslösningarna eller integrering med OMS logganalys tooprovide avancerad övervakning av runbook-jobb.  Du kan lägga till dessa hanteringslösningar eller integrera med logganalys när som helst hello framtida.  Med hello Automation-konto är du kan tooauthenticate runbooks hantera resurser i Azure Resource Manager eller i Azure klassiska distributionen.

När du skapar ett Automation-konto i hello Azure-portalen, skapas automatiskt:

* Kör som-konto som skapar ett nytt huvudnamn för tjänsten i Azure Active Directory, ett certifikat och tilldelar hello deltagare rollbaserad åtkomstkontroll (RBAC) som används för toomanage Resource Manager-resurser med hjälp av runbooks.   
* Klassiska kör som-konto genom att ladda upp ett hanteringscertifikat som används för toomanage klassiska resurser med hjälp av runbooks.  

Detta förenklar hello du och hjälper dig att snabbt börja skapa och distribuera runbooks toosupport ditt automation måste ha.  

## <a name="permissions-required-toocreate-automation-account"></a>Behörigheter som krävs toocreate Automation-konto
toocreate eller uppdatera ett Automation-konto måste du ha följande specifika privilegier hello och behörigheter krävs toocomplete det här avsnittet.   
 
* I ordning toocreate ett Automation-konto, din AD-användarkontot måste toobe tillagda tooa roll med behörigheter motsvarande toohello ägarrollen för Microsoft.Automation resurser som beskrivs i artikel [rollbaserad åtkomstkontroll i Azure Automation ](automation-role-based-access-control.md).  
* Om hello App registreringar inställning har angetts för**Ja**, icke-administratörer i din Azure AD-klient kan [registrera AD-program](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions).  Om hello app registreringar inställning har angetts för**nr**, hello-användaren som utför den här åtgärden måste vara en global administratör i Azure AD. 

Om du inte är medlem i Active Directory-instans för hello prenumeration innan du läggs toohello global administratör/co-administrator roll hello prenumeration, läggs tooActive Directory som en gäst. I så fall kan få du en ”du har inte behörighet toocreate...” varning för hello **lägga till Automation-konto** bladet. Användare som har lagts till toohello global administratör/co-administrator rollen först kan tas bort från hello prenumeration Active Directory-instans och läggs till igen toomake dem en fullständig användare i Active Directory. tooverify i den här situationen från hello **Azure Active Directory** rutan i hello Azure portal, Välj **användare och grupper**väljer **alla användare** och, när du har valt hello specifik användare, markerar du **profil**. Hej värdet för hello **användartyp** attributet under hello användare profil ska inte vara lika med **gäst**.

## <a name="create-a-new-automation-account-from-hello-azure-portal"></a>Skapa ett nytt Automation-konto från hello Azure-portalen
I det här avsnittet, utföra hello följande steg toocreate ett Azure Automation-konto i hello Azure-portalen.    

1. Logga in toohello Azure-portalen med ett konto som är medlem i rollen för hello Prenumerationsadministratörer och medadministratör för hello prenumeration.
2. Klicka på **Ny**.<br><br> ![Välj alternativet Ny på Azure Portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. Sök efter **Automation** och sedan i hello sökresultat väljer **Automation- och kontrollservern***.<br><br> ![Sök efter och välj Automation från Marketplace](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)<br> 
3. I bladet för hello Automation-konton klickar du på **Lägg till**.<br><br>![Lägga till ett Automation-konto](media/automation-create-standalone-account/automation-create-automationacct-properties.png)
   
   > [!NOTE]
   > Om du ser följande varning i hello hello **lägga till Automation-konto** bladet beror det på ditt konto inte är medlem i rollen för hello Prenumerationsadministratörer och medadministratör för prenumerationen hello.<br><br>![Varningsmeddelande för Lägga till ett Automation-konto](media/automation-create-standalone-account/create-account-without-perms.png)
   > 
   > 
4. I hello **lägga till Automation-konto** bladet i hello **namnet** skriver ett namn på det nya Automation-kontot.
5. Om du har mer än en prenumeration kan du ange en hello nytt konto, en ny eller befintlig **resursgruppen** och ett Azure-datacenter **plats**.
6. Verifiera hello värdet **Ja** har valts för hello **skapa kör som-kontot Azure** alternativ och klickar på hello **skapa** knappen.  
   
   > [!NOTE]
   > Om du vill skapa toonot hello-kör som-konto genom att välja alternativet hello **nr**, visas ett varningsmeddelande i hello **lägga till Automation-konto** bladet.  Medan hello kontot skapas i hello Azure-portalen, saknar en motsvarande autentiseringsidentitet inom din klassiska eller Resource Manager prenumeration katalogtjänst och därför ingen åtkomst tooresources i din prenumeration.  Detta förhindrar alla runbooks som refererar till det här kontot från att kunna tooauthenticate och utföra åtgärder mot resurser i dessa distributionsmodeller.
   > 
   > ![Varningsmeddelande för Lägga till ett Automation-konto](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)<br>
   > Hello deltagarrollen tilldelas när hello tjänstens huvudnamn inte har skapats.
   > 

7. Medan Azure skapar hello Automation-konto, du kan följa förloppet för hello under **meddelanden** hello-menyn.

### <a name="resources-included"></a>Resurser som ingår
När hello Automation-konto har skapats, skapas flera resurser automatiskt åt dig.  hello följande tabell sammanfattas resurser för hello kör som-konto.<br>

| Resurs | Beskrivning |
| --- | --- |
| AzureAutomationTutorial-runbook |Ett exempel grafisk runbook som visar hur tooauthenticate med hello kör som-konto och hämtar alla hello Resource Manager-resurser. |
| AzureAutomationTutorialScript-runbook |Ett exempel PowerShell-runbook som visar hur tooauthenticate med hello kör som-konto och hämtar alla hello Resource Manager-resurser. |
| AzureRunAsCertificate |Certifikattillgång automatiskt skapas under skapandet av Automation-konto eller använda hello PowerShell-skriptet nedan för ett befintligt konto.  Det gör du tooauthenticate med Azure så att du kan hantera Azure Resource Manager-resurser från runbooks.  Det här certifikatet har en livslängd på ett år. |
| AzureRunAsConnection |Anslutningstillgång automatiskt skapas under skapandet av Automation-konto eller använda hello PowerShell-skriptet nedan för ett befintligt konto. |

hello följande tabell sammanfattas resurser för hello klassiska kör som-konto.<br>

| Resurs | Beskrivning |
| --- | --- |
| AzureClassicAutomationTutorial-runbook |Ett exempel grafisk runbook, som hämtar alla hello klassiska virtuella datorer i en prenumeration med hjälp av hello klassiska kör som-konto (certifikat) och sedan anger hello VM namn och status. |
| AzureClassicAutomationTutorial Script-runbook |Ett exempel PowerShell-runbook, som hämtar alla hello klassiska virtuella datorer i en prenumeration med hjälp av hello klassiska kör som-konto (certifikat) och sedan anger hello VM namn och status. |
| AzureClassicRunAsCertificate |Certifikattillgång skapas automatiskt som används tooauthenticate med Azure så att du kan hantera Azure klassiska resurser från runbooks.  Det här certifikatet har en livslängd på ett år. |
| AzureClassicRunAsConnection |Anslutningstillgång skapas automatiskt som används tooauthenticate med Azure så att du kan hantera Azure klassiska resurser från runbooks. |


## <a name="next-steps"></a>Nästa steg
* toolearn mer information om hur du grafiskt redigering finns [grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md).
* tooget igång med PowerShell-runbooks, se [min första PowerShell-runbook](automation-first-runbook-textual-powershell.md).
* tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md).
