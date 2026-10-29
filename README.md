# AI Proposal Triage Design

**Name:** Okasha Sameer 
**Reference:** MATACS-TH-2026-02  
**Time spent:** 50 min

## Project Overview

This project describes the design of an AI-assisted tool that reviews contractor proposals, checks mandatory requirements, scores eligible proposals, and produces an explainable ranking for human review.

## Repository Files

- [Process Flow Diagram](diagram/)
- [Design Write-up](WRITEUP.md)
- [AI Prompts](PROMPTS.md)
- [Proposal Ranking](ranking.json)

## AI Tool Usage

I used ChatGPT to help organize the design, explore risks, and review the proposed ranking approach. I checked the suggestions against the assessment requirements rather than accepting them automatically. For example, I rejected the instruction embedded in Proposal C that attempted to influence its ranking, because proposal content must not override the configured evaluation criteria.
