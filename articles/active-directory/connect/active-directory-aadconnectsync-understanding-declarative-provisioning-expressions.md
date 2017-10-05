---
title: "Azure AD Connect: Uttryck för deklarativ etablering | Microsoft Docs"
description: "Beskrivs uttryck för deklarativ etablering."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: e3a03a97b10e04fb85261620879b2102e1db8465
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a><span data-ttu-id="1e2d2-103">Azure AD Connect-synkronisering: Förstå uttryck för deklarativ etablering</span><span class="sxs-lookup"><span data-stu-id="1e2d2-103">Azure AD Connect sync: Understanding Declarative Provisioning Expressions</span></span>
<span data-ttu-id="1e2d2-104">Azure AD Connect-synkronisering bygger på deklarativ etablering introducerades i Forefront Identity Manager 2010.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-104">Azure AD Connect sync builds on declarative provisioning first introduced in Forefront Identity Manager 2010.</span></span> <span data-ttu-id="1e2d2-105">På så sätt kan du implementera affärslogik fullständig identity integration utan att behöva skriva koden.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-105">It allows you to implement your complete identity integration business logic without the need to write compiled code.</span></span>

<span data-ttu-id="1e2d2-106">En väsentlig del av deklarativ etablering är språket för uttryck som används i attributflöden.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-106">An essential part of declarative provisioning is the expression language used in attribute flows.</span></span> <span data-ttu-id="1e2d2-107">Språket är en delmängd av Microsoft® Visual Basic for Applications (VBA).</span><span class="sxs-lookup"><span data-stu-id="1e2d2-107">The language used is a subset of Microsoft® Visual Basic® for Applications (VBA).</span></span> <span data-ttu-id="1e2d2-108">Det här språket används i Microsoft Office och användarna har erfarenhet av VBScript tolkar även.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-108">This language is used in Microsoft Office and users with experience of VBScript will also recognize it.</span></span> <span data-ttu-id="1e2d2-109">Deklarativ etablering Uttrycksspråk endast med hjälp av funktioner och är inte ett strukturerade språk.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-109">The Declarative Provisioning Expression Language is only using functions and is not a structured language.</span></span> <span data-ttu-id="1e2d2-110">Det finns inga metoder eller instruktioner.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-110">There are no methods or statements.</span></span> <span data-ttu-id="1e2d2-111">Funktioner kapslas i stället för snabb programflöde.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-111">Functions are instead nested to express program flow.</span></span>

<span data-ttu-id="1e2d2-112">Mer information finns i [Välkommen till Visual Basic for Applications Språkreferens för Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e2d2-112">For more details, see [Welcome to the Visual Basic for Applications language reference for Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span></span>

<span data-ttu-id="1e2d2-113">Attribut har strikt typkontroll.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-113">The attributes are strongly typed.</span></span> <span data-ttu-id="1e2d2-114">En funktion accepterar endast attribut av typen korrekt.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-114">A function only accepts attributes of the correct type.</span></span> <span data-ttu-id="1e2d2-115">Det är också skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-115">It is also case-sensitive.</span></span> <span data-ttu-id="1e2d2-116">Både funktionsnamnen och attributnamn måste ha rätt skiftläge eller ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-116">Both function names and attribute names must have proper casing or an error is thrown.</span></span>

## <a name="language-definitions-and-identifiers"></a><span data-ttu-id="1e2d2-117">Språk definitioner och identifierare</span><span class="sxs-lookup"><span data-stu-id="1e2d2-117">Language definitions and Identifiers</span></span>
* <span data-ttu-id="1e2d2-118">Funktioner har ett namn följt av argument inom parentes: FunctionName (argumentet 1, argumentet N).</span><span class="sxs-lookup"><span data-stu-id="1e2d2-118">Functions have a name followed by arguments in brackets: FunctionName(argument 1, argument N).</span></span>
* <span data-ttu-id="1e2d2-119">Attribut som identifieras av hakparenteser: [attributeName]</span><span class="sxs-lookup"><span data-stu-id="1e2d2-119">Attributes are identified by square brackets: [attributeName]</span></span>
* <span data-ttu-id="1e2d2-120">Parametrar som identifieras av procenttecken: % ParameterName %</span><span class="sxs-lookup"><span data-stu-id="1e2d2-120">Parameters are identified by percent signs: %ParameterName%</span></span>
* <span data-ttu-id="1e2d2-121">Strängkonstanter omges av citattecken: till exempel ”Contoso” (Obs: måste använda enkla citattecken ”” och inte typografiska citattecken ””)</span><span class="sxs-lookup"><span data-stu-id="1e2d2-121">String constants are surrounded by quotes: For example, "Contoso" (Note: must use straight quotes "" and not smart quotes “”)</span></span>
* <span data-ttu-id="1e2d2-122">Numeriska värden anges utan citattecken och förväntas vara decimal.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-122">Numeric values are expressed without quotes and expected to be decimal.</span></span> <span data-ttu-id="1e2d2-123">Hexadecimala värden föregås av & H.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-123">Hexadecimal values are prefixed with &H.</span></span> <span data-ttu-id="1e2d2-124">Till exempel 98052 & HFF</span><span class="sxs-lookup"><span data-stu-id="1e2d2-124">For example, 98052, &HFF</span></span>
* <span data-ttu-id="1e2d2-125">Booleska värden uttrycks med konstanter: SANT, FALSKT.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-125">Boolean values are expressed with constants: True, False.</span></span>
* <span data-ttu-id="1e2d2-126">Inbyggda konstanter och literaler uttrycks med endast deras namn: NULL, CRLF, IgnoreThisFlow</span><span class="sxs-lookup"><span data-stu-id="1e2d2-126">Built-in constants and literals are expressed with only their name: NULL, CRLF, IgnoreThisFlow</span></span>

### <a name="functions"></a><span data-ttu-id="1e2d2-127">Funktioner</span><span class="sxs-lookup"><span data-stu-id="1e2d2-127">Functions</span></span>
<span data-ttu-id="1e2d2-128">Deklarativ etablering använder många funktioner för att aktivera möjligheten att omvandla attributvärden.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-128">Declarative provisioning uses many functions to enable the possibility to transform attribute values.</span></span> <span data-ttu-id="1e2d2-129">Dessa funktioner kan kapslas så resultatet från en funktion som skickas till en annan funktion.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-129">These functions can be nested so the result from one function is passed in to another function.</span></span>

`Function1(Function2(Function3()))`

<span data-ttu-id="1e2d2-130">En fullständig lista över funktioner finns i den [fungerar referens](active-directory-aadconnectsync-functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="1e2d2-130">The complete list of functions can be found in the [function reference](active-directory-aadconnectsync-functions-reference.md).</span></span>

### <a name="parameters"></a><span data-ttu-id="1e2d2-131">Parametrar</span><span class="sxs-lookup"><span data-stu-id="1e2d2-131">Parameters</span></span>
<span data-ttu-id="1e2d2-132">En parameter har definierats av en koppling eller av en administratör med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-132">A parameter is defined either by a Connector or by an administrator using PowerShell.</span></span> <span data-ttu-id="1e2d2-133">Parametrar som vanligtvis innehåller värden som skiljer sig från ett system till, till exempel namnet på domänen användaren finns i.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-133">Parameters usually contain values that are different from system to system, for example the name of the domain the user is located in.</span></span> <span data-ttu-id="1e2d2-134">Dessa parametrar kan användas i attributflöden.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-134">These parameters can be used in attribute flows.</span></span>

<span data-ttu-id="1e2d2-135">Active Directory-koppling avses regler för inkommande synkronisering följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="1e2d2-135">The Active Directory Connector provided the following parameters for inbound Synchronization Rules:</span></span>

| <span data-ttu-id="1e2d2-136">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="1e2d2-136">Parameter Name</span></span> | <span data-ttu-id="1e2d2-137">Kommentar</span><span class="sxs-lookup"><span data-stu-id="1e2d2-137">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="1e2d2-138">Domain.Netbios</span><span class="sxs-lookup"><span data-stu-id="1e2d2-138">Domain.Netbios</span></span> |<span data-ttu-id="1e2d2-139">NetBIOS-format för den domän som för närvarande importeras, till exempel FABRIKAMSALES</span><span class="sxs-lookup"><span data-stu-id="1e2d2-139">Netbios format of the domain currently being imported, for example FABRIKAMSALES</span></span> |
| <span data-ttu-id="1e2d2-140">Domain.FQDN</span><span class="sxs-lookup"><span data-stu-id="1e2d2-140">Domain.FQDN</span></span> |<span data-ttu-id="1e2d2-141">FQDN-format i domänen som importerats, till exempel sales.fabrikam.com</span><span class="sxs-lookup"><span data-stu-id="1e2d2-141">FQDN format of the domain currently being imported, for example sales.fabrikam.com</span></span> |
| <span data-ttu-id="1e2d2-142">Domain.LDAP</span><span class="sxs-lookup"><span data-stu-id="1e2d2-142">Domain.LDAP</span></span> |<span data-ttu-id="1e2d2-143">LDAP-format för den domän som för närvarande importeras, till exempel DC = försäljning, DC = fabrikam, DC = com</span><span class="sxs-lookup"><span data-stu-id="1e2d2-143">LDAP format of the domain currently being imported, for example DC=sales,DC=fabrikam,DC=com</span></span> |
| <span data-ttu-id="1e2d2-144">Forest.Netbios</span><span class="sxs-lookup"><span data-stu-id="1e2d2-144">Forest.Netbios</span></span> |<span data-ttu-id="1e2d2-145">Skogens namn för närvarande importeras, till exempel FABRIKAMCORP NetBIOS-format</span><span class="sxs-lookup"><span data-stu-id="1e2d2-145">Netbios format of the forest name currently being imported, for example FABRIKAMCORP</span></span> |
| <span data-ttu-id="1e2d2-146">Forest.FQDN</span><span class="sxs-lookup"><span data-stu-id="1e2d2-146">Forest.FQDN</span></span> |<span data-ttu-id="1e2d2-147">Skogens namn för närvarande importeras, till exempel fabrikam.com FQDN-format</span><span class="sxs-lookup"><span data-stu-id="1e2d2-147">FQDN format of the forest name currently being imported, for example fabrikam.com</span></span> |
| <span data-ttu-id="1e2d2-148">Forest.LDAP</span><span class="sxs-lookup"><span data-stu-id="1e2d2-148">Forest.LDAP</span></span> |<span data-ttu-id="1e2d2-149">LDAP-format för skogsnamnet som för närvarande importeras, till exempel DC = fabrikam, DC = com</span><span class="sxs-lookup"><span data-stu-id="1e2d2-149">LDAP format of the forest name currently being imported, for example DC=fabrikam,DC=com</span></span> |

<span data-ttu-id="1e2d2-150">Systemet innehåller följande parameter som används för att hämta ID för koppling som för närvarande körs:</span><span class="sxs-lookup"><span data-stu-id="1e2d2-150">The system provides the following parameter, which is used to get the identifier of the Connector currently running:</span></span>  
`Connector.ID`

<span data-ttu-id="1e2d2-151">Här är ett exempel som fyller på domänen metaversum-attribut med netbios-namnet på den domän där användaren:</span><span class="sxs-lookup"><span data-stu-id="1e2d2-151">Here is an example that populates the metaverse attribute domain with the netbios name of the domain where the user is located:</span></span>  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a><span data-ttu-id="1e2d2-152">Operatorer</span><span class="sxs-lookup"><span data-stu-id="1e2d2-152">Operators</span></span>
<span data-ttu-id="1e2d2-153">Du kan använda följande operatorer:</span><span class="sxs-lookup"><span data-stu-id="1e2d2-153">The following operators can be used:</span></span>

* <span data-ttu-id="1e2d2-154">**Jämförelse**: <, < =, <>, =, >, > =</span><span class="sxs-lookup"><span data-stu-id="1e2d2-154">**Comparison**: <, <=, <>, =, >, >=</span></span>
* <span data-ttu-id="1e2d2-155">**Matematik**: +, -, \*, -</span><span class="sxs-lookup"><span data-stu-id="1e2d2-155">**Mathematics**: +, -, \*, -</span></span>
* <span data-ttu-id="1e2d2-156">**Strängen**: & (sammanfoga)</span><span class="sxs-lookup"><span data-stu-id="1e2d2-156">**String**: & (concatenate)</span></span>
* <span data-ttu-id="1e2d2-157">**Logisk**: & & (och). (eller)</span><span class="sxs-lookup"><span data-stu-id="1e2d2-157">**Logical**: && (and), || (or)</span></span>
* <span data-ttu-id="1e2d2-158">**Utvärderingsordningen**:)</span><span class="sxs-lookup"><span data-stu-id="1e2d2-158">**Evaluation order**: ( )</span></span>

<span data-ttu-id="1e2d2-159">Operatorer utvärderas vänster till höger och har samma prioritet för utvärdering.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-159">Operators are evaluated left to right and have the same evaluation priority.</span></span> <span data-ttu-id="1e2d2-160">Det vill säga den \* (multiplikator) utvärderas inte innan - (subtraktion).</span><span class="sxs-lookup"><span data-stu-id="1e2d2-160">That is, the \* (multiplier) is not evaluated before - (subtraction).</span></span> <span data-ttu-id="1e2d2-161">2\*(5 + 3) inte är samma som 2\*5 + 3.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-161">2\*(5+3) is not the same as 2\*5+3.</span></span> <span data-ttu-id="1e2d2-162">Hakparenteser () används för att ändra utvärderingsordningen när vänster till höger utvärderingsordning inte är lämpligt.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-162">The brackets ( ) are used to change the evaluation order when left to right evaluation order isn't appropriate.</span></span>

## <a name="multi-valued-attributes"></a><span data-ttu-id="1e2d2-163">Flera värden attribut</span><span class="sxs-lookup"><span data-stu-id="1e2d2-163">Multi-valued attributes</span></span>
<span data-ttu-id="1e2d2-164">Funktionerna fungerar med både enstaka och flera värden.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-164">The functions can operate on both single-valued and multi-valued attributes.</span></span> <span data-ttu-id="1e2d2-165">Funktionen fungerar över varje värde för flera värden attribut och gäller samma funktion för varje värde.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-165">For multi-valued attributes, the function operates over every value and applies the same function to every value.</span></span>

<span data-ttu-id="1e2d2-166">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1e2d2-166">For example:</span></span>  
<span data-ttu-id="1e2d2-167">`Trim([proxyAddresses])`Göra en Trimning av varje värde i attributet proxyAddress.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-167">`Trim([proxyAddresses])` Do a Trim of every value in the proxyAddress attribute.</span></span>  
<span data-ttu-id="1e2d2-168">`Word([proxyAddresses],1,"@") & "@contoso.com"`För varje värde med en @-sign, Ersätt den med @contoso.com.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-168">`Word([proxyAddresses],1,"@") & "@contoso.com"` For every value with an @-sign, replace the domain with @contoso.com.</span></span>  
<span data-ttu-id="1e2d2-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Leta efter SIP-adress och ta bort den från värden.</span><span class="sxs-lookup"><span data-stu-id="1e2d2-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])` Look for the SIP-address and remove it from the values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e2d2-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e2d2-170">Next steps</span></span>
* <span data-ttu-id="1e2d2-171">Läs mer om Konfigurationsmodell i [förstå deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="1e2d2-171">Read more about the configuration model in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>
* <span data-ttu-id="1e2d2-172">Se hur deklarativ etablering är att använda out-of-box i [förstå standardkonfigurationen](active-directory-aadconnectsync-understanding-default-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="1e2d2-172">See how declarative provisioning is used out-of-box in [Understanding the default configuration](active-directory-aadconnectsync-understanding-default-configuration.md).</span></span>
* <span data-ttu-id="1e2d2-173">Se hur du ändrar något praktiska med deklarativ etablering i [hur du gör en ändring i standardkonfigurationen](active-directory-aadconnectsync-change-the-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="1e2d2-173">See how to make a practical change using declarative provisioning in [How to make a change to the default configuration](active-directory-aadconnectsync-change-the-configuration.md).</span></span>

<span data-ttu-id="1e2d2-174">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="1e2d2-174">**Overview topics**</span></span>

* [<span data-ttu-id="1e2d2-175">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="1e2d2-175">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="1e2d2-176">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1e2d2-176">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

<span data-ttu-id="1e2d2-177">**Referensinformation**</span><span class="sxs-lookup"><span data-stu-id="1e2d2-177">**Reference topics**</span></span>

* [<span data-ttu-id="1e2d2-178">Azure AD Connect-synkronisering: funktioner referens</span><span class="sxs-lookup"><span data-stu-id="1e2d2-178">Azure AD Connect sync: Functions Reference</span></span>](active-directory-aadconnectsync-functions-reference.md)

