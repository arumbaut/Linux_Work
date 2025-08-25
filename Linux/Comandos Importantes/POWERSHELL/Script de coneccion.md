```
  

#Conectar a un destino por el nombre de la maquina

function connect {

    param($destino)

  

    if (-not $destino) {

        Write-Host "Uso: conect <nombre_maquina>"

        return

    }

    $usuarioGateway = $env:GATEWAY_USER

    $gateway = $env:GATEWAY_IP

    $usuarioDestino = $env:GATEWAY_USER

    $rsa = $env:RSA_KEY_PATH

    $sftp1= $env:sftp1

    $sftp2= $env:sftp2

    #Write-Host "Prueba de alias $sftp1 "

   switch ($destino) {

    "ftp1" { $destino = $env:sftp1 }

    "ftp2" { $destino = $env:sftp2 }

    }

  

    if ($destino -eq $sftp1 -or $destino -eq $sftp2 -or $destino -eq "ftp" -or $destino -eq "prcs30pr" -or $destino -eq "SPMG10YR" -or $destino -eq "OSSJ10DR" -or $destino -eq "OSSJ10IR" -or $destino -eq "OSSJ10RR")  {

        ssh -i "C:\Users\Adrian Alonso\.ssh\id_rsa" -o "KexAlgorithms=+diffie-hellman-group14-sha1" -o "HostKeyAlgorithms=+ssh-rsa" -o "PubkeyAcceptedAlgorithms=+ssh-rsa" -o "MACs=+hmac-sha1" -J "$usuarioGateway@$gateway" "$usuarioDestino@$destino"

  

    }        

  

    ssh -i "$rsa" -J "$usuarioGateway@$gateway" "$usuarioDestino@$destino"

  

}

  

#Conectar a la pc de salto

function adm01 {

  #  Write-Host "Prueba de alias $Destino "

    $usuarioGateway = $env:GATEWAY_USER

    $gateway = $env:GATEWAY_IP

    ssh "$usuarioGateway@$gateway"

}
```


Versio copilot
```
function Connect {
    param([string]$Destino)

    if (-not $Destino) {
        Write-Host "Uso: Connect <nombre_maquina>"
        return
    }

    $usuarioGateway   = $env:GATEWAY_USER
    $gateway          = $env:GATEWAY_IP
    $usuarioDestino   = $env:GATEWAY_USER
    $rsa              = $env:RSA_KEY_PATH
    $sftp1            = $env:sftp1
    $sftp2            = $env:sftp2

    # Alias para destinos
    $aliasMap = @{
        "ftp1" = $sftp1
        "ftp2" = $sftp2
    }

    if ($aliasMap.ContainsKey($Destino)) {
        $Destino = $aliasMap[$Destino]
    }

    $destinosEspeciales = @("ftp", "prcs30pr", "SPMG10YR", "OSSJ10DR", "OSSJ10IR", "OSSJ10RR", $sftp1, $sftp2)

    $sshOptions = @()
    if ($destinosEspeciales -contains $Destino) {
        $sshOptions += "-i 'C:\Users\Adrian Alonso\.ssh\id_rsa'"
        $sshOptions += "-o 'KexAlgorithms=+diffie-hellman-group14-sha1'"
        $sshOptions += "-o 'HostKeyAlgorithms=+ssh-rsa'"
        $sshOptions += "-o 'PubkeyAcceptedAlgorithms=+ssh-rsa'"
        $sshOptions += "-o 'MACs=+hmac-sha1'"
    } else {
        $sshOptions += "-i '$rsa'"
    }

    $sshOptions += "-J '$usuarioGateway@$gateway'"
    $sshCommand = "ssh $($sshOptions -join ' ') '$usuarioDestino@$Destino'"
    Invoke-Expression $sshCommand
}

function Adm01 {
    $usuarioGateway = $env:GATEWAY_USER
    $gateway        = $env:GATEWAY_IP

    ssh "$usuarioGateway@$gateway"
}

```

Version GPT
```
# Función para conectarse a una máquina destino por su nombre o alias
function Connect {
    param (
        [string]$Destino
    )

    if (-not $Destino) {
        Write-Host "Uso: connect <nombre_maquina>"
        return
    }

    # Variables de entorno
    $usuarioGateway  = $env:GATEWAY_USER
    $gateway         = $env:GATEWAY_IP
    $usuarioDestino  = $env:GATEWAY_USER
    $rsa             = $env:RSA_KEY_PATH
    $sftp1           = $env:sftp1
    $sftp2           = $env:sftp2

    # Resolver alias
    switch ($Destino.ToLower()) {
        "ftp1" { $Destino = $sftp1 }
        "ftp2" { $Destino = $sftp2 }
    }

    # Lista blanca de destinos que requieren opciones especiales
    $destinosEspeciales = @(
        $sftp1, $sftp2, "ftp", "prcs30pr", "spmg10yr", "ossj10dr", "ossj10ir", "ossj10rr"
    )

    $sshBase = "ssh -i `"$rsa`" -J `"$usuarioGateway@$gateway`" `"$usuarioDestino@$Destino`""

    if ($destinosEspeciales -contains $Destino.ToLower()) {
        # Agregar opciones especiales de compatibilidad SSH
        $sshCommand = "$sshBase -o `"KexAlgorithms=+diffie-hellman-group14-sha1`" -o `"HostKeyAlgorithms=+ssh-rsa`" -o `"PubkeyAcceptedAlgorithms=+ssh-rsa`" -o `"MACs=+hmac-sha1`""
    } else {
        $sshCommand = $sshBase
    }

    # Ejecutar el comando SSH
    Invoke-Expression $sshCommand
}

# Función para conectarse directamente a la máquina de salto (gateway)
function Adm01 {
    $usuarioGateway = $env:GATEWAY_USER
    $gateway        = $env:GATEWAY_IP

    ssh "$usuarioGateway@$gateway"
}

```