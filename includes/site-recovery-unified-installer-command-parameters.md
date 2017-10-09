|Parameternamn| Typ | Beskrivning| Möjliga värden|
|-|-|-|-|
| /ServerMode|Obligatorisk|Anger om båda hello konfiguration och processen servrarna ska installeras eller hello processen server|CS<br>PS|
|/InstallLocation|Obligatorisk|hello-mappen i vilka hello komponenter är installerade| En mapp på hello-dator|
|/MySQLCredsFilePath|Obligatorisk|hello filsökväg vilka hello MySQL serverautentiseringsuppgifter lagras|hello-filen ska vara hello-format som anges nedan|
|/VaultCredsFilePath|Obligatorisk|Hej valvautentiseringsfilen hello sökväg|Giltig sökväg|
|/EnvType|Obligatorisk|Typ av envrionment som du vill tooprotect |VMware<br>NonVMware|
|/PSIP|Obligatorisk|IP-adressen för hello NIC toobe används för att överföra data för replikering| Vilken giltig IP-adress som helst|
|/CSIP|Obligatorisk|hello hello NIC på vilka hello konfigurationsservern lyssnar på IP-adress| Vilken giltig IP-adress som helst|
|/PassphraseFilePath|Obligatorisk|hello fullständig sökväg toolocation hello lösenfras filen|Giltig sökväg|
|/BypassProxy|Valfri|Anger att hello konfigurationsservern ansluter tooAzure utan en proxy|toodo hämta värdet från Venu|
|/ProxySettingsFilePath|Valfri|Proxy-inställningar (hello standardproxy kräver autentisering, eller en anpassad proxy)|hello-filen ska ha formatet hello anges nedan|
|DataTransferSecurePort|Valfri|Portnumret på hello PSIP toobe används för data för replikering| Giltigt portnummer (standardvärdet är 9433)|
|/SkipSpaceCheck|Valfri|Hoppa över utrymmeskontroll för cachedisk| |
|/AcceptThirdpartyEULA|Obligatorisk|När du flaggar innebär det att du godkänner licensavtalet från tredje part| |
|/ShowThirdpartyEULA|Valfri|Visar licensavtalet (EULA) från tredje part. Om detta anges som indata ignoreras alla andra parametrar| |
