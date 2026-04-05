# Backend Testing 示例（Go / Java / Python）

在产出增量测试更新时，可将以下片段作为风格参考。

## Go（table-driven + subtests）

```go
func TestCreateOrder_Idempotency(t *testing.T) {
    tests := []struct {
        name string
        req  CreateOrderRequest
        wantCode int
    }{
        {"first-request", CreateOrderRequest{IdempotencyKey: "k1"}, 201},
        {"duplicate-request", CreateOrderRequest{IdempotencyKey: "k1"}, 200},
    }
    for _, tc := range tests {
        t.Run(tc.name, func(t *testing.T) {
            code := callCreateOrder(tc.req)
            if code != tc.wantCode {
                t.Fatalf("code=%d want=%d", code, tc.wantCode)
            }
        })
    }
}
```

## Java（JUnit 5 + Mockito）

```java
@Test
void shouldRollbackWhenPaymentGatewayTimeout() {
    when(paymentClient.charge(any())).thenThrow(new TimeoutException("timeout"));
    assertThrows(BusinessException.class, () -> orderService.placeOrder(req));
    verify(orderRepository, never()).saveCommittedOrder(any());
}
```

## Python（pytest + parameterize）

```python
@pytest.mark.parametrize(
    "role,expected",
    [
        ("admin", 200),
        ("viewer", 403),
    ],
)
def test_update_user_permission(client, token_factory, role, expected):
    token = token_factory(role=role)
    resp = client.patch("/users/1", headers={"Authorization": f"Bearer {token}"})
    assert resp.status_code == expected
```

## 增量更新的 Mapping 指南

- `API contract changed`：优先更新 request/response assertions。
- `transaction logic changed`：补充 rollback 与 partial-failure tests。
- `auth logic changed`：补充 role matrix 与 tenancy isolation 用例。
- `new external dependency`：补充 timeout、retry、circuit-breaker、fallback tests。
- `concurrency-sensitive code changed`：补充 race、duplicate request、lock contention tests。
