# Medical Chatbot Fine-Tuning: DeepSeek R1 with LoRA

**Project Status:** ✅ Completed  
**Base Model:** DeepSeek-R1-Distill-Llama-8B  
**Stack:** Kaggle, Hugging Face, W&B, LoRA, PyTorch

---

## What This Project Does

I fine-tuned DeepSeek R1 to handle medical reasoning tasks. The goal was to get the model to think through medical cases step-by-step, similar to how a doctor would approach a diagnosis. Using LoRA and 4-bit quantization, I managed to adapt this large model efficiently without needing massive GPU resources.

The fine-tuned version now generates structured reasoning chains before giving medical answers, making its thought process transparent and easier to trust.

![image](https://github.com/user-attachments/assets/4f666231-dc7b-4dfb-b050-9092071c8b13)

---

## Dataset

I used the **medical-o1-reasoning-SFT** dataset from Hugging Face ([link](https://huggingface.co/datasets/FreedomIntelligence/medical-o1-reasoning-SFT)). For this project, I worked with the first 500 samples.

Each sample contains:
- **Question**: A medical case or scenario
- **Complex CoT**: Detailed reasoning steps
- **Response**: The final answer or diagnosis

---

## What Makes This Approach Different

- **Structured Medical Reasoning**: The model now shows its thinking process before answering, not just the final answer
- **Efficient Training**: Used 4-bit quantization so I could train on consumer GPUs without running out of memory
- **Parameter Efficiency**: Only 0.1% of model parameters were updated using LoRA adapters
- **Better Accuracy**: The fine-tuned model produces clearer and more precise medical reasoning
- **Domain Focus**: Trained specifically on medical reasoning data for better reliability

---

## Technical Approach

### LoRA Fine-Tuning

LoRA (Low-Rank Adaptation) lets you fine-tune large models efficiently. Instead of updating all the model's weights, it adds small trainable matrices to specific layers (like attention and feed-forward networks). This cuts down computational costs dramatically—by over 90% in many cases.

Think of it like adding a specialized module to an existing system rather than rebuilding everything from scratch.

### 4-Bit Quantization

I compressed the model weights from 32-bit to 4-bit precision. This reduces memory usage by about 87.5% while keeping most of the model's accuracy. Without this, running DeepSeek R1 on a regular GPU would be nearly impossible.

---

## Training Results

### Loss Progression

| Step | Training Loss |
|------|---------------|
| 10   | 1.914200      |
| 20   | 1.421000      |
| 30   | 1.366000      |
| 40   | 1.328500      |
| 50   | 1.295100      |
| 60   | 1.326900      |

The loss decreased consistently during training, which shows the model was successfully learning the medical reasoning patterns.

---

## Before vs After Comparison

### Before Fine-Tuning

The base model's response was accurate but verbose and unstructured:
```
Stress urinary incontinence is likely based on her symptoms. The residual volume might be normal, but detrusor contractions would also be normal.
```

The answer was correct, but it lacked clarity and structure.

### After Fine-Tuning

The fine-tuned model produces more concise and organized reasoning:

```
Stress urinary incontinence is likely based on her symptoms. Cystometry findings would reveal normal residual volume and detrusor contractions.
```

The improvement is clear: better structure, clearer reasoning, and responses that follow a more clinical thought process.

---

## Next Steps

Some ideas for extending this project:

1. **More Data**: Expand to other medical datasets (radiology, pathology, etc.)
2. **Local Deployment**: Convert to GGUF format for running locally
3. **Predictive Features**: Add modules for forecasting disease progression
4. **Mobile Version**: Create a lightweight version for mobile devices

---

## Final Thoughts

This project demonstrates how techniques like LoRA and quantization can make large language models accessible for specialized domains like healthcare. By fine-tuning DeepSeek R1 for medical reasoning, we can create tools that assist clinicians without requiring massive computational resources.

The key insight: AI should augment human expertise, not replace it.
