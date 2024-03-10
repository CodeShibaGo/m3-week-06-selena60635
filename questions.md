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

Python dotenv 是一個用於加載環境變數的Python模組，可以從 .env 文件中加載環境變數，並使其得以運用在應用程式中。

安裝 python-dotenv - `pip install python-dotenv`。

如果安裝成功後無法使用`dotenv --help`…等dotenv指令；安裝 python-dotenv 命令行介面 - `pip install "python-dotenv[cli]"`。

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
# 載入python的os模組。
import os
# 從dotenv模組中匯入load_dotenv函數，用於加載.env 文件中的環境變數。
from dotenv import load_dotenv
# 執行load_dotenv函數，匯入目前專案資料夾下的.env文件中的環境變數，以便使用。
load_dotenv()
# 使用os.getenv()函數從環境變數中提取DB_HOST的值，並將值赋值给變數db_host。
db_host = os.getenv("DB_HOST")
db_user = os.getenv("DB_USER")
db_pass = os.getenv("DB_PASS")
# 印出變數db_host值。
print(db_host)  # localhost
print(db_user)  # myuser
print(db_pass)  # mypassword
```

### .env 與 .flaskenv 的差別

**.flaskenv**

專屬於 Flask 製作的專案，需要創建在專案根目錄，在**開啟時會自動載入.flaskenv文件**，通常包含一些特定於 Flask 的變數，例如**FLASK_APP**和**FLASK_ENV**等。

**.env**

更通用，**不限於任何特定框架或工具**，可以用於任何類型的應用程式。它可以包含各種類型的環境變數，但在一般情況下不會自動載入，要安裝python-dotenv並自行匯入。

雖然他們是**可以同時使用**的，但在某些情况下可能會發生衝突，例如：同時使用.env和.flaskenv文件，並且兩者都包含相同的環境變數時，在載入時就會發生衝突。因此在使用時要小心管理確保不會互相干擾。

## Q: 如何使用 Flask-SQLAlchemy 連接上 MySQL？ #123

- 確認有安裝MySQL。
- 安裝 PyMySQL - `pip install PyMySQL`，Python中用來操作MySQL的 DBAPI。
- 準備好你的資料庫。
    
    **使用MySQL的情況**與sqlite不同，**SQL ALChemy不會自動幫我們建立資料庫**，`mysql -u root -p`登入自行創建。
    
    ```sql
    create database 資料庫名稱;
    use 資料庫名稱;
    ```
    
- Flask-SQLAlchemy初始設定，在你的__init__.py加入。
    
    ```python
    from flask_sqlalchemy import SQLAlchemy
    ```
    
- Flask-SQLAlchemy 配置，與你的MySQL服務器作連接。
    
    ```python
    app.config['SQLALCHEMY_DATABASE_URI'] = "mysql+pymysql://資料庫使用者名稱:資料庫密碼@ip位置/資料庫名稱"
    db  = SQLAlchemy(app) 
    ```
    

設置完成，`flask run`，若成功運行就是連接成功。

[參考資料](https://medium.com/seaniap/python-web-flask-flask-sqlalchemy%E6%93%8D%E4%BD%9Cmysql%E8%B3%87%E6%96%99%E5%BA%AB-2a799acdec4c)

## Q: Flask-Migrate 如何使用？ #124

Flask-Migrate是Flask中，用於管理DB Migration的擴充模組，可以對資料庫進行版本控制，與SQLAlchemy搭配做使用。

在開發過程中，若是資料庫模型有做更動，都需要對資料庫同步進行更動，Flask-Migrate 可以幫我們自動化同步、更新資料庫，而不需要手動進行操作，並且也可以撤銷所做過的資料庫遷移，與git的概念有些類似。

安裝 Flask-Migrate - `pip install flask-migrate`。

- 初始設定。

```python
from flask_migrate import Migrate
# …
migrate = Migrate(app, db)
```

- 建立第一個遷移資料庫。
    
    初始化資料庫 - `flask db init`，建立遷移資料庫。
    
    建立資料庫遷移腳本 - `flask db migrate -m "更新資訊"`。
    
    更新到最新版本 - `flask db upgrade`。
    
- 資料庫已成功更新、同步。

若想刪除遷移資料庫並重新建立，除了刪除 migrations 資料夾外，資料庫中的 alembic_version 表格也要一併刪除。

## Q: 如何使用 SQLAlchemy 下 Raw SQL？ #125

### SQLAlchemy Core資料庫連接操作

- 引擎配置

`Engine`是資料庫與其 DBAPI 的基礎架構，透過DBAPI與資料庫做連接，在 SQLAlchemy 應用程序上做使用。

```python
# 匯入create_engine()
from sqlalchemy import create_engine
#……
# 建立一個引擎實例，並賦值給 engine
engine = create_engine("資料庫+DBAPI://username:password@host:port/database")
```

- 開始對資料庫進行操作

`engine.connect()` - 用來建立一個**資料庫與應用程式的連接器**。

`execute()` - 連接器用來執行SQL 語句的方法，第二個參數可以是物件，或是物件組成的陣列。

`text()` - 用來創建SQL語言。*你可以把它想像成是SQL翻譯機，翻成python看得懂的*

```python
# 插入數筆資料
with engine.connect() as con:
    data = ( { "id": 1, "title": "The Hobbit", "primary_author": "Tolkien" },
             { "id": 2, "title": "The Silmarillion", "primary_author": "Tolkien" },
    )
    statement = text("""INSERT INTO book(id, title, primary_author) VALUES(:id, :title, :primary_author)""")
    for line in data:
        con.execute(statement, line)
        con.commit()
```

插入資料時如果資料有重複導致錯誤怎麼辦？看看`rollback()`、`commit()`、`close()`的用法。

```python
with engine.connect() as connection:
# 試試插入一筆資料
    try:
        connection.execute(text("INSERT INTO book VALUES (4, 'HAHA')"))
        connection.commit()  # 要記得commit()提交更動，否則不會成功執行
        print("成功")
# 插入失敗、發生異常...回滾(回復)
    except:
        connection.rollback()
        print("失敗")
```

為什麼都沒有使用到`close()`呢？看看下面兩個範例。

```python
# 印出book中所有資料，使用with語句會自動關閉，engine.connect()用完就丟
with engine.connect() as connection:
    result = connection.execute(text("""SELECT * FROM book"""))
    for row in result:
        print (row)
```

```python
# 印出book中所有資料，沒有使用with語句的情況，就要手動把連接口關閉。
result = engine.connect().execute(text("""SELECT * FROM book"""))
for row in result:
    print(row)
engine.connect().close()   # 要自己寫close()
```

## Q: 如何用土炮的方式建立 Table？ #126

### PyMySQL 資料庫庫連接操作

我們知道要與資料庫做連接、查詢操作，需要一個 DBAPI 工具，在 SQLAlchemy 內有包含用於各資料庫的預設 DBAPI。

**PyMySQL 就是 Python3 連接 MySQL 的 DBAPI**，這個範例我們直接使用 PyMySQL 來進行操作。

`pip install PyMySQL` - 安裝 PyMySQL。

`execute()` - 連接器用來執行SQL 語句的方法，與`engine`不同的是，**PyMySQL 可以支援直接寫RAW SQL**，**不需要使用 `text()` 創建**。

`fetchone()` - 讀取一行資料。 

`fetchall()` - 讀取所有資料。 

- 連接前，請確保已經建立好資料庫
- 打開資料庫連接口

```python
# 匯入 pymysql 來做使用
import pymysql
# 使用連接器，打開資料庫與腳本連接口
dbmy = pymysql.connect(host='172.26.243.211',
                       user='root',
	                     password='papy10319',
	                     database='data')
# 使用 cursor() 建立一個連接器游標，要用游標來進行操作
cursor = dbmy.cursor()
```

- 開始對資料庫進行操作

```python
###創建表格
# 執行SQL語句，如果EMPLOYEE已存在則刪除
cursor.execute("DROP TABLE IF EXISTS EMPLOYEE")
# 將建立表格的SQL語句賦值給sql
sql = """CREATE TABLE EMPLOYEE (
         FIRST_NAME  CHAR(20) NOT NULL,
         LAST_NAME  CHAR(20),
         AGE INT,  
         SEX CHAR(1),
         INCOME FLOAT )"""
# 執行 
cursor.execute(sql)
# 執行完所有的動作要記得關閉資料庫連接
dbmy.close()
```

- 運行腳本，如果資料庫有成功連接，你可以發現已經新增了一個表格。

[參考資料](https://www.runoob.com/python3/python3-mysql.html)

## Q: 什麼是密碼雜湊？如何使用 Python 實現？ #129

### 在密碼中加點料

密碼雜湊是一種安全手段，透過在密碼中隨機加入一系列亂碼(加點鹽)，使其轉換為一個長編碼字串，讓別人無法輕易破解我們的使用者或是會員的密碼，並且這些操作是不可逆的，即使有雜湊密碼也無法還原回原本的密碼，因為每次密碼雜湊都是使用不同的料。*加了甚麼料，鹽加多少，只有系統知道*

在python中，可以使用各種套件來進行密碼雜湊的運算，這裡我們使用 Werkzeug 做為範例。

- 開啟python編譯器模擬
- 產生密碼雜湊
    
    `generate_password_hash('密碼')` - 產生密碼雜湊，長編碼字串。
    
    ```python
    *from* werkzeug.security *import* generate_password_hash, check_password_hash
    ```
    
    ```python
    # 第一次產生
    test = generate_password_hash('selena')
    print(test)
    # scrypt:32768:8:1$EJy5QXtYCe9JH56B$df289fe8f70a7a5ebcbdb591f837aad267609d2d32c8e5d3595ecbad734c80b7fa33d2a0d5a98878626967701b0d97c8a5ea872b1ecdd4bac501b8f637e842e7
    # 再產生一次看看，每次產生都不同
    test = generate_password_hash('selena')
    print(test)
    # scrypt:32768:8:1$eCDBBNa60IJbACm7$f68b643f5415bceee2d674027a64d886910b7aed3606166f773821fb57357d929c280eb5125ca1fd962902651022bedcdad47e3d79d186a5474f07e25cf4808e
    ```
    
- 驗證密碼
    
    `check_password_hash('密碼')` - 驗證密碼雜湊，回傳為true或false。
    
    ```python
    check_password_hash(test, 'selena')
    # True
    check_password_hash(test, 'haha')
    # False
    ```