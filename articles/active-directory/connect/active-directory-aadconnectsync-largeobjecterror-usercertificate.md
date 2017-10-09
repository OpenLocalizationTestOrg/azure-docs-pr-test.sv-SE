---
title: 'Azure AD Connect-synkronisering: hantera LargeObject fel som orsakats av userCertificate attributet | Microsoft Docs'
description: "Det här avsnittet innehåller steg hello för LargeObject fel som orsakats av userCertificate attribut."
services: active-directory
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 146ad5b3-74d9-4a83-b9e8-0973a19828d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e547fb5f601b92f6f5154de9997651b1f28c51b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-handling-largeobject-errors-caused-by-usercertificate-attribute"></a>Azure AD Connect-synkronisering: hantera LargeObject fel som orsakats av userCertificate attribut

Azure AD tillämpar övre gräns **15** certifikat värden på hello **userCertificate** attribut. Om Azure AD Connect exporterar ett objekt med mer än 15 värden tooAzure AD, Azure AD returnerar en **LargeObject** fel med meddelandet:

>*”Hej objektet är för stor. Trim hello antalet attributvärden för det här objektet. hello åtgärden måste göras i hello nästa synkroniseringscykel... ”*

Hej LargeObject fel kan orsakas av andra AD-attribut. tooconfirm orsakas verkligen av hello userCertificate attribut, behöver du tooverify mot hello-objektet antingen i lokala AD eller hello [Synchronization Service Manager metaversumsökningen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-mvsearch).

tooobtain hello listan med objekt i din klient med LargeObject fel med någon av följande metoder hello:

 * Om din klient är aktiverat för Azure AD Connect Health för synkronisering, kan du läsa toohello [synkronisering felrapporten](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-sync#object-level-synchronization-error-report-preview) tillhandahålls.
 
 * hello e-postmeddelande för directory synkroniseringsfel som skickas hello slutet av varje cykel för synkronisering har hello lista med objekt med LargeObject fel. 
 * Hej [Synchronization Service Manager Operations fliken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-operations) visar hello lista över objekt med LargeObject fel om du klickar på hello senaste tooAzure AD exportåtgärden.
 
## <a name="mitigation-options"></a>Alternativ för lösning
Tills hello LargeObject fel har åtgärdats exporteras tooAzure AD andra toohello för ändringar av attributet inte får vara samma objekt. tooresolve hello fel, kan du hello följande alternativ:

 * Uppgradera Azure AD Connect toobuild 1.1.524.0 eller efter. Skapa 1.1.524.0 i Azure AD Connect, hello out-of-box Synkroniseringsregler har uppdaterats toonot export attribut userCertificate och userSMIMECertificate om hello attribut har mer än 15 värden. Mer information om hur tooupgrade Azure AD Connect finns tooarticle [Azure AD Connect: uppgradera från en tidigare version toohello senaste](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

 * Implementera en **utgående synkroniseringsregel** i Azure AD Connect som exporterar en **null-värde i stället för hello faktiska värden för objekt med mer än 15 certifikat värden**. Det här alternativet är lämpligt om du inte behöver någon hello certifikat värden toobe exporteras tooAzure AD för objekt med mer än 15 värden. Mer information om hur tooimplement synkroniseringen regeln, se avsnittet toonext [implementera sync regeln toolimit export av userCertificate attributet](#implementing-sync-rule-to-limit-export-of-usercertificate-attribute).

 * Minska hello antal certifikat värden på hello lokala AD-objekt (15 eller mindre) genom att ta bort värden som inte längre används av din organisation. Detta är lämpligt om hello attributet överdriven storlek orsakas av certifikat som har upphört att gälla eller används inte. Du kan använda hello [PowerShell-skript finns här](https://gallery.technet.microsoft.com/Remove-Expired-Certificates-0517e34f) toohelp hitta säkerhetskopiera och ta bort utgångna certifikat i din lokala AD. Innan du tar bort hello certifikat, bör du kontrollera med hello infrastruktur för offentliga nycklar administratörer i din organisation.

 * Konfigurera Azure AD Connect tooexclude hello userCertificate attribut från att exporteras tooAzure AD. I allmänhet rekommenderas inte det här alternativet eftersom hello-attributet kan användas av Microsoft Online Services tooenable specifika scenarier. I synnerhet:
    * hello userCertificate attribut för användarobjektet hello används av Exchange Online och Outlook-klienter för Meddelandesignering och kryptering. toolearn mer om den här funktionen finns tooarticle [S/MIME för Meddelandesignering och kryptering](https://technet.microsoft.com/library/dn626158(v=exchg.150).aspx).

    * hello userCertificate attributet på hello datorobjekt används av Azure AD tooallow Windows 10 lokalt domänanslutna enheter tooconnect tooAzure AD. toolearn mer om den här funktionen finns tooarticle [ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy).

## <a name="implementing-sync-rule-toolimit-export-of-usercertificate-attribute"></a>Implementera sync regeln toolimit export av userCertificate attribut
tooresolve hello LargeObject orsakas felet av hello userCertificate attribut, du kan implementera en synkroniseringsregel för utgående i Azure AD Connect som exporterar en **null-värde i stället för hello faktiska värden för objekt med mer än 15 certifikat värden**. Det här avsnittet beskrivs synkroniseringsregel för hello steg krävs tooimplement hello för **användaren** objekt. hello steg kan anpassas för **Kontakta** och **datorn** objekt.

> [!IMPORTANT]
> Exportera nullvärde tar bort certifikatet värden som tidigare har exporterats tooAzure AD.

hello steg kan sammanfattas som:

1. Inaktivera sync scheduler och kontrollera att det finns ingen synkronisering pågår.
3. Hitta hello befintliga utgående synkroniseringsregel för userCertificate attribut.
4. Skapa hello utgående synkroniseringsregel krävs.
5. Kontrollera hello nya synkroniseringsregel på ett befintligt objekt med LargeObject fel.
6. Tillämpa hello nya sync regeln tooremaining objekt med LargeObject fel.
7. Kontrollera att inga ändringar har gjorts oväntat väntar på toobe exporteras tooAzure AD.
8. Exportera hello ändringar tooAzure AD.
9. Återaktivera sync scheduler.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Steg 1. Inaktivera sync scheduler och kontrollera att det finns ingen synkronisering pågår
Se till att ingen synkronisering sker när du arbetar med hello i mitten av en ny synkronisering regeln tooavoid oavsiktliga ändringar som exporteras tooAzure AD. toodisable hello inbyggda sync scheduler:
1. Starta PowerShell-session på hello Azure AD Connect-servern.

2. Inaktivera schemalagda synkroniseringen genom att köra cmdlet:`Set-ADSyncScheduler -SyncCycleEnabled $false`

> [!Note]
> hello är föregående steg endast tillämplig toonewer versioner (1.1.xxx.x) av Azure AD Connect med hello inbyggda Schemaläggaren. Om du använder äldre versioner (1.0.xxx.x) av Azure AD Connect som använder Schemaläggaren i Windows eller du använder egna anpassade scheduler (inte vanliga) tootrigger återkommande synkronisering, måste toodisable dem i enlighet med detta.

3. Starta hello **Synchronization Service Manager** genom att gå tooSTART → synkroniseringstjänsten.

4. Gå toohello **Operations** fliken och bekräfta att ingen åtgärd vars status är *”pågår”.*

### <a name="step-2-find-hello-existing-outbound-sync-rule-for-usercertificate-attribute"></a>Steg 2. Hitta hello befintliga utgående synkroniseringsregel för userCertificate attribut
Det bör finnas en befintlig synkroniseringsregel som har aktiverats och konfigurerats tooexport userCertificate attribut för användaren objekt tooAzure AD. Leta upp den här synkroniseringen regeln toofind ut dess **prioritet** och **målgrupp filter** konfiguration:

1. Starta hello **Synchronization regler Editor** genom att gå tooSTART → synkronisering regler Editor.

2. Konfigurera hello sökfilter med hello följande värden:

    | Attribut | Värde |
    | --- | --- |
    | Riktning |**Utgående** |
    | MV-objekttyp |**Person** |
    | koppling |*namnet på Azure AD-koppling* |
    | Objekttyp för kopplingen |**användaren** |
    | MV-attribut |**userCertificate** |

3. Om du använder OOB (out of box) sync regler tooAzure AD connector tooexport userCertficiate attribut för användarobjekt du bör få tillbaka hello *”ut tooAAD – användaren ExchangeOnline”* regeln.
4. Skriv ner hello **prioritet** värdet för den här regeln för synkronisering.
5. Välj hello synkroniseringsregel och klicka på **redigera**.
6. I hello *”redigera reserverade regeln bekräftelse”* popup-fönstret klickar du på **nr**. (Oroa dig inte, vi inte toomake Standardregeln ändra toothis sync).
7. Välj hello i hello redigera skärmen **Scoping filter** fliken.
8. Notera hello scope filterkonfiguration. Om du använder hello OOB-synkroniseringsregel det exakt ska **en målgrupp filter-grupp som innehåller två satser**, inklusive:

    | Attribut | Operatorn | Värde |
    | --- | --- | --- |
    | sourceObjectType | LIKA MED | Användare |
    | cloudMastered | NOTEQUAL | True |

### <a name="step-3-create-hello-outbound-sync-rule-required"></a>Steg 3. Skapa hello utgående synkroniseringsregel krävs
hello ny synkroniseringsregel för måste ha hello samma **målgrupp filter** och **högre prioritet** än hello befintliga sync-regeln. Detta säkerställer att hello nya sync regeln gäller toohello samma uppsättning objekt som hello befintliga synkronisering och åsidosättningar hello befintliga sync-regel för hello userCertificate attribut. synkroniseringsregel för toocreate hello:
1. Hello synkronisering regler redigeraren, klicka på hello **Lägg till ny regel** knappen.
2. Under hello **fliken Beskrivning**, ange hello följande konfiguration:

    | Attribut | Värde | Information |
    | --- | --- | --- |
    | Namn | *Ange ett namn* | T.ex. *”ut tooAAD – anpassad åsidosätta för userCertificate”* |
    | Beskrivning | *Ange en beskrivning* | T.ex. *”userCertificate attributet har mer än 15 värden, exportera NULL”.* |
    | Det anslutna systemet | *Välj hello Azure AD-koppling* |
    | Anslutna System objekttyp | **användaren** | |
    | Typ av Metaversumobjekt | **person** | |
    | Länktypen | **Anslut dig** | |
    | Prioritet | *Om du har valt ett tal mellan 1 och 99* | hello antalet valda får inte användas av någon befintlig synkroniseringsregel och har ett lägre värde (och därför högre prioritet) än hello befintliga sync-regeln. |

3. Gå toohello **Scoping filter** fliken och implementera hello med hjälp av samma målgrupp hello befintliga sync regelns bearbetningsordning.
4. Hoppa över hello **ansluta regler** fliken.
5. Gå toohello **transformationer** fliken tooadd ett nytt omvandling med följande konfiguration:

    | Attribut | Värde |
    | --- | --- |
    | Flöde |**Uttryck** |
    | Målattribut |**userCertificate** |
    | Källattributet |*Använd hello efter uttrycket*:`IIF(IsNullOrEmpty([userCertificate]), NULL, IIF((Count([userCertificate])> 15),AuthoritativeNull,[userCertificate]))` |
    
6. Klicka på hello **Lägg till** knappen toocreate hello synkroniseringsregel.

### <a name="step-4-verify-hello-new-sync-rule-on-an-existing-object-with-largeobject-error"></a>Steg 4. Kontrollera hello nya synkroniseringsregel på ett befintligt objekt med LargeObject fel
Detta är tooverify som hello sync regeln som skapade fungerar på ett befintligt AD-objekt med LargeObject fel innan du installerar tooother objekt:
1. Gå toohello **Operations** fliken i hello Synchronization Service Manager.
2. Välj hello senaste tooAzure AD exportåtgärden och klicka på ett hello-objekt med LargeObject fel.
3.  I hello objektegenskaper för Anslutarplats popup-skärmen, klickar du på hello **Preview** knappen.
4. I hello Preview popup-skärmen, Välj **fullständig synkronisering** och på **genomför Preview**.
5. Stäng hello Preview skärmen och objektegenskaper för Anslutarplats hello-skärmen.
6. Gå toohello **kopplingar** fliken i hello Synchronization Service Manager.
7. Högerklicka på hello **Azure AD** koppling och välj **kör...**
8. I hello kör Connector popup-fönstret väljer **exportera** steg och på **OK**.
9. Vänta tills exporten tooAzure AD toocomplete och bekräfta att det finns ingen mer LargeObject fel på det markerade objektet.

### <a name="step-5-apply-hello-new-sync-rule-tooremaining-objects-with-largeobject-error"></a>Steg 5. Tillämpa hello nya sync regeln tooremaining objekt med LargeObject fel
När hello synkroniseringsregel har lagts till, behöver du toorun en fullständig synkronisering steg på hello AD-koppling:
1. Gå toohello **kopplingar** fliken i hello Synchronization Service Manager.
2. Högerklicka på hello **AD** koppling och välj **kör...**
3. I hello kör Connector popup-fönstret väljer **fullständig synkronisering** steg och på **OK**.
4. Vänta tills hello fullständig synkronisering steg toocomplete.
5. Upprepa hello ovanstående steg för hello återstående AD kopplingar om du har flera AD-kopplingar. Vanligtvis krävs flera kopplingar om du har flera lokala kataloger.

### <a name="step-6-verify-there-are-no-unexpected-changes-waiting-toobe-exported-tooazure-ad"></a>Steg 6. Kontrollera att inga ändringar har gjorts oväntat väntar på toobe exporteras tooAzure AD
1. Gå toohello **kopplingar** fliken i hello Synchronization Service Manager.
2. Högerklicka på hello **Azure AD** koppling och välj **söka Anslutarplats**.
3. I hello Sök anslutningsplatsen popup-fönstret:
    1. Ange omfattning för**väntande exportera**.
    2. Markera kryssrutorna för alla 3, inklusive **Lägg till**, **ändra** och **ta bort**.
    3. Klicka på **Sök** knappen tooreturn alla användarobjekt med ändringar som väntar på toobe exporteras tooAzure AD.
    4. Kontrollera att inga ändringar har gjorts oväntat. tooexamine hello ändringar för ett angivet objekt, dubbelklicka på hello-objektet.

### <a name="step-7-export-hello-changes-tooazure-ad"></a>Steg 7. Exportera hello ändringar tooAzure AD
tooexport hello ändringar tooAzure AD:
1. Gå toohello **kopplingar** fliken i hello Synchronization Service Manager.
2. Högerklicka på hello **Azure AD** koppling och välj **kör...**
4. I hello kör Connector popup-fönstret väljer **exportera** steg och på **OK**.
5. Vänta tills exporten tooAzure AD toocomplete och bekräfta att det inte finns några fler LargeObject-fel.

### <a name="step-8-re-enable-sync-scheduler"></a>Steg 8. Återaktivera sync scheduler
Nu när hello problemet är löst återaktivera hello inbyggda sync scheduler:
1. Starta PowerShell-session.
2. Aktivera schemalagd synkronisering igen genom att köra cmdlet:`Set-ADSyncScheduler -SyncCycleEnabled $true`

> [!Note]
> hello är föregående steg endast tillämplig toonewer versioner (1.1.xxx.x) av Azure AD Connect med hello inbyggda Schemaläggaren. Om du använder äldre versioner (1.0.xxx.x) av Azure AD Connect som använder Schemaläggaren i Windows eller du använder egna anpassade scheduler (inte vanliga) tootrigger återkommande synkronisering, måste toodisable dem i enlighet med detta.

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

