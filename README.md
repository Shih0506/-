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
## 系統需求說明
使用者 \r
註冊與登入
掛號預約功能(醫師、時段)
掛號紀錄查詢與取消
醫護人員
登入
掛號紀錄查詢與取消
查看基本資料
櫃台管理員
登入
醫師排班
手動掛號
填寫基本資料
## ER diagram
![image](https://github.com/user-attachments/assets/6fcc5a4a-b66b-4be3-9697-4abf2a40fcfa)

## DBMS ER diagram
![drawSQL-image-export-2025-06-04](https://github.com/user-attachments/assets/9a30530b-e4e9-42dc-9c49-a73f61cac90b)

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
    <td>PRIMARY KEY , NOT NULL</td>
    <td>病患身分證字號</td>
  </tr>
  <tr>
    <td>sick_birth</td>
    <td>DATE</td>
    <td>NOT NULL</td>
    <td>病患生日</td>
  </tr>
  <tr>
    <td>sick_blood</td>
    <td>CHAR(2)</td>
    <td>NOT NULL</td>
    <td>病患血型(A,B,O,AB)</td>
  </tr>
  <tr>
    <td>sick_name</td>
    <td>STRING</td>
    <td>NOT NULL</td>
    <td>病患姓名</td>
  </tr>
  <tr>
    <td>sick_gender</td>
    <td>CHAR(1)</td>
    <td>只可填寫M或F</td>
    <td>病患性別(男M女F)</td>
  </tr>
</table>

<table>
  <tr>
    <td>欄位名稱</td>
    <td>完整性限制</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>sick_id</td>
    <td>第一個字元只必為大寫字母，第二個字元則是1或2 , 總長度10</td>
    <td>病患身分證字號</td>
  </tr>
  <tr>
    <td>sick_birth</td>
    <td>日期格式為 yyyy-mm-dd ， 不可填寫未來日期</td>
    <td>病患生日</td>
  </tr>
  <tr>
    <td>sick_blood</td>
    <td>只可輸入'A', 'B', 'AB', 'O' ， 不能含有數字、特殊符號、除這四種英文字組外的英文出現</td>
    <td>病患血型</td>
  </tr>
  <tr>
    <td>sick_name</td>
    <td>長度不可超過50字元 ， 不包含特殊符號</td>
    <td>病患姓名</td>
  </tr>
  <tr>
    <td>sick_gender</td>
    <td>只可填寫`M`或`F` ， 不能含有數字、特殊符號、除這兩種英文字外的英文出現</td>
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
    <td>CHAR(10)</td>
    <td> PRIMARY KEY</td>
    <td>掛號編號</td>
  </tr>
  <tr>
    <td>sick_id</td>
    <td>CHAR(10)</td>
    <td>NOT NULL , FOREIGN KEY -> sick_register(sick_id) </td>
    <td>病患身分證字號</td>
  </tr>
  <tr>
    <td>doctor_name</td>
    <td>STRING</td>
    <td>NOT NULL </td>
    <td>醫生姓名</td>
  </tr>
  <tr>
    <td>register_date</td>
    <td>DATE</td>
    <td>NOT NULL </td>
    <td>就診日期</td>
  </tr>
  <tr>
    <td>register_time</td>
    <td>CHAR(1)</td>
    <td>NOT NULL </td>
    <td>就診時段</td>
  </tr>
</table>

<table>
  <tr>
    <td>欄位名稱</td>
    <td>完整性限制</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>register_number</td>
    <td>格式為前六碼為日期，後兩碼為大於0小於100的雙數編號</td>
    <td>掛號編號</td>
  </tr>
  <tr>
    <td>sick_id</td>
    <td>第一個字元只可為大寫字母，第二個字元則是1或2 , 總長度10</td>
    <td>病患身分證字號</td>
  </tr>
  <tr>
    <td>doctor_name</td>
    <td>長度不可超過50字元 ， 不包含特殊符號</td>
    <td>醫生姓名</td>
  </tr>
  <tr>
    <td>register_date</td>
    <td>格式為 yyyy-mm-dd ， 不可填寫過去日期</td>
    <td>就診日期</td>
  </tr>
  <tr>
    <td>register_time</td>
    <td>只可填寫'早', '中', '晚' ， 不能含有數字、特殊符號、除這三種文字外的字出現</td>
    <td>就診時段</td>
  </tr>
</table>


```SQL
CREATE TABLE sick_register (
  register_number CHAR(10)  PRIMARY KEY,
  sick_id CHAR(10) ,
  doctor_name VARCHAR(50) NOT NULL,
  register_data DATE,
  register_time ENUM('早', '中', '晚')
);
```

建立 Trigger 自動產生 register_number
```SQL
DELIMITER $$

CREATE TRIGGER before_insert_sick_register
BEFORE INSERT ON sick_register
FOR EACH ROW
BEGIN
  DECLARE reg_prefix CHAR(8);
  DECLARE next_suffix CHAR(2);
  DECLARE max_suffix INT;

  -- 建立 YYYYMMDD 字串
  SET reg_prefix = DATE_FORMAT(NEW.register_data, '%Y%m%d');

  -- 取出今天的最大後兩碼數字
  SELECT MAX(CAST(RIGHT(register_number, 2) AS UNSIGNED))
  INTO max_suffix
  FROM sick_register
  WHERE LEFT(register_number, 8) = reg_prefix;

  -- 若今天沒有掛號過，就從 2 開始，否則下一個雙數
  IF max_suffix IS NULL THEN
    SET next_suffix = '02';
  ELSE
    SET max_suffix = max_suffix + 2;

    -- 若超過 98，報錯（最多 49 筆）
    IF max_suffix > 98 THEN
      SIGNAL SQLSTATE '45000'
      SET MESSAGE_TEXT = '當日掛號已滿（最大為 98）';
    END IF;

    SET next_suffix = LPAD(max_suffix, 2, '0');
  END IF;

  -- 合併成完整 register_number
  SET NEW.register_number = CONCAT(reg_prefix, next_suffix);
END$$

DELIMITER ;
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
    <td>CHAR(8)</td>
    <td>NOT NULL , FOREIGN KEY -> doctor_basic(doctor_id) </td>
    <td>醫生id</td>
  </tr>
  <tr>
    <td>schedule_date</td>
    <td>DATE</td>
    <td>NOT NULL</td>
    <td>排班日期</td>
  </tr>
  <tr>
    <td>clinic_room</td>
    <td>INTEGER</td>
    <td>NOT NULL , FOREIGN KEY -> clinicroom_basic(clinic_room) </td>
    <td>診間編號</td>
  </tr>
</table>

<table>
  <tr>
    <td>欄位名稱</td>
    <td>完整性限制</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>schedule_number</td>
    <td>系統依照順序編號編號，從1開始的整數</td>
    <td>排班編號</td>
  </tr>
  <tr>
    <td>doctor_id</td>
    <td>只可為八碼的數字</td>
    <td>醫生id</td>
  </tr>
  <tr>
    <td>schedule_date</td>
    <td>格式為 yyyy-mm-dd ， 不可填寫過去日期</td>
    <td>排班日期</td>
  </tr>
  <tr>
    <td>clinic_room</td>
    <td>只能填寫'101', '102', '103' ， 不能含有數字、特殊符號、除這三種數組外的數組出現 , 系統若查詢到診間設備為'N'則不可填寫</td>
    <td>診間編號</td>
  </tr>
</table>

```SQL
CREATE TABLE sick_register (
  schedule_number INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  doctor_id CHAR(8) NOT NULL,
  schedule_date DATE NOT NULL,
  clinic_room ENUM('101', '102', '103') NOT NULL
);
```
```
+------------------+------------+----------------+-------------+
| schedule_number  | doctor_id  | schedule_data  | clinic_room |
+------------------+------------+----------------+-------------+
| 1                | 23456789   | 2025-05-14     | 101         |
| 2                | 87654321   | 2025-05-14     | 102         |
| 3                | 12233445   | 2025-05-14     | 103         |
| 4                | 23456789   | 2025-05-15     | 101         |
| 5                | 87654321   | 2025-05-15     | 102         |
| 6                | 12233445   | 2025-05-15     | 103         |
| 7                | 23456789   | 2025-05-16     | 101         |
| 8                | 87654321   | 2025-05-16     | 102         |
| 9                | 12233445   | 2025-05-16     | 103         |
| 10               | 23456789   | 2025-05-17     | 101         |
+------------------+------------+----------------+-------------+
```
**4.clinic_room_basic診間基本資料表**
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
    <td>NOT NULL , PRIMARY KEY</td>
    <td>診間編號</td>
  </tr>
  <tr>
    <td>device</td>
    <td>CHAR(1)</td>
    <td>NOT NULL</td>
    <td>設備好壞</td>
  </tr>
</table>

<table>
  <tr>
    <td>欄位名稱</td>
    <td>完整性限制</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>clinic_room</td>
    <td>為'101', '102', '103'</td>
    <td>診間編號</td>
  </tr>
  <tr>
    <td>device</td>
    <td>只能填寫'Y', 'N' ，  不能含有數字、特殊符號、除這兩種英文外的字母出現</td>
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
**5.doctor_basic基本資料表**
<table>
  <tr>
    <td>欄位名稱</td>
    <td>資料型別</td>
    <td>限制條件</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>doctor_id</td>
    <td>CHAR(8)</td>
    <td>NOT NULL , PRIMARY KEY</td>
    <td>醫師編號</td>
  </tr>
  <tr>
    <td>doctor_name</td>
    <td>STRING</td>
    <td>NOT NULL</td>
    <td>醫生姓名</td>
  </tr>
</table>

<table>
  <tr>
    <td>欄位名稱</td>
    <td>完整性限制</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>doctor_id</td>
    <td>只可為八碼的數字</td>
    <td>醫師編號</td>
  </tr>
  <tr>
    <td>doctor_name</td>
    <td>長度不可超過50字元 ， 不包含特殊字元</td>
    <td>醫生姓名</td>
  </tr>
</table>

```SQL
CREATE TABLE doctor_register (
  doctor_id CHAR(8) NOT NULL PRIMARY KEY,
  doctor_name VARCHAR(50) NOT NULL
);
```
```
+------------+------------+
| doctor_id  | doctor_name|
+------------+------------+
| 92025001   | 陳建安     |
| 62025002   | 林彥廷     |
| 22025003   | 黃怡君     |
+------------+------------+
```
## VIEW表設計
a.一般使用者
| 名稱                     | 選擇的屬性                                                                 | 說明                                                                 |
|--------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------|
| 查詢個人掛號記錄         | `sick_register(register_number, doctor_name, register_data, register_time)` | 病患可以查詢自己的掛號記錄，包含掛號號碼、醫師姓名、掛號日期及時間 |
| 查詢可預約的醫師與時段   | `sick_register(doctor_name, register_data, register_time, remaining_slots)` | 病患可以查詢目前可預約的醫師與時段，包含排班號碼、醫師姓名、日期及診間 |
| 查詢個人基本資料         | `sick_basic(sick_id, sick_name, sick_gender, sick_birth, sick_blood)`     | 病患可以查看自己的基本資料，包含姓名、性別、生日及血型             |

**查詢個人掛號記錄**
```sql
-- 查詢個人掛號記錄
CREATE VIEW personal_register_history_view AS
SELECT 
  r.register_number,
  r.sick_id,
  b.sick_name,
  b.sick_gender,
  b.sick_birth,
  b.sick_blood,
  r.doctor_name,
  r.register_data,
  r.register_time
FROM sick_register r
JOIN sick_basic b ON r.sick_id = b.sick_id;
SELECT * 
FROM personal_register_history_view 
WHERE sick_id = 'A123456789';
```

**查詢可預約的醫師與時段**
```sql
-- 查詢可預約的醫師與時段
CREATE VIEW available_doctor_schedule_view AS
SELECT 
  ds.schedule_number,
  ds.doctor_id,
  ds.doctor_name,
  ds.schedule_date,
  ds.schedule_time,
  ds.clinic_room,
  33 - COUNT(sr.register_number) AS available_slots
FROM doctor_schedule ds
LEFT JOIN sick_register sr
  ON ds.doctor_id = sr.doctor_id
  AND ds.schedule_date = sr.register_date
  AND ds.schedule_time = sr.register_time
GROUP BY 
  ds.schedule_number,
  ds.doctor_id,
  ds.doctor_name,
  ds.schedule_date,
  ds.schedule_time,
  ds.clinic_room
HAVING available_slots > 0;
```

**查詢個人基本資料**
```sql
-- 查詢個人基本資料
CREATE VIEW user_basic_info_view AS
SELECT
  sick_id,
  sick_name,
  sick_gender,
  sick_birth,
  sick_blood
FROM sick_basic;
SELECT * FROM user_basic_info_view
WHERE sick_id = 'A123456789';
```

b.管理員
| 名稱                     | 選擇的屬性                                                                 | 說明                                                                 |
|--------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------|
| 系統內今日掛號單         | `sick_register(sick_id, register_data), sick_basic(sick_id, sick_name, sick_gender, sick_birth, sick_blood)	` | 管理者可以查詢到當日掛號單資訊，包含掛號號碼、病患ID、醫師姓名、掛號日期及時間 |
| 今日醫生排班表           | `sick_register(schedule_number, doctor_id, doctor_name, schedule_data, clinic_room)` | 查詢今日醫生排班表，包含排班號碼、醫師ID、醫師姓名、排班日期及診間 |
| 管理者透過今日掛號單查詢患者個人資料 | `sick_basic(sick_id, sick_name, sick_gender, sick_birth, sick_blood)` | 管理者可透過掛號單以病患身分字號查詢個人資料，包含姓名、性別、生日及血型 |
| 統計每位醫師今日看診數   | `sick_register(doctor_name, register_data), COUNT(*) AS pending_patient_count`            | 統計每位醫師今日看診人數                                         |
| 查出同一病患在同一天是否重複掛號 | `sick_register(sick_id, register_data), COUNT(*) AS count_per_day` | 查出同一病患在同一天是否重複掛號，若次數大於1則顯示                |


**系統內今日掛號單**
```sql
-- 系統內今日掛號單
CREATE VIEW sick_register_today AS
SELECT 
  register_number, 
  sick_id, 
  doctor_name, 
  register_data, 
  register_time
FROM sick_register
WHERE register_data = CURDATE();
```

**今日醫生排班表**
```sql
-- 今日醫生排班表
CREATE VIEW today_doctor_schedule_view AS
SELECT 
  schedule_number,
  doctor_id,
  doctor_name,
  schedule_data,
  clinic_room
FROM sick_register
WHERE schedule_data = CURDATE();
```
**管理者透過今日掛號單查詢患者個人資料**
```sql
-- 管理者透過今日掛號單查詢患者個人資料
CREATE VIEW today_sick_basic_info_view AS
SELECT 
  b.sick_id,
  b.sick_name,
  b.sick_gender,
  b.sick_birth,
  b.sick_blood
FROM sick_register r
JOIN sick_basic b ON r.sick_id = b.sick_id
WHERE r.register_data = CURDATE();
```

**統計每位醫師今日看診數**
```sql
-- 統計每位醫師今日看診數
CREATE VIEW today_doctor_pending_patients_view AS
SELECT 
  doctor_name,
  COUNT(*) AS pending_patient_count
FROM sick_register
WHERE register_data = CURDATE()
GROUP BY doctor_name;
SELECT * FROM today_doctor_pending_patients_view
```

**查出同一病患在同一天是否重複掛號**
```sql
-- 查出同一病患在同一天是否重複掛號
CREATE VIEW duplicate_register_check_view AS
SELECT 
  sick_id,
  register_data,
  COUNT(*) AS count_per_day
FROM sick_register
GROUP BY sick_id, register_data
HAVING COUNT(*) > 1;
```


