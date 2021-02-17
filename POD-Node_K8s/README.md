# Các lệnh khi làm việc với POD
* kubectl get nodes: Liệt kê danh sách các Node trong Cluster
* kubectl describle node name-node: Thông tin của Node có tên là name-node
* kubectl get pods: liệt kê các pod trong namespace hiện tại, thêm -o wide để hiện thông tin chi tiết hơn
* kubectl explain pod --recursive=true: xem cấu trúc định nghĩa POD trong file cấu hình yaml
#  Node trong k8s
* Node là đơn vị nhỏ nhất xét về phần cứng. Node có thể là một máy vật lý hay một máy ảo VM tromg cụm Cluster. Để xem thông tin về các node trong cụm cluster: kubectl get nodes.
* xem thông tin chi tiết của một node: kubectl describle node tên-node
* thông tin của một node bao gồm: Hostname, InternalIP ( địa chỉ IP của node sử dụng trong nội bộ cluster), ExternalIP: địa chỉ IP của Node  có hiệu lực từ bên ngoài cluster gửi đến
* ready: giá trị true: chấp nhận triển khai chạy các pod
* MemoryPressure: giá trị là true: thì đang bị cạn kiệt bộ nhớ
* DiskPressure: giá trị là true: nếu đang cạn kiệt ổ đĩa lưu trữ
* NetworkUnavailable: giá trị là true: nếu cấu hình mạng đang bị lỗi
![alt](https://user-images.githubusercontent.com/63154819/93842773-29997800-fcc2-11ea-9303-794436e192b4.PNG)
# Pods trong kubernetes
* Khái niệm của Pod: trong k8s không chạy các container một cách trực tiếp mà thay vào đó nó **bọc** một hoặc một vài container vào với nhau trong 1 Pod ( trong một Pod có thể có một hoặc một vài container). Các container trong cùng một Pod thì chia sẻ tài nguyên và mạng cục bộ của pod.
![1](https://user-images.githubusercontent.com/63154819/93843284-9bbe8c80-fcc3-11ea-9bf9-b512790452c2.PNG)
* Pod là đơn vị **nhỏ nhất** để k8s thực hiện việc nhân bản. có nghĩa là trong một số trường hợp cần thiết thì k8s có thể cấu hình triển khai nhân bản ra nhiều Pod có chức năng giống nhau để tránh quá tải. ***cách sử dụng hiệu quả và thông dụng là trong một Pod chỉ chạy một container*** 
* Mỗi Pod được gán một địa chỉ IP, các container trong Pod chia sẻ cùng địa chỉ IP này và các container này có thể liên lạc được với nhau qua địa chỉ localhost( địa chỉ localhost: là địa chỉ của Pod)
## Làm việc với Pod trong Kubernestes
* Tạo ra Pod qua triển khai deployment.yaml để Controller thực hiện( ***khi tạo Pod thông qua Deployment thì có một Controller thực hiện việc quản lý các Pod, thực hiện theo cách này thì nó cung cấp khả năng giám sát, phục hồi, nhân bản...***)
### các câu lệnh làm việc với Pod
* kubectl get pods: liệt kê các pod ở namespaces mặc định
* kubectl get pod -o wide: Hiển thị nhiều thông tin của Pod hơn
* kubectl get pod -o wide -n k8s-dashboard: liệt kê thông tin các pod ở namespace: k8s-dashboard
# Tạo Pod từ file cấu hình .yaml
***nên khai báo Pod trong file config.yaml rồi ra lệnh đọc file này tạo pod với lệnh kubectl apply***
* tạo file: 1-swarmtest-node.yaml
![2](https://user-images.githubusercontent.com/63154819/93844888-e989c380-fcc8-11ea-9052-4d8d58de17a9.PNG)
* file trên khai báo 1 Pod: tên Pod là: ungdungnode, gán nhãn app: app1, ungdung: ungdung1, trong Pod có  một container từ image ichte/swarmtest:php, cổng của container: 8085 . Trong trường metadata: thì khai báo cho Pod ví dụ: tên pod, các nhãn, còn trường spec: thì dùng cho việc khai báo thông số của các container.
* triển khai tạo Pod từ file này, thực hiện câu lệnh sau: kubectl apply -f 1-swarmtest-node.yaml
***mặc định thì k8s không tạo và chạy pod ở node master để đảm bảo yêu cầu an toàn.***
* ví dụ: nginx.yaml
![119785242_328027395136790_5445463395655301916_n](https://user-images.githubusercontent.com/63154819/93847083-24dbc080-fcd0-11ea-88bd-51137d1c7d7d.png)
 
 ( Pod có tên là nginx, trong Pod có một container chạy từ image: nginx:1.17.6, đặt cho container có tên là n1, cổng của container là: 80)
## Pod có nhiều container

![Capture](https://user-images.githubu
sercontent.com/63154819/94039161-89db0780-fdf1-11ea-8b03-32b5be911666.PNG)

## Các trường hợp sử dụng Pod có nhiều container
=> Mục đích chính của Pod có nhiều container là hỗ trợ quản lý nhiều process cùng trợ giúp cho một ứng dụng chính. Một số trường hợp sử dụng process trợ giúp trong Pod.
  + **Sidecar containers** : giúp cho container chính. Ví dụ như theo dõi sự thay đổi của log hoặc data, giám sát... 
  + **Proxies, bridges, adapters** kết hợp container chính với thế giới bên ngoài( external world), Ví dụ một máy chủ Apache hoặc nginx phục vụ các static file. Nó có thể hoạt động như một reverse proxy với một web application trong container chính để ghi log với giới hạn HTTP request.
 => ***Khuyến nghị là nên một Pod chạy một container.***
## Truy cập Pod từ bên ngoài Cluster(Kiểm tra-Debug)
* Trong thông tin của Pod ta thấy có IP của Pod và port lắng nghe, tuy nhiên IP này là nội bộ, chỉ các Pod trong Cluster liên lạc với nhau. Nếu bên ngoài muốn truy cập cần tạo một Service để chuyển traffic bên ngoài vào Pod, tại đây để debug-truy cập kiểm tra bằng cách chạy proxy: kubectl proxy hoặc kubectl proxy --address="0.0.0.0" --accept-host='^*$'
* Khi kiểm tra chạy thử, cũng có thể chyển cổng truy cập. Ví dụ cổng host 8080 được chuyển huwogns truy cập đến cổng  8085 của Pod mypod: kubectl port-forward mypod 8080:8085
## Ổ đĩa / Volume trong Pod
* Để thêm các Volume: đầu tiên cần định nghĩa ổ đĩa ở trường spec.volumes tại mỗi container cần gắn vào ổ đĩa với thuộc tính volumeMounts
![s](https://user-images.githubusercontent.com/63154819/94230968-4685b380-ff2d-11ea-9247-369b8b235e25.PNG)
* Nếu muốn sử dụng ổ đĩa - giống nhau về dữ liệu trên nhiều Pod, kể cả các Pod đó chạy trên các máy khác nhau thì cần dùng các loại đĩa Remote( ***Phương pháp này hay được sử dụng)***
## Chia sẻ volume trong một K8s Pod
* Trong k8s, bạn có thể sử dụng một shared k8s volume một cách đơn giản và hiệu quả việc chia sẻ dữ liệu giữa các container trong một Pod (đối với Pod có nhiều container). Đối với hầu hết các trường hợp, nó đơn giản là ***sử dụng một thư mục trên máy chủ (host) để chia sẻ cho tất cả container trong một Pod.***
* K8s volume cho phép dữ liệu tồn tại khi khởi động lại một container, những volume này có cùng thời gian sống với Pod. Điều đó có nghĩa là dữ liệu sẽ tồn tại cùng với Pod. Nếu Pod đó bị xóa vì bất kì lý do gì thì volume cũng bị hủy và được tạo lại.
* Một trường hợp cho việc sử dụng Pod có nhiều container với một shared volume là khi một container chứa log hoặc các file khác cho thư mục chung, và các container khác đọc từ thư mục chia sẻ (share directory). ví dụ, có thể tạo một Pod như sau:

![Capture](https://user-images.githubusercontent.com/63154819/97255835-0e330700-1844-11eb-8ef0-64a2d245a963.PNG)


=> Trong ví dụ này, ta xác định một file có tên là html. Nó có kiểu là emptyDir (có nghĩa là volume được tạo lần đầu khi một Pod được gán cho một node và tồn tại tới khi nào Pod vẫn đang chạy trên node đó).  Container đầu tiên chạy nginx server và chia sẻ volume được gán vào thư mục /usr/share/nginx/html. Container thứ hai chạy debian và chia sẻ volume được gán vào thư mục /html. Mỗi giây container thứ 2 sẽ tự động thêm ngày giờ hiện tại vào file index.html. Khi người dùng thực hiện yêu cầu HTTP tới Pod, Nginx server đọc tệp này và chuyển nó trở lại cho người dùng trong một phản hồi cho request.
