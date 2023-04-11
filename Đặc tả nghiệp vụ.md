# Đặc tả nghiệp vụ
Một bệnh viện X cần xây dựng một hệ thống quản lý thông tin nhằm quản lý thông tin của các bệnh nhân, bác sĩ và y tá của họ.<br />

Cơ sở dữ liệu của bệnh viện X cần lưu trữ thông tin của nhân viên (bác sĩ và y tá) bao gồm: một mã định danh duy nhất, tên đầy đủ gồm họ và tên, ngày tháng năm sinh, giới
tính, địa chỉ, ngày bắt đầu (ngày đầu tiên làm việc), một hoặc nhiều số điện thoại, và chuyên môn (gồm tên chuyên môn và năm cấp bằng). Bệnh viện cũng có nhiều phòng
ban. Mỗi phòng ban có một mã định danh duy nhất, một cái tên, và một trưởng khoa (phải là bác sĩ). Tất cả nhân viên phải thuộc về một phòng ban cụ thể. Một phòng ban có
ít nhất một nhân viên. Trưởng khoa phải có một chuyên môn tương ứng và có hơn 5 năm kinh nghiệm kể từ khi được cấp bằng chuyên môn.<br />

Bệnh nhân cần cung cấp cho bệnh viện các thông tin như: tên đầy đủ (họ và tên), ngày sinh, giới tính, địa chỉ và số điện thoại. Sau khi nhận được thông tin, hệ thống sẽ
lưu trữ chúng vào cơ sở dữ liệu, và đồng thời tạo ra một mã định danh duy nhất để nhận dạng mỗi bệnh nhân. Bệnh nhân được chia làm 2 loại: bệnh nhân ngoại trú và
bệnh nhân nội trú. Bệnh viện cũng muốn dùng hai ký tự đầu tiên của mã định danh để xác định loại bệnh nhân. Nếu là bệnh nhân ngoại trú, mã sẽ bắt đầu bằng “OP” với 9 chữ
số theo sau, ví dụ: “OP000000001”. Nếu là bệnh nhân nội trú, mã sẽ bắt đầu bằng “IP” với 9 chữ số theo sau, ví dụ: “IP000000001”. <br />

* Đối với bệnh nhân ngoại trú, thông tin của bác sĩ chẩn đoán cần phải được lưu lại. Bệnh nhân có thể có nhiều buổi khám với bác sĩ chẩn đoán của họ.
Bệnh viện cần lưu thông tin của mỗi buổi khám như: ngày khám, chẩn đoán, ngày tái khám (nếu có), thuốc, và phí.

* Đối với bệnh nhân nội trú, một số thông tin khác được thêm vào như: ngày nhập viện, các bác sĩ điều trị, y tá điều dưỡng, phòng bệnh, ngày xuất viện,
và phí. Sau khi nhập viện, bệnh nhân có thể được điều trị bởi ít nhất một bác sĩ. Một bác sĩ có thể điều trị cho nhiều bệnh nhân cùng lúc, hoặc đôi
khi không điều trị cho ai cả. Bệnh viện cần thông tin của mỗi lần điều trị như: thời gian điều trị (ngày bắt đầu và kết thúc), kết quả, và thuốc. Mỗi
bệnh nhân nội trú được chăm sóc bởi một y tá. Một y tá có thể chăm sóc nhiều bệnh nhân một lúc. Hơn nữa, nếu một bệnh nhân đã hồi phục và lần
điều trị cuối cùng của họ đã được xác nhận là “đã bình phục” bởi bác sĩ, bệnh nhân đó sẽ được xuất viện. Do đó, ngày xuất viện phải được lưu bởi
hệ thống.

Thông tin thuốc cũng được lưu trong cơ sở dữ liệu. Thông tin này bao gồm một
mã định danh duy nhất, tên thuốc, các tác dụng, giá tiền và ngày hết hạn. Một loại thuốc
có thể được cung cấp bởi một hay nhiều nhà sản xuất, và một nhà sản xuất có thể cung
cấp nhiều loại thuốc khác nhau. Một nhà sản xuất được lưu vết bởi một số độc nhất,
tên, địa chỉ và số điện thoại. Thêm vào đó, bệnh viện cũng cần lưu thông tin các loại
thuốc nhập, bao gồm ngày nhập, giá, và số lượng. Nếu như một loại thuốc đã hết hạn,
nó sẽ được đánh dấu là “hết hạn” trong cơ sở dữ liệu.

