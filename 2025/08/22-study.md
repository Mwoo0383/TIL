# Room91 리팩토링 작업 요약(진행 로그)

> 도메인 중심 구조로 개편 · 엔티티 노출 제거 · 공통 응답/예외 일원화 · 프런트/백 연동 수정  
> **흐름**: 패키지 재구성 → 공통 베이스/응답/예외 도입 → DTO/매퍼 적용 → 컨트롤러 교체 → 저장소 스캔/설정 정리 → 프런트 API 파싱/가드 적용

---

## 1) 브랜치
- 생성: `refactor/domain-package-structure-issue-12`  
- 규칙: `{type}/{slug}-{issueNumber}`

---

## 2) 목표/원칙
- 도메인 중심 폴더링(`domain/<bounded-context>/{controller,service,repository,dto,entity}`)
- 컨트롤러에서 **엔티티 직접 노출 금지** → DTO 변환
- 응답 스키마 통일: `ApiResponse<T>`
- 예외 응답 중앙 처리: `@RestControllerAdvice`
- 공통/공유는 `global/common` 하위로만 배치, 보안/설정은 `global/config` 하위로 관심사별 분리


---

## 3) 공통 베이스: BaseEntity / BaseTimeEntity
```java
// global/common/base/BaseEntity.java (id + 시간)
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseEntity {
  @Id @GeneratedValue(strategy = GenerationType.IDENTITY) private Long id;
  @CreatedDate @Column(updatable=false, nullable=false) private LocalDateTime createdAt;
  @LastModifiedDate @Column(nullable=false) private LocalDateTime updatedAt;
}

// global/common/base/BaseTimeEntity.java (시간만)
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseTimeEntity {
  @CreatedDate @Column(updatable=false, nullable=false) private LocalDateTime createdAt;
  @LastModifiedDate @Column(nullable=false) private LocalDateTime updatedAt;
}
```

---
## 4) 공통 응답 래퍼
```Java
public record ApiResponse<T>(boolean success, T data, String message) {
  public static <T> ApiResponse<T> ok(T data){ return new ApiResponse<>(true,data,null); }
  public static <T> ApiResponse<T> ok(T data, String msg){ return new ApiResponse<>(true,data,msg); }
  public static <T> ApiResponse<T> fail(String msg){ return new ApiResponse<>(false,null,msg); }
}
```

---
## 5) 예외처리
```java
// global/common/exception/ErrorCode.java
public enum ErrorCode {
  EXTERNAL_API_ERROR(HttpStatus.BAD_GATEWAY, "외부 API 통신 실패");
  public final HttpStatus status; public final String message;
  ErrorCode(HttpStatus s, String m){ this.status=s; this.message=m; }
}

// global/common/exception/BizException.java
public class BizException extends RuntimeException {
  private final ErrorCode code;
  public BizException(ErrorCode code, Throwable cause){ super(code.message, cause); this.code=code; }
  public ErrorCode getCode(){ return code; }
}

// global/common/exception/GlobalExceptionHandler.java
@RestControllerAdvice
public class GlobalExceptionHandler {
  @ExceptionHandler(BizException.class)
  public ResponseEntity<ApiResponse<Void>> handleBiz(BizException e){
    return ResponseEntity.status(e.getCode().status).body(ApiResponse.fail(e.getMessage()));
  }
  @ExceptionHandler(Exception.class)
  public ResponseEntity<ApiResponse<Void>> handle(Exception e){
    return ResponseEntity.status(500).body(ApiResponse.fail("서버 오류가 발생했습니다."));
  }
}
```
