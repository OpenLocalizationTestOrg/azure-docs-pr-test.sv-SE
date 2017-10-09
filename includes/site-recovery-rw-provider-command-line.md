UnifiedSetup.exe [/ ServerMode < CS/PS >] [/ InstallDrive <DriveLetter>] [/ MySQLCredsFilePath <MySQL credentials file path>] [/ VaultCredsFilePath <Vault credentials file path>] [/ EnvType < VMWare/NonVMWare >] [/ PSIP < IP-adress toobe används för att överföra data] [/CSIP <IP address of CS toobe registered with>] [/ PassphraseFilePath <Passphrase file path>]

Parametrar:

* / ServerMode: Obligatorisk. Anger om båda hello konfiguration och processen servrarna ska installeras eller hello processen server. Indatavärden: CS, PS.
* InstallLocation: Obligatorisk. hello mapp vilka hello komponenter installeras.
* /MySQLCredsFilePath. Obligatorisk. hello filsökvägen vilka hello MySQL serverautentiseringsuppgifter lagras. hello-filen ska vara i formatet:
* [MySQLCredentials]
* MySQLRootPassword = "<Password>"
* MySQLUserPassword = "<Password>"
* /VaultCredsFilePath. Obligatorisk. hello platsen för hello valvautentiseringsfilen
* /EnvType. Obligatorisk. hello typ av installation. Värden: VMware, NonVMware
* /PSIP och /CSIP. Obligatorisk. hello IP-adress hello processervern och konfigurationsservern.
* /PassphraseFilePath. Obligatorisk. hello sökväg till hello lösenfras.
* /BypassProxy. Valfri. Anger att hello konfigurationsservern ansluter tooAzure utan en proxy.
* /ProxySettingsFilePath. Valfri. Proxy-inställningar (hello standardproxy kräver autentisering, eller en anpassad proxy). hello-filen ska vara i formatet:
* [ProxySettings]
* ProxyAuthentication = "Ja/Nej"
* Proxy IP = "IP-adress >"
* ProxyPort = "<Port>"
* ProxyUserName="<User Name>"
* ProxyPassword="<Password>"
* DataTransferSecurePort. Valfri. hello portnummer för replikeringsdata.
* SkipSpaceCheck. Valfri. Hoppa över utrymmeskontroll för cache.
* AcceptThirdpartyEULA. Obligatorisk. Accepterar hello tredjeparts licensavtal.
* ShowThirdpartyEULA. Obligatorisk. Visar licensavtalet (EULA) från tredje part. Om detta anges som indata ignoreras alla andra parametrar.
