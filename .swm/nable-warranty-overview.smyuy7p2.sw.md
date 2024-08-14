---
title: Nable Warranty Overview
---
Nable Warranty refers to the process of retrieving warranty information for devices managed by <SwmToken path="/private/Get-WarrantyNable.ps1" pos="25:12:14" line-data="            write-host &quot;Grabbing devices from N-Central&quot; -ForegroundColor Green" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`N-Central`</SwmToken>, a remote monitoring and management (RMM) platform.

The function <SwmToken path="/private/Get-WarrantyNable.ps1" pos="1:2:4" line-data="function  Get-WarrantyNable {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyNable`</SwmToken> is responsible for connecting to the N-Central server using a provided URL and JWT key to fetch device information.

It collects serial numbers and other relevant details from the devices managed by N-Central.

The function then calls <SwmToken path="/public/Get-WarrantyInfo.ps1" pos="1:2:4" line-data="function  Get-Warrantyinfo {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-Warrantyinfo`</SwmToken> to retrieve the warranty status for each device based on its serial number.

The retrieved warranty information is processed and returned as a PowerShell custom object.

The function also handles scenarios where the warranty information needs to be synchronized with the source, although N-Central does not support writing back warranty data.

<SwmSnippet path="/private/Get-WarrantyNable.ps1" line="1" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

# Function <SwmToken path="/private/Get-WarrantyNable.ps1" pos="1:2:4" line-data="function  Get-WarrantyNable {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyNable`</SwmToken>

The function <SwmToken path="/private/Get-WarrantyNable.ps1" pos="1:2:4" line-data="function  Get-WarrantyNable {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyNable`</SwmToken> is responsible for connecting to the N-Central server using a provided URL and JWT key to fetch device information. It collects serial numbers and other relevant details from the devices managed by N-Central. The function then calls <SwmToken path="/public/Get-WarrantyInfo.ps1" pos="1:2:4" line-data="function  Get-Warrantyinfo {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-Warrantyinfo`</SwmToken> to retrieve the warranty status for each device based on its serial number. The retrieved warranty information is processed and returned as a PowerShell custom object. The function also handles scenarios where the warranty information needs to be synchronized with the source, although N-Central does not support writing back warranty data.

```powershell
function  Get-WarrantyNable {
    [CmdletBinding()]
    Param(
        [string]$NableURL,
        [String]$JWTKey,
        [boolean]$SyncWithSource,
        [boolean]$Missingonly,
        [boolean]$OverwriteWarranty
    )

    # Generate a pseudo-unique namespace to use with the New-WebServiceProxy and
    # associated types.
    $NWSNameSpace = "NAble" + ([guid]::NewGuid()).ToString().Substring(25)

    # Bind to the namespace, using the Webserviceproxy
    $bindingURL = "https://" + $NableURL + "/dms/services/ServerEI?wsdl"
    $nws = New-Webserviceproxy $bindingURL -Namespace ($NWSNameSpace)
    If ($ResumeLast) {
        write-host "Found previous run results. Starting from last object." -foregroundColor green
        $Devices = get-content 'Devices.json' | convertfrom-json
    }
```

---

</SwmSnippet>

<SwmSnippet path="/public/Get-WarrantyInfo.ps1" line="1" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

# Function <SwmToken path="/public/Get-WarrantyInfo.ps1" pos="1:2:4" line-data="function  Get-Warrantyinfo {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-Warrantyinfo`</SwmToken>

The function <SwmToken path="/public/Get-WarrantyInfo.ps1" pos="1:2:4" line-data="function  Get-Warrantyinfo {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-Warrantyinfo`</SwmToken> is called by <SwmToken path="/private/Get-WarrantyNable.ps1" pos="1:2:4" line-data="function  Get-WarrantyNable {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyNable`</SwmToken> to retrieve the warranty status for each device based on its serial number. The retrieved warranty information is processed and returned as a PowerShell custom object.

```powershell
function  Get-Warrantyinfo {
    [CmdletBinding()]
    Param(
        [string]$DeviceSerial,
        [String]$client,
        [String]$vendor
    )
    if ($LogActions) { add-content -path $LogFile -Value "Starting lookup for $($DeviceSerial),$($Client)" -force }
    if ($vendor) {
        switch ($vendor) {
            HP { get-HPWarranty -SourceDevice $DeviceSerial -Client $line.client }
            Dell { get-DellWarranty -SourceDevice $DeviceSerial -Client $line.client }
            Lenovo { get-LenovoWarranty -SourceDevice $DeviceSerial -Client $line.client }
            MS { Get-MSWarranty -SourceDevice $DeviceSerial -Client $line.client }
            Apple { get-AppleWarranty -SourceDevice $DeviceSerial -client $line.client }
            Toshiba { get-ToshibaWarranty -SourceDevice $DeviceSerial -client $line.client }
        }
    }
    else {
        switch ($DeviceSerial.Length) {
            7 { get-DellWarranty -SourceDevice $DeviceSerial -client $Client }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" doc-type="overview"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
