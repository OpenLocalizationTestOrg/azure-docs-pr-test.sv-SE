---
ms.assetid: 
title: aaaAzure Key Vault mjuk borttagning | Microsoft Docs
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/10/2017
ms.openlocfilehash: 1b402c58db6f25ae4ae5e2720786fa81eb0e839a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-soft-delete-overview"></a>Översikt för mjuk borttagning av Azure Key Vault

Key Vault mjuk borttagning funktionen kan återställning av hello bort valv och valvet objekt, kallas även mjuk borttagning. Mer specifikt adressera vi hello följande scenarier:

- Stöd för återställningsbara borttagningen av ett nyckelvalv
- Stöd för återställningsbara borttagning av nyckelvalv objekt (t.ex. nycklar, hemligheter, certifikat)

## <a name="supporting-interfaces"></a>Stöder gränssnitt

hello soft-ta bort funktionen är är tillgängliga via hello vila, .NET / C# och PowerShell-gränssnitt. Se toohello referenser för dessa mer information [valvet nyckelreferens](https://docs.microsoft.com/azure/key-vault/).

## <a name="scenarios"></a>Scenarier

Azure Key valv är spårade resurser som hanteras av Azure Resource Manager. Azure Resource Manager anger också en väldefinierad beteendet för borttagning, vilket kräver att en lyckad borttagningsåtgärd måste resultera i den här resursen inte är tillgänglig längre. hello soft-ta bort funktionen adresser hello återställning av hello bort objektet om hello borttagning har oavsiktligt eller avsiktligt.

1. I hello typiskt scenario, kan en användare ha oavsiktligt bort ett nyckelvalv eller ett nyckelvalv-objekt. Om den nyckelvalv eller nyckel valvet objektet har toobe återställningsbara för en förutbestämd tid, hello användaren Ångra hello borttagning och återställa data.

2. I ett annat scenario kan en obehörig användare försöka toodelete ett nyckelvalv eller ett nyckelvalv objekt, t.ex en nyckel i ett valv, toocause avbrott i verksamheten. Avgränsa hello borttagning av hello nyckelvalv eller nyckelvalv objekt från hello faktiska borttagning av hello underliggande data kan användas som en säkerhetsåtgärd genom att, exempelvis att begränsa behörigheter för data tas bort tooa olika, betrodd roll. Den här metoden kräver i praktiken kvorum för en åtgärd som annars kan leda till en omedelbar dataförlust.

### <a name="soft-delete-behavior"></a>Beteende för mjuk borttagning

Med den här funktionen hello DELETE-åtgärden på ett nyckelvalv eller nyckelvalv objektet är en mjuk borttagning effektivt hålla hello resurser under en viss period, medan ger hello utseende hello objektet tas bort. ytterligare hello-tjänsten tillhandahåller en mekanism för att återställa hello bort objektet, i stort sett Ångra hello borttagning. 

Mjuk borttagning är ett valfritt Key Vault-beteende och **inte aktiverad som standard** i den här versionen. Mer information om hur du aktiverar mjuk borttagning för nyckelvalvet, finns hello specifika anvisningar i hello referens för hello-gränssnittet för ditt val [valvet nyckelreferens](https://docs.microsoft.com/azure/key-vault/).

### <a name="key-vault-recovery"></a>Nyckelvalv återställning

När du tar bort ett nyckelvalv skapar hello tjänsten en proxy-resurs under hello-prenumeration, lägga till tillräckligt metadata för återställning. hello proxy resursen är en lagrade objektet finns i hello samma plats som hello bort nyckelvalvet. 

### <a name="key-vault-object-recovery"></a>Nyckelvalv objektåterställning

När du tar bort ett nyckelvalv objekt, t.ex en nyckel placerar hello service hello objekt i ett borttaget tillstånd vilket gör den tillgänglig tooany hämtning av åtgärder. I det här tillståndet kan hello nyckelvalv objekt bara visas, återställda eller tvång/tas bort permanent. 

Vid hello samma time, Key Vault schemalägger hello borttagning av hello underliggande data motsvarande toohello bort nyckelvalv eller nyckelvalv objekt för körning när ett förutbestämt Kvarhållningsintervall. hello DNS-poster motsvarande toohello valv bevaras också hello länge hello Kvarhållningsintervall.

### <a name="soft-delete-retention-period"></a>Kvarhållningsperiod för mjuk borttagning

Ej permanent borttagna resurser bevaras under en angiven tidsperiod, 90 dagar. Under hello mjuk borttagning Kvarhållningsintervall, hello följande gäller:

- Du kan visa alla hello nyckelvalv och nyckelvalv objekt borttagningsstatus hello soft-för din prenumeration, samt komma åt tas bort och återställa information om dem.
    - Endast användare med särskilda behörigheter kan visa en lista över borttagna valv. Vi rekommenderar att våra användare skapar en anpassad roll med dessa särskilda behörigheter för hantering av bort valv.
- Nyckelvalv med samma namn inte kan skapas i hello hello samma plats. på motsvarande sätt ett nyckelvalv-objekt kan inte skapas i ett givet valv om att nyckelvalvet innehåller ett objekt med hello samma namn och som är i ett borttaget tillstånd 
- Endast en specifikt Privilegierade användare kan återställa en nyckelvalv eller ett nyckelvalv objekt genom att utfärda kommandot Återställ på hello motsvarande proxy-resurs.
    - hello användaren tillhör hello anpassad roll, som har hello privilegium toocreate nyckelvalv under hello resursgrupp kan återställa hello-valvet.
- Bara en specifikt Privilegierade användare kan framtvinga ta bort ett nyckelvalv eller nyckelvalv objekt genom att utfärda ett borttagningskommando på hello motsvarande proxy-resurs.

Om ett nyckelvalv eller nyckelvalv objekt återställs utför hello slutet av hello kvarhållning intervall hello-tjänsten en rensning av hello ej permanent borttagna nyckelvalv eller nyckelvalv objekt och dess innehåll. Ta bort resursen kan inte planeras.

### <a name="permitted-purge"></a>Tillåtna Rensa

Permanent ta bort, rensa, ett nyckelvalv går via en POST-åtgärd på hello proxy resursen och kräver särskilda behörigheter. I allmänhet blir endast hello prenumerationsägaren kan toopurge ett nyckelvalv. hello efter åtgärden utlösare hello omedelbart och oåterkalleligt borttagning av det valvet. 

Ett undantag toothis gäller hello när hello Azure-prenumeration har markerats som *permanent*. I det här fallet hello-tjänsten kan sedan utföra hello faktiska borttagning och sker detta som en schemalagd process. 



