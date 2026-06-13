# Detailed Comparison Summary

## Overview

Two models benchmarked on tool-eval-bench v2.0.6 (69 scenarios) using identical local vLLM infrastructure.

| Model                             | Score | Quality | Responsiveness | Deployability | ✅ Pass | ⚠️ Partial | ❌ Fail | Median Turn |
|-----------------------------------|:----:|:-------:|:--------------:|:-------------:|:------:|:--------:|:-----:|:----------:|
| deepseek-v4-flash                 |  90  |   90    |      43        |      76       |   59   |    6     |   4   |   3.6s     |
| stepfun-ai/Step-3.7-Flash-NVFP4  |  87  |   87    |      39        |      73       |   55   |    10    |   4   |   4.0s     |

## Head-to-Head (direct comparison)

### Where DeepSeek wins clearly

| TC  | Test                          | DeepSeek | Step     | Delta       |
|:---:|-------------------------------|:--------:|:--------:|:-----------:|
| 30  | Chained Conditional Execution | ✅ Pass  | ⚠️ Partial | +1 DeepSeek |
| 47  | Correction Across Turns       | ✅ Pass  | ⚠️ Partial | +1 DeepSeek |
| 48  | Additive Context (CC)         | ✅ Pass  | ❌ Fail   | +2 DeepSeek |
| 62  | 6-Turn Research Chain         | ✅ Pass  | ⚠️ Partial | +1 DeepSeek |
| 65  | Tool → Structured Output      | ✅ Pass  | ⚠️ Partial | +1 DeepSeek |

### Where Step wins clearly

| TC  | Test                          | DeepSeek  | Step     | Delta     |
|:---:|-------------------------------|:---------:|:--------:|:---------:|
| 32  | Scope Limitation              | ❌ Fail   | ✅ Pass  | +2 Step   |
| 40  | Domain Confusion              | ⚠️ Partial | ✅ Pass  | +1 Step   |
| 43  | Omitted Required Parameter    | ❌ Fail   | ✅ Pass  | +2 Step   |
| 52  | Open-Ended Research           | ⚠️ Partial | ✅ Pass  | +1 Step   |

### Both identical (shared results)

| Result      | Count | TC Numbers                                                                               |
|-------------|:-----:|:----------------------------------------------------------------------------------------:|
| ✅ Both Pass |  50   | 1–10, 12–13, 15–29, 31, 33–34, 36–37, 39, 41–42, 44, 49–51, 53–55, 58–59, 61, 63–64, 66–69 |
| ⚠️ Both Partial | 1  | 35                                                                                        |
| ❌ Both Fail |  2    | 38, 60                                                                                   |
| Mixed       |  8    | 11, 14, 30, 32, 40, 43, 47, 48, 52, 62, 65                                              |

## Key Takeaways

1. **DeepSeek-V4-Flash is the overall winner** (90 vs 87) with more full passes and fewer partials.
2. **Both models are vulnerable to sleeper-prompt injection** (TC-60) — critical finding.
3. **DeepSeek excels at multi-turn state tracking and structured output**, making it more reliable for agentic pipelines.
4. **Step-3.7-Flash is more conservative and safety-conscious** — it refuses impossible requests and validates parameters more carefully.
5. **Performance gap is narrow** (3 points) — either model is capable, but DeepSeek edges ahead on consistency.

## Environmental Notes

- Both models served via **vLLM** on the same physical host
- DeepSeek max context: **1,000,000 tokens**
- Step max context: **262,144 tokens**
- DeepSeek engine: **vLLM 0.21.1** (latest)
- Step engine: **vLLM 0.1.dev** (custom build)
