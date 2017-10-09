---
title: aaaHow tooadminister Azure Redis-Cache | Microsoft Docs
description: "Lär dig hur tooperform administrationsuppgifter som omstart och Schemalägg uppdateringar för Azure Redis-Cache"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 8c915ae6-5322-4046-9938-8f7832403000
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 07/05/2017
ms.author: sdanie
ms.openlocfilehash: eb24668a3f6264444e7d4daf1ac43b41b12dfe66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadminister-azure-redis-cache"></a>Hur tooadminister Azure Redis-Cache
Det här avsnittet beskrivs hur tooperform administrativa uppgifter som [omstart](#reboot) och [schemalägga uppdateringar](#schedule-updates) för Azure Redis-Cache-instanser.

## <a name="reboot"></a>Starta om
Hej **omstart** bladet kan du tooreboot en eller flera noder i ditt cacheminne. Den här funktionen för omstart kan du tootest ditt program för återhämtning om det finns ett fel i en cachenod.

![Starta om](./media/cache-administration/redis-cache-administration-reboot.png)

Välj hello noder tooreboot och klicka på **omstart**.

![Starta om](./media/cache-administration/redis-cache-reboot.png)

Om du har en premium-cache med aktiverad klustring, kan du välja vilka delar av hello cache tooreboot.

![Starta om](./media/cache-administration/redis-cache-reboot-cluster.png)

tooreboot en eller flera noder i ditt cacheminne, Välj önskad hello noder och klicka på **omstart**. Om du har en premium-cache med aktiverad klustring, Välj önskad hello shards tooreboot och klicka sedan på **omstart**. Hej valda noderna omstart efter några minuter och är tillbaka online och ett par minuter senare.

hello inverkan på klientprogram som varierar beroende på vilka noder att du startar om.

* **Master** – när hello Huvudnoden är omstartad, Azure Redis-Cache misslyckas över toohello replik nod och främjar den toomaster. Under den här redundansen kan det finnas ett kort intervall där anslutningar kan misslyckas toohello cache.
* **Underordnade** – när hello underordnade nod startas är vanligtvis ingen påverkan toocache klienter.
* **Huvud- och underordnade** – när båda cachenoderna startas om, försvinner alla data i hello cache och anslutningar toohello cache misslyckas tills hello primära noden är online igen. Om du har konfigurerat [-datapersistence](cache-how-to-premium-persistence.md), senaste säkerhetskopian av hello återställs när hello cache är online igen, men alla cache-skrivningar som uppstått efter hello den senaste säkerhetskopieringen går förlorade.
* **Noder i ett premium-cache med aktiverad klustring** – när du startar om en eller flera noder i premium-cache med kluster aktiverad hello beteende för hello valda noder är hello densamma som när du startar om hello motsvarande nod eller noder i en icke-klustrad cache.

> [!IMPORTANT]
> Starta om datorn är nu tillgänglig för alla prisnivåer.
> 
> 

## <a name="reboot-faq"></a>Starta om vanliga frågor och svar
* [Vilken nod bör jag startar om tootest mitt program?](#which-node-should-i-reboot-to-test-my-application)
* [Kan jag startar om hello cache tooclear klientanslutningar?](#can-i-reboot-the-cache-to-clear-client-connections)
* [Jag förlorar data från min cachen om jag göra en omstart?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
* [Kan jag startar om min cacheminne med hjälp av PowerShell, CLI eller andra hanteringsverktyg?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
* [Priser för vilka nivåer kan använda hello omstart funktioner?](#what-pricing-tiers-can-use-the-reboot-functionality)

### <a name="which-node-should-i-reboot-tootest-my-application"></a>Vilken nod bör jag startar om tootest mitt program?
tootest hello återhämtning för ditt program mot fel för hello primära noden till ditt cacheminne omstart hello **Master** nod. tootest hello återhämtning för ditt program mot fel på hello sekundära noden omstart hello **slavserver** nod. tootest hello återhämtning av programmet mot totalt hello-cache, omstart **både** noder.

### <a name="can-i-reboot-hello-cache-tooclear-client-connections"></a>Kan jag startar om hello cache tooclear klientanslutningar?
Ja, om du startar om hello cache alla klientanslutningar avmarkerad. Omstart kan vara användbart i hello fall där alla klientanslutningar används på grund av tooa logikfel eller ett programfel i hello-klientprogrammet. Varje prisnivå har olika [klienten anslutningsgränser](cache-configure.md#default-redis-server-configuration) för hello olika storlekar och inga fler klientanslutningar accepteras när dessa gränser har uppnåtts. Omstart hello cachen innehåller en sätt tooclear alla klientanslutningar.

> [!IMPORTANT]
> Om du startar om din cache tooclear klientanslutningar återansluter StackExchange.Redis automatiskt när hello Redis noden är online igen. Om hello underliggande problem kvarstår kan hello klientanslutningar fortsätta toobe in.
> 
> 

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Jag förlorar data från min cachen om jag göra en omstart?
Om du startar om båda hello **Master** och **slavserver** noder, alla data i cacheminnet hello (eller i den Fragmentera om du använder en premium-cache med aktiverad klustring) går förlorade. Om du har konfigurerat [-datapersistence](cache-how-to-premium-persistence.md), hello senaste säkerhetskopian ska återställas när hello cache är online igen, men alla cache-skrivningar som har inträffat när hello säkerhetskopieringen gjordes går förlorade.

Om du startar om en av noderna hello data försvinner inte normalt, men det kan fortfarande vara. Till exempel om hello huvudnoden startas om och en cache-skrivning pågår, hello data från hello cache write är förlorade. Ett annat scenario för dataförlust skulle vara om du vill starta om en nod och hello andra nod händer toogo ned på grund av tooa fel i hello samtidigt. Mer information om möjliga orsaker till förlust av data finns [vad hände toomy data i Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md)

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Kan jag startar om min cacheminne med hjälp av PowerShell, CLI eller andra hanteringsverktyg?
Ja, PowerShell instruktioner finns i [tooreboot Redis-cache](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-hello-reboot-functionality"></a>Priser för vilka nivåer kan använda hello omstart funktioner?
Starta om datorn är tillgänglig för alla prisnivåer.

## <a name="schedule-updates"></a>Schemauppdateringar
Hej **schemalägga uppdateringar** bladet kan du toodesignate en underhållsperiod för ditt cacheminne för Premium-nivån. När hello underhållsperiod har angetts görs alla uppdateringar för Redis-servern under den här perioden. 

> [!NOTE] 
> Hej underhållsperioden gäller endast uppdateringar för tooRedis server och inte tooany Azure uppdaterar eller uppdaterar toohello operativsystemet hello virtuella datorer som är värdar för hello cache.
> 
> 

![Schemauppdateringar](./media/cache-administration/redis-schedule-updates.png)

toospecify en underhållsperiod hello önskad dagar och ange hello fönstret starttid för underhåll för varje dag och klickar på **OK**. Observera att hello underhållsfönstertiden i UTC. 

> [!NOTE]
> Hej standardunderhållsfönstret för uppdateringar är fem timmar. Det här värdet kan inte konfigureras från hello Azure-portalen, men du kan konfigurera i PowerShell använder hello `MaintenanceWindow` parametern för hello [ny AzureRmRedisCacheScheduleEntry](/powershell/module/azurerm.rediscache/new-azurermrediscachescheduleentry) cmdlet. Mer information finns i [kan jag hantera schemalagda uppdateringar med hjälp av PowerShell, CLI eller andra hanteringsverktyg?](#can-i-manage-scheduled-updates-using-powershell-cli-or-other-management-tools)
> 
> 

## <a name="schedule-updates-faq"></a>Schemalägg uppdateringar vanliga frågor och svar
* [När uppdateringar, sker om jag inte använder hello schema-uppdateringar?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
* [Vilken typ av uppdateringar som görs under hello schemalagda underhållsperiod?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
* [Kan jag hantera schemalagda uppdateringar med hjälp av PowerShell, CLI eller andra hanteringsverktyg?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
* [Priser för vilka nivåer kan använda funktionen för hello schema-uppdateringar?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-hello-schedule-updates-feature"></a>När uppdateringar, sker om jag inte använder hello schema-uppdateringar?
Om du inte anger en underhållsperiod kan uppdateringar göras när som helst.

### <a name="what-type-of-updates-are-made-during-hello-scheduled-maintenance-window"></a>Vilken typ av uppdateringar som görs under hello schemalagda underhållsperiod?
Endast Redis-server uppdateringar görs under hello schemalagda underhållsperiod. Hej underhållsfönstret gäller inte tooAzure uppdateringar eller uppdaterar toohello VM-operativsystemet.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Kan jag hanterade schemalagda uppdateringar med hjälp av PowerShell, CLI eller andra hanteringsverktyg?
Ja, kan du hantera din schemalagda uppdateringar med hjälp av hello följande PowerShell-cmdlets:

* [Get-AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/get-azurermrediscachepatchschedule)
* [Ny AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/new-azurermrediscachepatchschedule)
* [Ny AzureRmRedisCacheScheduleEntry](/powershell/module/azurerm.rediscache/new-azurermrediscachescheduleentry)
* [Ta bort AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/remove-azurermrediscachepatchschedule)

### <a name="what-pricing-tiers-can-use-hello-schedule-updates-functionality"></a>Priser för vilka nivåer kan använda funktionen för hello schema-uppdateringar?
Hej **schemalägga uppdateringar** funktionen är endast tillgänglig i hello premium-prisnivån.

## <a name="next-steps"></a>Nästa steg
* Utforska mer [Azure Redis-Cache premium-nivån](cache-premium-tier-intro.md) funktioner.

