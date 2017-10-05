---
title: "Översikt över Azure hanterade program | Microsoft Docs"
description: "Beskriver konceptet för Azure hanterade program"
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 7ace8e1ea8038e0748bfed00c0cc0a4fa340588b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-managed-applications-overview"></a>Översikt över Azure hanterade program

Leverantörer som använder Azure kan erbjuda lösningar till kunder runtom i världen. Azure Marketplace är ett galleri som består av hundratals komplexa, multiresource mallar från första part och från tredje part-leverantörer. Kunder kan distribuera och börja använda plattform som en tjänst (PaaS) och programvara som en tjänst (SaaS)-program i minuter. 

Även om Marketplace innehåller ett bra sätt för kunderna att snabbt distribuera ett erbjudande, ansvarar kunden för att underhålla och uppdatera lösningen. Utöver virtuella avbildningen fakturering, kan inte leverantörer debitera kunder för användning av ett program. Leverantörer kan dessutom förhindra kunder från att modifiera kritiska programresurser. Leverantörer kan också blockera åtkomst till immateriella som utgör ett program. Azure hanterade program tillhandahålla lösningar för dessa problem. 

Ett hanterat program liknar en lösningsmall i Marketplace med en nyckel skillnad. I ett hanterat program etableras resurser till en resursgrupp som hanteras av leverantören. Resursgruppen finns i kundens prenumeration, men en identitet i leverantörens klienten har åtkomst till resursgruppen.

## <a name="advantages-of-managed-applications"></a>Fördelarna med hanterade program

Hanterade tjänsteleverantörer (MSPs) ISV: er, och företagets central IT-team kan använda hanterade program för att leverera lösningar via Marketplace eller Tjänstkatalogen. Kunder distribuera dessa hanterade program i sina prenumerationer, men de inte behöver underhålla, uppdatera eller tjänsten dem. Eftersom leverantörer hantera och stöd för program, inte kunder att utveckla programspecifika kunskap för att hantera dessa program. Kunder kan automatiskt hämta programuppdateringar utan att behöva bekymra dig om felsökning och diagnos av problem med program.

Skapa en kanal för att sälja infrastruktur- och programvara på marknadsplatsen för leverantörer och providers hanterade program. Hanterade program innehåller också ett sätt att koppla tjänster och operativa stöd i Azure-kunder. Leverantörer kan debiterar kunder med hjälp av Azure faktureringssystem. De kan använda mallar för att hantera livscykeln för distribuerade program. Dessa lösningar är fristående och förseglade till kunden, så leverantörer kan betjäna av hög kvalitet. Den här metoden förmåner PaaS och SaaS-leverantörer. Det hjälper också till att företagets centrala plattform team och systemintegrerare (SIs) som vill att paketera och sälja sina lösningar.

## <a name="managed-application-types"></a>Hanterade programtyper
Azure hanterade program finns i två typer: Tjänstkatalogen och Marketplace.
 
### <a name="service-catalog"></a>Tjänstkatalog  

Kunder kan Tjänstkatalog skapa en katalog med godkända lösningar för Azure som ska användas av personer i organisationen. Exempelvis är en översikt över lösningar att upprätthålla bra för central IT-team i företag. De kan använda katalogen för att säkerställa kompatibilitet med vissa organisationens normer medan de tillhandahålla lösningar för deras organisationer. De kan styra, uppdatera och hantera dessa program. Anställda kan använda katalogen för att enkelt identifiera omfattande uppsättning program som rekommenderas och godkänts av sina IT-avdelningar. Se Tjänstkatalogen hanterade program som de har skapat. De kan också se hanterade program som andra personer i organisationen som delar med dem..
 
Information om hur du publicerar ett program för Tjänstkatalog hanteras finns [skapa och publicera en applikation för Tjänstkatalog hanteras](managed-application-publishing.md).
 
Information om att använda ett Tjänstkatalogen hanterade program, se [använder ett Tjänstkatalogen hanterade program](managed-application-consumption.md).
 
### <a name="marketplace"></a>Marketplace

Hanterade program är tillgängligt via Marketplace i Azure-portalen. När leverantören publicerar programmen, är de tillgängliga för alla inom och utanför organisationen använda. Med den här metoden kan MSPs ISV: er och SIs erbjuda lösningar till alla Azure-kunder. Kunder få fördelarna med att använda dessa komplexa lösningar utan att behöva ska investera i förstå och hantera lösningar. 

För närvarande utgivare kan tillhandahålla sina erbjudanden som ett hanterat program eller en lösningsmall som har ohanterad. Huvudkomponenterna i publicering av ett hanterat program inkluderar mallfilen och definitionsfilen Användargränssnittet. Mallfilen beskriver de resurser som har etablerats. UI-definitionsfil beskriver hur kräver indata för att etablera dessa resurser visas i portalen. De nödvändiga filerna i en .zip-fil och överföras via publishing portal.
 
Information om hur du publicerar ett hanterat program till Marketplace finns [Azure hanterade program i Marketplace](managed-application-author-marketplace.md).

Information om hur du använder ett hanterat program från Marketplace finns [använda Azure hanterade program i Marketplace](managed-application-consume-marketplace.md).

## <a name="key-concepts"></a>Viktiga begrepp

### <a name="managed-resource-group"></a>Hanterade resursgruppen.
Hanterade resursgruppen är där alla Azure-resurser som tillhandahålls i mallen har skapats. Om installationen för att skapa ett lagringskonto, innehåller den här resursgruppen lagringsresurs för kontot. Den innehåller inte resursen installation.

### <a name="appliance-package"></a>Paket för installation
Ett paket som innehåller mallfilerna och filen createUIDefinition skapas. Mer specifikt innehåller den följande filer:

- **applianceMainTemplate.json**: filen med den här mallen definierar alla resurser som har etablerats av produkten. Den här filen är en vanlig mall som används för att skapa resurser.

- **MainTemplate.json**: filen med den här mallen definierar resursen installation (Microsoft.Solutions/appliances). En nyckelegenskap som definierats i den här resursen är ManagedResourceGroupId. Den här egenskapen anger vilken resursgrupp som används som värd för de faktiska resurser som är definierade i applianceMainTemplate.json.

- **applianceCreateUIDefinition.json**: den här filen beskriver hur UI som behövs för de parametrar som definierats i mallen återges.

### <a name="authorization"></a>Auktorisering
Utgivaren måste ange de behörigheter som krävs av leverantören för att hantera resurserna för kundens räkning. Den här behörigheten gäller hanterade resursgruppen. Ställ in följande värden:

- **PrincipalID**: Azure Active Directory (AD Azure) identifieraren för användaren, gruppen eller program som används för att bevilja åtkomst till hanterade resursgruppen. Den här identifieraren tillhör utgivarens klient.

- **RoleDefinitionID**: Azure AD-identifieraren för den roll som tilldelats till den föregående UPN-ID. Det kan vara något av inbyggda roller för rollbaserad åtkomstkontroll i utgivarens klient. Mer information finns i [inbyggda roller för rollbaserad åtkomstkontroll i](../active-directory/role-based-access-built-in-roles.md).

## <a name="next-steps"></a>Nästa steg

* Information om hur du publicerar hanterade program till Marketplace finns [Azure hanterade program i Marketplace](managed-application-author-marketplace.md).
* Information om hur du använder ett hanterat program från Marketplace finns [använda Azure hanterade program i Marketplace](managed-application-consume-marketplace.md).
* Information om hur du publicerar ett program för Tjänstkatalog hanteras finns [skapa och publicera en applikation för Tjänstkatalog hanteras](managed-application-publishing.md).
* Information om att använda ett Tjänstkatalogen hanterade program, se [använder ett Tjänstkatalogen hanterade program](managed-application-consumption.md).
* Om du vill skapa en UI-definitionsfil finns [Kom igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
