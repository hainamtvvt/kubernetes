# NGINX-Ingress Controller trong Kubernetes

![Capture](https://user-images.githubusercontent.com/63154819/95647945-cbf88e80-0afd-11eb-8fff-575803f70912.PNG)

**Nginx Kubernetes Ingress Controller là một ingress hỗ trợ khả năng cân bằng tải,SSL,URI rewrite...**
* Igress Controller được cung cấp bởi Nginx là một proxy nổi tiếng, mã nguồn của Nginx Ingress Controller có trên github.
* Nginx là một web server mã nguồn mở, sử dụng kiến trúc đơn luồng. Nó có thể làm load balancing, HTTP caching, hay sử dụng như một reverse proxy. 
* 

![1](https://user-images.githubusercontent.com/63154819/95648075-cc455980-0afe-11eb-8fc2-1dc2e04aac30.PNG)

## Tính năng của Nginx
* Khả năng xử lý hơn 10.000 kết nối cùng một lúc với bộ nhớ thấp.
* Tăng tốc reverse proxy bằng bộ nhớ đệm (cache), cân bằng tải đơn giản và khả năng chịu lỗi.
...
## Cấu hình Nginx.
*Những thiết lập quan trọng ở trong tập tin nginx.conf, mặc định sẽ là như thế này.*

![Capture](https://user-images.githubusercontent.com/63154819/95648197-92288780-0aff-11eb-9fd4-f2f763ba2b15.PNG)


  - worker_processes: Định nghĩa số worker processes mà NGINX sẽ sử dụng. Bởi vì NGINX là đơn luồng nên nó thường bằng với số lõi CPU (processes của CPU).
  - worker_connection: Số lượng tối đa của các kết nối đông thời cho mỗi worker processes và nói cho worker processes của chúng ta có bao nhiêu người có thể được phục vụ đồng thời bởi NGINX.
  - access-log & error_log: Đây là những tệp tin mà NGINX sẽ sử dụng để log bất kì lỗi và số lần truy cập. Các bản ghi này thường được sử dụng để gỡ lỗi hoặc sửa chữa.
  - gzip: Là các thiết lập nén GZIP của các NGINX reponse. Tính năng này có nhiều thiết lập phụ, phần bị comment bởi mặc định có thể giúp hiệu suất được cải thiện đáng kể. 
  
