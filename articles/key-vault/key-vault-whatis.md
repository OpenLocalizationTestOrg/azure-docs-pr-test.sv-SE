---
title: "aaaWhat är Azure Key Vault? | Microsoft Docs"
description: "Azure Key Vault hjälper dig att skydda krypteringsnycklar och hemligheter som används av molnprogram och molntjänster. Genom att använda Azure Key Vault kan kunder kryptera nycklar och hemligheter (till exempel autentiseringsnycklar, lagringskontonycklar, datakrypteringsnycklar, PFX-filer och lösenord) med hjälp av nycklar som skyddas av maskinvarusäkerhetsmoduler (HSM)."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 296fcce03658b96b84afab299b73681bbe8ac9fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-key-vault"></a>Vad är Azure Key Vault?
Azure Key Vault är tillgängligt i de flesta regioner. Mer information finns i hello [Key Vault-priser](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Introduktion
Azure Key Vault hjälper dig att skydda krypteringsnycklar och hemligheter som används av molnprogram och molntjänster. Med Key Vault kan du kryptera nycklar och hemligheter (till exempel autentiseringsnycklar, lagringskontonycklar, datakrypteringsnycklar, PFX-filer och lösenord) med hjälp av nycklar som skyddas av maskinvarusäkerhetsmoduler (HSM). För ytterligare säkerhet kan du importera eller generera nycklar i HSM-moduler. Om du väljer toodo processer detta, Microsoft Level dina nycklar i FIPS 140-2 2 verifierade HSM (maskinvara och inbyggd programvara).  

Key Vault förenklar hello nyckelhanteringen och låter dig toomaintain kontrollen över nycklar som kommer åt och krypterar dina data. Utvecklare kan skapa nycklar för utveckling och testning på några minuter och sedan sömlöst migrera dem tooproduction nycklar. Säkerhetsadministratörer kan bevilja (och återkalla) behörighet tookeys efter behov.

Använd hello efter tabellen toobetter förstå hur Key Vault kan toomeet hello behoven för utvecklare och säkerhetsadministratörer.

| Roll | Problembeskrivning | Åtgärdas av Azure Key Vault |
| --- | --- | --- |
| Azure-programutvecklare |”Jag vill toowrite ett program för Azure som använder nycklar för signering och kryptering, men jag vill att dessa nycklar toobe externa från mitt program så att hello lösningen passar ett program som distribueras geografiskt. <br/><br/>Jag vill också att dessa nycklar och hemligheter toobe skyddas utan att behöva toowrite hello koden själv. Jag vill också att dessa nycklar och hemligheter toobe enkelt för mig toouse från Mina program, med optimala prestanda ”. |√ Nycklar lagras i ett valv och anropas av en URI vid behov.<br/><br/> √ Nycklar skyddas av Azure med branschstandardalgoritmer, nyckellängder och maskinvarusäkerhetsmoduler (HSM).<br/><br/> √ Nycklar bearbetas i HSM som finns i hello samma Azure-datacenter som hello program. Detta ger bättre tillförlitlighet och minskade svarstider än om hello nycklarna finns på en separat plats, till exempel lokalt. |
| Utvecklare av programvara som en tjänst (SaaS) |”Jag vill inte hello ansvar eller hitta potentiella ansvar för Mina kunders klientnycklar och hemligheter. <br/><br/>Jag vill hello kunder tooown och hantera sina nycklar så att jag kan koncentrera dig på att göra vad jag gör bäst, som tillhandahåller hello programvara kärnfunktioner ”. |√ Kunder kan importera sina egna nycklar till Azure och hantera dem. När ett SaaS-program måste tooperform kryptografiska åtgärder med hjälp av sina kunders nycklar, gör Key Vault dessa åtgärder åt hello program. hello-programmet kan inte se hello kundernas nycklar. |
| Säkerhetschef |”Jag vill tooknow att våra program uppfyller FIPS 140-2 Level 2-HSM för säker nyckelhantering. <br/><br/>Jag vill toomake till att min organisation kontroll över hello viktiga livscykel och kan övervaka nyckelanvändningen. <br/><br/>Och även om vi använder flera Azure-tjänster och resurser, jag vill toomanage hello nycklar från en enda plats i Azure ”. |√ HSM-modulerna är FIPS 140-2 Level 2-verifierade.<br/><br/>√ Key Vault är utformat så att Microsoft inte kan se eller extrahera dina nycklar.<br/><br/>√ Loggningen av nyckelanvändningen sker praktiskt taget i realtid.<br/><br/>√ hello valvet tillhandahåller ett enda gränssnitt, oavsett hur många valv du har i Azure, vilka regioner de support och vilka program som använder dem. |

Vem som helst med en Azure-prenumeration kan skapa och använda nyckelvalv. Key Vault hjälper utvecklare och säkerhetsadministratörer, men kan även implementeras och hanteras av en administratör som hanterar andra Azure-tjänster för en organisation. Den här administratören skulle till exempel logga in med en Azure-prenumeration, skapa ett valv för hello organisation i vilken toostore nycklar och sedan ansvara för driftåtgärder, t.ex:

* Skapa eller importera en nyckel eller hemlighet.
* Återkalla eller ta bort en nyckel eller hemlighet.
* Auktorisera användare eller program tooaccess hello nyckelvalvet, så att de kan sedan hantera eller använda sina nycklar och hemligheter
* Konfigurera nyckelanvändningen (till exempel signera eller kryptera).
* Övervaka nyckelanvändningen.

Den här administratören kan sedan ge utvecklare URI: er toocall från sina program och förse säkerhetsadministratören med logginformation för nyckelanvändning. 

   ![Översikt över Azure Key Vault][1]

Utvecklare kan också hantera hello nycklar direkt, med hjälp av API: er. Mer information finns i [hello Key Vault Utvecklarhandbok](key-vault-developers-guide.md).

## <a name="next-steps"></a>Nästa steg
En Komma igång-självstudiekurs för administratörer finns i [Komma igång med Azure Key Vault](key-vault-get-started.md).

Mer information om av användningsloggning för Key Vault finns i avsnittet om [Azure Key Vault-loggning](key-vault-logging.md).

Mer information om hur du använder nycklar och hemligheter med Azure Key Vault finns i [About Keys, Secrets, and Certificates](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx) (Om nycklar, hemligheter och certifikat).

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
