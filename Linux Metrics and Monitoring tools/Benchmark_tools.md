# Linux Benchmark Tools

## LMbench

LMbench là một bộ microbenchmark có thể được sử dụng để phân tích các cài đặt hệ điều hành khác nhau, chẳng hạn như hệ thống hỗ trợ SELinux so với hệ thống không phải là SELinux

Các điểm chuẩn được bao gồm trong LMbench đo các thói quen khác nhau của hệ điều hành như context switching, local communications, memory bandwidth, và file operations. Sử dụng LMbench khá đơn giản vì chỉ có ba lệnh quan trọng cần biết:

- **make results**: Lần đầu tiên LMbench được chạy, nó sẽ nhắc một số chi tiết về cấu hình hệ thống và những gì nó sẽ thực hiện kiểm tra

- **make rerun**: Sau cấu hình ban đầu và lần chạy chuẩn đầu tiên, sử dụng lệnh **make rerun** chỉ cần lặp lại điểm chuẩn bằng cách sử dụng cấu hình được cung cấp trong quá trình **make results** chạy.

- **make see**: Cuối cùng, sau tối thiểu ba lần chạy, kết quả có thể được xem bằng lệnh **make see**. Các kết quả sẽ được hiển thị và có thể được sao chép vào một ứng dụng bảng tính để phân tích thêm hoặc biểu diễn đồ họa của dữ liệu.

## IOzone

IOzone là một file system benchmark có thể được sử dụng để mô phỏng nhiều kiểu truy cập đĩa khác nhau. Vì các khả năng cấu hình của IOzone là chi tiết, có thể mô phỏng chính xác cấu hình khối lượng công việc được nhắm tới. IOzone ghi một hoặc nhiều file có kích thước khác nhau bằng cách sử dụng variable block sizes.

Mặc dù IOzone cung cấp chế độ benchmarking tự động rất thoải mái, nhưng việc xác định các đặc điểm khối lượng công việc như kích thước tệp, kích thước I/O và mẫu truy cập thường hiệu quả hơn. Nếu một hệ thống tệp phải được đánh giá cho khối lượng công việc của cơ sở dữ liệu, thì sẽ hợp lý khi IOzone tạo một mẫu truy cập ngẫu nhiên vào một tệp lớn ở kích thước khối lớn thay vì truyền một tệp lớn với kích thước khối nhỏ. Một số option quan trọng cho IOzone:

| Option | Descript | 
|--------|----------|
| -b <output.xls> | Lưu kết quả trong file Excel |
| -C | Hiển thị đầu ra cho từng child process |
| -f <filename> | Cho IOzone biết nơi để ghi dữ liệu |
| -i <number of test> | Chỉ định loại thử nghiệm nào được chạy. **0** để ghi test file lần đầu, **1** cho streaming reads, **2** cho random read và random write access, **8** cho workload với mixed random access |
| -h | Hiển thị trợ giúp |
| -r | Cho IOzone biết bản ghi hoặc kích thước I / O nào nên được sử dụng cho các bài test. Kích thước bản ghi phải càng gần càng tốt với kích thước bản ghi sẽ được sử dụng bởi khối lượng công việc được nhắm mục tiêu. |
| -k <number of async I/Os> | Sử dụng tính năng async I/O của kernel 2.6 |
| -m | Nếu ứng dụng được nhắm mục tiêu sử dụng nhiều bộ đệm nội bộ thì hành vi này có thể được mô phỏng bằng cờ -m |
| -s <size in KB> | Chỉ định kích thước tệp cho benchmark |
| -+u | Là một switch thử nghiệm có thể được sử dụng để đo mức độ sử dụng của bộ xử lý trong quá trình thử nghiệm |

Sử dụng IOzone để đo hiệu suất đọc ngẫu nhiên của một disk subsystem nhất định được gắn tại /perf cho tệp có kích thước 10 GB ở kích thước I/O 32 KB (các mô hình đặc điểm này một cơ sở dữ liệu đơn giản) sẽ như sau:

```
./iozone -b results.xls -R -i 0 -i 2 -f /perf/iozone.file -r 32 -s 10g
```

Cuối cùng, kết quả thu được có thể được nhập vào ứng dụng bảng tính của bạn và sau đó chuyển thành biểu đồ. Sử dụng đầu ra đồ họa của dữ liệu có thể giúp phân tích một lượng lớn dữ liệu và xác định xu hướng dễ dàng hơn. Nếu IOzone được sử dụng với kích thước tệp phù hợp với bộ nhớ hoặc bộ nhớ cache của hệ thống thì nó cũng có thể được sử dụng để lấy một số dữ liệu về bộ nhớ cache và thông lượng bộ nhớ. Cần lưu ý rằng do file system overheads, IOzone sẽ chỉ báo cáo 70-80% băng thông của hệ thống.

## netperf

`netperf` là một công cụ benchmark hiệu suất tập trung vào hiệu suất TCP/IP networking. Nó hỗ trợ UNIX domain socket và SCTP benchmarking.

`netperf` được thiết kế theo mô hình client-server. `netserver` chạy trên một target system và `netperf` chạy trên client. `netperf` điều khiển `netserver` và chuyển dữ liệu cấu hình tới `netserver`, generates network traffic, và lấy kết quả từ `netserver` thông qua một control connection được chia ra từ benchmark traffic connection thực. Trong quá trình benchmark, không có giao tiếp nào xảy ra trên kết nối điều khiển nên không có bất kỳ ảnh hưởng nào đến kết quả. Công cụ benchmark netperf cũng có khả năng báo cáo bao gồm báo cáo sử dụng CPU.

`netperf` có thể tạo ra một số loại traffic. Cơ bản có 2 loại: bulk data transfer traffic và request/response type traffic. Nên lưu ý rằng netperf chỉ sử dụng 1 socket một lần. 

