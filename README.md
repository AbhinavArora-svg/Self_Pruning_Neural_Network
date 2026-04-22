# **102316069_CaseStudy**
# **Self-Pruning Neural Network**

## **Overview**

This project implements a Self-Pruning Neural Network on the CIFAR-10 dataset.  
The model learns to automatically prune its own weights during training using learnable gate parameters and sparsity regularization.

---

## **Approach**

- Each weight is associated with a learnable **gate parameter**
- Gate values are passed through a **sigmoid function** to constrain them between 0 and 1

- Effective weight used in forward pass:
  
  weight × sigmoid(gate_score)

- If the gate value becomes close to 0, the corresponding weight is effectively pruned

---

## **Loss Function**

Total Loss = Classification Loss + λ × Sparsity Loss

- Classification Loss: Cross Entropy
- Sparsity Loss: Sum of all gate values (L1-like regularization)
- λ (lambda): Controls the strength of pruning

---

## **Why L1 Encourages Sparsity**

The sparsity loss behaves like L1 regularization:

- It applies constant pressure to reduce gate values
- Many values are pushed toward zero
- This results in removal of less important connections

---

## **Model Architecture**

Fully Connected Neural Network (MLP):

- Input: 3072 (32×32×3 image)
- Layer 1: 3072 → 512
- Layer 2: 512 → 256
- Output: 256 → 10 (CIFAR-10 classes)

---

## **Results**

| Lambda | Accuracy (%) | Sparsity (%) |
|--------|-------------|--------------|
| 1e-05  | 54.43       | 0.04         |
| 0.0001 | 54.40       | 0.07         |
| 0.001  | 53.88       | 0.08         |

---

## **Observations**

- Increasing λ slightly decreases accuracy
- Sparsity increases with λ but remains very low
- The model does not prune aggressively under current settings
- Most gate values remain close to 1 (weights remain active)

---

## **Gate Distribution**

The histogram of gate values shows:

- Majority of gates are near 1 → active weights
- Very few gates near 0 → limited pruning

---

## **Summary Plots**

- Accuracy vs Lambda
- Sparsity vs Lambda

These plots show the trade-off between performance and pruning.

---

## **Conclusion**

The model successfully implements self-pruning using learnable gates and sparsity loss.

However:

- Current λ values are too small to enforce strong pruning
- Sparsity remains minimal
- Better results can be achieved with higher λ or longer training

---
