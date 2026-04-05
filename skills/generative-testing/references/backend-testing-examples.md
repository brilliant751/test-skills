# Backend Testing 示例（Go / Java / Python）

生成测试资产时，可将以下片段作为实现风格参考。

## Go（HTTP handler error path）

```go
func TestGetProfile_Unauthorized(t *testing.T) {
    req := httptest.NewRequest(http.MethodGet, "/profile", nil)
    rr := httptest.NewRecorder()
    router.ServeHTTP(rr, req)
    if rr.Code != http.StatusUnauthorized {
        t.Fatalf("status=%d want=%d", rr.Code, http.StatusUnauthorized)
    }
}
```

## Java（Spring Boot integration）

```java
@Test
void shouldRejectChangeWhenInsufficientBalance() throws Exception {
    mockMvc.perform(post("/transfer")
            .contentType(MediaType.APPLICATION_JSON)
            .content("{\"from\":\"A\",\"to\":\"B\",\"amount\":99999}"))
        .andExpect(status().isBadRequest())
        .andExpect(jsonPath("$.errorCode").value("INSUFFICIENT_BALANCE"));
}
```

## Python（pytest boundary case）

```python
def test_coupon_expiry_boundary(client):
    payload = {"coupon": "SPRING50", "ts": "2026-04-04T23:59:59Z"}
    resp = client.post("/apply-coupon", json=payload)
    assert resp.status_code in (200, 400)  # define expected behavior by AC
```

## 生成资产的 Mapping 指南

- Requirement-only 输入：优先 black-box 测试设计与边界场景。
- Code-based 输入：纳入 white-box 信号，如分支与错误处理路径。
- 混合输入：在扩展 test case 之前先产出 AC-to-code traceability matrix。
- 始终给出 observability 检查点：response、DB state、logs、events。
