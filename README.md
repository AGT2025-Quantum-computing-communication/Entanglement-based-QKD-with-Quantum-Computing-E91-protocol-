# Entanglement-based-QKD-with-Quantum-Computing-E91-protocol-
In this project, I intend to simulating entanglement based QKD (the E91 protocol) in quantum computers. I demonstrate that the secure key rate depends on the selected quantum hardware qubit-pair on which the experiment is executed, in the sense that the pair of qubit with lower two-qubit error provide lower QBER and higher key length.




<h2>Introduction</h2>
Quantum key distibution (QKD) is a direct application of quantum mechanics. It consists of generating a secret key using quantum mechanics principles, such as entanglement, no-clonning theorem, and non-locality of quantum particles. The security of QKD relies on those principles in comparison to clssical key distribution whose security relies on complexity. QKD was first introduced by Bennett and Brassard in 1984, with their famous BB84 protocol. This led to sevral other protocols later which includes, the E91 (Ekert 1991), the B92 (Bennett 1992), etc. The latest are entanglement based protocols, while the former is a prepare-and-measure protocol. 

In this project however, we focus on E91 protocol, using quantum computers. In this protocol, the legitimate users (Ange the sender and Bill the receiver) share entangled qubits (photonic qubits) prepared on the sender's side (i.e. by Ange). We assume that Ange's and  Bill's quantum labs are physically separated, but linked by a quantum channel, which can be under the control of an untrustrued third party (i.e. an eavesdroper Eve). We also assume that this quantum channel sends qubits to their respective  quantum computers that are  used to measure the  state of the qubit, and generate by the way a secret key. To assure the secrecy of the protocol, the legitimate users compute the Clauser-Horn-Shimony-Holt (CHSH) inequality based on their respective measurement results, whose value should determine whether the qubits exchanged through the quantum channel were subject to interferrence or not. The latest shows therefore a high importance to determine whether the communication beteewn legitimate user is been listerned by Eve or not, since the action of Eve on the qubits will inevitably introduce errors that modify the correlation. Thus this protocol sounds very interesting and promising for secure communication with recent developments of quantum computers, with error corrected qubits. That means one is able to prepare entangled qubits with long enough decoherence time and also determine (or measure) its exact state with a high probability.

This notebook is structured as followed: in section 1, we discuss the algorithm in detail, in section 2, we develop the stragy to test for eavesdroping (i.e. the CHSH inequality). The exerimental result is presented in section 3, where we first implement the algorithm on ideal conditions (no background noise, no eavesdroper's interference), we secondly implement the algorithm without background noise, but with an eavesdropper, and finally the main result on real conditions (i.e. real quantum hardware and eavesdroper interference).  


<h2> 1. The Algorithm</h2>
In this project, we assume the source of entangled qubits on the sender's quantum lab, connected to the receiver lab through an unauthenticated quantum channel. The algorithm to generate a secure key is as follows:

1 - Ange prepares $N$ pairs of maximally entangled qubits, keeps qubit A and sends qubit B to bill through the quantum channel, sensitive to eavesdroping and noise. 

2- Both parties randomly select a basis to measure their respective qubits ({$X$, $\frac{1}{\sqrt{2}}(X + Z)$, $Z$} for Ange and {$\frac{1}{\sqrt{2}}(X + Z)$, $Z$, $\frac{1}{\sqrt{2}}(-X + Z)$} for Bill).

3- They record their respective measurement results as a bit string ($a = {0,1}^N$, and $b = {0,1}^N$ for Ange and Bill respectively. It is worth mentioning that the measurement basis in step 2 are assumed as observables to measure in maximally entangled state, i.e. if X is the selected measurement basis on Ange side and $\frac{1}{\sqrt{2}}(X + Z)$ on Bill side, then the corresponding bit is obtained by taking $\bra \Psi X \otimes \frac{1}{\sqrt{2}}(X + Z) \ket \Psi$. Measuring this quantity in the computational basis will lead to either of the following bit strings, '00', '01', '10' or '11', depending on the state $\Psi$, where the last bit in each bit string refers to Ange measurement result and the fist to Bill's one.

4- They thus employe a clssaical channel to comminicate to each other their respective measurement basis choices. This step is known as measurement basis reconcilliation, which consists of discarting from their respective results those bits that measurment basis do not coincide. It is important to note that in an ideal situation (no noise, neither eavesdroper interference), their remaing bit strings should be identical and can be used as a key for secret communication with high privacy, but this in real situation this not the case, thus they need extra mecanisms to determine whether or not someone interfered with their exchanged qubits.

5- Based on their respective measurement results, they compute the correlation value, which if equal to $2\sqrt{2}$ in absolute value, they can be sure that no noise or no one interfered with their communication process, if not they repeat the procedure from step 1 in different set up. 

6 - Both parties sacrify half of their respective bit strings to approximate the quantum bit error rate (i.e. the ratio of the number of bits that differ with the total number of bit), since eventhough there might not be anyone interferring with their communication process there is always high probability of error due to background noise. If the value of the latest is too high, they abort the procedure if not the continue (In general if the  quantum bit error rate is above some treshold value it implies the quantum channel is too noisy).

<h2>2. Testing the eavesdroper by CHSH inequality</h2>

The CHSH inequality is employed to experimentally demonstrate Bell's theorem, which states that ''local hidden variables theories do not account for some consequences of quantum mechanics''. The violation of that inequality demonstrates that local hidden variable theorie is incompatible with quantum mechanics. In particular, local hidden variables theory shows that the quantum correlation is alwyas less than or equal to 2. The violation of  that inequality can be used to proof entanglemen, i.e. $|C| \leq 2\sqrt{2}$, for a maximally entangled state. It is therefore consistent to employ this inequality demonstrate security of E91 protocol, because its violation will certify that someone interfered with the exchanged qubit given that that the measurement by a third party collapses entanglement, and thus the crrelation reduce to classical correlation, i.e. $|C| \leq 2$, rather than  $|C| \leq 2\sqrt{2}$ as expected. 

Indeed, if ${a_0,a_1}$ is the measurment outcome of Ange ${b_0,b_1}$ that of Bill, then the CHSH inequality reads:


$$ |E(a_0,b_0) - E(a_0,b_1) + E(a_1,b_0) +E(a_1,b_1)| \leq 2\sqrt{2}.$$

 Under these circumstances, the experiment is executed in the notebook.
