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
# <a name="sqlfilter-syntax"></a><span data-ttu-id="fd3a6-103">SQLFilter syntax</span><span class="sxs-lookup"><span data-stu-id="fd3a6-103">SQLFilter syntax</span></span>

<span data-ttu-id="fd3a6-104">En *SqlFilter* är en instans av hello [SqlFilter klassen](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), och representerar en SQL-baserad filteruttryck som ska utvärderas mot en [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="fd3a6-104">A *SqlFilter* is an instance of hello [SqlFilter class](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), and represents a SQL language-based filter expression that is evaluated against a [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="fd3a6-105">En SqlFilter stöder en delmängd av hello SQL-92-standard.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-105">A SqlFilter supports a subset of hello SQL-92 standard.</span></span>  
  
 <span data-ttu-id="fd3a6-106">Det här avsnittet innehåller information om SqlFilter grammatik.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-106">This topic lists details about SqlFilter grammar.</span></span>  
  
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
  
## <a name="arguments"></a><span data-ttu-id="fd3a6-107">Argument</span><span class="sxs-lookup"><span data-stu-id="fd3a6-107">Arguments</span></span>  
  
-   <span data-ttu-id="fd3a6-108">`<scope>`är en valfri sträng som anger hello omfattning hello `<property_name>`.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-108">`<scope>` is an optional string indicating hello scope of hello `<property_name>`.</span></span> <span data-ttu-id="fd3a6-109">Giltiga värden är `sys` eller `user`.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-109">Valid values are `sys` or `user`.</span></span> <span data-ttu-id="fd3a6-110">Hej `sys` värdet anger scope för system där `<property_name>` är en offentlig egenskapsnamnet på hello [BrokeredMessage klassen](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="fd3a6-110">hello `sys` value indicates system scope where `<property_name>` is a public property name of hello [BrokeredMessage class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="fd3a6-111">`user`Anger användarens scope där `<property_name>` är en nyckel för hello [BrokeredMessage klassen](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) ordlistan.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-111">`user` indicates user scope where `<property_name>` is a key of hello [BrokeredMessage class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) dictionary.</span></span> <span data-ttu-id="fd3a6-112">`user`omfånget är hello standardomfång om `<scope>` har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-112">`user` scope is hello default scope if `<scope>` is not specified.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="fd3a6-113">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="fd3a6-113">Remarks</span></span>

<span data-ttu-id="fd3a6-114">Ett försök tooaccess en obefintlig systemegenskap är ett fel när ett försök tooaccess en obefintlig Användaregenskapen inte är ett fel.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-114">An attempt tooaccess a non-existent system property is an error, while an attempt tooaccess a non-existent user property is not an error.</span></span> <span data-ttu-id="fd3a6-115">I stället utvärderas en obefintlig användaregenskap internt som ett okänt värde.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-115">Instead, a non-existent user property is internally evaluated as an unknown value.</span></span> <span data-ttu-id="fd3a6-116">Ett okänt värde behandlas särskilt under utvärdering av operatorn.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-116">An unknown value is treated specially during operator evaluation.</span></span>  
  
## <a name="propertyname"></a><span data-ttu-id="fd3a6-117">kubattributbindningen</span><span class="sxs-lookup"><span data-stu-id="fd3a6-117">property_name</span></span>  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a><span data-ttu-id="fd3a6-118">Argument</span><span class="sxs-lookup"><span data-stu-id="fd3a6-118">Arguments</span></span>  

 <span data-ttu-id="fd3a6-119">`<regular_identifier>`är en sträng som representeras av hello följande reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-119">`<regular_identifier>` is a string represented by hello following regular expression:</span></span>  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
<span data-ttu-id="fd3a6-120">Denna grammatik innebär alla strängar som börjar med en bokstav och följs av en eller flera understreck/bokstav/siffra.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-120">This grammar means any string that starts with a letter and is followed by one or more underscore/letter/digit.</span></span>  
  
<span data-ttu-id="fd3a6-121">`[:IsLetter:]`innebär att alla Unicode-tecken som är kategoriserade som ett Unicode-tecken.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-121">`[:IsLetter:]` means any Unicode character that is categorized as a Unicode letter.</span></span> <span data-ttu-id="fd3a6-122">`System.Char.IsLetter(c)`Returnerar `true` om `c` är ett Unicode-tecken.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-122">`System.Char.IsLetter(c)` returns `true` if `c` is a Unicode letter.</span></span>  
  
<span data-ttu-id="fd3a6-123">`[:IsDigit:]`innebär att alla Unicode-tecken som är kategoriserade som en siffra.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-123">`[:IsDigit:]` means any Unicode character that is categorized as a decimal digit.</span></span> <span data-ttu-id="fd3a6-124">`System.Char.IsDigit(c)`Returnerar `true` om `c` är en Unicode-siffra.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-124">`System.Char.IsDigit(c)` returns `true` if `c` is a Unicode digit.</span></span>  
  
<span data-ttu-id="fd3a6-125">En `<regular_identifier>` får inte vara ett reserverat nyckelord.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-125">A `<regular_identifier>` cannot be a reserved keyword.</span></span>  
  
<span data-ttu-id="fd3a6-126">`<delimited_identifier>`är en sträng som medföljde åt vänster och höger hakparentes ([]).</span><span class="sxs-lookup"><span data-stu-id="fd3a6-126">`<delimited_identifier>` is any string that is enclosed with left/right square brackets ([]).</span></span> <span data-ttu-id="fd3a6-127">Höger hakparentes representeras som två höger hakparentes.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-127">A right square bracket is represented as two right square brackets.</span></span> <span data-ttu-id="fd3a6-128">hello följande är exempel på `<delimited_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-128">hello following are examples of `<delimited_identifier>`:</span></span>  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
<span data-ttu-id="fd3a6-129">`<quoted_identifier>`är en sträng som omges av dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-129">`<quoted_identifier>` is any string that is enclosed with double quotation marks.</span></span> <span data-ttu-id="fd3a6-130">Ett dubbelt citattecken i identifierare representeras av dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-130">A double quotation mark in identifier is represented as two double quotation marks.</span></span> <span data-ttu-id="fd3a6-131">Det rekommenderas inte toouse identifierare eftersom den enkelt kan förväxlas med en strängkonstant.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-131">It is not recommended toouse quoted identifiers because it can easily be confused with a string constant.</span></span> <span data-ttu-id="fd3a6-132">Använd om möjligt en avgränsad identifierare.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-132">Use a delimited identifier if possible.</span></span> <span data-ttu-id="fd3a6-133">hello följande är ett exempel på `<quoted_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-133">hello following is an example of `<quoted_identifier>`:</span></span>  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a><span data-ttu-id="fd3a6-134">Mönstret</span><span class="sxs-lookup"><span data-stu-id="fd3a6-134">pattern</span></span>  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="fd3a6-135">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="fd3a6-135">Remarks</span></span>
  
<span data-ttu-id="fd3a6-136">`<pattern>`måste vara ett uttryck som utvärderas som en sträng.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-136">`<pattern>` must be an expression that is evaluated as a string.</span></span> <span data-ttu-id="fd3a6-137">Den används som ett mönster för hello som operator.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-137">It is used as a pattern for hello LIKE operator.</span></span>      <span data-ttu-id="fd3a6-138">Det kan innehålla följande jokertecken hello:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-138">It can contain hello following wildcard characters:</span></span>  
  
-   <span data-ttu-id="fd3a6-139">`%`: En sträng med noll eller flera tecken.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-139">`%`:  Any string of zero or more characters.</span></span>  
  
-   <span data-ttu-id="fd3a6-140">`_`: Ett valfritt tecken.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-140">`_`: Any single character.</span></span>  
  
## <a name="escapechar"></a><span data-ttu-id="fd3a6-141">escape_char</span><span class="sxs-lookup"><span data-stu-id="fd3a6-141">escape_char</span></span>  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="fd3a6-142">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="fd3a6-142">Remarks</span></span>  

<span data-ttu-id="fd3a6-143">`<escape_char>`måste vara ett uttryck som utvärderas som en sträng med längden 1.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-143">`<escape_char>` must be an expression that is evaluated as a string of length 1.</span></span> <span data-ttu-id="fd3a6-144">Den används som escape-tecken för hello som operator.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-144">It is used as an escape character for hello LIKE operator.</span></span>  
  
 <span data-ttu-id="fd3a6-145">Till exempel `property LIKE 'ABC\%' ESCAPE '\'` matchar `ABC%` i stället för en sträng som börjar med `ABC`.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-145">For example, `property LIKE 'ABC\%' ESCAPE '\'` matches `ABC%` rather than a string that starts with `ABC`.</span></span>  
  
## <a name="constant"></a><span data-ttu-id="fd3a6-146">konstant</span><span class="sxs-lookup"><span data-stu-id="fd3a6-146">constant</span></span>  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a><span data-ttu-id="fd3a6-147">Argument</span><span class="sxs-lookup"><span data-stu-id="fd3a6-147">Arguments</span></span>  
  
-   <span data-ttu-id="fd3a6-148">`<integer_constant>`är en sträng som inte är inom citattecken och inte innehåller decimaltecken.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-148">`<integer_constant>` is a string of numbers that are not enclosed in quotation marks and do not contain decimal points.</span></span> <span data-ttu-id="fd3a6-149">hello värdena lagras som `System.Int64` internt, och följ hello samma intervall.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-149">hello values are stored as `System.Int64` internally, and follow hello same range.</span></span>  
  
     <span data-ttu-id="fd3a6-150">Det här är exempel på lång konstanter:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-150">These are examples of long constants:</span></span>  
  
    ```  
    1894  
    2  
    ```  
  
-   <span data-ttu-id="fd3a6-151">`<decimal_constant>`är en sträng som inte är inom citattecken och innehåller ett decimaltecken.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-151">`<decimal_constant>` is a string of numbers that are not enclosed in quotation marks, and contain a decimal point.</span></span> <span data-ttu-id="fd3a6-152">hello värdena lagras som `System.Double` internt, och följ hello samma intervall/precision.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-152">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span>  
  
     <span data-ttu-id="fd3a6-153">I en framtida version siffran kan lagras i en annan typ toosupport exakt nummer format, så du inte bör lita på hello fakta hello underliggande-datatypen är `System.Double` för `<decimal_constant>`.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-153">In a future version, this number might be stored in a different data type toosupport exact number semantics, so you should not rely on hello fact hello underlying data type is `System.Double` for `<decimal_constant>`.</span></span>  
  
     <span data-ttu-id="fd3a6-154">hello följande är exempel på decimal konstanter:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-154">hello following are examples of decimal constants:</span></span>  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   <span data-ttu-id="fd3a6-155">`<approximate_number_constant>`är ett antal skrivna i matematisk notation.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-155">`<approximate_number_constant>` is a number written in scientific notation.</span></span> <span data-ttu-id="fd3a6-156">hello värdena lagras som `System.Double` internt, och följ hello samma intervall/precision.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-156">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span> <span data-ttu-id="fd3a6-157">hello följande är exempel på ungefärliga numeriska konstanter:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-157">hello following are examples of approximate number constants:</span></span>  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a><span data-ttu-id="fd3a6-158">boolean_constant</span><span class="sxs-lookup"><span data-stu-id="fd3a6-158">boolean_constant</span></span>  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a><span data-ttu-id="fd3a6-159">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="fd3a6-159">Remarks</span></span>  

<span data-ttu-id="fd3a6-160">Booleska konstanter som representeras av hello nyckelord **SANT** eller **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-160">Boolean constants are represented by hello keywords **TRUE** or **FALSE**.</span></span> <span data-ttu-id="fd3a6-161">hello värdena lagras som `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-161">hello values are stored as `System.Boolean`.</span></span>  
  
## <a name="stringconstant"></a><span data-ttu-id="fd3a6-162">string_constant</span><span class="sxs-lookup"><span data-stu-id="fd3a6-162">string_constant</span></span>  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a><span data-ttu-id="fd3a6-163">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="fd3a6-163">Remarks</span></span>  

<span data-ttu-id="fd3a6-164">Strängkonstanter omges av enkla citattecken och inkludera alla giltiga Unicode-tecken.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-164">String constants are enclosed in single quotation marks and include any valid Unicode characters.</span></span> <span data-ttu-id="fd3a6-165">Ett enkelt citattecken inbäddat i en strängkonstant representeras som två enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-165">A single quotation mark embedded in a string constant is represented as two single quotation marks.</span></span>  
  
## <a name="function"></a><span data-ttu-id="fd3a6-166">Funktionen</span><span class="sxs-lookup"><span data-stu-id="fd3a6-166">function</span></span>  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a><span data-ttu-id="fd3a6-167">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="fd3a6-167">Remarks</span></span>
  
<span data-ttu-id="fd3a6-168">Hej `newid()` fungerar returnerar en **System.Guid** genereras av hello `System.Guid.NewGuid()` metod.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-168">hello `newid()` function returns a **System.Guid** generated by hello `System.Guid.NewGuid()` method.</span></span>  
  
<span data-ttu-id="fd3a6-169">Hej `property(name)` returnerar funktionen hello värde för hello-egenskap som refererar till `name`.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-169">hello `property(name)` function returns hello value of hello property referenced by `name`.</span></span> <span data-ttu-id="fd3a6-170">Hej `name` värdet kan vara ett uttryck som returnerar ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-170">hello `name` value can be any valid expression that returns a string value.</span></span>  
  
## <a name="considerations"></a><span data-ttu-id="fd3a6-171">Överväganden</span><span class="sxs-lookup"><span data-stu-id="fd3a6-171">Considerations</span></span>
  
<span data-ttu-id="fd3a6-172">Tänk hello följande [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantik:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-172">Consider hello following [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantics:</span></span>  
  
-   <span data-ttu-id="fd3a6-173">Egenskapsnamnen är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-173">Property names are case-insensitive.</span></span>  
  
-   <span data-ttu-id="fd3a6-174">Operatörer Följ C# implicit konvertering semantik när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-174">Operators follow C# implicit conversion semantics whenever possible.</span></span>  
  
-   <span data-ttu-id="fd3a6-175">Systemegenskaper är offentliga egenskaper som exponeras i [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) instanser.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-175">System properties are public properties exposed in [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) instances.</span></span>  
  
    <span data-ttu-id="fd3a6-176">Tänk hello följande `IS [NOT] NULL` semantik:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-176">Consider hello following `IS [NOT] NULL` semantics:</span></span>  
  
    -   <span data-ttu-id="fd3a6-177">`property IS NULL`utvärderas som `true` om antingen hello-egenskapen inte finns eller hello egenskapens värde är `null`.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-177">`property IS NULL` is evaluated as `true` if either hello property doesn't exist or hello property's value is `null`.</span></span>  
  
### <a name="property-evaluation-semantics"></a><span data-ttu-id="fd3a6-178">Egenskapen utvärdering semantik</span><span class="sxs-lookup"><span data-stu-id="fd3a6-178">Property evaluation semantics</span></span>  
  
-   <span data-ttu-id="fd3a6-179">Ett försök tooevaluate en obefintlig Systemegenskapen utlöser en [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) undantag.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-179">An attempt tooevaluate a non-existent system property throws a [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) exception.</span></span>  
  
-   <span data-ttu-id="fd3a6-180">En egenskap som inte finns internt utvärderas som **okänd**.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-180">A property that does not exist is internally evaluated as **unknown**.</span></span>  
  
 <span data-ttu-id="fd3a6-181">Okänd utvärdering i aritmetiska operatorerna:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-181">Unknown evaluation in arithmetic operators:</span></span>  
  
-   <span data-ttu-id="fd3a6-182">För binära operatorer om antingen hello vänster eller höger sida av operander utvärderas som **okänd**, och sedan hello resultatet är **okänd**.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-182">For binary operators, if either hello left and/or right side of operands is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
-   <span data-ttu-id="fd3a6-183">För unära operatorer, om en operand utvärderas som **okänd**, och sedan hello resultatet är **okänd**.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-183">For unary operators, if an operand is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="fd3a6-184">Okänd utvärdering i binära operatorer:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-184">Unknown evaluation in binary comparison operators:</span></span>  
  
-   <span data-ttu-id="fd3a6-185">Om antingen hello vänster eller höger sida av operander utvärderas som **okänd**, och sedan hello resultatet är **okänd**.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-185">If either hello left and/or right side of operands is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="fd3a6-186">Okänd utvärdering i `[NOT] LIKE`:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-186">Unknown evaluation in `[NOT] LIKE`:</span></span>  
  
-   <span data-ttu-id="fd3a6-187">Om operanden alla utvärderas som **okänd**, och sedan hello resultatet är **okänd**.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-187">If any operand is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="fd3a6-188">Okänd utvärdering i `[NOT] IN`:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-188">Unknown evaluation in `[NOT] IN`:</span></span>  
  
-   <span data-ttu-id="fd3a6-189">Om hello vänsteroperanden utvärderas som **okänd**, och sedan hello resultatet är **okänd**.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-189">If hello left operand is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="fd3a6-190">Okänd utvärdering i **och** operator:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-190">Unknown evaluation in **AND** operator:</span></span>  
  
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
  
 <span data-ttu-id="fd3a6-191">Okänd utvärdering i **eller** operator:</span><span class="sxs-lookup"><span data-stu-id="fd3a6-191">Unknown evaluation in **OR** operator:</span></span>  
  
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
  
### <a name="operator-binding-semantics"></a><span data-ttu-id="fd3a6-192">Operatorn bindning semantik</span><span class="sxs-lookup"><span data-stu-id="fd3a6-192">Operator binding semantics</span></span>
  
-   <span data-ttu-id="fd3a6-193">Jämförelseoperatorer som `>`, `>=`, `<`, `<=`, `!=`, och `=` Följ hello samma semantik som hello C#-operatorn bindande i data skriver kampanjer och implicita konverteringar.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-193">Comparison operators such as `>`, `>=`, `<`, `<=`, `!=`, and `=` follow hello same semantics as hello C# operator binding in data type promotions and implicit conversions.</span></span>  
  
-   <span data-ttu-id="fd3a6-194">Aritmetiska operatorer som `+`, `-`, `*`, `/`, och `%` Följ hello samma semantik som hello C#-operatorn bindande i data skriver kampanjer och implicita konverteringar.</span><span class="sxs-lookup"><span data-stu-id="fd3a6-194">Arithmetic operators such as `+`, `-`, `*`, `/`, and `%` follow hello same semantics as hello C# operator binding in data type promotions and implicit conversions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd3a6-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fd3a6-195">Next steps</span></span>

- [<span data-ttu-id="fd3a6-196">SQLFilter-klass</span><span class="sxs-lookup"><span data-stu-id="fd3a6-196">SQLFilter class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [<span data-ttu-id="fd3a6-197">SQLRuleAction-klass</span><span class="sxs-lookup"><span data-stu-id="fd3a6-197">SQLRuleAction class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)