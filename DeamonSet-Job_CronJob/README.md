# DaemonSet & Job & CronJob trong Kubernetes.
## DaemonSet.
* DaemonSet có thể được coi là một bản sao đặc biệt của ReplicaSet. RaplicaSet sẽ bố trí tổng số Pod trên Node trong Kubernetes phù hợp với tình trạng tài nguyên của Node đó như thế nào. Chính vì vậy nên không hẳn số lượng Pod được phân bổ vào các Node là bằng nhau, và cũng không hẳn là phân bổ cho tất cả các Node.

=> Trường hợp sử dụng DaemonSet như: Khi chúng ta cần một chương trình nào đó mà nhất định phải chạy trên tất cả các Node.
* DaemonSet đảm bảo chạy trên mỗi Node một bản copy của Pod. Triển khai DaemonSet khi cần mỗi máy Node một Pod ( thường dùng cho các ứng dụng như thu thập log, tạo ổ đĩa trên mỗi Node...)

![Capture2](https://user-images.githubusercontent.com/63154819/95160035-bca9d600-07c9-11eb-8d28-8cd6871f8b43.PNG)
* Liệt kê các DaemonSet: kubectl get ds -o wide
* Liệt kê các Pod theo nhãn: kubectl get pod -o wide -l "app=ds-nginx"
* Chi tiết về DaemonSet: kubectl describe ds/dsapp
* Xóa DaemonSet: kubectl delete ds/dsapp

## Job trong Kubernetes
* Job có chức năng tạo các POD đảm bảo nó chạy và kết thúc thành công. Khi các Pod do Job tạo ra: chạy và kết thúc thành công thì Job đó hoàn thành. Khi xóa Job thì các Pod do Job tạo ra cũng bị xóa theo. Sử dụng Job khi muốn thi hành một vài chức năng hoàn thành xong thì dừng lại ( ví dụ như backup hoặc kiểm tra hoặc dọn dẹp hệ thống ...)
* Khi Job tạo Pod mà Pod chưa kết thúc bị xóa hoặc lỗi Node thì nó sẽ thực hiện tạo Pod khác. 
* Một tác vụ được coi là thành công khi nó tạo thành công các Pod và chạy thành công các lệnh trong container của Pod đó.

![Capture](https://user-images.githubusercontent.com/63154819/95161735-2330f300-07ce-11eb-8006-58e665c43c15.PNG)
(*backoff limit:3 tức là chạy quá 3 lần bị lỗi thì không tạo nữa*)

## CronJob trong Kubernetes
* CronJob: chạy Job theo một lịch được định sẵn. Việc lên lịch cho CronJob khai báo giống Cron của Linux.

![Capture1](https://user-images.githubusercontent.com/63154819/95161881-8458c680-07ce-11eb-9294-430d2bbe5701.PNG)

(khi tạo quá 3 job thì tự động xóa đi các Job cũ. Dấu * thể hiện mọi phút sẽ chạy, mọi giờ sẽ chạy....ví dụ: ![Capture](https://user-images.githubusercontent.com/63154819/95162967-11048400-07d1-11eb-974b-8a42562e64ae.PNG)
 ).
 
![Capture](https://user-images.githubusercontent.com/63154819/95163219-9720ca80-07d1-11eb-816d-35d077c728b0.PNG)
