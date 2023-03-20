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

This one does NOT work with `HKEY_LOCAL_MACHINE`.

(as Admin)
```pwsh
[Microsoft.Win32.Registry]::SetValue("HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Search", "BingSearchEnabled", "0", [Microsoft.Win32.RegistryValueKind]::DWord)
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

`HubsSidebarEnabled`

See https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies#hubssidebarenabled

(as Admin)
```pwsh
[Microsoft.Win32.Registry]::SetValue("HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Lenovo\Commercial Vantage", "page.wifiSecurity", "1", [Microsoft.Win32.RegistryValueKind]::DWord)
```
