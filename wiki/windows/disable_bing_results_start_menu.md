# Disable Bing results (web search) in start menu

## In Windows Pro

1. Run gpedit.msc -> Local Group Policy Editor
2. Go to Computer Configuration > Administrative Templates > Windows Components > Search
3. Enable "Do not allow web search".
4. Enable "Don't search the web or display web results in Search".


## In Windows Home

1. Run Regedit
2. Navigate to Computer\HKEY_CURRENT_USER\Softweare\Policies\Microsoft\Windows
3. Create a new "DWORD (32-bit) Value", name it DisableSearchBoxSuggestions
4. Assign value (Hex) 1
5. Navigate to Computer\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Search
6. Create a new "DWORD (32-bit) Value", name it BingSearchEnabled
7. Assign value (Hex) 0
