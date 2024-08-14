---
title: Hudu Warranty Overview
---
Hudu Warranty refers to the process of retrieving and updating warranty information for devices managed within the Hudu IT documentation system.

The function <SwmToken path="/private/Get-WarrantyHudu.ps1" pos="1:2:4" line-data="function  Get-WarrantyHudu {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyHudu`</SwmToken> is responsible for fetching warranty details from Hudu by utilizing the Hudu API.

It requires parameters such as the Hudu API key, base URL, device asset layout, and warranty field to interact with the Hudu system.

The function processes the warranty information and updates the relevant fields in Hudu, ensuring that the warranty data is synchronized with the source.

This automation helps in maintaining up-to-date warranty information for devices, which is crucial for IT asset management and support.

## Configuring Hudu

To update Hudu, first edit the asset layout of the devices you wish to update to add a new field. If you wish to use expirations and not have apple devices, choose a date field and enable add to expirations. If you wish to have apple devices you will need to make this a string field and not use expirations. Make a note of the name of the field and the name of the asset layout then call the script.

## Running the Script

Use the following command to update the warranty information in Hudu: `update-warrantyinfo -Hudu `<SwmToken path="/private/Get-WarrantyHudu.ps1" pos="24:2:3" line-data="    New-HuduAPIKey $HuduAPIKey" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`-HuduAPIKey`</SwmToken>` "YourAPIKey" `<SwmToken path="/private/Get-WarrantyHudu.ps1" pos="25:2:3" line-data="    New-HuduBaseUrl $HuduBaseURL" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`-HuduBaseUrl`</SwmToken>` "YourHuduDomain" -HuduDeviceAssetLayout "Desktops / Laptops" -HuduWarrantyField "Warranty Expiry" -SyncWithSource -OverwriteWarranty -ExcludeApple`.

<SwmSnippet path="/private/Get-WarrantyHudu.ps1" line="1" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

## Fetching Warranty Information

The function <SwmToken path="/private/Get-WarrantyHudu.ps1" pos="1:2:4" line-data="function  Get-WarrantyHudu {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyHudu`</SwmToken> is responsible for fetching warranty details from Hudu by utilizing the Hudu API. It requires parameters such as the Hudu API key, base URL, device asset layout, and warranty field to interact with the Hudu system. The function processes the warranty information and updates the relevant fields in Hudu, ensuring that the warranty data is synchronized with the source.

```powershell
function  Get-WarrantyHudu {
    [CmdletBinding()]
    Param(
        [string]$HuduAPIKey,
        [String]$HuduBaseURL,
        [String]$HuduDeviceAssetLayout,
        [string]$HuduWarrantyField,
        [boolean]$SyncWithSource,
        [boolean]$Missingonly,
        [boolean]$OverwriteWarranty
    )


    write-host "Source is Hudu. Grabbing all devices." -ForegroundColor Green
    #Get the Hudu API Module if not installed
    if (Get-Module -ListAvailable -Name HuduAPI) {
        Import-Module HuduAPI 
    }
    else {
        Install-Module HuduAPI -Force
        Import-Module HuduAPI
```

---

</SwmSnippet>

# Main functions

There are several main functions in this folder. Some of them are <SwmToken path="/private/Get-WarrantyHudu.ps1" pos="1:2:4" line-data="function  Get-WarrantyHudu {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyHudu`</SwmToken> and Get-Warrantyinfo. We will dive a little into <SwmToken path="/private/Get-WarrantyHudu.ps1" pos="1:2:4" line-data="function  Get-WarrantyHudu {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyHudu`</SwmToken> and <SwmToken path="/public/Get-WarrantyInfo.ps1" pos="1:2:4" line-data="function  Get-Warrantyinfo {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-Warrantyinfo`</SwmToken>.

<SwmSnippet path="/private/Get-WarrantyHudu.ps1" line="1" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

## <SwmToken path="/private/Get-WarrantyHudu.ps1" pos="1:2:4" line-data="function  Get-WarrantyHudu {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyHudu`</SwmToken>

The function <SwmToken path="/private/Get-WarrantyHudu.ps1" pos="1:2:4" line-data="function  Get-WarrantyHudu {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyHudu`</SwmToken> is responsible for fetching warranty details from Hudu by utilizing the Hudu API. It requires parameters such as the Hudu API key, base URL, device asset layout, and warranty field to interact with the Hudu system. The function processes the warranty information and updates the relevant fields in Hudu, ensuring that the warranty data is synchronized with the source.

```powershell
function  Get-WarrantyHudu {
    [CmdletBinding()]
    Param(
        [string]$HuduAPIKey,
        [String]$HuduBaseURL,
        [String]$HuduDeviceAssetLayout,
        [string]$HuduWarrantyField,
        [boolean]$SyncWithSource,
        [boolean]$Missingonly,
        [boolean]$OverwriteWarranty
    )


    write-host "Source is Hudu. Grabbing all devices." -ForegroundColor Green
    #Get the Hudu API Module if not installed
    if (Get-Module -ListAvailable -Name HuduAPI) {
        Import-Module HuduAPI 
    }
    else {
        Install-Module HuduAPI -Force
        Import-Module HuduAPI
```

---

</SwmSnippet>

<SwmSnippet path="/public/Get-WarrantyInfo.ps1" line="1" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=">

---

## <SwmToken path="/public/Get-WarrantyInfo.ps1" pos="1:2:4" line-data="function  Get-Warrantyinfo {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-Warrantyinfo`</SwmToken>

The function <SwmToken path="/public/Get-WarrantyInfo.ps1" pos="1:2:4" line-data="function  Get-Warrantyinfo {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-Warrantyinfo`</SwmToken> is called within <SwmToken path="/private/Get-WarrantyHudu.ps1" pos="1:2:4" line-data="function  Get-WarrantyHudu {" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`Get-WarrantyHudu`</SwmToken> to retrieve warranty information based on the device serial number and client. It determines the vendor and calls the appropriate function to get the warranty details, such as <SwmToken path="/public/Get-WarrantyInfo.ps1" pos="11:5:7" line-data="            HP { get-HPWarranty -SourceDevice $DeviceSerial -Client $line.client }" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`get-HPWarranty`</SwmToken>, <SwmToken path="/public/Get-WarrantyInfo.ps1" pos="12:5:7" line-data="            Dell { get-DellWarranty -SourceDevice $DeviceSerial -Client $line.client }" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`get-DellWarranty`</SwmToken>, <SwmToken path="/public/Get-WarrantyInfo.ps1" pos="13:5:7" line-data="            Lenovo { get-LenovoWarranty -SourceDevice $DeviceSerial -Client $line.client }" repo-id="Z2l0aHViJTNBJTNBUG93ZXJTaGVsbFdhcnJhbnR5UmVwb3J0cyUzQSUzQVN3aW1tLURlbW8=" repo-name="PowerShellWarrantyReports">`get-LenovoWarranty`</SwmToken>, etc.

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
