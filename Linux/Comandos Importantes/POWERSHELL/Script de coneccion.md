
Ultima Actualizacion
```
  
  

$Script:destinosEspeciales = @("prcs30pr", "SPMG10YR", "OSSJ10DR", "OSSJ10IR", "OSSJ10RR", "OSSJ10RR",

                            "WMIR40RR","WSRE01IR", "WSRE01RR","AOTLXPRGDA00001","AOTLXPRGDA00002",

                            "AOTLXPPGDA00001","WAPC61RR","WAPC20RR","PEAB20RR", "POMV10YR","excc20pr",

                            "excc20br","FACE30DR","NAGI10PR","AOTLXPPVOK00001","WILY93PR","WILY94PR",

                            "aotlxprwlp00007","AOTLXPRWLG00014","CCCB30DR","CCWL10DR","AVAY20MR","CCCM31DR",

                            "FACE30PR","SLG1PRM2","AOTLXDEMIT00001","AOTLXDEMIT00002","AOTLXTEMIT00001",

                            "dwbi30pr","aotlxppmio00002","AOTLXTEWLP00001","AOTLXPRTAF00001","AOTLXPRTAF00002",

                            "AOTLXPRTAF00005","AOTLXPRTAF00006","aotlxtewlp00002","AOTLXTETAF00004","AOTLXTETAF00001",

                            "AOTLXPRWLP00002","AOTLXPRWLP00003","AOTLXPRWLP00004","AOTLXPRWLP00005","AOTLXPRWLP00006",

                            "AOTLXPRWLP00007","AOTLXPRWLP00008","aotlxprpor00001","aotlxprpor00002","AOTLXTEWLG00006",

                            "AOTLXPRWCS00015","AOTLXPRWCS00014","AOTLXPRBAM00001","AOTLXPRCCA00001","AOTLXPRCCA00002",

                            "dwhdes03",$env:sftp1, $env:sftp2)

  

function Connect_Old {

    param([string]$Destino)

  

    if (-not $Destino) {

        Write-Host "Uso: Connect <nombre_maquina>"

        return

    }

  

    $usuarioGateway   = $env:GATEWAY_USER

    $gateway          = $env:GATEWAY_IP

    $usuarioDestino   = $env:GATEWAY_USER

    $rsa              = $env:RSA_KEY_PATH

    $sftp1            = $env:sftp1

    $sftp2            = $env:sftp2

  

    # Alias para destinos

    $aliasMap = @{

        "ftp1" = $sftp1

        "ftp2" = $sftp2

    }

  

    if ($aliasMap.ContainsKey($Destino)) {

        $Destino = $aliasMap[$Destino]

    }

  

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

  

#Connect "AOTLXPRWLG00001"

function Connect {

     param([string]$Destino)

  

    if (-not $Destino) {

        Write-Host "Uso: Connect <nombre_maquina>"

        return

    }

  

    $usuarioGateway   = $env:GATEWAY_USER

    $gateway          = $env:GATEWAY_IP

    $usuarioDestino   = $env:GATEWAY_USER

    $rsa              = $env:RSA_KEY_PATH

    $sftp1            = $env:sftp1

    $sftp2            = $env:sftp2

  

    # Alias para destinos

    $aliasMap = @{

        "ftp1" = $sftp1

        "ftp2" = $sftp2

    }

  

     if ($aliasMap.ContainsKey($Destino)) {

        $Destino = $aliasMap[$Destino]

    }

  

    $sshOptions = @()

    $sshOptions += "-i '$rsa'"

    $sshOptions += "-J '$usuarioGateway@$gateway'"

    $sshCommand = "ssh $($sshOptions -join ' ') '$usuarioDestino@$Destino'"

    Invoke-Expression $sshCommand

}

  
  
  
  

function Connect1 {

     param([string]$Destino)

  

    if (-not $Destino) {

        Write-Host "Uso: Connect <nombre_maquina>"

        return

    }

  

    $usuarioGateway   = $env:GATEWAY_USER

    $gateway          = $env:GATEWAY_IP

    $usuarioDestino   = $env:GATEWAY_USER

   # $rsa              = $env:RSA_KEY_PATH

    $sftp1            = $env:sftp1

    $sftp2            = $env:sftp2

  

    # Alias para destinos

    $aliasMap = @{

        "ftp1" = $sftp1

        "ftp2" = $sftp2

    }

  

     if ($aliasMap.ContainsKey($Destino)) {

        $Destino = $aliasMap[$Destino]

    }

  

     $sshOptions += "-i 'C:\Users\Adrian Alonso\.ssh\id_rsa'"

     $sshOptions += "-o 'KexAlgorithms=+diffie-hellman-group14-sha1'"

     $sshOptions += "-o 'HostKeyAlgorithms=+ssh-rsa'"

     $sshOptions += "-o 'PubkeyAcceptedAlgorithms=+ssh-rsa'"

     $sshOptions += "-o 'MACs=+hmac-sha1'"

  

     $sshOptions += "-J '$usuarioGateway@$gateway'"

     $sshCommand = "ssh $($sshOptions -join ' ') '$usuarioDestino@$Destino'"

     Invoke-Expression $sshCommand

}

  
  

function Adm01 {

    $usuarioGateway = $env:GATEWAY_USER

    $gateway        = $env:GATEWAY_IP

  

    ssh "$usuarioGateway@$gateway"

}

  

function Adm03 {

    $usuarioGateway = $env:GATEWAY_USER

    $gateway        = $env:ADM03

  

    ssh "$usuarioGateway@$gateway"

}

  

function Terep {

    $usuarioGateway = $env:GATEWAY_USER

    $gateway        = $env:TEREP

  

    ssh "$usuarioGateway@$gateway"

}

# Función para levantar el túnel SSH y devolver el PID

function Start-SshTunnel {

    param (

        [string]$ConfigPath = "C:\Users\Adrian Alonso\.ssh\config.atlas",

        [int]$LocalPort = 2080,

        [string]$HostAlias = "atlas"

    )

  

    # Levanta SSH en segundo plano y captura el proceso

    $proc = Start-Process ssh -ArgumentList "-F `"$ConfigPath`" -D $LocalPort $HostAlias -Nqf" -PassThru

  

    Write-Host "Túnel SSH levantado (PID $($proc.Id))"

    return $proc.Id

}

  

# Función para cerrar el túnel usando el PID

function Stop-SshTunnel {

    param (

        [int]$P_id

    )

  

    if (Get-Process -Id $P_id -ErrorAction SilentlyContinue) {

        Stop-Process -Id $P_id

        Write-Host "Túnel SSH cerrado (PID $Pid)"

    } else {

        Write-Host "No se encontró proceso con PID $Pid"

    }

}

  
  

function ValParcheo {

    param([string]$Destino)

  

    if (-not $Destino) {

        Write-Host "Uso: ValParcheo <nombre_maquina>"

        return

    }

  

    $usuarioGateway   = $env:GATEWAY_USER

    $gateway          = $env:GATEWAY_IP

    $usuarioDestino   = $env:GATEWAY_USER

    $rsa              = $env:RSA_KEY_PATH

    $sftp1            = $env:sftp1

    $sftp2            = $env:sftp2

    # Comando remoto

    $query = "sudo crqcheck --verbose; cat /etc/selinux/config | grep SELINUX= | grep -v '# SELINUX'; df -hPT /boot; df -hPT /usr; df -hPT /var; ip a; route -n; cat /etc/redhat-release; uname -a; sudo cat /etc/shadow | grep root | cut -d ':' -f1-2"

  

    # Alias para destinos

    $aliasMap = @{

        "ftp1" = $sftp1

        "ftp2" = $sftp2

    }

  

    if ($aliasMap.ContainsKey($Destino)) {

        $Destino = $aliasMap[$Destino]

    }

  

    # Opciones SSH

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

  

    # Construir la línea SSH completa

    $sshCommand = "ssh $($sshOptions -join ' ') $usuarioDestino@$Destino `"$query`""

  

    # Ejecutar

    Invoke-Expression $sshCommand

}

  

#Maquias de Yecora

function ConnectYecora {

    param([string]$Destino)

  

    if (-not $Destino) {

        Write-Host "Uso: ConnectYecora <nombre_maquina>"

        return

    }

  

    # Comando remoto

    $query = "sudo su - -c `"whoami`""

  
  

    $usuarioGateway = $env:GATEWAY_USER

    $gateway        = $env:GATEWAY_IP

  
  

    # Construir la línea SSH completa

    $sshCommand = "ssh $usuarioGateway@$gateway `"$query`""

  
  

    # Ejecutar

    Invoke-Expression $sshCommand

}
```


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