# SafetyNet: Real‑Time Threat Detection from Video

> Deep‑learning pipeline for motion‑ and pose‑based threat classification from body‑cams and CCTV.

<p align="center">
  <img src="docs/overview.png" alt="SafetyNet overview diagram" width="720"/>
</p>

---

## Highlights

* **Goal:** Classify **THREAT / NO‑THREAT / WATCH‑OUT** from short video clips.
* **Best model:** **Bi‑LSTM** on CNN features. Val accuracy ≈ **0.92**.
* **Stack:** Python, TensorFlow/Keras, OpenCV, Streamlit, Hugging Face Spaces.
* **Outcome:** A+ grade. Live‑feed inference demo. Team of five led by the author.

---

## Problem Statement

Build a system that flags whether a subject’s motion indicates threat using pose/motion cues, enabling **real‑time officer alerts** and **CCTV crowd monitoring**.

**Stakeholders:** Police departments, courts, security companies.

**Why it matters:** Research reports ~1,000 civilian deaths and hundreds of thousands of injuries annually in law‑enforcement interactions. The target is **safer, more accountable** encounters.

---

## Approach

1. **Frames → Features:** Extract fixed‑length frame windows from video. Use a CNN to embed frames.
2. **Temporal Modeling:** Feed per‑frame CNN features to a sequence model (Bi‑LSTM; also tested ConvLSTM, LRCN).
3. **Training:** Categorical cross‑entropy, ReLU activations; tuned sequence length and sampling strategy.
4. **Deployment:** Streamlit app with live webcam feed; packaged to Hugging Face Spaces for demos.

---

## Data

* **Sources merged:** HMDB51, UCF50, Real‑Life Violence (VID) → balanced into **THREAT / NO‑THREAT / WATCH‑OUT**.
* **Pre‑processing:**

  * Clip standardization to a **fixed temporal length** (e.g., ≈3 seconds).
  * Frame extraction with OpenCV; optional resize and normalization.
  * Train/val/test stratified splits.
* **Constraints:** Google Colab limits informed sequence length, batch size, and augmentation.

> See `data/README.md` for exact folder layout, class counts, and scripts.

---

## Models

* **CNN feature extractor:** Any ImageNet‑pretrained backbone (e.g., MobileNetV2/ResNet18) on per‑frame images.
* **Sequence heads tested:**

  * **Bi‑LSTM** (selected): robust temporal context, best validation accuracy (~0.92).
  * ConvLSTM: ~0.67.
  * LRCN (CNN+LSTM): ~0.74.
* **Training details:** SGD/Adam trials; early stopping; dropout; batch size ≈ 8; ~20 epochs typical.

---

## Results

| Model       | Val Acc  |
| ----------- | -------- |
| ConvLSTM    | 0.67     |
| LRCN        | 0.74     |
| **Bi‑LSTM** | **0.92** |

**Confusions to watch:** borderline WATCH‑OUT vs THREAT when motion is subtle or occluded.

---

## Live Demo

* **Streamlit UI:** Real‑time webcam inference and clip upload.
* **Hugging Face Spaces:** One‑click public demo.

---

## Reproducibility

* Deterministic seeds where supported.
* Saved configs and checkpoints per run directory.
* Logged metrics and confusion matrices.

---

## Ethics & Safety

* Model **assists** humans and must not be treated as sole arbiter.
* Known risks: dataset bias, domain shift, occlusion, adverse lighting, adversarial behavior.
* Required controls: audit logs, human‑in‑the‑loop review, calibrated thresholds, periodic re‑training, community oversight.

---

## Limitations

* Pose ambiguity and occlusions degrade accuracy.
* Dataset class balance affects WATCH‑OUT sensitivity.
* Real‑time constraints limit backbone size and input resolution.

---

## Roadmap

* [ ] Integrate **object detection** for weapons.
* [ ] Add **speech recognition** channel for verbal cues.
* [ ] **Facial sentiment** analysis for context.
* [ ] Quantize and prune for edge devices.
* [ ] Domain adaptation for body‑cam vs CCTV.

---

## Acknowledgments

* Advisors: **Dr. Tan Wee Kek**, **Dr. Amirhassan Monajemi** (National University of Singapore).
* Team of five; project lead: repository author.

---

## Citation

If you use SafetyNet, cite as:

```bibtex
@software{SafetyNet2024,
  author  = {Parmaj, Viraj and team},
  title   = {SafetyNet: Real-Time Threat Detection from Video},
  year    = {2024},
  url     = {https://github.com/virajparmaj/SafetyNet}
}
```

---

## License

MIT. See `LICENSE`.
