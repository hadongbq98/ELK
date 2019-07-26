# TÌM HIỂU CHUYÊN SÂU VỀ ELK
##I. Tổng quan
Nền tảng ELK là một giải pháp phân tích log hoàn chỉnh, được xây dựng trên sự kết hợp của ba công cụ nguồn mở - Elaticsearch, Logstash và Kibana. Nó cố gắng giải quyết tất cả các vấn đề và thách thức mà chúng ta đã thấy trong phần trước. ELK sử dụng ngăn xếp mã nguồn mở của Elaticsearch để tìm kiếm sâu và phân tích dữ liệu; Đăng nhập để quản lý ghi nhật ký tập trung, bao gồm vận chuyển và chuyển tiếp log từ nhiều máy chủ, làm đầy log và phân tích cú pháp; và cuối cùng, Kibana cho trực quan hóa dữ liệu mạnh mẽ và đẹp mắt. ELK stack hiện đang được duy trì và hỗ trợ tích cực bởi công ty có tên là Elastic (trước đây là Elaticsearch).
  
  Trong một đường ống dữ liệu ELK Stack điển hình, các bản ghi từ nhiều máy chủ ứng dụng được chuyển qua người gửi Logstash đến một bộ chỉ mục Logstash tập trung. Trình lập chỉ mục Logstash sẽ xuất dữ liệu sang cụm Elaticsearch, được Kibana truy vấn để hiển thị trực quan hóa tuyệt vời và xây dựng bảng điều khiển trên log dữ liệu.
Bắt đầu xem xét về những hệ thống 
### 1. Elasticsearch
 Elaticsearch là một công cụ tìm kiếm nguồn mở phân tán dựa trên Apache Lucene và được phát hành theo giấy phép Apache 2.0 (có nghĩa là nó có thể được tải xuống, sử dụng và sửa đổi miễn phí). Nó cung cấp khả năng mở rộng theo chiều ngang, độ tin cậy và khả năng đa nhiệm cho tìm kiếm thời gian thực. Các tính năng tìm kiếm thông tin có sẵn thông qua JSON qua API RESTful. Các khả năng tìm kiếm được hỗ trợ bởi Apache Lucene Engine, không cho phép nó lập chỉ mục động dữ liệu mà không cần biết cấu trúc trước. Elaticsearch có thể đạt được các phản hồi tìm kiếm nhanh vì nó sử dụng lập chỉ mục để tìm kiếm trên các văn bản.
  Nhiều công ty lớn sử dụng Elasticsearch như GitHub, SoundCloud, NetFlix, LinkedIn,.... và các công ty khác. Các ứng dụng chính của Elasticsearch như: 
 • Có thể mở rộng và có tính sẵn sàng cao
 • Nó cung cấp khả năng tìm kiếm và phân tích thời gian thực
 • Nó cung cấp API RESTful để hoạt động để tra cứu, và tính năng khác nhau, chẳng hạn như tìm kiếm đa ngôn ngữ, định vị địa lý, tự động hoàn thành, đề xuất theo ý nghĩa theo ngữ cảnh và đoạn trích kết quả.
 ### 2. Kibana 
 ### 3. Logstash 
## II. Cài đặt ELK Stack 
 Trong bài tập lần này em dùng OpenJDK 8 để cài đặt ELK
### 1. Cài đặt Elasticsearch 
### 2. Cài đặt Kibana 
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
• Discover: Trang Khám phá cho phép tìm kiếm văn bản miễn phí, tìm kiếm dựa trên trường, tìm kiếm dựa trên phạm vi, v.v.
• Visualize: Trang Visualize cho phép xây dựng nhiều trực quan hóa, chẳng hạn như biểu đồ hình tròn, biểu đồ thanh, biểu đồ đường, v.v., có thể được lưu và sử dụng trong bảng điều khiển sau này.
• Dashboard: Bảng điều khiển thể hiện các bộ sưu tập nhiều trực quan hóa và tìm kiếm, có thể được sử dụng để dễ dàng áp dụng các bộ lọc dựa trên tương tác nhấp chuột và đưa ra kết luận dựa trên nhiều tập hợp dữ liệu.
### 2. Chi tiết hơn về Discover trong giao diện 

### 3. Visualize
### 4. Dashboard 
### 5. Logs
### 6. Uptime
### 7. Monitoring 
## V. Vai trò của Logstash trong ELK
