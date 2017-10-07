---
title: aaaProtect personliga data med Azure identitets- och kontroller | Microsoft Docs
description: "Med hjälp av Azure identitets- och kontroller toohelp skyddar du dina personliga data"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 3132c2af25f86662668e5b555eab1d81de7f2e6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-and-multi-factor-authentication-protect-personal-data-with-identity-and-access-controls"></a>Azure Active Directory och Multi-Factor Authentication: skydda personuppgifter med identitets- och kontroller

Den här artikeln innehåller information och procedurer som du kan använda tooprotect personliga data med hjälp av Azure Active Directory och Multi-Factor authentication-säkerhetsfunktioner och tjänster.

## <a name="scenario"></a>Scenario

Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavet, Adriatiska havet, baltiska havet samt hello brittiska staterna. toosupport dessa ansträngningar den genererade flera mindre kryssning rader i Italien Tyskland, Danmark och hello Storbritannien 

hello företag använder Microsoft Azure toostore företagsdata i hello molnet. Detta inkluderar personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation av dess globala kundbas. Den innehåller också traditionella personal information, till exempel adresser, telefonnummer, skatt identifikationsnummer och medicinska information om företagets anställda på alla platser. hello kryssning rad har också en stor databas med medlemmar av trafik och förmåner som innehåller personuppgifter tootrack relationer med aktuella och tidigare kunder.

Företagets anställda åtkomst hello nätverket från hello företagets fjärranslutna kontor och resa agenter finns runt hello world har åtkomst till företagsresurser för toosome.

## <a name="problem-statement"></a>Problembeskrivning

hello företag måste skydda hello sekretess kundernas och anställdas personliga data från angripare söker toouse komprometteras identiteter toogain åtkomst. De måste kontrollera även att åtkomst toopersonal data genom att behöriga användare är begränsad till endast de som behöver den toodo sitt arbete.

## <a name="company-goal"></a>Företagets mål

hello företagets mål är tooensure som toopersonal dataåtkomst strikt kontrollerad. Det är viktigt att identiteten för användare med åtkomst till toopersonal data skyddas av stark autentisering. En princip av [minsta privilegium] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) måste vara tvingande så att behöriga användare har bara hello åtkomstnivå de behöver, och inga fler.

## <a name="solutions"></a>Lösningar

Microsoft Azure tillhandahåller identitets- och hanteringsverktyg toohelp företag kontrollera vem som har åtkomst till tooresources som innehåller personuppgifter.

### <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) hanterar identiteter och styr åtkomsten tooAzure samt andra lokalt och andra molnresurser, data och program. [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) hjälper Azure administratörer toominimize hello antalet personer som har åtkomst toocertain information, till exempel personuppgifter. Det möjliggör toodiscover, begränsa och övervaka Privilegierade identiteter och åtkomst tooresources och tooassign tillfälliga in JIT-administratörsrättigheter tooeligible användarna. Det ger också inblick i de som har administratörsbehörighet för AAD.

hello aktiviteter med hjälp av AAD PIM omfattar:

- Aktivera Privileged Identity Management för din katalog

- Med Privileged Identity Management admin instrumentpanelen toosee viktig information på ett ögonblick

- Hantera hello Privilegierade identiteter (administratörer) genom att lägga till eller ta bort permanent eller är tillämpliga tooeach administratörsrollen

- Konfigurera hello produktaktivering rollinställningar

- Aktivera roller

- Granska rollen aktivitet

#### <a name="how-do-i-enable-aad-pim"></a>Hur aktiverar AAD PIM?

toostart med hjälp av PIM för din katalog hello följande:

1. Logga in toohello Azure-portalen som global administratör för din katalog.

2. Om din organisation har mer än en katalog, väljer du ditt användarnamn i hello övre högra hörnet av hello Azure-portalen. Välj hello katalog där du ska använda Azure AD Privileged Identity Management.

3. Välj **fler tjänster** och använda hello **Filter** textruta toosearch för Azure AD Privileged Identity Management.

4. Kontrollera **PIN-kod toodashboard** och klicka sedan på **skapa**. hello programmet Privileged Identity Management öppnas.

När Azure AD Privileged Identity Management har konfigurerats, finns hello navigering bladet varje gång du öppnar programmet hello.

![](media/protect-personal-data-identity-access-controls/azure-pim.png)

Mer information och instruktioner för att komma igång med AAD PIM finns [starta med hjälp av Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)

### <a name="azure-role-based-access-control"></a>Rollbaserad åtkomstkontroll i Azure

[Azure rollbaserad åtkomstkontroll](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) hjälper Azure administratörer att hantera åtkomst tooAzure resurser genom att aktivera hello beviljas åtkomst baserat på hello användarens tilldelade rollen. Du kan särskilja uppgifter i ett team och ge endast hello mängd åtkomst toousers, grupper och program som de behöver tooperform sitt arbete.

Rollbaserad åtkomst kan beviljas toousers med hello Azure-portalen, Azure kommandoradsverktyg eller Azure Management-API: er.

Läs mer om grunderna i Azure RBAC [Kom igång med rollbaserad åtkomstkontroll i hello Azure-portalen.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)

#### <a name="how-do-i-manage-azure-rbac-with-powershell"></a>Hur hanterar Azure RBAC med PowerShell?

Du kan använda PowerShell-cmdlets toomanage Azure RBAC, inklusive följande hanteringsuppgifter hello:

- Lista roller

- Se vem som har åtkomst

- Bevilja åtkomst

- Ta bort åtkomst

- Skapa en anpassad roll

- Hämta åtgärder för en Resursprovider

- Ändra en anpassad roll

- Ta bort en anpassad roll

- Lista över anpassade roller

Anvisningar för hur toomanage Azure RBAC med PowerShell, se [Hantera rollbaserad åtkomst med Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).

### <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) är en lösning för verifiering av två steg som hjälper dig att skydda åtkomst toodata och program, samtidigt som du uppfyller efterfrågan från användarna för en process för enkel inloggning. Den ger stark autentisering via en mängd verifieringsmetoder, inklusive telefonsamtal, textmeddelande eller verifiering av mobilappar.

toodeploy MFA i hello Azure-molnet måste toofirst aktivera den och sedan aktivera tvåstegsverifiering för användare.

#### <a name="how-do-i-enable-azure-toouse-mfa"></a>Hur aktiverar toouse Azure MFA?

Om användarna har licenser som innehåller Azure Multi-Factor Authentication, finns det inget att du behöver toodo tooturn på Azure MFA. Om inte, du behöver toocreate en Multi-Factor Authentication-leverantör i din katalog. toodo, gör du följande:

1. Välj **Active Directory** i hello klassiska Azure-portalen (logga in som administratör).

2. Välj **Multi-Factor Authentication-Providers.**

3. Välj **ny** och sedan under **Apptjänster,** Välj **leverantör av Multifaktorautent.**

4. Välj **Snabbregistrering.**

5. Fyll i fältet för hello namn och välj en användningsmodell (per autentisering eller per aktiverad användare).

6. Ange en katalog som hello MFA-leverantören är kopplad.

7. Klicka på hello **skapa** knappen.

![](media/protect-personal-data-identity-access-controls/quick-create.png)

Mer information om hur toomanage din leverantör av Multifaktorautent, se [komma igång med en Azure leverantör av Multifaktorautent.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)

#### <a name="how-do-i-turn-on-two-step-verification-for-users"></a>Hur aktiverar tvåstegsverifiering för användarna?

Du kan tillämpa tvåstegsverifiering för alla inloggningar eller skapa tvåstegsverifiering för villkorlig åtkomst principer toorequire endast när särskilda villkor gäller.

Om du aktiverar Azure MFA genom att ändra användartillstånd är hello traditionella metoden för att kräva tvåstegsverifiering. Alla hello-användare som du aktiverar har hello samma krav tooperform tvåstegsverifiering varje gång de loggar in. Aktivera en användare åsidosätter alla principer för villkorlig åtkomst som kan påverka den användaren.

Om du aktiverar Azure MFA med en princip för villkorlig åtkomst är ett mer flexibelt sätt för att kräva tvåstegsverifiering. Du kan skapa principer för villkorlig åtkomst som gäller toogroups samt enskilda användare. Hög risk grupper kan få fler begränsningar än låg risk grupper eller tvåstegsverifiering kan krävs endast för hög risk molnappar och hoppades över för de låg risk. Villkorlig åtkomst är dock en betald funktion i Azure Active Directory.

tooenable MFA genom att ändra användarens tillstånd, hello följande:

1. Logga in toohello Azure portal som administratör.
2. Gå för**Azure Active Directory \> användare och grupper \> alla användare**.
3. Välj **Multifaktorautentisering**.
4. Hitta hello användare som du vill tooenable för Azure MFA. Du kan behöva toochange hello visa hello överst.
5. Kontrollera hello rutan nästa toohello användarens namn.
6. Välj på höger under snabbsteg hello, **aktivera**.

   ![](media/protect-personal-data-identity-access-controls/quick-create.png)

7. Bekräfta dina val i hello popup-fönster som öppnas.  Användare som MFA har aktiverats måste tooregister hello nästa gång de loggar in.

tooenable Azure MFA med en princip för villkorlig åtkomst hello följande:

1. Logga in toohello Azure portal som administratör.

2. Gå för**Azure Active Directory \> villkorlig åtkomst**.

3. Välj **ny princip**.

4. Under **tilldelningar**väljer **användare och grupper**. Använd hello **inkludera** och **undanta** flikarna toospecify vilka användare och grupper som ska hanteras av hello princip.

5. Under **tilldelningar**väljer **Molnappar.** Välj för**omfattar alla molnappar**.
6.  Under **åtkomstkontroller**väljer **bevilja**. Välj **kräver Multi-Factor authentication**.
7.  Aktivera **aktiverar principen** för**på** och välj sedan **spara**.

Mer information om hur tooconfigure Azure MFA inställningar tooset in bedrägerivarningar, skapar en engångsförbikoppling kan använda anpassade röstmeddelanden, konfigurera cachelagring, ange tillförlitliga IP-adresser, skapa applösenord, aktivera komma ihåg MFA för enheter som användare litar på och välj verifieringsmetoderna, se [konfigurera inställningarna för Azure Multi-Factor Authentication.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)

## <a name="next-steps"></a>Nästa steg

- [Skydda privilegierad åtkomst i Azure AD](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)

- [Vanliga frågor om Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-faq)

- [Rollbaserad åtkomstkontroll felsökning](https://docs.microsoft.com/azure/active-directory/role-based-access-control-troubleshooting)

- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)
