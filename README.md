# Awesome-Small-Field-ZKP

Small finite fields used in real implementations include:

| **Project**          | **Subproject**       | **Field Name**      | **Field Description**                                                                                     | **Links**                                                                                                             |
|----------------------|----------------------|---------------------|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Starknet**         | **Stone**            |         | `p = 2^251 + 17 * 2^192 + 1`     | [Link to code](https://github.com/starkware-libs/stone-prover/blob/main/src/starkware/algebra/fields/big_prime_constants.h#L51) |
|                      | **Stwo**             | Mersenne31          | `p = 2^31 - 1`                                                                                            | [Link to code](https://github.com/starkware-libs/stwo/blob/dev/crates/prover/src/core/fields/m31.rs#L13)              |
| **RISC Zero zkVM**   |                      | BabyBear            | `p = 2^31 - 2^27 + 1` (or `p = 15 * 2^27 + 1`)                                                            | [Link to code](https://github.com/risc0/risc0/blob/main/risc0/core/src/field/baby_bear.rs#L44)                       |
| **Polygon Zero**     | **Plonky2, zkEVM, Miden** | Goldilocks          | `p = 2^64 - 2^32 + 1`                                                                                     |                                                                                                                       |
|                      | **Plonky3**          | Mersenne31          | `p = 2^31 - 1`                                                                                            |                                                                                                                       |
| **Binius**           |                      | Binary Tower Field  |                                                               |                                                                                                                       |

---

## Benefits of Using Smaller Fields in zk-Prove Systems

### 1. Insights from Polygon
Polygon has extensively discussed the benefits of smaller fields in their blog about Plonky2 ([source](https://polygon.technology/blog/plonky2-a-deep-dive)):  
- They compared elliptic curve commitment (ECC)-based polynomial commitment schemes (PCS) with FRI and found that FRI offers an interesting tradeoff. It allows for faster proof generation, but the resulting proofs are significantly larger. FRI becomes even faster when using the **Goldilocks Field**, which matches the native 64-bit arithmetic operations on commercially available CPUs. This alignment with CPU architecture accelerates proving time significantly.  
- By leveraging FRI with a 64-bit **Goldilocks Field** instead of a 256-bit KZG field, performance improved **40-fold**.  

In **Plonky3**, Polygon transitioned to a **Mersenne31 Field**, which is better suited for 32-bit CPU/GPU architectures ([source](https://www.youtube.com/watch?v=giFA3UXbu_s&t=20s)).

### 2. Insights from RISC Zero
According to the RISC Zero repository comments, the **BabyBear Field** provides two key advantages:  
1. **CPU Compatibility**: The base field elements are smaller than \( 2^{31} \), allowing them to be represented as 32-bit words. This is highly compatible with standard CPUs, making computations efficient.  
2. **FFT Suitability**: \( p-1 \) of the BabyBear Field can easily be factored, providing a large enough power of 2 to support efficient FFT operations.  

## Existing Work on Accelerating Small Field Prove Systems

| **Paper Title**                                   | **Field Name**  | **Description**                                                                                                   | **Author**  | **Links**                                                                                                          |
|---------------------------------------------------|-----------------|-------------------------------------------------------------------------------------------------------------------|-------------|--------------------------------------------------------------------------------------------------------------------|
| Efficient Implementation of Mersenne31 Field and Polynomial Arithmetic | Mersenne31      | Describes optimized polynomial arithmetic over Mersenne31, based on the context of Circle Stark                  | Ingonyama   | [Link to paper](https://github.com/ingonyama-zk/papers/blob/main/Mersenne31_polynomial_arithmetic.pdf)            |
| NTT in the 64-bit Goldilocks Field               | Goldilocks      | Discusses NTT optimization in the 64-bit Goldilocks Field for faster FFT operations                               | Ingonyama   | [Link to paper](https://github.com/ingonyama-zk/papers/blob/main/goldilocks_ntt_trick.pdf)                        |
| RISC Zero Prover Protocol and Analysis           | BabyBear        | Analyzes RISC Zero's use of the BabyBear field for efficient proof generation                                     | Ingonyama   | [Link to paper](https://github.com/ingonyama-zk/papers/blob/main/risc0_protocol_analysis.pdf)                     |
| Improvements to the Sumcheck Prover              | Various Fields  | Describes techniques to run the sumcheck prover in a fully parallelizable and memory-efficient manner             | Ingonyama   | [Link to repo](https://github.com/ingonyama-zk/smallfield-super-sumcheck/tree/main)                               |
