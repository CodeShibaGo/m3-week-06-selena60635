# Questions

## Q: 使用 `virtualenv` 建立虛擬環境 #116

安裝 virtualenv - `pip install virtualenv`，解除安裝`pip uninstall virtualenv`。

建立 virtualenv 環境 - `virtualenv 環境名稱`。

查看 virtualenv 版本 - `virtualenv --version`，查看版本號，確認是否安裝成功。
### 由於我是win系統，因此在WSL系統(Ubuntu)進行安裝：

安裝 virtualenv - `sudo apt-get install python3-virtualenv`。

解除安裝 - `sudo apt-get remove python3-virtualenv`，刪除所有設定文件`sudo apt-get purge python3-virtualenv`。

啟動虛擬環境 - `source 環境名稱/bin/activate`，執行.venv/bin/activate；停用環境`deactivate`。

[參考資料](https://learn.microsoft.com/zh-tw/windows/python/web-frameworks)

## Q: python-dotenv 如何使用？ #119

Python dotenv 是一個用於加載環境變數的Python模組，可以從 .env 文件中加載環境變數，並使其得以運用在應用程式中

安裝 python-dotenv - `pip install python-dotenv`

如果安裝成功後無法使用`dotenv --help`…等dotenv指令；安裝 python-dotenv 命令行介面 - `pip install "python-dotenv[cli]"`

查看模組詳細資訊 - `pip show python-dotenv`，包含模組安裝路徑。

查看 python-dotenv 版本 - `dotenv --version`，查看版本，確認是否安裝成功。

### 開始使用 python-dotenv

1. 在專案資料夾根目錄，`touch .env`創建一個.env檔，你可以在裡面設定環境變數。

```
DB_HOST=localhost
DB_USER=myuser
DB_PASS=mypassword
```

2. 在專案資料夾中，創建一個`檔案名稱.py`，用以載入.env文件配置。

```python
# 載入python的os模組
import os
# 從dotenv模組中匯入load_dotenv函數，用於加載.env 文件中的環境變數
from dotenv import load_dotenv
# 執行load_dotenv函數，匯入目前專案資料夾下的.env文件中的環境變數，以便使用
load_dotenv()
# 使用os.getenv()函數從環境變數中提取DB_HOST的值，並將值赋值给變數db_host。
db_host = os.getenv("DB_HOST")
db_user = os.getenv("DB_USER")
db_pass = os.getenv("DB_PASS")
# 印出變數db_host值
print(db_host)  # localhost
print(db_user)  # myuser
print(db_pass)  # mypassword
```

### .env 與 .flaskenv 的差別

**.flaskenv**

專屬於 Flask 製作的專案，需要創建在專案根目錄，在**開啟時會自動載入.flaskenv文件**，通常包含一些特定於 Flask 的變數，例如**FLASK_APP**和**FLASK_ENV**等。
.**env**

更通用，**不限於任何特定框架或工具**，可以用於任何類型的應用程式。它可以包含各種類型的環境變數，但在一般情況下不會自動載入，要安裝python-dotenv並自行匯入。

雖然他們是**可以同時使用**的，但在某些情况下可能會發生衝突，例如：同時使用**.env**和**.flaskenv**文件，並且兩者都包含相同的環境變數時，在載入時就會發生衝突。因此在使用時要小心管理確保不會互相干擾。

## Q: 如何使用 Flask-SQLAlchemy 連接上 MySQL？ #123

## Q: Flask-Migrate 如何使用？ #124

## Q: 如何使用 SQLAlchemy 下 Raw SQL？ #125

connection.execute 的好處是更簡潔。cursor.execute 的好處是標準/通用

## Q: 如何用土炮的方式建立 Table？ #126

## Q: 什麼是密碼雜湊？如何使用 Python 實現？ #129
