# k8s_overview
## Kiến trúc của Kubernetes
* Gồm hai phần: Master và Worker
   - Master:  là thành phần điều khiển các Worker, triển khai các ứng dụng trên worker.
   - Worker: là nơi mà các ứng dụng được triển khai.
 * Master và Worker có thể chạy trên máy vật lý hoặc là máy ảo(VM).
 ## Những khái niệm cơ bản của Kubernetes
 * Master Node
   - là máy chủ điều khiển các worker node. Master node bao gồm 4 thành phần chính:
     - kubernetes API server: là thành phần giúp các thành phần khác liên lạc nói chuyện với nhau. Lập trình viên khi triển khai ứng dụng sẽ gọi API Kubernetes API Server này.
     - Scheduler: Gán cho ứng dụng vào worker node nào để chạy
     - Controller Manager: Thành phần đảm nhiệm phần quản lý các Worker (kiểm tra worker node sống hay chết, đảm nhận việc auto scalling)
     - Etcd: Đây là cơ sở dữ liệu của Kubernetes, tất cả các thông tin của Kubernetes được lưu trữ cố định tại đây (database)
* Worker Node
  - là máy chủ chạy các ứng dụng. bao gồm 3 thành phần chính:
     - Container runtime: Là thành phần giúp chạy các ứng dụng dưới dạng Container ( thông thường thì sử dụng docker)
     - Kubelet: Đây là thành phần giao tiếp với kubernetes API Server, và cũng quản lý container.( chính là tiến trình để nói chuyện với master: cung cấp thông tin hay nhận chỉ thị mới từ kube master)
     - Kubenetes Service Proxy: khi có connnection hướng ra ngoài hay hướng vào trong thì thành phần này đảm nhận việc điều hướng traffic đi vào)
![Capture](https://user-images.githubusercontent.com/63154819/94369360-7c848c80-0113-11eb-8503-881653977225.PNG)
* kubectl
  - Công cụ quản trị kubernetes, 
* Pod
  - Pod là khái niệm cơ bản và quan trọng nhất trong k8s. Bản thân Pod có thể chứa 1 hoặc nhiều container( thông thường là 1 container). Pod chính là nơi chứa ứng dụng chạy trong nó thông qua container. Pod là các tiến trình nằm trên các Worker Node. Bản thân Pod cũng có tài nguyên riêng về file system, cpu, ram, volumes, địa chỉ network....
* Image
  - Là một phần mềm để chạy ứng dụng tuy nhiên được gói lại thành  một chương trình nào đó để có thể triển khai dưới dạng container. Các image thông thường được quản lý ở một nơi lưu trữ tập trung, ví dụ như Docker Hub là nơi chứa nhiều các image phổ biến như: nginx, mysql, wordpress...
* Deployment
  - Là cách thức để triển khai, cập nhập và quản trị Pod
 
