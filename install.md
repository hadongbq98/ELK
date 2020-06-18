# Cài đặt ELK Stack 
 Cài đặt OpenJDK 8 để cài đặt ELK
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

   `$ sudo vi /etc/nginx/conf.d/default.conf`

   Đảm bảo cập nhật **example123.com** để khớp với FQDN hoặc địa chỉ IP công cộng của máy chủ của bạn. Mã này định cấu hình Nginx để hướng lưu lượng HTTP của máy chủ của bạn đến ứng dụng Kibana, đang lắng nghe trên `localhost: 5601`. Ngoài ra, nó cấu hình Nginx để đọc tệp `*htpasswd.kibana*` và yêu cầu xác thực cơ bản.

   server {
    listen 80;

    server_name example.com;

    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/htpasswd.kibana;

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
