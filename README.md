# Powershell Mouse Jiggle Script
\n
New-Item -ItemType File -Path $PROFILE -Force \n
notepad $PROFILE \n
\n
""" 
function Start-Jig {
    Add-Type -AssemblyName System.Windows.Forms
    $idleTime = [TimeSpan]::FromMinutes(1)
    $lastUserMove = Get-Date
    $lastCursorPos = [System.Windows.Forms.Cursor]::Position
    $lastJigPos = $null
    while ($true) {
        $currentPos = [System.Windows.Forms.Cursor]::Position
        $userMoved = $false
        if ($lastJigPos -and $currentPos.Equals($lastJigPos)) {
            # Mouse is at the jiggle position — ignore
            $userMoved = $false
        } elseif (-not $currentPos.Equals($lastCursorPos)) {
            # Real user movement
            $userMoved = $true
            $lastUserMove = Get-Date
        }
        $lastCursorPos = $currentPos
        if ((Get-Date) - $lastUserMove -ge $idleTime) {
            # Perform jiggle
            $x = Get-Random -Minimum 0 -Maximum 100
            $y = Get-Random -Minimum 0 -Maximum 100
            $jigPoint = New-Object System.Drawing.Point($x, $y)
            [System.Windows.Forms.Cursor]::Position = $jigPoint
            $lastJigPos = $jigPoint
        }
        Start-Sleep -Seconds 5
    }
}
Set-Alias jig Start-Jig
 """

. $PROFIL
