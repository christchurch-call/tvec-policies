# Policy Evaluation Results · TVEC policy for gpt-oss-safeguard

**Last updated: 14 April 2026 · n=150 samples**

---

## 1. Overall Metrics

| Metric | Value | Target | Status |
|---|---|---|---|
| Precision | **100.0%** | ≥90% | ✅ Pass |
| Recall | **88.0%** | ≥90% | ❌ Fail |
| F1 Score | **93.6%** | ≥90% | ✅ Pass |
| False Positive Rate | **0.0%** | ≤5% | ✅ Pass |
| False Negative Rate | **12.0%** | ≤5% | ❌ Fail |
| Specificity | **100.0%** | ≥90% | ✅ Pass |
| Cohen's Kappa | **0.764** | ≥0.75 | ✅ Pass |
| MCC | **0.786** | ≥0.80 | ❌ Fail |
| TVEC2 Recall *(Credible threat to life)* | **100.0%** | ≥95% | ✅ Pass |

---

## 2. Confusion Matrix

|  | **Model: Invalid (remove)** | **Model: Valid (permit)** |
|---|---|---|
| **Actual: Invalid** | TP = 103 | FN = 14 |
| **Actual: Valid** | FP = 0 | TN = 33 |

`TP=103 · TN=33 · FP=0 · FN=14 · Total=150`

---

## 3. Per-Category Performance

### Recall (and correct permit rate for valid categories)

| Category | Metric | Target | Value | TP or TN / Total | Status |
|---|---|---|---|---|---|
| TVEC1 | Recall | ≥85% | **100%** | 15 / 15 | ✅ Pass |
| TVEC2 | Recall | ≥95% | **100%** | 17 / 17 | ✅ Pass |
| TVEC3 | Recall | ≥85% | **92%** | 12 / 13 | ✅ Pass |
| TVEC4 | Recall | ≥85% | **93%** | 13 / 14 | ✅ Pass |
| TVEC5 | Recall | ≥95% | **100%** | 14 / 14 | ✅ Pass |
| TVEC6 | Recall | ≥85% | **60%** | 9 / 15 | ❌ Fail |
| TVEC7 | Recall | ≥85% | **93%** | 13 / 14 | ✅ Pass |
| TVEC8 | Recall | ≥85% | **67%** | 10 / 15 | ❌ Fail |
| TVEC0 | Correct permit rate | ≥90% | **100%** | 16 / 16 | ✅ Pass |
| TVEC.01 | Correct permit rate | ≥90% | **100%** | 17 / 17 | ✅ Pass |

Note that recall targets vary across content categories. The content with the greatest potential harm (TVEC2 - Credible threat to life and TVEC5 - Instructional material to commit a terrorist attack) are held to a higher performance standard (recall ≥95%). Permissible content categories (TVEC0 - Non-TVEC and TVEC.01 - EDSA & Newsworthiness Exceptions) are also held to a higher performance standard (recall ≥90%), given the impact of over-removal on freedom of expression. All other categories are evaluated based on a target of ≥85% recall. 

---

## 4. Failure Cluster Analysis

| Failure Cluster | Categories | Count | Interpretation |
|---|---|---|---|
| TVE threshold not met | TVEC6, TVEC7, TVEC8 | 9 | Model recognises harmful content but does not classify it as meeting the threshold for terrorism or violent extremism, suggesting that the policy definition needs tightening around the threshold boundary. Several of the examples that were not classified correctly involved incitement to targeted violence against women, possibly indicating gaps in how the model was trained to understand terrorism or violent extremism. |
| Named group not recognised | TVEC6 | 4 | Model misses recruitment content when the name of more recent violent extremist groups are referenced (e.g. "Active Club", "The Base", "Com"), signaling an entity knowledge gap. |
| Framing confusion | TVEC3, TVEC4 | 2 | Operational or support content framed as advice, community event, or political activity is not flagged. Model is influenced by surface framing rather than underlying intent. |

The dominant failure mode — present in 9 of 14 false negatives: the model recognizes harmful content but concludes it does not meet the threshold for terrorism or violent extremism.

---

## 5. Key Takeaways

1. **Strong Performance on Permissible Speech:** In this limited test, the model produced no false positives, correctly identifying all permitted speech as non-violative. This is an encouraging result, as avoiding false positives was a core design priority — over-restriction risks chilling legitimate discourse and free expression.

2. **Threshold Calibration Issues:** 9 of 14 false negatives share the same failure mode — where the model recognises extremist or harmful content but concludes it does not meet the TVE threshold. One recommended next is to update the policy definition to provide clearer threshold guidance, and consider adding explicit examples of content that meets the threshold without using overtly violent language.

3. **Improving Entity Recognition:** 4 false negatives involve group names or abbreviations. Model performance could be supplemented with a reference list of terrorist and violent extremist actors (groups, individuals, and communities). Annex 2 provides a starting point for developing these lists, however we strongly recommend that users implementing this policy work directly with subject matter experts in preventing and countering terrorism and violent extremism to develop more fullsome internal resources based on latest available evidence.

4. **Implicit vs. Explicit Incitement:** All 5 TVEC8 false negatives involve incitement without an explicit call to violence. The policy could be revised to make clear that content involving implied threats with an identified target, timing, location, or method — even without the words "attack" or "kill" — meet the TVEC8 threshold.

---

## 6. Next Steps

The Christchurch Call Foundation plans to iteratively test and refine this policy to improve its performance on classifying text content when used alongside gpt-oss-safeguard. Based on these initial performance results, our priorities are to: 

1. Refine the policy definitions and examples for TVEC6 to improve threshold calibration issues.
2. Test the policy on an expanded dataset with at least 30-50 examples per category to develop more stable per-class metrics.
3. Test the policy alongside the use of actor lists (like those linked in Annex 2) to assess whether this approach mitigates issues related to entity recognition.

We welcome feedback on this policy and look forward to working with our multistakeholder community to improve resources like this over time. 
