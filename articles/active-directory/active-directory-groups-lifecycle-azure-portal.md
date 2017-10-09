---
title: "aaaPreview Office 365-grupper upphör att gälla i Azure Active Directory | Microsoft Docs"
description: "Hur tooset in förfallodatum för Office 365 grupper i Azure Active Directory (förhandsgranskning)"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: a8c99961cff3aad3f4d8b0cc1b2eb8e8a4c9ba95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-office-365-groups-expiration-preview"></a>Konfigurera förfallodatum för Office 365-grupper (förhandsgranskning)

Du kan nu hantera hello livscykeln för Office 365-grupper genom att ange upphör att gälla för alla Office 365-grupper som du väljer. När den här giltighetstid har angetts är ägare till dessa grupper och toorenew deras grupper om de fortfarande behöver hello grupper. En Office 365-grupp som inte förnyas tas bort. En Office 365-grupp som har tagits bort kan återställas inom 30 dagar av hello gruppen ägare eller administratör hello.  


> [!NOTE]
> Du kan ställa in ett utgångsdatum för Office 365-grupper.
>
> Ange förfallodatum för O365-grupper måste du ange att en Azure AD Premium-licens har tilldelats
>   - Hej administratör som konfigurerar inställningar för hello giltighetstid för hello-klient
>   - Alla medlemmar i hello grupper som du valt för den här inställningen

## <a name="set-office-365-groups-expiration"></a>Ställa in ett utgångsdatum för Office 365-grupper

1. Öppna hello [administrationscentret för Azure AD](https://aad.portal.azure.com) med ett konto som är en global administratör i Azure AD-klienten.

2. Öppna Azure AD, Välj **användare och grupper**.

3. Välj **gruppinställningar** och välj sedan **giltighetstid** inställningar för tooopen hello giltighetstid.
  
  ![Bladet upphör att gälla](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. På hello **giltighetstid** bladet kan du:

  * Ange hello grupp livslängd i dagar. Du kan välja något av hello förinställda värden eller ett anpassat värde (ska vara 31 dagar eller mer). 
  * Ange en e-postadress där hello förnyelse och förfallodatum meddelanden ska skickas när en grupp har ingen ägare. 
  * Välj vilka Office 365-grupper att gälla. Du kan aktivera förfallodatum för **alla** Office 365-grupper som du kan välja bland hello Office 365-grupper eller du väljer **ingen** att inaktivera upphör att gälla för alla grupper.
  * Spara dina inställningar när du är klar genom att välja **spara**.

Anvisningar för hur toodownload och installera hello Microsoft PowerShell-modulen tooconfigure giltighetstid för Office 365-grupper via PowerShell finns [Azure Active Directory V2 PowerShell-modul - offentliga förhandsversionen 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

E-postmeddelanden som den här skickas toohello Office 365 gruppen ägare 30 dagar, 15 dagar och 1 dag tidigare tooexpiration hello-gruppen.

![Förfallodatum för e-postavisering](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

hello gruppägare kan sedan välja **förnya grupp** toorenew hello Office 365-grupp eller kan du välja **gå toogroup** toosee hello medlemmar och annan information om hello grupp.

När en grupp upphör att gälla hello gruppen har tagits bort en dag efter hello upphör att gälla. Ett e-postmeddelande som den här skickas toohello Office 365 gruppen ägare informerar dem om hello förfallodatum och efterföljande borttagning av deras Office 365-grupp.

![Gruppen borttagning e-postavisering](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

hello gruppen kan återställas genom att välja **återställning grupp** eller med hjälp av PowerShell-cmdlets som beskrivs i [återställa en borttagen Office 365-grupp i Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/ Active-Directory-groups-Restore-Azure-Portal).
    
Om hello-grupp som du återställa innehåller dokument, SharePoint-webbplatser eller andra beständiga objekt, kan det ta upp too24 timmar toofully återställning hello grupp och dess innehåll.

> [!NOTE]
> * När du distribuerar hello inställningar för giltighetstid kan uppstå vissa grupper som är äldre än hello giltighetstid fönster. Dessa grupper är inte omedelbart bort, men är ställa in too30 dagar tills upphör att gälla. hello första förnyelse e-postmeddelande skickas ut inom en dag. Till exempel grupp A skapades 400 dagar sedan och hello giltighetstid intervallet anges too180 dagar. När du använder inställningar för giltighetstid har grupp A 30 dagar innan den tas bort, såvida inte hello ägare förnyar den.
> * När en dynamisk grupp tas bort och återställs, visas den som en ny grupp och nytt ifyllda bl.a toohello regeln. Den här processen kan ta upp too24 timmar.

## <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure AD-grupper.

* [Se befintliga grupper](active-directory-groups-view-azure-portal.md)
* [Hantera inställningar för en grupp](active-directory-groups-settings-azure-portal.md)
* [Hantera medlemmar i en grupp](active-directory-groups-members-azure-portal.md)
* [Hantera medlemskap i en grupp](active-directory-groups-membership-azure-portal.md)
* [Hantera dynamiska regler för användare i en grupp](active-directory-groups-dynamic-membership-azure-portal.md)
