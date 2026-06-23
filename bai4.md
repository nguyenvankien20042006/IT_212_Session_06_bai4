# BÀI 4: Thực hành Tích hợp API Vận chuyển Bất đồng bộ (Technical Learning Prompts)

---

# 1. Prompt học tập kiến thức kỹ thuật nâng cao

```text
[Vai trò]

Bạn là Senior Java Backend Engineer và Solution Architect có hơn 10 năm kinh nghiệm về:

- Spring Boot
- Spring WebFlux
- Reactive Programming
- Microservices
- High Performance Systems

Bạn có khả năng hướng dẫn lập trình viên Junior đến Senior theo phương pháp đào tạo thực chiến.

[Mục tiêu]

Giúp tôi hiểu cách tích hợp API vận chuyển bất đồng bộ bằng Spring WebClient và triển khai được mã nguồn Production-ready.

[Ngữ cảnh]

Hệ thống thương mại điện tử SpeedyCart sau khi đặt hàng thành công cần đồng bộ dữ liệu sang hệ thống vận chuyển bên thứ ba (GHN, Viettel Post,...).

Yêu cầu:

- Không được làm chậm luồng xử lý chính.
- Gọi API bất đồng bộ.
- Chịu được tải lớn.
- Có cơ chế Retry khi mạng lỗi.

Tôi đã biết:

- Java Core
- Spring Boot
- REST API

Nhưng chưa từng sử dụng:

- Spring WebFlux
- WebClient
- Reactive Programming

[Ràng buộc]

1. Giải thích theo từng cấp độ.
2. Có ví dụ thực tế dễ hiểu.
3. Có so sánh với RestTemplate.
4. Mã nguồn phải tương thích Spring Boot 3.x.
5. Code phải theo chuẩn Production-ready.
6. Sử dụng Lombok và @Slf4j.
7. Retry tối đa 3 lần.
8. Timeout kết nối 5 giây.
9. Giải thích từng dòng code quan trọng.

[Định dạng đầu ra]

PHẦN 1 - LEVEL-BASED EXPLANATION

Cấp độ 1 (Beginner)

Giải thích:

- Blocking là gì?
- Non-blocking là gì?
- WebClient hoạt động như thế nào?

Sử dụng ví dụ thực tế:

Nhà hàng, phục vụ bàn, đặt món ăn,...

Cấp độ 2 (Senior Developer)

Giải thích:

- Event Loop
- Reactive Streams
- Publisher / Subscriber
- Mono
- Flux
- Backpressure

Kèm sơ đồ ASCII.

PHẦN 2 - COMPARATIVE ANALYSIS

Lập bảng so sánh:

| Tiêu chí | RestTemplate | WebClient |

Bao gồm:

- Blocking/Non-blocking
- Thread Usage
- RAM Consumption
- Throughput
- Scalability
- 10.000 Concurrent Requests
- Độ phức tạp

Sau bảng hãy phân tích chi tiết.

PHẦN 3 - PRACTICAL EXAMPLE

Viết:

DeliveryIntegrationService

Yêu cầu:

1. Sử dụng Spring WebClient.
2. Gửi POST request sang Delivery API.
3. Timeout 5 giây.
4. Retry 3 lần.
5. Log INFO và ERROR.
6. Trả về Mono<DeliveryResponse>.
7. Xử lý lỗi rõ ràng.

Bao gồm:

- Maven Dependency
- WebClientConfig
- DTO Request
- DTO Response
- DeliveryIntegrationService

PHẦN 4 - BEST PRACTICES

Liệt kê:

- Những lỗi phổ biến khi dùng WebClient.
- Khi nào nên dùng Mono.
- Khi nào nên dùng Flux.
- Khi nào KHÔNG nên dùng Reactive.
```

---

# 2. Minh chứng chạy thực tế (Log phản hồi AI)

# PHẦN 1 - LEVEL-BASED EXPLANATION

## Cấp độ 1 (Beginner)

### Blocking là gì?

Ví dụ nhà hàng:

```text
Khách gọi món
    |
    v
Nhân viên chạy vào bếp
    |
    v
Đứng chờ món ăn
    |
    v
Mang ra cho khách
```

Trong thời gian chờ:

```text
Nhân viên KHÔNG làm việc khác
```

Đây là:

```text
Blocking
```

---

### Non-blocking là gì?

```text
Khách gọi món
    |
    v
Nhân viên ghi order
    |
    v
Đưa bếp
    |
    v
Tiếp tục phục vụ bàn khác
```

Khi món ăn xong:

```text
Bếp thông báo
```

Nhân viên quay lại phục vụ.

Đây là:

```text
Non-blocking
```

---

### WebClient hoạt động thế nào?

```text
Order Service
      |
      v
WebClient gửi request
      |
      v
Không đứng chờ
      |
      v
Tiếp tục xử lý việc khác
      |
      v
Nhận callback khi có response
```

---

## Cấp độ 2 (Senior Developer)

### Event Loop

WebClient sử dụng:

```text
Netty Event Loop
```

Thay vì:

```text
1 Request = 1 Thread
```

nó dùng:

```text
N Request
    |
    v
Few Event Loop Threads
```

---

### Reactive Streams

```text
Publisher
    |
    v
Subscriber
```

Publisher phát dữ liệu.

Subscriber nhận dữ liệu.

---

### Mono

```java
Mono<Order>
```

Tối đa:

```text
0 hoặc 1 phần tử
```

---

### Flux

```java
Flux<Order>
```

Có thể:

```text
0..N phần tử
```

---

### Backpressure

Giúp hệ thống:

```text
Không bị ngập dữ liệu
```

---

### Sơ đồ

```text
Client
  |
  v
Event Loop
  |
  v
WebClient
  |
  v
Remote API
```

---

# PHẦN 2 - COMPARATIVE ANALYSIS

| Tiêu chí | RestTemplate | WebClient |
|-----------|-------------|-----------|
| Mô hình | Blocking | Non-blocking |
| Thread | 1 request / 1 thread | Event Loop |
| RAM | Cao | Thấp |
| Throughput | Trung bình | Cao |
| Scalability | Trung bình | Rất cao |
| 10.000 Request | Rất nhiều thread | Ít thread |
| Reactive | Không | Có |
| Hiệu năng | Thấp hơn | Cao hơn |

---

## Phân tích

### RestTemplate

10.000 request:

```text
≈ 10.000 thread
```

Dẫn tới:

- Context switching cao.
- Tốn RAM.

---

### WebClient

10.000 request:

```text
≈ vài chục Event Loop Thread
```

Lợi ích:

- Tiết kiệm RAM.
- Hiệu năng cao.

---

# PHẦN 3 - PRACTICAL EXAMPLE

## Maven Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>
        spring-boot-starter-webflux
    </artifactId>
</dependency>
```

---

## WebClientConfig

```java
@Configuration
public class WebClientConfig {

    @Bean
    public WebClient deliveryWebClient() {

        HttpClient httpClient =
                HttpClient.create()
                        .responseTimeout(
                                Duration.ofSeconds(5));

        return WebClient.builder()
                .clientConnector(
                        new ReactorClientHttpConnector(
                                httpClient))
                .baseUrl(
                        "https://delivery-api.com")
                .build();
    }
}
```

---

## DeliveryRequest

```java
@Data
@Builder
public class DeliveryRequest {

    private Long orderId;

    private String receiverName;

    private String address;
}
```

---

## DeliveryResponse

```java
@Data
public class DeliveryResponse {

    private String trackingCode;

    private String status;
}
```

---

## DeliveryIntegrationService

```java
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;

import org.springframework.stereotype.Service;

import org.springframework.web.reactive.function.client.WebClient;

import reactor.core.publisher.Mono;

import reactor.util.retry.Retry;

import java.time.Duration;

@Service
@RequiredArgsConstructor
@Slf4j
public class DeliveryIntegrationService {

    private final WebClient deliveryWebClient;

    public Mono<DeliveryResponse>
    createShipment(DeliveryRequest request) {

        log.info(
                "Sending order {} to delivery provider",
                request.getOrderId());

        return deliveryWebClient
                .post()
                .uri("/shipments")
                .bodyValue(request)
                .retrieve()
                .bodyToMono(DeliveryResponse.class)

                .timeout(Duration.ofSeconds(5))

                .retryWhen(
                        Retry.fixedDelay(
                                3,
                                Duration.ofSeconds(1)))

                .doOnSuccess(response ->
                        log.info(
                                "Shipment created successfully. Tracking={}",
                                response.getTrackingCode()))

                .doOnError(error ->
                        log.error(
                                "Delivery API failed",
                                error));
    }
}
```

---

# PHẦN 4 - BEST PRACTICES

## Khi nào dùng Mono?

```java
Mono<User>
```

Khi chỉ cần:

```text
0 hoặc 1 kết quả
```

Ví dụ:

```java
findUserById()
```

---

## Khi nào dùng Flux?

```java
Flux<User>
```

Khi cần:

```text
Danh sách nhiều phần tử
```

Ví dụ:

```java
findAllUsers()
```

---

## Lỗi phổ biến

### Gọi block()

```java
webClient.get()
         .retrieve()
         .bodyToMono(...)
         .block();
```

=> Mất toàn bộ lợi ích Reactive.

---

### Retry vô hạn

```java
retry()
```

=> Có thể gây vòng lặp vô tận.

---

## Khi KHÔNG nên dùng Reactive

- CRUD đơn giản.
- Dự án nhỏ.
- Team chưa có kinh nghiệm Reactive.

---

# Kết luận

Đối với SpeedyCart:

```text
Order Service
     |
     v
WebClient
     |
     v
GHN / Viettel Post API
```

là giải pháp phù hợp vì:

- Non-blocking.
- Tiết kiệm tài nguyên.
- Chịu tải cao.
- Hỗ trợ Retry.
- Dễ mở rộng trong Microservices.