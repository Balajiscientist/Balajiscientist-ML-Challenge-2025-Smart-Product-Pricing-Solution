<!-- Banner Image -->
<p align="center">
  <img src="https://github.com/Balajiscientist/Balajiscientist-ML-Challenge-2025-Smart-Product-Pricing-Solution/blob/main/README_BANNER.jpg?raw=true" 
       alt="ML Challenge 2025 Banner" 
       width="100%">
</p>


**Team Name:** Problem solving blood  

**Team Members:**  
- Boddu Balaji  
- Kunchala Yuva Shankar  
- Rajesh G  
- Surya Akhil Varma Kopperla  

**Submission Date:** 11-10-2025 

1. Loaded CSVs: train.csv, test.csv, sample_test.csv, sample_test_out.csv.
2. Limited training to first 1000 rows for fast experiments.
3. Downloaded product images to /balaji/train with multiprocessing + urllib.
4. Built chat-format dataset per row:
   - Instruction: output ONLY the price as a number in USD.
   - User content: instruction + catalog_content + image (RGB).
   - Assistant content: ground-truth price (with optional normalization).
5. Loaded base model: unsloth/Qwen2.5-VL-3B-Instruct-bnb-4bit (4-bit).
6. Enabled LoRA (RSLoRA) on vision and language layers (r=64, lora_alpha=128, dropout=0.05, bias=none).
7. Trained with SFTTrainer:
   - per_device_train_batch_size=1, gradient_accumulation_steps=8
   - max_steps=2000, warmup_steps=50, weight_decay=0.02
   - optim=adamw_8bit, scheduler=cosine_with_restarts, bf16=True
   - save_steps=500, eval_steps=500, save_total_limit=3
8. Saved model and tokenizer to /scientistbalaji/working/fine_tuned_model.
9. Implemented predict_price:
   - Cache/download image if missing; build chat from text+image
   - Try multiple decoding configs; parse numeric; use median of valid
   - Fallback value 10.0 if none valid
10. Generated predictions for sample_test.csv → /balaji/train/sample_test_out_pred.csv.
11. Evaluated vs sample_test_out.csv: SMAPE ~122.54%, MAE ~42.42, RMSE ~50.79.
