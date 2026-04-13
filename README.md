# voseo-asr-finetune

Fine-tuning Qwen3-ASR to recognize voseo verb forms in Spanish dialect.

## Problem

Standard ASR models handle general Spanish well but struggle with **voseo** (the use of *vos* instead of tú as the 2nd-person singular pronoun. The core difficulty is a subtle stress shift on verb endings:

| Form | Tuteo (tú) | Voseo (vos) |
|------|------------|-------------|
|hablar|  *hablas*  |  *hablás*   |
|comer |  *comes*   |  *comés*    |
|vivir |  *vives*   |  *vivís*    |

The forms of the -ar and -er verbs are near-identical acoustically. The must learn to write the correct accent mark based on speaker dialect.

## Approach

Fine-tune **Qwen3-ASR-1.7B** using LoRA on the LLM decoder, with the audio encoder frozen initially. Training data comes from the **PRESEEA corpus** (https://preseea.uah.es/).

## Pipeline

1. Identify interviews from PRESEEA corpus
2. Chunk audio by timestamps
3. Cut into 5-10 second segments
4. Align with WhisperX to get word-level timestamps
5. Build fine-tuning JSONL with voseo oversampling
6. LoRA fine-tune Qwen3-ASR-1.7B

## Expected Output

A fine-tuned model checkpoint that correctly transcribes voseo verb forms with proper accent marks, evaluated against held-out PRESEEA gold transcriptions.
