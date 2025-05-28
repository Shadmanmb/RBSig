# Redactable Blind Signature Scheme

**Author:** [Shdman Mohammdi]  
**Date:** May 2025  

---

## Overview

This repository contains the original redactable blind signature primitive introduced by Shadman Mohammadi, The scheme enables privacy-preserving blind signatures with the ability to redact (selectively reveal) parts of the signed message.

---

## Algorithm Description

### Setup(1^λ)

On input a security parameter λ, output public parameters:

$$
pp \leftarrow (p, G_1, G_2, G_T, e)
$$

---

### KeyGen(pp)

- Select generators \( g \in G_1 \), \( \tilde{g} \in G_2 \), and secret keys \( (x, y_1, \ldots, y_n) \in \mathbb{Z}_p^{n+1} \).
- Compute:
  \[
  (X, Y_1, \ldots, Y_n) = (g^x, g^{y_1}, \ldots, g^{y_n})
  \]
  \[
  (\tilde{X}, \tilde{Y}_1, \ldots, \tilde{Y}_n) = (\tilde{g}^x, \tilde{g}^{y_1}, \ldots, \tilde{g}^{y_n})
  \]
  \[
  Z_{i,j} = \tilde{g}^{y_i y_j}, \quad \forall 1 \leq i \neq j \leq n
  \]
- Signing key: \( bsk = X \)
- Verification key:
  \[
  bvk = (g, \tilde{g}, \tilde{X}, \{Y_i, \tilde{Y}_i\}_{i \in [n]}, \{Z_{i,j}\}_{1 \leq i \neq j \leq n})
  \]

---

### Mask(bvk, \(\{m_i\}\))

- User selects random \( t \in \mathbb{Z}_p \).
- Computes:
  \[
  C = g^t \prod_{i=1}^n Y_i^{m_i}
  \]
- Sends \( (C, \pi) \) to the signer, where \( \pi \) is a proof of correctness.

---

### Sign(bsk, C)

- Verify \( \pi \). If invalid, return \( \perp \).
- Else, select random \( u \in \mathbb{Z}_p \).
- Compute:
  \[
  \hat{\sigma}_1 = g^u, \quad \hat{\sigma}_2 = (X C)^u
  \]
- Return \( \hat{\sigma} = (\hat{\sigma}_1, \hat{\sigma}_2) \).

---

### Unblind(st, \( \hat{\sigma} \))

- Using \( t \) from Mask state \( st \), compute:
  \[
  \sigma_1 = \hat{\sigma}_1, \quad \sigma_2 = \hat{\sigma}_2 / \hat{\sigma}_1^t
  \]
- Return \( \sigma = (\sigma_1, \sigma_2) \), a valid signature on \( \{m_i\} \).

---

### Derive(bvk, \( \sigma, \{m_i\}, I \))

- Select random \( r, \tau \in \mathbb{Z}_p \).
- Compute:
  \[
  \sigma_1' = \sigma_1^r, \quad \sigma_2' = \sigma_2^r \cdot (\sigma_1')^\tau
  \]
  \[
  \bar{\sigma}_1 = \tilde{g}^\tau \prod_{j \in \bar{I}} \tilde{Y}_j^{m_j}, \quad \bar{\sigma}_2 = \left(\prod_{i \in I} \tilde{Y}_i\right)^\tau \prod_{i \in I, j \in \bar{I}} Z_{m_i, j}
  \]
- Return derived signature \( \sigma_I = (\bar{\sigma}_1, \bar{\sigma}_2, \sigma_1', \sigma_2') \).

---

### Ver(bvk, \( \{m_i\}_{i \in I} \), \( \sigma_I \))

- Verify the derived signature \( \sigma_I \) on the subset \( \{m_i\}_{i \in I} \).

---

## Usage

Add instructions here on how to run or test your implementation.

---

## License

Specify your license here (e.g., MIT).

---

## Contact

For questions or collaborations, contact [Your Email].
