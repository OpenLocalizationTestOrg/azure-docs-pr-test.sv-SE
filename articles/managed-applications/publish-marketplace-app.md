---
title: Azure hanterade program i Marketplace | Microsoft Docs
description: "Beskriver Azure hanterade program som är tillgängliga via Marketplace."
services: azure-resource-manager
author: tfitzmac
manager: timlt
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/18/2018
ms.author: tomfitz
ms.openlocfilehash: fccc2dbb7623f4ceb0d3decc7037f75a05858910
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/19/2018
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Azure hanterade program i Marketplace

Leverantörer kan använda Azure hanterade program att erbjuda sina lösningar för alla Azure Marketplace-kunder. Dessa leverantörer kan innehålla hanterade tjänsteleverantörer (MSPs), oberoende programvaruleverantörer (ISV) och systemintegrerare (SIs). Hanterade program minska underhållet och underhåll kostnader för kunder. Leverantörer sälja infrastruktur- och programvara på marknadsplatsen. De kan koppla tjänster och operativa stöd till hanterade program. Mer information finns i [hanteras Programöversikt](overview.md).

Den här artikeln förklarar hur du publicerar ett program till marketplace och göra den tillgänglig för kunder.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Krav för att publicera ett hanterat program

För att slutföra den här artikeln, måste du redan har ZIP-filen för din definition för hanterade program. Mer information finns i [skapa katalogen tjänstprogram](publish-service-catalog-app.md).

Dessutom finns flera förutsättningar för företag. De är:

* Ditt företag eller dess dotterbolag måste finnas i ett land där försäljning stöds av marketplace.
* Produkten måste ha licens på ett sätt som är kompatibel med fakturering modeller som stöds av marketplace.
* Tillhandahålla teknisk support för kunder på ett kommersiellt rimliga sätt. Stöd kan vara fria, betald, eller via community stöd.
* Licens för programvaran och eventuella beroenden för programvara från tredje part.
* Ange innehåll som uppfyller villkoren för ditt erbjudande ska visas i Marketplace och i Azure-portalen.
* Acceptera villkoren i avtalet för utgivaren och Azure Marketplace deltagande principer.
* Acceptera att uppfylla den användningsvillkoren och sekretesspolicyn för Microsoft certifierade programavtalet för Microsoft Azure.

## <a name="become-a-publisher"></a>Bli en utgivare

Om du vill bli en utgivare i Azure Marketplace, måste du:

1. Skapa ett Microsoft-ID - skapa ditt Microsoft-konto med hjälp av en e-postadress som hör till företagets domän, men inte till enskilda användare. E-postadressen används för både Microsoft Developer Center och molnet Partner-portalen. Mer information finns i [Azure Marketplace Publisher Guide](https://aka.ms/sellerguide).
1. Skicka [Azure Marketplace kandidat formuläret](https://aka.ms/ampnomination) – **lösning som du vill publicera?**väljer **hanterat program**. När formuläret skickas Marketplace Onboarding-teamet har granskat programmet och verifierar begäran. Godkännandeprocessen kan ta en till tre dagar. När din kandidat har godkänts får du följer en kod för att avstå från registreringsavgift för developer center. Om du gör **inte** Slutför formuläret Marketplace kandidat du uppmanas att betala en avgift för registrering av $99.
1. Registrera i [Developer Center](https://developer.microsoft.com) -Microsoft bekräftar att din organisation är en giltig juridisk person med ett giltigt SKATTE-ID för det land där de är registrerade. Godkännandeprocessen kan ta 5 till 10 dagar. Använd kampanjkoden som du fick i e-post från kandidat processen för att undvika registreringsavgift. Mer information finns i [Azure Marketplace Publisher Guide](https://aka.ms/sellerguide).
1. Logga in på [moln partnerportalen](https://cloudpartner.azure.com) – i profilen för utgivaren associera Developer Center-konto med profilen Marketplace utgivare. Mer information finns i [Azure Marketplace Publisher Guide](https://aka.ms/sellerguide).

## <a name="create-a-new-azure-application-offer"></a>Skapa ett nytt erbjudande för Azure-program

När du har skapat ditt partner-portalen konto är du redo att skapa erbjudandet hanterade program.

### <a name="set-up-an-offer"></a>Konfigurera ett erbjudande

Erbjudande för ett hanterat program motsvarar en klass av produkten erbjudande från en utgivare. Om du har en ny typ av program som du vill ska vara tillgängliga i marketplace kan konfigurera du den som ett nytt erbjudande. Ett erbjudande är en samling av SKU: er. Varje erbjudande visas som sin egen enhet i marketplace.

1. Logga in på den [molnpartnerportalen](https://cloudpartner.azure.com/).

1. I navigeringsfönstret till vänster, Välj **+ nytt erbjudande** > **Azure program**.

1. I den **Editor** kan du se de nödvändiga formulär. Varje formulär beskrivs senare i den här artikeln.

## <a name="offer-settings-form"></a>Erbjudande inställningar för formulär

Fälten för den **erbjuder inställningar** formatet är:

* **Erbjudande-ID**: den unika identifieraren identifierar erbjudandet i en profil för utgivaren. Detta ID visas i produkten URL: er, Resource Manager-mallar och fakturering rapporter. Det kan bara består av gemena alfanumeriska tecken och bindestreck (-). ID: T får inte sluta med ett streck. Det är begränsat till högst 50 tecken. När ett erbjudande lanseras är i det här fältet låst.
* **Publicerings-ID**: Använd den här listrutan för att välja publisher-profil som du vill publicera det här erbjudandet under. När ett erbjudande lanseras är i det här fältet låst.
* **Namnet**: det här visningsnamnet för ditt erbjudande visas i Marketplace och i portalen. Det kan ha högst 50 tecken. Innehåller ett okänt varumärke för produkten. Inkludera inte ditt företagsnamn, om det inte är hur släpps. Om du marknadsföring erbjudandet på din egen webbplats, måste du kontrollera att namnet är exakt hur den visas på webbplatsen.

När du är klar väljer **spara** att spara ditt arbete.

## <a name="skus-form"></a>SKU: er formulär

Nästa steg är att lägga till SKU: er för ditt erbjudande.

En SKU är den minsta köpbara enheten ett erbjudande. Du kan använda en SKU inom samma produkten klass (erbjudandet) för att skilja mellan:

* Olika funktioner som stöds
* Om erbjudandet är hanterade eller ohanterade
* Fakturering modeller som stöds

En SKU visas under överordnade erbjudandet på marketplace. Det verkar som sin egen köpbara enhet i Azure-portalen.

1. Välj **SKU: er** > **nya SKU**.

1. Ange en **SKU ID**. En SKU-ID är en unik identifierare för SKU: N i ett erbjudande. Detta ID visas i produkten URL: er, Resource Manager-mallar och fakturering rapporter. Det kan bara består av gemena alfanumeriska tecken och bindestreck (-). ID: T får inte sluta med ett streck och är begränsat till högst 50 tecken. När ett erbjudande lanseras är i det här fältet låst. Du kan ha flera SKU: er i ett erbjudande. Du behöver en SKU för varje avbildning som du planerar att publicera.

1. Fyll i den **SKU information** avsnitt i följande format:

   Fylla i följande fält:

   * **Rubrik**: Ange en rubrik för denna SKU. Den här rubriken visas i galleriet för det här objektet.
   * **Översikt över**: Ange en kort sammanfattning för denna SKU. Den här texten visas under rubriken.
   * **Beskrivning**: Ange en detaljerad beskrivning av SKU: N.
   * **SKU-typen**: de tillåtna värdena är *hanterat program* och *Lösningsmallar*. Det här fallet markerar *hanterat program*.
   * **Land/Region tillgänglighet**: Välj de länder där det hanterade programmet är tillgängligt.
   * **Priser**: Ange ett pris för hanteringen av programmet. Välj de tillgängliga länderna innan du anger priset.

1. Lägg till ett nytt paket. Fyll i den **Paketinformation** avsnitt i följande format:

   Fylla i följande fält:

   * **Aktuell Version**: Ange en version för det paket som du överför. Det bör vara i formatet `{number}.{number}.{number}{number}`.
   * **Välj en paketfil**: det här paketet innehåller två nödvändiga filer komprimeras till en ZIP-paketet. En fil är en Resource Manager-mall som definierar resurserna som ska distribueras för det hanterade programmet. Den andra filen definierar den [användargränssnittet](create-uidefinition-overview.md) för konsumenter distribution av hanterade program via portalen. I användargränssnittet anger du element som möjlighet att ange parametervärden.
   * **PrincipalId**: den här egenskapen är Azure Active Directory (Azure AD)-ID för en användare, grupp eller ett program som beviljas åtkomst till resurser i kundens prenumeration. Rolldefinitionen beskriver behörigheten.
   * **Rolldefinitionen**: den här egenskapen är en lista med alla inbyggda rollbaserad åtkomstkontroll (RBAC) roller tillhandahålls av Azure AD. Du kan välja den roll som är mest lämpligt att använda för att hantera resurserna för kundens räkning.

Du kan lägga till flera tillstånd. Vi rekommenderar att du skapar en grupp i AD-användare och ange dess ID i **PrincipalId**. På så sätt kan du lägga till fler användare i användargruppen utan att behöva uppdatera SKU: N.

Läs mer om hur RBAC [komma igång med RBAC på Azure portal](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Marketplace-formulär

Marketplace-formulär uppmanar för fält som visas på den [Azure Marketplace](https://azuremarketplace.microsoft.com) och på den [Azure-portalen](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Prenumerations-ID: N för förhandsgranskning

Ange en lista över Azure-prenumeration ID: N som kan komma åt erbjudandet när den har publicerats. Du kan använda dessa vitt visas prenumerationer för att testa förhandsgranskade erbjudandet innan du gör den live. Du kan sammanställa en lista för tillåten av upp till 100 prenumerationer i partnerportalen.

### <a name="suggested-categories"></a>Föreslagna kategorier

Välj upp till fem kategorier i listan som erbjudandet bäst kan associeras med. Dessa kategorier som används för att mappa erbjudandet till produktkategorier som är tillgängliga i den [Azure Marketplace](https://azuremarketplace.microsoft.com) och [Azure-portalen](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Marketplace

Sammanfattning av det hanterade programmet visar följande fält:

![Marketplace-sammanfattning](./media/publish-marketplace-app/publishvm10.png)

Den **översikt över** för det hanterade programmet visar följande fält:

![Marketplace-översikt](./media/publish-marketplace-app/publishvm11.png)

Den **planer + priser** för det hanterade programmet visar följande fält:

![Marketplace-planer](./media/publish-marketplace-app/publishvm15.png)

#### <a name="azure-portal"></a>Azure Portal

Sammanfattning av det hanterade programmet visar följande fält:

![Översikt över Portal](./media/publish-marketplace-app/publishvm12.png)

Översikt för det hanterade programmet visar följande fält:

![Portalen översikt](./media/publish-marketplace-app/publishvm13.png)

#### <a name="logo-guidelines"></a>Logotypen riktlinjer

Följ dessa riktlinjer för en logotyp som du överför i molnpartnerportalen:

*   Azure designen har en enkel färgpalett. Begränsa antalet primära och sekundära färger i din logotyp.
*   Färger med portalen är vit och svart. Använd inte färgerna som bakgrundsfärg för din logotyp. Använd en färg som gör din logotyp framträdande i portalen. Vi rekommenderar enkla primära färger. *Om du använder en genomskinlig bakgrund, se till att logotyp och texten inte vitt, svart eller blå.*
*   Använd inte en toning bakgrund på logotypen.
*   Placera inte text på logotypen, inte ens företaget eller varumärke. Utseendet och känslan logotypens bör platt och undvika toningar.
*   Kontrollera att logotypen inte har sträckts ut.

#### <a name="hero-logo"></a>Hjälte-logotyp

Logotypen hjälte är valfritt. Utgivaren kan du inte överföra en hjälte logotyp. När ikonen hjälte har överförts kan inte tas bort. Partnern måste följa Marketplace riktlinjer för hjälte ikoner som helst.

Följ dessa riktlinjer för ikonen hjälte logotyp:

*   Visningsnamn för utgivaren, plan rubrik och erbjudandet lång sammanfattning visas i vitt. Därför inte använda en ljusare ikonen hjälte bakgrunden. En svart, vit eller genomskinlig bakgrund är inte tillåten för hjälte ikoner.
*   När erbjudandet visas inbäddade element via programmering i hjälte logotypen. Inbäddade elementen omfattar visningsnamn utgivare, planera rubrik, erbjudandet lång sammanfattning och **skapa** knappen. Därför inte ange valfri text när du utformar hjälte logotypen. Lämna tomt utrymme till höger eftersom texten programmässigt ingår i detta utrymme. Det tomma utrymmet för texten som ska vara 415 x 100 bildpunkter till höger. Det är förskjutning med 370 bildpunkter från vänster.

    ![Hjälte logotypen exempel](./media/publish-marketplace-app/publishvm14.png)

## <a name="support-form"></a>Stöd för formulär

Fyll i den **stöder** formuläret med stöd för kontakter från ditt företag. Den här informationen kan vara engineering customer support kontakter.

## <a name="publish-an-offer"></a>Publicera ett erbjudande

När du fyller i alla avsnitt markerar **publicera** att starta processen som gör erbjudandet tillgängliga för kunder.

## <a name="next-steps"></a>Nästa steg

* En introduktion till hanterade program finns i [Managed application overview](overview.md) (Översikt över hanterade program).
* Information om hur du publicerar ett program för Tjänstkatalog hanteras finns [skapa och publicera en applikation för Tjänstkatalog hanteras](publish-service-catalog-app.md).