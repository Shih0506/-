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
**sick_basic基本資料表**
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
    <td>PRIMARY KEY</td>
    <td>病患身分證字號</td>
  </tr>
  <tr>
    <td>sick_birth</td>
    <td>DATE</td>
    <td></td>
    <td>病患生日</td>
  </tr>
  <tr>
    <td>sick_blood</td>
    <td>CHAR(2)</td>
    <td>CHECK('A', 'B', 'AB', 'O')</td>
    <td>病患血型(A,B,O,AB)</td>
  </tr>
  <tr>
    <td>sick_name</td>
    <td>VARCHAR(50)</td>
    <td>NOT NULL</td>
    <td>病患姓名</td>
  </tr>
  <tr>
    <td>sick_gender</td>
    <td>CHAR(1)</td>
    <td></td>
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



**sick_register掛號資料表**
<table>
  <tr>
    <td>欄位名稱</td>
    <td>資料型別</td>
    <td>限制條件</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>register_number</td>
    <td>INT</td>
    <td>NOT NULL,AUTO_INCREMENT</td>
    <td>掛號編號</td>
  </tr>
  <tr>
    <td>sick_name</td>
    <td>VARCHAR(50)</td>
    <td>NOT NULL</td>
    <td>病患姓名</td>
  </tr>
  <tr>
    <td>sick_id</td>
    <td>CHAR(10)</td>
    <td> FOREIGN KEY -> sick_register(sick_id)</td>
    <td>病患身分證字號</td>
  </tr>
  <tr>
    <td>doctor_name</td>
    <td>VARCHAR(50)</td>
    <td>NOT NULL</td>
    <td>醫生姓名</td>
  </tr>
  <tr>
    <td>register_data</td>
    <td>DATE</td>
    <td></td>
    <td>就診日期</td>
  </tr>
  <tr>
    <td>register_time</td>
    <td>ENUM('早', '中', '晚')</td>
    <td></td>
    <td>就診時段</td>
  </tr>
</table>

```SQL
CREATE TABLE sick_register (
  register_number INT NOT NULL,AUTO_INCREMENT,
  sick_name VARCHAR(50) NOT NULL,
  sick_id CHAR(10) PRIMARY KEY,
  doctor_name VARCHAR(50) NOT NULL,
  register_data DATE,
  register_time ENUM('早', '中', '晚')
);
```
```
+------------------+----------+------------+------------+----------------+----------------+
| register_number  | sick_name|  sick_id   | doctor_name| register_data  | register_time  |
+------------------+----------+------------+------------+----------------+----------------+
| 1                | 王小明   | A123456789 | 陳建安     | 2025-05-01     | 早             |
| 2                | 李美麗   | B987654321 | 林彥廷     | 2025-05-01     | 中             |
| 3                | 張大華   | C135792468 | 陳建安     | 2025-05-01     | 晚             |
| 4                | 陳志強   | D246813579 | 陳建安     | 2025-05-02     | 早             |
| 5                | 林佳慧   | E112233445 | 林彥廷     | 2025-05-02     | 中             |
| 6                | 黃子豪   | F556677889 | 黃怡君     | 2025-05-02     | 晚             |
| 7                | 周宛如   | G998877665 | 黃怡君     | 2025-05-03     | 早             |
| 8                | 許文婷   | H334455667 | 陳建安     | 2025-05-03     | 中             |
| 9                | 邱志明   | I776655443 | 林彥廷     | 2025-05-03     | 晚             |
| 10               | 洪佩君   | J123789456 | 黃怡君     | 2025-05-04     | 早             |
+------------------+----------+------------+------------+----------------+----------------+
```


**doc_schedule排班資料表**
<table>
  <tr>
    <td>欄位名稱</td>
    <td>資料型別</td>
    <td>限制條件</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>schedule_number</td>
    <td>VARCHAR(20)</td>
    <td>NOT NULL,AUTO_INCREMENT</td>
    <td>排班編號</td>
  </tr>
  <tr>
    <td>doctor_id</td>
    <td>CHAR(10)</td>
    <td> </td>
    <td>醫生id</td>
  </tr>
  <tr>
    <td>doctor_name</td>
    <td>VARCHAR(50)</td>
    <td>NOT NULL</td>
    <td>醫生姓名</td>
  </tr>
  <tr>
    <td>schedule_data</td>
    <td>DATE</td>
    <td></td>
    <td>排班日期</td>
  </tr>
  <tr>
    <td>clinic_room</td>
    <td>VARCHAR(3)</td>
    <td></td>
    <td>診間編號</td>
  </tr>
</table>

```SQL
CREATE TABLE sick_register (
  schedule_number VARCHAR(20) NOT NULL,AUTO_INCREMENT,
  doctor_id CHAR(10) ,
  doctor_name VARCHAR(50) NOT NULL,
  schedule_data DATE,
  clinic_room VARCHAR(3)
);
```
