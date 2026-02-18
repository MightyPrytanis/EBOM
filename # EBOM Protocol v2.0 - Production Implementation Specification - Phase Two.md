# EBOM Protocol v2.0 - Production Implementation Specification - Phase Two

## Future Enhancements

**Status**: DEFERRED - Not required for MVP
**Prerequisite**: EBOM v2.0 prototype fully functional and tested
**Date**: 2026-02-18

---

## Overview

This document contains features identified as valuable but NOT BLOCKING for initial prototype deployment. These features should be implemented AFTER v2.0 is stable, tested, and successfully deployed.

---

## Contents

1. User-Developed Requirements (GitHub Sections A-H)
2. Advanced Technical Features
3. Scope Expansion Items
4. Nice-to-Have Improvements

---

## Part 1: User-Developed Requirements

These sections were provided in GitHub Copilot feedback as "User-Developed Requirements" but are not part of the core v1.0 spec. They represent expanded vision for EBOM that should be integrated in Phase 2.

### Section A: Judgment Boundary Principle

**Status**: Conceptual framework, needs full specification
**Priority**: High (after MVP stable)

**Summary**: Establishes clear boundaries between different types of judgment (factual, professional, ethical, personal) and appropriate system responses for each.

**Implementation requirements**:
- Expand domain classifier to detect judgment type
- Create judgment-specific policies
- Develop refusal templates for inappropriate judgment requests
- Testing across judgment boundary scenarios

**Estimated effort**: 2-3 weeks

[FILE LINK]
---

### Section B: Crisis Intervention Framework

**Status**: High-risk, requires expert validation
**Priority**: High (legal/ethical review required)

**Summary**: Protocols for handling crisis situations (suicide risk, abuse, immediate danger) that override normal EBOM constraints to prioritize human safety.

**Implementation requirements**:
- Crisis detection keywords and ML classifier
- Immediate escalation protocols
- Resource database (hotlines, emergency services)
- Legal review of liability
- Ethical review board approval
- Testing with crisis intervention experts

**Estimated effort**: 6-8 weeks + external review time

**Blocking dependencies**: Legal counsel, ethics review, crisis expert consultation

[FILE LINK]
---

### Section C: Four-Sphere Topology Refinement

**Status**: Expansion of three-sphere model
**Priority**: Medium

**Summary**: Adds fourth sphere for "unknowable" or "fundamentally uncertain" knowledge that cannot be resolved by retrieval.

**Implementation requirements**:
- Expand sphere classification logic
- Add "unknowable" detection heuristics
- Update confidence scoring for unknowable claims
- Test edge cases

**Estimated effort**: 1-2 weeks

[FILE LINK]
---

### Section D: Liminal Navigation Framework

**Status**: Enhanced liminal sphere handling
**Priority**: Medium

**Summary**: More sophisticated handling of liminal sphere with graduated confidence, temporal tracking, and conflict management.

**Implementation requirements**:
- Implement graduated confidence for liminal claims
- Add temporal tracking for evolving liminal knowledge
- Enhanced contradiction handling for liminal conflicts
- Testing across time-sensitive scenarios

**Estimated effort**: 3-4 weeks

[FILE LINK]
---

### Section E: Sentience-Agnostic Framework

**Status**: Philosophical/ethical framework
**Priority**: Low (conceptual)

**Summary**: Establishes EBOM compliance regardless of whether system is "sentient" - focuses on behavior, not ontology.

**Implementation requirements**:
- Documentation update
- No code changes required
- Philosophical/ethical review

**Estimated effort**: Documentation only

[FILE LINK]
---

### Section F: Enforcement Reality

**Status**: Acknowledgment of limitations
**Priority**: Low (documentation)

**Summary**: Explicit acknowledgment that perfect enforcement is impossible; focuses on measurable compliance rates.

**Implementation requirements**:
- Update documentation with realistic expectations
- Add monitoring for enforcement failures
- Failure analysis and improvement loop

**Estimated effort**: 1 week (monitoring infrastructure)

[FILE LINK]
---

### Section G: Professional Mode Resolution

**Status**: User experience enhancement
**Priority**: Medium

**Summary**: Different interaction modes based on user expertise level and domain familiarity.

**Implementation requirements**:
- User profile system
- Mode switching logic
- Adjust output verbosity/technical level by mode
- Testing across user types

**Estimated effort**: 2-3 weeks

[FILE LINK]
---

### Section H: Integration Requirements

**Status**: Meta-requirement for Sections A-G
**Priority**: After individual sections implemented

**Summary**: Ensures all user-developed requirements work together coherently.

**Implementation requirements**:
- Integration testing across all Phase 2 features
- Conflict resolution between features
- Performance optimization
- Documentation update

**Estimated effort**: 2-3 weeks (after all features complete)

[FILE LINK]
---

## Part 2: Advanced Technical Features

### High-Stakes Diagnostic Refusal (HSDR)

**Source**: Gemini feedback
**Priority**: Medium
**Status**: UX enhancement

**Description**: More informative and actionable refusals for high-stakes queries when confidence is insufficient.

**Current behavior**: "Cannot provide verified answer. Confidence insufficient."

**Enhanced behavior**:
Cannot provide verified answer for this legal question.

Why: Only 1 source found (minimum: 2), credibility 0.6 (minimum: 0.7)

Sources searched: michigan.gov, legal databases, case law repositories

Recommendation: Consult Michigan-licensed attorney. If urgent, call State Bar
referral: [phone]

What you can do:

Rephrase question to be more specific

Provide additional context

Upload relevant documents


**Implementation**:
- Template system for refusal messages
- Diagnostic info from policy enforcer
- Domain-specific recommendations
- Resource database

**Estimated effort**: 1-2 weeks

[FILE LINK]
---

### Iterative Retrieval

**Source**: Grok feedback
**Priority**: Low
**Status**: Complexity vs. benefit unclear

**Description**: Multi-round retrieval where initial results inform subsequent queries.

**Benefit**: Better coverage for complex queries requiring multiple perspectives

**Drawback**: Significantly increased latency, more complex to tune

**Implementation**:
- Query refinement logic
- Stopping criteria (when to stop iterating)
- Result merging
- Performance optimization

**Estimated effort**: 3-4 weeks

**Recommendation**: Wait for user feedback on v2.0 before implementing

[FILE LINK]
---

### Dynamic Thresholds

**Source**: Grok feedback
**Priority**: Medium
**Status**: Requires extensive testing data

**Description**: Adaptive confidence/similarity thresholds based on query characteristics.

**Current**: Static thresholds per domain (0.6, 0.7, 0.85)

**Enhanced**: 
- Adjust based on query complexity
- Account for source diversity
- Consider historical accuracy for similar queries

**Implementation**:
- Threshold adjustment algorithm
- Query complexity analyzer
- Extensive testing across query types
- A/B testing for validation

**Estimated effort**: 4-6 weeks

**Blocking dependency**: Large test dataset from v2.0 usage

[FILE LINK]
---

### ML-Based Domain Classifier

**Source**: v2.0 limitation
**Priority**: Medium
**Status**: Training data needed

**Description**: Replace keyword classifier with trained ML model for better edge case handling.

**Benefit**: 
- Handles rephrased queries better
- Detects implicit high-stakes content
- Reduces false negatives in domain detection

**Implementation**:
- Collect training data from v2.0 usage
- Label dataset (domain + risk level)
- Train classifier (BERT-based or similar)
- A/B test against keyword classifier
- Gradual rollout

**Estimated effort**: 4-6 weeks

**Blocking dependency**: Training data from production usage


---

### Attention-Based Verification

**Source**: v1.0 spec
**Priority**: Low (experimental)
**Status**: Research needed

**Description**: Use attention weights to verify grounding during generation.

**Benefit**: Real-time verification, could catch issues before full generation

**Concerns**: 
- Attention doesn't reliably indicate grounding (ChatGPT feedback)
- Model-specific implementation
- May not work well with quantized models

**Implementation**:
- Extract attention weights from llama.cpp
- Attention analysis algorithms
- Extensive validation
- Compare with post-generation verification

**Estimated effort**: 6-8 weeks (research + implementation)

**Recommendation**: Research project, not production feature yet

---

### Multi-Model Verification Ensemble

**Source**: v1.0 spec
**Priority**: Low
**Status**: Resource intensive

**Description**: Use multiple verification models and voting for higher accuracy.

**Example**:
- Semantic similarity (fast filter)
- NLI model 1 (DeBERTa)
- NLI model 2 (RoBERTa)
- Vote: pass if 2/3 agree

**Benefit**: Higher accuracy, especially for edge cases

**Drawback**: 3x verification latency, 3x memory for models

**Implementation**:
- Load multiple verification models
- Voting logic
- Parallel execution optimization
- Benchmark vs. single model

**Estimated effort**: 3-4 weeks

**Recommendation**: Only if single-model verification shows systematic failures

---

## Part 3: Scope Expansion

### Additional High-Stakes Domains

#### Medical Information

**Status**: Requires expert validation
**Priority**: High (but extensive prep needed)

**Requirements**:
- Medical expert consultation
- Domain-specific credibility scoring (medical journals, FDA, etc.)
- Enhanced verification for medical claims
- Legal review (medical advice regulations)
- Testing with medical professionals

**Estimated effort**: 8-12 weeks + expert review

---

#### Financial Advice

**Status**: Requires regulatory compliance
**Priority**: High (but regulatory review needed)

**Requirements**:
- Financial expert consultation
- Regulatory compliance review (SEC, FINRA)
- Domain-specific policies
- Risk disclosure templates
- Testing with financial professionals

**Estimated effort**: 8-12 weeks + regulatory review

---

#### Additional Coding Domains

**Status**: Natural extension
**Priority**: Medium

**Examples**:
- Mobile development (iOS, Android)
- Web frameworks (React, Vue, etc.)
- DevOps and infrastructure
- Database systems
- Machine learning/AI

**Implementation**:
- Expert libraries for each domain
- Domain-specific keywords
- Testing with developers

**Estimated effort**: 2-3 weeks per domain (parallelizable)

---

### Mobile Sync Architecture

**Source**: v1.0 spec
**Priority**: Medium
**Status**: Requires server component

**Description**: Full edge + mobile hybrid architecture from v1.0 spec.

**Components**:
- Edge server (your hardware with larger model)
- Mobile client (lightweight model + sync)
- Encrypted sync protocol
- Offline mode (cached evidence graph)
- Query routing (local vs. escalate)

**Requirements**:
- Server infrastructure
- Mobile app development
- Encryption implementation
- Sync protocol design
- Conflict resolution
- Testing across devices

**Estimated effort**: 12-16 weeks

**Blocking dependency**: Stable v2.0 + available server hardware

---

### Full Audit Trail System

**Source**: v1.0 spec
**Priority**: Medium
**Status**: UI/UX heavy

**Description**: Web-based interface for viewing and analyzing audit trails.

**Features**:
- Query history browser
- Evidence graph visualization
- Verification result analysis
- Performance analytics
- Failure analysis dashboard
- Export capabilities

**Requirements**:
- Web backend (Flask/FastAPI)
- Web frontend (React/Vue)
- Graph visualization library
- Analytics engine
- Database for history

**Estimated effort**: 8-10 weeks

---

## Part 4: Nice-to-Have Improvements

### User Override Mechanisms

**Source**: GitHub Gap 7
**Priority**: Medium
**Status**: UX improvement

**Description**: Allow users to provide feedback and corrections.

**Features**:
1. **False Rejection Override**:
   - User: "This claim IS supported, you're wrong"
   - System: Request explanation or additional source
   - Manual review queue

2. **Manual Source Upload**:
   - User uploads document supporting claim
   - System re-verifies with new source
   - Updates evidence graph

3. **Credibility Feedback**:
   - User: "This source is unreliable"
   - System: Adjusts credibility score
   - Learning loop

**Implementation**:
- Feedback UI
- Manual review queue
- Learning system for credibility adjustments
- A/B testing for impact

**Estimated effort**: 3-4 weeks

---

### Expanded Adversarial Testing

**Source**: GitHub Gap 8
**Priority**: High (quality assurance)
**Status**: Continuous improvement

**Description**: More comprehensive adversarial test


## END PHASE TWO SPEC