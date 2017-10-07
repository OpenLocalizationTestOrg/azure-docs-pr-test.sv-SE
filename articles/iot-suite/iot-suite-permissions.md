---
title: aaaAzure IoT Suite och Azure Active Directory | Microsoft Docs
description: "Beskriver hur Azure IoT Suite använder Azure Active Directory toomanage behörigheter."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: dobett
ms.openlocfilehash: 4768630f2de4bb431731fbd4e8929232bc80b9f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-on-hello-azureiotsuitecom-site"></a>Behörigheter för hello azureiotsuite.com plats

## <a name="what-happens-when-you-sign-in"></a>Vad händer när du loggar in

Hej första gången du loggar in på [azureiotsuite.com][lnk-azureiotsuite], hello platsen avgör hello behörighetsnivåer baserat på hello för närvarande valda Azure Active Directory (AAD)-klient och Azure prenumeration.

1. Först toopopulate hello lista över klienter visas nästa tooyour användarnamn, hello plats hittar från Azure vilka AAD-klienter som du tillhör. Hello plats kan för närvarande kan endast hämta användartoken för en klient i taget. Därför när du växlar klienter med hjälp av hello listrutan i hello övre högra hörnet loggar hello plats du i toothat klient tooobtain hello token för den klienten.

2. Därefter hittar hello plats från Azure vilka prenumerationer som du har associerat med hello valt klient. Du kan se hello tillgängliga prenumerationer när du skapar en ny förkonfigurerade lösning.

3. Slutligen hello plats hämtar alla hello resurser i hello prenumerationer och resursgrupper märks med förkonfigurerade lösningar och fyller hello paneler på hello startsidan.

hello följande avsnitt beskrivs hello roller som styr toohello förkonfigurerade lösningar.

## <a name="aad-roles"></a>AAD-roller

Hej AAD roller styr hello kan etablera förkonfigurerade lösningar och hantera användare i en förkonfigurerade lösning.

Du hittar mer information om administratörsroller i AAD i [Tilldela administratörsroller i Azure AD][lnk-aad-admin]. hello aktuella artikeln fokuserar på hello **Global administratör** och hello **användaren** directory roller som används av hello förkonfigurerade lösningar.

### <a name="global-administrator"></a>Global administratör

Det kan finnas flera globala administratörer per AAD-klient:

* När du skapar en AAD-klient är som standard hello global administratör för att klienten.
* hello global administratör kan etablera en förkonfigurerade lösning och har tilldelats en **Admin** roll för hello program i deras AAD-klient.
* Om en annan användare i hello samma AAD-klient skapar ett program är hello standardroll beviljas toohello global administratör **ReadOnly**.
* En global administratör kan tilldela användare tooroles för program som använder hello [Azure-portalen][lnk-portal].

### <a name="domain-user"></a>Domänanvändare

Det kan finnas många domänanvändare per AAD-klient:

* En domänanvändare kan etablera en förkonfigurerade lösningen genom hello [azureiotsuite.com] [ lnk-azureiotsuite] plats. Som standard beviljas hello domänanvändare hello **Admin** roll i hello etablerats program.
* En domänanvändare kan skapa ett program med hello build.cmd skript i hello [azure iot-fjärr-övervakning][lnk-rm-github-repo], [azure iot – förutsägande-Underhåll] [ lnk-pm-github-repo], eller [azure iot-ansluten-fabrik] [ lnk-cf-github-repo] databasen. Dock hello standardroll beviljas toohello domänanvändare **ReadOnly**, eftersom en domänanvändare inte har behörigheten tooassign roller.
* Om en annan användare i hello AAD-klient skapar ett program, hello domänanvändare tilldelas hello **ReadOnly** roll som standard för programmet.
* En domänanvändare kan inte tilldela roller för program. därför en domänanvändare kan inte lägga till användare eller roller för användare för ett program även om de har etablerats den.

### <a name="guest-user"></a>Gästanvändare

Det kan finnas många gästanvändare per AAD-klient. Gästanvändare har en begränsad uppsättning rättigheter i hello AAD-klient. Därför kan inte gästanvändare etablera en förkonfigurerade lösning i hello AAD-klient.

Mer information om användare och roller i AAD finns hello följande resurser:

* [Skapa användare i Azure AD][lnk-create-edit-users]
* [Tilldela användare tooapps][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Administratörsroller i Azure-prenumeration

hello Azure-administratörsroller styra hello möjlighet toomap en Azure-prenumeration tooan AD-klient.

Lär dig mer om hello Azure administratörsroller i hello artikeln [hur tooadd eller ändra Medadministratör för Azure och tjänstadministratör kontoadministratör][lnk-admin-roles].

## <a name="application-roles"></a>Programroller

Hej programroller styra åtkomst toodevices i din förkonfigurerade lösning.

Det finns två definierade roller och en implicit roll som definierats i ett etablerat program:

* **Admin:** har fullständig kontroll tooadd, hantera, ta bort enheter och ändra inställningarna.
* **Skrivskyddad:** kan visa enheter, regler, åtgärder, jobb och telemetri.

Du kan hitta hello behörigheter tooeach roll i hello [RolePermissions.cs] [ lnk-resource-cs] källfilen.

### <a name="changing-application-roles-for-a-user"></a>Ändra programroller för en användare

Du kan använda följande procedur toomake en användare i din Active Directory-administratör för din förkonfigurerade lösning hello.

Du måste vara en AAD global administratör toochange roller för en användare:

1. Gå toohello [Azure-portalen][lnk-portal].
2. Välj **Azure Active Directory**.
3. Kontrollera att du använder hello-katalog som du valde på azureiotsuite.com när du har etablerat din lösning. Om du har flera kataloger som hör till din prenumeration kan växla du mellan dem om du klickar på namnet på ditt konto hello längst upp till höger i hello-portalen.
4. Klicka på **företagsprogram**, sedan **alla program**.
4. Visa **alla program** med **alla** status. Söka efter ett program med namnet på din förkonfigurerade lösning.
5. Klicka på hello namnet på hello-program som matchar din förkonfigurerade lösningsnamn.
6. Klicka på **användare och grupper**.
7. Välj hello-användare som du vill tooswitch roller.
8. Klicka på **tilldela** och välj hello roll (som **Admin**) som tooassign toohello användaren klicka på kryssmarkeringen hello.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

### <a name="im-a-service-administrator-and-id-like-toochange-hello-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Jag är en tjänstadministratör och jag vill toochange hello directory mappning mellan min prenumeration och en specifik AAD-klient. Hur jag för att slutföra den här uppgiften?

1. Gå toohello [klassiska Azure-portalen][lnk-classic-portal], klickar du på **inställningar** i hello lista över tjänster hello vänster.
2. Välj hello-prenumeration som du vill att toochange hello katalogmappning till.
3. Klicka på **redigera Directory**.
4. Välj hello **Directory** som toouse i hello listrutan. Klicka på pilen för hello framåt.
5. Bekräfta hello katalogmappning och påverkas medadministratörer. Om du flyttar från en annan katalog tas alla hello medadministratörer från hello ursprungliga katalogen bort.

### <a name="im-a-domain-usermember-on-hello-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Jag är användaren/medlem i en domän på hello AAD-klient och du har skapat en förkonfigurerade lösning. Hur jag tilldelas en roll för Mina program?

Be en global administratör toomake du en global administratör på hello AAD-klient och sedan tilldela roller toousers själv. Du kan också be en global administratör tooassign du en roll direkt. Om du vill att toochange hello AAD-klient din förkonfigurerade lösning har distribuerats till, finns i hello nästa fråga.

### <a name="how-do-i-switch-hello-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Hur växlar min remote förkonfigurerade övervakningslösning och programmet har tilldelats hello AAD-klient?

Du kan köra en molndistribution från <https://github.com/Azure/azure-iot-remote-monitoring> och distribuera till en nyligen skapade AAD-klient. Eftersom du är som standard en global administratör när du skapar en AAD-klient har behörigheter tooadd användare och tilldela roller toothose användare.

1. Skapa ett AAD-katalogen i hello [Azure-portalen][lnk-portal].
2. Gå för<https://github.com/Azure/azure-iot-remote-monitoring>.
3. Kör `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (till exempel `build.cmd cloud debug myRMSolution`)
4. När du uppmanas att ange hello **tenantid** toobe nyligen skapade klienten i stället för din tidigare klient.

### <a name="i-want-toochange-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Jag vill toochange en tjänstadministratör eller en Medadministratör när du har loggat in med ett organisatoriska konto

Se hello support-artikeln [ändra tjänstadministratören och Medadministratör när du har loggat in med ett konto som organisatoriska][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-hello-proper-permissions-toocreate-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Varför ser jag det här felet? ”Ditt konto har inte hello åtkomstbehörighet toocreate en lösning. Kontrollera med din kontoadministratör eller försök med ett annat konto ”.

Titta på hello följande diagram anvisningar:

![][img-flowchart]

> [!NOTE]
> Om du fortsätter toosee hello fel efter verifiering är en global administratör på hello AAD-klient och en medadministratör för prenumerationen hello måste ha din kontoadministratör om du tar bort hello användare och tilldela behörigheter som krävs i den här ordningen. Först lägger till hello användare som en global administratör och sedan lägga till användare som medadministratör på hello Azure-prenumeration. Om problem kvarstår kontaktar du [hjälp och Support][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-toocreate-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Varför ser jag det här felet när jag har en Azure-prenumeration? ”En Azure-prenumeration är obligatoriska toocreate förkonfigurerade lösningar. Du kan skapa ett kostnadsfritt utvärderingskonto på bara några minuter ”.

Om du är säker på att du har en Azure-prenumeration Validera hello klient mappning för din prenumeration och kontrollera hello rätt klient är markerad i listrutan hello. Om du har verifierats hello önskat klient är korrekt, följ hello föregående diagram och verifiera hello mappningen av din prenumeration och AAD-klient.

## <a name="next-steps"></a>Nästa steg
Lär dig mer om IoT Suite toocontinue se hur du kan [anpassa förkonfigurerade lösning][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
