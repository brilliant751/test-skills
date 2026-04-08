# Schema and Gate Profile (v1)

本文件定义 `progressive-test-integration` 的双层输出基线：
- Section A: `structured_result`（JSON）
- Section B: `review_report`（自然语言）

`review_report` 只能渲染 `structured_result` 已存在的事实，不得新增结论。

## Structured Result JSON (minimum contract)

```json
{
  "meta": {
    "schema_version": "1.0",
    "skill_version": "1.1",
    "generated_at": "2026-04-05T10:00:00Z",
    "evidence_sources": ["ac_doc", "diff_summary", "repo_readme"],
    "using_default_profile": true,
    "models": {
      "primary": "gpt-5.4",
      "fallback": "gpt-5.4-mini"
    },
    "prompt_artifacts": [
      "prompts/system-v1.md",
      "prompts/incremental-order-pr42-v2.md"
    ]
  },
  "change_summary": {
    "changed_components": ["order_service", "payment_adapter"],
    "risk_tiers": ["high", "medium"]
  },
  "impact_matrix": [
    {
      "diff_item": "order_service.placeOrder",
      "capability": "order placement with payment",
      "affected_tests": ["ORD-101", "PAY-204"],
      "risk_type": "consistency"
    }
  ],
  "incremental_decisions": [
    {
      "action": "UPDATE",
      "test_id": "ORD-101",
      "reason": "transaction rollback behavior changed",
      "evidence_ref": ["diff:order_service", "ac:rollback"]
    }
  ],
  "defects": [
    {
      "id": "BUG-12",
      "severity": "P1",
      "status": "open",
      "reproducible": true,
      "evidence_ref": ["log:trace_223", "test:ORD-101"]
    }
  ],
  "coverage_metrics": {
    "ac_coverage": 0.92,
    "risk_coverage": 0.85,
    "blocked_tests": 1
  },
  "analysis_metrics": {
    "accuracy": {
      "impact_precision": 0.83,
      "defect_precision": 0.79,
      "validation_sample_size": 38
    },
    "coverage": {
      "ac_coverage": 0.92,
      "risk_coverage": 0.85,
      "critical_path_coverage": 0.94
    },
    "generalization": {
      "projects_tested": 4,
      "pass_rate_by_stack": ["Go:0.91", "Java:0.86", "Python:0.90"]
    }
  },
  "test_debt": [
    {
      "item": "timeout-retry scenario not automated",
      "risk_level": "medium",
      "owner": "backend-qa",
      "eta": "2026-04-12",
      "blocking_release": false
    }
  ],
  "assumptions": ["staging DB behavior is production-like"],
  "unknowns": ["payment sandbox timeout threshold not confirmed"],
  "blockers": [],
  "gate_evaluation": {
    "profile_id": "default-profile-v1",
    "rules_hit": ["0.85 <= ac_coverage < 0.95 => GO_WITH_CONDITIONS"],
    "final_decision": "GO_WITH_CONDITIONS",
    "rationale": "Coverage is acceptable but below strict-go threshold."
  }
}
```

## Default Quantitative Gate Profile

默认门禁（无需用户配置）：
- `P0 open > 0 => NO_GO`
- `P1 open > 3 => NO_GO`
- `ac_coverage < 0.85 => NO_GO`
- `0.85 <= ac_coverage < 0.95 => GO_WITH_CONDITIONS`
- `risk_coverage < 0.80 => GO_WITH_CONDITIONS`
- `blocked_tests > 0` 且命中核心链路 => `NO_GO`，否则 `GO_WITH_CONDITIONS`
- 其余满足 => `GO`

## Gate Overrides (optional)

用户可选提供 `gate_overrides`：
- 仅允许覆盖阈值参数，不允许删除字段或改变 schema 结构。
- 使用覆盖时，输出：
  - `meta.using_default_profile = false`
  - `gate_evaluation.profile_id = "custom"`
- 未提供覆盖时，输出：
  - `meta.using_default_profile = true`
  - `gate_evaluation.profile_id = "default-profile-v1"`

## Decision Trace in Review Report

`review_report` 的最后必须包含 3-5 行 `Decision Trace`：
- 已命中的门禁规则（按优先级展示）。
- 对最终判定的直接解释。
- 若存在未知项/阻塞项，说明其对判定的影响。
