# Database architecture

#### Centralised Database

Data located in a single location. Administration is easy.  
It is the most used approach nowadays. Optimisation is effective. Not suitable for applications that requires distributed data. Cloud Computing / Data farms are examples of that.

#### Client-server

Processing shared between client and server processes. Both client and server may be at different locations. Server provides all DB functionally. Client provides users interface for I/O. System administration is relatively simple. System recovery similar to centralised DB.

#### Distributed Database

Data located in multiple nodes. Those nodes communicate each other through network. Users do not need to know nodes location. Administration is difficult as well as guarantee consistency of the information. Hard to process queries efficiently. Transaction processing can be very inefficient.

#### World Wide Web

Data located anywhere. Multiple sources spread around the world. Difficult to identify correct resource information.

#### Grid Database

Similar to distributed DB. It is an specialezd inplementation of distributed DB for a specific purpose. e.g. Science. The administration

#### P2P Databases

#### Cloud Computing

# Basic Hardware

## Memories

**Moore's law** Capacity of chips doubles each 18 months since 1970.

$$ = 2 ^ {\frac{(year - 1970) \times 2}{ 3}}$$

**Joy's Law** Chip speed doubles every two years since 1984.

$$ = 2 ^ {\frac{year - 1984}{2}}$$

# Hardware

$$ \text{Hit ratio} = \frac{\text{references satisfied by cache}}{\text{total references}}$$

**Effective Memory Access Time \(EA\)**

\begin{equation}  
\text{EA} = H \times C + \(1 - H\) \times S  
\end{equation}

Where _H =_ Hit ratio - _C =_ Cache access time - _S =_ Memory Access Time

Example:

With  $$ S = 100C $$ and $$ H = 50\% = 1/2$$

Then $$ C = S / 100 $$

Replacing in formula:

$$ EA = \frac{1}{2} \times \frac{S}{100} + (1- \frac{1}{2}) \times 100C $$

$$ = \frac{S}{200} + \frac{100C}{2} $$

replacing S

$$ = \frac{100C}{200} + 50C  = \frac{C}{2} + 50C $$

$$ = \frac{C + 100C}{2} = \frac{101C}{2} = 50.5C $$

**Effective disk buffer cache access time**  
$$ \text{Disk Access Time} = \text{seek time} + \text{rotational time} + \frac{transfer length}{bandwidth} $$

\begin{equation}  
\text{EA} = H \times C + \(1 - H\) \times S  
\end{equation}

Where _H =_ Hit ratio - _C =_ Buffer access time - _S =_ Disk Access Time

### SSD \(Solid-State Drive / Solid-State Disk\)

No moving parts. Memory based on silicon. No notion of sek and rotational latency. No start-up times. Relative expensive.

#### Reliability

* Un-correctable Block Error Rate \(UBER\). 1 sector by each $$ 10^{16}$$ bit read.
* Mean Timen Between Failure \(MTBF\). 136 years
* Shock \(Operating or no-operating\) 1000 G / 0.5 msec

### Storage Systems

Storage system can determine reliability of DB systems.

#### Redundant Array of Independent Disk \(RAID\)

**RAID 0**  
Blocks of data \(A\) are stored in different disks \(4k - 8k\). Provides balanced I/O. If one disk fail it would be catastrophic and MTTF reduces by factor of 2.

| Disk 0 | Disk1 |
| --- | --- |
| A0 | A1 |
| A2 | A3 |

**RAID 1 \(Mirroring\)**  
Blocks \(A of 4k-8k\) are contiguos. High Read Throughput and low write throughput. MTTF grows to $$ MTTF^2 $$

| Disk1 | Disk2 |
| --- | --- |
| A0 | A0 |
| A1 | A1 |

**RAID 2**  
Bits \(b\) level stripping. Each bit is stored in disk. MTTF is half in RAID 0.

| Disk1 | Disk2 |
| --- | --- |
| b0 | b1 |
| b2 | b3 |

**RAID 3**  
Byte \(B\) level stripping. Containg an additional Disk for parity bit storage. MTTF increase substantially.$$ 1/3 \times MTTF_{RAID1} = \frac{MTTF^2}{3}$$  
$$ P_i = B_i + B_{i+1} \text{Where} + \text{is an exclusive operator} $$

| Disk1 | Disk2 | Disk3 |
| --- | --- | --- |
| B0 | B1 | P0 |
| B2 | B3 | P1 |

**RAID 4**  
Block \(A\) level stripping. Dedicated Disk to store parity. MTTF same as RAID3.  
$$ P_i = A_i + A_{i+1} \text{Where} + \text{is an exclusive operator} $$

| Disk1 | Disk2 | Disk3 |
| --- | --- | --- |
| A0 | A1 | P0 |
| A2 | A3 | P1 |

**RAID 5**  
Block Level. No dedicated disk for parity. Parity blocks are also stripped. MTTF increases substantially as RAID3.

| Disk1 | Disk2 | Disk3 |
| --- | --- | --- |
| A0 | A1 | P0 |
| A2 | P1 | A3 |
| P2 | A4 | A5 |

$$ \text{Create:} P0 = A0 \oplus A1 $$  
$$ \text{Update:} P0_{new} = A0_{old} \oplus A0_{new} \oplus P0_{old} $$  
$$ \text{Reconstruct:} A0 = A1 \oplus P0 $$

**RAID 6**  
Block level. Two parity blocks used. Reliability is of the order of $$ MTTF^3 / 10 $$

| Disk1 | Disk2 | Disk3 | Disk4 |
| --- | --- | --- | --- |
| A0 | A1 | P0 | P1 |
| A2 | P2 | P3 | A3 |
| P4 | P5 | A4 | A5 |

#### Storage Area Networks

Can be organised as RAID systems.Storage is partitioned and allocated to each system and can also shared.

### Communication Hardware

$$ \text{Trasmit time} = \frac{distance}{Cm} + \frac{\text{message bits}}{bandwidth} $$

Where Cm = Speed of light \(200 million meters/second\)

# Transaction Processing

transaction is a collection of operations that need to be performed on the physical and abstract application state.

Transaction processong contributd many concepts of distributed computing and fault tolerant computing.

### ACID Properties

**Atomicity** Every operation happend or none of them happend.

**Consistency** Transaction is a correct transformation of th state. It does not violate the integrity of application state.

**Isolation** Despite many transactions run simustaneously it appears that one transaction comes after another.

**Durability** State changes committed by a transaction survive failures.

