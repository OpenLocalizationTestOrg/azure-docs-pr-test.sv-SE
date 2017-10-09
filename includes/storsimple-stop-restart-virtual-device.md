#### <a name="toostop-and-start-a-virtual-device"></a>toostop och starta en virtuell enhet
toostop en virtuell enhet klickar du på namnet och klicka sedan på **avstängning**. Medan hello virtuella enheten stängs av, är dess status **stoppar**. När hello virtuella enheten har avbrutits dess status är **stoppad**.

Använd följande cmdlet: ar toostop hello och starta en virtuell enhet.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-virtual-device"></a>toorestart en virtuell enhet
När en virtuell enhet är igång och du vill toorestart, klickar du på namnet och klickar sedan på **starta om**. Hello virtuella enheten startas om är dess status är **omstart**. När hello virtuell enhet är redo för du toouse, är dess status **kör**.

Använd hello följande cmdlet toorestart en virtuell enhet.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

