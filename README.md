
Claude finished the response

Qwen25VL_Drone_Detection_Eval.ipynb
ipynb

just add different type of prompting this zero few shot and chain of thought and give me the full file . user promp and system prompt use be different


Architected modular functions for systematic XML processing and metric computation
Architected modular functions for systematic XML processing and metric computation
Now let's update the final report cell (20/21) to produce a combined per-prompt-type CSV and comparison summary.


Update final report cell to compare all three prompting strategies
Update final report cell to compare all three prompting strategies
Now let's validate the notebook JSON is well-formed and syntactically valid Python.


Identified syntax errors and recalibrated quoting approach
Identified syntax errors and recalibrated quoting approach
Good — cell 1's "error" is just the !pip install shell-magic line, which is expected and pre-existing (not valid standalone Python, but valid Jupyter syntax). All my edits are now syntactically clean. Let's copy the final notebook to outputs.


Ran a command, read a file
Ran a command, read a file
Here's what changed in the notebook:

Cell 4 — now defines build_system_prompt() and build_user_prompt() with separate system and user prompts for each of the three strategies:

Zero-shot: plain expert-persona system prompt, task instructions with no examples.
Few-shot: system prompt tells the model examples will follow; user prompt includes 2 worked examples (drone present / absent) before the real task.
Chain-of-thought: system prompt instructs step-by-step visual reasoning (shape, contrast, motion blur, size); user prompt asks for brief reasoning followed by the final YES/NO + box on the last line.
Cell 8 (inference loop) — now loops over all three PROMPT_TYPES, saving predictions to separate folders (qwen25vl_predictions_xml_zero_shot, ..._few_shot, ..._chain_of_thought) so results never mix.

Cell 9 & 10 — the comparison and metrics logic is refactored into reusable functions (build_comparisons(), compute_metrics()) and run once per prompting strategy.

Cell 11 — saves a CSV per strategy + a combined CSV, and writes a single summary report comparing TP/FP/FN/TN, IoU, precision, recall, F1, and mAP@0.5 across all three strategies side by side, plus a summary_table DataFrame shown in-notebook.

Everything else (Ollama connection, image loading, XML saving, IoU math) is untouched. One thing to check: I used your Windows paths as-is (C:\Users\HP\...), so the new per-strategy folders will sit alongside your existing qwen25vl_predictions_xml folder — let me know if you'd rather have them somewhere else.


Qwen25vl drone detection eval
IPYNB 
--------------------------------------------------------------------------- NameError                                 Traceback (most recent call last) Cell In[5], [line 36](vscode-notebook-cell:?execution_count=5&line=36)      32     xmax = max(0, min(xmax, img_w))      33     ymax = max(0, min(ymax, img_h))      34     return (int(xmin), int(ymin), int(xmax), int(ymax))      35  ---> [36](vscode-notebook-cell:?execution_count=5&line=36) has_drone, pred_box = parse_response(raw_response, img.width, img.height)      37 print("Drone detected:", has_drone)      38 print("Predicted box:", pred_box)  NameError: name 'raw_response' is not defined


Identified missing variable assignment from refactoring oversight
Identified missing variable assignment from refactoring oversight
Good catch — my replacement of Cell 4 removed the raw_response variable that Cell 5 (parse_response demo) depends on. Let me fix that.


Ran 2 commands
Ran 2 commands
give m efull reDME FILE FOR THIS


Ran a command, created a file, read a file
Ran a command, created a file, read a file
Here's the full README for the notebook — covers what it does, the three prompting strategies (with system/user prompt breakdown), setup requirements, expected folder structure, run instructions, metrics definitions, and known limitations. Let me know if you want the paths/folder names adjusted to match your actual project layout, or if you'd like a broader README covering the whole internship pipeline instead of just this notebook.


Readme
Document · MD 





Claude is AI and can make mistakes. Please double-check responses.


Readme · MD
Qwen2.5-VL Small Drone Detection Evaluation
This project evaluates Qwen2.5-VL-7B (served locally via Ollama) on the task of detecting small, distant UAVs/drones in still images, using three different prompting strategies: zero-shot, few-shot, and chain-of-thought (CoT). Predictions are compared against Pascal VOC ground-truth annotations and scored with standard detection metrics (IoU, Precision, Recall, F1, mAP@0.5).

It is part of a broader internship study comparing general-purpose Vision-Language Models against a domain-specific, fine-tuned YOLOv11 detector for small-object UAV detection.

1. What this notebook does
Connects to a local Ollama server and confirms the qwen2.5vl:7b model is available.
Loads a set of drone/no-drone images and their Pascal VOC ground-truth XML annotations.
For each of the three prompting strategies, sends every image to Qwen2.5-VL with a strategy-specific system prompt and user prompt, and parses the response into:
has_drone (YES/NO), and
a bounding box (xmin, ymin, xmax, ymax) in pixel coordinates, if a drone is detected.
Saves each prediction as a Pascal VOC XML file, in a folder dedicated to that strategy.
Compares predictions against ground truth per strategy, computing:
Mean IoU, Precision, Recall, F1-score, mAP@0.5, and a TP/FP/FN/TN breakdown.
Exports per-strategy and combined CSVs, plus a single text summary report comparing zero-shot vs. few-shot vs. chain-of-thought side by side.
2. Prompting strategies
Strategy	System prompt	User prompt
Zero-shot	Expert drone-detector persona, told to decide with no prior examples.	Task instructions + required output format only — no examples given.
Few-shot	Same persona, plus a note that worked examples will precede the real task.	Two worked examples (one drone-present, one drone-absent) demonstrating the exact input/output format, followed by the real image.
Chain-of-thought (CoT)	Same persona, instructed to reason step by step (shape, contrast with background, motion blur, relative size) before answering.	Instructions to briefly reason (2–3 sentences), then give the final YES/NO + bounding box as the last line of the response.
All three strategies share the same strict output contract so responses can be parsed consistently:

YES
xmin,ymin,xmax,ymax
or

NO
3. Requirements
Python 3.9+
Ollama installed and running locally, with the model pulled:
bash
  ollama pull qwen2.5vl:7b
Python packages (installed by the first notebook cell):
bash
  pip install -q ollama opencv-python pillow lxml pandas numpy matplotlib tqdm
4. Folder structure expected by the notebook
Update the paths in Cell 3 to match your machine. By default the notebook expects:

data/detection_drone/
├── img/                        # input images (.jpg/.png)
└── xml/                        # ground-truth Pascal VOC annotations (.xml)
The notebook creates the following outputs automatically (alongside the folder set as PRED_DIR):

qwen25vl_predictions_xml_zero_shot/         # predicted VOC XMLs, zero-shot
qwen25vl_predictions_xml_few_shot/          # predicted VOC XMLs, few-shot
qwen25vl_predictions_xml_chain_of_thought/  # predicted VOC XMLs, chain-of-thought

qwen25vl_drone_eval_results_zero_shot.csv
qwen25vl_drone_eval_results_few_shot.csv
qwen25vl_drone_eval_results_chain_of_thought.csv
qwen25vl_drone_eval_results_all_prompts.csv     # combined per-image results

qwen25vl_drone_eval_summary.txt                 # combined metrics summary, all strategies
5. How to run
Start Ollama and confirm the model is pulled (ollama list).
Open Qwen25VL_Drone_Detection_Eval.ipynb in Jupyter / VS Code.
Update IMAGES_DIR, ANNOTS_DIR, and PRED_DIR in Cell 3 to point at your data.
Run all cells top to bottom. Cell 4 runs a quick sanity check on one image for all three strategies; Cell 8 runs full inference (zero-shot → few-shot → chain-of-thought, in that order) over the entire image set — this is the slowest step.
Metrics for each strategy print inline (Cell 10), and the final cell (Cell 11) writes all CSVs and the combined summary report to disk.
6. Metrics reference
IoU (Intersection over Union) — overlap between predicted and ground-truth boxes.
IoU threshold — 0.5 (a prediction counts as a true positive only if IoU ≥ 0.5 against a ground-truth box that also contains a drone).
Precision — of all predicted drone detections, the fraction that were correct.
Recall — of all actual drones, the fraction that were successfully detected.
F1-score — harmonic mean of precision and recall.
mAP@0.5 — average precision at IoU ≥ 0.5, computed via 11-point interpolation, using IoU as a ranking proxy since Qwen2.5-VL does not output a confidence score.
7. Notes & known limitations
Qwen2.5-VL is queried at temperature=0.0 for reproducibility.
If ground truth has no XML file for an image, it is treated as a "no object" ground truth (i.e., the image is assumed to contain no drone).
A predicted box with IoU below the 0.5 threshold against its ground-truth box is counted as both a false positive and a false negative (it neither correctly localizes the object nor correctly reports "no object").
Response parsing expects the model's final answer to appear as the last line of its output — this matters most for the chain-of-thought strategy, where reasoning text precedes the answer.
8. Related work
This evaluation is part of a larger study benchmarking general-purpose VLMs (Qwen2.5-VL, Qwen3-VL, Gemma3-4B, LLaVA-7B, Moondream) against a fine-tuned YOLOv11 detector (P2 detection head + SAHI tiled inference) and prior transformer-based UAV detectors such as TransVisDrone, on the DUT Anti-UAV and NPS UAV Tracking datasets.




