# Deep-Research-Agent
A modular multi-agent Deep Research system built with the OpenAI Agents SDK. It autonomously plans research, performs parallel web searches, synthesizes a detailed report, and delivers it via email, all orchestrated through a clean async manager and deployed with a Gradio interface.

## Overview

Deep-Research-Agent takes a user query and executes a full research pipeline:

Plans what needs to be researched

Runs multiple web searches in parallel

Synthesizes results into a detailed report

Sends the report via email

Streams live progress updates through a Gradio interface

Each responsibility is handled by a specialized agent.

## Project Structure
planner_agent.py

search_agent.py

writer_agent.py

email_agent.py

research_manager.py

deep_research.py

## Architecture

The system follows a layered design:

UI Layer → Orchestration Layer → Specialized Agents → Tools

Each component has a single responsibility, making the system modular and extensible.

## Agents and Their Responsibilities
### 1. planner_agent.py
Role: Research Strategy Planner

The planner agent decides what to search.

Responsibilities:

Takes the original user query

Breaks it into multiple targeted web searches

Explains why each search is necessary

Produces structured search instructions using Pydantic

It does not perform searches.
It only creates the strategy.

### 2. search_agent.py
Role: Web Research Executor

The search agent gathers information from the web.

Responsibilities:

Receives a search term and reasoning

Forces web tool usage

Extracts relevant findings

Produces concise, grounded summaries

It focuses only on collecting and summarizing information.
It does not combine results across searches.

### 3. writer_agent.py
Role: Report Synthesizer

The writer agent turns research findings into a cohesive report.

Responsibilities:

Receives the original query

Receives all search summaries

Produces:

A short executive summary

A detailed markdown research report

Suggested follow-up research questions

This agent handles structure, clarity, and analytical synthesis.

### 4. email_agent.py
Role: Report Delivery Agent

The email agent handles communication.

Responsibilities:

Converts markdown into formatted HTML

Generates a subject line

Sends the final report using SendGrid

This keeps delivery logic separate from research logic.

### 5. research_manager.py
Role: Orchestration Engine

This is the core of the system.

Responsibilities:

Coordinates all agents

Manages async execution

Runs searches in parallel using asyncio

Streams progress updates

Validates structured outputs

Handles error control

Generates OpenAI trace IDs

#### Execution Flow:

User Query
→ Planner Agent
→ Parallel Search Agents
→ Writer Agent
→ Email Agent
→ Final Report

### 6. deep_research.py
Role: User Interface Layer

This file connects users to the research system.

Built using Gradio.

Responsibilities:

Accepts user query

Streams status updates in real time

Displays the final report

Calls the ResearchManager asynchronously

No business logic lives here — only UI interaction.

Async & Parallel Execution

The system uses:

asyncio.create_task()

asyncio.as_completed()

This allows:

Multiple web searches to run concurrently

Faster research execution

Real-time progress updates

## Technologies Used

Python 3.12+

OpenAI Agents SDK

WebSearchTool

Pydantic (Structured Outputs)

asyncio (parallel processing)

SendGrid (email delivery)

Gradio (UI layer)

## Observability

Each run generates an OpenAI trace.

This allows:

Tool call inspection

Debugging

Performance analysis

Full transparency into agent execution

## Why This Project Matters

Most research tools are simple LLM wrappers.

This project demonstrates:

True multi-agent decomposition

Structured outputs for reliability

Forced tool usage for grounded research

Parallel execution

Autonomous task completion

Clean separation of concerns

It moves beyond chatbots into real agent systems.
