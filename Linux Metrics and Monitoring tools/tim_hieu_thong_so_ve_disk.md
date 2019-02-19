# Tìm hiểu các thông số IOPS, Latency, Throughput

## IOPS 

- IOPS - Input/ Output operation second - Số lần đọc/ghi dữ liệu mỗi giây.

- Ở các thiết bị lưu trữ file thì băng thông (MBps) là thông số quan trọng nhất. Còn đối với các thiết bị lưu trữ cho đám mây Cloud thì IOPS quyết định độ "nhạy" và độ "nhanh" của máy ảo.

- Đây là giá trị cho phép xác định trước máy ảo của bạn kiểm soát được bao nhiêu hoạt động nhập/xuất được phép cùng một lúc trên máy ảo của bạn. Sau khi đạt đến ngưỡng cho phép, máy chủ ảo có thể bắt đầu điều tiết các hoạt động này, tạo ra các yêu cầu và quá trình chờ đợi. Điều này lần lượt gây ra tình trạng "ngủ", làm tăng tải máy chủ cho đến khi các yêu cầu được xử lý hết. Các quá trình chờ đợi trong thời gian này bị ảnh hưởng bởi "IOWait"

- Nói dễ hiểu, IOPS càng cao thì tốc độ xử lý càng nhanh, số tác vụ xử lý được sẽ nhiều hơn, hiệu năng của ứng dụng trên Cloud Server sẽ cao hơn. Tuy nhiên, có trường hợp IOPS quá cao đến giới hạn vật lý sẽ gây ra tình trạng thắt cổ chai (IOPS quá cao --> Latency cao --> giảm Throughput)

## Latency

- Là tốc độ xử lý 1 yêu cầu I/O của hệ thống

- Khái niệm này rất quan trọng bởi vì một hệ thống lưu trữ mặc dù có 1000 IOPS với thời gian trung bình xử lý Latency 5ms vẫn tốt hơn một hệ thống với 5000 IOPS nhưng Latency là 50ms. Đặc biệt đối với các ứng dụng cần có thông số Latency cao như database.

- Latency càng thấp càng tốt để đáp ứng nhiều truy cấp dữ liệu khi có nhiều tác vụ cùng một lúc.

## Throughput

- Dữ liệu được truyền đi mỗi giây 

- Càng cao càng tốt 

Thực tế thay vì đặt câu hỏi "Hệ thống lưu trữ có bao nhiêu IOPS" thì nên hỏi rằng "Thời gian phản hồi các truy xuất là bao nhiêu?". Latency được xem là thống số hữu ích nhất, vì nó tác động trực tiếp lên hiệu năng của hệ thống lưu trữ, là yếu tố chính kết hợp với IOPS và Throughput.

## Tham khảo

https://viettelidc.com.vn/tin-tuc/iops-la-gi-va-tai-sao-no-lai-quan-trong
https://viettelidc.com.vn/tin-tuc/tim-hieu-ve-cac-thong-so-iops-latency-va-throughput
https://exa.vn/yeu-to-nao-quyet-dinh-hieu-nang-cua-he-thong-luu-tru-1601