# hybrid-neuro-symbolic-ai
Self-correcting AI combining neural &amp; symbolic reasoning
"""
HYBRID NEURO-SYMBOLIC AI PIPELINE
Anti-Hallucination | Multi-Modal | Temporal Reasoning | Self-Correction

Author: Juragan
Purpose: Solve "Logika vs Stokastik" problem
"""

import time
import statistics
from typing import List, Dict

# =========================
# SYMBOLIC RULE ENGINE
# =========================
class RuleEngine:
    def __init__(self):
        self.rules = []

    def add_rule(self, condition, action):
        self.rules.append((condition, action))

    def evaluate(self, context: Dict):
        decisions = []
        for cond, act in self.rules:
            if cond(context):
                decisions.append(act(context))
        return decisions


# =========================
# TEMPORAL ANALYZER
# =========================
class TemporalAnalyzer:
    def __init__(self, window=10):
        self.window = window
        self.buffer = []

    def update(self, score):
        self.buffer.append(score)
        if len(self.buffer) > self.window:
            self.buffer.pop(0)

    def stability_score(self):
        if len(self.buffer) < 3:
            return 0.0
        return statistics.pstdev(self.buffer)


# =========================
# NEURAL PLACEHOLDERS
# =========================
def vision_model(frame) -> Dict:
    """
    Simulated output of Vision AI
    """
    return {
        "label": "LARVA",
        "severity": 0.85  # 0–1
    }

def audio_model(audio_chunk) -> Dict:
    """
    Simulated Audio Sentiment AI
    """
    return {
        "tone": "CALM",
        "panic_score": 0.1
    }

def ocr_model(frame) -> List[str]:
    """
    Simulated OCR output
    """
    return ["METODE STERILISASI", "EDUKASI MEDIS"]

def medical_verifier(labels) -> float:
    """
    Simulated Medical Knowledge Verification
    """
    trusted_terms = {"LARVA", "STERILISASI", "MEDIS"}
    hits = sum(1 for l in labels if l in trusted_terms)
    return min(hits / len(trusted_terms), 1.0)


# =========================
# SELF-CORRECTING REASONER
# =========================
class SelfCorrectingAI:
    def __init__(self):
        self.temporal = TemporalAnalyzer()
        self.rules = RuleEngine()
        self._build_rules()

    def _build_rules(self):
        # Rule 1: Extreme visual but calm + scientific → whitelist
        self.rules.add_rule(
            lambda c: c["visual"] > 0.7 and c["audio"] < 0.3 and c["medical"] > 0.6,
            lambda c: "WHITELIST"
        )

        # Rule 2: Extreme visual appears briefly → context explanation
        self.rules.add_rule(
            lambda c: c["temporal"] < 0.15 and c["visual"] > 0.7,
            lambda c: "CONTEXTUAL_EXTREME"
        )

        # Rule 3: Panic audio + extreme visual → restrict
        self.rules.add_rule(
            lambda c: c["audio"] > 0.7 and c["visual"] > 0.7,
            lambda c: "RESTRICT"
        )

    def analyze(self, frame, audio):
        vision = vision_model(frame)
        audio_res = audio_model(audio)
        ocr_text = ocr_model(frame)
        medical_score = medical_verifier(ocr_text)

        self.temporal.update(vision["severity"])

        context = {
            "visual": vision["severity"],
            "audio": audio_res["panic_score"],
            "medical": medical_score,
            "temporal": self.temporal.stability_score(),
            "ocr": ocr_text
        }

        decisions = self.rules.evaluate(context)

        # =========================
        # SELF-CORRECTION LAYER
        # =========================
        if not decisions:
            if context["medical"] > 0.7:
                decisions.append("SAFE_BY_KNOWLEDGE")
            else:
                decisions.append("UNSURE_REVIEW")

        return {
            "decisions": decisions,
            "context": context
        }


# =========================
# PIPELINE EXECUTION
# =========================
def run_pipeline():
    ai = SelfCorrectingAI()

    for t in range(15):
        frame = f"frame_{t}"
        audio = f"audio_{t}"

        result = ai.analyze(frame, audio)

        print(f"\nFRAME {t}")
        print("DECISION:", result["decisions"])
        print("CONTEXT:", result["context"])

        time.sleep(0.1)


# =========================
# ENTRY POINT
# =========================
if __name__ == "__main__":
    run_pipeline()
# Hybrid Neuro-Symbolic Self-Correcting AI

This repository contains a **research-oriented prototype** that addresses one of the core problems in modern AI systems:

> **The “Logic vs Stochastic” (Hallucination) Problem**

Current AI models excel at statistical pattern recognition but often fail at simple, rigid logic when facing unseen scenarios.  
This project explores a **hybrid approach** combining neural inference with symbolic reasoning and self-correction.

---

## 🧠 Core Idea

The system integrates:

- **Neural Components** (pattern recognition)
  - Visual analysis (object severity detection)
  - Audio sentiment analysis
  - OCR-based semantic extraction
  - Knowledge-based semantic verification

- **Symbolic AI Components** (deterministic logic)
  - Rule-based decision engine
  - Explicit if–then reasoning
  - Hard constraints and logical consistency checks

- **Temporal Reasoning**
  - Prevents overreaction to single extreme frames
  - Evaluates stability across time windows

- **Self-Correction Layer**
  - Detects uncertainty
  - Re-evaluates decisions using symbolic rules and knowledge confidence
  - Reduces hallucinated conclusions

---

## 🎯 Goal

To demonstrate how **Neural Networks + Symbolic AI** can work together to:

- Reduce hallucination
- Improve logical consistency
- Enable context-aware moderation and classification
- Support explainable AI decisions

---

## 🧩 Example Use Case

If a video contains:
- **Extreme visual content**
- **Calm, scientific narration**
- **Valid medical or educational references**

Then the system can:
- Contextualize the extremity
- Whitelist the content
- Allow monetization or safe distribution

---

## 📂 Project Structure

---

## ⚠️ Disclaimer

This project is a **conceptual and architectural prototype**.

- Neural models are represented as modular placeholders
- Real-world deployment would require:
  - Trained vision/audio/OCR models
  - Verified medical knowledge databases
  - Scalable inference infrastructure

---

## 🤖 Development Note

This project was built as a **personal exploration** using **AI-assisted coding** combined with original system design and reasoning decisions.

---

## 📜 License

Open for research, learning, and experimentation.
