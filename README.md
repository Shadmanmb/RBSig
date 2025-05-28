# RBSig
This repository contains the introduction and implementation of a new primitive called redactable blind signature (RBSig).
Setup:
  pup = Setup(1^lambda)

KeyGen:
  bsk, bvk = KeyGen(pup)

Mask:
  t = random()
  C = g^t * product(Y_i^{m_i})
  send (C, pi) to signer

Sign:
  if VerifyProof(pi) == False:
    return ⊥
  u = random()
  hat_sigma_1 = g^u
  hat_sigma_2 = (X * C)^u
  return (hat_sigma_1, hat_sigma_2)

Unblind:
  sigma_1 = hat_sigma_1
  sigma_2 = hat_sigma_2 / (hat_sigma_1^t)
  return (sigma_1, sigma_2)

Derive:
  r, tau = random(), random()
  sigma_1_prime = sigma_1^r
  sigma_2_prime = sigma_2^r * (sigma_1_prime)^tau
  bar_sigma_1 = tilde_g^tau * product(tilde_Y_j^{m_j} for j in complement(I))
  bar_sigma_2 = (product(tilde_Y_i for i in I))^tau * product(Z_{i,j}^{m_j} for i in I, j in complement(I))
  return (bar_sigma_1, bar_sigma_2, sigma_1_prime, sigma_2_prime)

Verify:
  valid = (e(tilde_X * bar_sigma_1 * product(tilde_Y_i^{m_i}), sigma_1_prime) == e(tilde_g, sigma_2_prime))
          and (e(bar_sigma_1, product(Y_i)) == e(g, bar_sigma_2))
  return valid
