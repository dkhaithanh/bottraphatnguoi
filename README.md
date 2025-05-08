# Hướng dẫn tạo bot Telegram thông báo biển số xe hàng ngày

Hướng dẫn chi tiết từng bước để tạo một bot Telegram đơn giản, giúp bạn nhận thông báo về các biển số xe được theo dõi hàng ngày.

## Bước 1: Tạo bot Telegram thông qua BotFather (@BotFather)

1.  Mở ứng dụng Telegram và tìm kiếm bot có tên **@BotFather**.
2.  Nhấn **Start** để bắt đầu trò chuyện với BotFather.
3.  Gửi lệnh `/newbot` để tạo một bot mới.
4.  BotFather sẽ yêu cầu bạn đặt tên cho bot của mình. Hãy nhập tên bạn muốn (ví dụ: "Thông báo Biển Số Xe").
5.  Tiếp theo, bạn cần chọn một username cho bot. Username phải kết thúc bằng từ "bot" hoặc "Bot" (ví dụ: "ThongBaoBienSoXeBot").
6.  Sau khi tạo thành công, BotFather sẽ cung cấp cho bạn **API token** của bot. Hãy lưu lại token này cẩn thận, bạn sẽ cần nó ở các bước sau.
7.  Mở Bot vừa tạo, nhấn vào nút Start hoặc Bắt đầu

![Tạo bot Telegram bằng BotFather](https://github.com/user-attachments/assets/00aa47d0-92ed-4622-a782-eb6353d2baf1)

## Bước 2: Lấy ID người dùng của bạn thông qua bot Get My ID (@getmyid_bot)

1.  Tìm kiếm bot có tên **@getmyid_bot** trên Telegram.
2.  Nhấn **Start** để bắt đầu trò chuyện.
3.  Bot sẽ ngay lập tức gửi cho bạn **User ID** của bạn. Hãy ghi nhớ ID này.

![Lấy User ID từ Get My ID bot](https://github.com/user-attachments/assets/151d0bec-393c-4217-aaff-afcb61ddc295)

## Bước 3: Tạo Google Sheet và App Script

1.  Truy cập [sheet.new](https://sheet.new) để tạo một Google Sheet mới.
2.  Trong trang tính mới, vào menu **Tiện ích mở rộng** (Extensions) > **Apps Script**.

![Tạo Google Sheet và mở App Script](https://github.com/user-attachments/assets/4a089a26-4a64-44fc-a86c-84a69ca99b30)

## Bước 4: Sao chép và dán code App Script

1.  Mở file App Script vừa tạo.
2.  Sao chép toàn bộ nội dung code từ [LINK](https://github.com/dkhaithanh/bottraphatnguoi/blob/main/appscrip_Google_sheet).
3.  Dán (ghi đè) nội dung đã sao chép vào trình soạn thảo App Script.

![Dán code App Script](https://github.com/user-attachments/assets/b03f84bc-5d55-4fb8-a88f-862f095ddf07)

## Bước 5: Thêm Telegram Bot Token và Telegram User ID vào App Script

1.  Trong code App Script, tìm đến các dòng sau:

    ```javascript
    var telegram_bot_id = 'YOUR_TELEGRAM_BOT_TOKEN';
    var telegram_user_id = 'YOUR_TELEGRAM_USER_ID';
    ```

2.  Thay thế `'YOUR_TELEGRAM_BOT_TOKEN'` bằng **API token** bạn đã nhận được ở Bước 1.
3.  Thay thế `'YOUR_TELEGRAM_USER_ID'` bằng **User ID** bạn đã lấy được ở Bước 2.
4.  Lưu ý: Không bỏ dấu '' ở đầu và cuối.

![Thêm Token và User ID vào App Script](https://github.com/user-attachments/assets/bf082a89-b005-4572-8869-1bff3513c224)

## Bước 6: Lưu App Script, thêm biển số và chạy thử

### 1. Thêm biển số vào Google Sheet

* Trong Google Sheet của bạn, hãy nhập các biển số xe bạn muốn theo dõi vào **cột A**, bắt đầu từ hàng đầu tiên. Mỗi biển số xe trên một dòng.

![Thêm biển số vào Google Sheet](https://github.com/user-attachments/assets/82b7e6fd-bfa5-4fd9-b632-e54bb290a0af)

### 2. Chạy thử lần đầu

1.  Trong trình soạn thảo App Script, chọn hàm `sendDailyPhatNguoiReport` từ dropdown menu (thường ở trên thanh công cụ).
2.  Nhấn nút **Run** (biểu tượng hình tam giác).
3.  Bạn có thể sẽ được yêu cầu cấp quyền cho script truy cập vào Google Sheet và gửi thông báo Telegram. Hãy chấp nhận các yêu cầu này.

![Chạy thử App Script](https://github.com/user-attachments/assets/424a2849-9772-43ca-9f78-a6b82a28ceb7)

### 3. Kiểm tra Log

1.  Để kiểm tra xem script đã chạy thành công hay chưa, hãy vào menu **Xem** (View) > **Nhật ký thực thi** (Executions).
2.  Xem lại nhật ký để đảm bảo không có lỗi xảy ra và bot đã gửi thông báo thành công đến Telegram của bạn.

![Kiểm tra Log](https://github.com/user-attachments/assets/7fb151d7-b167-412c-afa2-a9d608ff0d09)

## Bước 7: Tạo Trigger để tự động chạy theo thời gian

1.  Trong trình soạn thảo App Script, chọn biểu tượng **Trình kích hoạt** (biểu tượng đồng hồ báo thức) ở menu bên trái.
2.  Nhấn vào nút **Thêm trình kích hoạt** (Add Trigger) ở góc dưới bên phải.
3.  Trong cửa sổ cấu hình trình kích hoạt:
    * **Chọn hàm để chạy:** `sendDailyPhatNguoiReport`
    * **Chọn sự kiện:** `Dựa trên thời gian` (Time-driven)
    * **Chọn loại trình kích hoạt dựa trên thời gian:** `Hẹn giờ theo phút` (Minutes timer) hoặc `Hẹn giờ theo giờ` (Hour timer) tùy theo tần suất bạn muốn nhận thông báo.
    * **Ví dụ:** Để nhận thông báo vào khoảng 7-8 giờ sáng hàng ngày, bạn có thể chọn `Hẹn giờ theo giờ` và đặt thời gian chạy từ 7 đến 8.

![Tạo Trigger](https://github.com/user-attachments/assets/b2fa49b6-8bb2-468b-be7f-e7881c5b8847)

![Cài đặt thời gian cho Trigger](https://github.com/user-attachments/assets/3179414e-5b03-4120-ba35-b4032bc3347b)

**Chúc mừng!** Bạn đã tạo thành công bot Telegram để nhận thông báo biển số xe hàng ngày.

![Hoàn thành](https://github.com/user-attachments/assets/0b8b7134-1584-43d2-b002-576140fa74ca)
