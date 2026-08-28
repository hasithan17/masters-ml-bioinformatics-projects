# **Anti‑Cancer Peptide Classification Using an RNN (PyTorch)**  
**Author:** Hasitha Nannapaneni  

---

## **Overview**
This project builds a **Recurrent Neural Network (RNN)** using **PyTorch** to classify anti‑cancer peptides (ACPs) from raw amino‑acid sequences. The model uses:

- **Label‑encoded tokenization**  
- **Padding for variable‑length sequences**  
- **Embedding layer**  
- **LSTM sequence encoder**  
- **Binary classifier with sigmoid output**

The goal is to determine whether a peptide sequence exhibits anti‑cancer activity.

This project demonstrates:
- End‑to‑end biological sequence preprocessing  
- PyTorch embedding + LSTM modeling  
- Training with early stopping  
- ROC‑AUC evaluation  
- Model interpretability through metrics and visualization  

---

## **Dataset**
- Input: peptide sequences (variable length)  
- Target: binary label (1 = anti‑cancer peptide, 0 = non‑ACP)  
- Format: CSV file with columns:  
  - `sequence`  
  - `target`  

### **Tokenization**
- Extract unique amino‑acid characters  
- Encode each character using `LabelEncoder`  
- Pad sequences to uniform length using `nn.utils.rnn.pad_sequence`  
- Add a special **padding token**  

### **Final Shapes**
- `X`: padded integer token matrix  
- `y`: binary labels  

---

## **Model Architecture (PyTorch RNN)**

### **Layers**
- **Embedding layer**  
  Converts token indices → dense vectors  
- **LSTM layer**  
  Learns sequential peptide patterns  
- **Mean pooling**  
  Aggregates sequence information  
- **Fully connected layer**  
  Outputs a single probability  
- **Sigmoid activation**  
  Converts logits → ACP probability  

### **Architecture Summary**
```
Embedding(vocab_size + 1, embed_dim)
LSTM(embed_dim → hidden_dim)
Mean Pooling
Linear(hidden_dim → 1)
Sigmoid
```

### **Training Setup**
- Loss: **Binary Cross‑Entropy (BCELoss)**  
- Optimizer: **Adam**  
- Learning rate: **1e‑4**  
- Weight decay: **1e‑5**  
- Batch size: **256**  
- Early stopping with patience = 3  

---

## **Results**

### **Metrics**
The model reports:

- **Accuracy**  
- **Precision**  
- **Recall**  
- **ROC‑AUC**  

These metrics are computed on the held‑out test set.

### **Visualizations**
- Training vs. validation loss curves  
- ROC curve  

---

## **Project Structure**
```
acp-rnn-pytorch/
│
├── README.md
├── notebooks/
│   └── acp_rnn.ipynb
```
---

## **How to Run**
```
pip install -r requirements.txt
python src/train.py
python src/evaluate.py
```

---

## **Biological Relevance**
Anti‑cancer peptides are short amino‑acid sequences with therapeutic potential. Sequence‑based deep learning models can help identify ACPs by learning:

- motif‑like patterns  
- charge/hydrophobicity signals  
- structural tendencies encoded in sequence  

This project demonstrates how RNNs can model biological sequences effectively using PyTorch.
