---
title: "시스템 연결 (MCP)"
weight: 4
---

## MCP (Model Context Protocol) 이해

MCP는 에이전트가 외부 도구·데이터·서비스에 연결하는 **표준 프로토콜**입니다.

```
에이전트 ←──MCP──→ 도구/서비스
         (표준화된 인터페이스)
```

**MCP의 핵심 개념**
- **MCP Server**: 도구나 데이터를 노출하는 서버 (파일 시스템, DB, API 등)
- **MCP Client**: 에이전트가 도구를 호출하는 클라이언트
- **Resources**: 에이전트가 읽을 수 있는 데이터 (파일, DB 레코드 등)
- **Tools**: 에이전트가 실행할 수 있는 함수
- **Prompts**: 재사용 가능한 프롬프트 템플릿

## API 도구 설계 원칙

### 좋은 API 도구 스펙

```json
{
  "name": "create_ticket",
  "description": "지원 티켓을 생성합니다. 고객 문의가 해결되지 않을 때 사용하세요.",
  "parameters": {
    "type": "object",
    "properties": {
      "title": {
        "type": "string",
        "description": "티켓 제목 (50자 이내)"
      },
      "priority": {
        "type": "string",
        "enum": ["low", "medium", "high", "critical"],
        "description": "우선순위: 결제 오류=high, 일반 문의=low"
      },
      "category": {
        "type": "string",
        "description": "문의 카테고리"
      }
    },
    "required": ["title", "priority", "category"]
  }
}
```

### 도구 설계 안티패턴

```
❌ 너무 광범위한 도구
   "database_query(sql)" → LLM이 임의 SQL 실행 가능, 보안 위험

✅ 좁고 안전한 도구
   "get_customer_orders(customer_id, limit=10)" → 제한된 조회만 허용
```

## 외부 업무 시스템 연결 패턴

| 시스템 유형 | 연결 방법 | 주의사항 |
|-----------|---------|---------|
| REST API | OpenAPI 기반 MCP 서버 | Rate limit, auth 토큰 관리 |
| 데이터베이스 | DB MCP 서버 (읽기 전용 권장) | 최소 권한 원칙 |
| 파일 시스템 | Filesystem MCP | 접근 경로 제한 |
| 이메일/캘린더 | Gmail/Outlook MCP | OAuth 토큰 관리 |
| 슬랙/협업 도구 | Slack MCP | 채널 접근 제한 |

{{< callout type="warning" >}}
**보안 원칙**: 에이전트에게 필요한 최소 권한만 부여하세요. 쓰기 권한은 특히 신중하게 관리합니다.
{{< /callout >}}
