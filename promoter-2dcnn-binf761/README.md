# **Predicting Human Promoter Regions Using a 2D Convolutional Neural Network (BINF 761)**  
**Author:** Hasitha Nannapaneni  

---

## **Overview**
This project investigates whether a **2D Convolutional Neural Network (2D‑CNN)** can accurately distinguish human promoter regions from non‑promoter genomic DNA using only raw sequence input. Promoter sequences were obtained from the **EPDnew Human Promoter Database**, and matched non‑promoter sequences were generated from random 1000 bp windows in the **hg38** human reference genome.

After cleaning and one‑hot encoding each sequence into a **1000×4×1** tensor, the 2D‑CNN was trained to classify promoter vs. non‑promoter DNA **without handcrafted features or motif definitions**.

---

## **Dataset**

### **Promoter Sequences**
- Source: **EPDnew Human Promoter Database**  
- URL: [https://epd.epfl.ch/EPDnew/human.php](https://epd.epfl.ch/EPDnew/human.php)  
- Extracted window: **−500 bp to +500 bp** around each transcription start site (TSS)  
- Cleaned to exactly **1000 bp**  
- Removed sequences containing ambiguous bases (N)

### **Non‑Promoter Sequences**
- Source: **hg38 reference genome**  
- Random 1000 bp windows  
- Excluded known promoters  
- Removed ambiguous bases

### **Encoding**
- One‑hot encoding → **1000×4** matrix  
- Added channel dimension → **1000×4×1**  
- Stratified split: **80% train**, **10% validation**, **10% test**

---

## **Model Architecture (2D‑CNN)**

The model uses 2D convolutions to learn spatial patterns across both nucleotide identity and local sequence context.

**Architecture Summary**
- **Conv2D (32 filters, 5×4 kernel)** — learns short motif‑like patterns  
- **MaxPooling2D (2×1)**  
- **Conv2D (64 filters, 7×1 kernel)** — learns broader regulatory structure  
- **MaxPooling2D (2×1)**  
- **Flatten**  
- **Dense(128, ReLU)**  
- **Dropout(0.4)**  
- **Dense(1, Sigmoid)** — promoter probability  

**Training Setup**
- Optimizer: Adam (lr = 1e‑4)  
- Loss: Binary cross‑entropy  
- Batch size: 64  
- Early stopping based on validation loss  

---

## **Results**

### **Performance**
- **Test Accuracy:** 0.9067  
- **ROC‑AUC:** 0.9683  

### **Confusion Matrix**
- True negatives: 2682  
- False positives: 278  
- False negatives: 274  
- True positives: 2686  

### **Interpretation**
The model successfully learned promoter‑specific sequence patterns such as:
- TATA‑box‑like motifs  
- GC‑rich segments  
- CpG‑dense regions  
- positional dependencies  

These patterns emerged **without** handcrafted features, demonstrating the strength of CNNs for regulatory genomics.

---

## **Limitations**
- Negative samples may contain weak promoter‑like regions  
- No epigenetic context (ATAC‑seq, histone marks, TF binding)  
- Dense layer has >2M parameters → risk of overfitting  
- Promoters treated as a single class (no subtype modeling)

---

## **Future Improvements**
- Integrate ATAC‑seq or histone modification data  
- Use transformer‑based architectures for long‑range dependencies  
- Replace dense layer with global average pooling  
- Train tissue‑specific promoter models  
- Apply interpretability tools (saliency maps, DeepLIFT)

---

## **How to Run**
```
pip install -r requirements.txt
python src/train.py
python src/evaluate.py
```

---

## **Project Structure**
```
promoter-2dcnn-binf761/
│
├── README.md
├── notebooks/
│   └── promoter_2dcnn.ipynb
├── src/
│   ├── model.py
│   ├── train.py
│   ├── evaluate.py
│   └── utils.py
└── results/
    ├── roc_curve.png
    ├── confusion_matrix.png
    └── training_curves.png
```
