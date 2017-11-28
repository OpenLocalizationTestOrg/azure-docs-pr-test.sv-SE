---
title: "Azure AD Connect: Uttryck för deklarativ etablering | Microsoft Docs"
description: "Förklarar hello-uttryck för deklarativ etablering."
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
ms.openlocfilehash: 516bcf1991c608d33aefc19551254d8b2bfc024f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a><span data-ttu-id="035ac-103">Azure AD Connect-synkronisering: Förstå uttryck för deklarativ etablering</span><span class="sxs-lookup"><span data-stu-id="035ac-103">Azure AD Connect sync: Understanding Declarative Provisioning Expressions</span></span>
<span data-ttu-id="035ac-104">Azure AD Connect-synkronisering bygger på deklarativ etablering introducerades i Forefront Identity Manager 2010.</span><span class="sxs-lookup"><span data-stu-id="035ac-104">Azure AD Connect sync builds on declarative provisioning first introduced in Forefront Identity Manager 2010.</span></span> <span data-ttu-id="035ac-105">Det gör att du tooimplement affärslogik fullständig identity integration utan hello måste toowrite kompilerad kod.</span><span class="sxs-lookup"><span data-stu-id="035ac-105">It allows you tooimplement your complete identity integration business logic without hello need toowrite compiled code.</span></span>

<span data-ttu-id="035ac-106">En väsentlig del av deklarativ etablering är hello Uttrycksspråk som används i attributflöden.</span><span class="sxs-lookup"><span data-stu-id="035ac-106">An essential part of declarative provisioning is hello expression language used in attribute flows.</span></span> <span data-ttu-id="035ac-107">hello-språk som används är en delmängd av Microsoft® Visual Basic for Applications (VBA).</span><span class="sxs-lookup"><span data-stu-id="035ac-107">hello language used is a subset of Microsoft® Visual Basic® for Applications (VBA).</span></span> <span data-ttu-id="035ac-108">Det här språket används i Microsoft Office och användarna har erfarenhet av VBScript tolkar även.</span><span class="sxs-lookup"><span data-stu-id="035ac-108">This language is used in Microsoft Office and users with experience of VBScript will also recognize it.</span></span> <span data-ttu-id="035ac-109">hello deklarativ etablering Expression Language endast med hjälp av funktioner och är inte ett strukturerade språk.</span><span class="sxs-lookup"><span data-stu-id="035ac-109">hello Declarative Provisioning Expression Language is only using functions and is not a structured language.</span></span> <span data-ttu-id="035ac-110">Det finns inga metoder eller instruktioner.</span><span class="sxs-lookup"><span data-stu-id="035ac-110">There are no methods or statements.</span></span> <span data-ttu-id="035ac-111">Funktioner i stället är kapslade tooexpress programmet flödet.</span><span class="sxs-lookup"><span data-stu-id="035ac-111">Functions are instead nested tooexpress program flow.</span></span>

<span data-ttu-id="035ac-112">Mer information finns i [Välkommen toohello Visual Basic för program Språkreferens för Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span><span class="sxs-lookup"><span data-stu-id="035ac-112">For more details, see [Welcome toohello Visual Basic for Applications language reference for Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span></span>

<span data-ttu-id="035ac-113">hello attribut har strikt typkontroll.</span><span class="sxs-lookup"><span data-stu-id="035ac-113">hello attributes are strongly typed.</span></span> <span data-ttu-id="035ac-114">En funktion accepterar endast attribut för hello rätt typ.</span><span class="sxs-lookup"><span data-stu-id="035ac-114">A function only accepts attributes of hello correct type.</span></span> <span data-ttu-id="035ac-115">Det är också skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="035ac-115">It is also case-sensitive.</span></span> <span data-ttu-id="035ac-116">Både funktionsnamnen och attributnamn måste ha rätt skiftläge eller ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="035ac-116">Both function names and attribute names must have proper casing or an error is thrown.</span></span>

## <a name="language-definitions-and-identifiers"></a><span data-ttu-id="035ac-117">Språk definitioner och identifierare</span><span class="sxs-lookup"><span data-stu-id="035ac-117">Language definitions and Identifiers</span></span>
* <span data-ttu-id="035ac-118">Funktioner har ett namn följt av argument inom parentes: FunctionName (argumentet 1, argumentet N).</span><span class="sxs-lookup"><span data-stu-id="035ac-118">Functions have a name followed by arguments in brackets: FunctionName(argument 1, argument N).</span></span>
* <span data-ttu-id="035ac-119">Attribut som identifieras av hakparenteser: [attributeName]</span><span class="sxs-lookup"><span data-stu-id="035ac-119">Attributes are identified by square brackets: [attributeName]</span></span>
* <span data-ttu-id="035ac-120">Parametrar som identifieras av procenttecken: % ParameterName %</span><span class="sxs-lookup"><span data-stu-id="035ac-120">Parameters are identified by percent signs: %ParameterName%</span></span>
* <span data-ttu-id="035ac-121">Strängkonstanter omges av citattecken: till exempel ”Contoso” (Obs: måste använda enkla citattecken ”” och inte typografiska citattecken ””)</span><span class="sxs-lookup"><span data-stu-id="035ac-121">String constants are surrounded by quotes: For example, "Contoso" (Note: must use straight quotes "" and not smart quotes “”)</span></span>
* <span data-ttu-id="035ac-122">Numeriska värden anges utan citattecken och förväntade toobe decimal.</span><span class="sxs-lookup"><span data-stu-id="035ac-122">Numeric values are expressed without quotes and expected toobe decimal.</span></span> <span data-ttu-id="035ac-123">Hexadecimala värden föregås av & H.</span><span class="sxs-lookup"><span data-stu-id="035ac-123">Hexadecimal values are prefixed with &H.</span></span> <span data-ttu-id="035ac-124">Till exempel 98052 & HFF</span><span class="sxs-lookup"><span data-stu-id="035ac-124">For example, 98052, &HFF</span></span>
* <span data-ttu-id="035ac-125">Booleska värden uttrycks med konstanter: SANT, FALSKT.</span><span class="sxs-lookup"><span data-stu-id="035ac-125">Boolean values are expressed with constants: True, False.</span></span>
* <span data-ttu-id="035ac-126">Inbyggda konstanter och literaler uttrycks med endast deras namn: NULL, CRLF, IgnoreThisFlow</span><span class="sxs-lookup"><span data-stu-id="035ac-126">Built-in constants and literals are expressed with only their name: NULL, CRLF, IgnoreThisFlow</span></span>

### <a name="functions"></a><span data-ttu-id="035ac-127">Funktioner</span><span class="sxs-lookup"><span data-stu-id="035ac-127">Functions</span></span>
<span data-ttu-id="035ac-128">Deklarativ etablering använder många funktioner tooenable hello möjligheten tootransform attributvärden.</span><span class="sxs-lookup"><span data-stu-id="035ac-128">Declarative provisioning uses many functions tooenable hello possibility tootransform attribute values.</span></span> <span data-ttu-id="035ac-129">Dessa funktioner kan kapslas så hello resultatet från en funktion som du har angett tooanother funktion.</span><span class="sxs-lookup"><span data-stu-id="035ac-129">These functions can be nested so hello result from one function is passed in tooanother function.</span></span>

`Function1(Function2(Function3()))`

<span data-ttu-id="035ac-130">hello fullständig lista över funktioner finns i hello [fungerar referens](active-directory-aadconnectsync-functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="035ac-130">hello complete list of functions can be found in hello [function reference](active-directory-aadconnectsync-functions-reference.md).</span></span>

### <a name="parameters"></a><span data-ttu-id="035ac-131">Parametrar</span><span class="sxs-lookup"><span data-stu-id="035ac-131">Parameters</span></span>
<span data-ttu-id="035ac-132">En parameter har definierats av en koppling eller av en administratör med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="035ac-132">A parameter is defined either by a Connector or by an administrator using PowerShell.</span></span> <span data-ttu-id="035ac-133">Parametrarna innehåller vanligtvis värden som skiljer sig från system toosystem, till exempel hello namnet på hello domänanvändare hello finns i.</span><span class="sxs-lookup"><span data-stu-id="035ac-133">Parameters usually contain values that are different from system toosystem, for example hello name of hello domain hello user is located in.</span></span> <span data-ttu-id="035ac-134">Dessa parametrar kan användas i attributflöden.</span><span class="sxs-lookup"><span data-stu-id="035ac-134">These parameters can be used in attribute flows.</span></span>

<span data-ttu-id="035ac-135">hello Active Directory-kopplingen angivna hello följande parametrar för regler för inkommande synkronisering:</span><span class="sxs-lookup"><span data-stu-id="035ac-135">hello Active Directory Connector provided hello following parameters for inbound Synchronization Rules:</span></span>

| <span data-ttu-id="035ac-136">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="035ac-136">Parameter Name</span></span> | <span data-ttu-id="035ac-137">Kommentar</span><span class="sxs-lookup"><span data-stu-id="035ac-137">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="035ac-138">Domain.Netbios</span><span class="sxs-lookup"><span data-stu-id="035ac-138">Domain.Netbios</span></span> |<span data-ttu-id="035ac-139">NetBIOS-format i hello-domän som importerats, till exempel FABRIKAMSALES</span><span class="sxs-lookup"><span data-stu-id="035ac-139">Netbios format of hello domain currently being imported, for example FABRIKAMSALES</span></span> |
| <span data-ttu-id="035ac-140">Domain.FQDN</span><span class="sxs-lookup"><span data-stu-id="035ac-140">Domain.FQDN</span></span> |<span data-ttu-id="035ac-141">FQDN-format i hello-domän som importerats, till exempel sales.fabrikam.com</span><span class="sxs-lookup"><span data-stu-id="035ac-141">FQDN format of hello domain currently being imported, for example sales.fabrikam.com</span></span> |
| <span data-ttu-id="035ac-142">Domain.LDAP</span><span class="sxs-lookup"><span data-stu-id="035ac-142">Domain.LDAP</span></span> |<span data-ttu-id="035ac-143">LDAP-format för hello domän som importerats, till exempel DC = försäljning, DC = fabrikam, DC = com</span><span class="sxs-lookup"><span data-stu-id="035ac-143">LDAP format of hello domain currently being imported, for example DC=sales,DC=fabrikam,DC=com</span></span> |
| <span data-ttu-id="035ac-144">Forest.Netbios</span><span class="sxs-lookup"><span data-stu-id="035ac-144">Forest.Netbios</span></span> |<span data-ttu-id="035ac-145">NetBIOS-format för hello skogsnamnet som importerats, till exempel FABRIKAMCORP</span><span class="sxs-lookup"><span data-stu-id="035ac-145">Netbios format of hello forest name currently being imported, for example FABRIKAMCORP</span></span> |
| <span data-ttu-id="035ac-146">Forest.FQDN</span><span class="sxs-lookup"><span data-stu-id="035ac-146">Forest.FQDN</span></span> |<span data-ttu-id="035ac-147">Hello skogsnamnet som importerats, till exempel fabrikam.com FQDN-format</span><span class="sxs-lookup"><span data-stu-id="035ac-147">FQDN format of hello forest name currently being imported, for example fabrikam.com</span></span> |
| <span data-ttu-id="035ac-148">Forest.LDAP</span><span class="sxs-lookup"><span data-stu-id="035ac-148">Forest.LDAP</span></span> |<span data-ttu-id="035ac-149">LDAP-format för hello skogsnamnet som importerats, till exempel DC = fabrikam, DC = com</span><span class="sxs-lookup"><span data-stu-id="035ac-149">LDAP format of hello forest name currently being imported, for example DC=fabrikam,DC=com</span></span> |

<span data-ttu-id="035ac-150">hello system ger hello följande parameter, vilket används tooget hello identifierare för hello Connector körs:</span><span class="sxs-lookup"><span data-stu-id="035ac-150">hello system provides hello following parameter, which is used tooget hello identifier of hello Connector currently running:</span></span>  
`Connector.ID`

<span data-ttu-id="035ac-151">Här är ett exempel som fyller på hello metaversum attributdomän med hello netbios-namnet på hello domän där hello användaren finns:</span><span class="sxs-lookup"><span data-stu-id="035ac-151">Here is an example that populates hello metaverse attribute domain with hello netbios name of hello domain where hello user is located:</span></span>  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a><span data-ttu-id="035ac-152">Operatorer</span><span class="sxs-lookup"><span data-stu-id="035ac-152">Operators</span></span>
<span data-ttu-id="035ac-153">Du kan använda hello följande operatorer:</span><span class="sxs-lookup"><span data-stu-id="035ac-153">hello following operators can be used:</span></span>

* <span data-ttu-id="035ac-154">**Jämförelse**: <, < =, <>, =, >, > =</span><span class="sxs-lookup"><span data-stu-id="035ac-154">**Comparison**: <, <=, <>, =, >, >=</span></span>
* <span data-ttu-id="035ac-155">**Matematik**: +, -, \*, -</span><span class="sxs-lookup"><span data-stu-id="035ac-155">**Mathematics**: +, -, \*, -</span></span>
* <span data-ttu-id="035ac-156">**Strängen**: & (sammanfoga)</span><span class="sxs-lookup"><span data-stu-id="035ac-156">**String**: & (concatenate)</span></span>
* <span data-ttu-id="035ac-157">**Logisk**: & & (och). (eller)</span><span class="sxs-lookup"><span data-stu-id="035ac-157">**Logical**: && (and), || (or)</span></span>
* <span data-ttu-id="035ac-158">**Utvärderingsordningen**:)</span><span class="sxs-lookup"><span data-stu-id="035ac-158">**Evaluation order**: ( )</span></span>

<span data-ttu-id="035ac-159">Operatörer utvärderade vänstra tooright och hello har samma prioritet för utvärdering.</span><span class="sxs-lookup"><span data-stu-id="035ac-159">Operators are evaluated left tooright and have hello same evaluation priority.</span></span> <span data-ttu-id="035ac-160">Det vill säga hello \* (multiplikator) utvärderas inte innan - (subtraktion).</span><span class="sxs-lookup"><span data-stu-id="035ac-160">That is, hello \* (multiplier) is not evaluated before - (subtraction).</span></span> <span data-ttu-id="035ac-161">2\*(5 + 3) inte är hello samma som 2\*5 + 3.</span><span class="sxs-lookup"><span data-stu-id="035ac-161">2\*(5+3) is not hello same as 2\*5+3.</span></span> <span data-ttu-id="035ac-162">hello hakparenteser () används toochange hello utvärdering ordning när lämnas tooright utvärderingsordning inte är lämpligt.</span><span class="sxs-lookup"><span data-stu-id="035ac-162">hello brackets ( ) are used toochange hello evaluation order when left tooright evaluation order isn't appropriate.</span></span>

## <a name="multi-valued-attributes"></a><span data-ttu-id="035ac-163">Flera värden attribut</span><span class="sxs-lookup"><span data-stu-id="035ac-163">Multi-valued attributes</span></span>
<span data-ttu-id="035ac-164">hello funktioner fungerar med både enstaka och flera värden.</span><span class="sxs-lookup"><span data-stu-id="035ac-164">hello functions can operate on both single-valued and multi-valued attributes.</span></span> <span data-ttu-id="035ac-165">För flera värden attribut hello-funktionen fungerar över varje värde och tillämpar hello samma fungera tooevery värde.</span><span class="sxs-lookup"><span data-stu-id="035ac-165">For multi-valued attributes, hello function operates over every value and applies hello same function tooevery value.</span></span>

<span data-ttu-id="035ac-166">Exempel:</span><span class="sxs-lookup"><span data-stu-id="035ac-166">For example:</span></span>  
<span data-ttu-id="035ac-167">`Trim([proxyAddresses])`Göra en Trimning av varje värde i hello proxyAddress attributet.</span><span class="sxs-lookup"><span data-stu-id="035ac-167">`Trim([proxyAddresses])` Do a Trim of every value in hello proxyAddress attribute.</span></span>  
<span data-ttu-id="035ac-168">`Word([proxyAddresses],1,"@") & "@contoso.com"`För varje värde med en @-sign, Ersätt hello domän med @contoso.com.</span><span class="sxs-lookup"><span data-stu-id="035ac-168">`Word([proxyAddresses],1,"@") & "@contoso.com"` For every value with an @-sign, replace hello domain with @contoso.com.</span></span>  
<span data-ttu-id="035ac-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Leta efter hello SIP-adress och ta bort den från hello värden.</span><span class="sxs-lookup"><span data-stu-id="035ac-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])` Look for hello SIP-address and remove it from hello values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="035ac-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="035ac-170">Next steps</span></span>
* <span data-ttu-id="035ac-171">Läs mer om hello Konfigurationsmodell i [förstå deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="035ac-171">Read more about hello configuration model in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>
* <span data-ttu-id="035ac-172">Se hur deklarativ etablering är att använda out-of-box i [förstå hello standardkonfigurationen](active-directory-aadconnectsync-understanding-default-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="035ac-172">See how declarative provisioning is used out-of-box in [Understanding hello default configuration](active-directory-aadconnectsync-understanding-default-configuration.md).</span></span>
* <span data-ttu-id="035ac-173">Se hur toomake en praktisk ändras med hjälp av deklarativ etablering i [hur toomake en ändring toohello standard configuration](active-directory-aadconnectsync-change-the-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="035ac-173">See how toomake a practical change using declarative provisioning in [How toomake a change toohello default configuration](active-directory-aadconnectsync-change-the-configuration.md).</span></span>

<span data-ttu-id="035ac-174">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="035ac-174">**Overview topics**</span></span>

* [<span data-ttu-id="035ac-175">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="035ac-175">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="035ac-176">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="035ac-176">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

<span data-ttu-id="035ac-177">**Referensinformation**</span><span class="sxs-lookup"><span data-stu-id="035ac-177">**Reference topics**</span></span>

* [<span data-ttu-id="035ac-178">Azure AD Connect-synkronisering: funktioner referens</span><span class="sxs-lookup"><span data-stu-id="035ac-178">Azure AD Connect sync: Functions Reference</span></span>](active-directory-aadconnectsync-functions-reference.md)

