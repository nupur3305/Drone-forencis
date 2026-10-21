

# Robust Forensic Analysis of Unmanned Aerial Vehicles Using Federated Learning and Deobfuscation Techniques

**Project Domain:** Digital Forensics | Cybersecurity | Machine Learning | Federated Learning | UAV Systems

## Overview

Unmanned Aerial Vehicles (UAVs), commonly known as drones, generate valuable digital evidence through flight logs, GPS coordinates, telemetry, timestamps, sensor readings, and associated software.

Analyzing this evidence can be challenging when flight records are incomplete or corrupted, software is obfuscated, or forensic data is distributed across multiple sources.

This project explores a framework for improving UAV digital forensic investigations by combining **software deobfuscation, federated learning, and probabilistic flight-trajectory reconstruction**.

The objective is to support more reliable forensic analysis while considering data privacy, evidence integrity, and the limitations of reconstructed information.

## Problem Statement

Drone forensic investigations face several challenges:

* **Incomplete flight records:** Missing or corrupted GPS coordinates and telemetry can make flight reconstruction difficult.
* **Obfuscated software:** Code obfuscation can complicate the analysis of drone-related applications and software behavior.
* **Sensitive forensic data:** Flight records may contain location information and other sensitive operational details.
* **Distributed evidence:** Relevant information may be held by different devices, systems, or organizations.
* **Evidence integrity:** Investigators must preserve original evidence and document all processing activities.

A robust forensic workflow should address these challenges while distinguishing verified evidence from analytical inferences and estimated data.

## Project Objectives

1. Explore SMT-based program analysis techniques to reason about obfuscated program behavior.
2. Investigate federated learning as an approach to collaborative forensic analysis without routinely centralizing raw datasets.
3. Explore probabilistic methods for reconstructing missing or corrupted UAV flight trajectories.
4. Identify techniques for validating analytical results and measuring reconstruction errors.
5. Emphasize evidence integrity, reproducibility, privacy considerations, and transparent reporting.

## Proposed Methodology

The project can be organized into the following stages.

### 1. UAV Evidence Acquisition

Identify relevant forensic data sources, including:

* Flight logs
* GPS latitude and longitude
* Timestamps
* Altitude and velocity
* Sensor telemetry
* Relevant application metadata

Evidence should be obtained only from authorized sources, with its origin and acquisition process documented.

### 2. Evidence Preservation and Preprocessing

Prepare the data for analysis while preserving the original evidence.

Potential activities include:

* Calculating SHA-256 hashes to help verify file integrity.
* Parsing timestamps and telemetry records.
* Identifying missing values, duplicate records, and inconsistent timestamps.
* Checking GPS coordinates for invalid or implausible values.
* Maintaining a record of data transformations.

**Forensic principle:** Original evidence should remain unchanged. Analysis should be performed on verified working copies.

### 3. Software Deobfuscation and Program Analysis

Code obfuscation transforms a program to make its logic more difficult to understand while generally preserving its intended behavior.

The proposed analysis considers:

* Static analysis to examine code without executing it.
* Dynamic analysis to observe program behavior during execution.
* Symbolic execution to explore program paths using symbolic inputs.
* Satisfiability Modulo Theories (SMT) solvers to determine whether path constraints are satisfiable.

For example, a path containing the conditions `x > 10` and `x < 20` is satisfiable because values such as `x = 15` meet both conditions.

SMT-based analysis can help reason about program behavior, but it does not guarantee recovery of the original source code.

### 4. Federated Learning for Forensic Analysis

Federated learning enables multiple clients to train a shared machine-learning model using their local datasets.

A typical workflow involves:

1. Distributing an initial model to participating clients.
2. Training the model locally.
3. Sending model updates to an aggregation server.
4. Aggregating updates to create a new global model.
5. Repeating the process until the evaluation criteria are met.

One commonly used aggregation algorithm is Federated Averaging (FedAvg).
$

This approach can reduce the need to transfer raw forensic datasets to a central server. However, model updates can still reveal information, and federated learning alone does not guarantee privacy.

### 5. Probabilistic Flight-Trajectory Reconstruction

Missing GPS observations can make it difficult to understand a drone's flight path.

The proposed approach considers using available coordinates, timestamps, and movement patterns to estimate missing trajectory points.

Potential techniques include:

* Linear interpolation as a baseline.
* Bayesian inference for probabilistic estimation.
* Kalman filtering for state estimation under measurement noise.



### 6. Validation and Reporting

The final stage focuses on assessing the reliability of the analysis.

Potential evaluation methods include:

* Comparing SHA-256 hashes before and after processing.
* Testing program behavior on controlled inputs.
* Evaluating machine-learning performance using appropriate metrics.
* Measuring GPS reconstruction errors against known reference coordinates.
* Documenting assumptions, processing steps, and limitations.

For trajectory reconstruction, known GPS points can be temporarily hidden, estimated, and compared against the original values.

## System Architecture

The following describes a conceptual architecture rather than a claim that every component has been implemented.

```text
           UAV FORENSIC DATA SOURCES
                      |
                      v
          Evidence Acquisition
                      |
                      v
       Integrity Verification and Parsing
                      |
                      v
           Data Preprocessing
                      |
          +-----------+-----------+
          |           |           |
          v           v           v
     Program       Federated    Trajectory
     Analysis      Learning     Reconstruction
          |           |           |
          v           v           v
     SMT-Based    Aggregated    Estimated
     Reasoning    Model        Flight Path
          |           |           |
          +-----------+-----------+
                      |
                      v
            Validation and Evaluation
                      |
                      v
              Forensic Reporting
```

## Technologies and Tools

The following tools are potential candidates for implementing and evaluating the proposed workflow. They should not be interpreted as a list of technologies already used in the project.

| Technology            | Potential application                                              |
| --------------------- | ------------------------------------------------------------------ |
| Python                | Data processing and forensic analysis                              |
| Pandas                | Tabular flight-log analysis                                        |
| NumPy                 | Numerical computations                                             |
| Matplotlib            | Visualizing trajectories and analytical results                    |
| Z3 SMT Solver         | Constraint solving and program analysis                            |
| Scikit-learn          | Baseline machine-learning experiments                              |
| PyTorch or TensorFlow | Machine-learning and federated-learning experiments                |
| DATCON                | Decoding supported DJI DAT flight logs                             |
| KML                   | Visualizing geographic trajectories in compatible mapping software |
| Git and GitHub        | Version control and project documentation                          |

Actual tool selection depends on the available evidence, file formats, and implementation requirements.

## Evaluation Metrics

| Component                 | Possible evaluation criteria                                                             |
| ------------------------- | ---------------------------------------------------------------------------------------- |
| Evidence integrity        | SHA-256 hash comparison                                                                  |
| Program analysis          | Constraint-solving results and controlled behavior comparisons                           |
| Anomaly detection         | Precision, recall, F1-score, and false-positive rate                                     |
| Federated learning        | Model performance, communication overhead, and comparison with baselines                 |
| Trajectory reconstruction | Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), and geographic position error |
| Reproducibility           | Documentation of datasets, parameters, tools, and processing steps                       |

The appropriate metrics depend on the specific algorithms and experiments implemented.

## Security, Privacy, and Forensic Considerations

The project emphasizes the following principles:

* **Evidence integrity:** Preserve original files and document processing activities.
* **Chain of custody:** Maintain records of evidence acquisition, access, transfer, and analysis.
* **Privacy:** Limit unnecessary exposure of sensitive flight and location data.
* **Secure experimentation:** Analyze untrusted software only in an appropriately isolated environment.
* **Transparency:** Clearly distinguish measured values from estimates.
* **Reproducibility:** Record tools, configurations, assumptions, and evaluation procedures.
* **Responsible interpretation:** Treat analytical results as evidence requiring validation, not automatic proof of an event.

## Limitations

The proposed approach has several important limitations:

1. Missing flight data cannot always be reconstructed accurately.
2. SMT-based analysis can face path explosion when programs contain many branches.
3. Deobfuscation may reveal program logic without recovering the original source code.
4. Federated learning can remain vulnerable to privacy leakage and malicious model updates.
5. Different drone models may use different telemetry formats and data structures.
6. Conclusions depend on the quality, completeness, and authenticity of the available evidence.

These limitations should be evaluated against the actual implementation and experimental results.

## Expected Outcomes

The intended outcomes of this research direction are:

* A structured approach to UAV digital forensic analysis.
* Investigation of SMT-based techniques for reasoning about obfuscated program behavior.
* Assessment of federated learning for privacy-aware collaborative analysis.
* Evaluation of methods for estimating missing flight-trajectory observations.
* A documented approach to evidence integrity, validation, and reproducibility.

These are research objectives, not claims of experimentally demonstrated results.

## Repository Structure

A possible structure for a future implementation is:


## Research Reference


## Author and Contributions

**Project:** Robust Forensic Analysis of Unmanned Aerial Vehicles Using Federated Learning and Deobfuscation Techniques

**Author:** Nupur Sharma

**Institution:** Government Women Engineering College Ajmer

**Project type:** Internship

**Contribution:**

Contributed to the UAV Digital Forensics research project by working on data preprocessing and preparation for forensic analysis. My contributions included:

* **Data Preprocessing:** Assisted in cleaning, organizing, and preparing data for further analysis.
* **Data Quality:** Worked on identifying missing values, inconsistencies, and data-quality issues during preprocessing.
* **Data Preparation:** Supported the preparation of structured data for subsequent forensic analysis.
* **Research and Documentation:** Contributed to understanding the project methodology and documenting the relevant analysis workflow.

This was a collaborative research project, and my contribution focused on preprocessing and data preparation rather than independently developing the entire system.


---

*This repository documents the research direction and methodology for UAV digital forensics. Implementation claims, experimental results, and performance metrics should be added only after they have been independently verified.*
