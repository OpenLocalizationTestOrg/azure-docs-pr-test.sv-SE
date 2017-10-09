Om alla servrar på hello klustret kör Windows Server 2008 R2 eller Windows Server 2012, du måste kontrollera att snabbkorrigeringen hello [KB2854082](http://support.microsoft.com/kb/2854082) installeras på varje hello lokala servrar eller virtuella Azure-datorer som ingår i hello kluster. En server eller virtuell dator som är i hello kluster, men inte i hello tillgänglighetsgruppen bör ha den här snabbkorrigeringen.

Hämtar i hello värdserver för fjärrskrivbordssession för varje hello klusternoder, [KB2854082](http://support.microsoft.com/kb/2854082) tooa lokal katalog. Installera sedan hello snabbkorrigeringen på varje klusternod sekventiellt. Om hello klustertjänsten körs på hello klusternod, startas hello server hello slutet av installationen av hello snabbkorrigeringen.

> [!WARNING]
> Stoppa klustertjänsten hello eller hello-servern startas om påverkar hello kvorum hälsotillståndet för tillgänglighetsgruppen klustret och hello och kan leda till att ditt kluster toogo offline. toomaintain hello hög tillgänglighet på klustret under installationen, se till att:
> 
> * hello klustret är i optimala kvorum hälsa. 
> * Innan du installerar snabbkorrigeringen hello på någon nod, är alla noder i klustret online.
> * Innan du installerar snabbkorrigeringen hello på en annan nod i klustret hello Tillåt hello snabbkorrigeringen installation toorun toocompletion på en nod, inklusive fullständigt hello-servern startas om.
> 
> 

