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

### Khởi tạo bảng INPATIENT
```
CREATE TABLE INPATIENT
(
PChar CHAR(2),
PNumber DECIMAL(9, 0) ZEROFILL,
PFiName VARCHAR(15) NOT NULL,
PLName VARCHAR(15) NOT NULL,
PGender CHAR,
PDoB DATE,
PAddress VARCHAR(50),
PPhone DECIMAL(10, 0) ZEROFILL,
IPAdDate DATE,
IPDisDate DATE,
IPSickroom CHAR(3),
IPFee NUMERIC(15,4),
IPDiag VARCHAR(50),
NurseCode DECIMAL(9, 0) ZEROFILL NOT NULL,
PRIMARY KEY(Pchar,PNumber),
CONSTRAINT fk_inp_nurse_nursecode FOREIGN KEY(NurseCode)
REFERENCES NURSE(ECode)
ON DELETE NO ACTION,
CONSTRAINT inpchar_check CHECK (PChar = 'IP'),
CONSTRAINT inp_check_gender CHECK (PGender IN ('M', 'F'))
);
```
Trong đó:
* PChar là phần ký tự chữ trong mã bệnh nhân nội trú, chỉ nhận giá trị “IP”. Do đó, đây là
trường kiểu **CHAR(2)** và ràng buộc **inpchar_check** đảm bảo rằng chỉ có giá trị “IP” là hợp
lệ cho trường này.
* PNumber là phần ký số trong mã bệnh nhân nội trú, gồm một chuỗi 9 ký số như
“000000001”. Do đó, trường này sẽ nhận kiểu **DECIMAL(9, 0) ZEROFILL**.
* PFiName là tên của bệnh nhân, là một chuỗi bất kỳ với tối đa 15 ký tự. Lựa chọn kiểu **VARCHAR(15)** và yêu cầu tên bệnh nhân phải **NOT NULL**.
* PLName là họ của bệnh nhân, cũng là một chuỗi bất kỳ với tối đa 15 ký tự. Lựa chọn kiểu **VARCHAR(15)** và yêu cầu họ của bệnh nhân phải **NOT NULL**.
* PGender là giới tính của bệnh nhân. Tương tự như EGender thuộc bảng DOCTOR và
NURSE, lựa chọn kiểu **CHAR** với 2 giá trị hợp lệ là **M** và **F**. Ràng buộc này được
áp dụng bằng **CONSTRAINT inp_check_gender**.
* PDoB là ngày sinh của bệnh nhân, kiểu **DATE**.
* PAddress là địa chỉ của bệnh nhân, là một chuỗi bất kỳ tối đa 50 ký tự. Lựa
chọn kiểu **VARCHAR(50)**.
* PPhone là điện thoại của bệnh nhân. Tương tự với Phone của bảng DOCTOR và NURSE,
lựa chọn kiểu **DECIMAL(10, 0) ZEROFILL**.
* PAdDate là ngày bệnh nhân nhập viện. Kiểu dữ liệu lựa chọn là
*DATE**.
* PDisDate là ngày bệnh nhân xuất viện. Kiểu dữ liệu lựa chọn là
**DATE**.
* IPRoom là phòng bệnh của bệnh nhân. Đó là một chuỗi bất kỳ giới
hạn 3 ký tự, nên kiểu dữ liệu được chọn là **CHAR(3)**.
* IPFee là phí nhập viện của bệnh nhân. Vì là tiền nên chọn kiểu dữ liệu số với
nhiều nhất 15 ký số và 4 ký số sau dấu thập phân – **NUMERIC(15, 4)**.
* IPDiag là chẩn đoán bệnh của bệnh nhân. Cho đây là một chuỗi bất kỳ với giới
hạn 50 ký tự - **VARCHAR(50)**.
* NurseCode là mã nhân viên của y tá chăm sóc bệnh nhân nội trú. Đây là một khóa ngoại
tham khảo đến ECode trong bảng NURSE, được áp dụng bằng ràng buộc
**fk_inp_nurse_nursecode**. Khi ECode tương ứng trong bảng NURSE bị xóa, ta vẫn giữ lại
NurseCode trong bản INPATIENT – **ON DELETE NO ACTION**.

### Thêm dữ liệu vào bảng INPATIENT
```
INSERT INTO INPATIENT
VALUES ('IP', 0, 'Jack', 'Napier', 'M', '1939-01-01','Gotham', 0919987654, '2020-
05-19', NULL, '001', 1000, 'Psychosis', 6);
INSERT INTO INPATIENT
VALUES ('IP', 1, 'Harlene', 'Quinzel', 'F', '1995-03-01','Gotham', 0919985555,
'2022-10-31', NULL, '001', 900, 'Psychosis', 6);
INSERT INTO INPATIENT
VALUES ('IP', 2, 'Jane', 'Doe', 'F', '2001-12-23', 'Missouri', 0909090909, '2022-
11-01', '2022-11-27', '002', 2000, 'Covid', 9);
INSERT INTO INPATIENT
VALUES ('IP', 3, 'Van Nam', 'Nguyen', 'M', '2001-07-11', 'Long Xuyen',
0109090909, '2019-01-01', '200-11-30', '006', 2050, 'Cancer', 9);
```
### Khởi tạo bảng OUTPATIENT
```
CREATE TABLE OUTPATIENT
(
PChar CHAR(2),
PNumber DECIMAL(9, 0) ZEROFILL,
PFiName VARCHAR(15) NOT NULL,
PLName VARCHAR(15) NOT NULL,
PGender CHAR,
PDoB DATE,
PAddress VARCHAR(50),
PPhone DECIMAL(10, 0) ZEROFILL,
DocCode DECIMAL(9, 0) ZEROFILL NOT NULL,
PRIMARY KEY(Pchar,PNumber),
CONSTRAINT fk_outp_doc_doccode FOREIGN KEY(DocCode)
REFERENCES DOCTOR(ECode)
ON DELETE NO ACTION,
CONSTRAINT outp_check_gender CHECK (PGender IN ('M', 'F')),
CONSTRAINT outpchar_check CHECK (PChar = 'OP')
);
```
Trong đó:
* PChar là phần ký tự chữ trong mã bệnh nhân ngoại trú, chỉ nhận giá trị “OP”. Do đó, đây
là trường kiểu **CHAR(2)** và ràng buộc **outpchar_check** đảm bảo rằng chỉ có giá trị “OP” là
hợp lệ cho trường này.
* PNumber là phần ký số trong mã bệnh nhân ngoại trú, gồm một chuỗi 9 ký số như
“000000001”. Do đó, trường này sẽ nhận kiểu **DECIMAL(9, 0) ZEROFILL**.
* PFiName là tên của bệnh nhân, là một chuỗi bất kỳ với tối đa 15 ký tự. Lựa chọn kiểu **VARCHAR(15)** và yêu cầu tên bệnh nhân phải **NOT NULL**.
* PLName là họ của bệnh nhân, cũng là một chuỗi bất kỳ với tối đa 15 ký tự. Chọn kiểu **VARCHAR(15)** và yêu cầu họ của bệnh nhân phải **NOT NULL**.
* PGender là giới tính của bệnh nhân. Tương tự như EGender thuộc bảng DOCTOR và
NURSE, lựa chọn kiểu CHAR với 2 giá trị hợp lệ là **M** và **F**. Ràng buộc này được
áp dụng bằng **CONSTRAINT outp_check_gender**.
* PDoB là ngày sinh của bệnh nhân, kiểu **DATE**.
* PAddress là địa chỉ của bệnh nhân, là một chuỗi bất kỳ tối đa 50 ký tự. Chọn kiểu **VARCHAR(50)**.
* PPhone là điện thoại của bệnh nhân. Tương tự với Phone của bảng DOCTOR và NURSE, lựa chọn kiểu **DECIMAL(10, 0) ZEROFILL**.
* DocCode là mã nhân viên của bác sĩ khám bệnh cho bệnh nhân ngoại trú. Đây là một
khóa ngoại tham khảo đến ECode trong bảng DOCTOR, được áp dụng bằng ràng buộc
**fk_outp_doc_doccode**. Khi ECode tương ứng trong bảng DOCTOR bị xóa, ta vẫn giữ lại
DocCode trong bản OUTPATIENT – **ON DELETE NO ACTION**.
### Thêm dữ liệu vào bảng OUTPATIENT
```
INSERT INTO OUTPATIENT
VALUES ('OP', 0, 'Bruce', 'Wayne', 'M', '1991-07-04', 'Gotham', 0919193911, 0);
INSERT INTO OUTPATIENT
VALUES ('OP', 1, 'Selina', 'Kyle', 'F', '1993-03-14', 'Brudhaven', 0909094444,
4);
INSERT INTO OUTPATIENT
VALUES ('OP', 2, 'Richard', 'Grayson', 'M', '2001-03-20', 'Brudhaven',
0888123456, 0);
INSERT INTO OUTPATIENT
VALUES ('OP', 3, 'Oliver', 'Queen', 'M', '1985-05-16', 'Starling', 0444444444,
4);
```
### Khởi tạo bảng MEDICATION
```
CREATE TABLE MEDICATION
(
MCode CHAR(9) PRIMARY KEY,
MName VARCHAR(20) NOT NULL UNIQUE,
MPrice NUMERIC(15,4),
MExDate DATE,
MExpired BIT
);
```

Trong đó:
* MCode là mã định danh của loại thuốc, là một chuỗi bất kỳ 9 ký tự. Lựa
chọn kiểu dữ liệu là **CHAR(9)**. Đây cũng là **PRIMARY KEY** của bảng.
* MName là tên của loại thuốc, là một chuỗi bất kỳ tối đa 20 ký tự. Chọn
kiểu dữ liệu **VARCHAR(20)**. Đồng thời, tên thuốc phải **NOT NULL** và là độc nhất trong
bảng **UNIQUE**.
* MPrice là giá thuốc, vì là tiền nên chọn kiểu dữ liệu số với 15 ký số trước dấu
phẩy và 4 ký số sau dấu phẩy – **NUMERIC(15, 4)**.
* MExDate là ngày nhập thuốc, do đó đây sẽ là kiểu **DATE**.
* MExpired cho ta biết thuốc đã hết hạn sử dụng chưa. Do đó đây sẽ là giá trị **BIT** với 0 là
chưa và 1 là rồi.

### Tạo Trigger cho bảng MEDICATION
Chúng ta cần tạo một Trigger chạy hằng ngày nhằm chỉnh MExpired thành 1 nếu MExDate
vượt quá ngày hôm nay.<br />
CREATE EVENT STATEMENT
```
CREATE
    [DEFINER = user]
    EVENT
    [IF NOT EXISTS]
    event_name
    ON SCHEDULE schedule
    [ON COMPLETION [NOT] PRESERVE]
    [ENABLE | DISABLE | DISABLE ON SLAVE]
    [COMMENT 'string']
    DO event_body;

schedule: {
    AT timestamp [+ INTERVAL interval] ...
  | EVERY interval
    [STARTS timestamp [+ INTERVAL interval] ...]
    [ENDS timestamp [+ INTERVAL interval] ...]
}

interval:
    quantity {YEAR | QUARTER | MONTH | DAY | HOUR | MINUTE |
              WEEK | SECOND | YEAR_MONTH | DAY_HOUR | DAY_MINUTE |
              DAY_SECOND | HOUR_MINUTE | HOUR_SECOND | MINUTE_SECOND}
```

```
DELIMETER//

CREATE EVENT IF NOT EXISTS auto_expire
ON SCHEDULE EVERY 1 DAY
START CURRENT_TIMESTAMP
DO
UPDATE MEDICATION
SET MExpired = 1
WHERE MExDate = CURDATE();

DELIMETER;
```

### Khởi tạo bảng PROVIDER
```
CREATE TABLE PROVIDER
(
PrCode CHAR(9) PRIMARY KEY,
PrName VARCHAR(20) NOT NULL UNIQUE,
PrAddress VARCHAR(50),
PrPhone DECIMAL(10, 0) ZEROFILL;
);
```

Trong đó:
* PrCode là mã định danh của nhà cung cấp, và là một chuỗi 9 ký tự. Lựa
chọn kiểu dữ liệu **CHAR(9)**. Đồng thời PrCode cũng là **PRIMARY KEY** của bảng.
* PrName là tên của nhà cung cấp, là một chuỗi bất kỳ có tối đa 20 ký tự. Lựa chọn kiểu dữ liệu **VARCHAR(20)**. Đồng thời ta yêu cầu nhà cung cấp phải có tên cụ thể - **NOT NULL** và độc nhất trong bảng – **UNIQUE**.
* PrAddress là địa chỉ của nhà cung cấp, là một chuỗi bất kỳ tối đa 50 ký tự. Lựa chọn kiểu dữ liệu **VARCHAR(50)**.
* PrPhone là số điện thoại của nhà cung cấp. Đây là một chuỗi 10 ký số và được thêm 0
vào bên trái trong trường hợp chuỗi dưới 10 ký số. Vì vậy kiểu dữ liệu chọn là **DECIMAL(10, 0) ZEROFILL**.

### Thêm dữ liệu vào bảng MEDICATION và PROVIDER
```
INSERT INTO MEDICATION
VALUES ('000000000', 'Potion', '200', '2022-11-28', 1);
INSERT INTO MEDICATION
VALUES ('000000001', 'Super Potion', '600', '2023-07-30', 0);
INSERT INTO MEDICATION
VALUES ('000000002', 'Hyper Potion', '1200', '2024-12-31', 0);
INSERT INTO MEDICATION
VALUES ('000000003', 'Max Potion', '2000', '2030-08-31', 0);
INSERT INTO PROVIDER
VALUES ('000000000', 'Doraemon', 'Tokyo', '222222222');
INSERT INTO PROVIDER
VALUES ('000000001', 'Chopper Inc.', 'Drum', '333333333');
INSERT INTO PROVIDER
VALUES ('000000002', 'Mganga Medicine', 'Berlin', '444444444');
INSERT INTO PROVIDER
VALUES ('000000003', 'Doofenshmirtz Inc.', 'Danville', '555555555');
```
### Khởi tạo bảng PROVIDES
```
CREATE TABLE PROVIDES
(
PrCode CHAR(9) NOT NULL,
MCode CHAR(9) NOT NULL,
ProPrice NUMERIC(15,4),
ProDate DATE,
ProQuantity INT,
PRIMARY KEY(PrCode,MCode),
CONSTRAINT fk_pro_pr_prcode FOREIGN KEY(PrCode)
REFERENCES PROVIDER(PrCode)
ON DELETE CASCADE,
CONSTRAINT fk_pro_med_mcode FOREIGN KEY(MCode)
REFERENCES MEDICATION(MCode)
ON DELETE CASCADE
);
```
Trong đó:
* PrCode là mã định danh của nhà cung cấp, tương tự như trường PrCode trong bảng
PROVIDER. Ta yêu cầu trường này **NOT NULL**.
* MCode là mã định danh của thuốc, tương tự như MCode trong bảng MEDICATION. Ta
cũng yêu cầu trường này **NOT NULL**.
* ProPrice là giá nhập thuốc. Vì là tiền nên lựa chọn kiểu dữ liệu số
**NUMERIC(15, 4)**.
* ProDate là ngày nhập thuốc. Vì là ngày nên lựa chọn kiểu dữ liệu **DATE**.
* ProQuantity là số lượng nhập. Quy định trường này là kiểu **INT**.
* **PRIMARY KEY** của bảng là tổ hợp (PrCode, MCode) trong đó PrCode là khóa ngoại tham
khảo PrCode của bảng PROVIDER và MCode là khóa ngoại tham khảo MCode của bảng
MEDICATION. Hai ràng buộc thực thi việc này là **fk_pro_pr_pcrcode** và
**fk_pro_med_mcode**. Khi PrCode hay MCode tương ứng bị xóa thì tổ hợp tương ứng
trong bảng PROVIDES cũng bị xóa theo – **ON DELETE CASCADE**.

### Thêm giá trị vào bảng PROVIDES
```
INSERT INTO PROVIDES
VALUES ('000000000', '000000000', '2000', '2022-01-01', 10);
INSERT INTO PROVIDES
VALUES ('000000001', '000000000', '2000', '2022-01-01', 10);
INSERT INTO PROVIDES
VALUES ('000000001', '000000003', '2000', '2022-01-01', 1);
INSERT INTO PROVIDES
VALUES ('000000003', '000000002', '24000', '2021-01-01', 20);
```
### Khởi tạo bảng MEDICATION_EFFECT
```
CREATE TABLE MEDICATION_EFFECT
(
MCode CHAR(9) NOT NULL,
Effect VARCHAR(50) NOT NULL,
PRIMARY KEY(MCode,Effect),
CONSTRAINT fk_medef_med_mcode FOREIGN KEY(MCode)
REFERENCES MEDICATION(MCode)
ON DELETE CASCADE
);
```
Trong đó:
* MCode là mã định danh của thuốc, là một chuỗi 9 ký tự và NOT NULL. Do đó kiểu dữ
liệu lựa chọn là **CHAR(9) NOT NULL**.
* Effect là tác dụng của loại thuốc, là một chuỗi độ dài bất kỳ dưới 50 ký tự. Tác dụng của
thuốc cũng phải NOT NULL. Lựa chọn kiểu dữ liệu **VARCHAR(50) NOT NULL**.
* Tổ hợp (MCode, Effect) là **PRIMARY KEY** của bảng, trong đó MCode là khóa ngoại tham
khảo đến MCode trong bảng MEDICATION. Ràng buộc này được thực thi bởi
**CONSTRAINT fk_medef_med_mcode**. Nếu MCode tương ứng của thuốc trong bảng
MEDICATION bị xóa, tổ hợp tương ứng thuộc MEDICATION_EFFECT cũng bị xóa theo –
**ON DELETE CASCADE**.

### Thêm dữ liệu vào bảng MEDICATION_EFFECT
```
INSERT INTO MEDICATION_EFFECT
VALUES ('000000000', 'Heals 20HP');
INSERT INTO MEDICATION_EFFECT
VALUES ('000000001', 'Heals 50HP');
INSERT INTO MEDICATION_EFFECT
VALUES ('000000002', 'Heals 120HP');
INSERT INTO MEDICATION_EFFECT
VALUES ('000000003', 'Heals max HP');
INSERT INTO MEDICATION_EFFECT
VALUES ('000000000', 'May cause insomnia');
```
### Khởi tạo bảng TREATMENT
```
CREATE TABLE TREATMENT
(
PChar CHAR(2) NOT NULL,
PNumber DECIMAL(9, 0) ZEROFILL NOT NULL,
DocCode DECIMAL(9, 0) ZEROFILL NOT NULL,
TrID INT PRIMARY KEY,
TrSDate DATE,
TrEDate DATE,
TrResult VARCHAR(50),
CONSTRAINT fk_treat_inp_pcode FOREIGN KEY(PChar, PNumber)
REFERENCES INPATIENT(PChar, PNumber)
ON DELETE CASCADE,
CONSTRAINT fk_treat_doc_doccode FOREIGN KEY(DocCode)
REFERENCES DOCTOR(ECode)
ON DELETE NO ACTION,
CONSTRAINT check_edate CHECK (TrEDate = NULL OR TrEDate >= TrSDate)
);
```
Trong đó:
* PChar là phần ký tự trong mã định danh bệnh nhân nội trú. Tương tự PChar trong
INPATIENT, trường này là **CHAR(2) NOT NULL**.
* PNumber là phần ký số trong mã định danh bệnh nhân nội trú. Tương tự PNumber trong
INPATIENT, trường này là **DECIMAL(9, 0) ZEROFILL NOT NULL**.
* DocCode là mã định danh của bác sĩ thực hiện điều trị. Tương tự ECode trong DOCTOR,
trường này là **DECIMAL(9, 0) ZEROFILL**.
* TrID là mã định danh của lần điều trị. Lựa chọn kiểu **INT** và cho TrID là
**PRIMARY KEY** của bảng.
* TrSDate là ngày bắt đầu chữa trị. Vì là ngày nên chọn lựa kiểu dữ liệu là **DATE**.
* TrEDate là ngày chữa trị kết thúc. Vì là ngày nên chọn kiểu dữ liệu là **DAT**E.
* TrResult là kết quả của lần chữa trị, là một chuỗi bất kỳ với độ dài dưới 50 ký tự. Do đó lựa chọn kiểu dữ liệu **VARCHAR(50)**.
* Ta có tổ hợp (PChar, PNumber) là khóa ngoại tham khảo đến (PChar, Pnumber) thuộc
INPATIENT và DocCode là khóa ngoại tham khảo đến ECode trong bảng DOCTOR. Hai
ràng buộc này được thực thi lần lượt bởi **CONSTRAINT fk_treat_inp_pcode** và
**CONSTRAINT fk_treat_doc_doccode**.
* TrSDate phải là một ngày trước hoặc đúng với TrEDate. Ràng buộc này được thực thi bởi
**CONSTRAINT edate_check**.

### Tạo Trigger cho bảng TREATMENT
Chúng ta cần tạo Trigger cho bản TREATMENT sao cho khi bác sĩ cập nhật TrResult thành
‘Recovered’ thì bảng INPATIENT cũng thể hiện được bệnh nhân đã xuất viện.
```
DELIMETER//
CREATE TRIGGER recovered AFTER UPDATE ON TREATMENT
FOR EACH ROW BEGIN
IF NEW.TrResult = 'Recovered' THEN
UPDATE INPATIENT
SET IPDisDate = CURDATE()
WHERE NEW.PNumber = INPATIENT.PNumber;
END IF;
END//
DELIMETER;
```
### Thêm dữ liệu vào bảng TREATMENT
```
INSERT INTO TREATMENT
VALUES ('IP', 0, 0, 0, '2020-09-30', NULL, 'Not recovered');
INSERT INTO TREATMENT
VALUES ('IP', 0, 1, 1, '2021-09-30', NULL, 'Not recovered');
INSERT INTO TREATMENT
VALUES ('IP', 0, 2, 2, '2022-09-30', NULL, 'Not recovered');
INSERT INTO TREATMENT
VALUES ('IP', 3, 3, 3, '2020-05-30', NULL, 'Not recovered');
```
### Khởi tạo bảng IS_USED_TO_TREAT
```
CREATE TABLE IS_USED_TO_TREAT
(
TrID INT NOT NULL,
MCode CHAR(9) NOT NULL,
PRIMARY KEY(TrID,MCode),
CONSTRAINT fk_iutt_treatment FOREIGN KEY(TrID)
REFERENCES TREATMENT(TrID)
ON DELETE CASCADE,
CONSTRAINT fk_iutt_med_mcode FOREIGN KEY(MCode)
REFERENCES MEDICATION(MCode)
ON DELETE NO ACTION
);
```
Trong đó:
* TrID là mã định danh của buổi chữa trị, tương tự như TrID trong bảng TREATMENT, đây
là một trường **INT NOT NULL**.
• MCode là mã định danh của thuốc, tương tự như MCode trong bảng MEDICATION, đây
là một trường **CHAR(9) NOT NULL**.
• Tổ hợp (TrID, MCode) là PRIMARY KEY của bảng, với TrID là khóa ngoại tham khảo đến
TrID của TREATMENT và MCode là khóa ngoại tham khảo đến MCode của MEDICATION.
NếU TrID tương ứng trong TREATMENT bị xóa, tổ hợp trong IS_USED_TO_TREAT cũng sẽ bị xóa – **ON DELETE CASCADE**. Ngược lại, nếu MCode tương ứng trong MEDICATION bị
xóa, tổ hợp trong IS_USED_TO_TREAT sẽ không bị ảnh hưởng – **ON DELETE NO ACTION**.
### Thêm dữ liệu vào bảng IS_USED_TO_TREAT
```
INSERT INTO IS_USED_TO_TREAT
VALUES (0, '000000000');
INSERT INTO IS_USED_TO_TREAT
VALUES (0,'000000001');
INSERT INTO IS_USED_TO_TREAT
VALUES (0, '000000002');
INSERT INTO IS_USED_TO_TREAT
VALUES (1, '000000000');
INSERT INTO IS_USED_TO_TREAT
VALUES (2, '000000000');
INSERT INTO IS_USED_TO_TREAT
VALUES (3, '000000000');
```
### Khởi tạo bảng EXAMINATION
```
CREATE TABLE EXAMINATION
(
PChar CHAR(2) NOT NULL,
PNumber DECIMAL(9, 0) ZEROFILL NOT NULL,
ExID INT PRIMARY KEY,
ExDate DATE,
ExNextDate DATE,
ExFee NUMERIC(15,4),
ExDiag VARCHAR(50),
CONSTRAINT fk_exam_outp_pcode FOREIGN KEY(PChar, PNumber)
REFERENCES OUTPATIENT(PChar, PNumber)
ON DELETE CASCADE,
CONSTRAINT check_exam_nextdate CHECK (ExNextDate > ExDate OR ExNextDay =
NULL)
);
```
Trong đó:
* PChar là phần ký tự của mã định danh bệnh nhân tham gia vào buổi khám bệnh, tương
tự với PChar trong bảng OUTPATIENT, giá trị này là **CHAR(2) NOT NULL**.
* PNumber là phần ký số của mã định danh bệnh nhân tham gia vào buổi khám bệnh,
tường tự với Pnumber trong bảng OUTPATIENT, giá trị này là **DECIMAL(9, 0) ZEROFILL
NOT NULL**.
* ExID là **PRIMARY KEY** của bảng, là kiểu dữ liệu **INT**.
* ExDate là ngày khám bệnh. Vì là ngày nên kiểu dữ liệu là **DATE**.
* ExNextDate là ngày khám nệnh tiếp theo. Vì là ngày nên kêu dữ liệu là **DATE**.
* ExFee là phí khám bệnh. Vì là số tiền nên kiểu dữ liệu là **NUMERIC(15, 4)**.
* ExDiag là chẩn đoán, có thể là một chuỗi bất kỳ trong dưới 51 ký tự. Do đó kiểu dữ liệu lựa chọn là **VARCHAR(50)**.
* Ngoài ra, tổ hợp (PChar, Pnumber) là khóa ngoại tham khảo đến tổ hợp (PChar,
Pnumber) của OUTPATIENT. Ràng buộc này được thực thi bởi **CONSTRAINT
fk_exam_outp_code**.
* **CONSTRAINT check_exam_nextdate** giúp chúng ta đảm bảo rằn ExNextDate hoặc sẽ là
**NULL** (không cần tái khám) hoặc một ngày sau ExDate.
### Thêm dữ liệu vào bảng EXAMINATION
```
INSERT INTO EXAMINATION
VALUES ('OP', 0, 0, '2019-08-08', '2019-08-30', 1000, 'Viral fever');
INSERT INTO EXAMINATION
VALUES ('OP', 0, 1, '2019-08-30', NULL, 1000, 'Recovered');
INSERT INTO EXAMINATION
VALUES ('OP', 1, 0, '2020-11-05', NULL, 5000, 'Recovered');
INSERT INTO EXAMINATION
VALUES ('OP', 2, 0, '2022-11-10', '2022-12-31', 10000, 'Cancer');
```
### Khởi tạo bảng IS_USED_IN_EXAM
```
CREATE TABLE IS_USED_IN_EXAM
(
ExID INT NOT NULL,
MCode CHAR(9) NOT NULL,
PRIMARY KEY(ExID, MCode),
CONSTRAINT fk_iuie_exam FOREIGN KEY(ExID)
REFERENCES EXAMINATION(ExID)
ON DELETE CASCADE,
CONSTRAINT fk_iuie_med_mcode FOREIGN KEY(MCode)
REFERENCES MEDICATION(MCode)
ON DELETE NO ACTION
);
```
Trong đó:
* ExID là mã định danh của lần khám bệnh, tương tự như ExID trong EXAMINATION, là
kiểu dữ liệu **INT NOT NULL**.
* MCode là mã định danh của loại thuốc, tương tự như MCode trong MEDICATION, đây
cũng là kiểu **CHAR(9) NOT NULL**.
* Ngoài ra, tổ hợp (ExID, MCode) là **PRIMARY KEY**. Trong đó, ExID là khóa ngoại tham
khảo đến ExID trong EXAMINATION, và MCode là khóa ngoại tham khảo đến MCode
trong MEDICATION.
* Khi ExID tương ứng trong EXAMINATION bị xóa đi, tổ hợp tương ứng trong
IS_USED_IN_EXAM sẽ bị xóa – **ON DELETE CASCADE** trong **CONSTRAINT fk_iuie_exam**.
* Khi MCode tương ứng trong MEDICATION bị xóa đi, tổ hợp tương ứng trong
IS_USED_IN_EXAM sẽ vẫn giữ lại giá trị đã xóa – **ON DELETE NO ACTION** trong
**CONSTRAINT fk_iuie_med_mcode**.
