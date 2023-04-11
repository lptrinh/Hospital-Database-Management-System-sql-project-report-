Tôi sử dụng PostgreSQL trong dự án này
### Bảng DOCTOR
```
CREATE TABLE IF NOT EXISTS DOCTOR 
(
ECode DECIMAL(9, 0) ZEROFILL PRIMARY KEY,
EFiName VARCHAR(15) NOT NULL,
ELName VARCHAR(15) NOT NULL,
EGender CHAR,
EDoB DATE,
EAddress VARCHAR(50),
ESDate DATE,
SpName VARCHAR(15) NOT NULL,
SpYear DATE,
DepCode INT NOT NULL,
CONSTRAINT doctor_gender_check CHECK (EGender IN ('M', 'F'))
); 
```
Trong đó:
* ECode là mã nhân viên gồm 9 chữ số, ví dụ “000000001”. Lựa chọn kiểu dữ liệu **DECIMAL(9, 0) ZERO FILL**. Đây cũng là **PRIMARY KEY** của bảng.
* EFiName và ELName là tên và họ của bác sĩ, có thể là một chuỗi bất kỳ trong giới hạn 15 ký
tự. Lựa chọn kiểu dữ liệu **VARCHAR(15)** cho hai trường này. Họ và tên của
bác sĩ cũng được yêu cầu **NOT NULL**.
* EGender là giới tính của bác sĩ. Kiểu dữ liệu là **CHAR** và chỉ chấp nhận giá trị **M** hoặc **F**. Ràng
buộc này được thực thi bởi **CONSTRAINT doctor_gender_check**.
* EDoB là ngày sinh của bác sĩ. Vì vậy, kiểu dữ liệu ở đây là **DATE**.
* EAddress là địa chỉ của bác sĩ, có thể là một chuỗi trong giới hạn 50 ký tự. Lựa chọn kiểu dữ liệu **VARCHAR(50)**.
* ESDate là ngày bắt đầu làm việc của bác sĩ, llựa chọn kiểu dữ liệu là **DATE**.
* SpName là lĩnh vực chuyên môn của bác sĩ, là một chuỗi bất kỳ trong giới hạn 15 ký tự. Lựa chọn kiểu dữ liệu **VARCHAR(15)**. Một bác sĩ phải có một chuyên khoa cụ
thể, nên trường này phải **NOT NULL**.
* SpYear là năm nhận bằng chuyên môn của bác sĩ. Lựa chọn kiểu dữ liệu **DATE** để
có thể dễ dàng tính số năm so với hiện tại bằng hàm **TIMESTAMPDIFF()**. Do bác sĩ phải có
bằng chuyên khoa nên năm nhận bằng của họ cũng phải là **NOT NULL**.
* DepCode là mã phòng ban mà bác sĩ thuộc về, là kiểu **INT** và **NOT NULL** (một bác sĩ phải
thuộc về một phòng ban cụ thể).
* **CONSTRAINT doctor_gender_check**, như đã giải thích ở phần EGender, dùng để áp dụng
ràng buộc rằng trường này chỉ nhận 2 giá trị **M** và **F**.

### Bảng NURSE
```
CREATE TABLE IF NOT EXISTS NURSE 
(
ECode DECIMAL(9, 0) ZEROFILL PRIMARY KEY,
EFiName VARCHAR(15) NOT NULL,
ELName VARCHAR(15) NOT NULL,
EGender CHAR,
EDoB DATE,
EAddress VARCHAR(50),
ESDate DATE,
SpName VARCHAR(15) NOT NULL,
SpYear DATE NOT NULL,
DepCode INT NOT NULL,
CONSTRAINT nurse_gender_check CHECK (EGender IN ('M', 'F'))
); 
```
Trong đó:
* ECode là mã nhân viên gồm 9 chữ số, ví dụ “000000001”. Lựa chọn kiểu dữ liệu **DECIMAL(9, 0) ZERO FILL**. Đây cũng là **PRIMARY KEY** của bảng.
* EFiName và ELName là tên và họ của y tá, có thể là một chuỗi bất kỳ trong giới hạn 15 ký
tự. Lựa chọn kiểu dữ liệu **VARCHAR(15)** cho hai trường này. Họ và tên của
y tá cũng được yêu cầu **NOT NULL**.
* EGender là giới tính của y tá. Kiểu dữ liệu là **CHAR** và chỉ chấp nhận giá trị **M** hoặc **F**. Ràng
buộc này được thực thi bởi **CONSTRAINT nurse_gender_check**.
* EDoB là ngày sinh của y tá. Vì vậy, kiểu dữ liệu ở đây là **DATE**.
* EAddress là địa chỉ của y tá, có thể là một chuỗi trong giới hạn 50 ký tự. Lựa chọn kiểu dữ liệu **VARCHAR(50)**.
* ESDate là ngày bắt đầu làm việc của y tá, llựa chọn kiểu dữ liệu là **DATE**.
* SpName là lĩnh vực chuyên môn của y tá, là một chuỗi bất kỳ trong giới hạn 15 ký tự. Lựa chọn kiểu dữ liệu **VARCHAR(15)**. Một y tá phải có một chuyên khoa cụ
thể, nên trường này phải **NOT NULL**.
* SpYear là năm nhận bằng chuyên môn của y tá. Lựa chọn kiểu dữ liệu **DATE** để
có thể dễ dàng tính số năm so với hiện tại bằng hàm **TIMESTAMPDIFF()**. Do y tá phải có
bằng chuyên khoa nên năm nhận bằng của họ cũng phải là **NOT NULL**.
* DepCode là mã phòng ban mà y tá thuộc về, là kiểu **INT** và **NOT NULL** (một y tá phải
thuộc về một phòng ban cụ thể).
* **CONSTRAINT nurse_gender_check**, như đã giải thích ở phần EGender, dùng để áp dụng
ràng buộc rằng trường này chỉ nhận 2 giá trị **M** và **F**.

### Trigger cho hai bảng DOCTOR và NURSE
Cả bác sĩ và y tá đều là nhân viên, do đó ECode của họ phải là độc nhất trong cả 2 bảng. Để
thực thi điều này ta có thể sử dụng Trigger: <br />
Stored Procedure được định nghĩa như một tập các khai báo sql được lưu trữ ngay trong cơ sở dữ liệu (database) và sau đó
, được triệu gọi bởi một program, một trigger hay thậm chí là một stored procedure khác.<br />
Gán giá trị thông qua câu lệnh SELECT.<br />
 Trigger để thực thi yêu cầu ECode độc nhất giữa 2 bảng DOCTOR và NURSE:
```
DROP TRIGGER IF EXISTS unique_ecode_doctor;
DROP TRIGGER IF EXISTS unque_ecode_nurse;
/*ECode must be unique among doctors and nurses*/
DELIMITER //
CREATE TRIGGER unique_ecode_doctor BEFORE INSERT ON DOCTOR
FOR EACH ROW BEGIN
DECLARE c INT;
SELECT COUNT(*) INTO c FROM NURSE WHERE ECode = NEW.ECode;
IF (c > 0) THEN
SET new.ECode = NULL;
END IF;
END //
CREATE TRIGGER unique_ecode_nurse BEFORE INSERT ON NURSE
FOR EACH ROW BEGIN
DECLARE c INT;
SELECT COUNT(*) INTO c FROM DOCTOR WHERE ECode = NEW.ECode;
IF (c > 0) THEN
SET new.ECode = NULL;
END IF;
END //
DELIMITER ;
```
### Thêm dữ liệu vào hai bảng DOCTOR và NURSE
```
INSERT INTO DOCTOR
VALUES (0, 'Stephen', 'Strange', 'M', '1930-11-18', 'New York', '2020-01-04',
'Neurosurgery', '2001-01-01', 0);
INSERT INTO DOCTOR
VALUES (1, 'Kenzo', 'Tenma', 'M', '1958-01-02', 'Dusseldorf', '2003-05-05',
'Neurosurgery', '1986-01-01', 0);
INSERT INTO DOCTOR
VALUES (2, 'To Hang', 'Tu', 'F', '2001-03-02', 'Ho Chi Minh', '2022-11-20',
'Medicine', '2015-01-01', 1);
INSERT INTO DOCTOR
VALUES (3, 'Tran Hieu Nghia', 'Tang', 'M', '2000-01-02', 'Ho Chi Minh', '2019-
12-12', 'Cardiology', '2015-01-01', 2);
INSERT INTO DOCTOR
VALUES (4, 'Doc', 'Q', 'M', '1945-09-09', 'Queensland', '2001-09-04',
'Virology', '1999-01-01', 3);
INSERT INTO DOCTOR
VALUES (5, 'Tony', 'Chopper', 'M', '1990-06-18', 'Drum', '1995-07-30',
'Medicine', '1992-01-01', 1);
INSERT INTO NURSE
VALUES (6, 'Christine', 'Palmer', 'F', '1931-12-24', 'New York', '2020-01-04',
'Neurosurgery', '2003-01-01', 0);
INSERT INTO NURSE
VALUES (7, 'Joyce', 'Joy', 'F', '1996-06-06', 'Tokyo', '2005-12-01', 'Virology',
'2005-01-01', 3);
INSERT INTO NURSE
VALUES (8, 'Katara', 'Aang', 'F', '1997-11-24', 'Iceland', '2003-02-28',
'Medicine', '2003-01-01', 1);
INSERT INTO NURSE
VALUES (9, 'Asuna', 'Karino', 'F', '2001-05-05', 'Chiba', '2018-03-31',
'Virology', '2021-01-01', 3);
```
### Tạo bảng DOCTOR_PHONE
```
CREATE TABLE IF NOT EXISTS DOCTOR_PHONE
(
DCode DECIMAL(9, 0) ZEROFILL NOT NULL,
Phone DECIMAL(10, 0) ZEROFILL NOT NULL UNIQUE,
PRIMARY KEY(DCode,Phone),
CONSTRAINT fk_docphone_doc_dcode FOREIGN KEY(DCode)
REFERENCES DOCTOR(ECode)
ON DELETE CASCADE
);
```
Trong đó:
* DCode là mã nhân viên của bác sĩ, tương tự như trong bảng DOCTOR là kiểu
**DECIMAL(9, 0) ZERO FILL**.
* Phone là số điện thoại của bác sĩ, là một dãy số 10 ký tự, nếu ít hơn thì các số 0 sẽ được
thêm vào bên trái. Do đó kiểu dữ liệu ở đây sẽ là **DECIMAL(10, 0) ZEROFILL**. Trường này
phải **NOT NULL** và không được trùng nhau **UNIQUE**.
* Tổ hợp (DCode, Phone) đóng vai trò là **PRIMARY KEY** của bảng, với ràng buộc
**fk_docphone_doc_dcode** rằng DCode là khóa ngoại tham khảo đến ECode của bảng
DOCTOR. Khi Ecode bị xóa khỏi bảng DOCTOR thì tổ hợp tương ứng ở bảng
DOCTOR_PHONE cũng bị xóa **ON DELETE CASCADE**.

### Tạo bảng NURSE_PHONE
```
CREATE TABLE IF NOT EXISTS NURSE_PHONE
(
NCode DECIMAL(9, 0) ZEROFILL NOT NULL,
Phone DECIMAL(10, 0) ZEROFILL NOT NULL UNIQUE,
PRIMARY KEY(NCode,Phone),
CONSTRAINT fk_nursephone_nurse_ncode FOREIGN KEY(NCode)
REFERENCES NURSE(ECode)
ON DELETE CASCADE
);
```
Trong đó:
* NCode là mã nhân viên của y tá, tương tự như trong bảng DOCTOR là kiểu
**DECIMAL(9, 0) ZERO FILL**.
* Phone là số điện thoại của y tá, là một dãy số 10 ký tự, nếu ít hơn thì các số 0 sẽ được
thêm vào bên trái. Do đó kiểu dữ liệu ở đây sẽ là **DECIMAL(10, 0) ZEROFILL**. Trường này
phải **NOT NULL** và không được trùng nhau **UNIQUE**.
* Tổ hợp (NCode, Phone) đóng vai trò là **PRIMARY KEY** của bảng, với ràng buộc
**fk_nursephone_nurse_ncode** rằng NCode là khóa ngoại tham khảo đến ECode của bảng
NURSE. Khi Ecode bị xóa khỏi bảng NURSE thì tổ hợp tương ứng ở bảng
NURSE_PHONE cũng bị xóa **ON DELETE CASCADE**.
 ### Thêm dữ liệu vào bảng DOCTOR_PHONE và NURSE_PHONE
```
INSERT INTO DOCTOR_PHONE
VALUES (0, 0919123456);
INSERT INTO DOCTOR_PHONE
VALUES (0, 0919123457);
INSERT INTO DOCTOR_PHONE
VALUES (1, 0909123567);
INSERT INTO DOCTOR_PHONE
VALUES (2, 0919123455);
INSERT INTO DOCTOR_PHONE
VALUES (3, 0919123333);
INSERT INTO DOCTOR_PHONE
VALUES (4, 4444444444);
INSERT INTO NURSE_PHONE
VALUES (6, 0919123456);
INSERT INTO NURSE_PHONE
VALUES (6, 0919123457);
INSERT INTO NURSE_PHONE
VALUES (7, 0909123567);
INSERT INTO NURSE_PHONE
VALUES (7, 0919123455);
INSERT INTO NURSE_PHONE
VALUES (8, 0919123333);
INSERT INTO NURSE_PHONE
VALUES (9, 4444444444);
```
### Khởi tạo bảng DEPARTMENT
```
CREATE TABLE IF NOT EXISTS DEPARTMENT
(
DCode INT PRIMARY KEY,
DName VARCHAR(15) NOT NULL UNIQUE,
DeanCode DECIMAL(9, 0) ZEROFILL NOT NULL,
CONSTRAINT fk_dept_doc_deancode FOREIGN KEY(DeanCode)
REFERENCES DOCTOR(ECode)
ON DELETE NO ACTION
);
```
Trong đó:
* DCode là mã phòng ban, là một số nguyên. Đây cũng là **PRIMARY KEY** của bảng.
* DName là tên gọi của phòng ban, là một chuỗi bất kỳ tối đa 15 ký tự. Lựa chọn kiểu dữ liệu là **VARCHAR(15)**. Trường này phải không được bỏ trống, do đó có
**NOT NULL**, và các phòng ban khác nhau phải có tên khác nhau nên phải có **UNIQUE**,
* DeanCode là mã nhân viên của trưởng khoa, là khóa ngoại nên nó có cùng kiểu với
ECode thuộc bảng DOCTOR. Khi ECode ở bảng DOCTOR bị xóa, ta vẫn giữ DeanCode lại ở
bảng DEPARTMENT – **ON DELETE NO ACTION**.

### Trigger cho bảng DEPARMENT
Ràng buộc “trưởng khoa phải là bác sĩ có chuyên khoa tương ứng và nhận bằng chuyên môn
trước hiện tại ít nhất là 5 năm” được thể hiện qua Trigger sau:
```
DELIMITER //
CREATE TRIGGER dean_check BEFORE INSERT ON DEPARTMENT
FOR EACH ROW BEGIN
DECLARE c INT;
SELECT COUNT(*) INTO c FROM DOCTOR d WHERE d.ECode = NEW.DeanCode AND
d.SpName = NEW.DName AND TIMESTAMPDIFF(year, SpYear, NOW()) >= 5;
IF (c <= 0) THEN
SET NEW.DeanCode = NULL;
END IF;
END //
DELIMITER ;
```
Trigger này kiểm tra xem DeanCode nhập vào có tương ứng với một bác sĩ – **FROM
DOCTOR d**, vị này có bằng chuyên môn trùng tên với phòng ban – **d.SpName = NEW.DName**, và
đã nhận bằng ít nhất 5 năm – **TIMESTAMPDIFF(year, SpYear, NOW()) >= 5**. Nếu không tồn tại
bác sĩ nào thỏa điều kiện đó, DeanCode sẽ bị đưa về NULL dẫn đến vi phạm định nghĩa
DeanCode.

### Thêm dữ liệu vào bảng DEPARTMENT
```
INSERT INTO DEPARTMENT
VALUES (0, 'Neurosurgery', 0);
INSERT INTO DEPARTMENT
VALUES (1, 'Medicine', 2);
INSERT INTO DEPARTMENT
VALUES (2, 'Cardiology', 3);
INSERT INTO DEPARTMENT
VALUES (3, 'Virology', 4);
```
### Thêm ràng buộc khóa ngoại vào bảng DOCTOR và NURSE
```
ALTER TABLE DOCTOR
ADD CONSTRAINT fk_doc_dept_depcode FOREIGN KEY(DepCode)
REFERENCES DEPARTMENT(DCode)
ON DELETE NO ACTION
ON UPDATE CASCADE;
ALTER TABLE NURSE
ADD CONSTRAINT fk_nurse_dept_depcode FOREIGN KEY(DepCode)
REFERENCES DEPARTMENT(DCode)
ON DELETE NO ACTION
ON UPDATE CASCADE;
```
Hai ràng buộc này về căn bản là tương tự nhau: DepCode trong hai bảng DOCTOR và
NURSE là khóa ngoại tham khảo đến DCode trong DEPARTMENT. Nếu phòng ban bị xóa, bảng
DOCTOR và NURSE vẫn lưu lại DepCode của phòng ban đã bị xóa này – **ON DELETE NO ACTION**.
Nếu phòng ban được cập nhật, các bác sĩ và y tá sẽ được cập nhật DepCode tương ứng – **ON
UPDATE CASCADE**.

