# LLMs-and-OR

This repository contains a small end-to-end demo that combines **Large Language Models (LLMs)** with **Operations Research (OR)** for a lithium-ion battery recycling network.

The goal is to show how natural-language disruption descriptions (in Chinese) can be translated into structured optimization parameters using an LLM, and then solved with a Gurobi model.

---

## 1. Overview

- **Network**  
  - 5 recycling centers: `R1`–`R5`  
  - 8 demand nodes: `D1`–`D8`  
  - Fully connected network (every center can serve every demand node)

- **LLM component (OpenAI API)**  
  - Input: Chinese description of a disruption scenario, e.g.  
    > “未来 5 天，回收中心 R3 由于设备故障，产能下降到 40%。”  
  - Output (JSON):  
    - `center_id` (e.g. `"R3"`)  
    - `capacity_factor` (e.g. `0.4`)  
    - `duration_days`  
    - `summary_zh` (one-sentence Chinese summary)

- **OR component (Gurobi)**  
  - Decision variables: shipment quantity from each recycling center to each demand node  
  - Objective: minimize total transportation cost  
  - Constraints:  
    - all demand must be satisfied  
    - flows from each center must not exceed its (scenario-specific) capacity  
  - Scenarios:
    - **Baseline** (no disruption, base capacities)  
    - **Disrupted** (capacity of one center scaled by `capacity_factor` from the LLM)

---

## 2. Repository contents

- `LLM_OR_demo1.ipynb`  
  A Jupyter notebook that implements the full LLM + OR loop:

  1. Define the recycling network (centers, demand, capacities, costs)  
  2. Call the OpenAI API to parse disruption descriptions into JSON parameters  
  3. Solve the baseline and disrupted scenarios with Gurobi  
  4. Produce publication-style outputs:
     - **Table 1**: capacity and utilization by center (baseline vs disrupted)  
     - **Figure 1**: bar chart of utilization by center  
     - **Table 2**: total cost comparison (baseline vs disrupted)

More notebooks and experiments may be added later.

---

## 3. Requirements

- **Python** 3.9+  
- Python packages:
  - `openai`
  - `gurobipy`
  - `jupyterlab`
  - `pandas`
  - `matplotlib`

- **External requirements**
  - A valid **OpenAI API key**  
  - A working installation and license of **Gurobi** (academic license is sufficient)

---

## 4. Quick setup

You can either clone the repo or download it as a ZIP.

```bash
# clone the repository
git clone https://github.com/xuli-0204/LLMs-and-OR.git
cd LLMs-and-OR

# create and activate a virtual environment (Windows PowerShell)
python -m venv llm_or_env
.\llm_or_env\Scripts\activate

# install required packages
pip install openai gurobipy jupyterlab pandas matplotlib
