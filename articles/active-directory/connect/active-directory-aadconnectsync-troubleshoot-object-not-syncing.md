---
title: aaaTroubleshoot ett objekt som inte synkroniseras tooAzure AD | Microsoft Docs
description: "Felsöka anledningen till ett objekt inte synkroniserar tooAzure AD."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 81e0a0793a1d5ec76cfcaec6e974726d7854f58e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-tooazure-ad"></a>Felsökning av ett objekt som inte synkroniseras tooAzure AD

Om ett objekt inte synkroniseras som förväntade tooAzure AD, kan det vara på grund av flera skäl. Om du har fått ett e-postmeddelande med fel från Azure AD eller visas hello fel i Azure AD Connect Health, Läs [felsöka export](active-directory-aadconnect-troubleshoot-sync-errors.md) i stället. Men om du felsöker ett problem där hello-objektet inte är i Azure AD, sedan det här avsnittet för dig. Beskriver hur toofind fel i hello lokalt komponenten Azure AD Connect-synkronisering.

toofind hello fel ska toolook på några olika platser i hello följande ordning:

1. Hej [åtgärdsloggar](#operations) för att söka efter fel som identifierats av hello Synkroniseringsmotorn under import och synkronisering.
2. Hej [anslutarplats](#connector-space-object-properties) för att söka efter objekt som saknas och synkroniseringsfel.
3. Hej [metaversum](#metaverse-object-properties) för att söka efter data som är relaterade problem.

Starta [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md) innan du påbörjar de här stegen.

## <a name="operations"></a>Åtgärder
hello operations fliken hello Synchronization Service Manager är där du ska starta felsökningen. hello visar operations hello resultaten från de senaste hello-åtgärder.  
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/operations.png)  

hello övre delen visar alla kör kronisk. Som standard hello operations loggen sparas information om hello senaste sju dagarna, men den här inställningen kan ändras med hello [scheduler](active-directory-aadconnectsync-feature-scheduler.md). Vill du toolook för alla kör som inte visar statusen lyckades. Du kan ändra hello sortering genom att klicka på hello-huvuden.

Hej **Status** kolumnen är hello viktigaste informationen och visar hello mest allvarliga problem för en körning. Här är en kort sammanfattning av hello vanligaste status efter prioritet tooinvestigate (där * ange flera möjliga felsträngar).

| Status | Kommentar |
| --- | --- |
| Stoppad-* |hello kör kunde inte slutföras. Till exempel om hello fjärrsystemet är inte tillgänglig och kan inte kontaktas. |
| stoppats felgränsen |Det finns fler än 5 000 fel. hello kör stoppades automatiskt på grund av toohello många fel. |
| slutförda -\*-fel |hello kör slutfördes, men det finns fel (färre än 5 000) som bör undersökas. |
| slutförda -\*-varningar |hello kör slutfördes, men vissa data är inte i hello förväntat tillstånd. Om du har fel sedan är det här meddelandet vanligtvis bara ett symtom. Du bör inte undersöka varningar förrän du har åtgärdat felen. |
| lyckades |Inga problem. |

När du har valt en rad uppdateras hello nedre tooshow hello information om som körs. toohello längst till vänster i hello längst ned, kanske du har en lista som säger **steg #**. Den här listan visas bara om du har flera domäner i skogen där varje domän som representeras av ett steg. hello domännamn finns under rubriken hello **Partition**. Under **Synkroniseringsstatistik**, du kan hitta mer information om hello antalet ändringar som har bearbetats. Du kan klicka på hello länkar tooget en lista över hello ändras objekt. Om du har objekt med fel felen visas **synkroniseringsfel**.

### <a name="troubleshoot-errors-in-operations-tab"></a>Felsöka i fliken åtgärder
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorsync.png)  
När du har fel är båda hello-objekt på fel och hello själva länkar som ger mer information.

Starta genom att klicka på hello felsträng (**sync regeln-fel-funktionen-utlöst** hello bilden). Först visas en översikt över hello-objektet. toosee hello faktiska fel, klicka på hello **stackspårning**. Den här spårningen innehåller nivå felsökningsinformation för hello-fel.

Du kan högerklicka i hello **information för anropsstacken** väljer **Markera alla**, och **kopiera**. Sedan kan du kopiera hello stack och titta på hello fel i din favorit redigerare, t.ex Anteckningar.

* Om hello fel från **SyncRulesEngine**, och sedan hello information för anropsstacken först har en lista över alla attribut för hello-objekt. Bläddra nedåt tills du ser hello rubriken **InnerException = >**.  
  ![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorinnerexception.png)  
  hello raden efter visar hello fel. I hello bilden ovan är hello-fel från en anpassad Sync regeln Fabrikam skapas.

Om hello fel själva inte ger tillräckligt med information, är det tid toolook på själva hello informationen. Du kan klicka på Länka hello med hello objektidentifierare och fortsätta felsökningen hello [connector utrymme importerade objektet](#cs-import).

## <a name="connector-space-object-properties"></a>Objektegenskaper för anslutarplats
Om du inte har några fel hittades i hello [operations](#operations) fliken hello nästa steg är toofollow hello connector utrymme objekt från Active Directory och toohello metaversum tooAzure AD. I den här sökvägen bör du hitta där hello problemet är.

### <a name="search-for-an-object-in-hello-cs"></a>Söka efter ett objekt i hello CS

I **Synchronization Service Manager**, klickar du på **kopplingar**, Välj hello Active Directory-koppling och **söka Anslutarplats**.

I **omfång**väljer **RDN** (när du vill toosearch på hello CN-attribut) eller **DN eller fästpunkt** (när du vill toosearch på hello distinguishedName attribut). Ange ett värde och klicka på **Sök**.  
![Söka Anslutarplats](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearch.png)  

Om du inte hittar hello objektet du letar efter, och sedan den kanske har filtrerats med [domänbaserade filtreringen](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) eller [OU-baserade filtrering](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering). Läs hello [filtreringen](active-directory-aadconnectsync-configure-filtering.md) avsnittet tooverify som hello filtrering är korrekt konfigurerat.

En annan användbar sökning är tooselect hello Azure AD-koppling i **omfång** Välj **väntande Import**, och välj hello **Lägg till** kryssrutan. Den här sökningen ger alla synkroniserade objekt i Azure AD kan inte kopplas till ett lokalt objekt.  
![Kopplingen utrymme Sök rader](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearchorphan.png)  
Dessa objekt har skapats av en annan Synkroniseringsmotorn eller en Synkroniseringsmotorn med en annan filtrering konfiguration. Den här vyn visas en lista över **rader** objekt som inte längre hanteras. Du bör granska listan och ta bort dessa objekt med hjälp av hello [Azure AD PowerShell](http://aka.ms/aadposh) cmdlets.

### <a name="cs-import"></a>CS-Import
När du öppnar ett cs-objekt, finns det flera flikar hello överst. Hej **importera** visar hello data som mellanlagras efter en import.  
![CS-objekt](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/csobject.png)    
Hej **Gammalt värde** visar vad som är lagrad i Connect och hello **nytt värde** vad har tagits emot från källsystemet hello och har inte tillämpats ännu. Om det finns ett fel på hello-objekt kan bearbetas inte ändringar.

**Fel**  
![CS-objekt](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssyncerror.png)  
Hej **synkroniseringsfel** visas endast om det är problem med hello-objekt. Mer information finns i [felsöka synkroniseringsfel](#troubleshoot-errors-in-operations-tab).

### <a name="cs-lineage"></a>CS härkomst
hello härkomst fliken visar hur hello utrymme kopplingsobjektet relaterade toohello metaversumobjekt. Du kan se när hello Connector importeras senast en ändring från hello anslutna system och vilka regler som används toopopulate data i hello metaversum.  
![CS härkomst](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineage.png)  
I hello **åtgärd** kolumn och, du kan se en **inkommande** synkroniseringsregel med hello åtgärd **etablera**. Värde som anger så länge objektet connector utrymme finns kvar hello metaversumobjekt. Om hello lista över regler för synkronisering i stället visas en synkroniseringsregel med riktning **utgående** och **etablera**, indikerar det att objektet tas bort när hello metaversumobjekt tas bort.  
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineageout.png)  
Du kan också se i hello **PasswordSync** kolumn som hello inkommande anslutningsplatsen kan bidra ändringar toohello lösenord eftersom en synkroniseringsregel har hello värdet **SANT**. Det här lösenordet skickas tooAzure AD via hello utgående regel.

På hello härkomst fliken kan du få toohello metaversum genom att klicka på [metaversum objektegenskaper](#mv-attributes).

Längst ned hello i alla flikar finns två knappar: **Preview** och **loggen**.

### <a name="preview"></a>Förhandsversion
hello förhandsgranskningen är används toosynchronize ett enskilt objekt. Det är användbart om du felsöker vissa regler för synkronisering av anpassade och vill toosee hello effekten av en ändring i ett enda objekt. Du kan välja mellan **fullständig synkronisering** och **Deltasynkronisering**. Du kan också välja mellan **Generera förhandsgranskning**, som upprätthåller bara hello ändring i minnet, och **genomför Preview**, som uppdateras hello metaversum och alla steg ändras tootarget kopplingens utrymmen.  
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/preview.png)  
Du kan inspektera hello objektet och vilken regel tillämpas för en viss attributflöde.  
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/previewresult.png)

### <a name="log"></a>Logga
hello är loggen används toosee hello lösenord sync status och historik. Mer information finns i [Felsöka Lösenordssynkronisering](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="metaverse-object-properties"></a>Objektet metaversumegenskaper
Det är oftast bättre toostart söka från hello källa Active Directory [anslutarplats](#connector-space). Men du kan också starta sökningen från hello metaversum.

### <a name="search-for-an-object-in-hello-mv"></a>Sök efter ett objekt i hello MV
I **Synchronization Service Manager**, klickar du på **metaversumsökningen**. Skapa en fråga som du vet hittar hello användare. Du kan söka efter vanliga attribut, till exempel accountName (SAM) och userPrincipalName. Mer information finns i [metaversumsökningen](active-directory-aadconnectsync-service-manager-ui-mvsearch.md).
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvsearch.png)  

I hello **sökresultat** fönster, klicka på hello-objekt.

Om du inte kan hitta hello objekt, har sedan den ännu inte nått hello metaversum. Fortsätt toosearch för hello objektet i hello Active Directory [anslutarplats](#connector-space-object-properties). Det kan finnas ett fel i synkronisering som blockerar hello objekt från kommande toohello metaversum eller det kan finnas ett filter.

### <a name="mv-attributes"></a>MV-attribut
Du kan se hello värden och vilka Connector bidragit den hello attribut på fliken.  
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvobject.png)  

Om ett objekt inte synkroniserar titta på följande attribut i hello metaversum hello:
- Är hello attribut **cloudFiltered** finns och ange för**SANT**? Om det är sedan har filtrerats enligt toohello stegen i [attributet baserat filtrering](active-directory-aadconnectsync-configure-filtering.md#attribute-based-filtering).
- Är hello attribut **sourceAnchor** finns? Om inte, har du ett konto-resurs skogstopologi? Om ett objekt som identifieras som en länkad postlåda (hello attributet **msExchRecipientTypeDetails** har hello värdet 2), och sedan hello sourceAnchor bidragit med hello skog med ett aktiverat Active Directory-konto. Kontrollera att hello huvudkontot har importerats och synkroniseras korrekt. hello master konto måste anges i hello [kopplingar](#mv-connectors) för hello-objektet.

### <a name="mv-connectors"></a>MV-kopplingar
hello kopplingar fliken visas alla kopplingens utrymmen som har en representation av hello-objektet.  
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvconnectors.png)  
Du bör ha en koppling till:

- Varje användare i Active Directory-skog hello representeras i. Detta kan inkludera foreignSecurityPrincipals och kontaktobjekt.
- En koppling i Azure AD.

Om du saknar hello connector tooAzure AD läser [MV attribut](#MV-attributes) tooverify hello kriterier för att etablera tooAzure AD.

Den här fliken kan du också toonavigate toohello [utrymme kopplingsobjektet](#connector-space-object-properties). Välj en rad och klicka på **egenskaper**.

## <a name="next-steps"></a>Nästa steg
Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
