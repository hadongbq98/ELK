# TÌM HIỂU VỀ ELK
##I. Tổng quan
Nền tảng ELK là một giải pháp phân tích log hoàn chỉnh, được xây dựng trên sự kết hợp của ba công cụ nguồn mở - Elaticsearch, Logstash và Kibana. Nó cố gắng giải quyết tất cả các vấn đề và thách thức mà chúng ta đã thấy trong phần trước. ELK sử dụng ngăn xếp mã nguồn mở của Elaticsearch để tìm kiếm sâu và phân tích dữ liệu; Đăng nhập để quản lý ghi nhật ký tập trung, bao gồm vận chuyển và chuyển tiếp log từ nhiều máy chủ, làm đầy log và phân tích cú pháp; và cuối cùng, Kibana cho trực quan hóa dữ liệu mạnh mẽ và đẹp mắt. ELK stack hiện đang được duy trì và hỗ trợ tích cực bởi công ty có tên là Elastic (trước đây là Elaticsearch).
  
  Trong một đường ống dữ liệu ELK Stack điển hình, các bản ghi từ nhiều máy chủ ứng dụng được chuyển qua người gửi Logstash đến một bộ chỉ mục Logstash tập trung. Trình lập chỉ mục Logstash sẽ xuất dữ liệu sang cụm Elaticsearch, được Kibana truy vấn để hiển thị trực quan hóa tuyệt vời và xây dựng bảng điều khiển trên log dữ liệu.
Bắt đầu xem xét về những hệ thống 
### 1. Elasticsearch
 Elaticsearch là một công cụ tìm kiếm nguồn mở phân tán dựa trên Apache Lucene và được phát hành theo giấy phép Apache 2.0 (có nghĩa là nó có thể được tải xuống, sử dụng và sửa đổi miễn phí). Nó cung cấp khả năng mở rộng theo chiều ngang, độ tin cậy và khả năng đa nhiệm cho tìm kiếm thời gian thực. Các tính năng tìm kiếm thông tin có sẵn thông qua JSON qua API RESTful. Các khả năng tìm kiếm được hỗ trợ bởi Apache Lucene Engine, không cho phép nó lập chỉ mục động dữ liệu mà không cần biết cấu trúc trước. Elaticsearch có thể đạt được các phản hồi tìm kiếm nhanh vì nó sử dụng lập chỉ mục để tìm kiếm trên các văn bản.
  Nhiều công ty lớn sử dụng Elasticsearch như GitHub, SoundCloud, NetFlix, LinkedIn,.... và các công ty khác. Các ứng dụng chính của Elasticsearch như: 
 * Có thể mở rộng và có tính sẵn sàng cao
 * Nó cung cấp khả năng tìm kiếm và phân tích thời gian thực
 * Nó cung cấp API RESTful để hoạt động để tra cứu, và tính năng khác nhau, chẳng hạn như tìm kiếm đa ngôn ngữ, định vị địa lý, tự động hoàn thành, đề xuất theo ý nghĩa theo ngữ cảnh và đoạn trích kết quả.
 ### 2. Kibana 
 ### 3. Logstash 
## II. Cài đặt ELK Stack 
 Trong bài tập lần này em dùng OpenJDK 8 để cài đặt ELK
   *$ sudo apt-get install openjdk-8-jre*
   Sau khi cài đặt xong, chạy lệnh: *$ java -version*
### 1. Cài đặt Elasticsearch 
 Để bắt đầu, hãy chạy lệnh sau để nhập khóa GPG công khai của Elaticsearch vào APT:

  `*$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -*`
  
  Tiếp theo, thêm danh sách nguồn Elastic vào thư mục **sources.list.d**.

  `*$ echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list*`

  Tiếp theo, cập nhật danh sách gói của bạn để APT sẽ đọc nguồn Elastic mới:

  `*$ sudo apt-get update*`

  Sau đó tiến hành cài đặt Elasticsearch: 

  `*$ sudo apt-get install elasticsearch*`

  Sau khi hoàn tất việc cài Elasticsearch, chỉnh sửa file cấu hình của Elasticsearch `*/etc/elasticsearch/elasticsearch.yml*`
  Elaticsearch lắng nghe lưu lượng truy cập trên cổng 9200. Bạn sẽ muốn hạn chế quyền truy cập bên ngoài vào đối tượng Elaticsearch của mình để ngăn người ngoài đọc dữ liệu của bạn hoặc tắt Elaticsearch cluster của bạn thông qua API REST. Tìm dòng chỉ định network.host, bỏ ghi chú và thay thế giá trị của nó bằng `*localhost*`.

    network.host: *localhost* 

    network.public_host: *địa chỉ IP* 

Phần port: 

http.port: *9200*

transport.tcp.port: *9300*

Phần discovery:

discovery.type: *single-node*

Lưu và thoát.
 Khởi động Elasticsearch. 
 
 `$ systemctl start elasticsearch`

Kiểm tra Elasticsearch đã chạy hay chưa bằng cách gửi 1 HTTP request.

`*$ curl -X GET "localhost:9200"*`
  

### 2. Cài đặt Kibana 
 bạn chỉ nên cài đặt Kibana sau khi cài đặt Elaticsearch. Cài đặt theo thứ tự này đảm bảo rằng các thành phần mỗi sản phẩm phụ thuộc vào đúng vị trí.
Vì bạn đã thêm nguồn gói Elasticsearch ở bước trước, bạn chỉ có thể cài đặt các thành phần còn lại của Elastic Stack bằng apt:
  
  `*$ sudo apt-get install kibana*`

Sau đó kích hoạt và khởi động Kibana

 `*$ sudo systemctl enable kibana*`

 `*$ sudo systemctl start kibana*`

 Vì Kibana được cấu hình để chỉ nghe trên localhost, chúng ta phải thiết lập `*reserve proxy*` để cho phép truy cập bên ngoài vào nó. Sử dụng Nginx.(ở đây em dùng Nginx version 1.17.1)
 Đầu tiên, sử dụng lệnh *openssl* để tạo người dùng Kibana quản trị mà bạn sẽ sử dụng để truy cập vào giao diện web Kibana. Ví dụ đặt tên tài khoản là **kibanaadmin**.
Lệnh sau sẽ tạo người dùng và mật khẩu Kibana quản trị và lưu trữ chúng trong tệp `*htpasswd.kibana*`. Bạn sẽ định cấu hình Nginx để yêu cầu tên người dùng và mật khẩu này và đọc tệp này trong giây lát: 

   `*echo "kibanaadmin:` `openssl passwd abc123` `" | sudo tee -a /etc/nginx/htpasswd.kibana*`

   Nhập mật khẩu cho tài khoản *kibanaadmin*. 
   Tiếp theo, tạo một tệp khối máy chủ Nginx. Ví dụ, đề cập đến tệp này là **example123.com**, mặc dù bạn có thể thấy hữu ích khi đặt cho bạn một tên mô tả hơn. Ví dụ: nếu có bản ghi FQDN và DNS được thiết lập cho máy chủ này, bạn có thể đặt tên tệp này theo tên FQDN của mình:

   `*$ sudo vi /etc/nginx/conf.d/default*`

   Đảm bảo cập nhật **example123.com** để khớp với FQDN hoặc địa chỉ IP công cộng của máy chủ của bạn. Mã này định cấu hình Nginx để hướng lưu lượng HTTP của máy chủ của bạn đến ứng dụng Kibana, đang lắng nghe trên `*localhost: 5601*`. Ngoài ra, nó cấu hình Nginx để đọc tệp `*htpasswd.kibana*` và yêu cầu xác thực cơ bản.

   *server {*
    *listen 80;*

    *server_name example.com;*

    *auth_basic "Restricted Access";*
    *auth_basic_user_file /etc/nginx/htpasswd.users;*

    *location / {*
        *proxy_pass http://localhost:5601;*
        *proxy_http_version 1.1;*
        *proxy_set_header Upgrade $http_upgrade;*
        *proxy_set_header Connection 'upgrade';*
        *proxy_set_header Host $host;*
        *proxy_cache_bypass $http_upgrade;*
    *}*
*}*

Lưu và thoát.
Kiểm tra cấu hình cài đặt bằng lệnh: `*$ nginx -t*`
Không có lỗi thì hiển thị thông báo `syntax is ok`. Nếu chưa thì kiểm tra lại file cấu hình.
Khởi động lại Nginx: `*$ sudo systemctl restart nginx*`
### 3. Cài đặt Logstash
### 4. Cài đặt Beat
## III. Vai trò của Elasticsearch trong ELK
Như đã nói ở mục trước, một số kiến trúc phân tán lớn như GitHub, StackOverflow và Wikipedia sử dụng tìm kiếm toàn văn bản, tìm kiếm có cấu trúc, khả năng phân tích nhanh để đưa ra tìm kiếm phù hợp. 
### 1. Các khái niệm trong Elasticsearch
### 2. API Elasticsearch
### 3. Truy vấn Elasticsearch DSL
## IV. Vai trò của Kibana trong ELK
Trong phần này, chúng ta sẽ xem Kibana đóng vai trò là đầu mối của ELK, nơi nó che giấu tất cả sự phức tạp của dữ liệu và trình bày các hình ảnh, biểu đồ và bảng điều khiển đẹp mắt được xây dựng trên dữ liệu, giúp hiểu rõ hơn về dữ liệu.
Kibana giúp dễ dàng tạo và chia sẻ bảng điều khiển bao gồm nhiều loại biểu đồ và đồ thị khác nhau. Trực quan hóa Kibana tự động hiển thị các thay đổi trong dữ liệu theo thời gian dựa trên các truy vấn Elaticsearch. Thật dễ dàng để cài đặt và thiết lập, đồng thời giúp chúng tôi nhanh chóng tìm hiểu và khám phá nhiều khía cạnh của dữ liệu.
### 1. Các tính năng của Kibana
**Elasticsearch tổng hợp.**
Chủ yếu có hai loại tổng hợp - Bucketing và Metrics. Bucketing tạo ra một danh sách các nhóm, mỗi nhóm có một bộ tài liệu thuộc về nó, ví dụ: các điều khoản, phạm vi, biểu đồ, v.v. Số liệu tính toán số liệu tính toán cho một tập hợp các tài liệu, ví dụ: min, max, sum, trung bình, v.v. Các loại tính toán này chỉ có thể được thực hiện trên loại trường số.

**Scripted fields.**
Các trường script được sử dụng để thực hiện tính toán nhanh chóng trên dữ liệu được lập chỉ mục. Ví dụ: đối với một trường nhất định, bạn luôn muốn nhân với 100 trước khi hiển thị. Bạn có thể lưu nó dưới dạng một trường script. Các trường theo kịch bản, mặc dù, không thể được tìm kiếm.
Ví dụ về 1 script: doc['volume'].value * 100 .
Kịch bản này sẽ luôn nhân giá trị với 100 trước khi nó hiển thị.

Bảng điều khiển động
Bảng điều khiển rất linh hoạt và năng động vì trực quan hóa cá nhân có thể được sắp xếp dễ dàng theo sự thuận tiện và dữ liệu có thể được làm mới tự động.
**Giao diện Kibana.** 
Giao diện Kibana bao gồm bốn tab chính:
* Discover: Trang Khám phá cho phép tìm kiếm văn bản miễn phí, tìm kiếm dựa trên trường, tìm kiếm dựa trên phạm vi, v.v.
* Visualize: Trang Visualize cho phép xây dựng nhiều trực quan hóa, chẳng hạn như biểu đồ hình tròn, biểu đồ thanh, biểu đồ đường, v.v., có thể được lưu và sử dụng trong bảng điều khiển sau này.
* Dashboard: Bảng điều khiển thể hiện các bộ sưu tập nhiều trực quan hóa và tìm kiếm, có thể được sử dụng để dễ dàng áp dụng các bộ lọc dựa trên tương tác nhấp chuột và đưa ra kết luận dựa trên nhiều tập hợp dữ liệu.
### 2. Chi tiết hơn về Discover trong giao diện 
#### Discover: Trang Khám phá cho phép tìm kiếm văn bản miễn phí, tìm kiếm dựa trên trường, tìm kiếm dựa trên phạm vi, v.v.
Ở đây hiển thị tất cả các trường trong Index Pattern ở bên trái, Time Filter ở phía trên, và Query Bar  để nhập truy vấn. Ngoài ra có biểu đồ mặc định dựa trên giá trị @timestamp và hiển thị số lần truy cập tài liệu tương ứng với truy vấn. 
Mặc định sẽ hiển thị 500 document mới nhất dựa trên thời gian ở góc trên bên phải 
 * Time Filter là câu trả lời cho các loại tìm kiếm này. Bạn có thể lọc dữ liệu trên bất kỳ khoảng thời gian cụ thể nào được chọn từ lịch, được gọi là **Absolute hoặc** làm cho nó **Relative** dựa trên thời gian hiện tại. Ngoài ra còn có một số bộ lọc thời gian nhanh chóng có sẵn để sử dụng.
 * Truy vấn: 
   **Tìm kiếm freetext** nhằm mục đích lọc các tài liệu có chứa cụm từ tìm kiếm. Nó tìm kiếm trong tất cả các tài liệu cho tất cả các trường có chứa thuật ngữ tìm kiếm.
 Các tìm kiếm Boolean có thể được thực hiện theo các thuật ngữ sau:
**AND**
  "Learning" AND "ELK"
Truy vấn trước sẽ tìm kiếm tất cả các tài liệu có chứa cả hai thuật ngữ:
"Learning" và "ELK"
**OR**
"Logstash" OR "ELK"
Truy vấn trước sẽ tìm kiếm tất cả các tài liệu có chứa 1 trong 2 thuật ngữ:
"Logstash" hoặc "ELK".
**NOT**
"Logstash" NOT "ELK"
Truy vấn trước sẽ tìm kiếm tất cả các tài liệu chứa thuật ngữ logstash chứ không phải  thuật ngữ ELK:
**Groupings**
("Logstash" OR "ELK") AND "Kibana"
Truy vấn trước sẽ tìm kiếm các tài liệu có chứa "Kibana" và có thể chứa "ELK" hoặc "Logstash".
   **Tìm kiếm ký tự đại diện**
Bạn cũng có thể thực hiện tìm kiếm ký tự đại diện bằng các thuật ngữ sau:
* plan *: sẽ tìm kiếm tất cả các tài liệu có các điều khoản, chẳng hạn như plant, planting,v.v.
* plan? : sẽ tìm kiếm plant hoặc plans.
* ? and *: không thể được sử dụng làm ký tự đầu tiên trong tìm kiếm
   **Tìm kiếm theo trường.**
Tìm kiếm trường nhằm tìm kiếm các giá trị cụ thể hoặc phạm vi giá trị cho các trường trong tài liệu được lập chỉ mục của bạn hiển thị ở phía bên trái của trang **Discover.**
### 3. Visualize
### 4. Dashboard 
### 5. Logs
### 6. Uptime
### 7. Monitoring 
## V. Vai trò của Logstash trong ELK
