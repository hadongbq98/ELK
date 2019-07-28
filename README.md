# TÌM HIỂU VỀ ELK
## I. Tổng quan
Nền tảng ELK là một giải pháp phân tích log hoàn chỉnh, được xây dựng trên sự kết hợp của ba công cụ nguồn mở - Elaticsearch, Logstash và Kibana. Nó cố gắng giải quyết tất cả các vấn đề và thách thức mà chúng ta đã thấy trong phần trước. ELK sử dụng ngăn xếp mã nguồn mở của Elaticsearch để tìm kiếm sâu và phân tích dữ liệu; Đăng nhập để quản lý ghi nhật ký tập trung, bao gồm vận chuyển và chuyển tiếp log từ nhiều máy chủ, làm đầy log và phân tích cú pháp; và cuối cùng, Kibana cho trực quan hóa dữ liệu mạnh mẽ và đẹp mắt. ELK stack hiện đang được duy trì và hỗ trợ tích cực bởi công ty có tên là Elastic (trước đây là Elaticsearch).
  
  Trong một đường ống dữ liệu ELK Stack điển hình, các bản ghi từ nhiều máy chủ ứng dụng được chuyển qua người gửi Logstash đến một bộ chỉ mục Logstash tập trung. Trình lập chỉ mục Logstash sẽ xuất dữ liệu sang cụm Elaticsearch, được Kibana truy vấn để hiển thị trực quan hóa tuyệt vời và xây dựng bảng điều khiển trên log dữ liệu.

![alt](https://scontent.fhan1-1.fna.fbcdn.net/v/t1.15752-9/67198642_376213409633574_8090849990521389056_n.png?_nc_cat=108&_nc_oc=AQmNkohnEkHoa6SxtWcwEl1zr57fLHrKuxTQAQiyUUp7aTPvoUvBi29rxeHjf_ymA8Y&_nc_ht=scontent.fhan1-1.fna&oh=fca48c63666eb9a32221c9ae4847f707&oe=5DDBD4A6)
 
 **ELK DATA PIPELINE**


Bắt đầu xem xét về những hệ thống 

### 1. Elasticsearch
 Elaticsearch là một công cụ tìm kiếm nguồn mở phân tán dựa trên Apache Lucene và được phát hành theo giấy phép Apache 2.0 (có nghĩa là nó có thể được tải xuống, sử dụng và sửa đổi miễn phí). Nó cung cấp khả năng mở rộng theo chiều ngang, độ tin cậy và khả năng đa nhiệm cho tìm kiếm thời gian thực. Các tính năng tìm kiếm thông tin có sẵn thông qua JSON qua API RESTful. Các khả năng tìm kiếm được hỗ trợ bởi Apache Lucene Engine, không cho phép nó lập chỉ mục động dữ liệu mà không cần biết cấu trúc trước. Elaticsearch có thể đạt được các phản hồi tìm kiếm nhanh vì nó sử dụng lập chỉ mục để tìm kiếm trên các văn bản.
  Nhiều công ty lớn sử dụng Elasticsearch như GitHub, SoundCloud, NetFlix, LinkedIn,.... và các công ty khác. Các ứng dụng chính của Elasticsearch như: 
 * Có thể mở rộng và có tính sẵn sàng cao
 * Nó cung cấp khả năng tìm kiếm và phân tích thời gian thực
 * Nó cung cấp API RESTful để hoạt động để tra cứu, và tính năng khác nhau, chẳng hạn như tìm kiếm đa ngôn ngữ, định vị địa lý, tự động hoàn thành, đề xuất theo ý nghĩa theo ngữ cảnh và đoạn trích kết quả.
 ### 2. Kibana 

Kibana là một nền tảng trực quan hóa dữ liệu được cấp phép Apache 2.0, giúp trực quan hóa mọi loại dữ liệu có cấu trúc và không cấu trúc được lưu trữ trong các chỉ mục của Elaticsearch. Kibana hoàn toàn được viết bằng HTML và JavaScript. Nó sử dụng các khả năng tìm kiếm và lập chỉ mục mạnh mẽ của Elaticsearch được thể hiện thông qua API RESTful của nó để hiển thị đồ họa mạnh mẽ cho người dùng cuối. Từ trí tuệ kinh doanh cơ bản đến gỡ lỗi thời gian thực, Kibana đóng vai trò của mình thông qua việc hiển thị dữ liệu thông qua biểu đồ đẹp, địa mạo, biểu đồ hình tròn, biểu đồ, bảng, v.v.

Kibana giúp dễ dàng hiểu được khối lượng dữ liệu lớn. Giao diện dựa trên trình duyệt đơn giản của nó cho phép bạn nhanh chóng tạo và chia sẻ bảng điều khiển động hiển thị các thay đổi đối với các truy vấn Elaticsearch trong thời gian thực. 

  Một số chức năng của Kibana: 
* Nó cung cấp các phân tích linh hoạt và một nền tảng trực quan hóa cho doanh nghiệp
Sự thông minh.
* Nó cung cấp phân tích thời gian thực, tóm tắt, biểu đồ và gỡ lỗi
khả năng.
* Nó cung cấp một giao diện trực quan và thân thiện với người dùng, rất cao
có thể tùy chỉnh thông qua một số tính năng kéo và thả và sắp xếp
Khi cần thiết.
* Nó cho phép lưu bảng điều khiển và quản lý nhiều hơn một bảng điều khiển.
Bảng điều khiển có thể dễ dàng chia sẻ và nhúng trong các hệ thống khác nhau.
* Nó cho phép chia sẻ ảnh chụp nhanh của log mà bạn đã tìm kiếm thông qua,
và cô lập nhiều giao dịch có vấn đề.

 ### 3. Logstash

 Logstash là một đường ống dữ liệu giúp thu thập, phân tích, xử lý log được viết bằng java  một lượng lớn dữ liệu và sự kiện có cấu trúc và không cấu trúc được tạo ra trên các hệ thống khác nhau. Nó cung cấp các plugin để kết nối với nhiều loại nguồn và nền tảng đầu vào khác nhau và được thiết kế để xử lý hiệu quả các bản ghi, sự kiện và nguồn dữ liệu phi cấu trúc để phân phối thành nhiều loại đầu ra với việc sử dụng các plugin đầu ra của nó, cụ thể là tệp, đầu ra (như đầu ra trên bảng điều khiển đang chạy Logstash) hoặc Elaticsearch. Mỗi dòng log của logstash được lưu trữ đưới dạng json.

Nó có các tính năng sau: 

* Xử lý dữ liệu tập trung: Logstash giúp xây dựng một đường ống dữ liệu có thể tập trung xử lý dữ liệu. Với việc sử dụng nhiều plugin cho đầu vào và đầu ra, nó có thể chuyển đổi rất nhiều nguồn đầu vào khác nhau thành một định dạng chung.

* Hỗ trợ cho các định dạng log tùy chỉnh: Log được viết bởi các ứng dụng khác nhau thường có các định dạng cụ thể dành riêng cho ứng dụng. Logstash giúp phân tích cú pháp và xử lý các định dạng tùy chỉnh trên quy mô lớn. Nó cung cấp hỗ trợ để viết các bộ lọc của riêng bạn cho mã thông báo và cũng cung cấp các bộ lọc sẵn sàng sử dụng.

* Phát triển plugin: Các plugin tùy chỉnh có thể được phát triển và xuất bản, và
có rất nhiều plugin được phát triển tùy chỉnh đã có sẵn.

Logstash giữ nhiệm vụ rất quan trọng bao gồm 3 giai đoạn trong chuỗi xử lý **ELK pipeline**, tương ứng với 3 module: 

* **INPUT**: tiếp nhận/thu thập dữ liệu logs event ở dạng thô từ các nguồn khác nhau như file, redis, beat, syslog,....

* **FILTER**: sau khi tiếp nhận dữ liệu sẽ tiến hành thao tác dữ liệu logs event (như thêm, xóa,.. nội dung log) theo cấu hình của quản trị viên để xây dựng lại cấu trúc dữ liệu log event theo ý của mình.

* **OUTPUT**: sau cùng sẽ thực hiện chuyển tiếp dữ liệu logs event về các dịch vụ khác như Elasticsearch tiếp nhận và lưu trữ log.

## II. Vai trò và chức năng của Elasticsearch trong ELK
Như đã nói ở mục trước, một số kiến trúc phân tán lớn như GitHub, StackOverflow và Wikipedia sử dụng tìm kiếm toàn văn bản, tìm kiếm có cấu trúc, khả năng phân tích nhanh để đưa ra tìm kiếm phù hợp.
### 1. Các khái niệm trong Elasticsearch
 * **Index**
Index trong Elaticsearch là một tập hợp các tài liệu có chung một số đặc điểm chung.
Mỗi chỉ mục chứa nhiều loại, lần lượt chứa nhiều tài liệu và mỗi tài liệu chứa nhiều trường. Một chỉ mục bao gồm nhiều tài liệu JSON trong Elaticsearch. Có thể có bất kỳ số lượng chỉ mục nào trong một cụm trong Elaticsearch.
Trong ELK, khi JSON document của Logstash được gửi đến Elasticsearch, chúng được gửi đi như 1 chỉ mục mặc định dưới dạng `"logstash-%{+YYYY.MM.dd}"`.Nó phân vùng các chỉ mục theo ngày để có thể dễ dàng tìm kiếm và xóa nếu cần. Mẫu này có thể được thay đổi trong cấu hình plugin đầu ra Logstash.
 * **Document** 
Một document trong Elaticsearch là một JSON document được lưu trữ trong một chỉ mục. Mỗi tài liệu có một loại và ID tương ứng, đại diện cho nó duy nhất. Ví dụ:
              
        {
         "_index" : "packtpub",
         "_type" : "elk",
         "_id" : "1",
         "_version" : 1,
         "found" : true,
         "_source":{
         book_name : "learning elk"
         }
        }
  * **Field (trường)**
Một trường là một đơn vị cơ bản trong một tài liệu. Như trong ví dụ trước, trường cơ bản là cặp giá trị khóa như sau: `book_name : "learning elk"`
 * **Type (Kiểu)**
 Được sử dụng để cung cấp một phân vùng hợp lý bên trong các chỉ mục. Về cơ bản nó đại diện cho một lớp các loại tài liệu tương tự. Một chỉ mục có thể có nhiều loại và chúng ta có thể định nghĩa chúng theo ngữ cảnh. Chẳng hạn, chỉ mục cho Facebook có thể có bài đăng là một trong những loại chỉ mục, bình luận loại khác. 
 * **Mapping (Ánh xạ)**
Ánh xạ được sử dụng để ánh xạ từng trường của tài liệu với kiểu dữ liệu tương ứng của nó, chẳng hạn như chuỗi, số nguyên, float, double, date, v.v. Elaticsearch tạo tự động ánh xạ cho các trường trong quá trình tạo chỉ mục và những ánh xạ đó có thể dễ dàng được truy vấn hoặc sửa đổi dựa trên các loại nhu cầu cụ thể.
 * **Shard (Phân đoạn)**
Phân đoạn là thực thể vật lý thực tế nơi dữ liệu cho mỗi chỉ mục được lưu trữ. Mỗi chỉ mục có thể có một số phân đoạn chính và bản sao nơi lưu trữ dữ liệu. Các phân đoạn được phân phối giữa tất cả các nút trong cụm và có thể được di chuyển từ nút này sang nút khác trong trường hợp lỗi nút hoặc thêm các nút mới.
 * **Primary shard and replica shard (Phân đoạn chính và bản sao)**
Mỗi tài liệu trong một chỉ mục Elaticsearch được lưu trữ trên một phân đoạn chính và một số phân đoạn sao chép. Trong khi lập chỉ mục, tài liệu đầu tiên được lưu trữ trên phân đoạn chính và sau đó trên phân đoạn bản sao tương ứng. Theo mặc định, số lượng phân đoạn chính cho mỗi chỉ mục là năm và có thể được định cấu hình theo nhu cầu của chúng tôi.
Các phân đoạn sao chép thường sẽ nằm trên một nút khác với phân đoạn chính và trợ giúp trong trường hợp chuyển đổi dự phòng và cân bằng tải để phục vụ cho nhiều yêu cầu.
 * **Cluster (Cụm)**
Một cụm là một tập hợp các nút lưu trữ dữ liệu được lập chỉ mục. Elaticsearch cung cấp khả năng mở rộng theo chiều ngang với sự trợ giúp của dữ liệu được lưu trữ trong cụm. Mỗi cụm được đại diện bởi một tên cụm, mà các nút khác nhau tham gia. Tên cụm được đặt bởi một thuộc tính được gọi là `cluster.name` trong cấu hình elaticsearch.yml, mặc định là `"elaticsearch"`:
   `cluster.name: elasticsearch`

 * **Node (Nút)**
 Một nút là một phiên bản chạy duy nhất của Elaticsearch, thuộc về một trong các cụm. Theo mặc định, mọi nút trong Elaticsearch đều tham gia cụm có tên là `"elaticsearch"`. Mỗi nút có thể có cấu hình riêng được xác định trong elasticsearch.yml, chúng có thể có các cài đặt khác nhau về phân bổ tài nguyên và bộ nhớ.
Trong Elasticsearch, nút có 3 vai trò: 
  * Data node: Các nút dữ liệu lập chỉ mục tài liệu và thực hiện tìm kiếm trên các tài liệu được lập chỉ mục. Chúng tôi luôn khuyến nghị thêm nhiều nút dữ liệu để tăng hiệu suất hoặc chia tỷ lệ cụm. Một nút có thể được tạo thành một nút dữ liệu bằng cách đặt các thuộc tính này trong cấu hình elasticsearch.yml cho nút:
*node.master = false*
*node.data=true*

  * Master node: master node Các nút chủ chịu trách nhiệm quản lý một cụm. Đối với các cụm lớn, nên có ba nút chính chuyên dụng (một chính và hai sao lưu), chỉ hoạt động như các nút chính và không lưu trữ các chỉ mục hoặc thực hiện tìm kiếm. Một nút có thể được cấu hình để trở thành một nút chủ chuyên dụng với cấu hình này trong elasticsearch.yml:
*node.master =true*

*node.data=false*
  * Routing node or load balancer node: Các nút này không đóng vai trò là nút chính hoặc nút dữ liệu, mà chỉ thực hiện cân bằng tải hoặc định tuyến các yêu cầu tìm kiếm hoặc lập chỉ mục tài liệu cho các nút thích hợp. Điều này rất hữu ích cho các tìm kiếm khối lượng lớn hoặc hoạt động chỉ mục. Một nút có thể được cấu hình để trở thành một nút định tuyến với cấu hình này trong elasticsearch.yml:
  *node.master =false*

  *node.data=false*
### 2. API Elasticsearch
Trong ELK, mặc dù Logstash và Kibana hoạt động như một giao diện để nói chuyện với các metric của Elaticsearch, nhưng vẫn cần phải hiểu cách Logstash và Kibana sử dụng API của Elaticsearch RESTful để thực hiện các hoạt động khác nhau, như tạo và quản lý các chỉ mục, lưu trữ và truy xuất tài liệu, và hình thành các loại truy vấn tìm kiếm khác nhau xung quanh metric. Nó cũng thường hữu ích để biết cách xóa các chỉ mục.
Elaticsearch cung cấp một API mở rộng để thực hiện các hoạt động khác nhau. Cú pháp chung của truy vấn cụm từ dòng lệnh như sau:
 `$curl -X<VERB>'<PROTOCOL>://<HOST>:<PORT>/<PATH>/<OPERATION_NAME>?<QUERY_STRING>' - d '<BODY>' `
 Khái niệm từng phần trong câu lệnh trên: 
  * VERB: Điều này có thể lấy các giá trị cho loại phương thức yêu cầu: *GET, POST, PUT, DELETE, HEAD*. 
  * *PROTOCOL*: Đây là http hoặc https.
  * *HOST*: Đây là tên máy chủ của nút trong cụm. Đối với cài đặt cục bộ, đây có thể là 'localhost' hoặc `'127.0.0.1'`.
  * *PORT*: Đây là cổng mà đối tượng Elaticsearch hiện đang chạy. Mặc định là `9200`.
  * *PATH*: Điều này tương ứng với tên của chỉ mục, loại và ID sẽ được truy vấn, ví dụ: `/ index / type / id`.
  * *OPERATION_NAME*: Điều này tương ứng với tên của hoạt động sẽ được thực hiện, ví dụ: _search, _count, v.v.
  * *QUERY_STRING*: Đây là một tham số tùy chọn được chỉ định cho các tham số chuỗi truy vấn. Ví dụ :? pretty để in các tài liệu JSON.
  * *BODY*: Điều này thực hiện một yêu cầu cho văn bản cơ thể.
  
**Trạng thái của cluster**.
 * **Màu đỏ** biểu thị rằng một số hoặc tất cả các phân đoạn chính chưa sẵn sàng để phục vụ các yêu cầu.
 * **Màu vàng** biểu thị rằng tất cả các phân đoạn chính được phân bổ nhưng một số hoặc tất cả các bản sao chưa được phân bổ. Thông thường, các cụm nút đơn sẽ có trạng thái màu vàng vì không có nút nào khác có sẵn để sao chép.
 * **Màu xanh lá cây** biểu thị rằng tất cả các phân đoạn và bản sao của chúng được phân bổ tốt và cụm hoạt động đầy đủ.

### 3. Truy vấn Elasticsearch DSL
Các truy vấn mà chúng tôi thấy cho đến bây giờ là các lệnh cơ bản được sử dụng để truy xuất dữ liệu, nhưng sức mạnh thực sự của truy vấn của Elaticsearch nằm trong **Query Domain Specific Language** dựa trên JSON cũng được gọi là Truy vấn DSL. Kibana sử dụng rộng rãi truy vấn DSL để có kết quả ở định dạng mong muốn cho bạn. Bạn gần như không bao giờ thực sự phải lo lắng về việc viết JSON truy vấn, vì Kibana sẽ tự động tạo và đặt kết quả ở định dạng đẹp.

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
![alt](https://scontent.fhan5-3.fna.fbcdn.net/v/t1.15752-9/67192818_2371365449606379_1490133706436771840_n.png?_nc_cat=106&_nc_oc=AQk70gkmbrZqYP5Q5_ZiNPqogn75yUYujQvcYqimvNTl_cEWsrFMiT5FnadMsQ1w_e8Z-KXXaJa3IefG_lFblOvU&_nc_ht=scontent.fhan5-3.fna&oh=03c79f602816e6b4c9e9a7e2ddfb9165&oe=5DA47100)

### Mục Discover ở máy em 

  **Discover** : Trang Khám phá cho phép tìm kiếm văn bản miễn phí, tìm kiếm dựa trên trường, tìm kiếm dựa trên phạm vi, v.v.
Ở đây hiển thị tất cả các trường trong Index Pattern ở bên trái, Time Filter ở phía trên, và Query Bar  để nhập truy vấn. Ngoài ra có biểu đồ mặc định dựa trên giá trị `@timestamp` và hiển thị số lần truy cập tài liệu tương ứng với truy vấn. 
Mặc định sẽ hiển thị 500 document mới nhất dựa trên thời gian ở góc trên bên phải 
 * **Time Filter** là câu trả lời cho các loại tìm kiếm này. Bạn có thể lọc dữ liệu trên bất kỳ khoảng thời gian cụ thể nào được chọn từ lịch, được gọi là **Absolute hoặc** làm cho nó **Relative** dựa trên thời gian hiện tại. Ngoài ra còn có một số bộ lọc thời gian nhanh chóng có sẵn để sử dụng.
 Time filter giới hạn kết quả tìm kiếm trong một khoảng thời gian cụ thể , ta có thể đặt bộ lọc thời gian nếu các index của bạn có các sự kiện xảy ra trong khoảng thời gian cụ thể  đó. Mặc định bộ lọc đặt 15 phút trước.
 * **Truy vấn**: 
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

![alt](https://scontent.fhan5-6.fna.fbcdn.net/v/t1.15752-9/67967026_462405147941152_2328743530022830080_n.png?_nc_cat=105&_nc_oc=AQlUmH0OuMSbpdWfK4x97i8feUQhPMX89IHJ4Wb0_J2EhzE4SydIYqB3R7fm9Ti7hx5IRFTHORogI3_HgTYJmZ_p&_nc_ht=scontent.fhan5-6.fna&oh=48c1e014d500597d240478b148becdb0&oe=5DA390E0)

Trang Visualize giúp tạo trực quan hóa dưới dạng biểu đồ và biểu đồ. Những hình ảnh này có thể được lưu và xem riêng lẻ hoặc có thể được sử dụng trong nhiều bảng điều khiển, hoạt động như một bộ sưu tập các hình ảnh trực quan.



Tạo một visualization gồm ba bước trên trang Visualize:
**1**. Chọn một loại trực quan.
**2**. Chọn nguồn dữ liệu (từ tìm kiếm mới hoặc tìm kiếm đã lưu).
**3**. Định cấu hình các tập hợp (số liệu và nhóm) sẽ được sử dụng để hiển thị trên trang Edit.
   
 Loại trực quan. (Visualization types)
Kibana hỗ trợ các loại trực quan: 
* Area chart
* Data table
* Line chart
* Markdown widget
* Metric
* Pie chart
* Tile map
* Vertical bar chart

Trước khi chúng tôi bắt đầu xây dựng trực quan hóa các loại khác nhau, chúng ta hãy hiểu một chút về các tập hợp Elaticsearch, tạo thành xương sống của các visualization trong Kibana

### 4. Timelion

Timelion cho phép bạn so sánh, kết hợp và kết hợp các bộ dữ liệu trên nhiều nguồn dữ liệu với một cú pháp biểu thức dễ làm chủ. Hướng dẫn này tập trung vào Elaticsearch, nhưng bạn sẽ nhanh chóng khám phá ra rằng những gì bạn học được ở đây áp dụng cho mọi hỗ trợ Timelion của nguồn dữ liệu.
![alt](https://scontent.fhan5-5.fna.fbcdn.net/v/t1.15752-9/67207655_2119375551699210_840568418867347456_n.png?_nc_cat=101&_nc_oc=AQmjEhovFNZKk1C7ZAknQralzoMprHf99jutFFinn3pRnfyzi2OrhbOt_XHCVs0tMVGJs9DRddPEc8pxHM7FmBDB&_nc_ht=scontent.fhan5-5.fna&oh=ed8386faa06d752b4651db83858e6258&oe=5DAE88B2)

### 5. Dashboard 

![alt](https://scontent.fhan5-3.fna.fbcdn.net/v/t1.15752-9/67414129_482545312510625_5365385867579883520_n.png?_nc_cat=111&_nc_oc=AQl_YUlZKrGddBAoP70WojoYoh4lhtcwi4jnTV-y7ZjdDSBLAQ7Rh1qd8VSEMDuYJ3hZXGaXfQbF23v3FhxC5ykb&_nc_ht=scontent.fhan5-3.fna&oh=b17b182182473c537bba0de5bffa5d9a&oe=5DAB6BC3)

**Dashboard Kibana** chỉ là một tập hợp các hình ảnh đã lưu hoặc các tìm kiếm đã lưu, có thể được sắp xếp theo bất kỳ thứ tự nào. Hình ảnh có thể được sử dụng trên nhiều bảng điều khiển và các thay đổi sẽ tự động phản ánh đến tất cả chúng. Một bảng điều khiển có thể được lưu và chia sẻ dễ dàng.

Ví dụ:
![alt](https://scontent.fhan5-5.fna.fbcdn.net/v/t1.15752-9/67954331_357470775188054_4321268735122866176_n.png?_nc_cat=101&_nc_oc=AQm5JasrZrIIVMR0PlmGc3Gn5X2jNxR0b8l-Jwe22lF_hhNi-PaPZ11hGt3WoP35nZ-YMGL3bxVCoYCCatb9G96M&_nc_ht=scontent.fhan5-5.fna&oh=88d86a2a48c695e3f6c5279e93e371ac&oe=5DEB22E3)

**Đây là Syslog Dashboard**

### 6. Logs

Sử dụng Logs UI để khám phá nhật ký cho các máy chủ, thùng chứa và dịch vụ phổ biến. Kibana cung cấp một màn hình nhỏ gọn, giống như bàn điều khiển mà bạn có thể tùy chỉnh.

![alt](https://scontent.fhan5-7.fna.fbcdn.net/v/t1.15752-9/67494532_1337516466453234_1031118005913780224_n.png?_nc_cat=100&_nc_oc=AQmKuiDYsRfR_VGAy7YrDxcO2Z4ecmbPfpEei-98rdt3AfQG1HrJFLEF49rFx9CNLQd3x2V33pv2cKRH0q6T3qol&_nc_ht=scontent.fhan5-7.fna&oh=128e4be9d714fed541fec981c8453a51&oe=5DA6DC83)

Ta có thể cấu hình source (configuration source) bằng cách nhấn vào Default ở góc trên bên phải. Sau đó sẽ hiện ra 1 bảng như sau:

![alt](https://scontent.fhan5-3.fna.fbcdn.net/v/t1.15752-9/67344313_466803347202188_4392033041493524480_n.png?_nc_cat=111&_nc_oc=AQlahgUQV230lH-OMMSPqXG7--tM-lm2CiSpYSumJjzwGeg6CzUJG3QKa6aDKRB4ieqE683v1HrUD37I1-vZjG2g&_nc_ht=scontent.fhan5-3.fna&oh=4537949304b5ead9ac3bff68be5601a4&oe=5DA16550)

 * **Name** : Tên của của configuration source
 * **Indices** :Các mẫu của các chỉ số elaticsearch để đọc metric và logs từ đâu tới.
 * **Fields** : Tên của các trường cụ thể trong các chỉ mục cần được biết đến Cơ sở hạ tầng và Logs UIs người dùng để truy vấn và giải thích chính xác dữ liệu.
### 7. Uptime

Tổng quan về  Uptime hoạt động nhằm giúp bạn nhanh chóng xác định và chẩn đoán về sự cố ngừng hoạt động và các sự cố kết nối khác trong mạng hoặc môi trường của bạn. Có một lựa chọn phạm vi ngày là toàn bộ cho Uptime UI; bạn có thể sử dụng lựa chọn này để làm nổi bật phạm vi ngày tuyệt đối hoặc tương đối, tương tự như các tính năng khác của Kibana.

![alt](https://scontent.fhan2-4.fna.fbcdn.net/v/t1.15752-9/67218292_736070376826871_157720806950961152_n.png?_nc_cat=105&_nc_eui2=AeFWiWr75dQTohD4wD0lK6ZmYEjVPcJ02SAj6NZw6u6Pa1provfkjHNfDEW4aQoWs7woXfq7vrdY2RT2q5VRqj0AtpEZmXWQSh1N0DNKA_SckQ&_nc_oc=AQkTNaLkcIjb3o0_Ue8U-YyIFL7qht7IlmZWjN7B5K1dlnt7MFU-Y5C_MmxDYKPimQtMnkaFRLreegpS_JkwmJvb&_nc_ht=scontent.fhan2-4.fna&oh=bbe6cb16609611cc21d650ea220e03f6&oe=5DEAB6D0)

**Filter Bar** được thiết kế để cho phép bạn nhanh chóng xem các nhóm màn hình cụ thể hoặc thậm chí một màn hình riêng lẻ, nếu bạn đã xác định nhiều màn hình. Điều khiển này cho phép bạn sử dụng các tùy chọn bộ lọc tự động, cũng như nhập văn bản bộ lọc tùy chỉnh để chọn các màn hình cụ thể theo trường, URL, ID và các thuộc tính khác.

![alt](https://scontent.fhan2-2.fna.fbcdn.net/v/t1.15752-9/67300776_1137120376482558_8921568211133005824_n.png?_nc_cat=111&_nc_eui2=AeH7vyIYSHfKAbFFnq3eM24BPwXy3R3skJbGwUL85EuOG12yJpHGbk9Y1YpB2YZWDeAWrH3bjL_1cWxZmGR6kHCti9QBPf0u-IuPtCSGJ__x4g&_nc_oc=AQlyViGISuTVQLOZUjXWIIfeXds61mxailD3PW9DaNCcMQaTkQw2dp9mBoxtKCJ_f0-y_F9gSj8srLliPgw9I8QA&_nc_ht=scontent.fhan2-2.fna&oh=d6dae4c813dc311d348030e8d9fcda8b&oe=5DDC085C)

**Snapshot view**  Chế độ xem này nhằm nhanh chóng cung cấp cho bạn cảm giác về tình trạng chung của môi trường mà bạn theo dõi hoặc một tập hợp con của các màn hình đó. Tại đây, bạn có thể thấy tổng số màn hình được phát hiện trong phạm vi ngày Thời gian đã chọn. Ngoài tổng số, số lượng màn hình ở trạng thái lên hoặc xuống được hiển thị, dựa trên kiểm tra cuối cùng được báo cáo bởi Heartbeat cho mỗi màn hình.
  Bên cạnh số đếm, có một biểu đồ hiển thị sự thay đổi theo thời gian trong phạm vi ngày đã chọn.

![alt](https://scontent.fhan2-1.fna.fbcdn.net/v/t1.15752-9/s2048x2048/67387532_1347329138753218_6628602527485001728_n.png?_nc_cat=102&_nc_eui2=AeE03JRmGe2Px9Qe3eXMSGnQrxt2uSqOpbC16OciVhwGfyjch9QACEUbIszN_R8K9z5nVP2iRBm78wldVgX6-xlsySboHb73fKFiHYkTJ6zz6w&_nc_oc=AQm-IHYDjdjPn-GFYQN1NmAEBf_gGaMVpA-syD7sjO0ncpOii1eMjB09kiAMk_Mbvw5oSPFrj-zxf9brvk8iCCew&_nc_ht=scontent.fhan2-1.fna&oh=fe7a4e6237987336f1d94b257e248fd6&oe=5DA7E12E)

**Monitor list** hình hiển thị thông tin ở cấp độ màn hình riêng lẻ. Dữ liệu hiển thị ở đây sẽ hiển thị các màn hình riêng lẻ của bạn và cung cấp nhanh chóng một hình ảnh trực quan sâu hơn cho các máy chủ hoặc điểm cuối.
 Bảng này bao gồm thông tin như trạng thái gần đây nhất, khi màn hình được kiểm tra lần cuối, ID và URL, địa chỉ IP của nó và một biểu đồ thu nhỏ chuyên dụng hiển thị trạng thái kiểm tra theo thời gian.

![alt](https://scontent.fhan2-2.fna.fbcdn.net/v/t1.15752-9/67726142_437866670401290_561896101590859776_n.png?_nc_cat=111&_nc_eui2=AeFs2EXwxcBl0ao0E_dv4Cfk_S8uP2qR1G2Qei93tJLSINl0vyyO667-ikB_Me8PXJC4KLbs1qIIg5u0w5MIve4ItYLCVMLH98ZvvwS9FPJpzw&_nc_oc=AQn0PjvKOJAmoX7JoVUTYmBi0E4W0JWmqAfW0ClTUzodwYaQcXyo65fewuVWdiQ02ei7Gw0QyC1TvXQk9chnwc0F&_nc_ht=scontent.fhan2-2.fna&oh=a58a1f6c006ba0a9de1995fb1e507633&oe=5DB06571)

**Error list** hiển thị tổng hợp các lỗi mà Heartbeat đã ghi lại. Lỗi được hiển thị theo Loại lỗi, Monitor ID và thông báo. Nhấp vào màn hình ID, ID sẽ đưa bạn đến chế độ xem Màn hình tương ứng, có thể cung cấp cho bạn thông tin phong phú hơn về các điểm dữ liệu riêng lẻ dẫn đến các lỗi được hiển thị.

![alt](https://scontent.fhan2-3.fna.fbcdn.net/v/t1.15752-9/67751308_1128699410649509_8094229957984649216_n.png?_nc_cat=109&_nc_eui2=AeEbhL4b91WMMglhjcLBR3vE7clbFqM3ZdCqELMIfoMp_j-aQGmZokMcPLbS4LZbHbbHWhWYmeUQY4SL2vrY8BN6ao7yHYu7dyzVIezWlYIIzA&_nc_oc=AQkjp1RvviiT6qWCtWzuVQn8H7FARBoqirHa8jNQQN9GdZh5269hg__QCB5HFRXcGa1s_nuPAFuYu4G7DG79H7-C&_nc_ht=scontent.fhan2-3.fna&oh=e856975a8d8d0c55e93619ea74afe776&oe=5DE5C32E)

> Đây là hình ảnh nhận được sau khi nhấp vào 1 monitor.

Các biểu đồ và thông tin trên có ý nghĩa như thế nào. 

   **Trang Monitor** sẽ giúp bạn hiểu rõ hơn về hiệu suất của điểm cuối mạng cụ thể. Bạn sẽ thấy một hình ảnh chi tiết về thời lượng yêu cầu của màn hình theo thời gian, cũng như trạng thái *up/down* theo thời gian.

   **Status bar** hiển thị một bản tóm tắt nhanh chóng các thông tin mới nhât đến màn hình của bạn. Bạn có thể xem trạng thái mới nhất của nó, nhấp vào liên kết để truy cập URL được nhắm mục tiêu, xem thời lượng yêu cầu gần đây nhất của nó và xác định lượng thời gian đã trôi qua kể từ lần kiểm tra cuối cùng.
    Bạn có thể sử dụng Status bar để có được bản tóm tắt nhanh về hiệu suất hiện tại, ngoài việc đơn giản là biết trạng thái đang *up* hay *down*.
  
  **Check history** hiển thị tổng số đếm của màn hình này kiểm tra cho phạm vi ngày đã chọn. Bạn cũng có thể lọc các kiểm tra theo trạng thái để giúp tìm các sự cố gần đây trên cơ sở mỗi lần kiểm tra. Bảng này có thể giúp bạn hiểu rõ hơn về các chi tiết chi tiết hơn về các điểm dữ liệu cá nhân gần đây Heartbeat đang ghi nhật ký về máy chủ hoặc điểm cuối của bạn.

### 8. Dev Tools

  Trang Dev Tools chứa các công cụ phát triển mà bạn có thể sử dụng để tương tác với dữ liệu của mình trong Kibana. Bao gồm: 
  * **Console** 
  * **Search Profiler**
  * **Grok Debugger**

  **Console** Bảng điều khiển cho phép bạn tương tác với API REST của Elaticsearch. 

  > Lưu ý: Bạn không thể tương tác với các điểm cuối API Kibana qua Bảng điều khiển.

   Console có 2 phần chính: 
   * Editor, nơi bạn soạn các yêu cầu để gửi tới Elaticsearch.
   * Response pane, hiển thị các phản hồi cho yêu cầu.

  Khi bạn nhập một lệnh trong trình chỉnh sửa, hãy nhấp vào hình tam giác màu xanh lá cây để gửi yêu cầu tới Elaticsearch.

  Bạn có thể chọn nhiều yêu cầu và gửi chúng cùng nhau. Console gửi các yêu cầu tới Elaticsearch từng cái một và hiển thị đầu ra trong response pane. Gửi nhiều yêu cầu sẽ hữu ích hơn khi bạn gỡ lỗi một vấn đề hoặc thử kết hợp truy vấn trong nhiều tình huống.

  **Sử dụng autocomplete**

  Khi gõ một lệnh, Console sẽ đưa ra các đề xuất theo ngữ cảnh. Những đề xuất này có thể giúp bạn khám phá các tham số cho từng API và tăng tốc độ nhập. Để định cấu hình tùy chọn của bạn cho tự động hoàn tất, chọn **Settings**.
  
  **Lấy yêu cầu lịch sử** : Console duy trì một danh sách 500 yêu cầu cuối cùng mà Elaticsearch thực hiện thành công. Để xem các yêu cầu gần đây nhất của bạn, nhấp vào Lịch sử. Nếu bạn chọn một yêu cầu và nhấp vào Áp dụng, Kibana sẽ thêm nó vào trình chỉnh sửa ở vị trí con trỏ hiện tại.

  **Search Profiler** 
   
  Elaticsearch có API lược tả rất tốt, có thể được sử dụng để kiểm tra và phân tích các truy vấn tìm kiếm của bạn. Tuy nhiên, phản hồi là một blob JSON rất lớn,rất khó để phân tích bằng tay.

  Công cụ Query Profiler có thể chuyển đổi đầu ra JSON này thành một trực quan dễ điều hướng, cho phép bạn chẩn đoán và gỡ lỗi các truy vấn thực hiện kém nhanh hơn nhiều.

  **Grok Debugger** : Grok là một cú pháp khớp mẫu mà bạn có thể sử dụng để phân tích văn bản tùy ý và cấu trúc nó. Grok là hoàn hảo để phân tích nhật ký syslog, apache và các nhật ký máy chủ web khác, nhật ký mysql và nói chung, bất kỳ định dạng nhật ký nào thường được viết cho con người và không tiêu thụ máy tính.

  Các mẫu Grok được hỗ trợ trong bộ xử lý Grok ingest node và bộ lọc Grok Logstash.Bạn có thể xây dựng và gỡ lỗi các mẫu Grok trong công cụ Grok Debugger trong Kibana trước khi bạn sử dụng chúng trong các đường ống xử lý dữ liệu của mình. Vì ingest node và Logstash chia sẻ cùng một thư viện mẫu và triển khai Grok, nên bất kỳ mẫu Grok nào bạn tạo trong Grok Debugger sẽ hoạt động trong nút nhập và Logstash.
### 9. Monitoring 

Các tính năng giám sát Kibana phục vụ hai mục đích riêng biệt:

Để trực quan hóa dữ liệu giám sát ELK Stack. Bạn có thể xem dữ liệu về trạng thái và hiệu suất cho Elaticsearch, Logstash và Beats trong thời gian thực, cũng như phân tích hiệu suất trong quá khứ.
Để giám sát chính Kibana và định tuyến dữ liệu đó đến cụm giám sát.
Nếu bạn cho phép giám sát trên ELK Stack, mỗi nút Elaticsearch, nút Logstash, Kibana và Beat được coi là duy nhất dựa trên UUID liên tục của nó, được ghi vào thư mục path.data khi nút hoặc thể hiện bắt đầu.

## V. Logstash trong ELK
Logstash có nhiều plugin để giúp tích hợp nó với nhiều nguồn đầu vào và đầu ra khác nhau.
 
 Tham khảo các plugin của Logstash [tại đây](https://www.elastic.co/guide/en/logstash/master/output-plugins.html)

 ![alt](https://scontent.fhan1-1.fna.fbcdn.net/v/t1.15752-9/67276013_639384169805047_4464807332429168640_n.png?_nc_cat=107&_nc_eui2=AeHpGxvK8IRcdSjA76KUAjQWwQiSVl_BEx1XgMktwW-sXxX7EZBRkSRWltCc0GBhwqEQDFjpYzb8TSiDoXhdMbcmhPeU1SVnBGohQGB3JI-hrQ&_nc_oc=AQn6zh2LHS87YTUjs8fl-fFFeh7AOj5zSJ1k3GK0IyAOCS-j_ca8K3lat7AexXoEAHM&_nc_ht=scontent.fhan1-1.fna&oh=48050f9d8ee795dfa4526a8551a52404&oe=5DA9C270)

Như đã nói ở mục trước, Logstash pipeline gồm 3 giai đoạn là input, filter và output. (input -> filter -> output). Ta sẽ đi vào giai đoạn: 

### INPUT 

 Cấu hình file input xem ở phần cài đặt và cấu hình Logstash, file cấu hình này quy định cơ chế nhận/lấy log vào chương trình Logstash. Ở đây em chỉ dùng plugin beats. Ngoài ra có những plugin:

  * **file**: đọc dữ liệu từ filesystem, giống như lệnh "tail -f" .

  * **syslog**: Logstash tiếp nhận dữ liệu syslog trên cổng 514.  
  * **redis**: đọc dữ liệu từ redis server, sử dụng 2 cơ chế redis channel và redis lists.

Và các input plugin khác. 

### FILTER
 Có thể kết hợp **filter** với các điều kiện so sánh nhằm thực hiện 1 số hành động khi 1 sự kiện khớp với các tiêu chí được đề ra. Các filter plugin hữu ích: 

 * **grok**: hiện là 1 plugin filter tốt nhất để phân tích cú pháp dữ liệu không 
 được cấu trúc văn bản thành dòng có cấu trúc có thể truy vấn được
 
 * **mutate**: thực hiện sự thay đổi trên thông tin sự log như: đổi tên, xóa, thay 
 thế, tinh chỉnh các trường (field) thông tin log event.
 
 * **drop**: dừng xử lý sự kiện ngay lập tức.

 * **clone**: tạo 1 bản copy của sự kiện.

 * **geoip**: thêm thông tin vị trí địa lý của IP (để hiển thị bản đồ trên **Kibana**)

### OUTPUT

Đây là giai đoạn cuối cùng của Logstash. Một sự kiện có thể đi ra nhiều output khác nhau. 

 * **Elasticsearch**: gửi dữ liệu đến hệ thống Elasticsearch (đầu cuối của hệ thống ELK logging để lưu trữ, tìm kiếm log,..)

 * **file**: để lưu file ra hệ thống. 
 
 * **graphite**: gửi dữ liệu tới graphite, là tool mã nguồn mở hỗ trợ lưu trữ và tạo biểu đồ metric
 
 * **statsd**: gửi dữ liệu dịch vụ đến "statsd.