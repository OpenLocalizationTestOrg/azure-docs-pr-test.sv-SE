---
title: "aaaRole-baserad åtkomstkontroll i Azure Automation | Microsoft Docs"
description: "Med rollbaserad åtkomstkontroll (RBAC) kan du hantera åtkomsten till Azure-resurser. Den här artikeln beskriver hur tooset in RBAC i Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: "automation rbac, rollbaserad åtkomstkontroll, azure rbac"
ms.assetid: 04b5625e-0ee8-4b5b-85cd-7734c1b3d4a3
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte;sngun
ms.openlocfilehash: 051438e44d0c5c514d6dbaac5a312344ee311cdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-in-azure-automation"></a>Rollbaserad åtkomstkontroll i Azure Automation
## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll
Med rollbaserad åtkomstkontroll (RBAC) kan du hantera åtkomsten till Azure-resurser. Med hjälp av [RBAC](../active-directory/role-based-access-control-configure.md), du kan särskilja uppgifter i din grupp och ge endast hello mängden åtkomst toousers, grupper och program som de behöver tooperform sitt arbete. Rollbaserad åtkomst kan beviljas toousers med hello Azure-portalen, Azure kommandoradsverktyg eller Azure Management-API: er.

## <a name="rbac-in-automation-accounts"></a>RBAC i Automation-konton
I Azure Automation beviljas åtkomst genom att tilldela hello lämpliga RBAC rollen toousers, grupper och program på hello omfånget för Automation-konto. Följande är hello inbyggda roller som stöds av ett Automation-konto:  

| **Roll** | **Beskrivning** |
|:--- |:--- |
| Ägare |hello ägarrollen kan komma åt tooall resurser och åtgärder i ett Automation-konto inklusive tillhandahåller åtkomst tooother användare, grupper och program toomanage hello Automation-konto. |
| Deltagare |hello deltagarrollen kan du toomanage allt utom ändra andra användare åtkomst till behörigheter tooan Automation-konto. |
| Läsare |hello Reader rollen kan tooview alla hello resurser i ett Automation-konto men inte göra några ändringar. |
| Automation-operatör |hello Automation operatörsrollen kan du tooperform åtgärder, till exempel starta, stoppa, pausa, återuppta och schemalägga. Den här rollen är användbart om du vill tooprotect resurserna Automation-konto som autentiseringsuppgifter tillgångar och runbooks från att visas eller ändras men fortfarande dessa runbooks tillåter medlemmar i din organisation tooexecute. |
| Administratör för användaråtkomst |Hej administratör för användaråtkomst rollen kan toomanage åtkomst tooAzure Automation användarkonton. |

> [!NOTE]
> Du kan inte bevilja åtkomst rättigheter tooa specifika eller de runbooks, endast toohello resurser och åtgärder i hello Automation-konto.  
> 
> 

I den här artikeln går du igenom hur tooset in RBAC i Azure Automation. Men först ska vi ta en närmare titta på hello enskilda behörigheter beviljade toohello deltagare, Reader, Automation operatorn och administratör för användaråtkomst så att vi får en god förståelse innan du beviljar alla rights toohello Automation-konto.  Annars kan beviljandet
resultera i oväntade eller oönskade konsekvenser.     

## <a name="contributor-role-permissions"></a>Deltagarbehörigheter
hello visar följande tabell hello specifika åtgärder som kan utföras av hello deltagarrollen i Automation.

| **Resurstyp** | **Läsa** | **Skriva** | **Ta bort** | **Andra åtgärder** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Automation – konto |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation – certifikattillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation – anslutningstillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation – anslutningstypstillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation – autentiseringstillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation – schematillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation – variabeltillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation – önskad tillståndskonfiguration | | | |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |
| Hybrid Runbook Worker – resurstyp |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |
| Azure Automation – jobb |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |
| Automation – jobbström |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – jobbschema |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation – modul |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |
| Azure Automation – Runbook |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |
| Automation – Runbook-utkast |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |
| Automation – testjobb för Runbook-utkast |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |
| Automation – webhook |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |

## <a name="reader-role-permissions"></a>Behörighet för läsare
hello visar följande tabell hello specifika åtgärder som kan utföras av rollen för hello läsare i Automation.

| **Resurstyp** | **Läsa** | **Skriva** | **Ta bort** | **Andra åtgärder** |
|:--- |:--- |:--- |:--- |:--- |
| Klassisk prenumerationsadministratör |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Hanteringslås |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Behörighet |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Leverantörsåtgärder |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Rolltilldelning |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Rolldefinition |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="automation-operator-role-permissions"></a>Behörigheter för Automation-operatör
hello visar följande tabell hello specifika åtgärder som kan utföras av hello Automation operatörsrollen i Automation.

| **Resurstyp** | **Läsa** | **Skriva** | **Ta bort** | **Andra åtgärder** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Automation – konto |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – certifikattillgång | | | | |
| Automation – anslutningstillgång | | | | |
| Automation – anslutningstypstillgång | | | | |
| Automation – autentiseringstillgång | | | | |
| Automation – schematillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | |
| Automation – variabeltillgång | | | | |
| Automation – önskad tillståndskonfiguration | | | | |
| Hybrid Runbook Worker – resurstyp | | | | |
| Azure Automation – jobb |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |
| Automation – jobbström |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – jobbschema |![Grön status](media/automation-role-based-access-control/green-checkmark.png) |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | |
| Automation – modul | | | | |
| Azure Automation – Runbook |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – Runbook-utkast | | | | |
| Automation – testjobb för Runbook-utkast | | | | |
| Automation – webhook | | | | |

Mer information hello [Automation operatorn åtgärder](../active-directory/role-based-access-built-in-roles.md#automation-operator) visar hello åtgärder som stöds av hello Automation operatörsrollen på hello Automation-kontot och dess resurser.

## <a name="user-access-administrator-role-permissions"></a>Behörigheter för Administratör för användaråtkomst
hello visar följande tabell hello specifika åtgärder som kan utföras av hello åtkomst Användaradministratör i Automation.

| **Resurstyp** | **Läsa** | **Skriva** | **Ta bort** | **Andra åtgärder** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Automation – konto |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – certifikattillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – anslutningstillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – anslutningstypstillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – autentiseringstillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – schematillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – variabeltillgång |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – önskad tillståndskonfiguration | | | | |
| Hybrid Runbook Worker – resurstyp |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Azure Automation – jobb |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – jobbström |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – jobbschema |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – modul |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Azure Automation – Runbook |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – Runbook-utkast |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – testjobb för Runbook-utkast |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation – webhook |![Grön status](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="configure-rbac-for-your-automation-account-using-azure-portal"></a>Konfigurera RBAC för ditt Automation-konto med hjälp av Azure-portalen
1. Logga in toohello [Azure Portal](https://portal.azure.com/) och öppna ditt Automation-konto från hello Automation-konton bladet.  
2. Klicka på hello **åtkomst** kontroll på hello övre högra hörnet. Då öppnas hello **användare** bladet där du kan lägga till nya användare, grupper och program toomanage ditt Automation-konto och visa befintliga rollerna kan konfigureras för hello Automation-konto.  
   
   ![Knappen Åtkomst](media/automation-role-based-access-control/automation-01-access-button.png)  

> [!NOTE]
> **Prenumerationsadministratörer** finns redan som hello standardanvändare. hello prenumeration administratörer active directory-grupp innehåller hello service administratörer och co-administrator(s) för din Azure-prenumeration. hello tjänstadministratör är hello ägare för din Azure-prenumeration och dess resurser och ska ha hello ägarrollen ärvt för hello automation-konton för. Det innebär att hello åtkomst **ärvda** för **service administratörer och medadministratörer** för en prenumeration och dess **tilldelad** för alla hello andra användare. Klicka på **prenumerationsadministratörer** tooview mer information om deras behörigheter.  
> 
> 

### <a name="add-a-new-user-and-assign-a-role"></a>Lägga till en ny användare och tilldela en roll
1. I bladet för hello användare klickar du på **Lägg till** tooopen hello **Lägg till åtkomst bladet** där du kan lägga till en användare, grupp eller ett program och tilldela en roll toothem.  
   
   ![Lägga till användare](media/automation-role-based-access-control/automation-02-add-user.png)  
2. Välj en roll hello listan över tillgängliga roller. Vi väljer hello **Reader** rollen, men du kan välja någon av hello tillgängliga inbyggda roller som har stöd för ett Automation-konto eller en anpassad roll som du har definierat.  
   
   ![Välja en roll](media/automation-role-based-access-control/automation-03-select-role.png)  
3. Klicka på **lägga till användare** tooopen hello **lägga till användare** bladet. Om du har lagt till alla användare, grupper eller program toomanage din prenumeration och användarna visas och välj tooadd åtkomst. Om det inte finns några användare i listan eller om hello-användare som du vill lägga till inte visas klickar du **bjuda in** tooopen hello **bjuda in gäst** bladet, där du kan bjuda in användare med ett giltigt microsoftkonto e-postadress som Outlook.com, OneDrive eller Xbox Live-ID: N. När du har angett hello hello användarens e-postadress, klickar du på **Välj** tooadd hello användaren och klicka sedan på **OK**. 
   
   ![Lägga till användare](media/automation-role-based-access-control/automation-04-add-users.png)  
   
   Nu bör du se hello användare som läggs till toohello **användare** bladet med hello **Reader** tilldelats rollen.  
   
   ![Visa användare](media/automation-role-based-access-control/automation-05-list-users.png)  
   
   Du kan också tilldela en användare med rollen toohello från hello **roller** bladet. 
4. Klicka på **roller** från hello användare bladet tooopen hello **roller bladet**. Du kan visa hello namnet på hello roll, hello antalet användare och grupper som tilldelas rollen toothat från det här bladet.
   
    ![Tilldela en roll från bladet Användare](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
   > [!NOTE]
   > Rollbaserad åtkomstkontroll kan bara anges på hello Automation kontonivå och inte på någon resurs nedan hello Automation-konto.
   > 
   > 
   
    Du kan tilldela fler än en roll tooa användare, grupp eller programmet. Om vi lägga till hello exempelvis **Automation operatorn** roll samt hello **rollen Läsare** toohello användaren och de kan visa alla hello Automation resurser, samt utföra hello runbook-jobb. Du kan expandera hello listrutan tooview en lista över roller toohello användare.  
   
    ![Visa flera roller](media/automation-role-based-access-control/automation-07-view-multiple-roles.png)  

### <a name="remove-a-user"></a>Ta bort en användare
Du kan ta bort hello åtkomstbehörighet för en användare som inte hanterar hello Automation-konto eller som inte längre fungerar för hello organisation. Följande är hello steg tooremove en användare: 

1. Från hello **användare** bladet, Välj hello rolltilldelning tooremove gärna.
2. Klicka på hello **ta bort** -knappen i hello tilldelning informationsbladet.
3. Klicka på **Ja** tooconfirm borttagning. 
   
   ![Ta bort användare](media/automation-role-based-access-control/automation-08-remove-users.png)  

## <a name="role-assigned-user"></a>Roll tilldelad till användare
När en tilldelad tooa användarroll loggar in tootheir Automation-konto, kan de nu se hello ägarens konto som angetts i hello lista över **standard kataloger**. De måste växla hello standard directory toohello ägarens standardkatalogen i ordning tooview hello Automation-konto som de har lagts till.  

![Standardkatalog](media/automation-role-based-access-control/automation-09-default-directory-in-role-assigned-user.png)  

### <a name="user-experience-for-automation-operator-role"></a>Användarupplevelsen för Automation-operatörsrollen
När en användare som är tilldelade toohello Automation operatorn rollen vyer hello Automation-konto som har tilldelats, de kan bara visa hello lista med runbooks, runbook-jobb och scheman har skapat i hello Automation-konto men det går inte att visa deras definition. De kan starta, stoppa, pausa, återuppta eller schemalägga hello runbook-jobbet. hello användaren inte komma åt tooother Automation resurser, till exempel konfigurationer, hybrid worker-grupper eller DSC-noder.  

![Ingen åtkomst tooresourcres](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

När hello användare klickar på hello runbook, tillhandahålls hello kommandon tooview hello källa eller redigera hello runbook inte som hello Automation operatörsrollen inte tillåter åtkomst toothem.  

![Ingen åtkomst tooedit runbook](media/automation-role-based-access-control/automation-11-no-access-to-edit-runbook.png)  

hello användare har åtkomst till tooview och toocreate scheman, men har inte åtkomst tooany andra tillgångstyp.  

![Ingen åtkomst tooassets](media/automation-role-based-access-control/automation-12-no-access-to-assets.png)  

Den här användaren har inte också åtkomst tooview hello webhooks som är kopplad till en runbook

![Ingen åtkomst toowebhooks](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## <a name="configure-rbac-for-your-automation-account-using-azure-powershell"></a>Konfigurera RBAC för ditt Automation-konto med hjälp av Azure PowerShell
Rollbaserad åtkomst kan också vara konfigurerade tooan Automation-konto med hjälp av följande hello [Azure PowerShell-cmdlets](../active-directory/role-based-access-control-manage-access-powershell.md).

• [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) visar alla RBAC-roller som är tillgängliga i Azure Active Directory. Du kan använda det här kommandot tillsammans med hello **namn** egenskapen toolist alla hello åtgärder som kan utföras av en viss roll.  
    **Exempel:**  
    ![Hämta rolldefinition](media/automation-role-based-access-control/automation-14-get-azurerm-role-definition.png)  

• [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) listor Azure AD RBAC rolltilldelningar på hello angivna omfattningen. Detta kommando returnerar alla hello rolltilldelningar som gjorts under hello prenumeration utan några parametrar. Använd hello **ExpandPrincipalGroups** parametern toolist åtkomst tilldelningar för hello specificerade användare samt hello grupper hello användaren är medlem i.  
    **Exempel:** Använd hello följande kommando toolist alla hello-användare och roller i ett automation-konto.

    Get-AzureRMRoleAssignment -scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>” 

![Hämta rolltilldelning](media/automation-role-based-access-control/automation-15-get-azurerm-role-assignment.png)

• [Ny AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) tooassign toousers, grupper och program tooa viss åtkomstomfånget.  
    **Exempel:** Använd hello följande kommando tooassign hello ”Automation-operatör” roll för en användare i hello omfånget för Automation-konto.

    New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish toogrant access> -RoleDefinitionName "Automation operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”  

![Ny rolltilldelning](media/automation-role-based-access-control/automation-16-new-azurerm-role-assignment.png)

• Använd [ta bort AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) tooremove åtkomst för en angiven användare, grupp eller ett program från ett visst omfång.  
    **Exempel:** Använd hello följande kommando tooremove hello användaren från hello ”Automation-operatör” roll i hello omfånget för Automation-konto.

    Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish tooremove> -RoleDefinitionName "Automation Operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”

I hello exemplen ovan ersätter **inloggning Id**, **prenumerations-Id**, **resursgruppens namn** och **Automation kontonamn** med din kontoinformation. Välj **Ja** när uppmanas tooconfirm innan du fortsätter tooremove rolltilldelningen för användaren.   

## <a name="next-steps"></a>Nästa steg
* Information om olika sätt tooconfigure RBAC för Azure Automation finns för[hantera RBAC med Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md).
* Mer information om olika sätt toostart en runbook finns [starta en runbook](automation-starting-a-runbook.md)
* Information om olika typer av runbookflöden finns för[typer för Azure Automation-runbook](automation-runbook-types.md)

