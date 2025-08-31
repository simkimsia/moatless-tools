# TODO

## 2025-08-31 Sunday

- finish running verify setup step in README.md for qwen3-coder in lm studio
- alter `run_evaluation.py` to make it work for qwen3-coder in lm studio
    ```python
    python3 scripts/run_evaluation.py \
        --litellm-model-name openai/qwen3-coder-30b \
        --model-base-url http://host.docker.internal:1234/v1 \
        --model-api-key faux_key \
        --flow swebench_react \
        --dataset-split verified_mini \
        --evaluation-name qwen3_coder_eval_20250831 \
        --num-parallel-jobs 1
    ```
  Key differences from docker_run.py:

  1. Multiple instances: Runs on entire dataset splits (like
  verified_mini with 50 instances)
  1. Parallel jobs: Can run multiple instances concurrently with
  --num-parallel-jobs
  1. Progress tracking: Shows real-time progress with instance status
  counts
  1. Final statistics: Automatically calculates and displays resolution
  percentage

  The script will show:

  - Instance status summary during execution
  - Final completion message
  - Resolution rate percentage (e.g., "45.2% (23/50 instances
  resolved)")

  Available dataset splits:

  - verified_mini: 50 instances (recommended for testing)
  - verified: 500 instances
  - lite: 300 instances
  - lite_and_verified_solvable: 84 instances

## call graph

 created swebench_react_with_callgraph.json which is
  identical to the original swebench_react.json flow but with the
  CallGraphAction added to the actions list.

  Key differences from the original:
  1. Added CallGraphAction in the actions list
  2. Updated system prompt to mention CallGraph as one of the search
  tools available
  3. Added guidance to use CallGraph for understanding function
  relationships and dependencies

  Now you can run your qwen3-coder evaluations with:

  # With call graph tool
  python scripts/run_evaluation.py --dataset-split your_dataset --flow
  swebench_react_with_callgraph

  # Without call graph tool (baseline)
  python scripts/run_evaluation.py --dataset-split your_dataset --flow
  swebench_react

  This gives you a perfect A/B comparison using the same ReACT format
  and configuration that qwen3-coder works well with, with the only
  difference being the availability of the call graph analysis tool.