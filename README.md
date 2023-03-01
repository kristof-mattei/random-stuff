# random-stuff

# Edge always show URL

`omnibox.prevent_url_elisions`

See https://chromium.googlesource.com/chromium/src/+/4d46fb81638ef502663f2db09266bad6eb0ea12b/components/omnibox/browser/omnibox_prefs.cc#39


```bash
PATH=/mnt/c/Users/Kristof/AppData/Local/Microsoft/Edge/User Data/Default/Preferences
jq ".omnibox.prevent_url_elisions=true" "${PATH}" > /tmp/edge_temp && mv /tmp/edge_temp ${PATH}
```

# Disable Bing search

`BingSearchEnabled`

See ... just found on the web

```pwsh
New-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Search" -Name "BingSearchEnabled" -Value 0 -PropertyType "DWORD" -Force
```
