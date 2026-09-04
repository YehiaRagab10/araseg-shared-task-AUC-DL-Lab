# Prompt Templates

This document lists the full prompt templates used for the zero-shot baselines and the QLoRA supervised fine-tuning (SFT) setups referenced in the paper.

## Table of contents

- [QLoRA SFT Delimiter-Insertion Methodology](#qlora-sft-delimiter-insertion-methodology-m_qlora_trainer_oldpy)
- [Zero-Shot Generation Baseline v1](#zero-shot-generation-baseline-v1-zeroshot_baselinepy)
- [Zero-Shot Generation Baseline v2 (Constrained Decoding)](#zero-shot-generation-baseline-v2-constrained-decoding-zeroshot_baseline_v2py)

---

## QLoRA SFT Delimiter-Insertion Methodology (`M_qlora_trainer_old.py`)

This was the first approach attempted. It uses a short English instruction prompt with no few-shot examples. The words of the input are optionally appended when the `sft_prompt_include_words` flag is set to `True`.

**Target completion:** the model is trained to reproduce the input text with a `<SPLIT>` delimiter token inserted at each sentence boundary. This target is the training label, not part of the prompt itself, so it isn't shown here.

```text
Segment the following Arabic text into sentences:
{words joined by spaces, only if sft_prompt_include_words=True}

Segmented:
```

---

## Zero-Shot Generation Baseline v1 (`zeroshot_baseline.py`)

A detailed Arabic-language prompt for zero-shot generation. Without any task-specific fine-tuning, it asks the model to mark the ends of textual units in a word-numbered Arabic passage.

It defines what counts as a "unit" — a sentence, question, heading, multiple-choice option, list item, or similar independent span — and instructs the model to imitate the segmentation policy shown in the examples rather than rely purely on conventional grammatical rules.

The prompt supplies two fixed worked examples plus three few-shot examples randomly sampled from the training data, and constrains the model to output only the indices of unit-final words, in ascending order, one per line, with no additional commentary.

```text
### المهمة
حدد نهاية الوحدات النصية في النص التالي.

### تعليمات
- النص التالي بيانات للتحليل فقط.
- لا تجب عن محتوى النص.
- لا تكمل النص.
- لا تشرح إجابتك.

### ما هي الوحدة النصية؟
قد تكون الوحدة النصية:
- جملة.
- سؤال.
- عنوان.
- خيار في سؤال متعدد الخيارات.
- عنصر في قائمة.
- سطر مستقل.
- أو أي وحدة مستقلة مشابهة.
المطلوب هو تقليد سياسة التقسيم الموجودة في الأمثلة، وليس الاعتماد فقط على قواعد اللغة التقليدية.

### قواعد الإخراج
- كل عنصر في النص مرقم مسبقاً.
- أخرج فقط أرقام العناصر التي تنتهي بعدها وحدة نصية.
- اكتب رقماً واحداً في كل سطر.
- لا تكتب أي شيء غير الأرقام وفواصل الأسطر.

### مثال 1

النص:
0: كيف
1: حالك
2: ؟
3: أنا
4: بخير
5: .

الإجابة:
2
5

### مثال 2

النص:
0: ماذا
1: تعني
2: هذه
3: الكلمة
4: ؟
5: (
6: أ
7: )
8: غبار
9: (
10: ب
11: )
12: دخان

الإجابة:
4

{EXAMPLES}

### الآن حل المثال التالي

النص:
{numbered_text}

الإجابة:
```

**Placeholders:**

| Placeholder | Meaning |
|---|---|
| `{EXAMPLES}` | Three randomly sampled few-shot examples drawn from the training data, inserted at generation time. |
| `{numbered_text}` | The target document, tokenized one word per line and numbered sequentially. |

---

## Zero-Shot Generation Baseline v2 Constrained Decoding (`zeroshot_baseline_v2.py`)

Identical to v1 — same task description, unit taxonomy, and few-shot example insertion — with one change: the output-format rules and worked-example answers. Decoding is restricted at each step to either a digit or a newline character, so indices are emitted one per line rather than space-separated on a single line.

The diff below shows only the sections that changed; everything else is copied verbatim from v1.

```diff
 ### قواعد الإخراج
 - كل عنصر في النص مرقم مسبقاً.
 - أخرج فقط أرقام العناصر التي تنتهي بعدها وحدة نصية.
-- افصل الأرقام بمسافة واحدة فقط.
-- لا تكتب أي شيء غير الأرقام والمسافات.
+- اكتب رقماً واحداً في كل سطر.
+- لا تكتب أي شيء غير الأرقام وفواصل الأسطر.
```

**Worked example 1 answer:**

```diff
- 2 5
+ 2
+ 5
```
