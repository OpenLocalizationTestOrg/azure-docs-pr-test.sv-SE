hello följande begränsningar gäller tooAzure säkerhetskopiering.

| Gränsen identifierare | Standardgräns |
| --- | --- |
| Många servrar/datorer som kan registreras mot varje valvet |50 för Windows Server/Client/SCDPM <br/> 200 för IaaS-VM |
| Storleken på en datakälla för data som lagras i Azure valvet lagring |54400 GB max<sup>1</sup> |
| Antal säkerhetskopieringsvalv som kan skapas i varje Azure-prenumeration |25 (säkerhetskopieringsvalv) <br/> 25 recovery Services-valvet per region |
| Antal gånger som säkerhetskopiering kan schemaläggas per dag |3 per dag för Windows Server eller-klienten <br/> 2 per dag för SCDPM <br/> En gång om dagen för IaaS-VM |
| Datadiskar kopplade tooan virtuella Azure-datorn för säkerhetskopiering |16 |

* <sup>1</sup>hello 54400 GB begränsningen gäller inte tooIaaS säkerhetskopiering.

