pipeline {
  agent {
    node {
      label 'DTEKAPPANSIBLET02'
    }

  }
  stages {
    stage('Test') {
      steps {
        echo 'Hello'
        pwsh 'cls;  $items = Foreach ($Site in Get-Website) {        $resolvedLogPath = $Site.logFile.directory.Replace(\'%SystemDrive%\', \'C:\\\')     $resolvedLogPath = $resolvedLogPath + \'\\W3SVC\' + $Site.id     $logObj = (Get-ChildItem -Path $resolvedLogPath -File | Sort-Object -Property LastWriteTime -Descending | Select-Object -First 1)      New-Object psobject -Property @{         SiteId = $Site.id         Name = $Site.name         LogDirectory = $resolvedLogPath         LastLogFile =  ($logObj | Select Name).Name         LastLogTime =  ($logObj | Select LastWriteTime).LastWriteTime         LastLogLength = (($logObj| Select Length).Length / 1024).ToString(\'F2\') + \' KB\'         HasTrafficInLastMonth = ($logObj| Where-Object  { $_.LastWriteTime -gt (Get-Date).AddDays(-30).Date } | Select  Name).Name.Length -gt 0        } }  $items | Sort-Object -Property LastLogTime -Descending |  Format-Table Name, SiteId, LogDirectory, LastLogFile, LastLogTime, LastLogLength, HasTrafficInLastMonth   $exportPath=\'C:\\\' + $env:COMPUTERNAME + \'.csv\' $exportPath  $items | Sort-Object -Property LastLogTime -Descending |   Export-Csv -Path $exportPath -NoTypeInformation'
      }
    }

  }
}