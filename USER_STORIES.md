# Limit TP/SL — 설정별 유저 스토리

## 공통 진입 플로우
```
1. 포지션 상세 또는 주문폼에서 TP/SL 설정 진입
2. Take Profit / Stop Loss 탭 선택
3. Trigger price 입력
4. Trigger price 라벨 옆 chip(Market ▾)으로 실행 방식 변경 가능
   → 탭하면 바텀시트로 Market / Limit / BBO 선택
5. 기본값은 Market — 대부분의 사용자는 chip을 건드리지 않고 그대로 사용
```

---

## 1. Market (기본값)

### 유저 스토리
> **트레이더로서**, TP/SL 트리거 가격만 설정하면 시장가로 즉시 체결되게 하고 싶다.
> **그래야** 가격 도달 시 확실하게 포지션이 청산되니까.

### 플로우
```
1. Trigger price 입력 (예: 130,000)
   → "+10.02% from entry" 실시간 표시
   → 25% / 50% / 75% / 100% 버튼으로 빠른 조정
2. chip은 "Market ▾" 기본 상태 — 터치 불필요
3. [Confirm] 탭
```

### 화면 구성
```
┌─────────────────────────────────┐
│ Trigger price          Market ▾ │
│ 130,000.0    +10.02% from entry │
│                                 │
│  (추가 필드 없음)                │
│                                 │
│ Estimated P&L     +1.10 (+9.99%)│
│ Entry price         12,431.2 USDT│
│ Liquidation price   67,288 USDT │
│                                 │
│  25%    50%    75%    100%      │
│         [ Confirm ]             │
└─────────────────────────────────┘
```

### 트리거 후
- Mid Price → Trigger Price 도달
- **시장가 주문** 즉시 제출 (Reduce-only)
- 거의 즉시 체결 (슬리피지 발생 가능)

---

## 2. Limit (신규)

### 유저 스토리
> **트레이더로서**, TP/SL이 발동될 때 내가 원하는 가격에 지정가 주문이 걸리게 하고 싶다.
> **그래야** 슬리피지 없이 원하는 가격에 체결할 수 있으니까.

### 플로우
```
1. Trigger price 입력 (예: 130,000)
2. "Market ▾" chip 탭 → 바텀시트 열림
3. 바텀시트에서 "Limit" 선택
   → chip이 "Limit ▾"로 변경
   → Trigger price 아래에 Limit price 필드가 나타남
4. Limit price 입력 (기본값: Trigger price와 동일)
   → 체결률을 높이려면 Trigger보다 약간 불리한 가격 입력
     예) TP Long: Trigger 130,000 → Limit 129,800
     예) SL Long: Trigger 100,000 → Limit  99,800
5. 경고 문구 확인
   - TP: "Limit order may not execute if the market does not
          reach your limit price."
   - SL: "Unfilled SL limit order may increase liquidation risk."
6. [Confirm] 탭
```

### 화면 구성
```
┌─────────────────────────────────┐
│ Trigger price           Limit ▾ │
│ 130,000.0    +10.02% from entry │
│                                 │
│ Limit price                     │
│ 129,800.0   +0.00% from trigger │
│                                 │
│ ⚠ Limit order may not execute   │
│   if the market does not reach  │
│   your limit price.             │
│                                 │
│ Estimated P&L     +1.10 (+9.99%)│
│ Entry price         12,431.2 USDT│
│ Liquidation price   67,288 USDT │
│                                 │
│  25%    50%    75%    100%      │
│         [ Confirm ]             │
└─────────────────────────────────┘
```

### 트리거 후
- Mid Price → Trigger Price 도달
- **지정가 주문** Limit Price로 제출 (Reduce-only, GTC)
- 오더북에 올라가서 대기
- 체결될 수도, 안 될 수도 있음 (부분 체결 가능)
- Open Orders > TP/SL 탭에서 확인 및 취소 가능
- 포지션 종료 시 자동 취소

### 미체결 리스크
| TP/SL | 미체결 시 |
|---|---|
| TP Limit | 수익 실현 못 함 (추가 손해는 아님) |
| SL Limit | 손절 실패 → **청산 리스크 증가** |

---

## 3. BBO (신규)

### 유저 스토리
> **트레이더로서**, TP/SL이 발동될 때 그 순간의 최우선 호가로 지정가 주문이 걸리게 하고 싶다.
> **그래야** 시장가보다 유리한 가격(메이커 수수료)을 받으면서도 체결 확률이 높으니까.

### 플로우
```
1. Trigger price 입력 (예: 130,000)
2. "Market ▾" chip 탭 → 바텀시트 열림
3. 바텀시트에서 "BBO" 선택
   → chip이 "BBO ▾"로 변경
   → "Limit price  Best Bid / Offer" 파란색 텍스트 표시
4. Limit price 입력 불필요 — 트리거 시 자동 결정
5. 경고 문구 확인
   "Limit price will be set to best bid/offer at trigger time."
6. [Confirm] 탭
```

### 화면 구성
```
┌─────────────────────────────────┐
│ Trigger price             BBO ▾ │
│ 130,000.0    +10.02% from entry │
│                                 │
│ Limit price   Best Bid / Offer  │
│                                 │
│ ⚠ Limit price will be set to   │
│   best bid/offer at trigger     │
│   time.                         │
│                                 │
│ Estimated P&L     +1.10 (+9.99%)│
│ Entry price         12,431.2 USDT│
│ Liquidation price   67,288 USDT │
│                                 │
│  25%    50%    75%    100%      │
│         [ Confirm ]             │
└─────────────────────────────────┘
```

### 트리거 후
- Mid Price → Trigger Price 도달
- 그 순간의 **최우선 매수호가(매도 시) / 최우선 매도호가(매수 시)** 확인
- 해당 가격으로 **지정가 주문** 제출 (Reduce-only, GTC)
- 체결 확률이 Limit보다 높음 (최적 호가이므로)
- 메이커 수수료 적용 가능 (시장가 대비 수수료 절감)

---

## 바텀시트 옵션 설명

chip 탭 시 열리는 바텀시트 구성:

```
┌─────────────────────────────────┐
│  ──                             │
│  Execution type                 │
│                                 │
│  ● Market                       │
│    Executes at market price     │
│    when triggered. Fastest      │
│    fill, but price may vary.    │
│                                 │
│  ○ Limit                   NEW  │
│    Places a limit order at      │
│    your specified price. No     │
│    slippage, but may not fill.  │
│                                 │
│  ○ BBO                     NEW  │
│    Limit order at best bid/     │
│    offer. High fill rate with   │
│    potential maker rebate.      │
└─────────────────────────────────┘
```

---

## 옵션 선택 가이드

| 상황 | 추천 |
|---|---|
| 확실한 체결이 최우선 | **Market** |
| 정확한 가격에 체결하고 싶음 | **Limit** |
| 체결 확률 + 수수료 절감 | **BBO** |
| 대규모 포지션, 슬리피지 우려 | **Limit** or **BBO** |
| SL이라서 반드시 체결되어야 함 | **Market** (가장 안전) |

## Market vs Limit vs BBO 비교

|  | Market | Limit | BBO |
|---|---|---|---|
| 사용자 입력 | Trigger만 | Trigger + Limit | Trigger만 |
| 트리거 후 주문 | 시장가 | 지정가 | 지정가 (최우선 호가) |
| 체결 확률 | 거의 100% | 보장 안 됨 | 매우 높음 |
| 슬리피지 | 있음 | 없음 | 거의 없음 |
| 수수료 | 테이커 | 메이커 가능 | 메이커 가능 |
| 부분 체결 | 없음 | 가능 | 가능 |
