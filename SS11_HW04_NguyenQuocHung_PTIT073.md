# Bài 4: Chiến Lược Sinh Dữ Liệu Giả Lập & Test Case

## 1. Mega-Prompt Sinh Mock Data & Test Case

```text
Đóng vai là một QA Automation Engineer. 
Nhiệm vụ: Sinh dữ liệu giả lập (Mock Data) và viết bộ Unit Test sử dụng JUnit 5 cho phương thức `login(String username, String password)` thuộc class `UserAuthentication`. Hệ thống chưa có Database test nên cần sử dụng Mock Data trực tiếp. Hãy tự giả định logic bên trong hàm `login` (ví dụ: trả về đối tượng User, true/false, hoặc ném Exception tương ứng).

Ràng buộc & Yêu cầu thực hiện:
1. Sinh bộ dữ liệu giả lập chuẩn JSON gồm 5 user đại diện cho 5 kịch bản kiểm thử:
   - (1) Happy Path: Đăng nhập thành công (Thông tin đúng).
   - (2) Edge Case: Đăng nhập thất bại (Sai mật khẩu).
   - (3) Edge Case: Tài khoản bị khóa (Locked status).
   - (4) Security: Chứa mã độc SQL Injection ở tham số username.
   - (5) Boundary: Mật khẩu bị bỏ trống (Empty/Blank).
2. Sinh mã nguồn JUnit 5: Viết một class test bằng Java. Bên trong, mô phỏng việc khai báo bộ JSON đó (dưới dạng chuỗi hoặc list giả lập) và viết 5 phương thức `@Test` tương ứng với 5 kịch bản trên để kiểm thử hàm `login`.

Định dạng trả về:
- Trả về bộ Mock Data ở một khối code định dạng JSON.
- Trả về mã nguồn Java ở một khối code định dạng Java.
- Tuyệt đối không thêm văn xuôi giải thích, chỉ xuất 2 khối code.
```

## 2. Kết quả đầu ra từ AI

### Mock Data (JSON)
```json
[
  {
    "testCaseId": 1,
    "scenario": "Valid credentials",
    "username": "user_active",
    "password": "CorrectPassword123",
    "expectedResult": "SUCCESS"
  },
  {
    "testCaseId": 2,
    "scenario": "Wrong password",
    "username": "user_active",
    "password": "WrongPassword999",
    "expectedResult": "INVALID_CREDENTIALS"
  },
  {
    "testCaseId": 3,
    "scenario": "Locked account",
    "username": "user_locked",
    "password": "CorrectPassword123",
    "expectedResult": "ACCOUNT_LOCKED"
  },
  {
    "testCaseId": 4,
    "scenario": "SQL Injection attempt",
    "username": "admin' OR '1'='1",
    "password": "AnyPassword",
    "expectedResult": "INVALID_FORMAT"
  },
  {
    "testCaseId": 5,
    "scenario": "Empty password",
    "username": "user_active",
    "password": "",
    "expectedResult": "EMPTY_FIELD_ERROR"
  }
]
```

### Mã nguồn JUnit 5 (Java)
```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class UserAuthenticationTest {

    private UserAuthentication authService;

    @BeforeEach
    void setUp() {
        // Giả định khởi tạo class UserAuthentication cần test
        authService = new UserAuthentication();
    }

    @Test
    void testLogin_ValidCredentials_ReturnsSuccess() {
        // Kịch bản 1: Thông tin đúng (user_active / CorrectPassword123)
        boolean result = authService.login("user_active", "CorrectPassword123");
        assertTrue(result, "Đăng nhập phải thành công khi tài khoản và mật khẩu hợp lệ.");
    }

    @Test
    void testLogin_WrongPassword_ThrowsException() {
        // Kịch bản 2: Sai mật khẩu (user_active / WrongPassword999)
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            authService.login("user_active", "WrongPassword999");
        });
        assertEquals("Invalid credentials", exception.getMessage());
    }

    @Test
    void testLogin_LockedAccount_ThrowsLockedException() {
        // Kịch bản 3: Tài khoản bị khóa (user_locked / CorrectPassword123)
        Exception exception = assertThrows(IllegalStateException.class, () -> {
            authService.login("user_locked", "CorrectPassword123");
        });
        assertEquals("Account is locked", exception.getMessage());
    }

    @Test
    void testLogin_SqlInjectionAttempt_ThrowsSecurityException() {
        // Kịch bản 4: Mã độc SQL Injection (admin' OR '1'='1 / AnyPassword)
        Exception exception = assertThrows(SecurityException.class, () -> {
            authService.login("admin' OR '1'='1", "AnyPassword");
        });
        assertEquals("Invalid characters in username", exception.getMessage());
    }

    @Test
    void testLogin_EmptyPassword_ThrowsValidationException() {
        // Kịch bản 5: Mật khẩu rỗng (user_active / "")
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            authService.login("user_active", "");
        });
        assertEquals("Password cannot be empty", exception.getMessage());
    }
}

// Dummy class giả định để tránh báo lỗi compile khi test
class UserAuthentication {
    public boolean login(String username, String password) {
        if (password == null || password.isEmpty()) {
            throw new IllegalArgumentException("Password cannot be empty");
        }
        if (username.contains("'")) {
            throw new SecurityException("Invalid characters in username");
        }
        if ("user_locked".equals(username)) {
            throw new IllegalStateException("Account is locked");
        }
        if ("user_active".equals(username) && "CorrectPassword123".equals(password)) {
            return true;
        }
        throw new IllegalArgumentException("Invalid credentials");
    }
}
```
