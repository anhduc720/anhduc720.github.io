---
title: High-Performance Load Balancing with NGINX
date: 2019-03-12 19:36:53
slug: high-performance-load-balancing-with-nginx
tags: ["nginx", "load balancer", "reverse proxy"]
status: active
---
Hiện nay, trải nghiệm người dùng internet yêu cầu hiệu năng và thời gian hoạt động. Để đáp ứng những điều này, nhiều instance của cùng một hệ thống được chạy, và tải được phân phối tới chúng. Khi lượng tải tăng lên. Thêm một instance khác được khởi động và đưa vào hệ thống để chia một phần tải, kỹ thuật của kiến trúc này được gọi là mở rộng theo chiều ngang (horizontal scaling). Cơ sở hạ tầng dựa trên phần mềm đang ngày càng phổ biến vì tính linh hoạt của nó, mở ra một thế giới rộng lớn về khả năng. Cho dù trường hợp sử dụng nhỏ như chỉ cần 2 instance hoặc lớn như hàng ngàn instance trên toàn cầu, thì cũng cần một giải pháp cân bằng tải năng động như cơ sở hạ tầng. NGINX đáp ứng nhu cầu này theo một số cách, chẳng hạn như cân bằng tải HTTP, TCP và UDP, mà tôi đề cập trong bài viết này.

Khi cân bằng tải, điều quan trọng là chỉ một instance tương tác đến một khách hàng. Nhiều kiến trúc web hiện đại sử dụng các tầng ứng dụng phi trạng thái, lưu trữ trạng thái trong bộ nhớ hoặc cơ sở dữ liệu dùng chung. Tuy nhiên, không thể áp dụng điều này cho tất cả các ứng dụng. Trạng thái session là vô cùng có giá trị trong các ứng dụng tương tác. Trạng thái này có thể được lưu trữ cục bộ đến máy chủ ứng dụng vì một số lý do; ví dụ, trong các ứng dụng mà dữ liệu đang hoạt động quá lớn đến mức chi phí mạng quá đắt. Khi trạng thái được lưu trữ cục bộ đến một máy chủ ứng dụng, điều cực kỳ quan trọng đối với trải nghiệm người dùng là các yêu cầu tiếp theo được tiếp tục được gửi đến cùng một máy chủ. Một khía cạnh khác của tình huống là các máy chủ không nên được thoát ra cho đến khi session kết thúc. Làm việc với các ứng dụng trạng thái ở quy mô đòi hỏi một bộ cân bằng tải thông minh. NGINX Plus cung cấp nhiều cách để giải quyết vấn đề này bằng cách theo dõi cookie hoặc định tuyến.

Bài viết này cũng đề cập tới session persistence và sự liên quan của nó trong viêch cân bằng tải với NGINX và NGINX Plus

### How distribute load between two or more HTTP servers?

Config upstream block để cân bằng tải giữa 2 HTTP servers bằng NGINX’s HTTP module :

```
upstream backend {
    server 10.10.12.45:80      weight=1;
    server app.example.com:80  weight=2;
}
server {
    location / {
        proxy_pass http://backend;
    }
}
```

cấu hình này sẽ cân bằng tải giữa 2 HTTP server ở cổng 80. Nginx sẽ dựa vào weight parameter để điều phối lượng kết nối giữa 2 server. Giá trị mặc định này là 1.

HTTP upstream module điều khiển cân bằng tải cho HTTP. Module này định nghĩa một set các điểm đích bao gồm Unix socket, địa chỉ IP, và DNS records. Upstream module còn định nghĩa bao nhiêu request đơn được assigned tới từng upstream servers

### How distribute load between two or more TCP servers?

Config upstream block để cân bằng tải giữa 2 TCP servers bằng NGINX’s stream module :

```
stream {
    upstream mysql_read {
        server read1.example.com:3306  weight=5;
        server read2.example.com:3306;
        server 10.10.12.34:3306        backup;
    }

    server {
        listen 3306;
        proxy_pass mysql_read;
    }
}
```

block server ở ví dụ này chỉ định NGINX nghe ở cổng TCP 3306 và cân bằng tải giữa 2 instance MySQL chỉ đọc và liệt kê một instance dự phòng khác. Traffic sẽ được điều hướng tới instance này nếu như 2 instance chính failed.

### How distribute load between two or more UDP servers?

Config upstream block định nghĩa udp để cân bằng tải giữa 2 UDP servers bằng NGINX’s stream module :

```
stream {
    upstream ntp {
        server ntp1.example.com:123  weight=2;
        server ntp2.example.com:123;
    }

    server {
        listen 123 udp;
        proxy_pass ntp;
    }
}
```
Phần cấu hình này cân bằng tải giữa hai máy chủ Giao thức thời gian mạng (NTP) sử dụng giao thức UDP. Chỉ định cân bằng tải UDP đơn giản như sử dụng tham số udp trên listen directive.

Nếu dịch vụ bạn cân bằng tải yêu cầu nhiều gói được gửi qua lại giữa máy khách và máy chủ, bạn có thể chỉ định tham số reuseport. Ví dụ về các loại dịch vụ này là OpenVPN, Giao thức thoại qua Internet (VoIP), giải pháp máy tính để bàn ảo và Bảo mật lớp truyền tải dữ liệu (DTLS). Sau đây là một ví dụ về việc sử dụng NGINX để xử lý các kết nối OpenVPN và ủy quyền chúng cho dịch vụ OpenVPN chạy cục bộ:

```
stream {
    server {
        listen 1195 udp reuseport;
        proxy_pass 127.0.0.1:1194;
    }
}
```

Bạn có thể thắc mắc, "Tại sao tôi cần một bộ cân bằng tải khi tôi có thể có nhiều máy chủ trong DNS A record hoặc SRV record?" Câu trả lời là: chúng ta không những có các thuật toán cân bằng thay thế mà chúng ta còn có thể tải cân bằng chính các máy chủ DNS. Các dịch vụ UDP tạo nên rất nhiều dịch vụ mà ta phụ thuộc vào trong các hệ thống mạng, chẳng hạn như DNS, NTP và VoIP. Cân bằng tải UDP có thể ít phổ biến hơn đối với một số người nhưng cũng hữu ích.

Bạn có thể tìm thấy cân bằng tải UDP trong strem module, giống như TCP và cấu hình nó tương tự. Sự khác biệt chính là chỉ thị lắng nghe xác định rằng open socket để làm việc với datagram. Khi làm việc với datagram, có một số chỉ thị khác có thể áp dụng khi chúng không có trong TCP, chẳng hạn như chỉ thị proxy_response, chỉ định NGINX có thể gửi bao nhiêu phản hồi dự kiến từ upstream server. Theo mặc định, cái này là không giới hạn cho đến khi đạt đến giới hạn proxy_timeout.

Tham số reuseport hướng dẫn NGINX tạo một socket nghe riêng cho mỗi worker process. Điều này cho phép kernel phân phối các kết nối đến giữa các worker process để xử lý nhiều gói được gửi giữa máy khách và máy chủ. Tính năng reuseport chỉ hoạt động trên Linux kernels 3.9 trở lên, DragonFly BSD và FreeBSD 12 trở lên.

### What load-balancing method should I use?

Nếu Cân bằng tải kiểu round-robin không phù hợp với trường hợp của bạn vì bạn có khối lượng công việc hoặc các máy chủ không đồng nhất, hãy sử dụng một trong những phương thức cân bằng tải sau:
least connections, least time, generic hash, IP hash, or random

```
upstream backend {
    least_conn;
    server backend.example.com;
    server backend1.example.com;
}
```

Ví dụ trên sử dụng cân bằng tải theo thuật toán ít kết nối nhất (least connections). Tất cả các thuật toán cân bằng tải, trừ generic hash, random, and least-time đều là chỉ thị độc lập. 

Không phải tất cả yêu các yêu cầu hoặc gói tin đều mang trọng lượng như nhau.Với điều này, round-robin, hoặc thậm chí là round-robin có trọng số được sử dụng trong các ví dụ trước, sẽ không phù hợp với ứng dụng hoặc luồng lưu lượng. NGINX cung cấp một số thuật toán cân bằng tải mà bạn có thể sử dụng để phù hợp với các trường hợp sử dụng cụ thể. Ngoài việc có thể chọn các thuật toán hoặc phương pháp cân bằng tải này, bạn cũng có thể cấu hình chúng. Các phương pháp cân bằng tải sau đây có sẵn cho các nhóm HTTP, TCP và UDP upstream.

### Round robin

Đây là thuật toán cân bằng tải mặc đinh. Nó phân phối lưu lượng theo thứ tự cho một danh sách các server trong pool. Bạn có thể thêm vào trọng số để trở thành weighted round-robin, nếu như khả năng phục vụ request của các server trong pool là khác nhau. giá trị trọng số của một server càng cao, càng nhiều request được chuyển tới server đó.

### Least connections

Phương thức này cân bằng tải bằng cách chuyển request tới server có số lượng kết nối đang mở thấp nhất. Cũng giống như round-robin, bạn có thể thêm trọng số vào server để điều chỉnh server đích sẽ nhận request. 
Tên chỉ thị là ```least_conn```.

### Least time

Chỉ có trên NGINX Plus.Nó gần giống với leat connection trong đó nó Chuyển kết nối cho server với số lượng kết nối hiện tại ít nhất nhưng ưu tiên các máy chủ có thời gian phản hồi trung bình thấp nhất. Phương pháp này là một trong những thuật toán cân bằng tải tinh vi nhất và phù hợp với nhu cầu của các ứng dụng web có hiệu suất cao. Thuật toán này tốt hơn ```Least connections``` vì số lượng kết nối nhỏ nhất không nhất thiết có nghĩa là phản ứng nhanh nhất. Một tham số của ```header``` hoặc ```last_byte``` phải được chỉ định cho lệnh này. Khi ```header``` được chỉ định, thời gian để nhận tiêu đề phản hồi được sử dụng. Khi ```last_byte``` được chỉ định, thời gian nhận phản hồi đầy đủ được sử dụng. 
Tên chỉ thị là ```less_time```.

### Generic hash

NGINX lựa chọn server dựa trên một key được user định nghĩa trước. vi dụ IP nguồn của request($remote_addr)

```
upstream stream_backend {
    hash $remote_addr;
    server backend1.example.com:12345;
    server backend2.example.com:12345;
    server backend3.example.com:12346;
}
```
Phương thức cân bằng tải kiểu băm cũng được sử dụng để lưu trữ session. Vì hash function dựa trên IP của máy khách, tất cả các kết nối đến từ một client sẽ luôn luôn được chuyển tới cùng một server trừ khi server đó bị tắt hoặc không khả dụng.
Tên chỉ thị là ```hash```.


### Random
Mỗi kết nối được chuyển một cách ngẫu nhiên tới server đích. nếu chỉ thị có param ```two```. Đầu tiên, NGINX sẽ chọn ngẫu nhiên ra 2 server. Sau đó, chọn 1 trong 2 server dựa theo các phương thức sau:

- least_conn – Ít kết nối đang hoạt động nhất
- least_time=connect (NGINX Plus) – thời gian kết nối tới server thấp nhất ($upstream_connect_time).
- least_time=first_byte (NGINX Plus) – thời gian nhận byte data đầu tiên thấp nhất ($upstream_first_byte_time).
- least_time=last_byte (NGINX Plus) – thời gian nhận byte data cuối cùng thấp nhất ($upstream_last_byte_time) 

```
upstream stream_backend {
  random two least_time=last_byte;
  server backend1.example.com:12345;
  server backend2.example.com:12345;
  server backend3.example.com:12346;
  server backend4.example.com:12346;
}
```

Phương thức cân bằng tải ngẫu nhiên thường đượch sử dụng trong nhưng môi trường phân tán, nơi mà nhiều bộ cân bằng tải đồng thời chuyển request tới một số backend servers. Trong những môi trường mà bộ cân bằng tải có cái nhìn toàn cảnh về các request, Ta nên sử dụng các phương thức cân bằng tải khác như round-robin, least connection, hay lest time.
