# Deployment-trong-K8s
* Deployment quản lý một nhóm các Pod, các Pod được scale tự động thay thế các Pod bị lỗi. Như vậy Deployment đảm bảo ứng dụng có số lượng Pod để phục vụ các yêu cầu
* Deployment sử dụng template Pod để tạo các Pod, khi template này thay đổi, các Pod mới sẽ được tạo để thay thế các Pod cũ.
* file cấu hình Deployment(ymal).
![Capture](https://user-images.githubusercontent.com/63154819/94506531-ccf50a80-0237-11eb-8fbf-82db49fe4925.PNG)
* Thực hiện lệnh sau để triển khai: kubectl apply -f 1.myapp-deploy.yaml
  Khi Deployment tạo ra, tên của nó là deployapp, có thể kiểm tra với lệnh: kubectl get deploy -o wide
* Deploy này sinh ra một ResplicaSet và quản lý nó, dùng lệnh sau để hiển thị các thông tin của ResplicaSet: kubectl get rs -o wide
* Thông tin chi tiết về deploy: kubectl describe deploy/deployapp
## Cập nhập Deployment
* Có thể cập nhập một Deployment bằng cách sử đổi template của Pod trong file Deployment.yaml. Khi template cập nhập thì nó tự động triển khai lại các Pod ( sau khi sửa file .yaml để cập nhập lại: kubectl apply -f....yaml)
* Khi một Deployment được cập nhập thì Deployment dừng các Pod, scale lại số lượng Pod về 0, sau đó sử dụng template mới để scale lại Pod. Để xem quá trình này diễn ra như thế nào: kubectl describe deploy/namedeploy.
* Thu hồi bản cập nhập: kubectl rollout undo.
