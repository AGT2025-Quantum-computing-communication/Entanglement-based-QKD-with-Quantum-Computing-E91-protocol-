# Entanglement-based-QKD-with-Quantum-Computing-E91-protocol-
In this project, I simulate an entanglement based QKD (the E91) protocol in quantum computer. I demonstrate that the secure key rate depends on the selected quantum hardware qubit-pair on which the experiment is executed, in the sense that the pair of qubit with lower two-qubit error provide lower QBER and higher key length.




<h2>Introduction</h2>
Quantum key distribution (QKD) is a direct application of quantum mechanics. It consists of generating a secret key using quantum mechanics principles, such as entanglement, no-cloning theorem, and non-locality of quantum particles. The security of QKD relies on those principles in comparison to classical key distribution whose security relies on complexity. QKD was first introduced by Bennett and Brassard in 1984, with their famous BB84 protocol. This led to several other protocols later which includes, the E91 (Ekert 1991), the B92 (Bennett 1992), etc. The latest are entanglement based protocols, while the former is a prepare-and-measure protocol. 

In this project however, we focus on E91 protocol, using quantum computers. In this protocol, the legitimate users (Ange the sender and Bill the receiver) share entangled qubits (photonic qubits) prepared on the sender's side (i.e. by Ange). We assume that Ange's and  Bill's quantum labs are physically separated, but linked by a quantum channel, which can be under the control of an untrusted third party (i.e. an eavesdropper Eve). We also assume that this quantum channel sends qubits to their respective  quantum computers that are  used to measure the  state of the qubit, and generate by the way a secret key. To assure the secrecy of the protocol, the legitimate users compute the Clauser-Horn-Shimony-Holt (CHSH) inequality based on their respective measurement results, whose value should determine whether the qubits exchanged through the quantum channel were subject to interference or not. The latest shows therefore a high importance to determine whether the communication between legitimate user is been listened by Eve or not, since the action of Eve on the qubits will inevitably introduce errors that modify the correlation. Thus this protocol sounds very interesting and promising for secure communication with recent developments of quantum computers, with error corrected qubits. That means one is able to prepare entangled qubits with long enough coherence time and also determine (or measure) its exact state with a high probability.

This notebook is structured as followed: in section 1, we discuss the algorithm in detail, in section 2, we develop the strategy to test for eavesdropping (i.e. the CHSH inequality). The experimental result is presented in section 3, where we first implement the algorithm on ideal conditions (i.e. no background noise, no eavesdropper's interference), we secondly implement the algorithm without background noise, but with an eavesdropper, and finally the main result on real conditions (i.e. real quantum hardware and eavesdropper interference).  


<h2> 1. The Algorithm</h2>
In this project, we assume the source of entangled qubits on the sender's quantum lab, connected to the receiver lab through an unauthenticated quantum channel. The algorithm to generate a secure key is as follows:

1 - Ange prepares $N$ pairs of maximally entangled qubits, keeps qubit A and sends qubit B to bill through the quantum channel, sensitive to eavesdropping and noise. 

2- Both parties randomly select a basis to measure their respective qubits i.e. (${X, \frac{1}{\sqrt{2}}(X + Z), Z}$) for Ange and (${\frac{1}{\sqrt{2}}(X + Z), Z, \frac{1}{\sqrt{2}}(-X + Z)}$) for Bill.

3- They record their respective measurement results as a bit string ($a = (0,1)^N$, and $b = (0,1)^N$ for Ange and Bill respectively. It is worth mentioning that the measurement basis in step 2 are assumed as an observable to measure in maximally entangled state, i.e. if $X$ is the selected measurement basis on Ange side and $\frac{1}{\sqrt{2}}(X + Z)$ on Bill side, then the corresponding bit is obtained by taking $\bra \Psi X \otimes \frac{1}{\sqrt{2}}(X + Z) \ket \Psi$. Measuring this quantity in the computational basis will lead to either of the following bit strings, '00', '01', '10' or '11', depending on the state $\ket{\Psi}$, where the last bit in each bit string refers to Ange measurement result and the fist to Bill's one.

4- They thus employ a classical channel to communicate to each other their respective measurement basis choices. This step is known as measurement basis reconciliation, which consists of discarding from their respective results those bits that measurement basis do not coincide. It is important to note that in an ideal situation (no noise, neither eavesdropper interference), their remaining bit strings should be perfectly correlated, identical and can be used as a key for secret communication with high privacy. But in real situation this not the case, thus they need extra mechanisms to correct errors and determine whether or not someone interfered with their exchanged qubits.

5- Based on their respective measurement results, they compute the correlation value, which if equal to $2\sqrt{2}$ in absolute value, they can be sure that no noise or no one interfered with their communication process, if not they repeat the procedure from step 1 in different set up. 

6 - Both parties sacrify half of their respective bit strings to approximate the quantum bit error rate (i.e. the ratio of the number of bits that differ with the total number of bits), since even though there might not be anyone interfering with their communication process there is always high probability of error due to background noise. If the value of the latest is too high, they abort the procedure if not the continue. In general if the  quantum bit error rate is above some threshold value it implies the quantum channel is too noisy or someone interfered with the exchanged qubits.

<h2>2. Testing the eavesdropper by CHSH inequality</h2>

The CHSH inequality is employed to experimentally demonstrate Bell's theorem, which states that ''local hidden variables theories do not account for some consequences of quantum mechanics''. The violation of that inequality demonstrates that local hidden variable theory is incompatible with quantum mechanics. In particular, local hidden variables theory shows that the quantum correlation is always less than or equal to 2. The violation of  that inequality can be used to proof entanglement, i.e. $|C| \leq 2\sqrt{2}$, for a maximally entangled state. It is therefore consistent to employ this inequality demonstrate security of E91 protocol, because its violation will certify that someone interfered with the exchanged qubit given that the measurement by a third party collapses entanglement, and thus the correlation reduce to classical correlation, i.e. $|C| \leq 2$, rather than  $|C| \leq 2\sqrt{2}$ as expected. 

Indeed, if $\{a_0,a_1\}$ is the measurement outcome of Ange $\{b_0,b_1\}$ that of Bill, then the CHSH inequality reads:

$$ |E(a_0,b_0) - E(a_0,b_1) + E(a_1,b_0) +E(a_1,b_1)| \leq 2\sqrt{2}.$$

In other words, if $A = (X, Z)$ and $B = (W=\frac{1}{\sqrt{2}}(X + Z), V=\frac{1}{\sqrt{2}}(-X + Z))$ are respectively the measurement basis of Ange and Bill, the CHSH inequality reads:

$$ |\bra \Psi X \otimes W \ket \Psi -\bra \Psi X \otimes V \ket \Psi +\bra \Psi Z \otimes W \ket \Psi+\bra \Psi Z \otimes V \ket \Psi| \leq 2\sqrt{2}. $$

Here in particular if we assume that the qubit pair are prepared in the Bell maximally entangled state $\ket \Psi = \frac{1}{\sqrt{2}}(\ket {01} - \ket {10})$, it is easy to check that $|C| = 2\sqrt{2}$. Therefore, our mission in this work will be to study how this quantity is affected by noise and/or eavesdropper's actions. But let's first demonstrate how to evaluate $|C|$ based on measurement outcomes. For this purpose, let's assume the measurement basis as an observable $O$ to be measured. In the spectral decomposition, this observable can be written as $O = \sum_j \lambda_j \ket {e_j}\bra {e_j}$, where $\lambda_j$ are the eigenvalues and $\ket {e_j}$  the eigenvectors, thus one has:

$$ \bra \Psi O \ket \Psi = \sum_j \lambda_j \langle \Psi|  {e_j}\rangle  \langle  {e_j} | \Psi  \rangle = \sum_j \lambda_j P(\lambda_j),$$ 

with $P(\lambda_j)$ the probability to measure the eigenvalues $\lambda_j$. It comes out naturally that,

$$ \bra \Psi O_1 \otimes O_2 \ket \Psi = \sum_{jk} \lambda_j\lambda_k   P(\lambda_j,\lambda_k),$$ 

where $P(\lambda_j,\lambda_k)$ is the joint probability to measure the eigenvalues $\lambda_j$ and $\lambda_k$ for observables $O_1$ and $O_2$ respectively. In this particular case where the measurement basis of Ange and Bill are well known to be $A = (X, Z)$ and $B =(W=\frac{1}{\sqrt{2}}(X + Z), V=\frac{1}{\sqrt{2}}(-X + Z))$ respectively, the corresponding eigenvalues of each of the observable are always $1$ and $-1$, therefore, 

$$ \bra \Psi A_j \otimes B_k \ket \Psi = P_{jk}(-1,-1) - P_{jk}(-1,1) - P_{jk}(1,-1) + P_{jk}(1,1).$$

Each probability term on the right hand side is determined by accounting the number of times $\lambda_j$ and $\lambda_k$ were measured given that the bases $A_j$ and $B_k$ were selected, i.e. if the experiment consists of sharing $N$ entangled qubit-pairs, thus,

$$P(\lambda_j,\lambda_k) = \frac{n(A_j|=\lambda_j, B_k|=\lambda_k)}{ N(A_j, B_k)},$$ 

For example, to be more precise if $N$ qubit-pairs are shared between Ange and Bill, and during the procedure they select respectively $Z$ and $W$ bases $M$ times, the probabilities are:

$P(-1,-1) = \frac{n(-1,-1)}{M}$, $P(1,-1) = \frac{n(1,-1)}{M}$, $P(-1,1) = \frac{n(-1,1)}{M}$ and $P(1,1) = \frac{n(1,1)}{M}$.

<h2> 3. Experiment: Simulation of E91 on quantum hardwards</h2>

<h3> 3.1. Simulation on ideal conditions: without noise neither eavesdropper interference</h3>
Here we assume having an authenticated quantum channel through out which the qubits are being exchanged during the experiment. We also assume a fault tolerant quantum hardware to measure the state of qubit. This is done by considering a noise-free quantum backend (qiskit Aer simulator). It is known that with such backend we don't escape shot-noise, but we assume its effect negligible. Under these circumstances, the experiment is the following:


Indeed, if ${a_0,a_1}$ is the measurment outcome of Ange ${b_0,b_1}$ that of Bill, then the CHSH inequality reads:


$$ |E(a_0,b_0) - E(a_0,b_1) + E(a_1,b_0) +E(a_1,b_1)| \leq 2\sqrt{2}.$$

 Under these circumstances, the experiment is executed in the notebook.


 <h1>4. Discussion</h1>

The main goal of this notebook was to simulate noise effects E91 QKD protocol QBER and discuss how to detect the eavesdropper by means of CHSH inequality on real quantum hardware.  This requires the execution of a sequence of two-qubit quantum circuits preparing Bell states. Here the Bell state prepared is the $\Psi^-$ state, containing three single qubit gates and one CNOT (two-qubit gate). This precision is important given the architecture of current quantum hardware, which are capable of providing the exact value of single and two-qubit gates, which should then help us to accurately model noise effects. In this case we employ the **IBM-Fez** quantum hardware, which has 156 qubits, with **Heron** type quantum processor([see](https://quantum.cloud.ibm.com/computers)). To simulate noise effects, we thus computed the single-qubit and two-qubit gates errors of the entire quantum hardware and classify the qubit pairs with respect to the two-qubit gate errors from the lowest to the highest. Here we only focused on two-qubit gate errors since they are 100 times higher than the two-single qubit gate errors. Therefore, we selected 29 qubit-pairs with direct connection and arranged them from the lowest CZ error (2.43 $\times 10^{-3}$ to the highest CZ error (approximately 1). Please note that these values are readout (using the python class function provided at the beginning of this notebook) directly from the IBM quantum platform and are dynamics, so, can change within time. 

We thus simulated our E91 QKD protocol in two different settings, namely with no eavesdropper interfering with the exchanged qubits on one hand and on the second hand with an eavesdropper in the middle who is able to intercept the exchanged qubit-pairs, measure and redistribute the qubits to legitimate parties (this process is known as the intercept-and-resend attack). In both situations, we computed the CHSH inequality and the QBER. For the first case, we have seen that most of the qubit-pairs remain entangled within the coherence time ($T_1$) despite the noise effects, since the correlations fall above the classical threshold limit in absolute value, violating  CHSH inequality, except the qubit-pair with the highest two-qubit gate error. In addition, the QBERs fall below the threshold limit of $5\%$, for most of the qubit-pairs, validating the protocol. However, we can observe that some qubit-pairs show QBER between $10\%$ and $15\%$, because those qubits present higher readout errors. An interesting observation here is that the qubit-pair with the highest two-qubit gate error has a QBER above $50\%$, which shows that if such noisy qubits are used to execute the protocol then it has  more than $50\%$ chance to fail, even though no unwanted third party interfered with the communication process. This is in agreement with our expectations.

Regarding the second case where there is a third party interfering with the exchanged qubit-pairs (i.e. who is able to perform the intercept-and-resend attack), the simulation shows that the correlation fall below the classical limit even for qubit-pair with the lowest two-qubit gate error. Moreover, the QBER are found to be all greater than the threshold limit of $5\%$ ( specifically between $10\%$ and $20\%$ for most qubit-pairs). In addition, the information rate learned by the eavesdropper is also subject to similar noise effects as shown by the red and green curves, representing the information rate learned by interfering with the sender's and receiver's qubits respectively. This is in agreement with our expectation in the sense that by interfering with the exchanged qubit-pairs the eavesdropper ends-up destroying entanglement. In this case, her presence is detected by legitimate parties given that the correlation does not break the classical limit.

