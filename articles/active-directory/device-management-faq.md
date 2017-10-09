---
title: "aaaAzure Active Directory-enhetshantering vanliga frågor och svar | Microsoft Docs"
description: "Azure Active Directory enhetshantering vanliga frågor och svar."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 000eb6a930187e13cb24cf628793afd06813be23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory-enhetshantering vanliga frågor och svar

**F: jag registrerade nyligen hello enhet. Varför ser inte hello enhet under Mina användarinformation i hello Azure-portalen?**

**S:** Windows 10-enheter som är ansluten till domänen med automatisk enhetsregistrering visas inte under hello användarinformation.
Toouse PowerShell toosee måste alla enheter. 

Endast visas hello följande enheter under hello användarinformation:

- Alla personliga enheter som inte är ansluten enterprise 
- Alla icke - Windows 10 / Windows Server 2016 
- Alla Windows-enheter 

---

**F: Varför kan jag inte se alla hello-enheter som registrerats i Azure Active Directory i hello Azure-portalen?** 

**S:** för närvarande finns ingen sätt toosee alla registrerade enheter i hello Azure-portalen. Du kan använda Azure PowerShell toofind alla enheter. Mer information finns i hello [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.

--- 

**F: hur vet jag vilka hello enheten registreringstillstånd klientens hello är?**

**S:** registreringstillstånd för hello enhet beror på:

- Vilka hello-enheten är
- Hur den har registrerats 
- Information om eventuella relaterade tooit. 
 

---

**F: Varför är en enhet som jag har tagit bort hello Azure-portalen eller med Windows PowerShell fortfarande visas som har registrerats?**

**S:** detta är avsiktligt. hello enheten har inte åtkomst tooresources i hello molnet. Om du vill tooremove hello enhet och registrera igen, måste en manuell åtgärd vara toobe som vidtagits hello enhet. 

För Windows 10 och Windows Server 2016 som är lokala AD-domänansluten:

1.  Öppna hello kommandotolk som administratör.

2.  Typ`dsregcmd.exe /debug /leave`

3.  Logga ut och logga in tootrigger hello schemalagd aktivitet som registrerar hello enheten igen. 

För andra Windows-plattformar som är lokala AD-domänansluten:

1.  Öppna hello kommandotolk som administratör.
2.  Skriv `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Skriv `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---

**F: Varför visas dubblettenhet poster i Azure-portalen?**

**S:**

-   För Windows 10 och Windows Server 2016, om de upprepade försök toounjoin och ansluta igen hello samma enhet, kan det finnas dubbla poster. 

-   Om du har använt Lägg till arbets- eller Skolkonto, de windowsanvändare som använder Lägg till arbets- eller Skolkonto skapar en ny enhetspost med hello samma enhetsnamn.

-   Andra Windows-plattformar som är lokala AD som ingår i domänen med hjälp av automatisk registrering skapar en ny enhetspost med hello samma enhetsnamn för varje domänanvändare som loggar in hello enhet. 

-   En AADJ-dator som har rensats kan installeras igen och nytt ansluts till hello samma namn, visas som en ny post med hello samma enhetsnamn.

---

**F: Varför kan en användare fortfarande komma åt resurser från en enhet som jag har inaktiverats i hello Azure-portalen?**

**S:** kan det ta upp tooan timmen för en återkalla toobe tillämpas.

>[!Note] 
>För förlorade enheter rekommenderar vi att rensa hello enheten tooensure att användare inte kan komma åt hello enhet. Mer information finns i [registrera enheter för hantering i Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**F: Varför Mina användare ser inte ”du kan hämta det här”?**

**S:** om du har konfigurerat vissa villkorlig åtkomst regler toorequire tillstånd för en specifik enhet och hello enheten uppfyller inte kraven för hello kan användare blockeras och finns i det här meddelandet. Kontrollera utvärdera hello regler och se till att hello-enheten är kan toomeet hello kriterier tooavoid det här meddelandet.

---


**F: jag finns hello enhetspost under hello användarinformation i hello Azure-portalen och kan se hello tillstånd som har registrerats på hello-klienten. Kan jag in korrekt för att använda villkorlig åtkomst?**

**S:** hello enhetspost (deviceID) och tillstånd på hello Azure-portalen måste matcha hello klienten och utvärdering kriterier för villkorlig åtkomst. Mer information finns i [Kom igång med Azure Active Directory Device Registration](active-directory-device-registration.md).

---

**F: Varför visas meddelandet ”användarnamnet eller lösenordet är felaktigt” för en enhet har jag bara ansluten tooAzure AD?**

**S:** vanliga orsaker till det här scenariot är:

- Användarens autentiseringsuppgifter är inte längre giltig.

- Datorn är toocommunicate med Azure Active Directory. Sök efter eventuella problem med nätverksanslutningen.

- hello Azure AD Join förutsättningar har inte uppfyllts. Se till att du har följt hello steg för [utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-overview.md).  

- Federerad inloggning kräver din federation server toosupport en aktiv WS-Trust-slutpunkt. 

---

**F: Varför visas hello ”Oops... Det uppstod ett fel”! dialogrutan när jag försöker Anslut min dator?**

**S:** detta beror på hur du konfigurerar Azure Active Directory-registrering med Intune. Mer information finns i [konfigurera enhetshantering för Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**F: Varför har min försök toojoin en dator misslyckas även om jag inte får någon information om fel?**

**S:** en trolig orsak är att hello användaren är inloggad toohello enhet med hjälp av hello inbyggda administratörskontot. Skapa ett annat lokalt konto innan du använder Azure Active Directory Join toocomplete hello-installationen. 

---

**F: var kan jag hitta instruktioner för hello installationsprogrammet för automatisk enhetsregistrering?**

**S:** detaljerade instruktioner finns [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)

---

**F: var kan jag hitta felsökning information om automatisk enhetsregistrering för hello?**

**S:** information om felsökning, se:

- [Felsökning av automatisk registrering av domän domänanslutna datorer tooAzure AD – Windows 10 och Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md)

- [Felsökning av automatisk registrering av domän anslutna datorer tooAzure AD för äldre Windows-klienter](active-directory-device-registration-troubleshoot-windows-legacy.md)
 
---

