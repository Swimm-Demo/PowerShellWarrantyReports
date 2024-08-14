---
title: DRMM Warranty Overview
---
DRMM Warranty refers to the process of retrieving and updating warranty information for devices managed by Datto RMM (Remote Monitoring and Management) systems.

The function <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="1:2:4" line-data="function  Get-WarrantyDattoRMM {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyDattoRMM`</SwmToken> is responsible for fetching warranty details for all devices registered in the Datto RMM system.

It first checks if the <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="13:5:5" line-data="        Import-module DattoRMM" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`DattoRMM`</SwmToken> module is available and imports it; if not, it installs and then imports the module.

API parameters such as URL, API key, and secret key are set using the <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="28:1:3" line-data="    Set-DrmmApiParameters @params" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Set-DrmmApiParameters`</SwmToken> function.

The function retrieves all devices from the Datto RMM account and processes each device to obtain its serial number.

The serial number is then used to fetch warranty information using the <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="58:6:8" line-data="        $WarState = Get-Warrantyinfo -DeviceSerial $DeviceSerial -client $device.siteName" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-Warrantyinfo`</SwmToken> function.

If the <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="60:5:5" line-data="        if ($SyncWithSource -eq $true) {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`SyncWithSource`</SwmToken> parameter is true, the warranty information is updated in the Datto RMM system.

The function also handles resuming from the last processed device if a previous run was interrupted, by checking for a <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="33:11:13" line-data="        $AllDevices = get-content &#39;Devices.json&#39; | convertfrom-json" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Devices.json`</SwmToken> file.

Finally, the function returns the collected warranty information and removes the <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="33:11:13" line-data="        $AllDevices = get-content &#39;Devices.json&#39; | convertfrom-json" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Devices.json`</SwmToken> file to clean up.

<SwmSnippet path="/private/Get-WarrantyDRMM.ps1" line="1" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

# Function Definition

The function <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="1:2:4" line-data="function  Get-WarrantyDattoRMM {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyDattoRMM`</SwmToken> is defined with parameters for API key, API URL, secret key, and options for syncing and overwriting warranty information.

```powershell
function  Get-WarrantyDattoRMM {
    [CmdletBinding()]
    Param(
        [string]$DRMMAPIKey,
        [String]$DRMMApiURL,
        [String]$DRMMSecret,
        [boolean]$SyncWithSource,
        [boolean]$Missingonly,
        [boolean]$OverwriteWarranty
    )
```

---

</SwmSnippet>

<SwmSnippet path="/private/Get-WarrantyDRMM.ps1" line="12" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

# Importing <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="13:5:5" line-data="        Import-module DattoRMM" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`DattoRMM`</SwmToken> Module

The function checks if the DattoRMM module is available and imports it. If not, it installs and then imports the module.

```powershell
    If (Get-Module -ListAvailable -Name "DattoRMM" | where-object {$_.version -ge "1.0.0.25"}) { 
        Import-module DattoRMM
    }
    Else { 
        Install-Module DattoRMM -Force
        Import-Module DattoRMM
    }
```

---

</SwmSnippet>

<SwmSnippet path="/private/Get-WarrantyDRMM.ps1" line="21" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

# Setting API Parameters

API parameters such as URL, API key, and secret key are set using the <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="28:1:3" line-data="    Set-DrmmApiParameters @params" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Set-DrmmApiParameters`</SwmToken> function.

```powershell
    $params = @{
        Url       = $DRMMApiURL
        Key       = $DRMMAPIKey
        SecretKey = $DRMMSecret
    }

    # Set API Parameters
    Set-DrmmApiParameters @params
```

---

</SwmSnippet>

<SwmSnippet path="/private/Get-WarrantyDRMM.ps1" line="30" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

# Retrieving Devices

The function retrieves all devices from the Datto RMM account and processes each device to obtain its serial number.

```powershell
    $ResumeLast = test-path 'Devices.json'
    If ($ResumeLast) {
        write-host "Found previous run results. Starting from last object." -foregroundColor green
        $AllDevices = get-content 'Devices.json' | convertfrom-json
    }
    else {
        $AllDevices = Get-DrmmAccountDevices | select-object DeviceClass, uid, SiteName, warrantyDate
    }
```

---

</SwmSnippet>

<SwmSnippet path="/private/Get-WarrantyDRMM.ps1" line="42" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

# Fetching Warranty Information

The serial number is used to fetch warranty information using the <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="58:6:8" line-data="        $WarState = Get-Warrantyinfo -DeviceSerial $DeviceSerial -client $device.siteName" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-Warrantyinfo`</SwmToken> function.

```powershell
    $warrantyObject = foreach ($device in $AllDevices) {
        try {
            if ($Device.DeviceClass -eq 'esxihost') {
                $DeviceSerial = (Get-DrmmAuditesxi  -deviceUid $device.uid).systeminfo.servicetag
            }
            else {
                $DeviceSerial = (Get-DrmmAuditDevice -deviceUid $device.uid).bios.serialnumber
            }
        }
        catch {
            write-host "Could not retrieve serialnumber for $device"
            continue
        }
        $i++
        Write-Progress -Activity "Grabbing Warranty information" -status "Processing $($DeviceSerial). Device $i of $($AllDevices.Count)" -percentComplete ($i / $AllDevices.Count * 100)

        $WarState = Get-Warrantyinfo -DeviceSerial $DeviceSerial -client $device.siteName
```

---

</SwmSnippet>

<SwmSnippet path="/private/Get-WarrantyDRMM.ps1" line="60" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

# Syncing with Source

If the <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="60:5:5" line-data="        if ($SyncWithSource -eq $true) {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`SyncWithSource`</SwmToken> parameter is true, the warranty information is updated in the Datto RMM system.

```powershell
        if ($SyncWithSource -eq $true) {
            switch ($OverwriteWarranty) {
                $true {
                    if ($null -ne $warstate.EndDate) {
                        Set-DrmmDeviceWarranty -deviceUid $device.uid -warranty ($warstate.EndDate).ToString('yyyy-MM-dd')
                    }
                     
                }
                $false { 
                    if ([string]::IsNullOrWhiteSpace($device.warrantyDate) -and $null -ne $warstate.EndDate) { 
                        Set-DrmmDeviceWarranty -deviceuid $device.uid -warranty ($warstate.EndDate).ToString('yyyy-MM-dd')
                    } 
                }
            }
        }
```

---

</SwmSnippet>

<SwmSnippet path="/private/Get-WarrantyDRMM.ps1" line="30" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

# Handling Interrupted Runs

The function handles resuming from the last processed device if a previous run was interrupted, by checking for a <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="33:11:13" line-data="        $AllDevices = get-content &#39;Devices.json&#39; | convertfrom-json" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Devices.json`</SwmToken> file.

```powershell
    $ResumeLast = test-path 'Devices.json'
    If ($ResumeLast) {
        write-host "Found previous run results. Starting from last object." -foregroundColor green
        $AllDevices = get-content 'Devices.json' | convertfrom-json
    }
```

---

</SwmSnippet>

<SwmSnippet path="/private/Get-WarrantyDRMM.ps1" line="77" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

# Returning Warranty Information

Finally, the function returns the collected warranty information and removes the <SwmToken path="/private/Get-WarrantyDRMM.ps1" pos="33:11:13" line-data="        $AllDevices = get-content &#39;Devices.json&#39; | convertfrom-json" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Devices.json`</SwmToken> file to clean up.

```powershell
    Remove-item 'devices.json' -Force -ErrorAction SilentlyContinue
    return $warrantyObject
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" doc-type="overview"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
