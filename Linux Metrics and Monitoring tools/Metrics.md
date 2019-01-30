# Tìm hiểu về các metrics của Linux

Linux là một hệ điều hành mở, vì thế nên có sẵn rất nhiều các công cụ đo lường hiệu suất. Nhưng cho dù có sẵn nhiều công cụ, các thông số đo lường này đều cùng một metrics, vì thế hiểu được các metrics sẽ cho phép bạn sử dụng mọi công cụ. Bài này sẽ tìm hiểu về một vài các metrics quan trọng nhất và ý nghĩa của chúng có liên quan đến hiệu suất ra sao.

## 1. Processor metrics

- CPU utilization

Đây có lẽ là metric đơn giản nhất. Nó miêu tả việc sử dụng tổng thể trên mỗi processor. Trong kiến trúc hệ thống IBM, nếu việc sử dụng CPU vượt quá 80% trong một thời gian dài, có thể xảy ra nghẽn processor.

- User time

Mô tả %CPU sử dụng cho tiến trình người dùng, bao gồm cả **nice time**. User time thường được mong muốn giá trị cao bởi vì trong trường hợp đó, hệ thống thực sự hoạt động.

- System time

Mô tả %CPU dành cho các hoạt động của kernel bao gồm IRQ (Interrupt ReQuest) và softirq time. Giá trị system time cao và ổn định có thể chỉ ra việc nghẽn mạng hoặc trình điều khiển. Hệ thống thường nên sử dụng ít thời gian nhất có thể cho kernel time.

- Waiting 

Tổng thời gian CPU dành cho việc chờ đợi hoạt động I/O xảy ra. Như giá trị *blocked*, một hệ thống không nên dành quá nhiều thời gian để chờ hoạt động I/O; nếu không thì bạn nên kiểm tra lại hiệu năng của I/O subsystem.

- Idle time 

Mô tả %CPU mà hệ thống đã không hoạt động khi chờ các task.

- Nice time 

Mô tả %CPU sử dụng vào việc re-nicing các tiến trình là thay đổi thứ tự và mức độ ưu tiên của các tiến trình.

- Load average

Load average không phải tỉ lệ phần trăm, mà là trung bình động của các tổng sau:

	- Số tiến trình trong hàng chờ đang đợi được xử lý.
	
	- Số tiến trình đang đợi uninterruptable task hoàn thành.
	
Đó là, trung bình của tổng các tiến trình TASK_RUNNING và TASK_UNINTERRUPTIBLE. Nếu các tiến trình yêu cầu CPU time bị chặn (nghĩa là CPU không có thời gian để xử lý chúng) load average sẽ tăng. Mặt khác, nếu mỗi tiến trình ngay lập tức được truy cập tới CPU time và không có CPU cycles nào mất, load average sẽ giảm.

- Runable processes

Giá trị này mô tả các tiến trình đã sẵn sàng để được thực thi. Giá trị này không được vượt quá 10 lần số lượng bộ xử lý vật lý trong một thời gian dài; nếu không sẽ xảy ra nghẽn processor.

- Blocked

Các tiến trình không thể thực thi khi đang chờ hoạt động I/O hoàn thành. Blocked processes có thể chỉ bạn tới I/O bottleneck.

- Context switch

Số lượng chuyển đổi giữa các luồng xảy ra trên hệ thống. Số lượng lớn các context switches liên quan đến một số lượng lớn các interrupt có thể báo hiệu các sự cố ứng dụng hoặc trình điều khiển. Các context switches thường không được mong muốn vì bộ đệm CPU được xóa theo từng cái, nhưng một số context switch là cần thiết.

- Interrupt

Giá trị interrupt chứa các hard interrupt và soft interrupt. Hard interrupt có ảnh hưởng xấu hơn đến hiệu năng hệ thống. Giá trị interrupt cao là dấu hiệu của software bottleneck, trong kernel hoặc trình điều khiển. Hãy nhớ rằng giá trị interrupt bao gồm các interrupt gây ra bởi xung nhịp CPU.

## Memory metrics 

- Free memory

So sánh với các hệ điều hành khác, giá trị bộ nhớ trống trong Linux không phải là một nguyên nhân gây lo ngại. Vì Linux kernel phân bố hầu hết bộ nhớ chưa sử dụng làm file system cache, nên trừ đi số lượng buffer và cache từ bộ nhớ đã sử dụng để xác định (hiệu quả) bộ nhớ trống.

- Swap usage

Giá trị này mô tả số lượng swap space đã dùng. Swap usage chỉ cho bạn biết rằng Linux quản lý bộ nhớ thực sự hiệu quả. Swap In/Out là một phương tiện đáng tin cậy để xác định memory bottleneck. Các giá trị trên 200 đến 300 trang mỗi giây trong một khoảng thời gian kéo dài thể hiện có khả năng memory bottleneck.

- Buffer and cache 

Bộ nhớ đệm được phân bố dưới dạng file system và block device cache.

- Slabs

Mô tả việc kernel sử dụng bộ nhớ. Lưu ý rằng các kernel page không thể được page out ra đĩa.

- Active versus inactive memory

Cung cấp cho bạn thông tin về việc sử dụng tích cực bộ nhớ hệ thống. Bộ nhớ inactive có khả năng bị swap out ra đĩa bởi kswapd daemon.

## Network interface metrics

- Packets received and sent

Số liệu này thông báo cho bạn về số lượng gói được nhận và gửi bởi network interface nhất định.

- Bytes received and sent

Giá trị này mô tả số lượng byte đã nhận và gửi bởi network interface nhất định.

- Collisions per second

Giá trị này cung cấp một dấu hiệu về số lượng va chạm xảy ra trên mạng mà giao diện tương ứng được kết nối với. Các giá trị va chạm được duy trì thường liên quan đến nút cổ chai trong cơ sở hạ tầng mạng, không phải máy chủ. Trên hầu hết các mạng được cấu hình đúng, các va chạm rất hiếm trừ khi cơ sở hạ tầng mạng bao gồm các hub.

- Packets dropped

Đây là số lượng gói đã bị hủy bởi kernel, do cấu hình tường lửa hoặc do thiếu bộ đệm mạng.

- Overruns

Overruns biểu thị số lần giao diện mạng hết dung lượng bộ đệm. Metric này nên được sử dụng cùng với giá trị packets dropped để xác định nút cổ chai có thể có trong bộ đệm mạng hoặc độ dài hàng đợi mạng.

- Errors

Số lượng frame được đánh dấu là lỗi. Điều này thường được gây ra bởi sự không phù hợp mạng hoặc cáp mạng bị hỏng một phần. Cáp mạng bị hỏng một phần có thể là một vấn đề hiệu suất đáng kể đối với mạng cáp đồng.

## Block device metrics 

- Iowait

Thời gian CPU đợi hoạt động I/O diễn ra. Giá trị này cao và lâu có khả năng là I/O bottleneck.

- Average queue length

Số lượng yêu cầu I/O chưa xử lý. Nói chung, một hàng đợi từ 2 đến 3 là tối ưu; giá trị cao hơn có thể dẫn tới I/O bottleneck.

- Average wait

Một phép đo thời gian trung bình tính bằng ms cần cho một yêu cầu I/O được thực hiện. Thời gian chờ bao gồm thao tác I/O thực tế và thời gian chờ trong hàng đợi I/O.

- Transfers per second

Mô tả số lượng hoạt động I/O được thực hiện mỗi giây (đọc và ghi). *Transfers per second* metric kết hợp với giá trị *kBytes per second* giúp bạn xác định kích thước transfers trung bình của hệ thống. Kích thước transfers trung bình thường phải phù hợp với kích thước sọc được sử dụng bởi disk subsystem của bạn.

- Blocks read/write per second

Số liệu này mô tả số lần đọc và ghi mỗi giây được thể hiện bằng các khối 1024 byte kể từ kernel 2.6. Các hạt nhân trước đó có thể báo cáo các kích thước khối khác nhau, từ 512 byte đến 4 KB.

- Kilobytes per second read/write

Đọc và ghi từ/đến thiết bị khối tính bằng kilobyte biểu thị lượng dữ liệu thực tế được truyền đến và từ thiết bị khối.