---
title: "Aktivera HTTPS på en anpassad domän för Azure CDN | Microsoft Docs"
description: "Lär dig mer om att aktivera HTTPS på Azure CDN-slutpunkten med en anpassad domän."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: casoper
ms.openlocfilehash: b334ba6bbec1d0a7e23a514174bffae01c7fff05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a>Aktivera HTTPS på en anpassad domän för Azure CDN

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

HTTPS-stöd för Azure CDN anpassade domäner kan du leverera skyddat innehåll via SSL med hjälp av ditt eget domännamn för att förbättra säkerheten för data under överföringen. Slutpunkt till slutpunkt-arbetsflöde för att aktivera HTTPS för den anpassade domänen är förenklad via en enda klickning aktivering, fullständig certifikathantering och alla med utan extra kostnad.

Det är viktigt att säkerställa sekretess och dataintegriteten för alla program web känsliga data under överföringen. Med hjälp av HTTPS-protokollet garanterar att dina känsliga data krypteras när de skickas över internet. Den ger litar på autentisering och skyddar ditt webbprogram från attacker. För närvarande stöder Azure CDN HTTPS på en CDN-slutpunkt. Till exempel om du skapar en CDN-slutpunkt från Azure CDN (t.ex. https://contoso.azureedge.net) är HTTPS aktiverat som standard. Nu med anpassad domän HTTPS, kan du aktivera säker leverans för en anpassad domän (t.ex. https://www.contoso.com) samt. 

Några viktiga attribut för HTTPS-funktionen är:

- Utan extra kostnad: det finns inga kostnader för certifikatanskaffningen eller förnyelse och utan extra kostnad för HTTPS-trafik. Du betalar bara för GB utgående trafik från CDN.

- Enkel aktivering: en Klicka etablering är tillgänglig från den [Azure-portalen](https://portal.azure.com). Du kan också använda REST API eller andra verktyg för utvecklare för att aktivera funktionen.

- Slutföra certifikathantering: alla certifikat inköp och hanteras åt dig. Certifikaten etableras automatiskt och förnyas innan upphör att gälla. Detta tar bort riskerna med avbrott i tjänsten på grund av ett certifikat upphör att gälla helt.

>[!NOTE] 
>Innan du aktiverar stöd för HTTPS, du måste har upprättat en [Azure CDN domänen](./cdn-map-content-to-custom-domain.md).

## <a name="step-1-enabling-the-feature"></a>Steg 1: Aktivera funktionen 

1. I den [Azure-portalen](https://portal.azure.com), bläddra till Verizon standard eller premium CDN-profilen.

2. Klicka på den slutpunkt som innehåller din anpassade domän i listan över slutpunkter.

3. Klicka på den anpassade domänen som du vill aktivera HTTPS.

    ![Slutpunkten bladet](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Klicka på **på** att aktivera HTTPS och spara ändringen.

    ![Dialogrutan för anpassade HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a>Steg 2: Verifiering av domän

>[!IMPORTANT] 
>Du måste slutföra verifiering av domän innan HTTPS kommer att vara aktiv på den anpassade domänen. Du har 6 arbetsdagar att godkänna domänen. Begäran avbryts med ingen godkännande inom 6 arbetsdagar.  

När du har aktiverat HTTPS på den anpassade domänen verifierar DigiCert för providern våra HTTPS-certifikat ägarskap för domänen genom att kontakta registrant för din domän, baserat på WHOIS registrant information via e-post (som standard) eller telefon. DigiCert kommer också att skicka e-postmeddelandet till den nedan adresser. Om WHOIS registrant information är privat kan kontrollera att du kan godkänna direkt från någon av dessa adresser.

>Admin @< din-domain-name.com > administratör @< din-domain-name.com >  
>webbadministratör @< din-domain-name.com >  
>hostmaster @< din-domain-name.com >  
>postmaster @< din-domain-name.com >


Vid mottagning av e-postmeddelandet kan ha två alternativ för verifiering:

1. Du kan godkänna alla framtida beställningar via samma konto för samma rotdomänen, t.ex. consoto.com. Detta är en rekommenderad metod om du planerar att lägga till ytterligare anpassade domäner i framtiden för samma rotdomänen.
 
2. Du kan bara godkänna specifika värdnamnet som används i denna begäran. Ytterligare godkännande krävas för efterföljande förfrågningar.

    Exempel e-post:
    
    ![Dialogrutan för anpassade HTTPS](./media/cdn-custom-ssl/domain-validation-email-example.png)

Efter godkännande och läggs DigiCert ditt domännamn till SAN-certifikat. Certifikatet ska vara giltigt för ett år och automatisk förnyas innan den har upphört att gälla.

## <a name="step-3-wait-for-the-propagation-then-start-using-your-feature"></a>Steg 3: Vänta tills spridning och starta sedan din funktionen

När domännamnet har verifierats tar upp till 6 – 8 timmar för anpassade domäner HTTPS-funktionen ska vara aktiv. När processen är klar sätts ”anpassade HTTPS” status i Azure portal till ”aktiverad”. HTTPS med den anpassade domänen är nu klar att användas.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

1. *Vem är certifikat-providern och vilken typ av certifikat används?*

    Vi använder alternativa namn på CERTIFIKATMOTTAGARE certifikat som tillhandahålls av DigiCert. Ett SAN-certifikat kan skydda flera fullständigt kvalificerade domännamn med ett certifikat.

2. *Kan jag använda mitt dedikerade certifikat?*
    
    För närvarande inte, men det är på Översikt.

3. *Vad händer om jag får inte domänen e-postmeddelandet från DigiCert?*

    Kontakta Microsoft om du inte får ett e-postmeddelande inom 24 timmar.

4. *Använder ett SAN-certifikat som är mindre säker än ett dedikerat certifikat?*
    
    Ett SAN-certifikat följer samma kryptering och säkerhet standarder som en dedikerad certifikat. Alla utfärdade SSL-certifikat använder SHA-256 för förbättrad säkerhet.

5. *Kan jag använda domänen HTTPS med Azure CDN från Akamai?*

    Den här funktionen är för närvarande bara tillgänglig med Azure CDN från Verizon. Vi arbetar på stöder den här funktionen med Azure CDN från Akamai under de kommande månaderna.


## <a name="next-steps"></a>Nästa steg

- Lär dig hur du ställer in en [domänen på Azure CDN-slutpunkten](./cdn-map-content-to-custom-domain.md)


