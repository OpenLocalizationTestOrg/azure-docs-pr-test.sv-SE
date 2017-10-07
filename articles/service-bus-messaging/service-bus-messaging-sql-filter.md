---
title: "aaaAzure söksyntax för Service Bus SQLFilter | Microsoft Docs"
description: Information om SQLFilter grammatik.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: ea49d42e343a6b324eb34c7831ff6be2855346e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sqlfilter-syntax"></a>SQLFilter syntax

En *SqlFilter* är en instans av hello [SqlFilter klassen](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), och representerar en SQL-baserad filteruttryck som ska utvärderas mot en [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). En SqlFilter stöder en delmängd av hello SQL-92-standard.  
  
 Det här avsnittet innehåller information om SqlFilter grammatik.  
  
```  
<predicate ::=  
      { NOT <predicate> }  
      | <predicate> AND <predicate>  
      | <predicate> OR <predicate>  
      | <expression> { = | <> | != | > | >= | < | <= } <expression>  
      | <property> IS [NOT] NULL  
      | <expression> [NOT] IN ( <expression> [, ...n] )  
      | <expression> [NOT] LIKE <pattern> [ESCAPE <escape_char>]  
      | EXISTS ( <property> )  
      | ( <predicate> )  
  
```  
  
```  
<expression> ::=  
      <constant>   
      | <function>  
      | <property>  
      | <expression> { + | - | * | / | % } <expression>  
      | { + | - } <expression>  
      | ( <expression> )  
  
```  
  
```  
<property> :=   
       [<scope> .] <property_name>  
  
```  
  
## <a name="arguments"></a>Argument  
  
-   `<scope>`är en valfri sträng som anger hello omfattning hello `<property_name>`. Giltiga värden är `sys` eller `user`. Hej `sys` värdet anger scope för system där `<property_name>` är en offentlig egenskapsnamnet på hello [BrokeredMessage klassen](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). `user`Anger användarens scope där `<property_name>` är en nyckel för hello [BrokeredMessage klassen](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) ordlistan. `user`omfånget är hello standardomfång om `<scope>` har inte angetts.  
  
## <a name="remarks"></a>Kommentarer

Ett försök tooaccess en obefintlig systemegenskap är ett fel när ett försök tooaccess en obefintlig Användaregenskapen inte är ett fel. I stället utvärderas en obefintlig användaregenskap internt som ett okänt värde. Ett okänt värde behandlas särskilt under utvärdering av operatorn.  
  
## <a name="propertyname"></a>kubattributbindningen  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a>Argument  

 `<regular_identifier>`är en sträng som representeras av hello följande reguljära uttryck:  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
Denna grammatik innebär alla strängar som börjar med en bokstav och följs av en eller flera understreck/bokstav/siffra.  
  
`[:IsLetter:]`innebär att alla Unicode-tecken som är kategoriserade som ett Unicode-tecken. `System.Char.IsLetter(c)`Returnerar `true` om `c` är ett Unicode-tecken.  
  
`[:IsDigit:]`innebär att alla Unicode-tecken som är kategoriserade som en siffra. `System.Char.IsDigit(c)`Returnerar `true` om `c` är en Unicode-siffra.  
  
En `<regular_identifier>` får inte vara ett reserverat nyckelord.  
  
`<delimited_identifier>`är en sträng som medföljde åt vänster och höger hakparentes ([]). Höger hakparentes representeras som två höger hakparentes. hello följande är exempel på `<delimited_identifier>`:  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
`<quoted_identifier>`är en sträng som omges av dubbla citattecken. Ett dubbelt citattecken i identifierare representeras av dubbla citattecken. Det rekommenderas inte toouse identifierare eftersom den enkelt kan förväxlas med en strängkonstant. Använd om möjligt en avgränsad identifierare. hello följande är ett exempel på `<quoted_identifier>`:  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a>Mönstret  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Kommentarer
  
`<pattern>`måste vara ett uttryck som utvärderas som en sträng. Den används som ett mönster för hello som operator.      Det kan innehålla följande jokertecken hello:  
  
-   `%`: En sträng med noll eller flera tecken.  
  
-   `_`: Ett valfritt tecken.  
  
## <a name="escapechar"></a>escape_char  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Kommentarer  

`<escape_char>`måste vara ett uttryck som utvärderas som en sträng med längden 1. Den används som escape-tecken för hello som operator.  
  
 Till exempel `property LIKE 'ABC\%' ESCAPE '\'` matchar `ABC%` i stället för en sträng som börjar med `ABC`.  
  
## <a name="constant"></a>konstant  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a>Argument  
  
-   `<integer_constant>`är en sträng som inte är inom citattecken och inte innehåller decimaltecken. hello värdena lagras som `System.Int64` internt, och följ hello samma intervall.  
  
     Det här är exempel på lång konstanter:  
  
    ```  
    1894  
    2  
    ```  
  
-   `<decimal_constant>`är en sträng som inte är inom citattecken och innehåller ett decimaltecken. hello värdena lagras som `System.Double` internt, och följ hello samma intervall/precision.  
  
     I en framtida version siffran kan lagras i en annan typ toosupport exakt nummer format, så du inte bör lita på hello fakta hello underliggande-datatypen är `System.Double` för `<decimal_constant>`.  
  
     hello följande är exempel på decimal konstanter:  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   `<approximate_number_constant>`är ett antal skrivna i matematisk notation. hello värdena lagras som `System.Double` internt, och följ hello samma intervall/precision. hello följande är exempel på ungefärliga numeriska konstanter:  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a>boolean_constant  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a>Kommentarer  

Booleska konstanter som representeras av hello nyckelord **SANT** eller **FALSKT**. hello värdena lagras som `System.Boolean`.  
  
## <a name="stringconstant"></a>string_constant  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a>Kommentarer  

Strängkonstanter omges av enkla citattecken och inkludera alla giltiga Unicode-tecken. Ett enkelt citattecken inbäddat i en strängkonstant representeras som två enkla citattecken.  
  
## <a name="function"></a>Funktionen  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a>Kommentarer
  
Hej `newid()` fungerar returnerar en **System.Guid** genereras av hello `System.Guid.NewGuid()` metod.  
  
Hej `property(name)` returnerar funktionen hello värde för hello-egenskap som refererar till `name`. Hej `name` värdet kan vara ett uttryck som returnerar ett strängvärde.  
  
## <a name="considerations"></a>Överväganden
  
Tänk hello följande [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantik:  
  
-   Egenskapsnamnen är skiftlägeskänsliga.  
  
-   Operatörer Följ C# implicit konvertering semantik när det är möjligt.  
  
-   Systemegenskaper är offentliga egenskaper som exponeras i [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) instanser.  
  
    Tänk hello följande `IS [NOT] NULL` semantik:  
  
    -   `property IS NULL`utvärderas som `true` om antingen hello-egenskapen inte finns eller hello egenskapens värde är `null`.  
  
### <a name="property-evaluation-semantics"></a>Egenskapen utvärdering semantik  
  
-   Ett försök tooevaluate en obefintlig Systemegenskapen utlöser en [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) undantag.  
  
-   En egenskap som inte finns internt utvärderas som **okänd**.  
  
 Okänd utvärdering i aritmetiska operatorerna:  
  
-   För binära operatorer om antingen hello vänster eller höger sida av operander utvärderas som **okänd**, och sedan hello resultatet är **okänd**.  
  
-   För unära operatorer, om en operand utvärderas som **okänd**, och sedan hello resultatet är **okänd**.  
  
 Okänd utvärdering i binära operatorer:  
  
-   Om antingen hello vänster eller höger sida av operander utvärderas som **okänd**, och sedan hello resultatet är **okänd**.  
  
 Okänd utvärdering i `[NOT] LIKE`:  
  
-   Om operanden alla utvärderas som **okänd**, och sedan hello resultatet är **okänd**.  
  
 Okänd utvärdering i `[NOT] IN`:  
  
-   Om hello vänsteroperanden utvärderas som **okänd**, och sedan hello resultatet är **okänd**.  
  
 Okänd utvärdering i **och** operator:  
  
```  
+---+---+---+---+  
|AND| T | F | U |  
+---+---+---+---+  
| T | T | F | U |  
+---+---+---+---+  
| F | F | F | F |  
+---+---+---+---+  
| U | U | F | U |  
+---+---+---+---+  
```  
  
 Okänd utvärdering i **eller** operator:  
  
```  
+---+---+---+---+  
|OR | T | F | U |  
+---+---+---+---+  
| T | T | T | T |  
+---+---+---+---+  
| F | T | F | U |  
+---+---+---+---+  
| U | T | U | U |  
+---+---+---+---+  
```  
  
### <a name="operator-binding-semantics"></a>Operatorn bindning semantik
  
-   Jämförelseoperatorer som `>`, `>=`, `<`, `<=`, `!=`, och `=` Följ hello samma semantik som hello C#-operatorn bindande i data skriver kampanjer och implicita konverteringar.  
  
-   Aritmetiska operatorer som `+`, `-`, `*`, `/`, och `%` Följ hello samma semantik som hello C#-operatorn bindande i data skriver kampanjer och implicita konverteringar.

## <a name="next-steps"></a>Nästa steg

- [SQLFilter-klass](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [SQLRuleAction-klass](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)