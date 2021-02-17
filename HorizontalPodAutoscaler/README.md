# HPA-HorizontalPodAutoscaler
## PeplicaSet trong Kubernetes
* ReplicaSet là một điều khiển Controller, nó đảm bảo ổn định về số lượng và tình trạng của Pod khi đang chạy.
![Capture](https://user-images.githubusercontent.com/63154819/94159781-2531b280-feae-11ea-9ee7-d10891dc3acf.PNG)
## Cách thức hoạt động
* Định nghĩa một ReplicaSet ( viết file yaml) gồm có các trường thông tin trong đó trường selector để chọn ra các Pod theo label, từ đó biết các Pod cần quản lý theo label đó( quản lý số lượng Pod, tình tPrạng Pod). Spec template để định nghĩa về dữ liệu Pod, để khi cần tạo Pod mới thì Pod mới được tạo từ template đó. Khi ReplicaSet tạo, chạy, cập nhập nó sẽ thực hiện tạo, xóa Pod với số lượng cần thiết được khai báo trong file yaml.
## Ví dụ file .yaml ReplicaSet trong Kubetnetes
* file cấu hình sau định nghĩa một ReplicaSet có tên là rsapp, quản lý nhân bản 3 Pod có label app=rsapp, và Pod có 1 container được tạo từ image ichte/swarmtest:node
![f](https://user-images.githubusercontent.com/63154819/94161099-8443f700-feaf-11ea-9b8e-004d6dd9fadf.PNG)
* Lệnh để thực hiện triển khai/cập nhập: kubectl apply -f 2.rs.ymal
* Để lấy các ReplicaSet thực hiện lệnh: kubectl get rs
* Xem thông tin về ReplicaSet có tên rsapp: kubectl describe rs/rsapp
* Nếu xóa Pod thì ReplicaSet sẽ thay thế nó bởi một Pod mới. Nếu xóa ReplicaSet thì các Pod cũng bị xóa theo.
## Horizontal Pod Autoscaler với ReplicaSet
* Horizontal Pod Autoscaler là chế độ tự động slcale dựa vào mức độ hoạt động của CPU đối với Pod, nếu một Pod quá tải- nó có thể tạo ra thêm Pod khác và ngược lại, số Pod tạo ra dao động trong khoảng min, max được cấu hình trong file .yaml
* Ví dụ với ReplicaSet rsapp trên đang thực hiện nhân bản 3 Pod, nếu muốn tạo ra một HPA(Horizontal Pod Autoscaler) theo mức độ đang làm việc CPU, có thể dùng lệnh sau: kubectl autoscaler rs rsapp --max=2 --min=1 ( Lệnh trên tạo ra một HPA có tên là rsapp, tham chiếu đến ReplicaSet có tên rsapp để scale các Pod với thiết lập min, max các Pod)
* Liệt kê các HPA(Horizonal Pod Autoscaler): kubectl get hpa
***tuy nhiên nên tạo HPA(Horizonal Pod AutoScaler) từ file.yaml). ví dụ:***
![1](https://user-images.githubusercontent.com/63154819/94223199-80e55580-ff19-11ea-91e3-70d4ddec7951.PNG)
* Thực hiện lệnh triển khai: kubectl apply -f 2.hpa.yaml
(mặc dù có thể sử dụng ReplicaSet một cachs độc lập, tuy nhiên trong hiện nay hay dùng Deployment, với Deployment nó sử hữu một ReplicaSet riêng.tức là trong file deployment.yaml có một trường ReplicaSet riêng).
## Rollback Deployment
* Kiểm tra các lần cập nhập (revision): *kubectl rollout history deploy/mydeploy*
* Để xem thông tin bản cập nhập 1 thì gõ lệnh: *kubectl rollout history deploy/deployapp --revision=1*
* Khi cần quay lại phiên bản cũ nào đó, ví dụ bản revision 1: *kubectl rollout undo deploy/deployapp --to-revision=1*
* Nếu muốn quay lại bản cập nhập trước gần nhất: *kubectl rollout undo deploy/mydeploy*
