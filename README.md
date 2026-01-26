# random-stuff

# Edge always show URL

`omnibox.prevent_url_elisions`

See https://chromium.googlesource.com/chromium/src/+/4d46fb81638ef502663f2db09266bad6eb0ea12b/components/omnibox/browser/omnibox_prefs.cc#39

```pwsh

# stop all instances of msedge.exe... by force. Save your stuff!
Stop-Process -Name "msedge"

# or better, go to tripple dot > Close Microsoft Edge

# path to settings
$path = ("{0}\Microsoft\Edge\User Data\Default\Preferences" -f $env:LOCALAPPDATA)

# read settings
$preferences = Get-Content $path | ConvertFrom-Json -AsHashTable

# we're nice, we don't want to just overwrite what is ther
# so we make sure omnibox exists or not
if ($preferences.omnibox -eq $null) {
    # doesn't exist, set to empty hashtable
    $preferences.omnibox = @{}
}

# add our value
$preferences.omnibox.Add("prevent_url_elisions", $True)

# dump back to disk. Notice -Depth 100 as pwsh by default stops at 2 (to prevent infinite recursion?)
$preferences | ConvertTo-Json -Compress -Depth 100 | Set-Content $path

# Launch Edge

# Verify AFTER you've launched Edge, to see if it stuck
$preferences = Get-Content $path | ConvertFrom-Json -AsHashTable
$preferences.omnibox

# Should print

# ~
# ❯ $preferences.omnibox

# Name                           Value
# ----                           -----
# prevent_url_elisions           True


# OR
# Launch Edge, visit http://neverssl.com, should show Not secure "http://"
```

# Start Menu: Disable Bing search

`BingSearchEnabled`

Found on the web.

This one does NOT work with `HKEY_LOCAL_MACHINE`.

(as Admin)

```pwsh
[Microsoft.Win32.Registry]::SetValue("HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Search", "BingSearchEnabled", "0", [Microsoft.Win32.RegistryValueKind]::DWord)
```

# Edge: Disable Big button top right (discover?), and Bing Sidebar Search everywhere (incl PWAs)

`HubsSidebarEnabled`, `StandaloneHubsSidebarEnabled	`, `SearchInSidebarEnabled`

See https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies#hubssidebarenabled
See https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies#standalonehubssidebarenabled
See https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies#searchinsidebarenabled

(as Admin)

```pwsh
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge", "HubsSidebarEnabled", "0", [Microsoft.Win32.RegistryValueKind]::DWord)
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge", "SearchInSidebarEnabled", "0", [Microsoft.Win32.RegistryValueKind]::DWord)
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge", "StandaloneHubsSidebarEnabled", "0", [Microsoft.Win32.RegistryValueKind]::DWord)
```

# WSL: Fix `/sbin/ldconfig.real: /usr/lib/wsl/lib/libcuda.so.1 is not a symbolic link`

(as Admin in CMD)

```cmd
wsl --shutdown
cd c:\Windows\System32\lxss\lib
del libcuda.so
del libcuda.so.1

mklink libcuda.so libcuda.so.1.1
mklink libcuda.so.1 libcuda.so.1.1
```

[Wait ~8 seconds...](https://learn.microsoft.com/en-us/windows/wsl/wsl-config#the-8-second-rule)

(in WSL)

```sh
sudo apt reinstall libc-bin
```

# Lenovo Commercial Vantage: Disable WiFi Security

From Lenovo's Commercial Vantage package documentation

(as Admin)

```pwsh
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Lenovo\Commercial Vantage", "page.wifiSecurity", "1", [Microsoft.Win32.RegistryValueKind]::DWord)
```

# Edge: Stop recommended settings prompt

`ShowRecommendationsEnabled`

See https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies#showrecommendationsenabled

(as Admin)

```pwsh
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Edge", "ShowRecommendationsEnabled", "0", [Microsoft.Win32.RegistryValueKind]::DWord)
```

# Edge: Enable Adobe Acrobat PDF, disable subscription nag

`NewPDFReaderEnabled` & `ShowAcrobatSubscriptionButton`

See https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies#newpdfreaderenabled & https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies#showacrobatsubscriptionbutton

(as Admin)

```pwsh
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Edge", "NewPDFReaderEnabled", "1", [Microsoft.Win32.RegistryValueKind]::DWord)
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Edge", "ShowAcrobatSubscriptionButton", "0", [Microsoft.Win32.RegistryValueKind]::DWord)
```

# Windows 11: Disable Widgets

`AllowNewsAndInterests`

See https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-newsandinterests#allownewsandinterests

(as Admin)

```pwsh
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh", "AllowNewsAndInterests", "0", [Microsoft.Win32.RegistryValueKind]::DWord)

```

# Windows 11: Disable Recommended section in start menu

`HideRecommendedSection`

See https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-start#hiderecommendedsection

(as Admin)

```pwsh
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Explorer", "HideRecommendedSection", "1", [Microsoft.Win32.RegistryValueKind]::DWord)

```

# Postgres: enable logging without restarting

```
ALTER SYSTEM SET log_statement = 'all';
SELECT pg_reload_conf();
```

Exit and log container output.

To reset:

```
ALTER SYSTEM RESET log_statement;
SELECT pg_reload_conf();
```

# Hide pesky Windows Updates:

For some reason Windows wants to roll-back certain drivers. Why? I don't know.

List installed Windows Updates:

```pwsh
 Get-WUHistory | Format-Table -Property @{ Name = "UpdateID"; Expr = { $_.UpdateIdentity.UpdateID } }, @{ Name = "RevisionNumber"; Expr = { $_.UpdateIdentity.RevisionNumber } }, Title
```

For example, to hide `Wacom Technology - HIDClass - 7.7.2.44`:

```pwsh
Hide-WindowsUpdate -UpdateID "32177145-fc17-4aee-b61a-03036dca296e" -RevisionNumber 1
```
