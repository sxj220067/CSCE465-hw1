# CSCE 465/765 Homework 1

This repository contains my work for Homework 1: Build and Threat-Model an AI Agent.

## Setup

Environment:
- Ubuntu 24.04 x86-64 VM
- Node.js 24.18.0
- OpenClaw 2026.7.1-2
- TAMUS AI model through the provided compatibility shim

Start the TAMUS shim:

```bash
export TAMU_API_KEY="YOUR_API_KEY"
node tamu-shim.mjs

open new terminal

openclaw daemon start
openclaw gateway status

test the agent:

openclaw agent --agent main -m "What is 2 + 2?"

Mainfiles
./bin/safe_marker.sh course-marker
* report.pdf — final report
* benign-tasks.md — Task 1.5
* injection-experiment.md — Task 3
* threat-model.md — Task 4 threat model
* advisory-analysis.md — Task 4 advisory analysis
* attack-surface-map.pdf — threat-model diagram
* evidence/ — raw transcripts, audit logs, and exec-policy output
* AI_USAGE.md — AI usage description
* versions.txt — software versions