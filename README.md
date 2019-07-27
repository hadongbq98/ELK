# TÌM HIỂU VỀ ELK
## I. Tổng quan
Nền tảng ELK là một giải pháp phân tích log hoàn chỉnh, được xây dựng trên sự kết hợp của ba công cụ nguồn mở - Elaticsearch, Logstash và Kibana. Nó cố gắng giải quyết tất cả các vấn đề và thách thức mà chúng ta đã thấy trong phần trước. ELK sử dụng ngăn xếp mã nguồn mở của Elaticsearch để tìm kiếm sâu và phân tích dữ liệu; Đăng nhập để quản lý ghi nhật ký tập trung, bao gồm vận chuyển và chuyển tiếp log từ nhiều máy chủ, làm đầy log và phân tích cú pháp; và cuối cùng, Kibana cho trực quan hóa dữ liệu mạnh mẽ và đẹp mắt. ELK stack hiện đang được duy trì và hỗ trợ tích cực bởi công ty có tên là Elastic (trước đây là Elaticsearch).
  
  Trong một đường ống dữ liệu ELK Stack điển hình, các bản ghi từ nhiều máy chủ ứng dụng được chuyển qua người gửi Logstash đến một bộ chỉ mục Logstash tập trung. Trình lập chỉ mục Logstash sẽ xuất dữ liệu sang cụm Elaticsearch, được Kibana truy vấn để hiển thị trực quan hóa tuyệt vời và xây dựng bảng điều khiển trên log dữ liệu.

![alt](https://www.netsolutions.com/insights/wp-content/uploads/2017/01/ELK211.jpg)
 **ELK DATA PIPELINE**


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
   `$ sudo apt-get install openjdk-8-jre`
   Sau khi cài đặt xong, chạy lệnh: `$ java -version`
### 1. Cài đặt Elasticsearch 
 Để bắt đầu, hãy chạy lệnh sau để nhập khóa GPG công khai của Elaticsearch vào APT:

  `$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -`
  
  Tiếp theo, thêm danh sách nguồn Elastic vào thư mục **sources.list.d**.

  `$ echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list`

  Tiếp theo, cập nhật danh sách gói của bạn để APT sẽ đọc nguồn Elastic mới:

  `$ sudo apt-get update`

  Sau đó tiến hành cài đặt Elasticsearch: 

  `$ sudo apt-get install elasticsearch`

  Sau khi hoàn tất việc cài Elasticsearch, chỉnh sửa file cấu hình của Elasticsearch `/etc/elasticsearch/elasticsearch.yml`
  Elaticsearch lắng nghe lưu lượng truy cập trên cổng 9200. Bạn sẽ muốn hạn chế quyền truy cập bên ngoài vào đối tượng Elaticsearch của mình để ngăn người ngoài đọc dữ liệu của bạn hoặc tắt Elaticsearch cluster của bạn thông qua API REST. Tìm dòng chỉ định network.host, bỏ ghi chú và thay thế giá trị của nó bằng `localhost`.

    network.host: localhost

    network.public_host: địa chỉ IP

Phần port: 

http.port: *9200*

transport.tcp.port: *9300*

Phần discovery:

discovery.type: *single-node*

Lưu và thoát.
 Khởi động Elasticsearch. 
 
 `$ systemctl start elasticsearch`

Kiểm tra Elasticsearch đã chạy hay chưa bằng cách gửi 1 HTTP request.

`$ curl -X GET "localhost:9200"`
  

### 2. Cài đặt Kibana 
 bạn chỉ nên cài đặt Kibana sau khi cài đặt Elaticsearch. Cài đặt theo thứ tự này đảm bảo rằng các thành phần mỗi sản phẩm phụ thuộc vào đúng vị trí.
Vì bạn đã thêm nguồn gói Elasticsearch ở bước trước, bạn chỉ có thể cài đặt các thành phần còn lại của Elastic Stack bằng apt:
  
  `$ sudo apt-get install kibana`

Sau đó kích hoạt và khởi động Kibana

 `$ sudo systemctl enable kibana`

 `$ sudo systemctl start kibana`

 Vì Kibana được cấu hình để chỉ nghe trên localhost, chúng ta phải thiết lập `reserve proxy` để cho phép truy cập bên ngoài vào nó. Sử dụng Nginx.(ở đây em dùng Nginx version 1.17.1)
 Đầu tiên, sử dụng lệnh *openssl* để tạo người dùng Kibana quản trị mà bạn sẽ sử dụng để truy cập vào giao diện web Kibana. Ví dụ đặt tên tài khoản là **kibanaadmin**.
Lệnh sau sẽ tạo người dùng và mật khẩu Kibana quản trị và lưu trữ chúng trong tệp `htpasswd.kibana`. Bạn sẽ định cấu hình Nginx để yêu cầu tên người dùng và mật khẩu này và đọc tệp này trong giây lát: 

   `echo "kibanaadmin:` `openssl passwd abc123` `" | sudo tee -a /etc/nginx/htpasswd.kibana`

   Nhập mật khẩu cho tài khoản *kibanaadmin*. 
   Tiếp theo, tạo một tệp khối máy chủ Nginx. Ví dụ, đề cập đến tệp này là **example123.com**, mặc dù bạn có thể thấy hữu ích khi đặt cho bạn một tên mô tả hơn. Ví dụ: nếu có bản ghi FQDN và DNS được thiết lập cho máy chủ này, bạn có thể đặt tên tệp này theo tên FQDN của mình:

   `$ sudo vi /etc/nginx/conf.d/default`

   Đảm bảo cập nhật **example123.com** để khớp với FQDN hoặc địa chỉ IP công cộng của máy chủ của bạn. Mã này định cấu hình Nginx để hướng lưu lượng HTTP của máy chủ của bạn đến ứng dụng Kibana, đang lắng nghe trên `localhost: 5601`. Ngoài ra, nó cấu hình Nginx để đọc tệp `*htpasswd.kibana*` và yêu cầu xác thực cơ bản.

   server {
    listen 80;

    server_name example.com;

    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/htpasswd.users;

    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
      }
    }

Lưu và thoát.
Kiểm tra cấu hình cài đặt bằng lệnh: `$ nginx -t`
Không có lỗi thì hiển thị thông báo `syntax is ok`. Nếu chưa thì kiểm tra lại file cấu hình.
Khởi động lại Nginx: `$ sudo systemctl restart nginx`

Kibana hiện có thể truy cập thông qua FQDN hoặc địa chỉ IP của server Elastic Stack. Bạn có thể kiểm tra trang trạng thái của máy chủ Kibana bằng cách điều hướng đến địa chỉ sau và nhập thông tin đăng nhập của bạn: **http://địa_chỉ_IP/status**
### 3. Cài đặt Logstash
Cài đặt Logstash: `$ sudo apt-get install logstash`
Sau khi cài đặt Logstash, có thể chuyển sang cấu hình nó. Các tệp cấu hình của Logstash được viết theo định dạng JSON và nằm trong thư mục `/etc/logstash/conf.d`. Khi muốn cấu hình nó, sẽ dễ dàng khi hình dung Logstash là một đường dẫn lấy dữ liệu ở một đầu, xử lý nó theo cách này hay cách khác và gửi nó đến đích của nó (trong trường hợp này, đích đến là Elaticsearch). Một đường dẫn Logstash có hai yếu tố bắt buộc, đầu vào và đầu ra, và một yếu tố tùy chọn, bộ lọc. Các plugin đầu vào tiêu thụ dữ liệu từ một nguồn, các plugin bộ lọc xử lý dữ liệu và các plugin đầu ra ghi dữ liệu đến đích.

 Tạo 1 file cấu hình là `02-beats-input.conf` để cấu hình Filebeat input:
`$ sudo vi /etc/logstash/conf.d/02-beats-input.conf `
Chèn đoạn cấu hình `input` dưới đây vào file cấu hình, để xác định đầu vào sẽ nghe trên cổng TCP `5044 `

    input {
      beats {
       port => 5044
     }
    }

> **Input plugins**: được sử dụng để cấu hình một tập hợp các sự kiện sẽ được cung cấp cho Logstash.

Lưu cấu hình và tạo file cấu hình `10-syslog-filter.conf` để thêm bộ lọc (filter) vào system logs. 
`$ sudo vi /etc/logstash/conf.d/10-syslog-filter.conf`
Thêm vào file đoạn code sau: 

     filter {
    if [fileset][module] == "system" {
    if [fileset][name] == "auth" {
      grok {
        match => { "message" => ["%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: %{DATA:[system][auth][ssh][event]} %{DATA:[system][auth][ssh][method]} for (invalid user )?%{DATA:[system][auth][user]} from %{IPORHOST:[system][auth][ssh][ip]} port %{NUMBER:[system][auth][ssh][port]} ssh2(: %{GREEDYDATA:[system][auth][ssh][signature]})?",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: %{DATA:[system][auth][ssh][event]} user %{DATA:[system][auth][user]} from %{IPORHOST:[system][auth][ssh][ip]}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: Did not receive identification string from %{IPORHOST:[system][auth][ssh][dropped_ip]}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sudo(?:\[%{POSINT:[system][auth][pid]}\])?: \s*%{DATA:[system][auth][user]} :( %{DATA:[system][auth][sudo][error]} ;)? TTY=%{DATA:[system][auth][sudo][tty]} ; PWD=%{DATA:[system][auth][sudo][pwd]} ; USER=%{DATA:[system][auth][sudo][user]} ; COMMAND=%{GREEDYDATA:[system][auth][sudo][command]}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} groupadd(?:\[%{POSINT:[system][auth][pid]}\])?: new group: name=%{DATA:system.auth.groupadd.name}, GID=%{NUMBER:system.auth.groupadd.gid}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} useradd(?:\[%{POSINT:[system][auth][pid]}\])?: new user: name=%{DATA:[system][auth][user][add][name]}, UID=%{NUMBER:[system][auth][user][add][uid]}, GID=%{NUMBER:[system][auth][user][add][gid]}, home=%{DATA:[system][auth][user][add][home]}, shell=%{DATA:[system][auth][user][add][shell]}$",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} %{DATA:[system][auth][program]}(?:\[%{POSINT:[system][auth][pid]}\])?: %{GREEDYMULTILINE:[system][auth][message]}"] }
        pattern_definitions => {
          "GREEDYMULTILINE"=> "(.|\n)*"
        }
        remove_field => "message"
      }
      date {
        match => [ "[system][auth][timestamp]", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
      }
      geoip {
        source => "[system][auth][ssh][ip]"
        target => "[system][auth][ssh][geoip]"
      }
    }
    else if [fileset][name] == "syslog" {
      grok {
        match => { "message" => ["%{SYSLOGTIMESTAMP:[system][syslog][timestamp]} %{SYSLOGHOST:[system][syslog][hostname]} %{DATA:[system][syslog][program]}(?:\[%{POSINT:[system][syslog][pid]}\])?: %{GREEDYMULTILINE:[system][syslog][message]}"] }
        pattern_definitions => { "GREEDYMULTILINE" => "(.|\n)*" }
        remove_field => "message"
      }
      date {
        match => [ "[system][syslog][timestamp]", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
        }
      }
     }
    }       

> **Chú thích**: Các plugin trong đoạn cấu hình trên có ý nghĩa gì? Ở mục vai trò và chức của Logstash trong ELK em sẽ giải thích. 

Lưu cấu hình và tạo 1 file cấu hình `30-elasticsearch-output.conf`.
 `$ sudo vi /etc/logstash/conf.d/30-elasticsearch-output.conf`
 Chèn đoạn cấu hình output dưới đây vào file. Về cơ bản, đầu ra này cấu hình Logstash để lưu trữ dữ liệu Beats trong Elaticsearch, đang chạy tại `localhost: 9200`, trong một chỉ mục được đặt tên theo Beat được sử dụng.

     output {
      elasticsearch {
       hosts => ["localhost:9200"]
       manage_template => false
       index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
      }
     }
Lưu cấu hình và thoát. Nếu bạn muốn thêm bộ lọc cho các ứng dụng khác sử dụng đầu vào Filebeat, hãy nhớ đặt tên cho các tệp để chúng được sắp xếp giữa đầu vào và cấu hình đầu ra, nghĩa là tên tệp phải bắt đầu bằng số có hai chữ số từ 02 đến 30.

Kiểm tra cấu hình Logstash đã được hay chưa, chạy dòng lệnh sau: 
`sudo -u logstash /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t`
Nếu không có lỗi cú pháp gì thì sẽ có thông báo `Configuration OK`, nó sẽ hiện ra 1 khoảng thời gian kiểm tra.
Khởi động và enable Logstash: 
`$ sudo systemctl start logstash`
`$ sudo systemctl enable logstash`
### 4. Cài đặt Beat
ELK Stack sử dụng 1 vài phần mềm gửi dữ liệu gọi là Beats để thu thập dữ liệu từ nhiều nguồn khác nhau và gửi về Logstash hoặc Elasticsearch. Có các Beats sẵn có từ Elastic, nhưng ở bài viết này hướng dẫn cài đặt và sử dụng **Filebeat**.
Tiến hành cài đặt Filebeat: 
`$ sudo apt-get install filebeat`
Tiếp theo cấu hình Filebeat kết nối đến Logstash. Cấu hình file `/etc/filebeat/filebeat.yml` như sau: 
 `$ sudo vi /etc/filebeat/filebeat.yml`
 Filebeat hỗ trợ nhiều kết quả output, nhưng bạn sẽ chỉ gửi các sự kiện trực tiếp đến Elaticsearch hoặc Logstash để xử lý bổ sung. Trong hướng dẫn này sử dụng Logstash để thực hiện xử lý bổ sung trên dữ liệu do Filebeat thu thập. Filebeat sẽ không cần gửi bất kỳ dữ liệu nào trực tiếp đến Elaticsearch, vì vậy hãy vô hiệu hóa đầu ra đó. Để làm như vậy, hãy tìm phần `output.elaticsearch` và nhận xét các dòng sau bằng cách đặt trước chúng bằng dấu `#`:
    ......
     `#`output.elasticsearch:
      # Array of hosts to connect to.
      `#`hosts: ["localhost:9200"]
.......
Sau đó, cấu hình phần `output.logstash`. Bỏ ghi chú các dòng output.logstash: và `hosts: ["localhost: 5044"]` bằng cách xóa `#`. Điều này sẽ cấu hình Filebeat để kết nối với Logstash trên máy chủ Elastic Stack tại cổng `5044`, cổng đã chỉ định đầu vào Logstash trước đó:
     ......
    output.logstash:
      #The Logstash hosts
      hosts: ["localhost:5044"]
......
 Lưu cấu hình và thoát. Chức năng của Filebeat có thể được mở rộng với các mô-đun Filebeat. Trong hướng dẫn này, chúng tôi sẽ sử dụng mô-đun hệ thống, thu thập và phân tích các bản ghi được tạo bởi dịch vụ ghi log hệ thống của các bản phân phối Linux phổ biến.
  `$ sudo filebeat modules enable system`

Tiếp theo, tải mẫu chỉ mục vào Elaticsearch. Một chỉ mục Elaticsearch là một tập hợp các tài liệu có các đặc điểm tương tự. Các chỉ mục được xác định bằng một tên, được sử dụng để chỉ các chỉ mục khi thực hiện các hoạt động khác nhau trong đó. Mẫu chỉ mục sẽ được tự động áp dụng khi tạo chỉ mục mới.
Để tải chỉ mục, sử dụng lệnh sau:
  `$ sudo filebeat setup --template -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["localhost:9200"]'`

      Output 
      Loaded index template
Nếu không xảy ra lỗi gì thì sẽ hiện ra kết quả như thế này.
Filebeat được đóng gói với bảng điều khiển Kibana mẫu cho phép bạn hình dung dữ liệu Filebeat trong Kibana. Trước khi bạn có thể sử dụng bảng điều khiển, bạn cần tạo mẫu chỉ mục và tải bảng điều khiển vào Kibana.
Khi bảng điều khiển tải, Filebeat kết nối với Elaticsearch để kiểm tra thông tin phiên bản. Để tải bảng điều khiển khi Logstash được bật, bạn cần tắt đầu ra Logstash và bật đầu ra Elaticsearch:
`$ sudo filebeat setup -e -E output.logstash.enabled=false -E output.elasticsearch.hosts=['localhost:9200'] -E setup.kibana.host=localhost:5601`

Bạn sẽ thấy kết quả sau: 

     2019-07-26T19:50:57.047Z	INFO	instance/beat.go:280	Setup Beat: filebeat; Version: 6.8.1
     2019-07-26T19:50:57.048Z	INFO	elasticsearch/client.go:164	Elasticsearch url: http://localhost:9200
     2019-07-26T19:50:57.181Z	INFO	[publisher]	pipeline/module.go:110	Beat name: ha
     2019-07-26T19:50:57.288Z	INFO	elasticsearch/client.go:164	Elasticsearch url: http://localhost:9200
     2019-07-26T19:50:57.298Z	INFO	elasticsearch/client.go:739	Attempting to connect to Elasticsearch version 6.8.1
     2019-07-26T19:50:57.349Z	INFO	template/load.go:128	Template already exists and will not be overwritten.
     2019-07-26T19:50:57.349Z	INFO	instance/beat.go:889	Template successfully loaded.
     Loaded index template
     Loading dashboards (Kibana must be running and reachable)
     2019-07-26T19:50:57.349Z	INFO	elasticsearch/client.go:164	Elasticsearch url: http://localhost:9200
     2019-07-26T19:50:57.352Z	INFO	elasticsearch/client.go:739	Attempting to connect to Elasticsearch version 6.8.1
     2019-07-26T19:50:57.407Z	INFO	kibana/client.go:118	Kibana url: http://localhost:5601
     2019-07-26T19:51:00.072Z	INFO	add_cloud_metadata/add_cloud_metadata.go:340	add_cloud_metadata: hosting provider type not detected.
     2019-07-26T19:51:34.070Z	INFO	instance/beat.go:736	Kibana dashboards successfully loaded.
     Loaded dashboards
     2019-07-26T19:51:34.070Z	INFO	elasticsearch/client.go:164	Elasticsearch url: http://localhost:9200
     2019-07-26T19:51:34.076Z	INFO	elasticsearch/client.go:739	Attempting to connect to Elasticsearch version 6.8.1
     2019-07-26T19:51:34.138Z	INFO	kibana/client.go:118	Kibana url: http://localhost:5601
     2019-07-26T19:51:34.268Z	WARN	fileset/modules.go:388	X-Pack Machine Learning is not enabled
     2019-07-26T19:51:34.385Z	WARN	fileset/modules.go:388	X-Pack Machine Learning is not enabled
     2019-07-26T19:51:34.500Z	WARN	fileset/modules.go:388	X-Pack Machine Learning is not enabled
     2019-07-26T19:51:34.533Z	WARN	fileset/modules.go:388	X-Pack Machine Learning is not enabled
     2019-07-26T19:51:34.565Z	WARN	fileset/modules.go:388	X-Pack Machine Learning is not enabled
     Loaded machine learning job configurations
Khởi động và enable Filebeat. 
  `$ sudo systemctl start filebeat`
  `$ sudo systemctl enable filebeat`

> Nếu bạn đã thiết lập Elastic Stack chính xác, Filebeat sẽ bắt đầu chuyển nhật ký log hệ thống và syslog của bạn tới Logstash, sau đó sẽ tải dữ liệu đó vào Elaticsearch.

Để xác minh rằng Elaticsearch thực sự đang nhận dữ liệu này, hãy truy vấn chỉ mục Filebeat bằng lệnh này:

`$ curl -XGET 'http://localhost:9200/filebeat-*/_search?pretty'`

Sau khi chạy lệnh trên, sẽ hiện ra kết quả tương tự như sau: 
            ..... 
    "@version" : "1",
     "host" : {
     "containerized" : false,
     "architecture" : "x86_64",
"name" : "ha",
"os" : {
"platform" : "ubuntu",
"version" : "18.04.2 LTS (Bionic Beaver)",
"family" : "debian",
"name" : "Ubuntu",
"codename" : "bionic"
},
"id" : "1c6297324fa0437796d5490ffe5db8f8"
},
"@timestamp" : "2019-07-18T12:32:29.000Z",
"tags" : [
"beats_input_codec_plain_applied"
   ],
  "source" : "/var/log/syslog",
  "log" : {
  "file" : {
   "path" : "/var/log/syslog"
         }
       }
      }
     }
    ]
   }
  }
## III. Vai trò của Elasticsearch trong ELK
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
### 8. Monitoring 
## V. Logstash trong ELK

