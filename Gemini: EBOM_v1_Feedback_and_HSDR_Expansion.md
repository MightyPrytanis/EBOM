# EBOM Protocol v1.0: Analysis, Feedback, and HSDR Technical Expansion

## 1. Professional Analysis & Critique
The **EBOM Protocol v1.0** is a superior alternative to standard RAG implementations because it shifts the burden of proof from the user to the system architecture. By treating the LLM as a stateless transformation component, it eliminates "hallucination by authority."

### 1.1 Key Strengths
* **Epistemic Modality:** The separation of Verified, Inferential, and Speculative content allows for "graded trust."
* **Sphere Architecture:** Explicitly acknowledging the "Liminal" boundary addresses the real-world problem of time-sensitive and contradictory data.
* **Grammar-Based Enforcement:** The move toward structural constraints (rather than just prompting) makes compliance a feature of the software, not the model's "intent."

### 1.2 Identified Risks & Recommendations
* **The "Black Box" Refusal:** In high-stakes domains, a model that simply refuses to answer can be as dangerous as one that lies (e.g., failing to provide emergency medical info). 
    * **Recommendation:** Implement **Diagnostic Refusals** (detailed in Section 2).
* **The Privacy Boundary:** Verification via external LLMs (LLM-as-Judge) can leak "Inside Sphere" data.
    * **Recommendation:** Enforce local-only verification (Semantic Similarity/Local NLI) for all "Inside Sphere" claims.
* **Liminal Decay:** Liminality is currently static.
    * **Recommendation:** Add a temporal decay function. If an "Outside" source is older than $T$ (e.g., 24 hours), it must be flagged as Liminal.

---

## 2. Expansion: High-Stakes Diagnostic Refusal (HSDR)
When the system cannot meet the strict evidentiary thresholds required for high-stakes domains (e.g., similarity $< 0.85$ or source count $< 2$), it must not return a generic "I can't answer." It must return a **Diagnostic Object** that identifies the specific evidentiary gap.

### 2.1 HSDR Logic Flow
If the Policy Enforcer detects a violation in a High-Stakes domain, the workflow branches to a **Diagnostic Node** which:
1.  Identifies the specific claim that failed.
2.  Explains why it failed (e.g., "Contradiction between Source A and Source B").
3.  Suggests remediation (e.g., "Please provide the specific lab results for a more accurate inside-sphere check").

---

## 3. Technical Implementation: The Policy Enforcer (Node 4)
The following Python code defines the `policy_enforcer_node` for a **LangGraph** implementation. This node acts as the final arbiter before output reaches the user.

```python
from typing import TypedDict, List, Union, Optional
from pydantic import BaseModel, Field

# Core EBOM Schemas for Node 4
class VerifiedFact(BaseModel):
    statement: str
    source_id: str
    confidence: float

class EBOMResponse(BaseModel):
    verified_facts: List[VerifiedFact]
    probabilistic_inferences: List[str] = []
    speculative_content: List[str] = []

class DiagnosticRefusal(BaseModel):
    status: str
    failure_mode: str
    target_claim: str
    evidence_gap: str
    remediation_steps: List[str]

class EBOMState(TypedDict):
    query: str
    domain: str
    is_high_stakes: bool
    raw_response: EBOMResponse 
    final_output: Union[EBOMResponse, DiagnosticRefusal]

def policy_enforcer_node(state: EBOMState):
    """
    EBOM Node 4: Programmatic Policy Enforcement & HSDR Logic.
    """
    response = state["raw_response"]
    
    # 1. High-Stakes Domain Enforcement
    if state["is_high_stakes"]:
        # Immediately strip speculative and inferential content
        response.speculative_content = []
        response.probabilistic_inferences = []
        
        # 2. Evidence Threshold Verification (HSDR Logic)
        for claim in response.verified_facts:
            # Policy: High-Stakes requires confidence >= 0.85
            if claim.confidence < 0.85:
                return {
                    "final_output": DiagnosticRefusal(
                        status="REFUSED_EVIDENCE_GAP",
                        failure_mode="CONFIDENCE_BELOW_THRESHOLD",
                        target_claim=claim.statement,
                        evidence_gap="The claim lacks sufficient grounding in current sources.",
                        remediation_steps=["Provide more context", "Trigger deep web-search"]
                    )
                }
        
        # 3. Source Count Check
        if len(response.verified_facts) < 2:
            return {
                "final_output": DiagnosticRefusal(
                    status="REFUSED_INSUFFICIENT_SOURCES",
                    failure_mode="MIN_SOURCE_NOT_MET",
                    target_claim="General Inquiry",
                    evidence_gap="Policy requires at least 2 independent sources.",
                    remediation_steps=["Check Outside Sphere tools for additional validation"]
                )
            }

    # 4. Final Compliance Passed
    return {"final_output": response}

