---
title: 'Azure AD Connect: Designbegreppen | Microsoft Docs'
description: "Det här avsnittet beskrivs vissa implementering design områden"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 4114a6c0-f96a-493c-be74-1153666ce6c9
ms.service: active-directory
ms.custom: azure-ad-connect
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1e5d5c6a716ca653fb14fc059e8155124b433732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Designbegrepp
hello syftet med det här avsnittet är toodescribe områden som måste betraktas under hello implementeringsdesign av Azure AD Connect. Det här avsnittet är en djupdykning i vissa områden och dessa koncept beskrivs kortfattat i andra avsnitt samt.

## <a name="sourceanchor"></a>sourceAnchor
attributet sourceAnchor för hello definieras som *ett attribut som inte ändras under hello livslängd för ett objekt*. Identifierar ett objekt som hello samma objekt lokalt och i Azure AD. hello attribut kallas också **immutableId** och hello två namn används utbytbara.

hello ord ändras, är ”kan inte ändras”, är viktiga toothis avsnittet. Eftersom värdet för det här attributet inte kan ändras när den väl har angetts, är det viktigt toopick en design som har stöd för ditt scenario.

hello-attribut används för hello följande scenarier:

* När en ny synkroniseringsserver för motorn inbyggda eller återskapas efter en katastrofåterställning, länkar det här attributet befintliga objekt i Azure AD med objekt på lokalt.
* Om du flyttar från en molnbaserad tooa synkroniserade identitet identitetsmodellen, så det här attributet tillåter objekt för ”hårda matchar” befintliga objekt i Azure AD med lokalt objekt.
* Om du använder federation och sedan det här attributet tillsammans med hello **userPrincipalName** används i hello anspråk toouniquely identifiera en användare.

Det här avsnittet handlar om sourceAnchor endast när det gäller toousers. hello samma regler gäller tooall objekttyper, men endast för användare är det här problemet är vanligtvis ett problem.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Att välja en bra sourceAnchor attribut
hello attributvärdet måste följa hello följande regler:

* Vara mindre än 60 tecken
  * Tecken som inte är a-z, A-Z eller 0-9 kodas och räknas som 3 tecken
* Inte innehåller ett specialtecken: &#92;! # $ % & * + / = ? ^ &#96; { } | ~ < > ( ) ' ; : , [ ] " @ _
* Måste vara globalt unika
* Måste vara en sträng, heltal eller binär
* Ska inte baseras på användarens namn, ändringarna
* Bör inte skiftlägeskänsliga och undvika värden som kan variera efter fall
* Ska tilldelas när hello objekt skapas

Om hello markerat sourceAnchor är inte av typen string, Azure AD Connect Base64Encode hello-attributet värdet tooensure några specialtecken visas. Om du använder en annan federationsserver än AD FS kan du kontrollera om servern kan också Base64Encode hello attribut.

Hej sourceAnchor attributet är skiftlägeskänsliga. Värdet ”AndersSvensson” är inte hello samma som ”AndersSvensson”. Men du bör inte ha två olika objekt med endast en avvikelse i fall.

Om du har en enda skog är lokalt, sedan hello-attribut som du bör använda **objectGUID**. Detta är även hello-attribut som används när du använder standardinställningar i Azure AD Connect och även hello attribut som används av DirSync.

Om du har flera skogar och flytta inte användare mellan skogar och domäner, sedan **objectGUID** är en bra attributet toouse även i det här fallet.

Om du flyttar användare mellan skogar och domäner, måste du hitta ett attribut som inte ändras eller kan flyttas med hello användare under hello move. En rekommenderad metod är toointroduce syntetiska attribut. Ett attribut som kan innehålla något som ser ut som ett GUID skulle vara lämplig. När objekt skapas en ny GUID skapas och stämplad hello användaren. En anpassad synkroniseringsregel kan skapas i hello sync motorn server toocreate detta värde baserat på hello **objectGUID** och uppdatera hello valda attribut i lägger till. Kontrollera att tooalso kopiera hello innehållet i det här värdet när du flyttar hello-objekt.

En annan lösning är toopick ett befintligt attribut som du vet inte ändras. Vanliga attribut inkluderar **employeeID**. Om du anser att ett attribut som innehåller bokstäverna, kontrollera att det finns aldrig chansen hello (versal eller gemen) kan ändra för hello attributvärdet. Felaktigt attribut som inte ska användas med dessa attribut med hello namnet på hello användare. I en annullering eller äktenskapsskillnad är hello namn förväntat toochange, vilket inte är tillåtet för det här attributet. Detta är också ett skäl till varför attribut som **userPrincipalName**, **e**, och **targetAddress** inte är även möjligt tooselect i hello Azure AD Connect-installationen guiden. Attributen också innehålla hello ”@” tecken, vilket inte är tillåtet i hello sourceAnchor.

### <a name="changing-hello-sourceanchor-attribute"></a>Ändra hello sourceAnchor attribut
Hej sourceAnchor attributvärdet kan inte ändras när hello-objektet har skapats i Azure AD och hello identitet synkroniseras.

Därför hello följande begränsningar gäller tooAzure AD Connect:

* Hej sourceAnchor attributet kan endast anges under installationen. Om du kör installationsguiden för hello är det här alternativet skrivskyddad. Om du behöver toochange den här inställningen, måste du avinstallera och installera om.
* Om du installerar en annan Azure AD Connect-servern, kan du måste markera hello samma sourceAnchor-attribut som används som tidigare. Om du tidigare har använt DirSync och flytta tooAzure AD Connect måste du använda **objectGUID** eftersom hello-attribut som används av DirSync.
* Om hello värde för sourceAnchor har ändrats efter har hello-objektet exporterade tooAzure AD, sedan Azure AD Connect-synkronisering genererar ett fel och tillåter inte några fler ändringar på objektet innan hello problemet har åtgärdats och hello sourceAnchor ändras tillbaka i hello källkatalog.

## <a name="using-msds-consistencyguid-as-sourceanchor"></a>Med msDS-ConsistencyGuid som sourceAnchor
Som standard, Azure AD Connect (version 1.1.486.0 och äldre) använder objectGUID som hello sourceAnchor attribut. ObjectGUID är systemgenererad. Du kan inte ange värdet när du skapar lokala AD-objekt. Enligt beskrivningen i avsnittet [sourceAnchor](#sourceanchor), finns det scenarier där du behöver toospecify hello sourceAnchor värde. Om hello scenarier är tillämpliga tooyou, måste du använda en konfigurerbar AD-attributet (till exempel msDS-ConsistencyGuid) som hello sourceAnchor attribut.

Azure AD Connect (version 1.1.524.0 och efter) nu underlättar hello användningen av msDS-ConsistencyGuid som attributet sourceAnchor. När du använder den här funktionen konfigurerar hello Synkroniseringsregler till automatiskt i Azure AD Connect:

1. Använd msDS-ConsistencyGuid som hello sourceAnchor attribut för användarobjekt. ObjectGUID används för andra objekttyper.

2. För alla angivna lokala AD-användare vars attributet msDS-ConsistencyGuid inte är ifylld, Azure AD Connect skriver ett objectGUID värdet tillbaka toohello msDS-ConsistencyGuid-attributet i lokala Active Directory-objekt. När hello msDS-ConsistencyGuid attribut fylls exporterar sedan hello objektet tooAzure AD i Azure AD Connect.

>[!NOTE]
> När en lokal AD-objekt har importerats till Azure AD Connect (som, importeras till hello AD anslutningsplatsen och planerad i hello metaversum), du kan inte ändra dess sourceAnchor värde längre. toospecify hello sourceAnchor värdet för ett givet lokala AD objekt, konfigurera ett attributet msDS-ConsistencyGuid innan den har importerats till Azure AD Connect.

### <a name="permission-required"></a>Behörigheten som krävs
För den här funktionen toowork ges hello AD DS-konto används toosynchronize med lokala Active Directory skrivåtgärder behörighet toohello msDS-ConsistencyGuid attribut i lokala Active Directory.

### <a name="how-tooenable-hello-consistencyguid-feature---new-installation"></a>Hur tooenable hello ConsistencyGuid funktionen - nyinstallation
Du kan aktivera hello användningen av ConsistencyGuid som sourceAnchor vid nyinstallation. Det här avsnittet beskriver både snabb och anpassad installation i informationen.

  >[!NOTE]
  > Endast nyare versioner av Azure AD Connect (1.1.524.0 och efter) stöder hello användningen av ConsistencyGuid som sourceAnchor vid nyinstallation.

### <a name="how-tooenable-hello-consistencyguid-feature"></a>Hur tooenable hello ConsistencyGuid funktion
För närvarande kan hello-funktionen bara aktiveras under endast nya Azure AD Connect-installationen.

#### <a name="express-installation"></a>Expressfunktion för Installation
När du installerar Azure AD Connect med Express-läge, bestämmer automatiskt hello lämpligaste AD attributet toouse som hello sourceAnchor attribut med hjälp av följande logik hello hello Azure AD Connect-guiden:

* Först hello hello Azure AD Connect-guiden frågor din Azure AD-klient tooretrieve hello AD-attribut som används som attributet sourceAnchor i hello tidigare Azure AD Connect-installationen (om sådan finns). Om denna information är tillgänglig använder Azure AD Connect hello samma AD-attributet.

  >[!NOTE]
  > Endast nyare versioner av Azure AD Connect (1.1.524.0 och efter) lagrar information i Azure AD-klienten om attributet sourceAnchor för hello används under installationen. Äldre versioner av Azure AD Connect inte.

* Om information om hello sourceAnchor attribut som används inte är tillgänglig kontrollerar hello guiden hello tillståndet för hello msDS-ConsistencyGuid attribut i din lokala Active Directory. Om hello-attributet inte är konfigurerad på alla objekt i katalogen hello används hello hello msDS-ConsistencyGuid som hello sourceAnchor attribut. Om hello-attribut konfigureras på en eller flera objekt i hello directory slutsatsen hello guiden hello-attribut som används av andra program och är inte lämpligt som attributet sourceAnchor...

* I så fall hello guiden använder toousing objectGUID som hello sourceAnchor attribut.

* När attributet sourceAnchor för hello bestäms lagrar hello guiden hello information i Azure AD-klienten. hello information kommer att användas av framtida installation av Azure AD Connect.

När Express-installationen är klar informerar hello guiden vilket attribut har plockats som hello Källfästpunkten attribut.

![Guiden informerar AD attributet sourceAnchor har valts](./media/active-directory-aadconnect-design-concepts/consistencyGuid-01.png)

#### <a name="custom-installation"></a>Anpassad installation
När du installerar Azure AD Connect med anpassade läge innehåller hello Azure AD Connect-guiden två alternativ när du konfigurerar attributet sourceAnchor:

![Anpassad installation – sourceAnchor konfiguration](./media/active-directory-aadconnect-design-concepts/consistencyGuid-02.png)

| Inställning | Beskrivning |
| --- | --- |
| Låt Azure hantera hello källfästpunkten för mig | Välj det här alternativet om du vill att Azure AD toopick hello attribut för dig. Om du väljer det här alternativet hello Azure AD Connect guiden gäller samma [sourceAnchor attributet markeringen logik som används under snabbinstallationen](#express-installation). Liknande tooExpress installation hello Guiden informerar vilket attribut har plockats som hello Källfästpunkten attribut när anpassade installationen är klar. |
| Ett specifikt attribut | Välj det här alternativet om du vill toospecify ett befintligt AD-attribut som hello sourceAnchor attribut. |

### <a name="how-tooenable-hello-consistencyguid-feature---existing-deployment"></a>Hur tooenable hello ConsistencyGuid funktionen - befintlig distribution
Om du har en befintlig Azure AD Connect-distribution som använder objectGUID som hello Källfästpunkten attribut kan växla du den toousing ConsistencyGuid i stället.

>[!NOTE]
> Endast nyare versioner av Azure AD Connect (1.1.552.0 och efter) har stöd för växlar från ObjectGuid tooConsistencyGuid som hello Källfästpunkten attribut.

tooswitch från objectGUID tooConsistencyGuid som hello Källfästpunkten attribut:

1. Starta hello Azure AD Connect-guiden och klicka på **konfigurera** toogo toohello uppgifter skärmen.

2. Välj hello **konfigurera Källfästpunkten** uppgift alternativ och klickar på **nästa**.

   ![Aktivera ConsistencyGuid för befintliga distribution – steg 2](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment01.png)

3. Ange dina autentiseringsuppgifter för Azure AD-administratör och klicka på **nästa**.

4. Azure AD Connect-guiden analyserar hello tillståndet för hello msDS-ConsistencyGuid attribut i din lokala Active Directory. Om hello-attributet inte är konfigurerad på alla objekt i katalogen, Azure AD Connect slutsatsen att inget annat program använder hello attribut och säker toouse hello den som hello Källfästpunkten attribut. Klicka på **nästa** toocontinue.

   ![Aktivera ConsistencyGuid för befintliga distribution – steg 4](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment02.png)

5. I hello **klar tooConfigure** klickar du på **konfigurera** toomake hello konfigurationsändring.

   ![Aktivera ConsistencyGuid för befintliga distribution – steg 5](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment03.png)

6. När hello konfigurationen är klar, anger hello guiden att msDS-ConsistencyGuid används som hello Källfästpunkten attribut.

   ![Aktivera ConsistencyGuid för befintliga distribution – steg 6](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment04.png)

Vid analys av hello (steg 4), om hello-attribut konfigureras på ett eller flera objekt i hello directory slutsatsen hello guiden hello-attribut som används av ett annat program och returnerar ett fel som visas i hello diagrammet nedan. Om du är säker på att hello-attributet inte är används av befintliga program behöver du toocontact stöd för information om hur toosuppress hello fel.

![Aktivera ConsistencyGuid för befintliga distribution – fel](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeploymenterror.png)

### <a name="impact-on-ad-fs-or-third-party-federation-configuration"></a>Påverkan på AD FS eller tredje parts federation-konfiguration
Om du använder Azure AD Connect toomanage lokalt AD FS-distribution, hello Azure AD Connect uppdaterar automatiskt hello anspråk regler toouse hello samma AD-attributet som sourceAnchor. Detta garanterar att hello ImmutableID anspråk som genererats av AD FS är konsekventa med hello sourceAnchor värden exporterade tooAzure AD.

Om du hanterar AD FS utanför Azure AD Connect eller om du använder federationsservrar från tredje part för autentisering, måste du manuellt uppdatera hello anspråksregler för ImmutableID anspråk toobe konsekvent med hello sourceAnchor värden exporteras tooAzure AD som beskrivs i artikel [ändra AD FS anspråksregler](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#modclaims). hello guiden returnerar hello följande varning när installationen är klar:

![Tredjeparts federation-konfiguration](./media/active-directory-aadconnect-design-concepts/consistencyGuid-03.png)

### <a name="adding-new-directories-tooexisting-deployment"></a>Lägga till nya kataloger tooexisting distributionen
Anta att du har distribuerat Azure AD Connect hello ConsistencyGuid funktionen är aktiverad och nu vill du tooadd en annan katalog toohello distribution. När du försöker tooadd hello directory kontrollerar hello tillstånd på hello mSDS-ConsistencyGuid attribut i directory hello Azure AD Connect-guiden. Om hello-attribut konfigureras på en eller flera objekt i hello directory slutsatsen hello guiden hello-attribut som används av andra program och returnerar ett fel som visas i hello diagrammet nedan. Om du är säker på att hello-attributet inte är används av befintliga program behöver du toocontact stöd för information om hur toosuppress hello fel.

![Lägga till nya kataloger tooexisting distributionen](./media/active-directory-aadconnect-design-concepts/consistencyGuid-04.png)

## <a name="azure-ad-sign-in"></a>Azure AD-inloggning
När du integrerar din lokala katalog med Azure AD, är det viktigt toounderstand hur hello synkroniseringsinställningar kan påverka hello sätt användaren autentiseras. Azure AD använder userPrincipalName (UPN) tooauthenticate hello användare. När du synkroniserar dina användare, måste du välja hello attributet toobe används för värdet för userPrincipalName noggrant.

### <a name="choosing-hello-attribute-for-userprincipalname"></a>Att välja hello attribut för userPrincipalName
När du väljer hello attribut för att tillhandahålla hello värdet för UPN-toobe används i Azure som bör kontrollera

* hello attributvärden överensstämmer toohello UPN-syntax (RFC 822), som det ska vara hello-formatusername@domain
* hello-suffix i hello värden matchar tooone av Hej kontrolleras anpassade domäner i Azure AD

I standardinställningar antas hello valet för hello-attributet är userPrincipalName. Om hello userPrincipalName attributet inte innehåller hello värde du vill att dina användare toosign i tooAzure och du måste välja **Anpassad Installation**.

### <a name="custom-domain-state-and-upn"></a>Tillstånd för anpassad domän- och UPN
Det är viktigt tooensure att det finns en verifierad domän för hello UPN-suffix.

John är en användare i contoso.com. Du vill John toouse hello lokala UPN john@contoso.com toosign i tooAzure efter att du har synkroniserats användare tooyour Azure AD directory contoso.onmicrosoft.com. toodo så kan du behöver tooadd och verifiera contoso.com som en anpassad domän i Azure AD innan du kan börja Synkroniserar hello-användare. Om hello UPN-suffix John, till exempel contoso.com inte matchar en verifierad domän i Azure AD, ersätter Azure AD hello UPN-suffix med contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Icke-dirigerbara lokala domäner och UPN för Azure AD
Vissa organisationer har icke-dirigerbara domäner, t.ex. contoso.local eller enkel etikett domäner som contoso. Du är inte kan tooverify en icke-dirigerbara domän i Azure AD. Azure AD Connect kan synkronisera tooonly en verifierad domän i Azure AD. När du skapar en Azure AD-katalog skapar en dirigerbara domän som blir standarddomän för din Azure AD, till exempel contoso.onmicrosoft.com. Därför blir det nödvändigt tooverify andra dirigerbara domänen i ett sådant scenario om du inte vill toosync toohello standarddomänen onmicrosoft.com.

Läs [lägga till din anpassade domän namn tooAzure Active Directory](../active-directory-add-domain.md) för mer information om att lägga till och verifiera domäner.

Azure AD Connect identifierar om du kör i en icke-dirigerbara domänmiljö och korrekt skulle varnar dig från att gå vidare med standardinställningar. Om du arbetar i en icke-dirigerbara domän, och sedan är det troligt att hello UPN hello användare ha icke-dirigerbara suffix för. Till exempel föreslår, om du kör under contoso.local, Azure AD Connect du toouse anpassade inställningar i stället för med standardinställningar. Med anpassade inställningar som kan toospecify hello attribut som ska användas som UPN-toosign i tooAzure när hello användare är synkroniserade tooAzure AD.

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
