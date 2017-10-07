---
title: "aaaCreate anpassade rollbaserad åtkomstkontroll roller och tilldela toointernal och externa användare i Azure | Microsoft Docs"
description: "Tilldela anpassade RBAC-roller med hjälp av PowerShell och CLI för interna och externa användare"
services: active-directory
documentationcenter: 
author: andreicradu
manager: catadinu
editor: kgremban
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/10/2017
ms.author: a-crradu
ms.openlocfilehash: 26793a66d6ca2f771338eed87d10ce2b3b431841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="intro-on-role-based-access-control"></a>Introduktion på rollbaserad åtkomstkontroll

Rollbaserad åtkomstkontroll är en Azure portal endast funktion som tillåter att hello ägare för en prenumeration tooassign detaljerade roller tooother användare som kan hantera en viss resurs-scope i sina miljöer.

RBAC kan bättre säkerhetshantering för stora organisationer och för små och medelstora företag arbetar med externa samarbetspartners, leverantörer eller freelancers som behöver åtkomst till toospecific resurser i din miljö, men inte nödvändigtvis toohello hela infrastruktur eller någon fakturerings-relaterade scope. RBAC ger hello flexibiliteten för en Azure-prenumeration som hanteras av hello administratörskonto (service administratörsrollen på en prenumerationsnivå) som äger och har flera användare som bjudits in toowork under hello samma prenumeration men utan några administrativa behörighet för den. Från en hanteringen och fakturering perspektiv bevisar hello RBAC funktionen toobe tids- och effektiv alternativet för att använda Azure i olika scenarier.

## <a name="prerequisites"></a>Krav
Med RBAC i hello Azure-miljön kräver:

* Med en fristående Azure-prenumeration har gett toohello användaren som ägare (prenumeration roll)
* Har hello ägarrollen av hello Azure-prenumeration
* Ha åtkomst toohello [Azure-portalen](https://portal.azure.com)
* Kontrollera att toohave hello följande Resursproviders registrerad för hello användaren prenumeration: **Microsoft.Authorization**. Mer information om hur tooregister hello resursprovidrar finns [Resource Manager-providers, regioner, API-versioner och scheman](/azure-resource-manager/resource-manager-supported-services.md).

> [!NOTE]
> Office 365-prenumerationer eller Azure Active Directory-licenser (till exempel: komma åt tooAzure Active Directory) etablerats från hello O365 portal inte kvalitet med RBAC.

## <a name="how-can-rbac-be-used"></a>Hur kan RBAC användas
RBAC kan tillämpas på tre olika scope i Azure. Från hello högsta omfånget toohello lägsta en är de följande:

* Prenumeration (högsta)
* Resursgrupp
* Resursen omfång (hello lägsta åtkomstnivå erbjudande riktade behörigheter tooan enskilda Azure-resurs scope)

## <a name="assign-rbac-roles-at-hello-subscription-scope"></a>Tilldela roller RBAC hello prenumeration definitionsområdet
Det finns två vanliga exempel när RBAC används (men inte begränsat till):

* Med externa användare från organisationer hello (inte ingår i hello admin användarens Azure Active Directory-klient) inbjuden toomanage vissa resurser eller hello hela prenumerationen
* Arbeta med användare i hello organisation (det är en del av hello användarens Azure Active Directory-klient), men en del av olika grupper eller grupper som behöver åtkomst noggrann antingen toohello scope hela prenumerationen eller toocertain resursgrupper eller resursen i hello miljö

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Bevilja åtkomst till en prenumerationsnivå för en användare utanför Azure Active Directory
RBAC-roller som kan beviljas endast av **ägare** hello prenumeration därför hello admin-användare måste vara inloggad med ett användarnamn som har rollen förinställda eller har skapat hello Azure-prenumeration.

Från hello Azure-portalen, när du logga in som administratör, Välj ”prenumerationer” och valt hello önskade en.
![prenumerationsbladet i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) som standard om hello administratörsanvändare har köpt hello Azure-prenumeration, hello användare visas som **kontoadministratören**använder den här som hello prenumeration roll. Mer information om hello Azure-prenumeration roller finns [lägga till eller ändra Azure-administratörsroller som hanterar hello prenumeration eller tjänster](/billing/billing-add-change-azure-subscription-administrator.md).

I det här exemplet hello användare ”alflanigan@outlook.com” är hello **ägare** prenumerationen i hello AAD av hello ”utvärderingsversion” klient ”standard klient Azure”. Eftersom den här användaren är hello skapare av hello Azure-prenumeration med hello inledande Account ”Outlook” (Account = Outlook Live etc.) hello standarddomännamnet för alla andra användare som lagts till i den här klienten kommer att **”@alflaniganuoutlook.onmicrosoft.com”**. Avsiktligt hello syntaxen för hello ny domän bildas genom att sätta ihop hello användarnamn och domän namnet på hello-användaren som skapade hello klient och lägga till hello tillägget **”. onmicrosoft.com”**.
Dessutom användare kan logga in med ett anpassat domännamn i hello-klienten efter att lägga till och verifierar för hello nya innehavaren. Mer information om hur tooverify ett anpassat domännamn i Azure Active Directory-klient Se [lägga till en anpassad domän namn tooyour katalog](/active-directory/active-directory-add-domain).

I det här exemplet hello ”standard Azure” klientkatalogen innehåller endast användare med hello domännamnet ”@alflanigan.onmicrosoft.com”.

När du har valt hello prenumeration hello administratörsanvändaren måste klicka på **Access Control (IAM)** och sedan **lägga till en ny roll**.





![funktionen IAM för åtkomstkontroll i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![lägga till nya användare i IAM-funktionen åtkomstkontroll i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

hello nästa steg är tilldelade tooselect hello rollen toobe och hello hello RBAC roll ska tilldelas användare. I hello **rollen** nedrullningsbara menyn hello administratörsanvändaren ser endast hello inbyggda RBAC roller som är tillgängliga i Azure. Mer detaljerade beskrivningar av varje roll och deras tilldelningsbara scope finns [inbyggda roller för rollbaserad åtkomstkontroll i](/active-directory/role-based-access-built-in-roles.md).

Hej administratör måste tooadd hello användarens e-postadress hello externa. hello förväntat beteende är för hello extern användare toonot visas i hello befintlig klient. När du har bjudits in hello externa användare, han visas under **prenumerationer > Access Control (IAM)** med alla aktuella hello-användare som är tilldelade en RBAC-rollen på hello prenumerationsomfattningen.





![lägga till behörigheter toonew RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![lista över RBAC-roller på prenumerationsnivån](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

hello användare ”chessercarlton@gmail.com” inbjudna toobe har en **ägare** för hello ”utvärderingsversion” prenumeration. När du har skickat hello inbjudan får hello externa användaren ett e-postbekräftelse med en aktiveringslänk.
![e-postinbjudan för RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)

Som externa toohello organisation har hello ny användare inte några befintliga attribut i hello ”klient Azure” standardkatalog. De kommer att skapas när hello extern användare har gett samtycke toobe registreras i hello-katalog som är kopplat till hello-prenumeration som har tilldelats en roll.





![e-postmeddelande för inbjudan för RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

hello extern användare visas i hello Azure Active Directory-klient hädanefter som externa användare och det kan visas både i hello Azure-portalen och hello klassiska portal.





![användare bladet azure active directory Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)





![användare bladet azure active directory klassiska Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/8.png)

I hello **användare** vyn i både portaler hello externa användare kan identifieras av:

* hello olika ikoner typen i hello Azure-portalen
* hello olika källor punkten i hello klassiska portalen

Dock bevilja **ägare** eller **deltagare** åtkomst tooan externa användare på hello **prenumeration** omfång, tillåter inte hello åtkomst toohello admin användarens directory Om inte hello **Global administratör** tillåter. I hello användaren proprieties hello **användartyp** som har två gemensamma parametrar, **medlem** och **gäst** kan identifieras. En medlem är en användare som är registrerad i hello directory medan gäst är en inbjuden toohello användarkatalog från en extern källa. Mer information finns i [hur till B2B-samarbete användare av Azure Active Directory-administratörer](/active-directory/active-directory-b2b-admin-add-users).

> [!NOTE]
> Kontrollera att hello extern användare väljer hello rätt katalog toosign i när du har angett hello autentiseringsuppgifter i hello-portalen. hello kan samma användare ha åtkomst toomultiple kataloger och kan välja någon av dem genom att klicka hello användarnamnet i hello översta högra sidan i hello Azure-portalen och välj sedan hello lämplig katalog hello listrutan.

Samtidigt som gäst i hello directory hello externa användare kan hantera alla resurser för hello Azure-prenumeration, men kan inte komma åt hello directory.





![åtkomst till begränsade tooazure active directory Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

Azure Active Directory och Azure-prenumeration har inte en underordnad-överordnad relation som andra Azure-resurser (till exempel: virtuella datorer, virtuella nätverk, webbprogram, lagring etc.) med en Azure-prenumeration. Alla hello senare skapas, hanteras och debiteras enligt en Azure-prenumeration medan en Azure-prenumeration används toomanage hello åtkomst tooan Azure-katalogen. Mer information finns i [hur Azure-prenumerationen är relaterade tooAzure AD](/active-directory/active-directory-how-subscriptions-associated-directory).

Från alla hello inbyggda RBAC roller, **ägare** och **deltagare** erbjuda full tillgång till tooall resurser i hello miljö, hello skillnaden som som en deltagare som inte kan skapa och ta bort nya RBAC-roller. hello andra inbyggda roller som **Virtual Machine-deltagare** och ger fullständig åtkomst bara toohello resurser som anges av hello-namnet, oavsett hello **resursgruppen** de skapas i.

Tilldela hello inbyggda RBAC roll **Virtual Machine-deltagare** innebär hello användartilldelade hello rollen på en på prenumerationsnivå:

* Kan visa alla virtuella datorer oavsett deras distribution datum och hello resursgrupper de ingår i
* Har fullständig åtkomst toohello virtuella hanteringsenheter hello prenumeration
* Kan inte visa några andra typer av resurser i hello prenumeration
* Fungerar inte ändringar ur fakturering

> [!NOTE]
> RBAC som en portal endast funktion i Azure, ger inte den åtkomst toohello klassiska portalen.

## <a name="assign-a-built-in-rbac-role-tooan-external-user"></a>Tilldela en inbyggda RBAC rollen tooan externa användare
Ett annat scenario i det här testet hello i externa användare ”alflanigan@gmail.com” läggs till som en **Virtual Machine-deltagare**.




![inbyggda deltagarrollen virtuell dator](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

hello normalt beteende för den här externa användare med den här inbyggda rollen är toosee och hantera endast virtuella datorer och deras intilliggande Resource Manager endast resurser som är nödvändiga vid distribution. Avsiktligt rollerna begränsad erbjuda åtkomst endast motsvarande tootheir resurser som du skapade i hello Azure-portalen, oavsett vissa fortfarande distribueras i hello samt den klassiska portalen (till exempel: virtuella datorer).





![deltagare rollen översikt över virtuella datorer i azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-hello-same-directory"></a>Bevilja åtkomst till på en prenumerationsnivå för en användare i hello samma katalog
processflöde för hello är identiska tooadding en extern användare både från hello perspektiv beviljande hello RBAC administratörsroll samt hello användaren beviljas åtkomst toohello roll. hello skillnaden här är hello uppmanas användaren inte får någon e-inbjudningar som alla hello resurs scope inom hello prenumeration blir tillgängliga i hello instrumentpanelen efter inloggningen.

## <a name="assign-rbac-roles-at-hello-resource-group-scope"></a>Tilldela RBAC-roller på hello resurs Gruppomfång
Tilldela en RBAC-rollen på en **resursgruppen** scope har en identisk process för att tilldela hello-rollen på hello prenumerationsnivån för båda typer av användare – extern eller intern (en del av hello samma katalog). hello användare som är tilldelade hello RBAC roll är toosee i sina miljöer bara hello resursgruppen de har tilldelats åtkomst från hello **resursgrupper** ikon i hello Azure-portalen.

## <a name="assign-rbac-roles-at-hello-resource-scope"></a>Tilldela roller RBAC hello resurs definitionsområdet
Tilldela en roll RBAC definitionsområdet resurs i Azure har en identisk process för att tilldela hello-rollen på hello prenumerationsnivån eller på hello resursgruppsnivå, följande hello samma arbetsflöde för båda scenarierna. Hello-användare som är tilldelade hello RBAC roll kan finns endast hello objekt som de har tilldelats åtkomst, antingen i hello **alla resurser** fliken eller direkt i sina instrumentpanelen.

En viktig aspekt för RBAC både på resursen Gruppomfång eller resurs scope är för hello användare toomake toohello toosign Kontrollera i rätt katalog.





![Directory inloggning i Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>Tilldela RBAC-roller för en Azure Active Directory-grupp
Alla hello scenarier med RBAC på hello tre olika scope i Azure-erbjudande hello behörighet för att hantera, distribuera och administrera olika resurser som tilldelats användare utan hello behöver för att hantera en personlig prenumeration. Oavsett hello RBAC roll har tilldelats för en prenumeration, resursgrupp eller resurs scope, alla hello resurser skapas ytterligare på av hello tilldelade användare debiteras under hello en Azure-prenumeration där hello användare har åtkomst till. Det här sättet hello användare som har administratörsbehörighet för att hela Azure-prenumeration fakturering har en fullständig översikt på hello-förbrukning, oavsett vem som hanterar hello resurser.

För större organisationer RBAC-roller kan användas i hello samma sätt för Azure Active Directory-grupper som överväger hello perspektiv hello administratörsanvändaren vill toogrant hello detaljerade åtkomst för team eller hela avdelningar inte individuellt för varje användare, vilket med tanke på det som ett mycket tid och hantering av effektivt alternativ. tooillustrate det här exemplet, hello **deltagare** roll har lagts till tooone hello grupper i hello-klient på hello prenumerationsnivå.





![Lägg till RBAC-rollen för AAD-grupper](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

Dessa grupper är säkerhetsgrupper som etableras och hanteras i Azure Active Directory.

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-powershell"></a>Skapa en anpassad RBAC rollen tooopen support-begäranden med PowerShell
hello inbyggda RBAC roller som är tillgängliga i Azure kontrollera vissa behörighetsnivåer baserat på hello tillgängliga resurser i hello-miljö. Om ingen av dessa roller passar hello administratörsanvändare, är det dock hello alternativet toolimit åtkomst ännu mer genom att skapa anpassade RBAC-roller.

När du skapar anpassade RBAC-roller måste tootake en inbyggd roll, redigera och importera den tillbaka i hello-miljö. hello hämtningen och överföring av hello rollen hanteras med PowerShell eller CLI.

Det är viktigt toounderstand hello förutsättningar för att skapa en anpassad roll som kan ge detaljerade åtkomst på hello prenumerationsnivå och även att hello uppmanas användaren hello flexibilitet att öppna supportärenden.

För exempel hello inbyggda rollen **Reader** vilket gör att användare åtkomst tooview alla hello resurs scope men inte tooedit dem eller skapa nya har anpassat tooallow hello användaren hello möjligheten att öppna supportärenden.

Hej första åtgärd för att exportera hello **Reader** roll måste toobe slutförts i PowerShell kördes med förhöjda behörigheter som administratör.

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![PowerShell skärmbild för läsare RBAC roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

Du måste sedan tooextract hello JSON-mall för hello roll.





![JSON-mall för anpassad Reader RBAC-roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

En typisk RBAC-rollen består av tre huvudavsnitt **åtgärder**, **NotActions** och **AssignableScopes**.

I hello **åtgärd** avsnitt visas alla hello tillåtna åtgärder för den här rollen. Det är viktigt toounderstand som tilldelas varje åtgärd från en resursleverantör. I detta fall för att skapa stöd biljetter hello **Microsoft.Support** resursprovidern måste anges.

toobe kan toosee alla hello resursproviders tillgänglig och registrerade i din prenumeration kan du använda PowerShell.
```
Get-AzureRMResourceProvider

```
Dessutom kan du hello alla hello tillgängliga PowerShell-cmdlets toomanage hello providrar.
    ![PowerShell skärmbild för providern resurshantering](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)

alla hello åtgärder för en viss RBAC roll resurs providers visas under hello avsnittet toorestrict **NotActions**.
Senast, är det obligatoriska hello RBAC rollen innehåller hello explicit prenumerations-ID: N där den används. hello prenumerations-ID: N visas under hello **AssignableScopes**, annars du tillåts inte tooimport hello roll i din prenumeration.

När du skapar och anpassar hello RBAC roll, måste den ha toobe importerade tillbaka hello miljö.

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

I det här exemplet är hello eget namn för den här rollen RBAC ”Reader stöd biljetter åtkomstnivå” tillåta hello användaren tooview allt i hello prenumeration och tooopen supportärenden.

> [!NOTE]
> hello bara två inbyggda RBAC-roller tillåter hello åtgärd att supportärenden är **ägare** och **deltagare**. För en användare toobe kan tooopen supportärenden, måste han tilldelas en RBAC roll bara definitionsområdet hello prenumeration, eftersom alla supportärenden skapas baserat på en Azure-prenumeration.

Den här nya anpassade rollen har tilldelats tooan användaren från hello samma katalog.





![Skärmbild av anpassade RBAC-roll som importeras i hello Azure-portalen](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![Skärmbild av tilldela anpassade importerade RBAC rollen toouser i hello samma katalog](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![Skärmbild av behörigheter för anpassad importerade RBAC-roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

hello exempel har mer detaljerad tooemphasize hello gränserna för den här anpassade RBAC-rollen på följande sätt:
* Kan skapa nya supportförfrågningar
* Det går inte att skapa den nya resursen scope (till exempel: virtuell dator)
* Det går inte att skapa nya resursgrupper





![Skärmbild av anpassade RBAC roll skapar supportärenden](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![Skärmbild av anpassad RBAC roll inte kan toocreate virtuella datorer](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![Skärmbild av anpassad RBAC roll inte kan toocreate nya RGs](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-azure-cli"></a>Skapa en anpassad RBAC rollen tooopen support-begäranden som använder Azure CLI
Kör på en Mac och utan att behöva åtkomst tooPowerShell, är Azure CLI hello sätt toogo.

hello steg toocreate en anpassad roll är hello detsamma, med hello enda undantaget att med hjälp av CLI hello roll inte kan hämtas i en JSON-mall, men den kan visas i hello CLI.

Det här exemplet har jag valt hello inbyggda rollen för **säkerhetskopiering Reader**.

```

azure role show "backup reader" --json

```





![Visa CLI Skärmbild av rollen säkerhetskopiering läsare](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

Redigera hello roll i Visual Studio när du har kopierat hello proprieties i en JSON-mall hello **Microsoft.Support** resursprovidern har lagts till i hello **åtgärder** avsnitt så att användaren kan öppna supportärenden medan toobe en läsare för hello säkerhetskopieringsvalv. Igen det är att nödvändiga tooadd hello prenumerations-ID där den här rollen ska användas i hello **AssignableScopes** avsnitt.

```

azure role create --inputfile <path>

```





![CLI Skärmbild av Importera anpassad RBAC-roll](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

hello nya rollen är nu tillgängligt i hello Azure-portalen och hello assignation processen är hello samma som hello föregående exempel.





![Azure portal Skärmbild av anpassade RBAC-roll som skapats med hjälp av CLI 1.0](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

Från och med hello är senaste Build 2017 hello Azure Cloud-gränssnittet allmänt tillgänglig. Azure Cloud-gränssnittet är ett komplement tooIDE och hello Azure-portalen. Med den här tjänsten får du ett webbaserat gränssnitt som är autentiserad och värdbaserad i Azure och du kan använda den i stället för CLI installerat på datorn.





![Azure-molnet Shell](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
