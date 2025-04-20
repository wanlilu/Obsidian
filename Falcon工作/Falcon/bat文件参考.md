## 快速清除系统temp文件
```bat 
tasklist
taskkill /f /t /im FBuildWorker.exe
taskkill /f /t /im FBuildWorker.exe.copy
taskkill /f /t /im MSBuild.exe
del %temp% /s /q /f  > null
rmdir %temp% /s/q
start FBuildWorker.exe
start MSBuild.exe
```

## git本地库与svn项目更新脚本
```bat
rmdir /S /Q "./GameTrunk/Plugins/Falcon"

"C:\Program Files\TortoiseSVN\bin\TortoiseProc.exe"   /command:cleanup  /path:"./GameTrunk"
"C:\Program Files\TortoiseSVN\bin\TortoiseProc.exe"   /command:update  /path:"./GameTrunk"

rmdir /S /Q "./GameTrunk/Plugins/Falcon"

mklink /J H:\TKRandomMap\GameTrunk\Plugins\Falcon H:\TKRandomMap\Falcon
```
![c18029432eebbf5d9ee0fde3047f61b8_MD5](https://raw.githubusercontent.com/wanlilu/imgBed/main/notec18029432eebbf5d9ee0fde3047f61b8_MD5.jpeg)


