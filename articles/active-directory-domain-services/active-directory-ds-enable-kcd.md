---
title: "Azure Active Directory Domain Services: Aktivera kerberos-begränsad delegering | Microsoft Docs"
description: "Aktivera kerberos-begränsad delegering på Azure Active Directory Domain Services hanterade domäner"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: d32547c8018dd1f99c992e95a01d2711d460a261
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Konfigurera kerberos-begränsad delegering (KCD) på en hanterad domän
Många program måste tooaccess resurser i hello hello användarens kontext. Active Directory stöder en mekanism som kallas Kerberos-delegering, vilket gör den här användningsfall. Dessutom kan du begränsa delegering så att bara vissa resurser kan användas i hello hello användarens kontext. Azure AD Domain Services-hanterade domäner skiljer sig från traditionella Active Directory-domäner eftersom de säkrare är låsta.

Den här artikeln visar hur tooconfigure kerberos-begränsad delegering på en hanterad Azure AD DS-domän.

## <a name="kerberos-constrained-delegation-kcd"></a>Kerberos-begränsad delegering (KCD)
Kerberos-delegering kan ett konto tooimpersonate en annan säkerhetsresurser UPN (till exempel en användare) tooaccess. Överväg att ett webbprogram som har åtkomst till en backend-webb-API i hello kontexten för en användare. I det här exemplet personifierar hello webbprogram (som körs i hello ett tjänstkonto eller ett dator-/ datorkonto) hello användare få åtkomst till resursen hello (backend-webb-API). Kerberos-delegering är osäker eftersom den inte begränsar hello resurser hello personifierar kontot kan komma åt i hello hello användarens kontext.

Kerberos-begränsad delegering (KCD) begränsar hello resurser och tjänster toowhich hello server kan utföra på en användares hello vägnar. Traditionell KCD kräver domän administratör privilegier tooconfigure ett domänkonto för en tjänst och det begränsar hello tooa enda kontodomän.

Traditionell KCD har även några problem som är kopplade till den. I tidigare operativsystem där hello domänadministratören konfigurerade tjänsten hello hade hello tjänsteadministratören inget användbart sätt tooknow vilka frontend-tjänster delegerad toohello resurstjänsterna de ägde. Och alla frontend-tjänster som kunde delegera tooa resurstjänst representerade en potentiell angreppspunkt. Om en server som är värd för en frontend-tjänst har drabbats och var konfigurerade toodelegate tooresource services, kan hello resurstjänsterna också komprometteras.

> [!NOTE]
> På en hanterad domän i Azure AD Domain Services har inte administratörsbehörighet för domänen. Därför **traditionella KCD kan inte konfigureras på en hanterad domän**. Använd resursbaserade KCD som beskrivs i den här artikeln. Den här mekanismen är också säkrare.
>
>

## <a name="resource-based-kerberos-constrained-delegation"></a>Resursbaserade kerberos-begränsad delegering
I Windows Server 2012 R2 och Windows Server 2012 överförts hello möjlighet tooconfigure begränsad delegering för hello tjänsten från Hej administratör toohello service domänadministratör. På så sätt kan kan hello backend-tjänst-administratören tillåta eller neka frontend-tjänster. Den här modellen kallas **resursbaserade kerberos-begränsad delegering**.

Resursbaserad KCD har konfigurerats med hjälp av PowerShell. Du använder hello Set-ADComputer eller Set-ADUser cmdlet: ar, beroende på om hello personifierar kontot är ett datorkonto eller ett användarkonto för konto-tjänsten.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>Konfigurera resursbaserade KCD för ett datorkonto på en hanterad domän
Anta att du har ett webbprogram som körs på datorn hello ”contoso100-webapp.contoso100.com'. Det behöver tooaccess hello resurs (en webb-API som körs på ”contoso100-api.contoso100.com') hello kontexten för domänanvändare. Här är hur du konfigurerar resursbaserade KCD för det här scenariot.

```
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>Konfigurera resursbaserade KCD för ett användarkonto på en hanterad domän
Anta att du har ett webbprogram som körs som ett tjänstkonto 'appsvc' och måste därför tooaccess hello resurs (en web API som körs som ett tjänstkonto - 'backendsvc') hello kontexten för domänanvändare. Här är hur du konfigurerar resursbaserade KCD för det här scenariot.

```
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Kerberos-begränsad delegering: översikt](https://technet.microsoft.com/library/jj553400.aspx)
