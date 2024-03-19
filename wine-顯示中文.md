# 參考文章
- [Ubuntu -wine中文亂碼的解決方法](https://www.twblogs.net/a/5e53304ebd9eee2117c39cff)

# 上述文章內容 (我親測有效)

新裝的wine中文全是亂碼，需要修改一下幾個配置文件，找到一篇比較詳細的配置說明，分享一下：

步驟： 
## 1. 初始設置
運行 winecfg，把模擬的 Windows 系統設置爲 Windows XP 或者 Windows 2000。
## 2. 準備字體
爲了讓 Windows 應用程序看上去更美觀，所以需要 Windows 下面的字體。
由於我已經將 simsun.ttc 複製到 /usr/share/fonts/windows/ 目錄中了。所以我只需要在 ~/.wine/drive_c/windows/fonts/ 目錄中爲 simsun.ttc 創建一個符號連接：

cd ~/.wine/drive_c/windows/fonts
ln -s /usr/share/fonts/windows/simsun.ttc simsun.ttc
ln -s /usr/share/fonts/windows/simsun.ttc simfang.ttc

創建一個 simfang.ttc 是許多 Windows 應用默認使用 simfang.ttc 字體。
## 3. 修改 ~/.wine/system.reg
裝好字體後，還要修改一下 Wine 的註冊表設置，指定與字體相關的設置：
gedit ~/.wine/system.reg
（一定要使用 gedit 或其他支持 gb2312/utf8 編碼的編輯器修改這些文件，否則文件中的中文可能變亂碼）
搜索： LogPixels
找到的行應該是：[System\\CurrentControlSet\\Hardware Profiles\\Current\\Software\\Fonts]
將其中的：
"LogPixels"=dword:00000060
改爲：
"LogPixels"=dword:00000070
搜索： FontSubstitutes
找到的行應該是：[Software\\Microsoft\\Windows NT\\CurrentVersion\\FontSubstitutes]
將其中的：
"MS Shell Dlg"="Tahoma"
"MS Shell Dlg 2″="Tahoma"
改爲：
"MS Shell Dlg"="SimSun"
"MS Shell Dlg 2″="SimSun"
## 4. 修改 ~/.wine/drive_c/windows/win.ini
如果想保留原字體大小的話，這部份可以不用改

gedit ~/.wine/drive_c/windows/win.ini
在文件末尾加入：

[Desktop]
menufontsize=13
messagefontsize=13
statusfontsize=13
IconTitleSize=13

## 5. 最關鍵的一步，網上很多文章中沒有提到的一步──把下面的代碼保存爲zh.reg，然後終端執行
regedit zh.reg

REGEDIT4
[HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\FontSubstitutes]
"Arial"="simsun"
"Arial CE,238"="simsun"
"Arial CYR,204"="simsun"
"Arial Greek,161"="simsun"
"Arial TUR,162"="simsun"
"Courier New"="simsun"
"Courier New CE,238"="simsun"
"Courier New CYR,204"="simsun"
"Courier New Greek,161"="simsun"
"Courier New TUR,162"="simsun"
"FixedSys"="simsun"
"Helv"="simsun"
"Helvetica"="simsun"
"MS Sans Serif"="simsun"
"MS Shell Dlg"="simsun"
"MS Shell Dlg 2"="simsun"
"System"="simsun"
"Tahoma"="simsun"
"Times"="simsun"
"Times New Roman CE,238"="simsun"
"Times New Roman CYR,204"="simsun"
"Times New Roman Greek,161"="simsun"
"Times New Roman TUR,162"="simsun"
"Tms Rmn"="simsun"

再winecfg，就可以看到中文能正常顯示了.
