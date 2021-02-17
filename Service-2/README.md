# Service-2-
***Kubernetes service có 4 kiểu chính***

![Capture](https://user-images.githubusercontent.com/63154819/94760706-79b4c080-03cd-11eb-9bf3-42016d835a8b.PNG)

(Nếu tạo một dịch vụ NodePort thì mặc định cũng tạo một dịch vụ ClusterIP. Nếu tạo LoadBalancer, thì cũng tạo NodePort và sau đó tạo ClusterIP).
## No Service
![Capture](https://user-images.githubusercontent.com/63154819/94760918-fb0c5300-03cd-11eb-9c15-5a31b5dca7d7.PNG)
* Chúng ta có 2 node và 1 pod. Node có địa chỉ ExternalIP: 4.4.4.1 và 4.4.4.1 và InternalIP là: 1.1.1.1 và 1.1.1.2. Pod pod-python chỉ có 1 phần Internal.
![Capture](https://user-images.githubusercontent.com/63154819/94761156-8b4a9800-03ce-11eb-8f53-6937b7f93f0e.PNG)
* Bây giờ thêm một POD tên là: pod-nginx trên node-1. Trong Kubernetes, tất cả các Pod đều có thể trỏ tới bất kì Pod nào trong địa chỉ IP bên trong (InternalIP). Có nghĩa là pod-nginx ping và kết nối với pod-python bằng cách sử dụng InternalIP:1.1.1.3
* Nếu pod-python ở Node-2 bị xóa và sử dụng ReplicaSet thì được tạo ra 1 Pod mới. Pod mới này có địa chỉ InternalIP khác 1.1.1.3 nên pod-nginx không thể kết nối với pod-python thông qua địa chỉ IP 1.1.1.3. Vì vậy chúng ta cần đến SERVICE.
![Capture](https://user-images.githubusercontent.com/63154819/94761489-60147880-03cf-11eb-9e70-679b58c4b8f6.PNG)
## ClusterIP trong Kubernetes
![Capture](https://user-images.githubusercontent.com/63154819/94761581-a2d65080-03cf-11eb-8c57-36b4b26125df.PNG)
* Sử dụng service ClusterIP.
  - pod-nginx luôn có kết nối an toàn với 1.1.10.1 sau đó service-python được chuyển hướng đến pod-python.
* Ví dụ: Với 3 trường hợp của Python và hiển thị các port thuộc địa chỉ IP bên trong (InternalIP) của tất cả các pod và service
![Capture](https://user-images.githubusercontent.com/63154819/94761871-62c39d80-03d0-11eb-9065-6ee33fb43e52.PNG)
* Tất cả các Pod bên trong cluster có thể connect với các pod trên port 443 thông qua http://1.1.10.1:300 hoặc http://service-python:3000. Đó là những gì ClusterIP thực hiện, nó cung cấp Pod có sẵn bên trong Cluster thông qua tên và IP.
![Capture](https://user-images.githubusercontent.com/63154819/94762159-25abdb00-03d1-11eb-84c1-9a156489b81d.PNG)
## NodePort
* Muốn cung cấp service ClusterIP ra bên ngoài, vì vậy cần chuyển nó thành service NodePort.
![1](https://user-images.githubusercontent.com/63154819/94762724-acad8300-03d2-11eb-9398-42fa8e23f4a3.PNG)
![2](https://user-images.githubusercontent.com/63154819/94762762-c222ad00-03d2-11eb-8c8a-f1d57443b6f1.PNG)
* Điều này có nghĩa là service-python bên trong của chúng ta có thể truy cập được từ mọi địa chỉ IP bên trong (port:443) và bên ngoài (port:30080).

![Capture](https://user-images.githubusercontent.com/63154819/94762957-42491280-03d3-11eb-9348-52f75e10af3b.PNG)
* Mỗi pod bên trong cụm cluster cũng có thể kết nối với NodeIP bên trong trên port 30080.
* Trong nội bộ service NodePort vẫn hoạt động như service ClusterIP. Nó giúp tưởng tượng rằng một service NodePort tạo ra một ClusterIP, mặc dù không còn một đối tượng ClusterIP nào nữa.
## LoadBalancer (Cân bằng tải)
* Chúng ta sử dụng service LoaBalancer nếu chúng ta muốn có một IP duy nhất phân phối các yêu cầu (sử dụng một số phương thức như vòng tròn) cho tất cả các NodeIP bên ngoài. Vì vậy, nó được xây dựng dựa trên service NodePort:

![Capture](https://user-images.githubusercontent.com/63154819/94768343-63acfd00-03d9-11eb-89f3-843cf0194aab.PNG)
* Hãy tưởng tượng rằng service LoadBalancer tạo ra service NodePort rồi tạo ra service  dịch vụ ClusterIP. yaml đã thay đổi cho LoadBalancer, trái ngược với NodePort trước đây chỉ đơn giản là: 


![1](https://user-images.githubusercontent.com/63154819/94769359-0a929880-03dc-11eb-99ad-418b72b6ca20.PNG)
