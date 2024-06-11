# Demo Service trên Kubernetes

Demo service trên Kubernetes

## Biến môi trường cần thiết cho app NodeJS

|    Tên biến   | Điều kiện  |           Giá trị mẫu            |      Diễn giải       |
|---------------|------------|----------------------------------|----------------------|
| `PORT`        | `required` | 3000                             | Cổng app lắng nghe   |
| `MONGODB_URI` | `required` | `mongodb://<host>:<port>/<dbname>` | Kết nối tới mongodb` |

Docker image để chạy app NodeJS: `minhpq331/demo-service`. Đã có sẵn chỉ cần pull về dùng

## Các lệnh kubectl hay dùng:

```bash
# Check các pod đang chạy
kubectl -n <ns> get pod

# Log pod đang chạy
kubectl -n <ns> logs <pod-name> 

# Check các deployment
kubectl -n <ns> get deployment

# Check các service
kubectl -n <ns> get service

# Kiểm tra mô tả service
kubectl -n <ns> describe service <svc-name>

# Apply định nghĩa resource
kubectl -n <ns> apply -f file.yaml

# Forward port về local
kubectl -n <ns> port-forward svc/<svc-name> <local_port>:<service_port>
```

## Task 1

- Tạo namespace mang tên mình bằng `kubectl create ns <name>`
- Copy nội dung `deployment.template.yaml` ra 1 file `deployment.mongo.yaml`
- Chỉnh sửa `deployment.mongo.yaml` (tên, image, label,...) sử dụng image official [mongodb](https://hub.docker.com/_/mongo) (không cần quan tâm tới volume hay data).
- Apply deployment mongodb trên bằng `kubectl apply`
- Kiểm tra mongo đã chạy thành công hay chưa bằng `kubectl logs`
- Tạo 1 file `service.mongo.yaml` và copy phần định nghĩa ClusterIP trong `service.demo.yaml`
- Sửa lại tên service, label selector, service port, container port... tương ứng với MongoDB
- Apply service mongodb vừa tạo bằng `kubectl apply`

## Task 2

- Copy nội dung `deployment.template.yaml` ra 1 file `deployment.app.yaml`
- Chỉnh sửa `deployment.app.yaml` (tên, image, label,...) sử dụng image `minhpq331/demo-service` và replica=2
- Chỉnh sửa biến môi trường của app với `MONGO_URI` trỏ tới service mongodb vừa mới tạo ở trên (sử dụng full DNS của service và port tương ứng). Database name đặt bất kỳ.
- Apply deployment app trên bằng `kubectl apply`
- Kiểm tra app đã chạy thành công hay chưa bằng `kubectl logs`
- Tạo 1 file `service.app.yaml` và copy phần định nghĩa NodePort trong `service.demo.yaml`
- Sửa lại tên service, label selector, service port, container port, node port... tương ứng với app NodeJS
- Apply service app vừa tạo bằng `kubectl apply`
- Truy cập IP của host ở cổng NodePort đã chọn (30000 ~> 32767) để test thử ứng dụng

## Task 3

- Sửa service app NodeJS phía trên về dạng ClusterIP
- Chỉnh sửa file `ingress.demo.yaml` và thêm ingress resource với domain `<tênmình>.demo.service` và path prefix `/crud` trong url trỏ tới tên service app ở trên.
- Apply file `ingress.demo.yaml` bằng `kubectl apply`
- Kiểm tra ingress đang hoạt động bằng lệnh `kubectl -n <ns> get ingress` và `kubectl describe ingress <name>`
- Chỉnh sửa file host của hệ thống để trỏ domain `<tênmình>.demo.service` tới IP của host
    + Với windows là `C:\Windows\System32\drivers\etc`
    + Với mac, linux là `/etc/hosts`
- Truy cập app CRUD qua `http:///<tênmình>.demo.service:30000/crud` (30000 là port của nginx-ingress-controller đang chạy sẵn)

CREATE TABLE IF NOT EXISTS public.fp_log
(
    created_at date,
    id bigint NOT NULL DEFAULT nextval('fp_log_id_seq1'::regclass),
    account character varying(255) COLLATE pg_catalog."default",
    action character varying(255) COLLATE pg_catalog."default",
    browser character varying(255) COLLATE pg_catalog."default",
    fp character varying(255) COLLATE pg_catalog."default",
    user_agent character varying(255) COLLATE pg_catalog."default",
    web character varying(255) COLLATE pg_catalog."default",
    components jsonb,
    random_txt character varying(255) COLLATE pg_catalog."default",
    CONSTRAINT fp_log_pkey PRIMARY KEY (id)
)

created_at	id	account	action	browser	fp	user_agent	web	components	random_txt
6/11/2024	6882	2291	XOA_COOKIE	Chrome - 125 - Windows	6d5447f387386869a9f070d12023b693	Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36	SMO	{"audio": {"value": 0.0000832115, "duration": 63}, "osCpu": {"duration": 0}, "vendor": {"value": "Google Inc.", "duration": 0}, "applePay": {"value": -1, "duration": 0}, "cpuClass": {"duration": 0}, "platform": {"value": "Win32", "duration": 0}, "indexedDB": {"value": true, "duration": 0}, "languages": {"value": [["en-US"]], "duration": 0}, "colorDepth": {"value": 24, "duration": 0}, "colorGamut": {"value": "srgb", "duration": 0}, "monochrome": {"value": 0, "duration": 0}, "webGlBasics": {"value": {"vendor": "WebKit", "version": "WebGL 1.0 (OpenGL ES 2.0 Chromium)", "renderer": "WebKit WebGL", "vendorUnmasked": "Google Inc. (Intel)", "rendererUnmasked": "ANGLE (Intel, Intel(R) HD Graphics 630 (0x00005912) Direct3D11 vs_5_0 ps_5_0, D3D11)", "shadingLanguageVersion": "WebGL GLSL ES 1.0 (OpenGL ES GLSL ES 1.0 Chromium)"}, "duration": 9}, "architecture": {"value": 255, "duration": 0}, "deviceMemory": {"value": 8, "duration": 0}, "forcedColors": {"value": false, "duration": 0}, "localStorage": {"value": true, "duration": 0}, "openDatabase": {"value": false, "duration": 0}, "vendorFlavors": {"value": ["chrome"], "duration": 0}, "cookiesEnabled": {"value": true, "duration": 0}, "invertedColors": {"duration": 0}, "sessionStorage": {"value": true, "duration": 0}, "pdfViewerEnabled": {"value": false, "duration": 0}, "privateClickMeasurement": {"duration": 0}}	dkcjzh
{
  "audio": {
    "value": 0.0000832115,
    "duration": 63
  },
  "osCpu": {
    "duration": 0
  },
  "vendor": {
    "value": "Google Inc.",
    "duration": 0
  },
  "applePay": {
    "value": -1,
    "duration": 0
  },
  "cpuClass": {
    "duration": 0
  },
  "platform": {
    "value": "Win32",
    "duration": 0
  },
  "indexedDB": {
    "value": true,
    "duration": 0
  },
  "languages": {
    "value": [
      [
        "en-US"
      ]
    ],
    "duration": 0
  },
  "colorDepth": {
    "value": 24,
    "duration": 0
  },
  "colorGamut": {
    "value": "srgb",
    "duration": 0
  },
  "monochrome": {
    "value": 0,
    "duration": 0
  },
  "webGlBasics": {
    "value": {
      "vendor": "WebKit",
      "version": "WebGL 1.0 (OpenGL ES 2.0 Chromium)",
      "renderer": "WebKit WebGL",
      "vendorUnmasked": "Google Inc. (Intel)",
      "rendererUnmasked": "ANGLE (Intel, Intel(R) HD Graphics 630 (0x00005912) Direct3D11 vs_5_0 ps_5_0, D3D11)",
      "shadingLanguageVersion": "WebGL GLSL ES 1.0 (OpenGL ES GLSL ES 1.0 Chromium)"
    },
    "duration": 9
  },
  "architecture": {
    "value": 255,
    "duration": 0
  },
  "deviceMemory": {
    "value": 8,
    "duration": 0
  },
  "forcedColors": {
    "value": false,
    "duration": 0
  },
  "localStorage": {
    "value": true,
    "duration": 0
  },
  "openDatabase": {
    "value": false,
    "duration": 0
  },
  "vendorFlavors": {
    "value": [
      "chrome"
    ],
    "duration": 0
  },
  "cookiesEnabled": {
    "value": true,
    "duration": 0
  },
  "invertedColors": {
    "duration": 0
  },
  "sessionStorage": {
    "value": true,
    "duration": 0
  },
  "pdfViewerEnabled": {
    "value": false,
    "duration": 0
  },
  "privateClickMeasurement": {
    "duration": 0
  }
}
