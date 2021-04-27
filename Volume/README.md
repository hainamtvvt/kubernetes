# Volume-in-Kubernetes
## 1.1 Các loại volume trong kubernetes
* emptyDir
* HostPath
* gcePersistentDisk
* NFS
### 1.1.1 emptyDir
* Là dạng volume được tạo ra khi 1 Pod được gán vào 1 node, tồn tại trong suốt quá trình Pod chạy trên Node.empty volume là một thành phần thuộc Pod.
* Volume trống.
* Container trong Pod có thể **đọc** và **ghi** dữ liệu vào volume này.
* Khi Pod xóa khỏi Node hoặc khởi động lại thì dữ liệu trong emptyDir bị xóa.
* Trong trường hợp Container bị crash thì dữ liệu không bị mất.
* emptyDir lưu trữ ở thư mục /var/lib/kubelet của host.

***Ví dụ***

![Capture](https://user-images.githubusercontent.com/63154819/95811134-cdfc6080-0d3c-11eb-9bd0-1973d363a6f9.PNG)

(*mount volume emptyDir vào thư mục cache)*.

=>***emptyDir volume không dùng cho việc lưu trữ các dữ liệu yêu cầu có tính dài hạn như: database, dữ liệu ứng dụng, dữ liệu giám sát hệ thống...).***

* Một số trường hợp sử dụng emptyDir volume
  + thường volume emptyDir dùng với mục đích là một thư mục chứa cache tạm thời( temporary cache).
  + ***Là khu vực lưu trữ dữ liệu chung (shared storage) giữa các container trong cùng 1 Pod từ đó các container trong cùng Pod có thể trao đổi dữ liệu với nhau nhau volume chung này.***
### 1.2.1 hostPath
* Là dạng volume sẽ mount file hoặc thư mục trên máy host vào Pod. 
* Khác với emptyDir volume là dữ liệu sẽ bị mất khi Pod bị xóa hay bị khởi động lại thì với hostPath volume dữ liệu được lưu trong volumes sẽ không bị mất khi Pods bị lỗi. Khi Pods được tạo mới để thay thế Pods cũ, nó sẽ mount đến hostPath volume để làm tiếp với các dữ liệu ở Pods cũ.

![Capture](https://user-images.githubusercontent.com/63154819/95813175-7d3b3680-0d41-11eb-9c3c-8ebac6c108ce.PNG)


![Capture](https://user-images.githubusercontent.com/63154819/95812872-a0191b00-0d40-11eb-998b-afcf41e1c076.PNG)

## 1.2 PersistentVolume
* PersistentVolume (PV) là nguồn tài nguyên lưu trữ qua mạng (networked storage) được cung cấp bởi admin trong 1 cluster.
* PersistentVolumeClaim(PVC) là yêu cầu của user về volume để sử dụng trong một Pod.

## Tầm quan trọng của việc lưu trữ liên tục Persistent storeage
* Một ví dụ về phương tiện lưu trữ dài hạn là các file system được nối mạng ( NFS, Ceph, GlusterFS...) hoặc các tùy chọn dựa trên Cloud, chẳng hạn như Azure Disk, Amazon EBS,...
=> ***Đây là cách có thể gắn NFS (Network File System) vào Pod bằng cách sử dụng loại Volume nfs***. Có thể trỏ đến một cá thể NFS hiện có bằng cách sử dụng thuộc tính server.

![Capture](https://user-images.githubusercontent.com/63154819/97257816-9bc52580-1849-11eb-9520-c2e5309b5c4a.PNG)

* trong bảng Pod trên, thông tin lưu trữ đối với NFS được chỉ định trực tiếp trong Pod ( sử dụng phần volume). Điều này có nghĩa rằng các dev cần biết chi tiết về máy chủ NFS, bao gồm cả vị trí của nó... Chắc chắn có phạm vi để cải thiện ở đây và giống như hầu hết mọi thành phần trong phần mềm, nó có thể được thực hiện với một mức độ gián tiếp hoặc trừu tượng khác nhau bằng cách sử dụng các khái niệm Persisten Volume và Persisten Volume Claim.
