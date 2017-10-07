---
title: "Azure AD Connect-synkronisering: gjort en konfigurationsändring i Azure AD Connect-synkronisering | Microsoft Docs"
description: "Vägleder dig igenom hur toomake en ändra toohello konfiguration i Azure AD Connect synkroniserar."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 78e96d9166831a668439c2b8aa6a0022bc472da4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-how-toomake-a-change-toohello-default-configuration"></a>Azure AD Connect-synkronisering: hur toomake en ändring toohello standard konfiguration
hello syftet med det här avsnittet är toowalk igenom hur toomake ändras toohello standardkonfigurationen i Azure AD Connect-synkronisering. Den innehåller steg för några vanliga scenarier. Med den kunskapen ska kunna toomake några enkla förändringar tooyour äger konfiguration baserat på dina egna regler.

## <a name="synchronization-rules-editor"></a>Redigeraren för regler för synkronisering
hello Synchronization regler Editor är används toosee och ändrar hello standardkonfigurationen. Du hittar i hello Start-menyn under hello **Azure AD Connect** grupp.  
![Start-menyn med synkronisering Rule Editor](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

När du öppnar den finns hello standardregler för out-of-box.

![Synkronisera Rule Editor](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-hello-editor"></a>Navigera i hello-redigeraren
hello-listrutor hello överst i Redigeraren för hello Tillåt tooquickly söka efter en viss regel. Om du vill toosee hello regler där hello attributet proxyAddresses inkluderas kan ändrar du till exempel hello-listrutor toohello följande:  
![SRE filtrering](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Tryck på tooreset filtrering och läsa in en ny konfiguration **F5** på hello tangentbord.

toohello längst upp till höger som du har en knapp **Lägg till ny regel**. Den här knappen är används toocreate en egen anpassad regel.

Hello längst ned i ha knappar för fungerar på en vald synkroniseringsregel. **Redigera** och **ta bort** göra vad du förväntade dig. **Exportera** genererar ett PowerShell-skript för att återskapa hello synkroniseringsregel. Den här proceduren kan du toomove en synkroniseringsregel från en server tooanother.

## <a name="create-your-first-custom-rule"></a>Skapa din första anpassad regel
hello vanligaste ändringen är ändringar toohello attributflöden. hello data i källkatalogen kanske inte som Azure AD. I hello exemplet i det här avsnittet som du vill att hello förnamn för en användare är alltid i toomake **rätt skiftläge**.

### <a name="disable-hello-scheduler"></a>Inaktivera hello Schemaläggaren
Hej [scheduler](active-directory-aadconnectsync-feature-scheduler.md) körs var 30: e minut som standard. Vill du toomake att den inte startar när du gör ändringar och felsöka din nya regler. tootemporarily inaktiverar hello Schemaläggaren, starta PowerShell och kör`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Inaktivera hello Schemaläggaren](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-hello-rule"></a>Skapa hello regel
1. Klicka på **Lägg till ny regel**.
2. På hello **beskrivning** sidan Ange hello följande:  
   ![Inkommande regel filtrering](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
   * Namn: Ge hello regeln ett beskrivande namn.
   * Beskrivning: Vissa information så att någon annan kan förstå vilken hello regeln gäller.
   * Anslutna system: hello system hello-objektet finns i. I det här fallet Välj hello Active Directory-kopplingen.
   * Anslutna System/metaversum-objekttyp: Välj **användare** och **Person** respektive.
   * Länktypen: Ändra värdet för**ansluta**.
   * Prioritet: Ange ett värde som är unikt i hello system. Ett lägre numeriskt värde anger högre prioritet.
   * Tagg: Lämna tomt. Endast out-of-box regler från Microsoft ska ha den här rutan fylls i med ett värde.
3. På hello **Scoping filter** anger **givenName ISNOTNULL**.  
   ![Regel för inkommande trafik Omfångsfilter](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
   Det här avsnittet är används toodefine vilka objekt hello regeln ska tillämpas på. Om tomt skulle hello regeln tooall användarobjekt. Men som även konferensrum, tjänstkonton och andra icke personer användarobjekt.
4. På hello **ansluta regler**, lämna det tomt.
5. På hello **transformationer** ändrar hello ExchangeRate för FlowType för**uttryck**. Välj hello målattribut **givenName**, och ange i källan `PCase([givenName])`.
   ![Regel för inkommande trafik omvandlingar](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
   Hej Synkroniseringsmotorn är skiftlägeskänslig både hello funktionsnamn och hello hello attributets namn. Om du skriver något fel, se en varning när du lägger till hello regeln. hello editor kan du toosave och fortsätta, så har du tooreopen hello och rätt hello-regel.
6. Klicka på **Lägg till** toosave hello regeln.

Den nya anpassa regeln ska visas med hello andra synkronisera regler i hello system.

### <a name="verify-hello-change"></a>Kontrollera hello ändring
Med den här nya ändringar vill du toomake till att den fungerar som väntat och inte som utlöste eventuella fel. Beroende på hello antalet objekt som du har, finns det två olika sätt toodo det här steget.

1. Kör en fullständig synkronisering för alla objekt
2. Kör en förhandsgranskning och en fullständig synkronisering på ett objekt

Starta **synkroniseringstjänsten** hello start-menyn. hello stegen i det här avsnittet finns i det här verktyget.

1. **Fullständig synkronisering för alla objekt**  
   Välj **kopplingar** hello överst. Identifiera hello Connector som du har gjort en ändring tooin hello föregående avsnitt i det här fallet hello Active Directory Domain Services och markera den. Välj **kör** åtgärder och välj **fullständig synkronisering** och **OK**.
   ![Fullständig synkronisering](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
   hello-objekt har uppdaterats i hello metaversum. Nu vill toolook på hello objekt i hello metaversum.
2. **Förhandsgranskning och fullständig synkronisering på ett objekt**  
   Välj **kopplingar** hello överst. Identifiera hello Connector som du har gjort en ändring tooin hello föregående avsnitt i det här fallet hello Active Directory Domain Services och markera den. Välj **söka Anslutarplats**. Använda omfång toofind ett objekt som du vill toouse tootest hello ändringen. Välj hello objekt och klicka på **Preview**. Välj i nya hello-skärmen **genomför Preview**.  
   ![Genomför preview](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
   hello ändringen är nu allokerade toohello metaversum.

**Titta på hello objekt i hello metaversum**  
Nu vill toopick några objekt toomake att hello exempelvärde förväntas och hello regeln tillämpas. Välj **metaversumsökningen** hello uppifrån. Lägg till eventuella filter som du behöver toofind hello relevanta objekt. Öppna ett objekt från hello sökresultatet. Titta på hello attributvärden och Kontrollera också i hello **Sync regler** kolumn som hello regeln tillämpas som förväntat.  
![Metaversumsökning](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  

### <a name="enable-hello-scheduler"></a>Aktivera hello Schemaläggaren
Om allt är som förväntat, kan du aktivera hello Schemaläggaren igen. Kör från PowerShell `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Andra vanliga flödet attributändringar
hello föregående avsnitt beskrivs hur toomake ändras tooan attributflöde. I det här avsnittet finns ytterligare exempel. hello anvisningar för hur toocreate hello synkroniseringsregel förkortas, men du kan hitta hello fullständig steg hello föregående avsnitt.

### <a name="use-another-attribute-than-hello-default"></a>Använd ett annat attribut än hello som standard
Det finns en skog där hello lokala alfabetet används för förnamn, efternamn och namn på Fabrikam. hello latinska teckenrepresentation av dessa attribut finns i hello tilläggsattribut. När du skapar hello globala adresslista i Azure AD och Office 365 hello organisationen vill dessa attribut toobe som används i stället.

Med en standardkonfiguration ett objekt från hello lokala skogen ser ut så här:  
![Attributflöde 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

toocreate en regel med andra attributflöden hello följande:

* Starta **Synchronization Rule Editor** hello start-menyn.
* Med **inkommande** fortfarande valda toohello vänster, klicka på hello **Lägg till ny regel**.
* Ge hello regeln ett namn och beskrivning. Välj hello lokala Active Directory och hello relevanta objekttyper. I **länktypen**väljer **ansluta**. Välj ett värde som inte används av en annan regel för prioritet. hello out-of-box regler börja med 100, så hello värdet 50 kan användas i det här exemplet.
  ![Attributflöde 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
* Lämna det tomt omfång (det vill säga tillämpas tooall användarobjekt i hello skog).
* Lämna anslutning till regler tomt (med är, vilka hello out-of-box regeln referensen eventuella kopplingar).
* Skapa följande flöden hello i omvandlingar:  
  ![Attributflöde 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
* Klicka på **Lägg till** toosave hello regeln.
* Gå för**Synchronization Service Manager**. På **kopplingar**, Välj hello Connector där vi har lagt till hello regeln. Välj **kör**, och **fullständig synkronisering**. En fullständig synkronisering beräknar alla objekt med aktuella hello-regler.

Detta är hello resultat för samma objekt med den här anpassade regeln hello:  
![Attributflöde 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Längden på attribut
Attribut för anslutningssträngen är som standard set toobe indexeras och hello maximala längden är 448 tecken. Om du arbetar med string-attribut som kan innehålla flera gör du följande för att tooinclude hello i hello attributflöde:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-hello-userprincipalsuffix"></a>Ändra hello userPrincipalSuffix
hello userPrincipalName attribut i Active Directory inte alltid är känt av hello användare och kan inte användas som hello inloggnings-ID. hello Azure AD Connect sync-installationsguiden kan välja ett annat attribut, till exempel e-post. Men i vissa fall hello attributet måste beräknas. Hello företaget Contoso har till exempel två Azure AD-kataloger, en för produktion och en för testning. De vill hello användare i deras test klient toouse en annan suffix i hello inloggnings-ID.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

I det här uttrycket ta allt vänsterkant hello först @-sign (Word) och sammanfoga med en fast sträng.

### <a name="convert-a-multi-value-tooa-single-value"></a>Konvertera ett Flervärde tooa värde
Vissa attribut i Active Directory är flera värden i hello schemat även om de letar enda värden i Active Directory-användare och datorer. Ett exempel är hello beskrivningsattribut.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

I det här uttrycket om hello-attributet har ett värde, ta hello första objektet (objekt) i hello attribut, ta bort inledande och avslutande blanksteg (Trim) och sedan se hello först 448 tecken (till vänster) i hello-sträng.

### <a name="do-not-flow-an-attribute"></a>Flöda inte ett attribut
Bakgrundsinformation om hello scenario för det här avsnittet finns [styra hello attributet flödet processen](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Det finns två sätt toonot flöda ett attribut. hello först finns i installationsguiden för hello och låter dig för[ta bort markerade attributen](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Det här alternativet fungerar om du inte har synkroniserat hello attributet innan. Om du har startat toosynchronize det här attributet och tar bort med den här funktionen sedan hello Synkroniseringsmotorn slutar att hantera hello attribut och hello befintliga värden finns kvar i Azure AD.

Om du vill tooremove hello värdet för ett attribut och kontrollera att den inte flödar i hello framtida måste du skapa en anpassad regel i stället.

På Fabrikam, har vi insåg att vissa av hello attribut som vi synkronisera toohello molnet inte får vara det. Vi vill toomake att attributen tas bort från Azure AD.  
![Felaktig tilläggsattribut](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

* Skapa en ny regel för inkommande synkronisering och fylla hello beskrivning ![beskrivningar](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
* Skapa attributflöden av typen **uttryck** och med hello källan **AuthoritativeNull**. hello literal **AuthoritativeNull** anger att hello värdet ska vara tomt i hello MV även om en lägre prioritet synkroniseringsregel försöker toopopulate hello värde.
  ![Omvandlingen för tilläggsattribut](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
* Spara hello Synkroniseringsregel. Starta **synkroniseringstjänsten**, hitta hello koppling, Välj **kör**, och **fullständig synkronisering**. Det här steget beräknar alla attributflöden.
* Kontrollera att hello avsedd ändras om toobe exporteras genom att söka hello anslutningsplatsen.
  ![Stegvis ta bort](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>Skapa regler med PowerShell
Med hjälp av hello sync Regelredigerare fungerar bra när du bara har några ändringar toomake. Om du behöver toomake många ändringar kan PowerShell vara ett bättre alternativ. Vissa avancerade funktioner är bara tillgängliga med PowerShell.

### <a name="get-hello-powershell-script-for-an-out-of-box-rule"></a>Hämta hello PowerShell-skript för en out-of-box-regel
toosee hello PowerShell-skript som skapats av en out-of-box regeln, väljer hello regeln i hello sync regler redigeraren och klicka på **exportera**. Den här åtgärden ger du hello PowerShell script skapade hello regeln.

### <a name="advanced-precedence"></a>Avancerade prioritet
hello out-of-box sync regler börja med högre prioriteringsvärdet 100. Om du har många skogar och du behöver toomake kanske många egna ändringar och sedan 99 sync regler inte finns tillräckligt med.

Du kan instruera hello Synkroniseringsmotorn som du vill att ytterligare regler som infogas före hello out-of-box-regler. tooget problemet, gör du följande:

1. Markera hello första out-of-box synkroniseringsregel (den här regeln är hello **i från AD-användare ansluta**) i hello sync Regelredigeraren och välj **exportera**. Kopiera hello SR ID-värde.  
![PowerShell före ändringen](./media/active-directory-aadconnectsync-change-the-configuration/powershell1.png)  
2. Skapa hello ny regel för synkronisering. Du kan använda hello sync regeln editor toocreate den. Exportera hello regeln tooa PowerShell-skript.
3. I hello egenskapen **PrecedenceBefore**, infoga hello ID-värde från hello out-of-box regeln. Ange hello **prioritet** för**0**. Kontrollera att hello identifierarattribut är unikt och du inte återanvänder en GUID från en annan regel. Kontrollera också att hello **ImmutableTag** anges inte egenskapen; den här egenskapen får endast anges för en out-of-box-regel. Spara hello PowerShell-skript och köra den. hello resultatet är att en anpassad regel tilldelas hello prioritet värde på 100 och alla andra regler i out-of-box ökas.  
![PowerShell efter ändring](./media/active-directory-aadconnectsync-change-the-configuration/powershell2.png)  

Du kan ha många anpassade sync regler med hjälp av hello samma **PrecedenceBefore** värdet vid behov.


## <a name="enable-synchronization-of-preferreddatalocation"></a>Aktivera synkroniseringen av PreferredDataLocation
Azure AD Connect har stöd för synkronisering av hello **PreferredDataLocation** attribut för **användaren** objekt i version 1.1.524.0 och efter. Följande ändringar har införts mer specifikt:

* hello schemat för hello objekttyp **användaren** i hello Azure AD Connector utökas tooinclude PreferredDataLocation attribut som är av typen string och enkelvärdesattribut.

* hello schemat för hello objekttyp **Person** i hello metaversum utökas tooinclude PreferredDataLocation attribut som är av typen string och enkelvärdesattribut.

Som standard aktiveras inte hello PreferredDataLocation attribut för synkronisering eftersom det inte finns något motsvarande attribut för PreferredDataLocation i lokala Active Directory. Du måste manuellt Aktivera synkronisering.

> [!IMPORTANT]
> Azure AD kan för närvarande hello PreferredDataLocation attributet på både synkroniserade objekt och i molnet användaren objekt toobe direkt konfigureras med Azure AD PowerShell. När du har aktiverat synkronisering av hello PreferredDataLocation attribut, måste du sluta använda Azure AD PowerShell tooconfigure hello attributet på **synkroniseras användarobjekt** som Azure AD Connect åsidosätter dem baserat på hello källa attributvärden i lokala Active Directory.

> [!IMPORTANT]
> I September 1 2017, Azure AD inte längre att tillåta hello PreferredDataLocation attributet på **synkroniseras användarobjekt** toobe direkt konfigureras med Azure AD PowerShell. tooconfigure PreferredLocation-attributet på synkroniserade objekt måste du använda Azure AD Connect endast.

Innan du aktiverar synkronisering av hello PreferredDataLocation attribut, måste du:

 * Bestäm vilka lokala Active Directory-attributet toobe används som hello källattribut först. Det ska vara av typen **sträng** och **enstaka**.

 * Om du tidigare har konfigurerat hello PreferredDataLocation attributet på befintliga synkroniseras användarobjekt i Azure AD med hjälp av Azure AD PowerShell, måste du **backport** hello attributet värden toohello motsvarande användarobjekt i lokala Active Directory.
 
    > [!IMPORTANT]
    > Om du gör inte backport hello attributet värden toohello motsvarande användarobjekt i lokala Active Directory och tar Azure AD Connect bort hello befintliga attributvärden i Azure AD när synkroniseringen för hello PreferredDataLocation attributet är aktiverad.

 * Bör du konfigurera hello källattribut på minst några lokala AD-användare objekt nu, som kan användas för verifiering senare.
 
hello steg tooenable synkronisering av hello PreferredDataLocation attribut kan sammanfattas som:

1. Inaktivera sync scheduler och kontrollera att det finns ingen synkronisering pågår

2. Lägg till hello källa attributet toohello lokala AD-koppling schema

3. Lägg till PreferredDataLocation toohello Azure AD Connector schema

4. Skapa ett attributvärde för inkommande synkronisering regeln tooflow hello från lokala Active Directory

5. Skapa ett utgående synkronisering regeln tooflow hello-attributet värdet tooAzure AD

6. Kör fullständig synkroniseringscykel

7. Aktivera sync scheduler

> [!NOTE]
> hello resten av det här avsnittet beskriver de här stegen i information. De beskrivs i hello kontexten för en Azure AD-distribution med en skog topologi och utan anpassade Synkroniseringsregler. Om du har flera skogar topologi anpassade Synkroniseringsregler konfigurerats eller har en fristående server måste du tooadjust hello steg i enlighet med detta.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Steg 1: Inaktivera sync scheduler och kontrollera att det finns ingen synkronisering pågår
Se till att ingen synkronisering sker när du arbetar med hello mitten av uppdatering synkronisering regler tooavoid oavsiktliga ändringar som exporteras tooAzure AD. toodisable hello inbyggda sync scheduler:

 1. Starta PowerShell-session på hello Azure AD Connect-servern.

 2. Inaktivera schemalagda synkroniseringen genom att köra cmdlet:`Set-ADSyncScheduler -SyncCycleEnabled $false`
 
 3. Starta hello **Synchronization Service Manager** genom att gå tooSTART → synkroniseringstjänsten.
 
 4. Gå toohello **Operations** fliken och bekräfta att ingen åtgärd vars status är *”pågår”.*

![Synchronization Service Manager - Kontrollera att inga åtgärder pågår](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step1.png)

### <a name="step-2-add-hello-source-attribute-toohello-on-premises-ad-connector-schema"></a>Steg 2: Lägg till hello källa attributet toohello lokala AD-koppling schema
Inte alla AD-attribut kan importeras till hello lokala AD-anslutningsplatsen. tooadd hello källa toohello attributlistan för hello importeras attribut:

 1. Gå toohello **kopplingar** fliken i hello Synchronization Service Manager.
 
 2. Högerklicka på hello **lokala AD-koppling** och välj **egenskaper**.
 
 3. Hello popup-fönstret, Välj toohello **Välj attribut** fliken.
 
 4. Kontrollera att hello källattributet är markerat i hello attributlistan.
 
 5. Klicka på **OK** toosave.

![Lägg till källan attributet tooon lokala AD-koppling schema](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step2.png)

### <a name="step-3-add-preferreddatalocation-toohello-azure-ad-connector-schema"></a>Steg 3: Lägg till PreferredDataLocation toohello Azure AD Connector schema
Hej PreferredDataLocation attributet är som standard inte importeras till hello Azure AD Connect utrymme. tooadd hello PreferredDataLocation attributet toohello listan över importerade attribut:

 1. Gå toohello **kopplingar** fliken i hello Synchronization Service Manager.

 2. Högerklicka på hello **Azure AD Connector** och välj **egenskaper**.

 3. Hello popup-fönstret, Välj toohello **Välj attribut** fliken.

 4. Kontrollera att kontrolleras hello PreferredDataLocation attribut i attributlistan hello.

 5. Klicka på **OK** toosave.

![Lägg till källan attributet tooAzure schemat för AD-koppling](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step3.png)

### <a name="step-4-create-an-inbound-synchronization-rule-tooflow-hello-attribute-value-from-on-premises-active-directory"></a>Steg 4: Skapa ett attributvärde för inkommande synkronisering regeln tooflow hello från lokala Active Directory
regel för inkommande synkronisering av hello tillåter hello-attributet värdet tooflow från hello källattribut från lokala Active Directory toohello metaversum:

1. Starta hello **Synchronization regler Editor** genom att gå tooSTART → synkronisering regler Editor.

2. Ange hello sökfilter **riktning** toobe **inkommande**.

3. Klicka på **Lägg till ny regel** knappen toocreate en ny inkommande regel.

4. Under hello **beskrivning** fliken, ange hello följande konfiguration:
 
    | Attribut | Värde | Information |
    | --- | --- | --- |
    | Namn | *Ange ett namn* | Till exempel *”i från AD-användaren PreferredDataLocation”* |
    | Beskrivning | *Ange en beskrivning* |  |
    | Det anslutna systemet | *Välj hello lokala AD-koppling* |  |
    | Anslutna System objekttyp | **Användaren** |  |
    | Typ av Metaversumobjekt | **Person** |  |
    | Länktypen | **Anslut dig** |  |
    | Prioritet | *Välj ett tal mellan 1 – 99* | 1 – 99 är reserverat för synkronisering av anpassade regler. Välj inte ett värde som används av en annan regel för synkronisering. |

5. Gå toohello **Scoping filter** fliken och Lägg till en **målgrupp filter grupp med hello följande sats**:
 
    | Attribut | Operatorn | Värde |
    | --- | --- | --- |
    | administratörsbeskrivning | NOTSTARTWITH | Användaren\_ | 
 
    Målgrupp filter avgör vilka lokala AD-objekt den här regeln för inkommande synkronisering tillämpas på. I det här exemplet använder vi hello samma målgrupp filter används som *”i från AD-användaren gemensamma”* OOB synkroniseringsregeln, vilket förhindrar att hello synkroniseringsregeln tillämpas tooUser objekt som skapats via tillbakaskrivning av Azure AD-användare funktionen. Du kan behöva tootweak hello målgrupp filter enligt tooyour Azure AD Connect-distribution.

6. Gå toohello **omvandling fliken** och implementera hello följande omvandlingsregeln:
 
    | Flöde | Målattribut | Källa | Använda en gång | Kopplingstyp |
    | --- | --- | --- | --- | --- |
    | Direkt | PreferredDataLocation | Välj hello källattribut | Alternativet är avmarkerat | Uppdatering |

7. Klicka på **Lägg till** toocreate hello inkommande regel.

![Skapa regel för inkommande synkronisering](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step4.png)

### <a name="step-5-create-an-outbound-synchronization-rule-tooflow-hello-attribute-value-tooazure-ad"></a>Steg 5: Skapa ett utgående synkronisering regeln tooflow hello-attributet värdet tooAzure AD
hello utgående synkroniseringsregel tillåter hello-attributet värdet tooflow från hello metaversum toohello PreferredDataLocation attribut i Azure AD:

1. Gå toohello **Synkroniseringsregler** Editor.

2. Ange hello sökfilter **riktning** toobe **utgående**.

3. Klicka på **Lägg till ny regel** knappen.

4. Under hello **beskrivning** fliken, ange hello följande konfiguration:

    | Attribut | Värde | Information |
    | --- | --- | --- |
    | Namn | *Ange ett namn* | Till exempel ”ut tooAAD – användaren PreferredDataLocation” |
    | Beskrivning | *Ange en beskrivning* |
    | Det anslutna systemet | *Välj hello AAD-koppling* |
    | Anslutna System objekttyp | Användare ||
    | Typ av Metaversumobjekt | **Person** ||
    | Länktypen | **Anslut dig** ||
    | Prioritet | *Välj ett tal mellan 1 – 99* | 1 – 99 är reserverat för synkronisering av anpassade regler. YDo inte hämtar ett värde som används av en annan regel för synkronisering. |

5. Gå toohello **Scoping filter** fliken och Lägg till en **målgrupp filter grupp med två klausuler**:
 
    | Attribut | Operatorn | Värde |
    | --- | --- | --- |
    | sourceObjectType | LIKA MED | Användare |
    | cloudMastered | NOTEQUAL | True |

    Målgrupp filter avgör vilka Azure AD-objekt den här utgående synkroniseringsregeln tillämpas på. I det här exemplet använder vi hello samma målgrupp filter från ”ut tooAD – användaridentitet” OOB synkroniseringsregeln. Det förhindrar hello synkroniseringsregeln tillämpade tooUser objekt som inte synkroniseras från lokala Active Directory. Du kan behöva tootweak hello målgrupp filter enligt tooyour Azure AD Connect-distribution.
    
6. Gå toohello **omvandling** fliken och implementera hello följande omvandlingsregeln:

    | Flöde | Målattribut | Källa | Använda en gång | Kopplingstyp |
    | --- | --- | --- | --- | --- |
    | Direkt | PreferredDataLocation | PreferredDataLocation | Alternativet är avmarkerat | Uppdatering |

7. Stäng **Lägg till** toocreate hello regel för utgående trafik.

![Skapa regel för utgående synkronisering](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step5.png)

### <a name="step-6-run-full-synchronization-cycle"></a>Steg 6: Kör fullständig synkroniseringscykel
I allmänhet krävs fullständig synkroniseringscykel eftersom vi har lagt till nya attribut tooboth hello AD och Azure AD Connector schema och introduceras anpassade Synkroniseringsregler. Vi rekommenderar att du kontrollerar hello ändringar innan du exporterar dem tooAzure AD. Du kan använda följande steg tooverify hello ändringar när du kör manuellt hello steg som utgör en fullständig synkroniseringscykel hello. 

1. Kör **fullständig import** steg på hello **lokala AD-koppling**:

   1. Gå toohello **Operations** fliken i hello Synchronization Service Manager.

   2. Högerklicka på hello **lokala AD-koppling** och välj **kör...**

   3. I popup-dialogrutan hello väljer **fullständig Import** och på **OK**.
    
   4. Vänta tills åtgärden toocomplete.

    > [!NOTE]
    > Du kan hoppa över fullständig Import på hello lokala AD-koppling om hello källattribut ingår redan i hello listan över importerade attribut. Med andra ord du inte har toomake ändringar under [steg 2: Lägg till hello källa attributet toohello lokala AD-koppling schemat](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Kör **fullständig import** steg på hello **Azure AD Connector**:

   1. Högerklicka på hello **Azure AD Connector** och välj **kör...**

   2. I popup-dialogrutan hello väljer **fullständig Import** och på **OK**.
   
   3. Vänta tills åtgärden toocomplete.

3. Kontrollera hello synkronisering regeländringar på ett befintligt användarobjekt:

hello källattribut från lokala Active Directory och PreferredDataLocation från Azure AD har importerats till hello respektive anslutningen utrymme. Innan du fortsätter med fullständig synkronisering steg, bör du göra en **Preview** på en befintlig användares objekt i hello lokala AD-anslutningsplatsen. hello objekt du valt bör ha hello källattribut fylls i. En lyckad **Preview** med hello PreferredDataLocation i hello metaversum är en bra indikator som du har konfigurerat hello Synkroniseringsregler korrekt. Information om hur toodo en **Preview**, se toosection [verifiera hello ändra](#verify-the-change).

4. Kör **fullständig synkronisering** steg på hello **lokala AD-koppling**:

   1. Högerklicka på hello **lokala AD-koppling** och välj **kör...**
  
   2. I popup-dialogrutan hello väljer **fullständig synkronisering** och på **OK**.
   
   3. Vänta tills åtgärden toocomplete.

5. Kontrollera **väntande exporter** tooAzure AD:

   1. Högerklicka på hello **Azure AD Connector** och välj **söka Anslutarplats**.

   2. I hello Sök anslutningsplatsen popup-fönstret:

      1. Ange **omfång** för**väntande exportera**.
      
      2. Markera alla tre kryssrutorna, inklusive **lägga till, ändra och ta bort**.
      
      3. Klicka på hello **Sök** knappen tooget hello lista med objekt med ändringar toobe exporteras. tooexamine hello ändringar för ett angivet objekt dubbelklicka hello-objektet.
      
      4. Kontrollera hello ändringar förväntas.

6. Kör **exportera** steg på hello **Azure AD-koppling**
      
   1. Högerklicka på hello **Azure AD Connector** och välj **kör...**
   
   2. I hello kör Connector popup-fönstret, Välj **exportera** och på **OK**.
   
   3. Vänta tills exporten tooAzure AD toocomplete.

> [!NOTE]
> Det kan hända att hello stegen inte omfattar hello fullständig synkronisering steg och Export på hello Azure AD-koppling. hello steg behövs inte eftersom hello attributvärden som flödar in från lokala Active Directory tooAzure AD endast.

### <a name="step-7-re-enable-sync-scheduler"></a>Steg 7: Återaktivera sync scheduler
Återaktivera hello inbyggda sync scheduler:

1. Starta PowerShell-session.

2. Aktivera schemalagd synkronisering igen genom att köra cmdlet:`Set-ADSyncScheduler -SyncCycleEnabled $true`



## <a name="next-steps"></a>Nästa steg
* Läs mer om hello Konfigurationsmodell i [förstå deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Läs mer om hello Uttrycksspråk i [förstå uttryck för deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
