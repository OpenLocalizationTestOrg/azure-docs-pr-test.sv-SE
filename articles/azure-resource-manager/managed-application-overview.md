---
title: aaaOverview Azure hanterade program | Microsoft Docs
description: "Beskriver hello begrepp för Azure hanterade program"
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 2fd1844a442515f4492c890c9798073475a66f88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-managed-applications-overview"></a>Översikt över Azure hanterade program

Leverantörer som använder Azure kan erbjuda lösningar toocustomers hello världen. Azure Marketplace är ett galleri som består av hundratals komplexa, multiresource mallar från första part och från tredje part-leverantörer. Kunder kan distribuera och börja använda plattform som en tjänst (PaaS) och programvara som en tjänst (SaaS)-program i minuter. 

Även om hello Marketplace innehåller ett bra sätt distribuera ett erbjudande för kunder tooquickly, hello kunden ansvarar för att bevara och uppdaterar hello lösning. Utöver hello virtuella avbildningen fakturering kan inte leverantörer debitera kunder för hello användning av ett program. Leverantörer kan dessutom förhindra kunder från att modifiera kritiska programresurser. Leverantörer kan också blockera åtkomst toointellectual egenskap som utgör ett program. Azure hanterade program tillhandahålla lösningar för dessa problem. 

Ett hanterat program är liknande tooa lösningsmall i hello Marketplace, med en nyckel skillnad. Hello resurser finns i ett hanterat program etablerade tooa resursgruppen som hanteras av hello leverantör. hello resursgruppen finns i hello kundens prenumeration, men en identitet i hello leverantörens klienten har åtkomst toohello resursgruppen.

## <a name="advantages-of-managed-applications"></a>Fördelarna med hanterade program

Hanterade tjänsteleverantörer (MSPs) ISV: er, och företagets central IT-team kan använda hanterade program toodeliver lösningar via hello Marketplace eller hello Tjänstkatalogen. Även om kunder distribuera dessa hanterade program i sina prenumerationer, de inte har toomaintain, uppdatera eller tjänsten dem. Eftersom leverantörer hantera och hello-program, har kunder inte toodevelop programspecifika domän knowledge toomanage dessa program. Kunder kan automatiskt hämta programuppdateringar för utan hello måste tooworry om felsökning och diagnos av problem med hello program.

För leverantörer och providers skapa en kanal toosell infrastruktur och programvara via hello Marketplace hanterade program. Hanterade program ger också ett sätt tooattach tjänster och operativa tooAzure-kunder. Leverantörer kan debiterar kunder med hjälp av hello Azure faktureringssystem. De kan använda mallar toomanage hello livscykeln för distribuerade program. Dessa lösningar är fristående och förseglade toohello kund så leverantörer kan betjäna av hög kvalitet. Den här metoden förmåner PaaS och SaaS-leverantörer. Det hjälper också till att företagets centrala plattform team och systemintegrerare (SIs) som vill toopackage och sälja sina lösningar.

## <a name="managed-application-types"></a>Hanterade programtyper
Azure hanterade program finns i två typer: Tjänstkatalogen och Marketplace.
 
### <a name="service-catalog"></a>Tjänstkatalog  

Med hello Tjänstkatalogen kan kunderna skapa en katalog godkända lösningar för Azure toobe som används av personer i organisationen. Exempelvis är en översikt över lösningar att upprätthålla bra för central IT-team i företag. De kan använda hello katalogen tooensure kompatibilitet med vissa organisationens normer medan de tillhandahålla lösningar för deras organisationer. De kan styra, uppdatera och hantera dessa program. Anställda kan använda hello katalogen tooeasily identifiera hello omfattande uppsättning program som rekommenderas och godkänts av sina IT-avdelningar. Kunder finns hello Tjänstkatalogen hanterade program som de har skapat. De kan också se hello hanterade program som andra personer i deras organisation delar med dem..
 
Information om hur du publicerar ett program för Tjänstkatalog hanteras finns [skapa och publicera en applikation för Tjänstkatalog hanteras](managed-application-publishing.md).
 
Information om att använda ett Tjänstkatalogen hanterade program, se [använder ett Tjänstkatalogen hanterade program](managed-application-consumption.md).
 
### <a name="marketplace"></a>Marketplace

Hanterade program är tillgängliga via hello Marketplace i hello Azure-portalen. När hello leverantör publicerar programmen, är de tillgängliga för alla inom och utanför en organisation tooconsume. Med den här metoden kan MSPs ISV: er och SIs erbjuda sina lösningar tooall Azure-kunder. Kunderna får hello fördel med dessa avancerade lösningar utan hello måste tooinvest för att förstå och underhåll av hello lösningar. 

För närvarande utgivare kan tillhandahålla sina erbjudanden som ett hanterat program eller en lösningsmall som har ohanterad. hello huvudkomponenter för publicering av ett hanterat program inkluderar hello mallfilen och definitionsfilen för hello-Användargränssnittet. hello beskrivs hello-resurser som har etablerats. hello UI definitionsfilen beskriver hur hello krävs indata för att etablera dessa resurser visas i hello portal. hello krävs filer i en .zip-fil och överförs via hello publishing portal.
 
Information om hur du publicerar ett hanterat program toohello Marketplace finns [Azure hanterade program i hello Marketplace](managed-application-author-marketplace.md).

Information om hur du använder ett hanterat program från hello Marketplace finns [använda Azure hanterade program i hello Marketplace](managed-application-consume-marketplace.md).

## <a name="key-concepts"></a>Viktiga begrepp

### <a name="managed-resource-group"></a>Hanterade resursgruppen.
hello hanterade resursgruppen är där alla hello Azure resurser som har etablerats i hello mall skapas. Om hello installation är används toocreate ett lagringskonto, innehåller den här resursgruppen hello lagringsresurs för kontot. Den innehåller inte hello installation resurs.

### <a name="appliance-package"></a>Paket för installation
hello skapas ett paket som innehåller hello mallfilerna och hello createUIDefinition fil. Mer specifikt innehåller hello följande filer:

- **applianceMainTemplate.json**: filen med den här mallen definierar alla hello-resurser som har etablerats av hello-enhet. Den här filen är en vanlig mall som har använt toocreate resurser.

- **MainTemplate.json**: filen med den här mallen definierar hello installation resurs (Microsoft.Solutions/appliances). En nyckelegenskap som definierats i den här resursen är ManagedResourceGroupId. Den här egenskapen anger vilken resursgrupp används toohost hello faktiska resurser som är definierade i applianceMainTemplate.json.

- **applianceCreateUIDefinition.json**: den här filen beskriver hur hello gränssnitt som behövs för hello parametrar som definierats i mallen för hello återges.

### <a name="authorization"></a>Auktorisering
hello publisher måste ange hello behörigheter som krävs av hello leverantör toomanage hello resurser för hello kunds räkning. Den här behörigheten gäller toohello hanterade resursgruppen. Ange hello följande värden:

- **PrincipalID**: hello Azure Active Directory (AD Azure) identifierare för hello användare, grupp eller program som har använt toogrant åtkomstgruppen toohello hanterade resurser. Den här identifieraren tillhör toohello publisher-klient.

- **RoleDefinitionID**: hello Azure AD-identifierare för hello tilldelats rollen toohello föregående UPN-ID. Det kan vara något av hello inbyggda rollbaserad åtkomstkontroll roller i hello publisher-klient. Mer information finns i [inbyggda roller för rollbaserad åtkomstkontroll i](../active-directory/role-based-access-built-in-roles.md).

## <a name="next-steps"></a>Nästa steg

* Information om publishing hanterade program toohello Marketplace finns [Azure hanterade program i hello Marketplace](managed-application-author-marketplace.md).
* Information om hur du använder ett hanterat program från hello Marketplace finns [använda Azure hanterade program i hello Marketplace](managed-application-consume-marketplace.md).
* Information om hur du publicerar ett program för Tjänstkatalog hanteras finns [skapa och publicera en applikation för Tjänstkatalog hanteras](managed-application-publishing.md).
* Information om att använda ett Tjänstkatalogen hanterade program, se [använder ett Tjänstkatalogen hanterade program](managed-application-consumption.md).
* toocreate en UI-definitionsfil finns [Kom igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
