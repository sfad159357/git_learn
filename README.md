# git_learn
Git and GitHub for Beginners - Crash Course from freecodecamp

開始日期：2020/05/29

# github字元符號
'#'，代表標題

'>'，代表code

'~'，代表刪除號

'\\'，代表取消符號功能


# terminal指令$
複製github專案到本機上：
>git clone "github repo網址"

起始化新的git repo：
>git init 

list所有檔案包含隱藏的檔案：
>ls -la 

查看狀態：
>git staus 

可以查看現在落在哪個主幹或分枝，也可查看哪些檔案的狀態

紅字，代表尚未被commit

綠字，代表檔案被追蹤足跡

讓git去追蹤更新後的檔案:
>git add “files" 

. 代表所有檔案

提交待上傳區：
>git commit -m "更新主題" -m "詳細內容" 

-m，message
先會在本機上儲存更新後的內容，還有上傳到github，然後status內的檔案都清空

追蹤並提交：
>git commit -a -m"更新主題" -m "詳細內容"

git add.之後，這指令一次只包括git add "修改後的files"，不包含"新創建的files"。

而-m，每次commit都要附加訊息，不然無法commit

上傳github：
>git push origin master 

orgin，代表git repository的位置

master，代表這個主幹我們要去push上傳的

查詢分枝狀態：
>git branch

master，主幹

創建新分枝：
>git branch "分枝名"

創建新分支並轉換：
>git checkout -b "分枝名稱"

-b，branch

git master，直接轉換回主幹

分枝改名：
>git branch -m "舊分枝名" "新分枝名"

!!!記得要按ctrl+S，儲存後電腦才知道那是已經修改過的內容(待確認是不是一定要儲存)

在checkout之前，要先把任何修改後的內容做個commit

在分枝中add, commit後，這時被修改的資訊被儲存於分枝裡頭，這時再切回主幹，會回復原來上次commit後的內容。因為後面做的修改，已經被岔出分枝當中，主幹就不受影響。

如果push後，分枝的commit已經上傳了，這時在checkout回master，不會有code上差異

查看離上次commit後差異了哪些code:
>git diff

查看主幹和分枝之間code的差異：
>git diff "主幹/分枝名稱"

*要儲存才會更改訊息

如果在master，就diff 分枝名。反之亦然。

可以在終端機直接判讀差異，可以按Q返回。(但目前我的終端機diff出來對不起來。ps:底下:往下拉會有訊息移上來，看到END才結束)

進行分枝push:
>git push --set-upstream origin "分枝名"

等效於:
>git push -u origin "分枝名"

合併：
>git merge "分枝名"

撤回stage:
>git reset ("檔名")

撤回commit:
>git reset master^

也等效於

>git reset HEAD^

reset，不是代表重新設定，代表前往

master^，代表master前一個狀態。^，往前一次的意思，^^往前兩次的狀態。

reset XXX，XXX可以是commit編號，可以是主幹/分枝，可以是HEAD

撤回commit並刪除在工作區被commit的內容：
>git reset --hard "commit編號"

打開git日誌：
>git log

裡面可以看到commit編號

加上分枝圖：
>git log --graph

若要看到倒數n筆紀錄：
>git log -"n"

查看HEAD位置：
>git show HEAD

一樣有新增刪除diff訊息，但是對起來就是怪怪的

刪除分枝：
>git branch -d "分枝名"

# 備註：
1.分枝feature-readme-instructions commit時是失敗的，創建分枝後居然還是跟主幹master一樣合併在一起，沒有岔開，我看log中HEAD一樣指在master上，同時也擁有分枝feature，我猜想應該是創建分枝時，直接用:
>git checkout -b "分枝名稱"

的關係吧？

2.如果分枝源自於master，然後分枝比master多幾個commit，接下來切回master，進行合併分枝。git會判斷其實分枝只多幾個commit，自動選用「快轉模式(Fast Forward)」來進行合併，在log --graph當中，就不會看到分叉後又接回去的樹形圖。

如果真的想要看到分岔合併線圖，可以在合併時：
>git merge "分枝名" --no-ff

--no-ff，代表不使用快轉模式

3.通常分枝在commit後，不能直接和master merge，而是會先push到github上，然後去create pull request，讓你的code供團隊瀏覽並在底下留言。可以針對第幾行的範圍下建議或評論，如果ok，有permission的人才能決定是否合併github中的master。合併成功後，原來在自己的分枝並不會有master，所以要:
>git pull origin master

將遠端合併完成的master拉到本地端的code，繼續進行coding。這樣做確保不會有錯誤的code混進master code裡面。

4.通常不會在本地端直接進行merge，但是master會有其他人的分枝進行新增或刪減，你不會希望自己處理的分枝，離github裡的master專案內容越差越遠，這樣到時候很難合併。所以會將遠地端的master進行git pull到自己本地端的master，然後自己本地端的分枝去合併拉到本地端的master。如此一來， 才能夠讓你自己的分枝follow最新master開發進度。

5.當master和分枝在同個行的code中發生兩個不一樣的code的時候，進行merge，git會丟出conflict出來，這時用VS code就很方便，看你要怎麼去合併他。選擇選項後的合併，需要再次進行一次commit -am，

## 回到家目錄~s
在上傳之前要打造ssh-key，才能夠push
>ssh-keygen -t rsa -b 4096 -C "sfad159357@gmail.com"

跑出底下訊息=>
Enter file in which to save the key (/Users/chieng-ming-yen/.ssh/id_rsa): testkey (如果沒有輸入，就會默認上面電腦給你的路徑~/.ssh/id_rsa)

輸入完檔案名稱，後面相關的設定也要跟著改成testkey

Enter passphrase (empty for no passphrase):

可以查詢ssh key檔案:
>ls | grep testkey  

顯示SSH token:
>cat testkey.pub  

要做終端機訊息的copy，首先要把copy的訊息highlight，而不是用cmd+C，對終端機而言，那是不同的指令，匡起來後輸入指令：

複製token到\~/.ssh目錄裡面：
>pbcopy < ~/.ssh

pbcopy:複製上一段終端機訊息， < 後面是你要放的目錄

然後去到自己的github，點自己的頭像進去自己的主頁，在最右上角自己頭像的地方打開選單，底下有的settings，左側欄位有個SSH and GPG keys，點進去，右上方有個New SSK key，命名自己的ssh key，底下輸入key token，記住不能讓別人看到這個token


# ssh檔案
testkey.pub : 用來上傳github的介面

pub，代表public，用來讓公開其他人可以看到你上傳的檔案

相對的，testkey後面沒有pub，則是private key，上傳自己才看得到東西

# Adding your key to the ssh-agent

開啟ssh-agent:
>eval "$(ssh-agent -s)"

跳出訊息：Agent pid 15759

如果自己使用macOS sirrea 10.12.12或以上，需要修改\~/.ssh/config檔

我的筆電是macOS Mojave 10.14.6，所以要修改


打開vim編輯器修改config:
>vim ~/.ssh/config

按E，進入編輯，按I，進入INSERT模式，輸入：

Host *

AddKeysToAgent yes

UseKeychain yes

IdentityFile testkey 

#(官方：\~/.ssh/id_rsa)

然後按ESC離開模式，輸入:wq

w，儲存。q:離開

最後要把ssh key安裝到git上:
>ssh-add -K \~/.ssh/testkey 

 (官方：\~/.ssh/id_rsa)

=> Identity added: testkey (sfad159357@gmail.com)

# 隱藏檔案.git：用來儲存所有commit後的檔案

# 其他dir進行git
當創建其他的目錄後，首先$ git init，然後做完commit後，發現$ git push origin master出現fatal，因為要給github第二個repository給他 

所以回到github創第二個repo:
>$ git remote add origin https://github.com/sfad159357/git_learn2.git

檢查看看是否有新增:
>$ git remote -v

# 默認設置
如果不想每次push都要輸入後面的origin master，可以輸入:
>$ git push -u origin master

# 被拒絕push
中間發生了小插曲，我在本地端先建立README.md的文件，後來在遠端的github創建新的repo，然後也順便創建了新的README.md。

然後，我想要把自己的文件push上去，跳出![rejected]訊息，在網路上查發現，因為我如果push上去，原本在本機上沒有github的文件，所以我push後，系統不知道到底要不要覆蓋掉，還是保留?

解決方法：
1.先拉回來推上去
>git pull

把遠端repo拉回來直接進行一般合併:
>git pull --rebase

--rebase代表用rebase方式合併

但這兩方法還是不行


使用底下方法後就ok了，分支樹(VS code)：

"""
            tmp
             /--tmp('1')--tmp(' ')-- 
master('2')--               [rebase]\
             \\--master('2')--------master,tmp(2,' ') => push HEAD:master , delete tmp 

"""

從github取檔在master分枝tmp:
>git fetch origin master:tmp

master本地內容合併分枝tmp遠端內容:
>git rebase tmp

在master頂端進行push:
>git push origin HEAD:master

刪掉分枝tmp:
>git branch -D tmp

fetch，代表先拉回來但不要自動合併

master:tmp，代表master的分枝

-D，代表刪除分枝

2.強迫覆蓋，不在我本機上的文件通通不見:
>git push -f

## master測試1

拉啦啦啦
