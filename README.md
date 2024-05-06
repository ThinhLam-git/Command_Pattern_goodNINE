![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8d18f7e3-0f97-40e4-8aff-4ecd3bbe2b50/760eac02-bf52-491f-93fc-c508db883fbc/Untitled.png)

## **Giới thiệu**

*“Encapsulate a resquest as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operation.”*

- Command (hay còn gọi là Action, Transaction) là một mẫu thiết kế trong nhóm Behavior.
- Là một pattern cho phép bạn chuyển đổi một request thành một object độc lập chứa tất cả thông tin về request. Việc chuyển đổi này cho phép bạn tham số hóa (parameterize) các methods với các yêu cầu khác nhau như log, queue (undo/redo), transaction.
- Khái niệm Command Object giống như **một class trung gian** được tạo ra để lưu trữ các lệnh và trạng thái của object tại một thời điểm nào đó.
- Command có nghĩa là ra lệnh. Commander là người chỉ huy, người này **không làm mà chỉ ra lệnh** cho người khác làm. Như vậy, phải có người nhận lệnh và thi hành lệnh. Người ra lệnh **cần cung cấp một class đóng gói những mệnh lệnh**. Người nhận mệnh lệnh cần **phân biệt** những interface nào để thực hiện đúng mệnh lệnh.
- Tần suất sử dụng pattern này: Khá cao.

## Mục đích ra đời

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8d18f7e3-0f97-40e4-8aff-4ecd3bbe2b50/a00c1ade-3d10-46a5-b325-c9e20d5eed53/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8d18f7e3-0f97-40e4-8aff-4ecd3bbe2b50/1ea8b98b-afc5-470f-b953-14a91a8d771d/Untitled.png)

- Trong thiết kế hướng đối tượng - OOP, đôi khi chúng ta cần gửi các requests cho các objects mà không cần biết bất cứ điều gì về hoạt động được yêu cầu hoặc người nhận yêu cầu. Chẳng hạn chúng có một ứng dụng văn bản, khi click lên button undo/redo, save, … yêu cầu sẽ được chuyển đến hệ thống xử lý, chúng ta sẽ không thể biết được object nào sẽ nhận xử lý, các nó thực hiện như thế nào. Command Pattern là một pattern được thiết kế cho những ứng dụng như vậy, giúp chúng ta:
    - Tránh các hard-wired (kết nối cứng). Việc triển khai hard-wired vào 1 lớp là không linh hoạt.
    - Không nên để lớp đối tượng **phụ thuộc** cụ thể vào một yêu cầu nào đó
    - Cần đưa ra yêu cầu cho đối tượng mà không cần biết bất cứ gì về hoạt động được yêu cầu cũng như cụ thể nơi nhận yêu cầu.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8d18f7e3-0f97-40e4-8aff-4ecd3bbe2b50/bc016cc3-f33d-4e17-9261-d81dc528dcc4/Untitled.png)
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8d18f7e3-0f97-40e4-8aff-4ecd3bbe2b50/733b55fd-e54b-4542-a518-758f26ec560a/Untitled.png)
    

## **Ưu và nhược điểm**

### **Ưu điểm**

- Đảm bảo nguyên tắc Single Responsibility
- Đảm bảo nguyên tắc Open/Closed
- Có thể thực hiện hoàn tác
- Giảm kết nối phụ thuộc giữa Invoker và Receiver
- Cho phép đóng gói yêu cầu thành đối tượng, dễ dàng chuyển dữ liệu giữa các thành phần hệ thống

### **Nhược điểm**

- Khiến code trở nên phức tạp hơn, sinh ra các lớp mới gây phức tạp cho mã nguồn.

## Khi nào thì sử dụng

- Khi cần tham số hóa các đối tượng theo một hành động thực hiện (biến action thành parameter)
- Khi cần tạo và thực thi các yêu cầu vào các thời điểm khác nhau (delay action)
- Khi cần hỗ trợ tính năng undo, log, callback hoặc transaction
- Phối hợp nhiều Command với nhau theo thứ tự

## Cài đặt Command Pattern như thế nào?

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8d18f7e3-0f97-40e4-8aff-4ecd3bbe2b50/db8d34b9-6527-49b5-98d5-ba960dd0ffbd/Untitled.png)

- Các thành phần trong pattern:
    - **Command**: là một interface hoặc abstract class, chứa một phương thức trừu tượng thực thi (execute) một hành động (operation). Resquest sẽ được đóng gói dưới dạng Command.
    - ConcreteCommand: là các implementation của Command. Định nghĩa một sự gắn kết giữa một đối tượng Reciever và một hành động. Thực thi execute() bằng việc gọi operation đang hoãn trên Recieve.
    - Client: tiếp nhận request từ phía người dùng, đóng gói request thành ConcreteCommand thích hợp và thiết lập reciever của nó.
    - Invoker: tiếp nhận ConcreteCommand từ Client và gọi execute() của ConcreteCommand để thực thi request.
    - Reciever: đây là thành phần thực sự xử lý business logic cho case resquest. Trong phương thức execute() của ConcreteCommand chúng ta sẽ gọi method thích hợp trong Reciever.
    
    → Như vậy, **Client** và **Invoker** sẽ thực hiện việc **tiếp nhận** request. Còn việc **thực thi** request sẽ do **Command**, **ConcreteCommand** và **Receiver** đảm nhận.
    
    ## Ví dụ: **Command Pattern trong ứng dụng mở tài khoản ngân hàng**
    
    Một hệ thống ngân hàng cung cấp ứng dụng cho khách hàng (client) có thể mở (open) hoặc đóng (close) tài khoản trực tuyến. Hệ thống này được thiết kế theo dạng module, mỗi module sẽ thực hiện một nhiệm vụ riêng của nó, chẳng hạn mở tài khoản (OpenAccount), đóng tài khoản (CloseAccount). Do hệ thống không biết mỗi module sẽ làm gì, nên khi có yêu cầu client (chẳng hạn clickOpenAccount, clickCloseAccount), nó sẽ đóng gói yêu cầu này và gọi module xử lý.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8d18f7e3-0f97-40e4-8aff-4ecd3bbe2b50/10db7275-b8d9-434f-b7c3-0aab36fa8a47/Untitled.png)
    
    Ứng dụng của chúng ta bao gồm các lớp xử lý sau:
    
    - **Account** : là một request class.
    - **Command** : là một interface của Command Pattern, cung cấp phương thức execute().
    - **OpenAccount**, **CloseAccount** : là các **ConcreteCommand**, cài đặt các phương thức của Command, sẽ thực hiện các xử lý thực tế.
    - **BankApp** : là một class, hoạt động như **Invoker**, gọi **execute()** của **ConcreteCommand** để thực thi request.
    - **Client** : tiếp nhận request từ phía người dùng, đóng gói request thành ConcreteCommand thích hợp và gọi thực thi các Command.
