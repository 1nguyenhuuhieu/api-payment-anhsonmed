

# api-payment-anhsonmed
API Payment at Anh Son Medical Center


# Hướng Dẫn Sử Dụng API: Thanh Toán Viện Phí Dành Cho Ngân Hàng Tại Trung Tâm Y Tế Huyện Anh Sơn

## 1. Giới Thiệu
API Thanh Toán Viện Phí của Trung tâm Y tế huyện Anh Sơn cung cấp một giải pháp tiên tiến cho các ngân hàng, giúp đơn giản hóa và tự động hóa quy trình thanh toán viện phí. Được thiết kế với mục tiêu tối ưu hóa trải nghiệm thanh toán cho bệnh nhân, API này hứa hẹn mang lại sự thuận tiện và hiệu quả cao trong quản lý tài chính y tế.

## 2. Yêu Cầu Xác Thực
**API_KEY và API_NAME**: Để đảm bảo an toàn thông tin, mỗi ngân hàng cần cung cấp `API_KEY` và `API_NAME` khi truy cập các endpoint. Những thông tin này sẽ được cấp riêng và quản lý nghiêm ngặt để bảo vệ dữ liệu.

## 3. Chi Tiết Các Endpoint

### 3.1. Truy Vấn Thông Tin Thanh Toán (Get Payment)
- ****Endpoint****: `GET /api/v1/payment/get`
- **Mục đích**: Truy vấn thông tin thanh toán dựa trên mã y tế của bệnh nhân
- **Tham số truy vấn**: 
	- `patient_number` (bắt buộc), mã y tế của bệnh nhân, hiển thị trên QRCode, app benhvienanhson.com hoặc bảng kê thanh toán
- **Headers**:
	-   `X-API-KEY`: API key của bạn.
	-   `X-API-NAME`: Tên API. 

- **Response thành công**: Trả về thông tin bệnh nhân và thanh toán.
	-   Mã trạng thái HTTP 200.
	- **Dữ liệu JSON bao gồm:**

		-   `patient`: Đối tượng chứa thông tin về bệnh nhân.
		   
		    -   `patient_name`: Tên của bệnh nhân.
		    -   `patient_number`: Mã số Y tế của bệnh nhân.
		-   `payment`: Đối tượng chứa thông tin về thanh toán.
		    
		    -   `payment_info`: ID của thông tin thanh toán (`PaymentInfo`).
		    -   `payment_amount`: Tổng số tiền thanh toán.
		    -   `payment_status`: Trạng thái thanh toán hiển thị dưới dạng chuỗi có thể đọc được.

JSON:
```
{
    "patient": {
        "patient_name": "Nguyen Van A",
        "patient_number": "701571.23001647"
    },
    "payment": {
        "payment_info": 1,
        "payment_amount": 1500.00,
        "payment_status": "Hoàn tất khám chữa bệnh, đang chờ thanh toán"
    }
}
```

 
- **Response lỗi**

	| status_code |description
	|--|--|
	|400|Yêu cầu không hợp lệ. Thông tin truy vấn không đủ hoặc sai định dạng.
	|404|Không tìm thấy. không tìm thấy bệnh nhân hoặc không có khoản viện phí cần thanh toán.
	|500|Lỗi máy chủ. Lỗi phát sinh từ phía máy chủ khi xử lý yêu cầu.


### 3.2. Tạo Một Giao Dịch Mới (Create Transaction)
- **Endpoint**: `POST api/v1/transaction/create`
- **Mục đích**: Tạo một giao dịch mới.
- **Dữ liệu đầu vào (JSON)**:
	-   `payment_info` (bắt buộc): ID thông tin thanh toán.
	-   `amount` (bắt buộc): Số tiền giao dịch.
	-   `bankref_id` (tùy chọn): ID tham chiếu ngân hàng.
- **Headers:**
	-   `X-API-KEY`: API key của bạn.
	-   `X-API-NAME`: Tên API.
- **Response thành công**: Trả về thông tin trạng thái của giao dịch.
  - Mã trạng thái HTTP 201
  - **Dữ liệu JSON bao gồm**:
	-   `transaction`: Đối tượng chứa thông tin giao dịch.
	    -   `amount`: Số tiền của giao dịch.
	    -   `bankref_id`: ID tham chiếu ngân hàng (nếu có).
	    -   `notes`: Ghi chú về giao dịch (nếu có).
	    -   `created_time`: Thời gian tạo giao dịch.
	-   `patient`: Đối tượng chứa thông tin bệnh nhân tương tự như ở trên.
	-   `payment`: Đối tượng chứa thông tin thanh toán tương tự như ở trên.

JSON:
```
{
    "transaction": {
        "amount": 500.00,
        "bankref_id": "12345ABC",
        "notes": "Payment for services",
        "created_time": "2021-01-01T12:00:00"
    },
    "patient": {
        "patient_name": "Nguyen Van A",
        "patient_number": "701571.23001647"
    },
    "payment": {
        "payment_info": 1,
        "payment_amount": 1500.00,
        "payment_status": "Thanh toán một phần"
    }
}
```


- **Response lỗi**

| status_code |description 
|--|--|
|400|Yêu cầu không hợp lệ. Thông tin dữ liệu đầu vào không đầy đủ hoặc sai định dạng.
|500|Lỗi máy chủ. Lỗi phát sinh từ phía máy chủ khi xử lý yêu cầu.


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

    const getPaymentInfo = async (patientNumber) => {
        const url = `api/v1/payment/get?patient_number=${patientNumber}`;
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
            payment_info: paymentId,
            amount: paymentAmount,
            bankref_id: bankRefId
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
        url = f"api/v1/payment/get?patient_number={patient_number}"
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
    
    get_payment_info('PATIENT_NUMBER_HERE')

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
            'payment_info': payment_id,
            'amount': payment_amount,
            'bankref_id': bank_ref_id
        }
    
        try:
            response = requests.post(url, json=payload, headers=headers)
            data = response.json()
            print(data)
            # Xử lý dữ liệu tại đây
        except Exception as error:
            print(f"Error creating transaction: {error}")
    
    create_transaction('PAYMENT_INFO_HERE', 'PAYMENT_AMOUNT_HERE', 'BANK_REF_ID_HERE')

