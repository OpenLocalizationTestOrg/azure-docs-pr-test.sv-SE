Du kan kontrollera att anslutningen lyckades genom att använda hello 'Get-AzureRmVirtualNetworkGatewayConnection-cmdlet med eller utan '-Debug ”. 

1. Använd hello följande cmdlet-exemplet, konfigurerar hello värden toomatch egna. Om du uppmanas, väljer du ”A” i ordning toorun 'All'. I exemplet hello ”-namnet ' refererar toohello namnet på hello-anslutning som du vill tootest.

  ```powershell
  Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG
  ```
2. Visa hello värden när hello cmdleten har slutförts. I hello exemplet nedan visar hello anslutningens status som ”ansluten” och du kan se ingående och utgående byte.
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```