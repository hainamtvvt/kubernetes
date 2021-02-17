# Service-Secret-TLs
(*Các POD có IP, hostname riêng chứa các container của ứng dụng. Client có thể kết nối đến các POD thông qua địa chỉ IP hay hostname tương ứng. Tuy nhiên có một vấn đề là: các POD có thể bị lỗi hay crash bất ngờ và khi ReplicaSet tạo lại POD mới thì các thông số như địa chỉ IP, hostname cũng bị thay đổi. Hơn nữa, một ứng dụng triển khai trên K8s có nhiều Pods chạy cùng một lúc, client không nên và cũng không cần thiết lưu trữ 1 tá các địa chỉ IP, hostname của các Pods => cần đến Service)*.
![f](https://user-images.githubusercontent.com/63154819/94510211-69bba600-0240-11eb-86e7-879400646348.PNG)
* Mỗi Service có địa chỉ IP và hostname không đổi. Client có thể kết nối đến IP và Port của Service sau đó được điều hướng đến các POD để xử lý. **Như vậy client muốn kết nối đến các POD thì phải thông qua Service)**.
![f](https://user-images.githubusercontent.com/63154819/94512079-5eb74480-0245-11eb-9426-8f8b2ca9382e.PNG)
* Cũng có thể hiểu Service là một dịch vụ tạo cơ chế cân bằng tải (Load Balancing) truy cập đến các Pod.
![Capture](https://user-images.githubusercontent.com/63154819/94510642-77256000-0241-11eb-8897-e0001a8fd92c.PNG)
## Tạo Service Kubernetes
* Tạo Service kiểu ClusterIP, không Selector.
![Capture](https://user-images.githubusercontent.com/63154819/94510894-1b0f0b80-0242-11eb-89fa-54f7f135d60c.PNG)
  - File trên khai báo một service đặt tên là svc1, loại service là ClusterIP (đây là kiểu mặc định), ngoài ra còn có kiểu: NodePort, LoadBalancer, ExternalName), phần khai báo ports: port:80 (cổng của service) tương ứng ánh xạ vào cổng của Pod ( targetPort:80).
 * Triển khai file trên: kubectl apply -f 1.svc1.yaml
 * lấy thông tin các service: kubectl get svc -o wide
 * xem thông tin của service svc1: kubectl describle svc/svc1.
## Các loại Service trong Kubernetes
 * ClusterIP: Service chỉ có địa chỉ IP cục bộ (Local) và chỉ có thể truy cập được từ các thành phần trong cụm cluster
 * NodePort: Service có thể tương tác qua Port của các worker node trong cluster
 * LoadBalancer: Service có địa chỉ IP public, có thể tương tác ở bất kì đâu
 * ExternalName: Ánh xạ service với DNS Name.
## Service NodePort
* Dùng lệnh: kubectl get svc để kiểm tra thông tin service thì thấy: CLUSTER_IP VÀ EXTERNAL_IP có giá trị: <node>.
  - CLUSTE_IP: Là địa chỉ IP cục bộ trong cụm cluster, với địa chỉ IP này thì các Pods hay service có thể tương tác với nhau như bên ngoài không thể tương tác với Service thông qua nó được
  - EXTERNAL_IP: IP public, có thể dùng để client tương tác với Service
* NodePort giúp Service có thể tương tác được từ bên ngoài thông qua port của worker node.
* Khởi tạo file config nodeport.yaml
![d](https://user-images.githubusercontent.com/63154819/94514386-37637600-024b-11eb-8d37-732e77455763.PNG)
  - nodePort: số hiệu cổng của Node worker được mở để bên ngoài tương tác với service
* Khi tương tác với service, client sẽ truy cập qua địa chỉ <IP public của Node>:Port
![s](https://user-images.githubusercontent.com/63154819/94514507-94f7c280-024b-11eb-8185-0f981a9e575c.PNG)
## Service Load Balancer
* Tạo một Service kiểu LoadBalancer sẽ cung cập thêm địa chỉ IP Public bên ngoài có thể gửi request đến.
![Capture](https://user-images.githubusercontent.com/63154819/94514903-8cec5280-024c-11eb-827d-e16d981b52b7.PNG)
