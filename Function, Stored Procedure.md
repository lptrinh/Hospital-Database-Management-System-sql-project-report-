### 1. Tăng phí nhập viện lên 10% cho tất cả các bệnh nhân nội trú hiện tại nhập viện sau ngày 01/09/2020.
```
UPDATE INPATIENT
SET IPFee = IPFee * 1.1
WHERE IPAdDate > date('2020-09-01');
```
### 2. Lựa chọn tất cả các bệnh nhân (nội trú và ngoại trú) của bác sĩ tên “Stephen Strange”.
```
SELECT concat(O.PFiName, ' ' , O.PLName) AS FullName, concat(O.PChar, O.PNumber) AS IP
FROM OUTPATIENT O
WHERE O.DocCode = (SELECT ECode FROM DOCTOR WHERE concat(EFiName, ' ' , ELName) = 'Stephen Strange')
UNION
SELECT concat(I.PFiName, ' ' , I.PLName) AS FullName, concat(I.PChar, I.PNumber) AS IP
FROM INPATIENT I, (SELECT DISTINCT PNumber FROM TREATMENT WHERE DocCode = (SELECT ECode FROM DOCTOR WHERE concat(EFiName, ' ' , ELName) = 'Stephen Strange')) as T
WHERE I.PNumber = T.PNumber;
```
### 3. Viết một function tính tổng tiền thuốc một bệnh phân phải trả cho mỗi lần điều trị hoặc khám bệnh. 
Trả về tổng số tiền
```
DELIMETER $$
CREATE FUNCTION IF NOT EXISTS TOTAL_FEE(Pid VARCHAR(11))
RETURNS NUMERIC(15,4)
DETERMINISTIC
BEGIN
DECLARE total NUMERIC(15,4);
IF SUBSTR(Pid, 1, 2) = 'IP' THEN
SELECT SUM(IPFee) INTO total FROM INPATIENT I, TREATMENT T WHERE I.PNumber = SUBSTR(Pid, 3, 9) AND I.PNumber = T.PNumber;
ELSEIF SUBSTR(Pid, 1, 2) = 'OP' THEN
SELECT SUM(EXFee) INTO total FROM EXAMINATION E WHERE E.PNumber = SUBSTR(Pid, 3, 9);
END IF;
RETURN total;
END $$
DELIMETER;
```

Thông tin hàm
```
SHOW FUNCTION STATUS
WHERE db = 'hospital';
```

Chạy hàm
```
SELECT hospital.TOTAL_FEE('IP000000000');
SELECT hospital.TOTAL_FEE('IP000000001');
SELECT *, hospital.TOTAL_FEE(concat(PChar, PNumber)) AS TotalFee FROM OUTPATIENT;
```
### 4. Viết một procedure để sắp xếp các bác sĩ theo thứ tự tăng dần số bệnh nhân mà họ tiếp nhận trong một khoảng thời gian.
* **Đầu vào**: Ngày bắt đầu, ngày kết thúc
* **Đầu ra**: Một danh sách các bác sĩ đã được sắp xếp.
