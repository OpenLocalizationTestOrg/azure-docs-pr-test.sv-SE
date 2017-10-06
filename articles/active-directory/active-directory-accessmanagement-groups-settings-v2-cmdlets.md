---
title: "aaaPowerShell exempel för att hantera grupper i Azure Active Directory | Microsoft Docs"
description: "Den här sidan innehåller PowerShell exempel toohelp du hantera dina grupper i Azure Active Directory"
keywords: Azure AD, Azure Active Directory PowerShell grupper, grupphantering
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7a5023dc-2727-4c25-8254-b531fc3244ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: rodejo
ms.openlocfilehash: ba049babc436e99a290f20899b3a87bcfa811d9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>Azure Active Directory version 2-cmdlets för grupphantering
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Den här artikeln innehåller exempel på hur toouse PowerShell toomanage grupper i Azure Active Directory (AD Azure).  Dessutom talar du hur tooget som konfigurerats med hello Azure AD PowerShell-modulen. Först måste du [hämta hello Azure AD PowerShell-modulen](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="installing-hello-azure-ad-powershell-module"></a>Installera hello Azure AD PowerShell-modul
tooinstall hello Azure AD PowerShell-modulen använder hello följande kommandon:

    PS C:\Windows\system32> install-module azuread

tooverify som hello modulen installerades, använder hello följande kommando:

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

Nu kan du börja använda hello-cmdlets i hello modul. En fullständig beskrivning av hello-cmdletar i hello Azure AD-modulen finns toohello online referensdokumentationen för [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="connecting-toohello-directory"></a>Ansluta toohello directory
Innan du kan börja hantera grupper med hjälp av Azure AD PowerShell-cmdlets, måste du ansluta din PowerShell-session toohello katalog som du vill toomanage. Använd hello följande kommando:

    PS C:\Windows\system32> Connect-AzureAD

hello cmdlet uppmanar dig för hello autentiseringsuppgifter du vill använda toouse tooaccess din katalog. I det här exemplet använder vi karen@drumkit.onmicrosoft.com tooaccess hello demonstration directory. hello cmdlet returnerar en bekräftelse tooshow hello session ansluten tooyour directory:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Nu kan du börja använda hello AzureAD cmdlets toomanage grupper i katalogen.

## <a name="retrieving-groups"></a>Hämtning av grupper
tooretrieve befintliga grupper från din katalog som du kan använda hello Get-AzureADGroups cmdlet. tooretrieve alla grupper i hello directory, Använd hello cmdlet utan parametrar:

    PS C:\Windows\system32> get-azureadgroup

hello-cmdlet returnerar alla grupper i hello ansluten katalog.

Du kan använda hello - objectID parametern tooretrieve en specifik grupp som du anger hello grupp objekt-ID:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

hello-cmdlet returnerar nu hello grupp vars objectID matchar hello värdet för hello-parametern som du angav:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Du kan söka efter en specifik grupp med hjälp av hello - Filterparametern. Den här parametern tar ett filter ODATA-satsen och returnerar alla grupper som matchar hello filter, som i följande exempel hello:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

> [!NOTE] 
> Hej AzureAD PowerShell-cmdlets implementera hello OData-frågan som standard. Mer information finns i **$filter** i [frågealternativ för OData-system med hello OData-slutpunkt](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Skapa grupper
toocreate en ny grupp i din katalog, Använd hello ny AzureADGroup cmdlet. Denna cmdlet skapar en ny säkerhetsgrupp som heter ”Marknadsföring”:

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Uppdatering av grupper
tooupdate en befintlig grupp, Använd hello Set-AzureADGroup cmdlet. I det här exemplet ändrar vi hello DisplayName-egenskap i hello gruppen ”Intune-administratörer”. Först vi söka efter hello gruppen med hello Get-AzureADGroup cmdlet och filter som använder hello DisplayName attributet:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Nu ska ändrar vi hello beskrivning toohello nya egenskapsvärdet ”Intune Enhetsadministratörer”:

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Nu om vi upptäcker hello grupp kan vi se hello beskrivningsegenskapen är uppdaterade tooreflect hello nytt värde:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Ta bort grupper
toodelete grupper från din katalog använder hello Remove-AzureADGroup cmdlet enligt följande:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Hantera medlemmar i grupper
Om du behöver tooadd nya medlemmar tooa grupp använda hello Lägg till AzureADGroupMember cmdlet. Detta kommando lägger till en medlem toohello Intune administratörsgruppen vi använde i hello föregående exempel:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

hello - parametern objektId är hello ObjectID hello grupp toowhich vi vill tooadd medlem och hello - RefObjectId är hello ObjectID för hello användare vi vill tooadd som en medlem toohello grupp.

tooget hello befintliga medlemmar i en grupp, använda hello Get-AzureADGroupMember cmdlet, som i följande exempel:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

tooremove hello medlem vi tidigare lagt till toohello grupp, Använd hello Remove-AzureADGroupMember cmdlet, som visas här:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

tooverify hello grupp membership(s) för en användare, Använd hello väljer AzureADGroupIdsUserIsMemberOf cmdlet. Den här cmdleten tar eftersom dess parametrar hello ObjectId för hello användare för vilka toocheck hello gruppmedlemskap, och en lista över grupper för vilka toocheck hello medlemskap. hello listan över grupper måste anges i hello form av en komplex variabel av typen ”Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck”, så vi först skapa en variabel med den typen:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Nu ska ange vi värden för hello groupIds toocheck i hello attributet ”GroupIds” för den här variabeln för komplex:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Nu, om vi vill toocheck hello gruppmedlemskap för en användare med ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea mot hello grupper i $g ska användas:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


hello-värde som returneras är en lista över grupper som användaren är medlem. Du kan också använda den här metoden toocheck kontakter, grupper eller tjänstens huvudnamn medlemskap för en angiven lista med grupper, med hjälp av Select-AzureADGroupIdsContactIsMemberOf, Välj AzureADGroupIdsGroupIsMemberOf eller Välj AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="managing-owners-of-groups"></a>Hantera ägare av grupper
tooadd ägare tooa grupp, Använd hello Lägg till AzureADGroupOwner cmdlet:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

hello - parametern objektId är hello ObjectID hello grupp toowhich vi vill tooadd en ägare och hello - RefObjectId är hello ObjectID för hello användare vi vill tooadd som ägare av hello grupp.

tooretrieve hello ägare för en grupp använder hello Get-AzureADGroupOwner cmdlet:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

hello-cmdlet returnerar hello listan över ägare för hello angiven grupp:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Om du vill tooremove ägare från en grupp, använder du hello Remove-AzureADGroupOwner cmdlet:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a>Reserverade alias 
När en grupp har skapats, kan vissa slutpunkter hello slutanvändaren toospecify en mailNickname eller alias toobe som används som en del av hello e-postadress hello grupp. Grupper med hello följande mycket Privilegierade e-alias kan bara skapas av en global administratör för Azure AD. 
  
* missbruk 
* Admin 
* Administratören 
* hostmaster 
* majordomo 
* postmaster 
* rot 
* Skydda 
* Säkerhet 
* SSL-admin 
* webbadministratör 

## <a name="next-steps"></a>Nästa steg
Du kan hitta mer Azure Active Directory PowerShell-dokumentationen på [Azure Active Directory-cmdletarna](/powershell/azure/install-adv2?view=azureadps-2.0).

* [Hantera åtkomst tooresources med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
