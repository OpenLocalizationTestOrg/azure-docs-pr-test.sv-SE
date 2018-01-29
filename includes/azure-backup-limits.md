
Följande begränsningar gäller för Azure Backup.

| Gränsen identifierare | Standardgräns |
| --- | --- |
| Många servrar/datorer som kan registreras mot varje valvet |50 för Windows Server/Client/SCDPM <br/> 200 för IaaS-VM |
| Storleken på en datakälla för data som lagras i Azure valvet lagring |54400 GB max<sup>1</sup> |
| Antal säkerhetskopieringsvalv som kan skapas i varje Azure-prenumeration |25 recovery Services-valv per region |
| Antal gånger som säkerhetskopiering kan schemaläggas per dag |3 per dag för Windows Server eller-klienten <br/> 2 per dag för SCDPM <br/> En gång om dagen för IaaS-VM |
| Datadiskar som är anslutna till en virtuell Azure-dator för säkerhetskopiering |16 |
| Storleken på enskilda disk ansluten till en virtuell Azure-dator för säkerhetskopiering| 1 023 GB <sup>2</sup>|

* <sup>1</sup>54400 GB gränsen gäller inte för IaaS VM-säkerhetskopiering.
* <sup>2</sup> vi har en [privat förhandsversion](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a?redir=0) för att stödja ohanterade diskar upp till 4 TB. 

