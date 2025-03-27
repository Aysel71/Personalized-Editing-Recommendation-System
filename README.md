# 🎨 Personalized Image Editing Recommender

A lightweight system for personalized image editing using user feedback and latent manipulation in StyleGAN-based models. Instead of generating new images, we recommend semantic **editing directions** and **intensities** (e.g., "add glasses", "make older") based on what the user prefers.

---

## 📌 Project Overview

This system allows a user to:
1. Select or generate a base image (e.g., face)
2. View edits of that image using predefined semantic directions
3. Choose which edits they prefer in A/B testing
4. Receive a personalized recommendation: _the best editing direction + strength_ to apply

Built using [StyleFeatureEditor](https://github.com/ControlGenAI/StyleFeatureEditor), `scikit-learn`, and optionally `Streamlit` or `Gradio` for UI.

---

## 🧠 Key Idea

We turn the problem of **image editing preference** into a **recommendation problem**:

> Given a user's past preferences over edited images, predict which semantic modification they would like most.

---

## 🛠 Pipeline

```text
1. Generate or load base image
2. Invert to latent space (w / w+)
3. Apply edits with random directions and magnitudes (using StyleFeatureEditor)
4. Show image pairs → collect user choices
5. Train model on user feedback
6. Recommend best (direction, alpha)
7. Apply and show final personalized edit
```

---

## 📁 Repository Structure

```bash
.
├── data/
│   └── interactions.csv         # Logs of user choices
├── directions/
│   └── available_directions.txt # Predefined editing directions
├── notebooks/
│   └── exploration.ipynb        # EDA and initial modeling
├── src/
│   ├── editor.py                # Apply edits via StyleFeatureEditor
│   ├── logger.py                # Log user choices
│   ├── model.py                 # Train/predict recommendation model
│   └── ui.py                    # Optional user interface (e.g., Streamlit)
├── README.md
└── requirements.txt
```

---

## 🧪 Example Interaction (CLI or UI)

```
Original vs Edited image:
  [A] Original      [B] Added bangs, alpha=1.4

Which do you prefer? → B

✅ Logged: direction='bangs', alpha=1.4, label=1
```

After 5+ rounds:
> 🎯 Recommended edit: `curly_hair` with alpha=1.3  
> ✅ Applied to image and saved as final result.

---

## 📊 Model Details

- **Input**: direction (one-hot / embedding), alpha (float)
- **Target**: liked (binary: 0/1)
- **Models**: Logistic Regression, LightGBM, or PreferenceRankNet

---

## 📦 Installation

```bash
git clone https://github.com/your-username/image-edit-recommender.git
cd image-edit-recommender
pip install -r requirements.txt
```

---

## ✅ Requirements

- `torch`, `numpy`, `pandas`
- `scikit-learn`
- `StyleFeatureEditor`
- *(Optional)* `streamlit` or `gradio` for frontend

---

## 🧩 Future Work

- Learn multi-direction edits (composite recommendations)
- Real-time personalization / online learning
- Integration with diffusion models (e.g., InstructPix2Pix)
- Active learning for smarter exploration

---

## 📄 License

MIT License

---

## ✨ Acknowledgements

Built on top of:
- [StyleFeatureEditor](https://github.com/ControlGenAI/StyleFeatureEditor)
- StyleGAN2 / e4e / pSp
