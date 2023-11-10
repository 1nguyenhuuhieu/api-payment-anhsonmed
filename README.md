# api-payment-anhsonmed
API Payment at Anh Son Medical Center


# Hướng Dẫn Sử Dụng API: Thanh Toán Viện Phí Dành Cho Ngân Hàng Tại Trung Tâm Y Tế Huyện Anh Sơn

## 1. Giới Thiệu
API Thanh Toán Viện Phí của Trung tâm Y tế huyện Anh Sơn cung cấp một giải pháp tiên tiến cho các ngân hàng, giúp đơn giản hóa và tự động hóa quy trình thanh toán viện phí. Được thiết kế với mục tiêu tối ưu hóa trải nghiệm thanh toán cho bệnh nhân, API này hứa hẹn mang lại sự thuận tiện và hiệu quả cao trong quản lý tài chính y tế.

## 2. Yêu Cầu Xác Thực
**API_KEY và API_NAME**: Để đảm bảo an toàn thông tin, mỗi ngân hàng cần cung cấp `API_KEY` và `API_NAME` khi truy cập các endpoint. Những thông tin này sẽ được cấp riêng và quản lý nghiêm ngặt để bảo vệ dữ liệu.

## Chi Tiết Các Endpoint

### 3.1. Endpoint 1: Truy Vấn Thông Tin Thanh Toán (Get Payment)
- **URL**: `api/v1/payment/get`
- **Phương Thức**: GET
- **Tham Số Query**: `unique_id` (ID duy nhất, hiển thị trên QRCode, app benhvienanhson.com hoặc bảng kê thanh toán)
- **Response**: 
	- `status_code`: Mã trạng thái
	 - `description`: Mô tả trạng thái
    
- **Mô tả chi tiết status code và chi tiết**

| status_code |description 
|--|--|
| 200 | Thành công. Khoản viện phí và thông tin liên quan được trả về.
|400|Yêu cầu không hợp lệ. Thông tin truy vấn không đủ hoặc sai định dạng.
|404|Không tìm thấy. Không có khoản viện phí nào phù hợp với unique_id cung cấp.
|500|Lỗi máy chủ. Lỗi phát sinh từ phía máy chủ khi xử lý yêu cầu.

### Trả về thông tin thanh toán nếu có khoản viện phí cần đóng`(status_code=200).` ###
  - `patient_id`: ID của bệnh nhân.
  - `payment_for`: Mục đích thanh toán.
  - `payment_amount`: Số tiền cần thanh toán.
  - `time_created`: Thời gian tạo khoản phí.
  - `time_expired`: Hạn chót thanh toán.
  - `payment_id`: ID của khoản thanh toán

### 3.2. Endpoint 2: Tạo Giao Dịch Thanh Toán (Create Transaction)
- **URL**: `api/v1/transaction/create`
- **Phương Thức**: POST
- **Payload**:
  - `payment_id`: ID của khoản thanh toán(Lấy từ Endpoint1)
  - `payment_amount`: Số tiền thanh toán(Lấy từ Endpoint1)
  - `bankRefID`: ID tham chiếu ngân hàng (có thể để trống)
- **Response**: Trả về thông tin trạng thái của giao dịch.
  - `status_code`: Mã trạng thái
  - `description`: Mô tả trạng thái
    
**Mô tả chi tiết status code và chi tiết**

| status_code |description 
|--|--|
| 201 | Thành công. Giao dịch tạo thành công.
|400|Yêu cầu không hợp lệ. Thông tin payload không đầy đủ hoặc sai định dạng.
|404|Không tìm thấy. Không có khoản viện phí nào phù hợp với payment_id cung cấp.
|409|Xung đột. Thông tin giao dịch không khớp với khoản viện phí.
|500|Lỗi máy chủ. Lỗi phát sinh từ phía máy chủ khi xử lý yêu cầu.

### Trường Thông Tin Trả Về Trong Response Của Endpoint 2

- `transaction_id`: Một ID duy nhất được tạo ra cho mỗi giao dịch thanh toán. ID này giúp theo dõi và quản lý giao dịch một cách dễ dàng.
    
- `transaction_status`: Trạng thái của giao dịch, ví dụ như `Đang xử lý`, `Thành công`, `Thất bại`, `Hủy bỏ`.
    
- `payment_id`: ID của khoản thanh toán liên quan, nhằm xác nhận giao dịch này liên kết với khoản thanh toán nào.
    
- `amount_processed`: Số tiền thực tế được xử lý trong giao dịch.
    
- `time_processed`: Thời gian giao dịch được xử lý.
    
- `bank_ref_id`: (nếu có) ID tham chiếu của ngân hàng, giúp liên kết giao dịch với các bản ghi ngân hàng cụ thể.
    
- `error_message`: (trong trường hợp `status_code` không phải là `201`) Mô tả chi tiết lỗi hoặc vấn đề gặp phải trong quá trình xử lý giao dịch.
    
- `confirmation_receipt`: (tuỳ chọn) Một bản chụp xác nhận giao dịch, có thể bao gồm thông tin như số tiền, ngày giờ, và trạng thái giao dịch.
    
- `additional_details`: Các thông tin phụ thêm mô tả chi tiết về giao dịch, có thể bao gồm thông tin về bệnh nhân, mục đích thanh toán, và các chi tiết khác liên quan.

## 4. Lưu Ý
- `HOST`, `API_KEY` và `API_NAME` sẽ được gửi riêng cho người có thẩm quyền.
- Đảm bảo rằng `API_KEY` và `API_NAME` được bảo mật và chỉ được sử dụng bởi các bên có thẩm quyền, đúng mục đích.
- Kiểm tra kỹ các tham số trước khi thực hiện gọi API để tránh lỗi.
- Xử lý các trường hợp trả về lỗi từ API một cách thích hợp.

## 5. Hỗ Trợ
- Để nhận hỗ trợ kỹ thuật hoặc giải đáp thắc mắc, vui lòng liên hệ [1nguyenhuuhieu@gmail.com/0946.127.555/Nguyễn Hữu Hiếu].

---

**Lưu ý**: Tài liệu này chỉ mang tính chất tham khảo và có thể thay đổi theo thời gian. Vui lòng kiểm tra thông tin cập nhật từ Trung tâm Y tế huyện Anh Sơn.

## 6. Code tham khảo
Dưới đây là các đoạn mã tham khảo cho cả hai endpoint, sử dụng JavaScript (với Fetch API) và Python (với thư viện `requests`). Chú ý rằng đây chỉ là các ví dụ cơ bản và bạn cần thay thế các giá trị như `API_KEY`, `API_NAME`, và các tham số cụ thể khác với thông tin thực tế của mình.
## JavaScript

**Endpoint 1: Lấy Thông Tin Thanh Toán (Get Payment)**

    const getPaymentInfo = async (uniqueId) => {
        const url = `api/v1/payment/get?unique_id=${uniqueId}`;
        const headers = {
            'API_KEY': 'YOUR_API_KEY_HERE',
            'API_NAME': 'YOUR_API_NAME_HERE'
        };
    
        try {
            const response = await fetch(url, { headers: headers });
            const data = await response.json();
            console.log(data);
            // Xử lý dữ liệu tại đây
        } catch (error) {
            console.error('Error fetching payment info:', error);
        }
    };
    
    getPaymentInfo('UNIQUE_ID_HERE');

**Endpoint 2: Tạo Giao Dịch (Create Transaction)**

    const createTransaction = async (paymentId, paymentAmount, bankRefId = '') => {
        const url = 'api/v1/transaction/create';
        const headers = {
            'API_KEY': 'YOUR_API_KEY_HERE',
            'API_NAME': 'YOUR_API_NAME_HERE',
            'Content-Type': 'application/json'
        };
        const body = JSON.stringify({
            payment_id: paymentId,
            payment_amount: paymentAmount,
            bankRefID: bankRefId
        });
    
        try {
            const response = await fetch(url, {
                method: 'POST',
                headers: headers,
                body: body
            });
            const data = await response.json();
            console.log(data);
            // Xử lý dữ liệu tại đây
        } catch (error) {
            console.error('Error creating transaction:', error);
        }
    };
    
    createTransaction('PAYMENT_ID_HERE', 'PAYMENT_AMOUNT_HERE', 'BANK_REF_ID_HERE');

### Python (Sử Dụng Requests)

#### Cài đặt Thư Viện Requests (nếu chưa có)

Trước hết, bạn cần cài đặt thư viện `requests` nếu chưa có:

    pip install requests

**Endpoint 1: Lấy Thông Tin Thanh Toán (Get Payment)**

    import requests
    
    def get_payment_info(unique_id):
        url = f"api/v1/payment/get?unique_id={unique_id}"
        headers = {
            'API_KEY': 'YOUR_API_KEY_HERE',
            'API_NAME': 'YOUR_API_NAME_HERE'
        }
    
        try:
            response = requests.get(url, headers=headers)
            data = response.json()
            print(data)
            # Xử lý dữ liệu tại đây
        except Exception as error:
            print(f"Error fetching payment info: {error}")
    
    get_payment_info('UNIQUE_ID_HERE')

**Endpoint 2: Tạo Giao Dịch (Create Transaction)**

    import requests
    
    def create_transaction(payment_id, payment_amount, bank_ref_id=''):
        url = "api/v1/transaction/create"
        headers = {
            'API_KEY': 'YOUR_API_KEY_HERE',
            'API_NAME': 'YOUR_API_NAME_HERE',
            'Content-Type': 'application/json'
        }
        payload = {
            'payment_id': payment_id,
            'payment_amount': payment_amount,
            'bankRefID': bank_ref_id
        }
    
        try:
            response = requests.post(url, json=payload, headers=headers)
            data = response.json()
            print(data)
            # Xử lý dữ liệu tại đây
        except Exception as error:
            print(f"Error creating transaction: {error}")
    
    create_transaction('PAYMENT_ID_HERE', 'PAYMENT_AMOUNT_HERE', 'BANK_REF_ID_HERE')
