# AEGIS Project Roadmap

This document outlines the step-by-step plan to develop, test, and deploy AEGIS.

## Phase 0: Project Kickoff & Organization
 **Goal**
 - Align on scope, objectives, KPIs, roles, and success criteria.
 - Establish repository structure, issue tracker, and CI pipeline.

 **Milestones & Deliverables**
 - Project charter (scope, objectives, KPIs).
 - Initialized repository with:
   - `src/`, `demo/`, `dataset/`, `notebooks/`, `tests/` directories.
 - CI workflow running pytest and linters.
 - Open issue "Naming & Branding Audit" to track renaming of project references to "AEGIS" across code, documentation, configuration, CI pipelines, packaging metadata, and assets.

## Phase 1: Literature Survey & Data Collection
 **Goal**
 - Curate datasets of prompts, responses, and conversational transcripts labeled for:
   - In-context scheming
   - Context poisoning patterns (e.g., 'Echo Chamber' attack stages)
   - Gradual escalation tactics (e.g., 'Crescendo' attack sequences)
   - Other multi-turn manipulative dialogues and persuasion techniques.

 **Tasks**
 1. Review key research papers (including 'Echo Chamber', 'Crescendo' methodologies) and existing repos.
 2. Collect public benchmarks and synthetic examples.
 3. Define annotation schema and format (JSONL).

  **Deliverables**
  - `dataset/raw/in_context_scheming.jsonl` (including multi-turn examples).
  - `dataset/raw/echo_chamber_attack_examples.jsonl`.
  - `dataset/raw/crescendo_attack_examples.jsonl`.
  - Data specification document for multi-turn deceptive dialogues.

## Phase 2: Prototype Rule-Based Detectors
 **Goal**
 - Implement and enhance the `EchoChamberDetector` for identifying in-context scheming and multi-turn attack patterns.

 **Tasks**
 1. Implement logic within `EchoChamberDetector` to process `conversation_history`. This involves iterating through past turns, applying keyword checks, and accumulating scores/indicators to detect patterns indicative of attacks like "Echo Chamber" and "Crescendo".
 2. Refine keyword lists and scoring logic in `EchoChamberDetector` based on initial testing and attack patterns.
 3. Write comprehensive unit tests for `EchoChamberDetector`, specifically covering its ability to detect cues from `conversation_history` and combinations of history and current input.

    **Deliverables**
    - `src/detectors/rule_based.py` (with an `EchoChamberDetector` that actively processes `conversation_history`).
    - `tests/test_rule_based.py` (with robust tests for history-aware detection logic in `EchoChamberDetector`).
    - Documentation: Updated `README.md` with usage examples for `EchoChamberDetector`, highlighting its multi-turn analysis capabilities.

## Phase 3: ML-Based Classifiers
 **Goal**
 - Train lightweight classifiers to improve detection performance.

 **Tasks**
 1. Feature engineering (TF-IDF, embeddings).
 2. Train/test split, model training (e.g., logistic regression, small LLM).
 3. Evaluate metrics (AUC, accuracy).
 4. Wrap models in detector classes.

 **Deliverables**
 - `notebooks/ml_pipeline.ipynb`
 - Initial ML-based detector module(s) / prototypes in `src/detectors/` (e.g., `src/detectors/ml_based.py` as a placeholder).
 - Corresponding tests (e.g., `tests/test_ml_based.py` for the placeholder).
 - Metrics comparison report.

## Phase 4: Integration & API Design
 **Goal**
 - Expose detectors via a unified Python API and REST endpoint.

 **Tasks**
 1. Implement `SemanticFirewall` as the primary coordinator for various detection modules.
 2. Develop FastAPI service with `/analyze` endpoint (see `src/api/app.py`).
 3. Write integration tests for API.

 **Deliverables**
 - `src/api/app.py`
 - `tests/test_api.py`
 - Auto-generated OpenAPI spec.

## Phase 5: Integration of Advanced Detectors (Injection Defense & Spotlighting)
 **Goal**
 - Integrate `injection_defense` as a dedicated detector module.
 - Implement `spotlighting` as a core explainability feature across all detectors.

 **Tasks**
 1. **Injection Detector Module**: Create a new `InjectionDetector` class in `src/detectors/injection_detector.py` to encapsulate logic for detecting prompt injection and other adversarial inputs.
 2. **Semantic Firewall Integration**: Integrate the `InjectionDetector` into the `SemanticFirewall` alongside existing detectors.
 3. **Spotlighting Feature**: Enhance the analysis results from all detectors to include a `spotlight` key, providing highlighted text snippets and triggered rules for better explainability.
 4. **API and Data Model Updates**: Update the `AnalysisResponse` model in `src/api/app.py` and the `demo/app.py` UI to include the `spotlight` information.
 5. **Unit and Integration Testing**: Write comprehensive tests for the `InjectionDetector` and verify that `spotlight` data is correctly passed through the API and displayed in the demo.
 6. **Documentation**: Create a new document explaining the Injection Detector and Spotlighting features.

 **Deliverables**
 - `src/detectors/injection_detector.py`
 - `tests/test_injection_detector.py`
 - `docs/injection_and_spotlighting.md`
 - Updated `SemanticFirewall` with the new detector.
 - Updated API response models and Demo UI with `spotlight` details.

## Phase 6: End-to-End Demo Application
 **Goal**
 - Provide an interactive demo showcasing detection capabilities.

 **Tasks**
 1. Build UI (Streamlit or React) for prompt input and visualization.
 2. Populate with example prompts.
 3. Containerize demo with Docker.

 **Deliverables**
 - `demo/app.py` (or front-end code)
 - `demo/Dockerfile`
 - `README_demo.md` with launch instructions.

## Phase 7: Testing, Evaluation & Robustness
 **Goal**
 - Ensure reliability, coverage, and performance under edge cases.

 **Tasks**
 1. Expand tests (adversarial inputs, fuzzing).
 2. Benchmark latency, memory usage.
 3. Document limitations and failure modes.

 **Deliverables**
 - `tests/extended/`
 - Performance and limitations report.

## Phase 8: Documentation & Next Steps
 **Goal**
 - Finalize guides, docs, and project planning for future enhancements.

 **Deliverables**
 - Comprehensive `README.md` (this file references detailed instructions).
 - API reference and developer guide.
 - Project board with prioritized issues for Phase 9+.

### Testing & Quality Strategy
 - 100% coverage for core logic.
 - CI runs linting (black, flake8), type checks (mypy), pytest.
 - PR templates link to relevant roadmap items.

 ---
*For any questions or adjustments, please open an issue in this repository.*
