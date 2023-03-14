# random-stuff

# Edge always show URL

`omnibox.prevent_url_elisions`

See https://chromium.googlesource.com/chromium/src/+/4d46fb81638ef502663f2db09266bad6eb0ea12b/components/omnibox/browser/omnibox_prefs.cc#39


```bash
PATH=/mnt/c/Users/Kristof/AppData/Local/Microsoft/Edge/User Data/Default/Preferences
jq ".omnibox.prevent_url_elisions=true" "${PATH}" > /tmp/edge_temp && mv /tmp/edge_temp ${PATH}
```

# Start Menu: Disable Bing search

`BingSearchEnabled`

Found on the web. 

```pwsh
New-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Search" -Name "BingSearchEnabled" -Value 0 -PropertyType "DWORD" -Force
```

# Edge: Disable Bing in Sidebar Search

`SearchInSidebarEnabled`

See https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies#searchinsidebarenabled

(as Admin)
```pwsh
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Edge", "SearchInSidebarEnabled", "2", [Microsoft.Win32.RegistryValueKind]::DWord)
```

# Edge: Disable Big button top right (discover?)

`HubsSidebarEnabled`

See https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies#hubssidebarenabled

(as Admin)
```pwsh
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge", "HubsSidebarEnabled", "0", [Microsoft.Win32.RegistryValueKind]::DWord)
```
