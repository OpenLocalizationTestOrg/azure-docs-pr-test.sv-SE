---
title: "aaa ”aktivera HTTPS på en anpassad domän för Azure CDN | Microsoft Docs ”"
description: "Lär dig hur tooenable HTTPS på Azure CDN-slutpunkten med en anpassad domän."
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
ms.openlocfilehash: 93746222616c9ed7977ec3b22c38ac1d43b118f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a>Aktivera HTTPS på en anpassad domän för Azure CDN

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

HTTPS-stöd för Azure CDN anpassade domäner möjliggör toodeliver skyddat innehåll via SSL med hjälp av en egen domän namn tooimprove hello säkerheten för data under överföringen. hello slutpunkt till slutpunkt-arbetsflöde tooenable HTTPS för den anpassade domänen är förenklad via en enda klickning aktivering, fullständig certifikathantering och alla med utan extra kostnad.

Det är kritiska tooensure hello sekretess och dataintegriteten för alla program web känsliga data under överföringen. Med hjälp av HTTPS-protokollet garanterar att dina känsliga data krypteras när de skickas över hello hello internet. Den ger litar på autentisering och skyddar ditt webbprogram från attacker. För närvarande stöder Azure CDN HTTPS på en CDN-slutpunkt. Till exempel om du skapar en CDN-slutpunkt från Azure CDN (t.ex. https://contoso.azureedge.net) är HTTPS aktiverat som standard. Nu med anpassad domän HTTPS, kan du aktivera säker leverans för en anpassad domän (t.ex. https://www.contoso.com) samt. 

Några av hello nyckelattribut för HTTPS-funktionen är:

- Utan extra kostnad: det finns inga kostnader för certifikatanskaffningen eller förnyelse och utan extra kostnad för HTTPS-trafik. Du betalar bara för GB utgående trafik från hello CDN.

- Enkel aktivering: en Klicka etablering är tillgänglig från hello [Azure-portalen](https://portal.azure.com). Du kan också använda REST API eller någon annan developer tools tooenable hello-funktion.

- Slutföra certifikathantering: alla certifikat inköp och hanteras åt dig. Certifikat tillhandahålls automatiskt och förnyas tidigare tooexpiration. Detta tar bort hello riskerna med avbrott i tjänsten på grund av ett certifikat upphör att gälla helt.

>[!NOTE] 
>HTTPS-stöd för tidigare tooenabling, du måste har upprättat en [Azure CDN domänen](./cdn-map-content-to-custom-domain.md).

## <a name="step-1-enabling-hello-feature"></a>Steg 1: Aktivera hello-funktionen 

1. I hello [Azure-portalen](https://portal.azure.com), bläddra tooyour Verizon standard eller premium CDN-profilen.

2. Hello-slutpunkt som innehåller den anpassade domänen på hello listan över slutpunkter.

3. Klicka på hello anpassad domän för vilken du vill tooenable HTTPS.

    ![Slutpunkten bladet](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Klicka på **på** tooenable HTTPS och spara hello ändringen.

    ![Dialogrutan för anpassade HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a>Steg 2: Verifiering av domän

>[!IMPORTANT] 
>Du måste slutföra verifiering av domän innan HTTPS kommer att vara aktiv på den anpassade domänen. Du har 6 företag dagar tooapprove hello domän. Begäran avbryts med ingen godkännande inom 6 arbetsdagar.  

När du har aktiverat HTTPS på den anpassade domänen verifierar DigiCert för providern våra HTTPS-certifikat ägarskap för domänen genom att kontakta hello registrant för din domän, baserat på WHOIS registrant information via e-post (som standard) eller telefon. DigiCert skickar också hello verifiering e-toohello nedan adresser. Om WHOIS registrant information är privat kan kontrollera att du kan godkänna direkt från någon av dessa adresser.

>Admin @< din-domain-name.com > administratör @< din-domain-name.com >  
>webbadministratör @< din-domain-name.com >  
>hostmaster @< din-domain-name.com >  
>postmaster @< din-domain-name.com >


Vid hello e-postmeddelanden, har du två alternativ för verifiering:

1. Du kan godkänna alla framtida beställningar via hello samma konto för hello samma rotdomänen, t.ex. consoto.com. Det här är en rekommenderad metod om du planerar tooadd ytterligare anpassade domäner i hello framtida för hello samma rotdomänen.
 
2. Du kan bara godkänna hello specifika värdnamn används i denna begäran. Ytterligare godkännande krävas för efterföljande förfrågningar.

    Exempel e-post:
    
    ![Dialogrutan för anpassade HTTPS](./media/cdn-custom-ssl/domain-validation-email-example.png)

Efter godkännande och DigiCert lägger till dina anpassade domäner namnet toohello SAN-certifikat. hello certifikatet är giltigt i ett år och automatisk förnyas innan den har upphört att gälla.

## <a name="step-3-wait-for-hello-propagation-then-start-using-your-feature"></a>Steg 3: Vänta tills hello spridningen och starta sedan din funktionen

När hello domännamnet har verifierats tar upp hello domänen HTTPS funktionen toobe active too6 8 timmar. När hello processen är slutförd anges hello ”anpassade HTTPS” status i hello Azure-portalen för ”Enabled”. HTTPS med den anpassade domänen är nu klar att användas.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

1. *Vem är hello certifikatutfärdaren och vilken typ av certifikat som ska användas?*

    Vi använder alternativa namn på CERTIFIKATMOTTAGARE certifikat som tillhandahålls av DigiCert. Ett SAN-certifikat kan skydda flera fullständigt kvalificerade domännamn med ett certifikat.

2. *Kan jag använda mitt dedikerade certifikat?*
    
    För närvarande inte, men dess på hello Översikt.

3. *Vad händer om jag får inte hello domän e-postmeddelandet från DigiCert?*

    Kontakta Microsoft om du inte får ett e-postmeddelande inom 24 timmar.

4. *Använder ett SAN-certifikat som är mindre säker än ett dedikerat certifikat?*
    
    Ett SAN-certifikat som följer hello samma standarder som en dedikerad cert som kryptering och säkerhet. Alla utfärdade SSL-certifikat använder SHA-256 för förbättrad säkerhet.

5. *Kan jag använda domänen HTTPS med Azure CDN från Akamai?*

    Den här funktionen är för närvarande bara tillgänglig med Azure CDN från Verizon. Vi arbetar på stöder den här funktionen med Azure CDN från Akamai hello kommande månaderna.


## <a name="next-steps"></a>Nästa steg

- Lär dig hur tooset upp en [domänen på Azure CDN-slutpunkten](./cdn-map-content-to-custom-domain.md)


