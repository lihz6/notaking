```cmd
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\openIE]
@="URL:OpenIE Protocol"
"URL Protocol"=""

[HKEY_CLASSES_ROOT\openIE\DefaultIcon]
@="iexplore.exe,1"

[HKEY_CLASSES_ROOT\openIE\shell]

[HKEY_CLASSES_ROOT\openIE\shell\open]

[HKEY_CLASSES_ROOT\openIE\shell\open\command]
@="cmd /c set m=%1 & call set m=%%m:openIE:=%% & call \"C:\\Program Files\\Internet Explorer\\iexplore.exe\" %%m%% & exit"
```

```html
<a href="openIE:www.baidu.com/"></a>
```
