---
title: aaaAzure Active Directory gruppbaserade licensiering fler scenarier | Microsoft Docs
description: "Flera scenarier för Azure Active Directory gruppbaserade licensiering"
services: active-directory
keywords: Azure AD-licensiering
documentationcenter: 
author: curtand
manager: femila
editor: piotrci
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/02/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 782b7c9aa1c062a55c1241d69af673466f7a849c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-limitations-and-known-issues-using-groups-toomanage-licensing-in-azure-active-directory"></a>Scenarier, begränsningar och kända problem med hjälp av grupper toomanage licensiering i Azure Active Directory

Använd följande information och exempel toogain större kunskap om Azure Active Directory (AD Azure) gruppbaserade licensiering hello.

## <a name="usage-location"></a>Användningsplats

Vissa Microsoft-tjänster är inte tillgängliga på alla platser. Innan en licens kan tilldelas tooa användare, hello-administratören har toospecify hello **användningsplats** egenskapen hello användaren. I [hello Azure-portalen](https://portal.azure.com), du kan ange i **användare** &gt; **profil** &gt; **inställningar**.

För gruppen licenstilldelning ärver alla användare utan en användningsplats anges hello platsen för hello katalog. Om du har användare på flera platser gör att tooreflect som korrekt i din användarobjekt innan du lägger till användare toogroups med licenser.

> [!NOTE]
> Gruppen licenstilldelning kommer aldrig att ändra ett befintligt plats-värde för användning på en användare. Vi rekommenderar att du alltid användningsplats som en del av din användare skapa flödet i Azure AD (t.ex. via AAD Connect-konfiguration) – som innebär att hello resultatet av licenstilldelning alltid är korrekt och användarna får inga tjänster på platser som inte är tillåtna.

## <a name="use-group-based-licensing-with-dynamic-groups"></a>Använda gruppbaserade licensiering med dynamiska grupper

Du kan använda gruppbaserade licensiering med någon säkerhetsgrupp, vilket innebär att den kan kombineras med dynamiska grupper i Azure AD. Dynamiska grupper kör regler mot användaren objektet attribut tooautomatically Lägg till och ta bort användare från grupper.

Du kan till exempel skapa en dynamisk grupp för vissa produkter som du vill tooassign toousers. Varje grupp fylls av en regel för att lägga till användare med deras attribut, och varje grupp är tilldelade hello licenser som du vill ha tooreceive. Du kan tilldela hello attributet lokalt och synkroniseras med Azure AD eller du kan hantera hello attributet direkt i hello molnet.

Licenser tilldelas toohello användaren strax efter att de har lagts till toohello grupp. När hello attribut ändras hello användaren lämnar hello grupper och hello licenser tas bort.

### <a name="example"></a>Exempel

Överväg att hello exempel på en lokal lösning för Identitetshantering som bestämmer vilka användare ska ha åtkomst tooMicrosoft webbtjänster. Den använder **extensionAttribute1** toostore ett strängvärde som representerar hello licenser hello användaren ska ha. Azure AD Connect synkroniserar den med Azure AD.

Användare kan behöva en licens men inte en annan eller behöver du både. Här är ett exempel där du distribuerar Office 365 Enterprise E5 och Enterprise Mobility + Security (EMS) licenser toousers i grupper:

#### <a name="office-365-enterprise-e5-base-services"></a>Office 365 Enterprise E5: grundläggande tjänster

![Skärmbild av Office 365 Enterprise E5 grundläggande tjänster](media/active-directory-licensing-group-advanced/o365-e5-base-services.png)

#### <a name="enterprise-mobility--security-licensed-users"></a>Enterprise Mobility + Security: licensierade användare

![Skärmbild av Enterprise Mobility + Security licensierade användare](media/active-directory-licensing-group-advanced/o365-e5-licensed-users.png)

I det här exemplet ändrar en användare och deras extensionAttribute1 toohello värdet för `EMS;E5_baseservices;` om du vill hello användaren toohave båda licenser. Du kan göra den här ändringen lokalt. Efter hello ändra synkroniseras med hello molnet, hello användare läggs till automatiskt tooboth grupper och tilldelade licenser.

![Skärmbild som visar hur tooset hello användarens extensionAttribute1](media/active-directory-licensing-group-advanced/user-set-extensionAttribute1.png)

> [!WARNING]
> Var försiktig när du ändrar en befintlig grupp medlemskapsregel. När en regel ändras, hello medlemskap i gruppen hello blir ny utvärdering och användare som inte längre uppfyller hello nya regel kommer att tas bort (användare som fortfarande matchar hello ny regel påverkas inte under den här processen). Dessa användare har licenser tas bort under hello process som kan resultera i förlust av tjänsten eller i vissa fall förlust av data.

> Om du har en stor dynamisk grupp beroende för licenstilldelning bör du verifiera några stora förändringar på en mindre testgrupp innan du tillämpar dem toohello HUVUDGRUPP.

## <a name="multiple-groups-and-multiple-licenses"></a>Flera grupper och flera licenser

En användare kan vara medlem i flera grupper med licenser. Här följer några saker tooconsider:

- Flera licenser aktiverade för hello samma produkt kan överlappa och de resultera i alla tjänster som används toohello användare. hello följande exempel visas två licensiering grupper: *E3 grundläggande tjänster* innehåller hello foundation services toodeploy först tooall användare. Och *E3 utökad services* innehåller ytterligare tjänster (gungning och Planner) toodeploy endast toosome användare. I det här exemplet lades hello användaren tooboth grupper:

  ![Skärmbild av aktiverade tjänster](media/active-directory-licensing-group-advanced/view-enabled-services.png)

  Hello användaren har därför 7 hello 12 tjänster hello produkten aktiverad när du använder endast en licens för den här produkten.

- Du väljer hello *E3* licens visar mer information, inklusive information om vilka grupper orsakade vilka tjänster toobe aktiverat för hello användaren.

  ![Skärmbild av aktiverade tjänster efter grupp](media/active-directory-licensing-group-advanced/view-enabled-service-by-group.png)

## <a name="direct-licenses-coexist-with-group-licenses"></a>Direkt licenser samexistera med grupp-licenser

När en användare ärver en licens från en grupp, kan du ta bort eller ändra denna licenstilldelning i hello Användaregenskaper. Ändringar måste göras i hello gruppen och sedan spridas tooall användare.

Det är möjligt, men tooassign hello samma produktlicensen direkt toohello användare i tillägg toohello ärvt licens. Du kan aktivera ytterligare tjänster från hello produkten för en användare utan att påverka andra användare.

Direkt tilldelade licenser kan tas bort och påverkar inte ärvt licenser. Överväg att hello användare en licens för Office 365 Enterprise E3 ärver från en grupp.

1. Inledningsvis hello användaren ärver hello licens från hello *E3 grundläggande tjänster* grupp, vilket gör att fyra serviceplaner som visas:

  ![Skärmbild av E3 grupp aktiverade tjänster](media/active-directory-licensing-group-advanced/e3-group-enabled-services.png)

2. Du kan välja **tilldela** toodirectly tilldela en E3 licens toohello användare. I det här fallet ska du toodisable alla serviceplaner utom Yammer Enterprise:

  ![Skärmbild av hur tooassign en licens direkt tooa användare](media/active-directory-licensing-group-advanced/assign-license-to-user.png)

3. Därför använder hello användaren fortfarande endast en licens med hello E3 produkten. Men hello direkttilldelning gör hello Yammer Enterprise service för den användaren. Du kan se vilka tjänster är aktiverade som hello gruppmedlemskap jämfört med hello direkttilldelning:

  ![Skärmbild av ärvd jämfört med direkttilldelning](media/active-directory-licensing-group-advanced/direct-vs-inherited-assignment.png)

4. När du använder direkttilldelning tillåts hello följande åtgärder:

  - Yammer Enterprise kan stängas av på hello användarobjektet direkt. Hej **på/av** växla hello bild har aktiverats för den här tjänsten som andra skillnad från toohello tjänsten växlar. Eftersom hello-tjänsten är aktiverad direkt på hello användare, kan den ändras.
  - Ytterligare tjänster kan även aktiveras som en del av hello direkt tilldelad licens.
  - Hej **ta bort** knapp kan vara används tooremove hello direkt licens från hello användare. Du kan se att hello användaren bara har nu hello ärvda gruppen licens och endast hello ursprungliga tjänster förbli aktiverat:

    ![Skärmbild som visar hur tooremove direkt tilldelning](media/active-directory-licensing-group-advanced/remove-direct-license.png)

## <a name="managing-new-services-added-tooproducts"></a>Hantera nya tjänster läggs tooproducts
När Microsoft lägger till en ny tjänst tooa produkt, den kommer att aktiveras som standard i alla grupper toowhich som du har tilldelat hello produktlicensen. Användare i din klient som prenumererar på toonotifications om produktförändringar får e-post i förväg meddela dem om hello kommande service-tillägg.

Som administratör kan du granska alla grupper som påverkas av ändring av hello och vidta åtgärder, t.ex inaktivera hello ny tjänst i varje grupp. Om du har skapat grupper riktad endast vissa tjänster för distribution kan du besöker dessa grupper och se till att nyligen tillagda tjänster är inaktiverade.

Här är ett exempel på hur den här processen kan se ut:

1. Du tilldelats hello *Office 365 Enterprise E5* tooseveral grupper. En av dessa grupper kallas *O365 E5 - endast Exchange* var utformad tooenable endast hello *Exchange Online (planera 2)* tjänsten för dess medlemmar.

2. Du har fått ett meddelande från Microsoft som hello E5 produkten utökas med en ny tjänst - *Microsoft Stream*. När hello tjänsten blir tillgänglig i din klient kan du göra hello följande:

3. Gå toohello [ **Azure Active Directory > licenser > alla produkter** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) och välj *Office 365 Enterprise E5*och välj **licensierad grupper**  tooview en lista över alla grupper med produkten.

4. Klicka på hello grupp du vill att tooreview (i det här fallet *O365 E5 - endast Exchange*). Då öppnas hello **licenser** fliken. När du klickar på hello E5 licens öppnas ett blad som visar en lista över alla aktiverade tjänster.
> [!NOTE]
> Hej *Microsoft Stream* service har lagt till och aktiveras i den här gruppen i tillägg toohello automatiskt *Exchange Online* tjänsten:

  ![Skärmbild av nya tjänster läggs tooa gruppen licens](media/active-directory-licensing-group-advanced/manage-new-services.png)

5. Om du vill att toodisable hello nya tjänsten i den här gruppen klickar du på hello **på/av** växla nästa toohello service och klicka på hello **spara** knappen tooconfirm hello ändringen. Azure AD kommer nu att bearbeta alla användare i hello grupp tooapply hello ändra; alla nya användare läggs till toohello gruppen inte har hello *Microsoft Stream* aktiverad.

  > [!NOTE]
  > Användare kan fortfarande ha hello-tjänsten har aktiverats genom vissa andra licenstilldelning (en annan grupp som de är medlemmar i eller direkt licenstilldelning).

6. Om det behövs, utför hello tilldelas samma steg för andra grupper med den här produkten.

## <a name="use-powershell-toosee-who-has-inherited-and-direct-licenses"></a>Använd PowerShell toosee som har ärvd och dirigera licenser
Du kan använda ett PowerShell-skript toocheck om användarna har en licens som tilldelas direkt eller ärvdes från en grupp.

1. Kör hello `connect-msolservice` cmdlet tooauthenticate och ansluta tooyour klient.

2. `Get-MsolAccountSku`kan vara används toodiscover alla etablerade produktlicenser i hello-klient.

  ![Skärmbild av hello Get-Msolaccountsku cmdlet](media/active-directory-licensing-group-advanced/get-msolaccountsku-cmdlet.png)

3. Använd hello *AccountSkuId* värde för hello-licens som du är intresserad av i med [detta PowerShell-skript](./active-directory-licensing-ps-examples.md#check-if-user-license-is-assigned-directly-or-inherited-from-a-group). Detta genererar en lista över användare som har denna licens med hello information om hur hello tilldelas.

## <a name="use-audit-logs-toomonitor-group-based-licensing-activity"></a>Använd granskningsloggar toomonitor gruppbaserade licensiering aktivitet

Du kan använda [granskningsloggar i Azure AD](./active-directory-reporting-activity-audit-logs.md#audit-logs) toosee all aktivitet relaterade toogroup-baserade licensiering, inklusive:
- vem som har ändrat licenser på grupper
- När hello system har börjat bearbeta en ändring av licens och när den har slutförts
- licens som gjorts tooa användare på grund av en grupp licenstilldelning.

>[!NOTE]
> Granskningsloggar är tillgängliga på de flesta blad i hello Azure Active Directory-delen av hello-portalen. Beroende på var du komma åt dem, kanske filter före tillämpade tooonly visa relevanta toohello sammanhang av hello-bladet. Om du inte ser hello resultat, granska [hello filtreringsalternativ](./active-directory-reporting-activity-audit-logs.md#filtering-audit-logs) eller komma åt hello ofiltrerad granskningsloggar under [ **Azure Active Directory > aktivitet > granskningsloggar** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Audit).

### <a name="find-out-who-modified-a-group-license"></a>Ta reda på vem som utförde en grupp-licens

1. Ange hello **aktiviteten** filtrera för*ange gruppen licens* och klicka på **tillämpa**.
2. hello resultat omfatta alla fall av licenser som anges eller ändras på grupper.
>[!TIP]
> Du kan också skriva hello hello gruppen i hello *mål* filtrera tooscope hello resultat.

3. Klicka på ett objekt i hello listan Visa toosee hello information i vad som har ändrats. Under *ändrade egenskaper* både gamla och nya värdena för hello licenstilldelning anges.

Här är ett exempel på gruppen licens ändringar, med information:

![Ändringar av skärmbild licens](media/active-directory-licensing-group-advanced/audit-group-license-change.png)

### <a name="find-out-when-group-changes-started-and-finished-processing"></a>Ta reda på när ändringar startat och har behandlats klart

När en licens ändras för en grupp, starta Azure AD tillämpas hello ändringar tooall användare.

1. toosee när grupper har börjat bearbeta, ange hello **aktiviteten** filtrera för*starta tillämpa grupp baserad licens toousers*. Observera att hello aktören för hello åtgärden är *Microsoft Azure AD gruppbaserade Licensing* -en system-konto som har använt tooexecute alla ändringar i gruppen licens.
>[!TIP]
> Klicka på ett objekt i hello listan toosee hello *ändrade egenskaper* fältet - visar hello licens ändringar som har hämtats för bearbetning. Detta är användbart om du gör flera ändringar tooa grupp och du inte är säker på vilken bearbetades.

2. På liknande sätt toosee när grupper bearbetat, Använd hello filtervärdet *avslutar arbetet grupp baserad licens toousers*.
>[!TIP]
> I det här fallet hello *ändrade egenskaper* fältet innehåller en sammanfattning av hello resultat – detta är användbart tooquickly kontroll om bearbetning resulterade i fel. Exempel på utdata:
> ```
Modified Properties
...
Name : Result
Old Value : []
New Value : [Users successfully assigned licenses: 6, Users for whom license assignment failed: 0.];
> ```

3. toosee logguppsättningen hello klar för hur en grupp har behandlats, inklusive alla användarändringar hello följande filter:
  - **Initierad av (aktören)**”: Microsoft Azure AD gruppbaserade licensiering”
  - **Datumintervall** (valfritt): anpassade intervallet för när du vet att en specifik grupp har startat och har behandlats klart

Detta exempel på utdata visar hello starta bearbetning, alla resulterande ändringar av användare och hello är klar för bearbetning.

![Ändringar av skärmbild licens](media/active-directory-licensing-group-advanced/audit-group-processing-log.png)

>[!TIP]
> Artiklar för relaterade*ändra användarlicens* visar information om licens ändringar tillämpas tooeach enskilda användare.

## <a name="limitations-and-known-issues"></a>Begränsningar och kända problem

Om du använder gruppbaserade licensiering, är det en bra idé toofamiliarize själv med hello följande lista över kända problem och begränsningar.

- Gruppbaserade licensiering för närvarande stöder inte grupper som innehåller andra grupper (kapslade grupper). Om du använder en tooa kapslade licensgrupp, har bara hello omedelbar första nivån användare medlemmar i hello hello licenser tillämpas.

- hello-funktionen kan endast användas med säkerhetsgrupper. Office-grupper stöds inte för närvarande och du kommer inte att kunna toouse dem i hello licens tilldelningen.

- Hej [administrationsportalen för Office 365](https://portal.office.com ) inte stöder gruppbaserade licensiering. Om en användare som ärver en licens från en grupp, visas denna licens hello administrationsportalen för Office som en vanlig användare-licens. Om du försöker toomodify som licens eller försök tooremove hello licens returnerar hello portal ett felmeddelande. Ärvda grupp licenser kan inte ändras direkt på en användare.

- När en användare tas bort från en grupp och förlorar hello licens, hello serviceplaner från den (till exempel SharePoint Online) licensen ställs tooa **pausad** tillstånd. hello serviceplaner har inte angetts tooa slutlig, inaktiverat tillstånd. Den här försiktighetsåtgärden kan undvika oavsiktlig borttagning av användardata, om en administratör gör fel i grupphantering för medlemskap.

- När licenserna har tilldelats eller ändras för en stor grupp (till exempel 100 000 användare), kan det påverka prestanda. Mer specifikt hello mängden ändringar som genereras av Azure AD-automation kan påverka hello prestandan för directory-synkronisering mellan Azure AD och lokalt system.

- Licens management automation reagerar inte automatiskt tooall typer av ändringar i hello-miljö. Exempelvis kanske du har kört utanför licenser, orsaka vissa användare toobe i ett feltillstånd. toofree in antal för hello tillgängliga platser du ta bort vissa direkt tilldelade licenser från andra användare. Hello system dock inte automatiskt reagera toothis ändringen och åtgärda användare i den feltillstånd.

  Som en lösning toothese typer av begränsningar i går du toohello **grupp** bladet i Azure AD och klicka på **Ombearbeta**. Det här kommandot bearbetar alla användare i gruppen och löser hello fel tillstånd om möjligt.

- Gruppbaserade licensiering registreras inte fel när en licens inte gick att tilldela tooa användare på grund av tooa dubblerade adressen proxykonfiguration i Exchange Online. Dessa användare hoppas över under licenstilldelning. Mer information om hur tooidentify och lösa problemet, se [i det här avsnittet](./active-directory-licensing-group-problem-resolution-azure-portal.md#license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online).

## <a name="next-steps"></a>Nästa steg

toolearn mer information om andra scenarier för licenshantering via gruppbaserade licensiering, se:

* [Vad är gruppbaserade licensiering i Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Tilldela licenser tooa grupp i Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Identifiera och lösa eventuella problem med licens för en grupp i Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Hur toomigrate individuella licensierade användare toogroup-baserade licensiering i Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
