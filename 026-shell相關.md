## 日誌時間

```shell
#日誌名稱
log="./log/checkWbsite.log"  #操作日誌存放路徑 
fsize=2000000      #如果日誌大小超過上限，則儲存舊日誌，重新生成日誌檔案    
exec 2>>$log  #如果執行過程中有錯誤資訊均輸出到日誌檔案中 
#日誌函式
#引數
#引數一，級別，INFO ,WARN,ERROR
#引數二，內容
#返回值
function zc_log(){
    
    #判斷格式
    if [ 2 -gt $# ]
    then
        echo "parameter not right in zc_log function" ;
        return ;
    fi

    if [ -e "$log" ]
    then
        touch $log
    fi

    #當前時間
    local curtime;
    curtime=`date +%Y%m%d%H%M%S`
    

    #判斷檔案大小
    local cursize ;
    cursize=`cat $log | wc -c` ;

    if [ $fsize -lt $cursize ]
    then
        mv $log $curtime".out"
        touch $log ;
    fi

    #寫入檔案
    echo "$curtime $*" >> $log;
} 
```