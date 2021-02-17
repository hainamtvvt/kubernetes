# ConfigMap  & Secret
***(Thông thường khi lập trình các ứng dụng, chúng ta đều cho các biến quan trọng (như mật khẩu url Database, secret key, tên Database...) vào file.env dưới dạng biến môi trường để đảm bảo tính bí mật.Tuy nhiên trong hệ thống Kubernetes, Config Map và Secret là 2 loại volumes giúp lưu trữ các biến môi trường để dùng cho các container thuộc các Pods khác nhau. Config Map lưu trữ các thông tin không quá nhạy cảm nhưng vẫn cần bí mật, Secret được dùng để lưu trữ các thông tin quan trọng. Khác với các loại volumes khác, Config Map và Secret sẽ được định nghĩa riêng với file .yaml thay vì cấu hình chung ở trong file.yaml khởi tạo Pods)***.


![Capture](https://user-images.githubusercontent.com/63154819/95157062-38a02000-07c2-11eb-91e9-709bc2f66ffc.PNG)

![Capture](https://user-images.githubusercontent.com/63154819/95157113-58374880-07c2-11eb-94d4-17645c9ec519.PNG)

## Sử dụng ConfigMaps
* Cách thức hoạt động là cấu hình được xác định trong ConfigMaps, sau đó được tham chiếu trong Pod hoặc Deployment.

**Sử dụng manifest file**
  - Nó có thể tạo một ConfigMap cùng với config data được lưu trữ dưới dạng cặp khóa-giá trị trong phần data của definition.

![Capture](https://user-images.githubusercontent.com/63154819/95158547-10b2bb80-07c6-11eb-831c-99b4936d0e5e.PNG)

![Capture1](https://user-images.githubusercontent.com/63154819/95158549-11e3e880-07c6-11eb-97bf-02c5e6431aa3.PNG)

* ConfingMap có tên simpleconfig  chứa hai phần dữ liệu: hello=world và foo=bar.
* simpleconfig được tham chiếu vào Pod2 và dữ liệu foo và hello được sử dụng như các biến enviroment HELLO_ENV_VAR và FOO_ENV_VAR tương ứng.
