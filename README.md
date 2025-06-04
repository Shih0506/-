# 題目：診所掛號系統
註冊登入後，病患可以線上掛號、查詢與取消掛號資訊；登入後，前台管理員可修改掛號資訊、填寫基本資料，以及管理醫師排班；登入後，醫護人員可查看掛號紀錄、基本資料。
## 應用情境
1. 病患掛號：病患登入後，可透過系統查詢可掛號醫師及時段，選擇醫師及掛號時段完成掛號。
2. 醫師排班管理：櫃台管理員登入後，可透過系統設定每位醫師的門診時段與可掛號人數。
3. 掛號查詢/取消：病患登入並完成掛號後，可查詢自己目前的掛號紀錄或取消。
4. 報到與診療紀錄管理：櫃台管理員和醫護人員登入後，能查詢當日掛號名單，進行診療紀錄建立。
## 使用案例
* 病患掛號：塵塵因為人不舒服，想透過線上掛號避免現場排隊，確保有無掛號名額。利用身分證號註冊登入後，查找林某某醫師的就診時段，選擇5/1的14：00到17：30，按下確認掛號後，會跳出掛號成功，若該醫師掛號名額已到，會跳出已額滿。
* 醫師排班管理：櫃台盧某某要幫每位醫師排班，使用統一的的帳號密碼登入後，設定林某某醫師5/12的08:30到12:00及18:00到21:30要上班並顯示可掛號人數。
* 掛號查詢/取消：停停掛號成功後，查詢了掛號紀錄，確保他有無掛到號，若要取消該掛號，點選取消鍵。
*  報到與診療紀錄管理：又又線上掛號到現場報到，因為她第一次去填寫了基本資料，櫃台管理員將她的資料打進系統，方便醫師為她整治。
## 資料表
**1.sick_basic基本資料表**
<table>
  <tr>
    <td>欄位名稱</td>
    <td>資料型別</td>
    <td>限制條件</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>sick_id</td>
    <td>CHAR(10)</td>
    <td>PRIMARY KEY , NOT NULL , 第一個數字只可為1或2 , 長度為10</td>
    <td>病患身分證字號</td>
  </tr>
  <tr>
    <td>sick_birth</td>
    <td>DATE</td>
    <td>NOT NULL , 不可填寫未來日期</td>
    <td>病患生日</td>
  </tr>
  <tr>
    <td>sick_blood</td>
    <td>CHAR(2)</td>
    <td>NOT NULL , 只可輸入'A', 'B', 'AB', 'O'</td>
    <td>病患血型(A,B,O,AB)</td>
  </tr>
  <tr>
    <td>sick_name</td>
    <td>string</td>
    <td>NOT NULL , 長度不可超過50字元</td>
    <td>病患姓名</td>
  </tr>
  <tr>
    <td>sick_gender</td>
    <td>CHAR(1)</td>
    <td>只可填寫M或F</td>
    <td>病患性別(男M女F)</td>
  </tr>
</table>

```SQL
CREATE TABLE sick_basic (
  sick_id CHAR(10) PRIMARY KEY,
  sick_birth DATE,
  sick_blood CHAR(2) CHECK (sick_blood IN ('A', 'B', 'AB', 'O')),
  sick_name VARCHAR(50) NOT NULL,
  sick_gender CHAR(1)
);
```
```
+------------+------------+------------+-----------+-------------+
| sick_id    | sick_birth | sick_blood | sick_name| sick_gender |
+------------+------------+------------+-----------+-------------+
| A123456789 | 1990-06-15 | A          | 王小明    | M           |
| B987654321 | 1985-11-23 | B          | 李美麗    | F           |
| C135792468 | 2000-01-30 | O          | 張大華    | M           |
| D246813579 | 1992-08-05 | AB         | 陳志強    | M           |
| E112233445 | 1998-12-12 | A          | 林佳慧    | F           |
| F556677889 | 1987-04-08 | O          | 黃子豪    | M           |
| G998877665 | 1995-03-22 | B          | 周宛如    | F           |
| H334455667 | 1993-07-17 | AB         | 許文婷    | F           |
| I776655443 | 1989-09-10 | A          | 邱志明    | M           |
| J123789456 | 1997-05-03 | O          | 洪佩君    | F           |
+------------+------------+------------+-----------+-------------+
```

**2.sick_register掛號資料表**
<table>
  <tr>
    <td>欄位名稱</td>
    <td>資料型別</td>
    <td>限制條件</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>register_number</td>
    <td>INTEGER</td>
    <td>NOT NULL , AUTO_INCREMENT , PRIMARY KEY</td>
    <td>掛號編號</td>
  </tr>
  <tr>
    <td>sick_id</td>
    <td>CHAR(10)</td>
    <td>NOT NULL , FOREIGN KEY -> sick_register(sick_id) , 第一個數字只可為1或2，長度為10</td>
    <td>病患身分證字號</td>
  </tr>
  <tr>
    <td>doctor_name</td>
    <td>string</td>
    <td>NOT NULL , 長度不可超過50字元</td>
    <td>醫生姓名</td>
  </tr>
  <tr>
    <td>register_data</td>
    <td>DATE</td>
    <td>NOT NULL , 不可填寫過去日期</td>
    <td>就診日期</td>
  </tr>
  <tr>
    <td>register_time</td>
    <td>CHAR(1)</td>
    <td>NOT NULL , 只可填寫'早', '中', '晚'</td>
    <td>就診時段</td>
  </tr>
</table>

```SQL
CREATE TABLE sick_register (
  register_number INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  sick_id CHAR(10) ,
  doctor_name VARCHAR(50) NOT NULL,
  register_data DATE,
  register_time ENUM('早', '中', '晚')
);
```
```
+------------------+------------+------------+----------------+----------------+
| register_number  |  sick_id   | doctor_name| register_data  | register_time  |
+------------------+------------+------------+----------------+----------------+
| 1                | A123456789 | 陳建安     | 2025-05-01     | 早             |
| 2                | B287654321 | 林彥廷     | 2025-05-01     | 中             |
| 3                | C135792468 | 陳建安     | 2025-05-01     | 晚             |
| 4                | D146813579 | 陳建安     | 2025-05-02     | 早             |
| 5                | E212233445 | 林彥廷     | 2025-05-02     | 中             |
| 6                | F156677889 | 黃怡君     | 2025-05-02     | 晚             |
| 7                | G298877665 | 黃怡君     | 2025-05-03     | 早             |
| 8                | H234455667 | 陳建安     | 2025-05-03     | 中             |
| 9                | I176655443 | 林彥廷     | 2025-05-03     | 晚             |
| 10               | J223789456 | 黃怡君     | 2025-05-04     | 早             |
+------------------+------------+------------+----------------+----------------+
```


**3.doc_schedule排班資料表**
<table>
  <tr>
    <td>欄位名稱</td>
    <td>資料型別</td>
    <td>限制條件</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>schedule_number</td>
    <td>INTEGER</td>
    <td>NOT NULL , AUTO_INCREMENT , PRIMARY KEY</td>
    <td>排班編號</td>
  </tr>
  <tr>
    <td>doctor_id</td>
    <td>CHAR(10)</td>
    <td>NOT NULL</td>
    <td>醫生id</td>
  </tr>
  <tr>
    <td>doctor_name</td>
    <td>string</td>
    <td>NOT NULL , 長度不可超過50字元</td>
    <td>醫生姓名</td>
  </tr>
  <tr>
    <td>schedule_data</td>
    <td>DATE</td>
    <td>NOT NULL , 不可填寫過去日期</td>
    <td>排班日期</td>
  </tr>
  <tr>
    <td>clinic_room</td>
    <td>INTEGER</td>
    <td>NOT NULL , 只能填寫'101', '102', '103', FOREIGN KEY -> clinicroom_basic(clinic_room) , 若診間設備為'N'則不可填寫</td>
    <td>診間編號</td>
  </tr>
</table>

```SQL
CREATE TABLE sick_register (
  schedule_number INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  doctor_id CHAR(10) NOT NULL,
  doctor_name VARCHAR(50) NOT NULL,
  schedule_data DATE NOT NULL,
  clinic_room ENUM('101', '102', '103') NOT NULL
);
```
```
+------------------+------------+--------------+----------------+-------------+
| schedule_number  | doctor_id  | doctor_name  | schedule_data  | clinic_room |
+------------------+------------+--------------+----------------+-------------+
| 1                | D123456789 | 陳建安       | 2025-05-14     | 101         |
| 2                | D987654321 | 林彥廷       | 2025-05-14     | 102         |
| 3                | D112233445 | 黃怡君       | 2025-05-14     | 103         |
| 4                | D123456789 | 陳建安       | 2025-05-15     | 101         |
| 5                | D987654321 | 林彥廷       | 2025-05-15     | 102         |
| 6                | D112233445 | 黃怡君       | 2025-05-15     | 103         |
| 7                | D123456789 | 陳建安       | 2025-05-16     | 101         |
| 8                | D987654321 | 林彥廷       | 2025-05-16     | 102         |
| 9                | D112233445 | 黃怡君       | 2025-05-16     | 103         |
| 10               | D123456789 | 陳建安       | 2025-05-17     | 101         |
+------------------+------------+--------------+----------------+-------------+
```
**3.clinic_room_basic診間基本資料表**
<table>
  <tr>
    <td>欄位名稱</td>
    <td>資料型別</td>
    <td>限制條件</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>clinic_room</td>
    <td>INTEGER</td>
    <td>NOT NULL , 只能填寫'101', '102', '103' , PRIMARY KEY</td>
    <td>診間編號</td>
  </tr>
  <tr>
    <td>device</td>
    <td>ENUM('Y', 'N')</td>
    <td>NOT NULL,只能填寫'Y', 'N'</td>
    <td>設備好壞</td>
  </tr>
</table>

```SQL
CREATE TABLE sick_register (
  clinic_room ENUM('101', '102', '103') NOT NULL PRIMARY KEY,
  device ENUM('Y', 'N') NOT NULL
);
```
```
+-------------+--------+
| clinic_room | device |
+-------------+--------+
| 101         | Y      |
| 102         | N      |
| 103         | Y      |
+-------------+--------+
```

## ER diagram
![ER_giagram](https://github.com/user-attachments/assets/66e3bddc-256f-4477-8b34-eef7cf8d6e4f)

## DBMS ER diagram
