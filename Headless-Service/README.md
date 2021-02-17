# Headless Service trong Kubernetes
***Thông thường khi ta muốn truy cập các ứng dụng trong Kubernetes, chúng ta sẽ khởi tạo một Service để Load Balancer lưu lượng xuống các Pod ứng dụng. Tuy nhiên sử dụng Headless Service để truy cập trực tiếp các Pod IP thông qua DNS nội bộ.***

## Headless Service là gì trong Kubernetes?
* Đôi khi bạn sẽ không cần tính năng Service ClusterIP Load Balancing hoặc một địa chỉ IP Service cụ thể. Thay vào đó bạn có mong muốn được truy cập trực tiếp đến các dịch vụ của Pod thay vì thông qua một lớp Proxy thường thấy với Service abstraction.
* Chỉ cần cấu hình phần ".spec.clusterip" là "none" là bạn đã quy định Service của bạn là loại Headless. Khi đó ClusterIP sẽ không được cấp phát cho Service mà bạn khai báo, kube-proxy sẽ không xử lý đối tượng Service của bạn. Lúc này DNS của Service sẽ trả về thông tin là các địa chỉ IP của Pod khớp với Selector.
* Một Headless Service (Service không IP) nó liên kết thẳng với IP của POD, có nghĩa bạn sẽ không tương tác trực tiếp với POD qua proxy. Tạo loại Service này giống như Service khác, chỉ việc thêm .spec.clusterIP có giá trị None.
## Cấu hình Headless Service trong Kubernetes
* Tạo một Deployment Nginx với 4 Pod.
* Tạo một ClusterIP Service để có quyền truy cập cân bằng tải 4 Pod Nginx thông qua DNS ClusterIP Service với 1 IP cụ thể.

=> Ta tạo ta một Deployment Nginx với ReplicaSet là 4 tương đương 4 Pod.

![Capture](https://user-images.githubusercontent.com/63154819/97774011-0a2a2080-1b87-11eb-92d5-55237aad952b.PNG)


![Capture](https://user-images.githubusercontent.com/63154819/97774036-38a7fb80-1b87-11eb-8f24-476fe9524de5.PNG)

* Tạo một Service với loại mặc định là ClusterIP, khai báo listen ở port 80(Nginx) TCP.

![Capture](https://user-images.githubusercontent.com/63154819/97774073-76a51f80-1b87-11eb-98b3-d67b02f67f85.PNG)

* Cuối cùng ta chuẩn bị xong 2 loại Service Kubernetes gồm ClusterIP và Headless. Tạo một pod dnsutils và mở terminal shell trên pod đó để kiểm tra tính năng nào.

![Capture](https://user-images.githubusercontent.com/63154819/97774120-eb785980-1b87-11eb-9c73-0272b830c736.PNG)

* Phân giải tên miền nội bộ Service ClusterIP. Bạn sẽ thấy DNS phân giải về đúng địa chỉ clusterIP sẽ được dùng để load balance xuống các PodIP 

![Capture](https://user-images.githubusercontent.com/63154819/97774160-31352200-1b88-11eb-8334-a257520f320a.PNG)
