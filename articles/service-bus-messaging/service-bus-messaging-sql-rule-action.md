---
title: aaaSQLRuleAction syntax referens i Azure | Microsoft Docs
description: Information om SQLRuleAction grammatik.
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
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 8ef281f942847bcc535b83a5ffb30d03539734f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sqlruleaction-syntax"></a><span data-ttu-id="7d5e1-103">SQLRuleAction syntax</span><span class="sxs-lookup"><span data-stu-id="7d5e1-103">SQLRuleAction syntax</span></span>

<span data-ttu-id="7d5e1-104">En *SqlRuleAction* är en instans av hello [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) klass och representerar uppsättning åtgärder på SQL-språk baserat syntax som utförs mot en [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="7d5e1-104">A *SqlRuleAction* is an instance of hello [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) class, and represents set of actions written in SQL-language based syntax that is performed against a [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span>   
  
<span data-ttu-id="7d5e1-105">Det här avsnittet innehåller information om hello SQL regeln åtgärd grammatik.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-105">This topic lists details about hello SQL rule action grammar.</span></span>  
  
```  
<statements> ::=
    <statement> [, ...n]  
  
```  
  
```  
<statement> ::=
    <action> [;]
    Remarks
    -------
    Semicolon is optional.  
  
```  
  
```  
<action> ::=
    SET <property> = <expression>
    REMOVE <property>  
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
  
## <a name="arguments"></a><span data-ttu-id="7d5e1-106">Argument</span><span class="sxs-lookup"><span data-stu-id="7d5e1-106">Arguments</span></span>  
  
-   <span data-ttu-id="7d5e1-107">`<scope>`är en valfri sträng som anger hello omfattning hello `<property_name>`.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-107">`<scope>` is an optional string indicating hello scope of hello `<property_name>`.</span></span> <span data-ttu-id="7d5e1-108">Giltiga värden är `sys` eller `user`.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-108">Valid values are `sys` or `user`.</span></span> <span data-ttu-id="7d5e1-109">Hej `sys` värdet anger scope för system där `<property_name>` är en offentlig egenskapsnamnet på hello [BrokeredMessage klassen](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="7d5e1-109">hello `sys` value indicates system scope where `<property_name>` is a public property name of hello [BrokeredMessage Class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="7d5e1-110">`user`Anger användarens scope där `<property_name>` är en nyckel för hello [BrokeredMessage klassen](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) ordlistan.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-110">`user` indicates user scope where `<property_name>` is a key of hello [BrokeredMessage Class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) dictionary.</span></span> <span data-ttu-id="7d5e1-111">`user`omfånget är hello standardomfång om `<scope>` har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-111">`user` scope is hello default scope if `<scope>` is not specified.</span></span>  
  
### <a name="remarks"></a><span data-ttu-id="7d5e1-112">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="7d5e1-112">Remarks</span></span>  

<span data-ttu-id="7d5e1-113">Ett försök tooaccess en obefintlig systemegenskap är ett fel när ett försök tooaccess en obefintlig Användaregenskapen inte är ett fel.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-113">An attempt tooaccess a non-existent system property is an error, while an attempt tooaccess a non-existent user property is not an error.</span></span> <span data-ttu-id="7d5e1-114">I stället utvärderas en obefintlig användaregenskap internt som ett okänt värde.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-114">Instead, a non-existent user property is internally evaluated as an unknown value.</span></span> <span data-ttu-id="7d5e1-115">Ett okänt värde behandlas särskilt under utvärdering av operatorn.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-115">An unknown value is treated specially during operator evaluation.</span></span>  
  
## <a name="propertyname"></a><span data-ttu-id="7d5e1-116">kubattributbindningen</span><span class="sxs-lookup"><span data-stu-id="7d5e1-116">property_name</span></span>  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a><span data-ttu-id="7d5e1-117">Argument</span><span class="sxs-lookup"><span data-stu-id="7d5e1-117">Arguments</span></span>  
 <span data-ttu-id="7d5e1-118">`<regular_identifier>`är en sträng som representeras av hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="7d5e1-118">`<regular_identifier>` is a string represented by hello following regular expression:</span></span>  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
 <span data-ttu-id="7d5e1-119">Det innebär att alla strängar som börjar med en bokstav och följs av en eller flera understreck/bokstav/siffra.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-119">This means any string that starts with a letter and is followed by one or more underscore/letter/digit.</span></span>  
  
 <span data-ttu-id="7d5e1-120">`[:IsLetter:]`innebär att alla Unicode-tecken som är kategoriserade som ett Unicode-tecken.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-120">`[:IsLetter:]` means any Unicode character that is categorized as a Unicode letter.</span></span> <span data-ttu-id="7d5e1-121">`System.Char.IsLetter(c)`Returnerar `true` om `c` är ett Unicode-tecken.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-121">`System.Char.IsLetter(c)` returns `true` if `c` is a Unicode letter.</span></span>  
  
 <span data-ttu-id="7d5e1-122">`[:IsDigit:]`innebär att alla Unicode-tecken som är kategoriserade som en siffra.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-122">`[:IsDigit:]` means any Unicode character that is categorized as a decimal digit.</span></span> <span data-ttu-id="7d5e1-123">`System.Char.IsDigit(c)`Returnerar `true` om `c` är en Unicode-siffra.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-123">`System.Char.IsDigit(c)` returns `true` if `c` is a Unicode digit.</span></span>  
  
 <span data-ttu-id="7d5e1-124">En `<regular_identifier>` får inte vara ett reserverat nyckelord.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-124">A `<regular_identifier>` cannot be a reserved keyword.</span></span>  
  
 <span data-ttu-id="7d5e1-125">`<delimited_identifier>`är en sträng som medföljde åt vänster och höger hakparentes ([]).</span><span class="sxs-lookup"><span data-stu-id="7d5e1-125">`<delimited_identifier>` is any string that is enclosed with left/right square brackets ([]).</span></span> <span data-ttu-id="7d5e1-126">Höger hakparentes representeras som två höger hakparentes.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-126">A right square bracket is represented as two right square brackets.</span></span> <span data-ttu-id="7d5e1-127">hello följande är exempel på `<delimited_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="7d5e1-127">hello following are examples of `<delimited_identifier>`:</span></span>  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
 <span data-ttu-id="7d5e1-128">`<quoted_identifier>`är en sträng som omges av dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-128">`<quoted_identifier>` is any string that is enclosed with double quotation marks.</span></span> <span data-ttu-id="7d5e1-129">Ett dubbelt citattecken i identifierare representeras av dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-129">A double quotation mark in identifier is represented as two double quotation marks.</span></span> <span data-ttu-id="7d5e1-130">Det rekommenderas inte toouse identifierare eftersom den enkelt kan förväxlas med en strängkonstant.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-130">It is not recommended toouse quoted identifiers because it can easily be confused with a string constant.</span></span> <span data-ttu-id="7d5e1-131">Använd om möjligt en avgränsad identifierare.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-131">Use a delimited identifier if possible.</span></span> <span data-ttu-id="7d5e1-132">hello följande är ett exempel på `<quoted_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="7d5e1-132">hello following is an example of `<quoted_identifier>`:</span></span>  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a><span data-ttu-id="7d5e1-133">Mönstret</span><span class="sxs-lookup"><span data-stu-id="7d5e1-133">pattern</span></span>  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="7d5e1-134">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="7d5e1-134">Remarks</span></span>
  
 <span data-ttu-id="7d5e1-135">`<pattern>`måste vara ett uttryck som utvärderas som en sträng.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-135">`<pattern>` must be an expression that is evaluated as a string.</span></span> <span data-ttu-id="7d5e1-136">Den används som ett mönster för hello som operator.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-136">It is used as a pattern for hello LIKE operator.</span></span>      <span data-ttu-id="7d5e1-137">Det kan innehålla följande jokertecken hello:</span><span class="sxs-lookup"><span data-stu-id="7d5e1-137">It can contain hello following wildcard characters:</span></span>  
  
-   <span data-ttu-id="7d5e1-138">`%`: En sträng med noll eller flera tecken.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-138">`%`:  Any string of zero or more characters.</span></span>  
  
-   <span data-ttu-id="7d5e1-139">`_`: Ett valfritt tecken.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-139">`_`: Any single character.</span></span>  
  
## <a name="escapechar"></a><span data-ttu-id="7d5e1-140">escape_char</span><span class="sxs-lookup"><span data-stu-id="7d5e1-140">escape_char</span></span>  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="7d5e1-141">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="7d5e1-141">Remarks</span></span>
  
 <span data-ttu-id="7d5e1-142">`<escape_char>`måste vara ett uttryck som utvärderas som en sträng med längden 1.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-142">`<escape_char>` must be an expression that is evaluated as a string of length 1.</span></span> <span data-ttu-id="7d5e1-143">Den används som escape-tecken för hello som operator.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-143">It is used as an escape character for hello LIKE operator.</span></span>  
  
 <span data-ttu-id="7d5e1-144">Till exempel `property LIKE 'ABC\%' ESCAPE '\'` matchar `ABC%` i stället för en sträng som börjar med `ABC`.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-144">For example, `property LIKE 'ABC\%' ESCAPE '\'` matches `ABC%` rather than a string that starts with `ABC`.</span></span>  
  
## <a name="constant"></a><span data-ttu-id="7d5e1-145">konstant</span><span class="sxs-lookup"><span data-stu-id="7d5e1-145">constant</span></span>  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a><span data-ttu-id="7d5e1-146">Argument</span><span class="sxs-lookup"><span data-stu-id="7d5e1-146">Arguments</span></span>  
  
-   <span data-ttu-id="7d5e1-147">`<integer_constant>`är en sträng som inte är inom citattecken och inte innehåller decimaltecken.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-147">`<integer_constant>` is a string of numbers that are not enclosed in quotation marks and do not contain decimal points.</span></span> <span data-ttu-id="7d5e1-148">hello värdena lagras som `System.Int64` internt, och följ hello samma intervall.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-148">hello values are stored as `System.Int64` internally, and follow hello same range.</span></span>  
  
     <span data-ttu-id="7d5e1-149">hello följande är exempel på lång konstanter:</span><span class="sxs-lookup"><span data-stu-id="7d5e1-149">hello following are examples of long constants:</span></span>  
  
    ```  
    1894  
    2  
    ```  
  
-   <span data-ttu-id="7d5e1-150">`<decimal_constant>`är en sträng som inte är inom citattecken och innehåller ett decimaltecken.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-150">`<decimal_constant>` is a string of numbers that are not enclosed in quotation marks, and contain a decimal point.</span></span> <span data-ttu-id="7d5e1-151">hello värdena lagras som `System.Double` internt, och följ hello samma intervall/precision.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-151">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span>  
  
     <span data-ttu-id="7d5e1-152">I en framtida version siffran kan lagras i en annan typ toosupport exakt nummer format, så du inte bör lita på hello fakta hello underliggande-datatypen är `System.Double` för `<decimal_constant>`.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-152">In a future version, this number might be stored in a different data type toosupport exact number semantics, so you should not rely on hello fact hello underlying data type is `System.Double` for `<decimal_constant>`.</span></span>  
  
     <span data-ttu-id="7d5e1-153">hello följande är exempel på decimal konstanter:</span><span class="sxs-lookup"><span data-stu-id="7d5e1-153">hello following are examples of decimal constants:</span></span>  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   <span data-ttu-id="7d5e1-154">`<approximate_number_constant>`är ett antal skrivna i matematisk notation.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-154">`<approximate_number_constant>` is a number written in scientific notation.</span></span> <span data-ttu-id="7d5e1-155">hello värdena lagras som `System.Double` internt, och följ hello samma intervall/precision.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-155">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span> <span data-ttu-id="7d5e1-156">hello följande är exempel på ungefärliga numeriska konstanter:</span><span class="sxs-lookup"><span data-stu-id="7d5e1-156">hello following are examples of approximate number constants:</span></span>  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a><span data-ttu-id="7d5e1-157">boolean_constant</span><span class="sxs-lookup"><span data-stu-id="7d5e1-157">boolean_constant</span></span>  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a><span data-ttu-id="7d5e1-158">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="7d5e1-158">Remarks</span></span>
  
<span data-ttu-id="7d5e1-159">Booleska konstanter som representeras av hello nyckelord `TRUE` eller `FALSE`.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-159">Boolean constants are represented by hello keywords `TRUE` or `FALSE`.</span></span> <span data-ttu-id="7d5e1-160">hello värdena lagras som `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-160">hello values are stored as `System.Boolean`.</span></span>  
  
## <a name="stringconstant"></a><span data-ttu-id="7d5e1-161">string_constant</span><span class="sxs-lookup"><span data-stu-id="7d5e1-161">string_constant</span></span>  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a><span data-ttu-id="7d5e1-162">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="7d5e1-162">Remarks</span></span>
  
<span data-ttu-id="7d5e1-163">Strängkonstanter omges av enkla citattecken och inkludera alla giltiga Unicode-tecken.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-163">String constants are enclosed in single quotation marks and include any valid Unicode characters.</span></span> <span data-ttu-id="7d5e1-164">Ett enkelt citattecken inbäddat i en strängkonstant representeras som två enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-164">A single quotation mark embedded in a string constant is represented as two single quotation marks.</span></span>  
  
## <a name="function"></a><span data-ttu-id="7d5e1-165">Funktionen</span><span class="sxs-lookup"><span data-stu-id="7d5e1-165">function</span></span>  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a><span data-ttu-id="7d5e1-166">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="7d5e1-166">Remarks</span></span>  

<span data-ttu-id="7d5e1-167">Hej `newid()` fungerar returnerar en **System.Guid** genereras av hello `System.Guid.NewGuid()` metod.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-167">hello `newid()` function returns a **System.Guid** generated by hello `System.Guid.NewGuid()` method.</span></span>  
  
<span data-ttu-id="7d5e1-168">Hej `property(name)` returnerar funktionen hello värde för hello-egenskap som refererar till `name`.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-168">hello `property(name)` function returns hello value of hello property referenced by `name`.</span></span> <span data-ttu-id="7d5e1-169">Hej `name` värdet kan vara ett uttryck som returnerar ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-169">hello `name` value can be any valid expression that returns a string value.</span></span>  
  
## <a name="considerations"></a><span data-ttu-id="7d5e1-170">Överväganden</span><span class="sxs-lookup"><span data-stu-id="7d5e1-170">Considerations</span></span>

- <span data-ttu-id="7d5e1-171">Använda toocreate ett nytt egenskapen eller update hello värde för en befintlig egenskap är.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-171">SET is used toocreate a new property or update hello value of an existing property.</span></span>
- <span data-ttu-id="7d5e1-172">Ta bort är används tooremove en egenskap.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-172">REMOVE is used tooremove a property.</span></span>
- <span data-ttu-id="7d5e1-173">Ange utför implicit konvertering om möjligt när hello uttryckstyp och hello befintliga egenskapstypen är olika.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-173">SET performs implicit conversion if possible when hello expression type and hello existing property type are different.</span></span>
- <span data-ttu-id="7d5e1-174">Åtgärden misslyckas om obefintlig Systemegenskaper har referenser till.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-174">Action fails if non-existent system properties were referenced.</span></span>
- <span data-ttu-id="7d5e1-175">Åtgärden misslyckas inte om ej existerande användaregenskaper har referenser till.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-175">Action does not fail if non-existent user properties were referenced.</span></span>
- <span data-ttu-id="7d5e1-176">En egenskap för obefintlig utvärderas som ”okänt” internt, följande hello samma semantik som [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) vid utvärdering av operatörer.</span><span class="sxs-lookup"><span data-stu-id="7d5e1-176">A non-existent user property is evaluated as "Unknown" internally, following hello same semantics as [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) when evaluating operators.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d5e1-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7d5e1-177">Next steps</span></span>

- [<span data-ttu-id="7d5e1-178">SQLRuleAction-klass</span><span class="sxs-lookup"><span data-stu-id="7d5e1-178">SQLRuleAction class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [<span data-ttu-id="7d5e1-179">SQLFilter-klass</span><span class="sxs-lookup"><span data-stu-id="7d5e1-179">SQLFilter class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
