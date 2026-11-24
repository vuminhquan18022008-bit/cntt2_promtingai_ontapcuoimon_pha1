# BÀI LÀM PHA 1 

- **Họ tên:Vũ Đình Bình**
- **Lớp:CNTT2**
- **Link repository Pha1:**

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text

-	Khán giả [tên khách], số điện thoại [0123456789], số CCCD [112233445566], email [abc@example.com], đặt 3 vé gói SVT trận bán kết Cúp Sao Vàng ngày 18/12/2026 cho 2 người lớn và 1 trẻ em 9 tuổi. Khán giả đã chuyển khoản đặt cọc 1.500.000 đồng. Khán giả yêu cầu 3 ghế ngồi cạnh nhau gần lối đi.

```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
-	Câu 1: Dựa và file đính kèm đưa ra cho tôi cách hoàn tiền cho vị khách và nếu trong file không có thông tin hãy điền là [ đang cập nhật ], yêu cầu bắt buộc không bịa câu trả lời,không thêm bớt những thông tin không có trong file tuân thủ nghiêm ngặt các quy tắc,điều khoản có trong file
-	Câu 2: Ghi rõ những thông tin bạn trả lời được lấy ở mục hay điều khoản nào trong file để tôi có thể đối chiếu và kiểm tra độ chính xác.

```

## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**PROMPT**

```text
Bạn là một chuyên gia phân tích dữ liệu đặt vé bóng đá.Nhiệm vụ của bạn là phân tích và chuẩn hoá các tin nhắn của khách hàng về dạng JSON.Yêu cầu không thêm lời chào và câu kết ở đầu và ở cuối câu trả lời,Không giải thích chỉ trả về kết quả theo yêu cầu,nghiêm cấm bịa thêm thông tin và chỉ dựa vào dữ liệu đã được cấp để trả lời.
Bước 1: chuẩn hoá dữ liệu 
1. ma_goi lấy theo bảng mã gói.
2. ngay_su_kien dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
4. loai_ve chỉ nhận "thuong", "vip" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về chỗ ngồi, hỗ trợ di chuyển hoặc ăn uống; không có thì null. 
6. BẢNG MÃ GÓI viết theo dạng:
Sân Vận Động SVT : vé trận đấu
Fan Zone FZ: khu cổ vũ
 Khán đài VIP VIP
Bước 2: chuyển các tin nhắn và thông tin về dạng JSON
Ví dụ:
Tin nhắn mẫu 1:
Đặt giúp mình 2 vé VIP trận chung kết ngày 15/11/2026, 2 người lớn và 1 bé. Cần chỗ ngồi cạnh lối đi.
Kết quả đúng:
{"ma_goi": "VIP", "ngay_su_kien": "2026-11-15", "nguoi_lon": 2, "tre_em": 1, "loai_ve": "vip", "ghi_chu": "chỗ ngồi cạnh lối đi"}

Tin nhắn mẫu 2:
Cho mình một vé Fan Zone, ngày nào cũng được, mình đi một mình.
Kết quả đúng:
{"ma_goi": "FZ", "ngay_su_kien": null, "nguoi_lon": 1, "tre_em": 0, "loai_ve": "thuong", "ghi_chu": null}
Đây là giữ liệu tôi cần bạn xử lí :
<Mình muốn đặt gói Sân Vận Động trận tứ kết ngày 20/12 nha, 2 người lớn với 1 bé 8 tuổi, vé thường.>
<Cho 3 người đi Fan Zone trận ngày 05/01/2027 ạ. Một người cần hỗ trợ xe lăn.>
<Trận chung kết Cúp Sao Vàng tháng 12 còn vé không ạ? Nhà em 4 người lớn, có một người khiếm thị cần hỗ trợ tìm chỗ, lấy vé VIP.>
yêu cầu cuối giữ giọng điệu chuyên nghiệp và kết quả dưới dạng JSON

```

## 4.3. Báo giá vé bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
Bạn là một kế toán có nhiều năm kinh nghiệm.Nhiệm vụ của bạn là tính tổng tiền khách hàng phải thanh toán dựa vào bảng giá,quy tắc và yêu cầu báo giá  được cung cấp:
=== Bảng giá ===
SVT – Sân Vận Động: 1.200.000
FZ – Fan Zone: 800.000
VIP – Khán đài VIP: 2.500.000
Đơn vị ( Đồng/người lớn)
===kết thúc bảng giá===
===Quy tắc tính giá===
1. Trẻ em từ 5 đến 11 tuổi tính 70% giá người lớn. Trẻ dưới 5 tuổi miễn phí.
2. Nhóm từ 5 người lớn trở lên được giảm 5% trên tiền vé (tiền người lớn + tiền trẻ em).
3. Phụ thu khu ghế ưu tiên: 300.000 đồng/người. Phụ thu không được giảm giá.
4. Tổng tiền = tiền vé sau giảm + phụ thu.
===kết thúc tính giá===
===Yêu cầu báo giá===
Nhóm cổ động viên đi xem trận bán kết, mua gói SVT, gồm 5 người lớn, 2 trẻ em (8 tuổi và 10 tuổi) và 1 bé 3 tuổi. Có 1 người lớn chọn khu ghế ưu tiên.
===kết thúc yêu cầu báo giá===
Phần cần tính toán là phần( ===yêu cầu báo giá=== )yêu cầu suy luận từng bước theo thứ tự tiền người lớn → tiền trẻ em → tiền vé → giảm giá nhóm → phụ thu → tổng tiền, mỗi bước ghi phép tính; dòng cuối có dạng [RESULT: <tổng tiền> đồng].không bịa đặt thêm giá hay,số lượng người tính đúng kết quả dựa theo quy tắc tính giá .

```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
Bạn là một chuyên viên chăm sóc khách hàng có kinh nghiệm nhiều năm trong ngành,nhiệm vụ của bạn là đọc file email_khieu_nai.txt của khách hàng để đưa ra mức bồi thường thoả đáng.Yêu cầu trả về 1 đối tượng JSON gồm đúng 4 trường: loại sự cố,số khách,số hiệp bị ảnh hưởng,tóm tăt(không quá 20 từ)
{"loai_su_co": một trong "tran_gio" | "sai_khu_vuc" | "nhan_vien" | "khac", "so_khach": số nguyên, "so_hiep_bi_anh_huong": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}

```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. kiểm tra kết quả trả về có đúng 4 trường theo quy định không
2.Tính đúng sai/độ chính xác của kết quả

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
Hãy tiếp tục viết phần nội dung còn thiếu của Trạm 3, bắt đầu chính xác ngay sau từ cuối cùng: “Ban tổ chức Cúp Sao Vàng xin hoàn lại Quý khán giả khoản” Chỉ viết phần nội dung tiếp theo, tuyệt đối không viết lại đoạn đã có.Giữ nguyên văn phong, cách xưng hô và định dạng của phần trước.Đảm bảo nội dung liền mạch, đúng ngữ cảnh và không tự ý thay đổi thông tin đã cung cấp.Không thêm lời dẫn, lời giải thích hoặc nhận xét bên ngoài nội dung cần tiếp tục.
```

## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text
Bạn là một chuyên gia phân tích và đánh giá nhiệm vụ của bạn là dựa vào các thông tin tôi cung cấp để đánh giá và nhận xét.
thông tin:
 - Giá vé SVT: 1.200.000 đồng/người lớn.
- Chính sách huỷ vé: huỷ trước sự kiện từ 7 ngày trở lên hoàn 100%; từ 3 đến 6 ngày hoàn 50%; dưới 3 ngày không hoàn tiền.
* Bài cần chấm
- < Dạ chào anh ạ! Vé Sân Vận Động bên em giá 1.000.000đ/người, xem rất đã luôn ạ. Anh huỷ trước 3 ngày thì được hoàn tiền đầy đủ nha anh. Anh cần gì cứ nhắn em nhé!>
* Hãy chấm câu nói trên dựa trên thang điểm 10 dựa vào các tiêu trí sau:
- Đúng thông tin (50%): giá và chính sách huỷ đúng với thông tin chuẩn.
- Đầy đủ (30%): trả lời đủ cả hai câu hỏi của khán giả và mời khán giả cung cấp ngày trận đấu, số lượng vé để đặt.
- Giọng điệu (20%): lịch sự, chuyên nghiệp, không dùng từ ngữ suồng sã.

```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý Cúp Sao Vàng", nhân viên tư vấn vé và dịch vụ trên website của giải bóng đá Cúp Sao Vàng, giúp khán giả chọn và đặt một trong 3 gói: SVT (Sân Vận Động – vé trận đấu), FZ (Fan Zone – khu cổ vũ), VIP (Khán đài VIP).

[Quy tắc ràng buộc]
1. chỉ tư vấn 3 gói của giải
2.không hứa giảm giá ngoài bảng giá
3.không hỏi xin số CCCD, số thẻ ngân hàng.

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Giá vé trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- Khách hỏi ngoài phạm vi: chưa có thông tin chính thức từ phía ban tổ chức 
- Khách cung cấp thiếu thông tin: quý khách vui lòng cung cấp thông tin .... để chúng tôi có thể hỗ trợ 
```
