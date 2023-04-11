### Biểu đồ EERD
![erd](https://user-images.githubusercontent.com/125241615/231153730-7b985e65-3381-4faa-8f15-b92f57bb4a24.png)

### Lược đồ quan hệ
![Databaseschema](https://user-images.githubusercontent.com/125241615/231155136-c20a225e-942e-4910-a04a-1cc01929b0e2.png)

### Ràng buộc ngữ nghĩa
1. Trưởng khoa phải có một chuyên môn tương ứng và có hơn 5 năm kinh nghiệm
kể từ khi được cấp bằng chuyên môn.
2. Trong thực thể PATIENT, thuộc tính PNumber là một dãy 9 kí tự số.
3. Trong thực thể INPATIENT, thuộc tính IPRecovered được suy ra từ thuộc tính
TrResult của thực thể TREATMENT có quan hệ TREATS với INPATIENT đó mà có
giá trị TrEDate trễ nhất.
4. Trong thực thể INPATIENT, thuộc tính IPDisDate được suy ra từ thuộc tính
TrEDate của thực thể TREATMENT có quan hệ TREATS với INPATIENT đó.
5. Trong thực thể MEDICATION, thuộc tính MExpired được suy ra từ thuộc tính
MExDate tương ứng.
6. Trong quan hệ UNDERGOES, thuộc tính ExDocCode được suy ra từ ECode của
thực thể DOCTOR tham gia quan hệ EXAMINES với thực thể OUTPATIENT tương
ứng trong quan hệ UNDERGOES.
