---
title: "Azure AD Connect-synkronisering: Förstå hello-arkitektur | Microsoft Docs"
description: "Det här avsnittet beskriver hello arkitekturen i Azure AD Connect-synkronisering och förklarar hello termer som används."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 465bcbe9-3bdd-4769-a8ca-f8905abf426d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 9fb979fcf8feb7b4d406789102239480b0b4bc94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-hello-architecture"></a>Azure AD Connect-synkronisering: Förstå hello-arkitektur
Det här avsnittet innehåller grundläggande hello-arkitektur för Azure AD Connect-synkronisering. I många aspekter är liknande tooits föregående MIIS 2003 ILM 2007 och FIM 2010. Azure AD Connect-synkronisering är hello utvecklingen av dessa tekniker. Om du är bekant med någon av dessa tidigare tekniker att hello innehållet i det här avsnittet bekant tooyou samt. Om du är ny toosynchronization, är det här avsnittet för dig. Det är emellertid inte ett krav tooknow hello information i det här avsnittet toobe lyckas gör anpassningar tooAzure AD Connect-synkronisering (kallas Synkroniseringsmotorn i det här avsnittet).

## <a name="architecture"></a>Arkitektur
Hej Synkroniseringsmotorn skapar en integrerad vy av objekt som lagras i flera anslutna datakällor och hanterar identitetsinformation i dessa datakällor. Den här integrerade vyn bestäms av hello identitetsinformation hämtas från anslutna datakällor och en uppsättning regler som bestämmer hur tooprocess informationen.

### <a name="connected-data-sources-and-connectors"></a>Anslutna datakällor och kopplingar
Hej Synkroniseringsmotorn bearbetar identitetsinformation från olika databaser, till exempel Active Directory eller en SQL Server-databas. Alla data databaser som organiserar data i en databas som format och som ger standard dataåtkomst metoder är en potentiell datakälla kandidat för hello Synkroniseringsmotorn. hello-databaser som är synkroniserad med Synkroniseringsmotorn kallas **anslutna datakällor** eller **anslutna kataloger** (CD).

Hej Synkroniseringsmotorn kapslar in interaktion med en ansluten datakälla i en modul som kallas en **Connector**. Varje typ av anslutna datakällan har en viss koppling. hello Connector omvandlar en obligatorisk åtgärd till hello-format som hello anslutna data källa förstår.

Kopplingar gör API-anrop tooexchange identitetsinformation (både läsning och skrivning) med en ansluten datakälla. Det är också möjligt tooadd en anpassad koppling med hello extensible connectivity framework. hello följande bild visar hur en koppling ansluter en ansluten datakälla toohello sync motor.

![Ark1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Data kan flöda i båda riktningarna, men det går inte att flöda i båda riktningarna samtidigt. Med andra ord en koppling kan vara konfigurerade tooallow data tooflow från hello anslutna datakälla toosync motorn eller synkronisera motorn toohello anslutna datakällan, men endast en av dessa åtgärder kan ske när som helst för ett objekt och attribut. hello riktning kan vara olika för olika objekt och för olika attribut.

tooconfigure en koppling, anger du hello objekttyper som du vill toosynchronize. Att ange hello objekttyper definierar hello omfånget för objekt som ingår i hello synkroniseringsprocessen. hello nästa steg är tooselect hello attribut toosynchronize, som kallas för ett attribut inkluderingslistan. De här inställningarna kan ändras när i svaret toochanges tooyour affärsregler. När du använder installationsguiden för hello Azure AD Connect konfigureras de här inställningarna för dig.

tooexport objekt tooa anslutna datakällan, hello lista över attribut måste innehålla minst hello minsta attribut krävs toocreate skriver du ett specifikt objekt i en ansluten datakälla. Till exempel hello **sAMAccountName** attributet måste tas med i hello attributet inkludering listan tooexport objekt tooActive Directory-användare eftersom alla användarobjekt i Active Directory måste ha en **sAMAccountName**  attribut. Hello installationsguiden matchar igen och den här konfigurationen åt dig.

Du kan begränsa hello områden i hello anslutna datakällan som används för en viss lösning om hello anslutna datakällan använder komponenter, till exempel partitioner eller behållare tooorganize-objekt.

### <a name="internal-structure-of-hello-sync-engine-namespace"></a>Interna strukturen för hello sync motorn namnområde
hello hela sync motorn namnområde består av två namnrymder som lagrar hello identitetsinformation. hello två namnområden är:

* hello anslutningsplatsen (CS)
* hello metaversum (MV)

Hej **anslutarplats** är ett mellanlagringsområde som innehåller representationer av hello avses objekt från en ansluten data käll- och hello attribut som anges i hello lista över attribut. Hej Synkroniseringsmotorn använder hello connector utrymme toodetermine vad som har ändrats i hello anslutna käll- och toostage inkommande ändringar. Hej Synkroniseringsmotorn använder också hello connector utrymme toostage utgående ändringar för export toohello anslutna datakällan. Hej Synkroniseringsmotorn underhåller distinkta anslutningsplatsen som ett mellanlagringsområde för varje koppling.

Med hjälp av ett mellanlagringsområde hello Synkroniseringsmotorn förblir oberoende av hello anslutna datakällor och påverkas inte av deras tillgänglighet och tillgänglighet. Du kan därför bearbeta identitetsinformation när som helst genom att använda hello data i hello mellanlagringsområdet. Hej Synkroniseringsmotorn kan begära endast hello ändringar i hello anslutna datakällan hello senaste kommunikationssessionen avslutas eller push endast hello ändringar tooidentity information som hello anslutna datakällan har inte har tagits emot, vilket minskar hello nätverkstrafik mellan hello Synkroniseringsmotorn och hello anslutna datakällan.

Dessutom lagras Synkroniseringsmotorn statusinformation om alla objekt som den skapar etapper i hello anslutningsplatsen. När nya data tas emot, utvärderar Synkroniseringsmotorn alltid om hello data redan har synkroniserats.

Hej **metaversum** är en lagringsplats som innehåller hello samman ID-information från flera anslutna datakällor, vilket ger en enda global vy över alla kombinerade objekt. Metaversum-objekt skapas baserat på hello identitetsinformation som har hämtats från hello anslutna datakällor och en uppsättning regler som tillåter toocustomize hello synkroniseringsprocessen.

hello följande bild visar hello connector utrymme namnområde och hello metaversum namnområde inom hello Synkroniseringsmotorn.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Synkronisera motorn identitetsobjekt
hello objekt i hello Synkroniseringsmotorn framställningar av antingen objekt i hello anslutna datakällan eller hello integrerad vy som synkroniserar motorn har av dessa objekt. Varje synkronisering motorobjekt måste ha en globalt unik identifierare (GUID). GUID ger dataintegritet och snabb förhållanden mellan objekt.

### <a name="connector-space-objects"></a>Kopplingen utrymme objekt
När Synkroniseringsmotorn kommunicerar med en ansluten datakälla, läser hello identitetsinformation i hello anslutna datakällan och använder denna information toocreate en representation av hello identitetsobjekt i hello anslutningsplatsen. Du kan inte skapa eller ta bort de här objekten individuellt. Du kan manuellt ta bort alla objekt i en anslutningsplatsen.

Alla objekt i hello anslutningsplatsen har två attribut:

* En globalt unik identifierare (GUID)
* Ett unikt namn (DN)

Om hello ansluten datakälla tilldelas ett unikt attribut toohello objekt och objekt i hello anslutningsplatsen kan också ha fästpunktsattribut. Hej fästpunktsattributet identifierar ett objekt i hello anslutna datakällan. Hej Synkroniseringsmotorn använder hello fästpunkt toolocate hello motsvarande representation av det här objektet i hello anslutna datakällan. Synkroniseringsmotorn förutsätter att hello fästpunkt för ett objekt aldrig ändringar under hello livslängd i hello-objektet.

Många av hello kopplingar använder en unik identifierare för kända toogenerate ankare automatiskt för varje objekt när den har importerats. Till exempel hello Active Directory-kopplingen använder hello **objectGUID** attribut för en fästpunkt. Du kan ange fästpunkt generation som en del av hello kopplingens konfiguration av anslutna datakällor som inte tillhandahåller ett tydligt definierade unika identifierare.

Då fallet hello fästpunkt skapas från en eller flera unikt attribut för ett objekt skriver, varken som ändras och som unikt identifierar hello objekt i hello anslutningsplatsen (till exempel en anställd tal eller ett användar-ID).

Ett utrymme connector-objekt kan vara något av följande hello:

* Ett fristående objekt
* En platshållare

### <a name="staging-objects"></a>Mellanlagring objekt
Ett fristående objekt representerar en instans av hello avses objekttyper från hello anslutna datakällan. Dessutom toohello GUID och hello unika namn, ett fristående objekt har alltid ett värde som anger hello objekttyp.

Mellanlagring objekt som har importerats alltid har ett värde för hello fästpunktsattributet. Mellanlagring objekt som nyligen har etablerats av Synkroniseringsmotorn och är i hello processen att skapas i hello anslutna datakällan har inte ett värde för hello fästpunktsattributet.

Objekt för Förproduktion utför också aktuella värden för företag-attribut och teknisk information som krävs av motorn tooperform hello synkronisering synkroniseringen. Funktionsinformation innehåller flaggor som anger hello typ av uppdateringar som mellanlagras på hello mellanlagring objekt. Om ett fristående objekt har fått ny identitetsinformation från hello anslutna datakällan som ännu inte bearbetats, hello objekt flaggas som **väntande import**. Om ett fristående objekt har nya identitetsinformation som ännu inte har exporterade toohello anslutna datakällan, den är markerad som **väntande export**.

Ett fristående objekt kan vara en importera eller exportera objekt. Hej Synkroniseringsmotorn skapar ett importobjekt med hjälp av Objektinformation från hello anslutna datakällan. När Synkroniseringsmotorn tar emot information om hello förekomsten av ett nytt objekt som matchar ett av hello objekttyper som valts i hello koppling, skapar en importobjekt i hello anslutningsplatsen som en representation av hello objekt i hello anslutna datakällan.

hello följande bild visar ett importobjekt som representerar ett objekt i hello anslutna datakällan.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

hello Synkroniseringsmotorn skapar en export-objekt med information om objekt i hello metaversum. Exportera objekt är exporterade toohello anslutna datakällan under hello nästa kommunikation session. Ur hello av hello Synkroniseringsmotorn finns exportera objekt inte i hello anslutna datakällan ännu. Hej fästpunktsattributet för en export-objektet är därför inte tillgänglig. När den tar emot hello objekt från Synkroniseringsmotorn skapar hello anslutna datakällan ett unikt värde för hello fästpunktsattributet i hello-objektet.

hello följande bild visar hur en export-objektet har skapats med hjälp av identitetsinformation i hello metaversum.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

Hej Synkroniseringsmotorn bekräftar hello export av hello objekt genom att importera hello objekt från hello anslutna datakällan. Exportera objekt blir importera objekt när Synkroniseringsmotorn tar emot dem under hello nästa importen från den anslutna datakällan.

### <a name="placeholders"></a>Platshållare
Hej Synkroniseringsmotorn använder en platt namnområde toostore objekt. Men vissa anslutna datakällor, till exempel Active Directory och använda ett hierarkiskt namnområde. tootransform information från ett hierarkiskt namnområde i en platt namnområde Synkroniseringsmotorn använder platshållare toopreserve hello hierarki.

Varje platshållare representerar en komponent (till exempel en organisationsenhet) i en hierarkisk objektnamn som inte har importerats till Synkroniseringsmotorn men är obligatoriska tooconstruct hello hierarkiska namn. De fyller luckor som skapats av referenser i hello anslutna datakälla tooobjects som inte mellanlagring objekt i hello anslutningsplatsen.

Hej Synkroniseringsmotorn använder också platshållare toostore refererar till objekt som ännu inte importerats. Om synkronisering är konfigurerade tooinclude hello manager attribut för hello exempelvis *Abbie Spencer* objekt och hello mottagna värdet är ett objekt som inte har importerats ännu, såsom *CN = Lee Sperry, CN = Users, DC = fabrikam, DC = com*, hello manager informationen lagras som platshållare i hello anslutningsplatsen. Om objekt hello senare importeras skrivs hello platshållarobjekt över av hello mellanlagring objekt som representerar hello manager.

### <a name="metaverse-objects"></a>Metaversum-objekt
En metaversumobjekt innehåller hello samman vy att Synkroniseringsmotorn har av hello mellanlagring objekt i hello anslutningsplatsen. Synkroniseringsmotorn skapar metaversum-objekt med hjälp av hello information i Importera objekt. Flera connector utrymme objekt kan vara länkad tooa enda metaversumobjekt, men ett utrymme connector-objekt får inte vara länkade toomore än ett metaversumobjekt.

Metaversum-objekt kan inte skapas eller tas bort manuellt. Hej Synkroniseringsmotorn tas automatiskt bort metaversum-objekt som inte har en länk tooany utrymme kopplingsobjektet i hello anslutningsplatsen.

toomap objekt inom en ansluten tooa motsvarande typ av datakällobjekt i hello metaversum, Synkroniseringsmotorn ger ett Utökningsbart schema med en fördefinierad uppsättning objekttyper och motsvarande attribut. Du kan skapa nya objekttyper och attribut för metaversum-objekt. Attribut kan vara eller flera värden och hello attributtyper kan vara strängar, referenser, siffror och booleska värden.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Relationer mellan fristående och metaversum-objekt
I hello sync motorn namnområde aktiveras hello dataflödet som hello länken relationen mellan fristående och metaversum-objekt. Mellanlagring av ett objekt som är länkade tooa metaversumobjekt kallas en **kopplade objekt** (eller **kopplingsobjektet**). Ett fristående objekt som inte är länkade tooa metaversumobjekt kallas en **degraderingsprocessen objektet** (eller **disconnector objektet**). hello villkoren ansluten och degraderingsprocessen är prioriterade toonot ihop med hello kopplingar som ansvarar för att importera och exportera data från en ansluten katalog.

Platshållare är aldrig länkade tooa metaversumobjekt

En domänansluten objektet består av ett fristående objekt och dess länkade relationen tooa enda metaversumobjekt. Kopplade objekten har använt toosynchronize attributvärden mellan en koppling utrymme objekt och metaversum-objekt.

När ett fristående objekt blir en domänansluten objektet under synkroniseringen, kan attribut flöda mellan hello mellanlagring objekt och hello metaversum-objekt. Attributflöde är dubbelriktad och konfigureras med hjälp av attributet importregler och exportera attributregler.

En enskild koppling utrymme objekt kan vara länkad tooonly en metaversumobjekt. Varje metaversumobjekt kan dock vara länkade toomultiple utrymme kopplingsobjekt i hello samma eller olika kopplingens utrymmen som visas i följande illustration hello.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

hello länkade förhållandet mellan hello mellanlagring objekt och en metaversumobjekt är permanent och kan tas bort bara av regler som du anger.

En frånkopplad objektet är ett fristående objekt som inte är länkade tooany metaversumobjekt. hello-attribut som värden i en frånkopplad objektet inte är bearbeta några ytterligare inom hello metaversum. Hej attributet värdena för hello motsvarande objekt i hello anslutna datakällan inte uppdateras av Synkroniseringsmotorn.

Du kan lagra identitetsinformation i Synkroniseringsmotorn och bearbetar den senare genom att använda skilda objekt. Som ett fristående objekt som en frånkopplad objekt i hello anslutningsplatsen har många fördelar. Eftersom hello system har redan mellanlagrat hello krävs information om det här objektet, men det är inte nödvändigt toocreate som en representation av det här objektet igen under hello bredvid importera från hello anslutna datakällan. På så sätt kan Synkroniseringsmotorn har alltid en fullständig ögonblicksbild av hello anslutna datakällan, även om det finns ingen aktuell anslutning toohello ansluten datakälla. Frånkopplad objekt kan konverteras till domänanslutna objekt, och vice versa beroende på hello regler som du anger.

Ett importobjekt skapas som en frånkopplad objekt. En export-objektet måste vara en domänansluten objekt. hello logik tillämpar regeln och tar bort alla export-objekt som inte är domänansluten objekt.

## <a name="sync-engine-identity-management-process"></a>Motorn identity management-synkroniseringen
process för hantering av hello identitet styr hur identitetsinformation uppdateras mellan olika anslutna datakällor. Identitetshantering sker i tre processer:

* Importera
* Synkronisering
* Exportera

Under importen hello utvärderar Synkroniseringsmotorn hello inkommande identitetsinformation från en ansluten datakälla. När ändringar identifieras den antingen skapar nya mellanlagrade objekt eller uppdaterar befintliga mellanlagrade objekt i hello anslutningsplatsen för synkronisering.

Under processen för synkronisering av hello Synkroniseringsmotorn uppdateras hello metaversum tooreflect ändringarna som skett i hello anslutningsplatsen samt hello connector utrymme tooreflect ändringarna som skett i hello metaversum.

Under exportprocessen hello skickar Synkroniseringsmotorn ändringar som mellanlagras på mellanlagring objekt och som är flaggade som väntande export.

hello följande bild visar där varje hello processer sker som identitet information flödar från en ansluten datakälla tooanother.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Importen
Under importen hello utvärderar Synkroniseringsmotorn tooidentity information om programuppdateringar. Synkroniseringsmotorn jämför hello identitetsinformation togs emot från hello anslutna datakällan med hello identitetsinformation om ett fristående objekt och avgör om hello mellanlagring objektet kräver uppdateringar. Om det är nödvändigt tooupdate hello mellanlagring objekt med nya data flaggas hello mellanlagring objekt som väntande import.

Synkroniseringsmotorn kan bearbeta endast hello identitetsinformation som har ändrats av mellanlagring objekt i hello anslutningsplatsen innan synkroniseringen. Den här processen innehåller hello följande fördelar:

* **Effektiv synkronisering**. hello mängden data som bearbetats under synkronisering minimeras.
* **Effektiv omsynkroniseringen**. Du kan ändra hur Synkroniseringsmotorn bearbetar identitetsinformation utan återanslutning hello sync motorn toohello datakälla.
* **Affärsmöjlighet toopreview synkronisering**. Du kan förhandsgranska synkronisering tooverify att din antaganden om hello identity management-processen är korrekta.

För varje objekt som angetts i hello koppling försöker hello Synkroniseringsmotorn toolocate en representation av hello objekt i hello anslutningsplatsen av hello Connector. Synkroniseringsmotorn undersöker alla mellanlagrade objekt i hello anslutningsplatsen och försöker toofind en motsvarande mellanlagrade objekt som har en matchande fästpunktsattributet. Om inga befintliga fristående objektet har en matchande fäster attribut, synkronisera engine försöker toofind en motsvarande mellanlagrade objekt med hello unikt samma namn.

När Synkroniseringsmotorn hittar ett fristående objekt som matchar med unika namn, men inte genom fästpunkt, händer hello särskilda följande:

* Om hello objekt finns i hello anslutningsplatsen har inga fästpunkten Synkroniseringsmotorn tar bort objektet från hello anslutningsplatsen och sedan markerar hello metaversumobjekt är det länkade tooas **försök etablering på nästa synkronisering kör**. Sedan skapar den hello nya importera objekt.
* Om hello-objekt som finns i hello anslutningsplatsen har ankare, förutsätter Synkroniseringsmotorn att det här objektet har antingen ändrats eller tagits bort i hello ansluten katalog. Tilldelar en tillfällig, nya unika namnet för hello utrymme kopplingsobjektet så att den kan mellanlagra hello inkommande objektet. hello gamla objektet blir **tillfälligt**, väntar på att hello Connector tooimport hello Byt namn på eller ta bort tooresolve hello situation.

Om Synkroniseringsmotorn hittar ett fristående objekt som motsvarar toohello objekt som angetts i hello anslutningstjänsten, anger vilken typ av ändringar tooapply. Till exempel Synkroniseringsmotorn kan byta namn på eller ta bort hello objekt i hello anslutna datakällan eller det kan bara uppdatera hello objektets attributvärden.

Mellanlagring objekt med uppdaterade data är markerade som väntande import. Det finns olika typer av väntande import. Beroende på hello resultatet av hello importen har ett fristående objekt i hello anslutningsplatsen något av följande typer av väntande import hello:

* **Ingen**. Det finns inga ändringar tooany hello attribut för hello mellanlagring objekt. Synkroniseringsmotorn flaggas inte den här typen som väntande import.
* **Lägg till**. hello mellanlagring objektet är ett nytt importera objekt i hello anslutningsplatsen. Synkroniseringsmotorn flaggor den här typen som väntande import för ytterligare bearbetning i hello metaversum.
* **Uppdatera**. Synkroniseringsmotorn söker efter ett motsvarande mellanlagrade objekt i hello anslutningsplatsen och flaggor den här typen som väntande import så att uppdateringar toohello attribut kan bearbetas i hello metaversum. Byta namn på objekt ingår.
* **Ta bort**. Synkroniseringsmotorn söker efter ett motsvarande mellanlagrade objekt i hello anslutningsplatsen och flaggor den här typen som väntande import så att hello domänanslutna objektet kan tas bort.
* **Ta bort/Lägg till**. Synkroniseringsmotorn söker efter ett motsvarande mellanlagrade objekt i hello anslutningsplatsen, men hello objekt av typen matchar inte. I det här fallet en delete-Lägg till ändring mellanlagras. En delete-Lägg till ändring anger toohello Synkroniseringsmotorn som en fullständig synkronisering av det här objektet måste inträffa eftersom olika uppsättningar av regler gäller toothis objekt när hello objekttypen ändras.

Det är möjligt med inställningen hello väntande importstatus av ett fristående objekt tooreduce hello avsevärt mängden data som bearbetas under synkroniseringen eftersom gör det blir hello system tooprocess endast de objekt som har uppdaterat data.

### <a name="synchronization-process"></a>Processen för synkronisering
Synkronisering består av två relaterade processer:

* Inkommande synkronisering när hello innehållet i hello metaversum uppdateras med hjälp av hello data i hello anslutningsplatsen.
* Utgående synkronisering när hello innehållet i hello anslutningsplatsen uppdateras med hjälp av data i hello metaversum.

Med hjälp av hello information mellanlagrad i hello anslutningsplatsen skapar hello inkommande synkroniseringsprocessen i hello metaversum hello integrerad vy över hello data som lagras i hello anslutna datakällor. Alla mellanlagrade objekt eller endast de som en väntande importera information aggregeras, beroende på hur hello regler är konfigurerade.

hello utgående synkronisering processen uppdateringar exportera objekt när metaversum objekt ändras.

Inkommande synkronisering skapar hello integrerad vy i hello metaversum hello identitet information som tas emot från hello anslutna datakällor. Synkroniseringsmotorn kan bearbeta identitetsinformation när som helst genom att använda hello senaste identitetsinformation att den har från hello ansluten datakälla.

**Inkommande synkronisering**

Inkommande synkronisering innehåller hello följande processer:

* **Etablera** (kallas även **projektion** om det är viktigt toodistinguish processen från utgående synkronisering etablering). Hej Synkroniseringsmotorn skapar ett nytt metaversumobjekt baserat på ett fristående objekt och länkar dem. Etablera är en åtgärd på objektnivå.
* **Ansluta till**. Hej Synkroniseringsmotorn länkar ett fristående objekt tooan befintliga metaversum-objekt. En koppling är en åtgärd på objektnivå.
* **Importera attributflöde**. Synkroniseringsmotorn uppdaterar hello attributvärden kallas attributflöde för hello objekt i hello metaversum. Importera attributflöde är ett attribut på objektnivå åtgärd som kräver en länk mellan ett fristående objekt och metaversum-objekt.

Etablera är hello endast process som skapar objekt i hello metaversum. Etablera påverkar endast importera objekt som är skilda objekt. Under etablera skapar Synkroniseringsmotorn ett metaversumobjekt som motsvarar toohello objekttypen för hello importera objekt och upprättar en länk mellan både objekt, vilket skapar ett objekt som är kopplade.

hello med processen upprättar även en länk mellan importera objekt och metaversum-objekt. hello skillnaden mellan koppling och etablera är att processen hello kräver hello importera objektet är länkade tooan befintliga metaversumobjekt, där hello etablera process skapar ett nytt metaversumobjekt.

Synkroniseringsmotorn försöker toojoin en importera objekt tooa metaversum-objekt med hjälp av villkor som anges i hello Synkroniseringsregeln konfiguration.

Synkroniseringsmotorn länkar under hello etablera och processer för anslutning till en frånkopplad objektet tooa metaversumobjekt, så att de anslutna. När dessa åtgärder på objektnivå har slutförts, kan Synkroniseringsmotorn uppdatera hello attributvärden för hello associerade metaversumobjekt. Den här processen kallas attributflöde för import.

Importera attributflöde uppstår på alla importera objekt som innehåller nya data och som är länkade tooa metaversumobjekt.

**Utgående synkronisering**

Utgående synkronisering uppdateringar exportera objekt när en metaversumobjekt ändra men tas inte bort. hello syftar utgående synkronisering tooevaluate om ändringar toometaverse objekt kräver uppdateringar toostaging objekt i hello-kopplingens utrymmen. I vissa fall kan uppdateras ändringar kan kräva att mellanlagring objekt i alla kopplingens utrymmen hello. Mellanlagring objekt som har ändrats flaggas som väntande export, gör dem exportera objekt. Dessa exportalternativ objekt senare push-installeras toohello anslutna datakällan under hello exporten.

Utgående synkronisering har tre processer:

* **Etablering**
* **Avetablering**
* **Exportera attributflöde**

Etablering och borttagning finns både på objektnivå-operationer. Avetablering beror på etablering eftersom endast etablering kan starta den. Avetablering utlöses vid etablering tar bort hello länken mellan ett metaversumobjekt och en export-objekt.

Etablering utlöses alltid när ändringarna har tillämpade tooobjects i hello metaversum. När ändringar görs toometaverse objekt, utför Synkroniseringsmotorn någon av följande aktiviteter som en del av hello etableringsprocessen hello:

* Skapa kopplade objekt, där en metaversumobjekt är länkade tooa nyskapad export-objekt.
* Byt namn på en domänansluten objekt.
* Sedan en frånkoppling av länkar mellan en metaversumobjekt och organisering objekt, skapa en frånkopplad objekt.

Om etablering kräver synkronisering motorn toocreate ett nytt connector-objekt, är hello mellanlagring objektet toowhich hello metaversumobjekt är länkad alltid en export-objektet, eftersom hello objektet inte finns ännu i hello anslutna datakällan.

Om etablering kräver synkronisering motorn toodisjoin en domänansluten objekt, skapar en frånkopplad objekt utlöses avetablering. Hej avetablering processen tar bort hello-objekt.

Under avetablering, påverkas ett exportera objekt bort inte fysiskt hello-objektet. hello objekt flaggas som **bort**, vilket innebär att hello borttagningsåtgärd mellanlagras på hello-objekt.

Exportera attributflöde inträffar också under hello utgående synkroniseringsprocessen liknande sätt som toohello som importerar attributflöde inträffar under inkommande synkronisering. Exportera attributflöde sker bara mellan metaversum och exportera objekt som är anslutna.

### <a name="export-process"></a>Exportprocess
Under exportprocessen hello Synkroniseringsmotorn undersöker alla export-objekt som är flaggade som väntande export i hello anslutningsplatsen och sedan skickar uppdateringar toohello ansluten datakälla.

Hej Synkroniseringsmotorn kan fastställa hello framgång för en export men tillräckligt bestämma inte att hello identity management processen har slutförts. Objekt i hello anslutna datakällan kan alltid ändras av andra processer. Synkroniseringsmotorn har inte en beständig anslutning toohello anslutna datakällan, men det är inte tillräcklig toomake antaganden om hello egenskaper för ett objekt i hello anslutna datakällan endast baseras på ett meddelande om lyckade exporten.

Till exempel en process i hello anslutna datakällan kan ändra hello objektets attribut tillbaka tootheir ursprungliga värden (det vill säga hello anslutna datakällan kan du skriva över hello värden omedelbart efter hello data skickas ut av Synkroniseringsmotorn och har tillämpas i hello anslutna datakällan).

hello sync motorn lagrar exportera och importera statusinformation om varje fristående objekt. Om du värdena för hello-attribut som anges i hello lista över attribut har ändrats sedan senaste export av hello, hello lagring av importera och exportera status aktiverar synkronisering motorn tooreact på lämpligt sätt. Synkroniseringsmotorn använder hello importera processen tooconfirm attributvärden som har exporterade toohello anslutna datakällan. En jämförelse mellan hello importerade och exporterade information som visas i följande bild, hello aktiverar synkronisering motorn toodetermine om hello exporten lyckades eller om det behövs toobe upprepas.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Till exempel Synkroniseringsmotorn exporterar attributet C, som har värdet 5 tooa ansluten datakälla, C = 5 lagras i minnet export status. Varje ytterligare export på det här objektet resulterar i ett försök tooexport C = 5 toohello anslutna datakällan igen eftersom Synkroniseringsmotorn förutsätter att det här värdet inte har beständigt tillämpade toohello objekt (det vill säga om ett annat värde har importerats nyligen (ansluten datakälla från hello). hello export minne rensas när C = 5 tas emot vid en import på hello-objekt.

## <a name="next-steps"></a>Nästa steg
Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

