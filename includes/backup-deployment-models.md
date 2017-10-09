hello Azure Backup-tjänsten har två typer av valv - hello Backup-valvet och hello Recovery Services-valvet. först kom hello Backup-valvet. Hello Recovery Services-valvet Kom sedan längs toosupport hello expanderas Resource Manager distributioner. Microsoft rekommenderar att använda Resource Manager-distributioner, såvida du inte måste ha en klassisk distribution.

| **Distribution** | **Portal** | **Valv** |
| --- | --- | --- |
| Klassisk |[Klassisk](https://manage.windowsazure.com) |Säkerhetskopiering |
| Resource Manager |[Azure](https://portal.azure.com) |Recovery Services |

> [!NOTE]
> Backup-valvet kan inte skydda Resource Manager-distribuerade lösningar. Du kan dock använda en återställningstjänster valvet tooprotect classically distribuerade servrar och virtuella datorer.  
> 
> 

