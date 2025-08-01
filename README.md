## System Architecture Diagram

The following flowchart illustrates the VeltraSync signal processing pipeline, from message input to output, detailing encryption, encoding, modulation, transmission, and decoding stages:

```mermaid
graph TD
    A[Input Message<br><i>e.g., 'HELLO'</i>] -->|Text| B[AES-256 Encryption<br><i>256-bit key, CBC mode</i>]
    B -->|Encrypted bits| C[Reed-Solomon Encoding<br><i>RS(255,223), 32-byte ECC</i>]
    C -->|Encoded bits| D[FHSS Sequence Generation<br><i>Secrets module, 100 hops/s</i>]
    D -->|Frequency sequence| E[BFSK Modulation<br><i>0: 1 kHz, 1: 1.5 kHz, 0.08 s/symbol</i>]
    E -->|Waveform| F[Virtual Acoustic Channel<br><i>AWGN, Multipath, Doppler</i>]
    F -->|Received signal| G[Receiver Synchronization<br><i>3-symbol preamble, 500 Hz</i>]
    G -->|Synchronized signal| H[BFSK Demodulation<br><i>Matched filtering</i>]
    H -->|Bitstream| I[RS Decoding<br><i>Error correction up to 16 bytes</i>]
    I -->|Corrected bits| J[AES-256 Decryption<br><i>Restore original message</i>]
    J -->|Text| K[Output Message<br><i>e.g., 'HELLO'</i>]

    style A fill:#4CAF50,stroke:#2E7D32,color:#FFFFFF
    style B fill:#2196F3,stroke:#1565C0,color:#FFFFFF
    style C fill:#FF9800,stroke:#EF6C00,color:#FFFFFF
    style D fill:#9C27B0,stroke:#6A1B9A,color:#FFFFFF
    style E fill:#F44336,stroke:#B71C1C,color:#FFFFFF
    style F fill:#607D8B,stroke:#455A64,color:#FFFFFF
    style G fill:#4CAF50,stroke:#2E7D32,color:#FFFFFF
    style H fill:#F44336,stroke:#B71C1C,color:#FFFFFF
    style I fill:#FF9800,stroke:#EF6C00,color:#FFFFFF
    style J fill:#2196F3,stroke:#1565C0,color:#FFFFFF
    style K fill:#4CAF50,stroke:#2E7D32,color:#FFFFFF
