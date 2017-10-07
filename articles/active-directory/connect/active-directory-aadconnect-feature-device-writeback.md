---
title: 'Azure AD Connect: Aktivera tillbakaskrivning av enheter | Microsoft Docs'
description: "Det här dokumentet information hur tooenable tillbakaskrivning av enheter med Azure AD Connect"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 2566a514137fed85b21929207cf3230e6878ebbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Aktivera tillbakaskrivning av enheter
> [!NOTE]
> Det krävs en prenumeration tooAzure AD Premium för tillbakaskrivning av enheter.
> 
> 

hello följande dokumentation innehåller information om hur tooenable hello tillbakaskrivning av enheter funktion i Azure AD Connect. Tillbakaskrivning av enheter används i hello följande scenarier:

* Aktivera villkorlig åtkomst baserat på enheter tooADFS (2012 R2 eller högre) skyddade applikationer (förtroenden för beroende part).

Detta ger ökad säkerhet och försäkran om att komma åt tooapplications beviljas endast tootrusted enheter. Mer information om villkorlig åtkomst finns [hantera risker med villkorlig åtkomst](../active-directory-conditional-access.md) och [konfigurera lokal villkorlig åtkomst med hjälp av Azure Active Directory Device Registration](../active-directory-conditional-access-automatic-device-registration-setup.md).

> [!IMPORTANT]
> <li>Enheter måste finnas i samma skog som hello användare hello. Eftersom enheter måste skrivas tillbaka tooa enkel skog, stöder funktionen för närvarande inte en distribution med flera skogar för användaren.</li>
> <li>Konfigurationsobjekt för endast en enhet registreringen kan läggas till toohello lokala Active Directory-skog. Den här funktionen är inte kompatibel med en topologi där hello lokala Active Directory är synkroniserade toomultiple Azure AD-kataloger.</li>> 

## <a name="part-1-install-azure-ad-connect"></a>Del 1: Installera Azure AD Connect
1. Installera Azure AD Connect med anpassade eller standardinställningar. Microsoft rekommenderar toostart med alla användare och grupper som synkroniserats innan du aktiverar tillbakaskrivning av enheter.

## <a name="part-2-prepare-active-directory"></a>Del 2: Förbereda Active Directory
Använd hello följande steg tooprepare för att använda tillbakaskrivning av enheter.

1. Starta PowerShell från hello datorn där Azure AD Connect är installerat i upphöjt läge.
2. Om hello Active Directory PowerShell-modulen inte är installerad, installerar du den med hjälp av hello följande kommando:
   
   `Add-WindowsFeature RSAT-AD-PowerShell`
3. Om hello Azure Active Directory PowerShell-modulen inte är installerat kan du hämta och installera den från [Azure Active Directory-modulen för Windows PowerShell (64-bitars version)](http://go.microsoft.com/fwlink/p/?linkid=236297). Den här komponenten har ett beroende på hello-inloggningsassistent, som installeras med Azure AD Connect.
4. Kör följande kommandon hello och avsluta sedan PowerShell med enterprise-administratörsautentiseringsuppgifter.
   
   `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`
   
   `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Enterprise-administratörsautentiseringsuppgifter krävs eftersom ändringar toohello configuration namnrymd krävs. En domänadministratör har inte tillräcklig behörighet.

![PowerShell för att aktivera tillbakaskrivning av enhet.](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Beskrivning:

* Om de inte finns redan, skapar och konfigurerar nya behållare och objekt under CN = konfiguration för Enhetsregistrering, CN = Services, CN = Configuration, [skog dn].
* Om de inte finns redan, skapar och konfigurerar nya behållare och objekt under CN = Registreradeenheter, [domän-dn]. Enhetsobjekt skapas i den här behållaren.
* Anger behörigheter som krävs för hello Azure AD-anslutningskontot toomanage enheter i din Active Directory.
* Behöver bara toorun i en skog, även om Azure AD Connect installeras på flera skogar.

Parametrar:

* Domännamn: Active Directory-domän där enhetsobjekt kommer att skapas. Obs: Alla enheter för en viss Active Directory-skog skapas i en domän.
* AdConnectorAccount: Active Directory-konto som ska användas av Azure AD Connect toomanage objekt i hello directory. Detta är hello-konto som används av Azure AD Connect sync tooconnect tooAD. Hello-kontot med MSOL_ prefixet är om du har installerat med standardinställningar.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Del 3: Aktivera tillbakaskrivning av enhet i Azure AD Connect
Använd följande procedur tooenable tillbakaskrivning av enhet i Azure AD Connect hello.

1. Kör hello installationsguiden igen. Välj **anpassa synkroniseringsalternativ** från hello ytterligare uppgifter och klickar på **nästa**.
   ![Anpassad installation anpassa synkroniseringsalternativ](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2. Hello valfria funktioner på sidan är tillbakaskrivning av enheter inte längre nedtonade. Observera att om hello Azure AD Connect prep stegen inte har slutförts kommer grå tillbakaskrivning av enheter ut hello valfria funktioner på sidan. Hej för tillbakaskrivning av enheter och klickar på **nästa**. Om kryssrutan hello fortfarande är inaktiverad finns hello [avsnittet om felsökning](#the-writeback-checkbox-is-still-disabled).
   ![Anpassad installation valfria funktioner för tillbakaskrivning av enhet.](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3. Hello tillbakaskrivning på sidan visas hello angivna domän som hello standard enheten tillbakaskrivning skog.
   ![Anpassad installation enheten tillbakaskrivning målskogen](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4. Slutför hello installationen av hello guiden utan ytterligare konfigurationsändringar. Om det behövs, referera för[anpassad installation av Azure AD Connect.](active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Aktivera villkorlig åtkomst
Detaljerade anvisningar tooenable det här scenariot är tillgängliga i [konfigurera lokal villkorlig åtkomst med hjälp av Azure Active Directory Device Registration](../active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="verify-devices-are-synchronized-tooactive-directory"></a>Kontrollera att enheterna är synkroniserade tooActive Directory
Tillbakaskrivning av enheter bör nu fungerar korrekt. Tänk på att det kan ta upp too3 timmar för enheten objekt toobe skrivs tillbaka tooAD.  tooverify att dina enheter som synkroniseras korrekt, hello efter när hello sync regler har slutfört:

1. Starta Active Directory Administrationscenter.
2. Expandera Registreradeenheter, inom hello domän är som federerat.
   ![Active Directory Administrationscenter registrerade enheter](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3. Aktuella registrerade enheter visas det.
   ![Active Directory Administrationscenter registrerade enheter lista](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Felsökning
### <a name="hello-writeback-checkbox-is-still-disabled"></a>hello tillbakaskrivning kryssrutan är fortfarande inaktiverad
Om hello kryssrutan för tillbakaskrivning av enheter inte är aktiverat trots att du har följt hello stegen ovan hello följande steg vägleder dig genom vilka hello installation verifierar guiden innan hello rutan är aktiverad.

Första sakerna första:

* Kontrollera att minst en skog som har Windows Server 2012 R2. hello enhetstyp objekt måste finnas.
* Om hello installationsguiden körs redan, kommer alla ändringar inte identifieras. I det här fallet Slutför installationsguiden för hello och kör det igen.
* Kontrollera att hello-konto som du anger i hello Initieringsskript är faktiskt hello rätt användare används av hello Active Directory-kopplingen. tooverify, gör du följande:
  * Hello start-menyn och öppna **synkroniseringstjänsten**.
  * Öppna hello **kopplingar** fliken.
  * Hitta hello anslutningen med Active Directory Domain Services och markera den.
  * Under **åtgärder**väljer **egenskaper**.
  * Gå för**ansluta tooActive Directory-skog**. Kontrollera att hello domän och användarnamn namnet som angetts på den här skärmen matchar hello konto toohello skript.
    ![Connector-kontot i Sync Service Manager](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Kontrollera konfigurationen i Active Directory:

* Kontrollera att hello registreringstjänsten för enheter finns i hello plats nedan (CN DeviceRegistrationService, CN = = enheten registreringen Services, CN = konfiguration för Enhetsregistrering, CN = Services, CN = Configuration) enligt konfigurationsnamngivningskontexten.

![Felsöka DeviceRegistrationService i konfigurationen namnområde](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

* Kontrollera att det finns endast en konfigurationsobjektet genom att söka hello configuration namnområde. Om det finns fler än en bort hello dubblett.

![Felsöka, Sök efter duplicerade hello-objekt](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

* Kontrollera att hello attributet msDS-DeviceLocation finns och har ett värde på hello Device Registration Service-objekt. Sökning sökvägen och se till att det finns en sådan med hello objectType msDS-DeviceContainer.

![Felsöka msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Felsöka Registreradeenheter objektklass](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

* Kontrollera hello konto som används av hello Active Directory-kopplingen har behörigheter som krävs på hello registrerade enheter behållare som påträffats av hello föregående steg. Detta är förväntat hello behörigheter på den här behållaren:

![Felsöka, kontrollera behörigheter för behållaren](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

* Kontrollera hello Active Directory-konto har behörigheter för hello CN = konfiguration för Enhetsregistrering, CN = Services, CN = Configuration-objekt.

![Felsöka, kontrollera behörigheter på konfigurationen för Enhetsregistrering](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Ytterligare Information
* [Hantera risker med villkorlig åtkomst](../active-directory-conditional-access.md)
* [Konfigurera lokal villkorlig åtkomst med hjälp av Azure Active Directory Device Registration](../active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

