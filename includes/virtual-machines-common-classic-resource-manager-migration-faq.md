# <a name="frequently-asked-questions-about-classic-tooazure-resource-manager-migration"></a>Vanliga frågor om klassiska tooAzure hanteraren för filserverresurser

## <a name="does-this-migration-plan-affect-any-of-my-existing-services-or-applications-that-run-on-azure-virtual-machines"></a>Påverkar den här migreringsplanen några befintliga tjänster eller program som körs på virtuella Azure-datorer? 

Nej. hello är (klassiskt) fullt stöd tjänster i allmän tillgänglighet. Du kan fortsätta toouse dessa resurser tooexpand din storleken på Microsoft Azure.

## <a name="what-happens-toomy-vms-if-i-dont-plan-on-migrating-in-hello-near-future"></a>Vad händer toomy virtuella datorer om jag inte planerar att migrera i hello nära framtiden? 

Vi inte sluta hello befintliga klassiska API: er och resursen modellen. Vi vill toomake migrering enkelt beaktar hello avancerade funktioner som är tillgängliga i hello Resource Manager-distributionsmodellen. Vi rekommenderar starkt att du läser igenom [några av förbättringarna hello](../articles/azure-resource-manager/resource-manager-deployment-model.md) som ingår i IaaS under Resource Manager.

## <a name="what-does-this-migration-plan-mean-for-my-existing-tooling"></a>Vad innebär den här migreringsplanen för mina befintliga verktyg? 

Uppdatera din verktygsuppsättning toohello Resource Manager-distributionsmodellen är en av hello viktigaste ändringarna som du har tooaccount för i din planer.

## <a name="how-long-will-hello-management-plane-downtime-be"></a>Hur länge hello management-plan driftstopp är? 

Det beror på hello antal resurser som migreras. För mindre distributioner (några flera virtuella datorer), bör hello hela migreringen ta mindre än en timme. För storskaliga distributioner (hundratals VMs) kan hello migreringen ta några timmar.

## <a name="can-i-roll-back-after-my-migrating-resources-are-committed-in-resource-manager"></a>Kan jag återställa när mina migreringsresurser har checkats in till Resource Manager? 

Du kan avbryta din migrering så länge hello resurser finns i hello förberedd tillstånd. Återställning stöds inte när hello resurser har migrerats korrekt genom hello commit-åtgärden.

## <a name="can-i-roll-back-my-migration-if-hello-commit-operation-fails"></a>Kan jag återställer min migrering om hello commit-åtgärden misslyckas? 

Du kan inte avbryta migrering om hello commit-åtgärden misslyckas. Alla migreringsåtgärder, inklusive hello commit-åtgärden är idempotent. Därför rekommenderar vi att du hello försök åtgärden igen efter en kort tid. Om du fortfarande står inför ett fel, skapa ett supportärende eller skapa ett foruminlägg med hello ClassicIaaSMigration tagg på vår [VM forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).

## <a name="do-i-have-toobuy-another-express-route-circuit-if-i-have-toouse-iaas-under-resource-manager"></a>Måste jag toobuy en annan express route-kretsen om jag har toouse IaaS under Resource Manager? 

Nej. Vi nyligen aktiverat [flyttar ExpressRoute-kretsar från hello klassiska toohello Resource Manager-distributionsmodellen](../articles/expressroute/expressroute-move.md). Du har inte toobuy en ny ExpressRoute-krets om du redan har en.

## <a name="what-if-i-had-configured-role-based-access-control-policies-for-my-classic-iaas-resources"></a>Vad händer om jag har konfigurerat principer för rollbaserad åtkomstkontroll för mina klassiska IaaS-resurser? 

Under migreringen transformera hello resurser från klassiska tooResource Manager. Därför rekommenderar vi att du planerar hello RBAC principuppdateringar som kräver toohappen efter migreringen.

## <a name="i-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-tooresource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Jag har säkerhetskopierat mina klassiska virtuella datorer i ett säkerhetskopieringsvalv. Kan jag migrera Mina virtuella datorer från klassiskt läge tooResource Manager läge och skydda dem i ett Recovery Services-valv? 

Klassiska Virtuella återställningspunkter i ett säkerhetskopieringsvalv migrerar inte tooa Recovery Services-valvet automatiskt när du flyttar hello VM från klassiska tooResource Manager-läget. Följ dessa steg tootransfer VM-säkerhetskopieringar:

1. Hello Backup-valvet, gå toohello **skyddade objekt** fliken och markera hello VM. Klicka på [Stoppa skydd](../articles/backup/backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Lämna alternativet *Ta bort associerade säkerhetskopieringsdata* **avmarkerat**.
2. Ta bort hello säkerhetskopiering/ögonblicksbild tillägg från hello VM.
3. Migrera hello virtuell dator från klassiskt läge tooResource Manager läge. Kontrollera att hello lagring och nätverk information motsvarande toohello virtuell dator är också migreras tooResource Manager-läget.
4. Skapa ett Recovery Services-valvet och konfigurera säkerhetskopiering på hello migrerade virtuella datorn med hjälp av **säkerhetskopiering** åtgärd ovanpå valvet instrumentpanelen. Detaljerad information om säkerhetskopiering av en VM tooa Recovery Services valvet, finns hello artikeln [skydda virtuella Azure-datorer med ett Recovery Services-valv](../articles/backup/backup-azure-vms-first-look-arm.md).

## <a name="can-i-validate-my-subscription-or-resources-toosee-if-theyre-capable-of-migration"></a>Kan jag verifiera min prenumeration eller resurser toosee om de är kompatibla av migrering? 

Ja. I hello stöds av plattformen alternativet är hello första steget när du förbereder för migrering toovalidate hello resurser klarar av migreringen. Om hello Validera åtgärden misslyckas, kan du få meddelanden för alla hello orsaker hello migreringen inte kan slutföras.

## <a name="what-happens-if-i-run-into-a-quota-error-while-preparing-hello-iaas-resources-for-migration"></a>Vad händer om jag en kvot problem när du förberedde migreringen hello IaaS-resurser? 

Vi rekommenderar att du avbryta migreringen och sedan loggar en stöd begäran tooincrease hello kvoter i hello region där du har hello migrera virtuella datorer. När hello kvoten begäran har godkänts, kan du börja köra hello migreringssteg igen.

## <a name="how-do-i-report-an-issue"></a>Hur gör jag för att rapportera ett problem? 

Publicera din problem och frågor om migrering tooour [VM forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows), med hello nyckelordet ClassicIaaSMigration. Vi rekommenderar att du lägger upp alla dina frågor på det här forumet. Om du har ett supportavtal, är du Välkommen toolog ett supportärende.

## <a name="what-if-i-dont-like-hello-names-of-hello-resources-that-hello-platform-chose-during-migration"></a>Vad händer om jag inte gillar hello resursnamnen hello som hello plattform valde under migreringen? 

Alla hello-resurser som du uttryckligen ange namn för i hello klassiska distributionsmodellen bevaras under migreringen. I vissa fall skapas nya resurser. Exempelvis skapas ett nätverksgränssnitt för varje virtuell dator. För närvarande stöds inte hello möjlighet toocontrol hello namnen på dessa nya resurser som skapades under migreringen. Logga din rösterna för den här funktionen på hello [Azure Feedbackforum](http://feedback.azure.com).

## <a name="can-i-migrate-expressroute-circuits-used-across-subscriptions-with-authorization-links"></a>Kan jag migrera ExpressRoute-kretsar som används över prenumerationer med auktoriseringslänkar? 

ExpressRoute-kretsar som använder auktoriseringslänkar över flera prenumerationer migreras inte automatiskt utan driftavbrott. Det finns information om hur de kan migreras manuellt. Se [migrera ExpressRoute-kretsar och associerade virtuella nätverk från hello klassiska toohello Resource Manager-distributionsmodellen](../articles/expressroute/expressroute-migration-classic-resource-manager.md) steg och mer information.

## <a name="i-got-a-message-vm-is-reporting-hello-overall-agent-status-as-not-ready-hence-hello-vm-cannot-be-migrated-ensure-that-hello-vm-agent-is-reporting-overall-agent-status-as-ready-or-vm-contains-extension-whose-status-is-not-being-reported-from-hello-vm-hence-this-vm-cannot-be-migrated-"></a>Jag fick ett meddelande *”VM rapporterar hello övergripande status för agent som inte är redo. Därför kan inte migreras hello VM. Se till att hello VM-agenten rapporterar övergripande status för agent som klar ”* eller *” VM innehåller tillägg vars Status inte rapporteras från hello VM. Den virtuella datorn kan därför inte migreras.” *

Det här meddelandet tas emot när hello VM saknar utgående anslutning toohello internet. hello VM-agenten använder utgående anslutning tooreach hello Azure storage-konto för att uppdatera agentstatusen hello var femte minut.
