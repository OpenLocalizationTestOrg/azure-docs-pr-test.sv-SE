---
title: aaaAzure CDN regler modulreferens | Microsoft Docs
description: "I referensdokumentationen för Azure CDN regler motorn matchar villkoren och funktioner."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 4159715d15fc792afb49dcd2a30752fabcd94a38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine"></a>Azure CDN regelmotor
Det här avsnittet innehåller detaljerade beskrivningar av hello tillgängliga matchar villkoren och funktioner för Azure Content Delivery Network (CDN) [regelmotor](cdn-rules-engine.md).

hello HTTP regelmotor är utformad toobe hello slutliga auktoritet på hur vissa typer av begäranden bearbetas av hello CDN.

**Vanliga användningsområden**:

- Åsidosätta eller definiera en anpassad princip.
- Skydda eller neka begäranden för känsligt innehåll.
- Omdirigeringsbegäranden.
- Lagra anpassade loggdata.

## <a name="terminology"></a>Terminologi
En regel definieras via hello [ **villkorsuttryck**](cdn-rules-engine-reference-conditional-expressions.md), [ **matchar**](cdn-rules-engine-reference-match-conditions.md), och [ ** funktioner**](cdn-rules-engine-reference-features.md). Dessa element markeras i hello följande illustration.

 ![CDN matchar villkoret](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Syntax

hello sätt specialtecken kommer att behandlas varierar bl.a toohow matchar villkor eller funktionen handtag textvärden. En matchar villkoret eller funktion kan tolka text i en hello följande sätt:

1. [**Teckenvärden**](#literal-values) 
2. [**Jokertecken värden**](#wildcard-values)
3. [**Reguljära uttryck**](#regular-expressions)

### <a name="literal-values"></a>Teckenvärden
Text som ska tolkas som ett litteralvärde hanterar specialtecken felaktigt, med undantag för hello av hello % symbolen som en del av hello-värde som måste matchas. Med andra ord en literal matchar villkoret för`\'*'\` bara uppfyllas om som exakt värde (d.v.s. `\'*'\`) hittas.
 
En procentsymbol är används tooindicate URL-kodning (t.ex. `%20`).

### <a name="wildcard-values"></a>Jokertecken värden
Text som ska tolkas som ett jokerteckenvärde tilldelas ytterligare betydelse toospecial tecken. hello i den följande tabellen beskrivs hur hello följande tecknen ska tolkas.

Tecken | Beskrivning
----------|------------
\ | Ett omvänt snedstreck är används tooescape hello tecken har angetts i den här tabellen. Ett omvänt snedstreck måste anges direkt innan hello specialtecken som ska undantas (escaped).<br/>Till exempel hoppas hello följa en asterisk:`\*`
% | En procentsymbol är används tooindicate URL-kodning (t.ex. `%20`).
* | En asterisk är ett jokertecken som representerar ett eller flera tecken.
Diskutrymme | Ett blankstegstecken anger att matchar villkor kan uppfyllas genom att antingen hello som angetts värden eller mönster.
'value' | Enkla citattecken har inte någon särskild innebörd. En uppsättning enkla citattecken används dock tooindicate som ett värde som ska behandlas som ett litteralvärde. Den kan användas i hello följande sätt:<br><br/>-Kan en matchar villkoret toobe uppfyllas när hello som angetts matchar någon del av hello Jämförelsevärde.  Till exempel `'ma'` matchar någon av följande strängar hello: <br/><br/>/Business/**ma**rathon/asset.htm<br/>**Ma**p.gif<br/>/ företag mallen. **ma**p<br /><br />-Kan en specialtecken toobe som angetts som en teckensträng. Du kan exempelvis ange literal blanksteg genom att skriva ett blanksteg i en uppsättning enkla citattecken (d.v.s. `' '` eller `'sample value'`).<br/>-Kan ett tomt värde toobe anges. Ange ett tomt värde genom att ange en uppsättning enkla citattecken (d.v.s. '').<br /><br/>**Viktigt:**<br/>-Om hello anges värdet inte innehåller ett jokertecken, sedan betraktas automatiskt ett litteralvärde. Det innebär att det inte är nödvändigt toospecify en uppsättning enkla citattecken.<br/>– Om ett omvänt snedstreck inte escape-tecknet i den här tabellen, kommer sedan den att ignoreras när anges i en uppsättning med enkla citattecken.<br/>-Ett annat sätt toospecify specialtecken som en teckensträng är tooescape den med ett omvänt snedstreck (d.v.s. `\`).

### <a name="regular-expressions"></a>Reguljära uttryck

Reguljära uttryck definiera ett mönster som ska genomsökas för inom ett textvärde. Vanliga uttryck definierar särskild innebörd tooa antal symboler. hello följande tabell visar hur specialtecken behandlas som matchar villkoren och funktioner som har stöd för reguljära uttryck.

Specialtecken | Beskrivning
------------------|------------
\ | Ett omvänt snedstreck visar hello tecken hello följer den. Detta gör att tecknet toobe behandlas som litteralt värde i stället för att ta med på innebörd reguljärt uttryck. Till exempel hoppas hello följa en asterisk:`\*`
% | hello innebörden av symbolen procentandel beror på dess användning.<br/><br/> `%{HTTPVariable}`: Den här syntaxen identifierar en HTTP-variabel.<br/>`%{HTTPVariable%Pattern}`: Den här syntaxen använder en procentandel symbolen tooidentify en HTTP-variabeln och som avgränsare.<br />`\%`: Undantagstecken symbolen procent kan det toobe som används som ett litteralvärde eller tooindicate URL-kodning (t.ex. `\%20`).
* | En asterisk kan hello föregående tecken toobe matchar noll eller flera gånger. 
Diskutrymme | Ett blankstegstecken behandlas vanligtvis som en teckensträng. 
'value' | Enkla citattecken behandlas som litteraler. En uppsättning enkla citattecken har inte någon särskild innebörd.


## <a name="next-steps"></a>Nästa steg
* [Regler motorn matchar villkor](cdn-rules-engine-reference-match-conditions.md)
* [Regler motorn villkorsuttryck](cdn-rules-engine-reference-conditional-expressions.md)
* [Regler motorn funktioner](cdn-rules-engine-reference-features.md)
* [Åsidosätta HTTP standardinställningar med hjälp av hello regelmotor](cdn-rules-engine.md)
* [Azure CDN-översikt](cdn-overview.md)
