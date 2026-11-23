Ultrasound Pulse-Echo Signal Simulator
Overview
This project presents a Python-based computational simulator designed to investigate the fundamental principles of ultrasound pulse-echo systems through one-dimensional acoustic propagation and digital signal processing.
The simulator generates Gaussian-modulated ultrasound pulses, models echoes from point reflectors, extracts signal envelopes using the Hilbert transform, and estimates reflector depths through automated peak detection.
Numerical experiments evaluate spectral characteristics, sampling frequency sensitivity, acoustic attenuation, axial resolution, and robustness against additive white Gaussian noise.
The project bridges biomedical instrumentation, acoustic physics, numerical modelling, and quantitative engineering verification within a reproducible computational environment.

Intended Scope
This simulator is intended for educational purposes, preliminary engineering analysis, and biomedical signal processing research. It does not constitute a clinically validated ultrasound system or a diagnostic medical device.

Engineering Problem
Ultrasound systems estimate the location of internal structures by analysing reflected acoustic waves. However, the reliability of these estimations depends on multiple factors, including signal bandwidth, sampling frequency, acoustic attenuation, noise, and signal processing algorithms.
Understanding the individual contribution of these parameters is essential when designing and evaluating biomedical acquisition systems.
This project addresses the need for a controlled computational environment in which these variables can be independently investigated, allowing the performance and limitations of a simplified pulse-echo system to be quantified without requiring specialised ultrasound hardware.

Objectives
General Objective
To develop and numerically verify a reproducible ultrasound pulse-echo simulator capable of generating, processing, and analysing acoustic signals under controlled simulation conditions.

Specific Objectives
Generate Gaussian-modulated ultrasound pulses with configurable spectral characteristics.
Simulate acoustic reflections from point reflectors at predefined depths.
Implement envelope extraction, automated peak detection, and time-of-flight-based depth estimation.
Quantify the influence of sampling frequency on depth estimation error.
Evaluate noise sensitivity and repeatability through Monte Carlo simulations.
Model simplified acoustic attenuation and investigate time gain compensation (TGC).
Assess axial resolution using closely spaced reflectors.
Document numerical results, verification criteria, and model limitations through reproducible code and interpretable figures.

System Architecture
The ultrasound pulse-echo simulator follows a modular functional architecture that separates physical modelling, signal generation, signal conditioning, digital signal processing, and numerical verification.

flowchart TD
    A[Simulation Parameters] --> B[Pulse Generation]
    B --> C[Acoustic Propagation]
    C --> D[Received RF Signal]
    D --> E[Hilbert Envelope]
    E --> F[Peak Detection]
    F --> G[Depth Estimation]
    G --> H[Numerical Verification]

The simulator's internal structure is categorized into six functional subsystems, mapping parameters directly to quantitative outputs:
Subsystem	Input	Output	Primary Function
1. Configuration	Physical parameters	Numerical configuration	Experimental control
2. Signal Generation	Frequency and bandwidth	RF pulse	Excitation modelling
3. Acoustic Modelling	Pulse and reflectors	Simulated echoes	Propagation and reflection
4. Signal Conditioning	Echoes, noise, and gain	Processed RF	Attenuation, noise, and TGC
5. Digital Signal Processing	RF signal	Estimated depths	Hilbert and detection
6. Verification	Estimations and references	Metrics and figures	Quantitative evaluation
The system begins with a configurable set of physical and computational parameters, including central frequency, fractional bandwidth, sampling frequency, acoustic velocity, reflector depths, attenuation coefficient, and noise intensity.
Gaussian-modulated RF pulses are generated and subsequently delayed to represent acoustic reflections from point reflectors. Depending on the experimental configuration, acoustic attenuation and additive white Gaussian noise may be incorporated into the received signal.
An optional time gain compensation (TGC) stage is included to investigate depth-dependent amplitude recovery. Its performance remains subject to further verification.
The received RF signal is processed through analytical envelope extraction using the Hilbert transform, followed by automated peak detection and time-of-flight-based depth estimation.
Finally, estimated reflector locations are compared against predefined reference values to calculate quantitative performance metrics and generate scientific visualisations.
This architecture supports controlled numerical experiments and provides a functional framework for future extensions and algorithmic improvements.

Physical Model
Acoustic Propagation Assumptions
The simulator represents ultrasound propagation through a simplified, one-dimensional, homogeneous acoustic medium with a constant sound velocity of 1540 m/s.
Acoustic interfaces are represented by discrete point reflectors with configurable effective reflection coefficients. The model assumes linear signal superposition and does not account for diffraction, refraction, acoustic scattering, nonlinear propagation, or multiple reflections.

Ultrasound Pulse Generation
The transmitted signal is represented by a Gaussian-modulated sinusoidal RF pulse with a nominal central frequency of 5 MHz and a fractional bandwidth of 60% at the −6 dB amplitude threshold.
The Gaussian envelope controls pulse duration and spectral bandwidth, enabling controlled investigation of temporal and frequency-domain characteristics.
The reference sampling frequency is 100 MHz, corresponding to a sampling interval of 10 ns and 20 samples per carrier cycle.

Time-of-Flight and Acoustic Reflection
The round-trip propagation delay is:

$$
\tau = \frac{2z}{c}
$$

Where:
- $\tau$: Round-trip propagation time (s).
- $z$: Reflector depth (m).
- $c$: Acoustic velocity (m/s).

Reflector depth is estimated as:

$$
z = \frac{c\tau}{2}
$$.

Acoustic Attenuation
A simplified depth-dependent acoustic attenuation model is implemented using an attenuation coefficient of 0.5 dB/(cm·MHz).
Amplitude losses are calculated at the nominal central frequency and account for the round-trip propagation distance.
This narrowband approximation provides a controlled representation of depth-dependent amplitude reduction but does not reproduce frequency-dependent spectral distortion across the complete pulse bandwidth.

Received RF Signal
The received radiofrequency signal is constructed through the linear superposition of delayed, amplitude-scaled, and optionally attenuated ultrasound echoes.
Additive noise may subsequently be incorporated to investigate the sensitivity of signal processing algorithms to random perturbations.
The model represents a computational approximation of pulse-echo acquisition rather than a complete physical representation of an ultrasound transducer.

Noise Model
Additive white Gaussian noise (AWGN) is used to evaluate the robustness and repeatability of depth estimation.
Noise variance is determined from the nominal signal-to-noise ratio (SNR) and the global mean-square power of the simulated RF signal.
Random number generation is controlled through predefined seeds to support reproducibility.
The nominal global SNR should not be interpreted as the local SNR of individual acoustic echoes.
Time Gain Compensation
An optional time gain compensation (TGC) stage applies depth-dependent amplification to investigate amplitude recovery following acoustic attenuation.
Although the theoretical compensation function represents the inverse of the implemented attenuation factor, practical amplitude recovery is affected by noise amplification and signal estimation procedures.
Further numerical verification is required to establish the performance of the implemented compensation stage.
Reference Simulation Parameters
Parameter	Reference value
Central frequency	5 MHz
Nominal fractional bandwidth	60%
Measured fractional bandwidth	59.90%
Sampling frequency	100 MHz
Acoustic velocity	1540 m/s
Gaussian temporal parameter	0.1249 µs
Acquisition duration	90 µs
Reflector depths	15, 35, and 55 mm
Reference reflection coefficients	1.0
Acoustic attenuation coefficient	0.5 dB/(cm·MHz)
Noise distribution	Additive white Gaussian
Monte Carlo random seed	2026
The reference configuration is modified according to the objectives of each numerical experiment.
Model Scope
The physical model is intended for educational purposes, numerical verification, and preliminary biomedical signal processing investigations.
