# git_learn
Git and GitHub for Beginners - Crash Course from freecodecamp

# github字元符號
'#'，代表標題
'>'，代表code
'~'，代表刪除號



# terminal指令$
git clone "github repo"

git init : 起始化新的git repo

ls -la : list所有檔案包含隱藏的檔案

git staus : 可以查看現在落在哪個主幹或分枝，也可查看哪些檔案的狀態

紅字，代表尚未被commit

綠字，代表檔案被追蹤足跡

git add “files" : 讓git去追蹤更新的檔案或檔案裡的內容
. 代表所有檔案

git commit -m "更新主題" -m "詳細內容" : 先會在本機上儲存更新後的內容，還有上傳到fithub，然後status內的檔案都清空

git commit -a : git add.之後，這指令一次包括git add .和git commit，每次commit都要附加訊息，不然無法commit

-m，message

git push origin master : 這個動作才是從本地端真正上傳到遠端的github
orgin，代表git repository的位置
master，代表這個主幹我們要去push上傳的
## 回到家目錄~

在上傳之前要打造ssh-key，才能夠push
ssh-keygen -t rsa -b 4096 -C "sfad159357@gmail.com"

跑出底下訊息=>
Enter file in which to save the key (/Users/chieng-ming-yen/.ssh/id_rsa): testkey (如果沒有輸入，就會默認上面電腦給你的路徑~/.ssh/id_rsa)
輸入完檔案名稱，後面相關的設定也要跟著改成testkey

Enter passphrase (empty for no passphrase):

>ls | grep testkey  #可以查詢ssh key檔案

>cat testkey.pub  #顯示SSH token

要做終端機訊息的copy，首先要把copy的訊息highlight，而不是用cmd+C，對終端機而言，那是不同的指令，匡起來後輸入指令：

>pbcopy < ~/.ssh #pbcopy:複製上一段終端機訊息， < 後面是你要放的目錄

然後去到自己的github，點自己的頭像進去自己的主頁，在最右上角自己頭像的地方打開選單，底下有的settings，左側欄位有個SSH and GPG keys，點進去，右上方有個New SSK key，命名自己的ssh key，底下輸入key token，記住不能讓別人看到這個token

# ssh檔案
testkey.pub : 用來上傳github的介面
pub，代表public，用來讓公開其他人可以看到你上傳的檔案

相對的，testkey後面沒有pub，則是private key，上傳自己才看得到東西

# Adding your key to the ssh-agent

開啟ssh-agent:
~$ eval "$(ssh-agent -s)"
>Agent pid 15759

如果自己使用macOS sirrea 10.12.12或以上，需要修改~/.ssh/config檔 
我的筆電是macOS Mojave 10.14.6，所以要修改
\~$ vim ~/.ssh/config
按E，進入編輯，按I，進入INSERT模式，輸入：
Host *
AddKeysToAgent yes
UseKeychain yes
IdentityFile testkey #(官方：\~/.ssh/id_rsa)
然後按ESC離開模式，輸入:wq
w，儲存。q:離開

最後要把ssh key安裝到git上
$ ssh-add -K ~/.ssh/id_rsa  #(官方：~/.ssh/id_rsa)
=> Identity added: testkey (sfad159357@gmail.com)

# 隱藏檔案.git：用來儲存所有commit後的檔案

# 其他dir進行git
當創建其他的目錄後，首先$ git init，然後做完commit後，發現$ git push origin master出現fatal，因為要給github第二個repository給他 

所以回到github創第二個repo

>$ git remote add origin https://github.com/sfad159357/git_learn2.git

檢查看看是否有新增
>$ git remote -v

# 默認設置
如果不想每次push都要輸入後面的origin master，可以輸入
>$ git push -u origin master

# 被拒絕push
中間發生了小插曲，我在本地端先建立README.md的文件，後來在遠端的github創建新的repo，然後也順便創建了新的README.md。

然後，我想要把自己的文件push上去，跳出![rejected]訊息，在網路上查發現，因為我如果push上去，原本在本機上沒有github的文件，所以我push後，系統不知道到底要不要覆蓋掉，還是保留?

解決方法：
1.先拉回來推上去
＄git pull
把遠端repo拉回來直接進行一般合併
$ git pull --rebase
--rebase代表用rebase方式合併
但這兩方法還是不行

使用底下方法後就ok了，分支樹：

            tmp
             /--tmp('1')--tmp(' ')-- 
master('2')--               [rebase]\
             \--master('2')--------master,tmp(2,' ') => push HEAD:master , delete tmp 
>$ git fetch origin master:tmp
>$ git rebase tmp
>$ git push origin HEAD:master
>$ git branch -D tmp

fetch，代表先拉回來但不要自動合併
master:tmp，代表master的分枝
-D，代表刪除分枝

2.強迫覆蓋，不在我本機上的文件通通不見