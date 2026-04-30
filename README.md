# AgriLabel-Agent
<p align="center">
  <img src="./asset/logo.png" alt="AgriLabel-Agent Logo" width="100%"/>
</p> 

**An autonomous visual annotation agent based on multimodal reinforcement learning**  
*Turning agricultural videos into instantly tradable data assets — fully Linux + CUDA, zero middleware, files as the system.*

[![License](https://img.shields.io/badge/license-Apache%202.0-blue)](LICENSE)
[![Linux](https://img.shields.io/badge/kernel-%3E%3D5.15-yellow)](https://kernel.org)
[![CUDA](https://img.shields.io/badge/CUDA-%3E%3D12.0-green)](https://developer.nvidia.com/cuda-toolkit)
[![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)](https://python.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0+-blue?logo=typescript)](https://www.typescriptlang.org/)
[![C++](https://img.shields.io/badge/C++-17-blue?logo=c%2B%2B)](https://isocpp.org/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)](CONTRIBUTING.md)

---

### 🚜 Why AgriLabel-Agent?

Modern agriculture generates massive volumes of drone footage and sensor video daily, yet extracting actionable insights still relies heavily on manual annotation—slow, delayed, and prohibitively expensive.

AgriLabel-Agent is an **AI annotation agent that runs autonomously on the Linux filesystem**. It completes a closed loop of perception, decision, execution, and continuous evolution. The agent comprises two collaborative parts:

- **Agent Core** — A kernel-native, high-concurrency autonomous labeling engine responsible for automatic perception, analysis, decision-making, and data asset packaging.
- **Agent Web** — A lightweight human-in-the-loop interaction layer that enables agricultural experts to review and inject knowledge in the most natural way, constantly enriching the agent's cognitive boundaries.

---

### 🤖 Agent Core — Autonomous Perception & Execution

The Core is the brain and hands of the agent, running directly on kernel capabilities and understanding every change in the filesystem:

- **Autonomous Perception**: Uses eBPF + fanotify to detect new videos without a single miss, proactively extracting spatio-temporal metadata and sensor parameters without any human command.
- **Proactive Decision-Making**: A built-in reinforcement learning policy engine dynamically adjusts frame sampling density, model scheduling, and active learning strategies based on throughput, model confidence, and human feedback.
- **Goal-Driven Execution**: Orchestrates YOLO (TensorRT), multimodal LLMs (vLLM/FasterTransformer), and NVDEC hardware decoders to run entirely within GPU memory with zero copy—targeting the single objective of maximizing high-quality data assets while minimizing human intervention.
- **Continuous Evolution**: Every interaction and annotation result is persisted as local experience and fine-tuning signals, making the models progressively more accurate and forming a self-reinforcing data flywheel.
- **Kernel-Level High Concurrency**: Exploits `io_uring` for batched asynchronous I/O, `dmabuf` for GPU memory sharing, `pidfd` for process management, and cgroup v2 for isolation—handling hundreds of concurrent video streams with minimal CPU overhead.
- **Files as the System, Zero Middleware**: Task queues, state machines, metadata, and RL experience buffers are all represented as standardized directories and atomic `rename()` operations. No databases, message queues, or external dependencies—crash-proof and edge-friendly.
- **Native Data Asset Output**: Every finished video automatically generates an AOM data package (`events.jsonl`, `segments/`, `provenance.json`, quality rating, digital signature), immediately ready for data trading spaces.

---

### 🌐 Agent Web — Human-in-the-Loop Collaboration

Agent Core is not a black box. Through a lightweight web interface, it transparently surfaces critical decision points for agricultural experts to review and correct, achieving the optimal mix of human intelligence and machine efficiency. Agent Web adheres to the same zero-middleware principle:

- **Filesystem-Native Design**: Reads states and contents directly from the `review/`, `done/`, and `staging/` directories. All interactions land back as files—no API layer or database required.
- **Audit & Review Workbench**: Presents low-confidence detection segments in timeline or map views. Experts can adjust bounding boxes, correct event types, and add comments directly in the browser. Modifications are instantly written as files, triggering Core to relearn.
- **Real-Time Insights Dashboard**: Using Server-Sent Events (SSE) and kernel fanotify events, it visualizes processing throughput, GPU utilization, model confidence distribution, RL decision history, and data asset generation speed—without any polling.
- **Ultra-Lightweight & Secure**: Embedded in the `agrilabel` binary, activated with the `--web` flag, listening only on a local Unix domain socket with filesystem-permission-based access and an optional simple token. No unnecessary ports ever exposed.
- **Human Knowledge Injection**: Every expert correction not only fixes the current annotation but is also recorded as a high-quality sample in the experience pool, continuously improving the agent's autonomous accuracy when facing similar agricultural scenarios and gradually reducing human dependency.
