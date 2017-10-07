---
title: aaaRestore en borttagen Office 365-grupp i Azure Active Directory | Microsoft Docs
description: "Hur toorestore en borttagen grupp, visa återställningsbara grupper och permamnently tar bort en grupp i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: curtand
ms.openlocfilehash: 4e6650d64aa19f0c909f1c511f48681de8032f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Återställa en borttagen Office 365-grupp i Azure Active Directory

När du tar bort en Office 365-grupp i hello Azure Active Directory (AD Azure) hello bort grupp är kvarhållna men är inte synliga i 30 dagar från hello borttagning datum. Detta är så att hello grupp och dess innehåll kan återställas om det behövs. Den här funktionen är begränsat enbart tooOffice 365 grupper i Azure AD. Det är inte tillgänglig för säkerhetsgrupper och distributionsgrupper.

> [!NOTE] 
> Använd inte `Remove-MsolGroup` eftersom det tar bort hello grupp permanent. Använd alltid `Remove-AzureADMSGroup` toodelete en O365-gruppen. 

hello behörigheter krävs toorestore en grupp kan vara något av följande hello:

Roll  | Behörigheter 
--------- | ---------
Företagsadministratör, Partner Tier2 stöd och InTune-tjänsten-administratörer | Återställa en borttagen grupp för Office 365 
Stöd för användaren kontoadministratör och Partner Tier1 | Återställa alla borttagna Office 365-gruppen utom de tilldelade toohello företagsadministratör roll 
Användare | Återställa en borttagen Office 365-grupp som de ägs 


## <a name="view-hello-deleted-office-365-groups-that-are-available-toorestore"></a>Visa hello bort Office 365-grupper som är tillgängliga toorestore
hello följande cmdlets kan vara används tooview hello bort grupper tooverify som hello något eller de som du är intresserad har inte ännu har permanent bort. Dessa cmdletar är en del av hello [Azure AD PowerShell-modulen](https://www.powershellgallery.com/packages/AzureAD/). Mer information om den här modulen kan hittas i hello [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0) artikel.

1.  Kör följande cmdlet toodisplay alla bort Office 365-grupper i din klient som är fortfarande tillgängliga toorestore hello.
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  Alternativt, om du vet hello objectID för en specifik grupp (och du får från hello cmdlet i steg 1) kör hello följande cmdlet tooverify som hello specifika borttagna gruppen har inte ännu permanent rensats.
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-toorestore-your-deleted-office-365-group"></a>Hur toorestore din borttagna Office 365-grupp
När du har kontrollerat att hello är fortfarande tillgängliga toorestore, återställa hello bort grupp med något av följande hello. Om hello grupp innehåller dokument, SP platser eller andra beständiga objekt, kan det ta upp too24 timmar toofully Återställ en grupp och dess innehåll.

1.  Kör hello följande cmdlet toorestore hello grupp och dess innehåll.
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

Du kan också kan hello följande cmdlet köras toopermanently ta bort hello bort grupp.
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a>Hur vet du detta arbetat?
tooverify som du har har återställt en Office 365-grupp, kör hello `Get-AzureADGroup –ObjectId <objectId>` cmdlet toodisplay information om hello-gruppen. Begäran har slutförts efter hello återställning:
- hello gruppen visas i hello vänstra navigeringsfältet i Exchange
- hello plan för hello gruppen visas i Planner
- Sharepoint-webbplatser och allt innehåll ska vara tillgängliga
- hello gruppen kan nås från alla hello Exchange slutpunkter och andra Office 365-arbetsbelastningar som har stöd för Office 365-grupper


## <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure Active Directory-grupper.

* [Se befintliga grupper](active-directory-groups-view-azure-portal.md)
* [Hantera inställningar för en grupp](active-directory-groups-settings-azure-portal.md)
* [Hantera medlemmar i en grupp](active-directory-groups-members-azure-portal.md)
* [Hantera medlemskap i en grupp](active-directory-groups-membership-azure-portal.md)
* [Hantera dynamiska regler för användare i en grupp](active-directory-groups-dynamic-membership-azure-portal.md)
