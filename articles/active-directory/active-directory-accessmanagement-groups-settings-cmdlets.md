---
title: "aaaConfigure gruppinställningar med hjälp av Azure Active Directory-cmdletarna | Microsoft Docs"
description: "Hur hanterar hello inställningar för grupper med hjälp av Azure Active Directory-cmdlets"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 9f2090e6-3af4-4f07-bbb2-1d18dae89b73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro;
ms.openlocfilehash: 46db49d9dec3eaa41c97ca74c57854189eddc16d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory-cmdletar för att konfigurera gruppinställningar

> [!IMPORTANT]
> Det här innehållet gäller endast tooOffice 365 grupper. Mer information om hur tooallow användare toocreate säkerhetsgrupper, ställa in `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` enligt beskrivningen i [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0). 

Inställningar för Office 365-grupper har konfigurerats med ett inställningsobjekt och ett SettingsTemplate-objekt. Inledningsvis visas alla för inställningsobjekt i katalogen, eftersom katalogen är konfigurerad med hello standardinställningar. standardinställningarna för toochange hello, måste du skapa ett nytt inställningsobjekt med en mall för inställningar. Inställningar för mallar har definierats av Microsoft. Det finns flera olika inställningar mallar. tooconfigure Office 365 gruppinställningar för din katalog du använder hello mall med namnet ”Group.Unified”. inställningar för tooconfigure Office 365-grupper på en enda grupp Använd hello mall med namnet ”Group.Unified.Guest”. Den här mallen är används toomanage gäst åtkomstgruppen tooan Office 365. 

hello cmdlets är en del av hello Azure Active Directory PowerShell V2-modulen. Anvisningar hur toodownload och installera hello-modulen på din dator, se hello artikel [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/azuread/). Du kan installera hello version 2-versionen av hello modul från [hello PowerShell-galleriet](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="retrieve-a-specific-settings-value"></a>Hämta ett värde för specifika inställningar
Om du känner till hello namn hello inställning du vill tooretrieve kan du använda hello nedan cmdlet tooretrieve hello aktuella inställningar för värdet. I det här exemplet hämtar vi hello-värdet för en inställning med namnet ”UsageGuidelinesUrl”. Du kan läsa mer om inställningar och deras namn ytterligare ned i den här artikeln.

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-hello-directory-level"></a>Skapa inställningar på hello katalog-nivå
De här stegen skapa inställningar på katalognivå, som gäller tooall Office 365-grupper (Unified grupper) i hello directory.

1. I hello DirectorySettings cmdlets, måste du ange hello-ID för hello SettingsTemplate som du vill toouse. Om du inte vet detta ID returnerar denna cmdlet hello lista med alla inställningar-mallar:
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  Det här anropet cmdlet returnerar alla mallar som är tillgängliga:
  
  ```
  Id                                   DisplayName         Description
  --                                   -----------         -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Unified Group
  16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define hello different settings that can be used for hello associ...
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
  ```
2. tooadd riktlinje URL-Adressen för användning, måste du först tooget hello SettingsTemplate objekt som definierar hello användning riktlinje URL-värdet; det vill säga hello Group.Unified mallen:
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. Därefter skapa ett nytt för inställningsobjekt som baseras på mallen:
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. Uppdatera sedan hello användarvärde riktlinjer:
  
  ```
  $setting["UsageGuidelinesUrl"] = "<https://guideline.com>"
  ```  
5. Slutligen gäller hello inställningar:
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

Vid slutförande returnerar cmdlet hello hello-ID för hello nya inställningsobjektet:
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
Här följer hello inställningarna i hello Group.Unified SettingsTemplate.

| **Inställning** | **Beskrivning** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Typ: booleskt<li>Standard: True |hello flagga som indikerar om skapande av en enhetlig grupp tillåts i hello directory. |
|  <ul><li>GroupCreationAllowedGroupId<li>Typ: Sträng<li>Standard ”:” |GUID för hello säkerhetsgrupp för vilka hello medlemmar får toocreate Unified grupper även om EnableGroupCreation == false. |
|  <ul><li>UsageGuidelinesUrl<li>Typ: Sträng<li>Standard ”:” |Länk-toohello riktlinjer för användning av grupp. |
|  <ul><li>ClassificationDescriptions<li>Typ: Sträng<li>Standard ”:” | En kommaavgränsad lista över klassificering beskrivningar. |
|  <ul><li>DefaultClassification<li>Typ: Sträng<li>Standard ”:” | hello klassificering som är toobe som används som hello standardklassificering för en grupp om inget har angetts.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Typ: Sträng<li>Standard ”:” |Inte implementerat ännu
|  <ul><li>AllowGuestsToBeGroupOwner<li>Typ: booleskt<li>Standard: False | Booleskt värde som anger huruvida en gästanvändare kan vara en ägare av grupper. |
|  <ul><li>AllowGuestsToAccessGroups<li>Typ: booleskt<li>Standard: True | Booleskt värde som anger huruvida en gästanvändare kan ha åtkomst tooUnified grupper innehåll. |
|  <ul><li>GuestUsageGuidelinesUrl<li>Typ: Sträng<li>Standard ”:” | hello-url för riktlinjer för en länk toohello gäst-användning. |
|  <ul><li>AllowToAddGuests<li>Typ: booleskt<li>Standard: True | Ett booleskt värde som anger om eller inte är tillåtna tooadd gäster toothis directory.|
|  <ul><li>ClassificationList<li>Typ: Sträng<li>Standard ”:” |En kommaavgränsad lista över giltiga klassificeringsvärden som kan vara tillämpade tooUnified grupper. |
|  <ul><li>EnableGroupCreation<li>Typ: booleskt<li>Standard: True | Ett booleskt värde som anger om icke-administratörer kan skapa en ny enhetlig grupper. |


## <a name="read-settings-at-hello-directory-level"></a>Läsinställningar på hello katalog-nivå
De här stegen läsa inställningar på katalognivå, som gäller tooall Office-grupper i hello directory.

1. Läsa alla befintliga directory-inställningar:
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  Denna cmdlet returnerar en lista över alla inställningar:
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. Läsa alla inställningar för en viss grupp:
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. Läsa alla värden som directory inställningar i en specifik katalog settings-objekt med hjälp av inställningarna för Id-GUID:
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  Denna cmdlet returnerar hello namn och värden i det här inställningsobjektet för den här specifika gruppen:
  ```
  Name                          Value
  ----                          -----
  ClassificationDescriptions
  DefaultClassification
  PrefixSuffixNamingRequirement
  AllowGuestsToBeGroupOwner     False 
  AllowGuestsToAccessGroups     True
  GuestUsageGuidelinesUrl
  GroupCreationAllowedGroupId
  AllowToAddGuests              True
  UsageGuidelinesUrl            <https://guideline.com>
  ClassificationList
  EnableGroupCreation           True
  ```

## <a name="update-settings-for-a-specific-group"></a>Uppdatera inställningarna för en specifik grupp

1. Sök efter hello inställningar mall med namnet ”Groups.Unified.Guest”
  ```
  Get-AzureADDirectorySettingTemplate
  
  Id                                   DisplayName            Description
  --                                   -----------            -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Unified Group
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
  ```
2. Hämta hello mallobjekt hello Groups.Unified.Guest mallen:
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. Skapa ett nytt inställningsobjekt från hello mall:
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. Ange hello Inställningsvärdet toohello som krävs:
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. Skapa ny hello-inställning för hello nödvändiga gruppen i hello directory:
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-hello-directory-level"></a>Uppdatera inställningar på hello katalog-nivå

De här stegen uppdatera inställningar på katalognivå, som gäller tooall Unified grupper i hello directory. De här exemplen antar att det finns redan ett inställningsobjekt i din katalog.

1. Hitta hello befintligt inställningsobjekt:
  ```
  Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ
  
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  
  $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323
  ```
2. Uppdatera hello värdet:
  
  ```
  $Setting["AllowToAddGuests"] = "false"
  ```
3. Uppdatera hello inställning:
  
  ```
  Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
  ```

## <a name="remove-settings-at-hello-directory-level"></a>Ta bort inställningar på hello katalog-nivå
Det här steget tar bort inställningar på katalognivå, som gäller tooall Office-grupper i hello directory.
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a>Cmdlet-referens för syntax
Du kan hitta mer Azure Active Directory PowerShell-dokumentationen på [Azure Active Directory-cmdletarna](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="additional-reading"></a>Ytterligare resurser

* [Hantera åtkomst tooresources med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
