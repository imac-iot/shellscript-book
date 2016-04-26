# codingstyle

##如何使用Shell？
Bash是目前唯一的Shell Script語言被官方承認的執行程式。
Shell執行檔開頭必須加上`#!/bin/bash`，可以在終端機內輸入`bash <shell_name>`來執行。

##什麼時候使用Shell？
Shell腳本僅適合作為小工具或是簡單的程式包裝腳本。
Shell並不是開發用語言，我們有些關於腳本語言使用的建議：
- 大部分時候都在呼叫別的工具程式且不需要大量運算時，Shell是一個很好的選擇。
- 當你很在意效能的時候，最好不要選用Shell。
- 當你需要很複雜的資料型態做運算或是你的程式相當龐大複雜，Python是比較好的選擇。

##副檔名
Shell執行檔可以沒有副檔名(建議)或是以`.sh`作為副檔名。函數庫則需要以`.sh`作為副檔名且本身需設為不可執行。

##SUID/SGID
所有語言執行時都會指定SUID和SGID兩個屬性，此程式在執行的時候必須由擁有特定權限的使用者來執行。然而在Shell腳本中考量到系統安全因素，這兩個屬性是無法被設定的。如果需要特定權限來執行腳本的話，使用sudo指令運行。

##STDOUT vs STDERR
為了方便區分普通狀態和錯誤，所有的錯誤訊息都應該用STDERR呈現。
建議使用函數印出內含相關訊息的錯誤如下。

```sh
err() {
    echo "[$(date +'%Y-%m-%dT%H:%M:%S%z')]: $@" >&2
}

if ! do_something; then
    err "Unable to do_something"
    exit "${E_DID_NOTHING}"
fi
```

##檔案開頭
所有檔案都應該以敘述此檔案的註解開頭，包含此腳本功能，也可註明作者和版權。

```sh
#!/bin/bash
#
#Perform GPIO Initial
```

##函式說明
有一定長度和複雜性的函數必須附上註解說明該函數作用、參數以及回傳值。

```sh
#######################################
# Output Error Message to console.
# Globals:
#     None
# Arguments:
#     ${ERR_MSG}
# Returns:
#     None
#######################################
err(){
...
}
```

##運行方式說明
不需要寫的太完整、太多。如果用了很複雜的演算法或是非常規作法也請簡單敘述就可。

##未辦事項說明
在註解最前面註明TODO，TODO後方有括弧包含寫此註解者。如果有的話可以在最後註明bug或是ticket編號。

```sh
# TODO(kiss5891): Bluetooth will shutdown here. (bug #173)
```

##縮排
四格空白，禁止使用tab，可以在編輯器設定中使用輸入四格空白取代tab行為。

##行長度及長字串
一行最大長度為80個字元。
如果字串長度大於80的話，可以使用Here文檔的方式呈現，也可以用斷行呈現。

```sh
# Here Document
cat << END
This is a very very very very
long string.

# Newline
str="This is a very very very very
long string."
```

長指令則可以使用反斜線'\'來做斷行，後面的內容需要縮排。

```sh
command1 \
  | command2 \
  | command3 \
  | command4
```

##迴圈和判斷式
使用`while`和`for`要與`; do`放在同一行，使用`if`的話要和`; then`同一行。

```sh
for pins in ${GPIO_PINS}; do
    if[ $pins -ne true ]; then
        pins=true
    fi
done
```
