# AI-based-Security-Anomaly-Detection-for-Infotainment-ECU
Lightweight AI-based Security Anomaly Detection for Infotainment ECU (Edge Deployment)

# Lightweight AI-based Security Anomaly Detection for Infotainment ECU (Edge Deployment)

## Overview

This project implements a lightweight, edge-deployable AI-based anomaly detection system for an automotive infotainment ECU. The goal is to detect abnormal system and communication behavior (potential cyber attacks or system misuse) under constrained embedded conditions.

Unlike cloud-based analytics, this solution focuses on "on-device inference", simulating real-world ECU constraints such as limited memory, processing power, and real-time execution.

---

## Problem Statement

Modern infotainment ECUs are exposed to multiple attack surfaces:

* CAN/Ethernet communication
* Bluetooth/Wi-Fi interfaces
* Third-party applications
* User interaction patterns

Traditional rule-based detection is limited. This project explores:

> Can we detect abnormal behavior using a lightweight ML model running directly on the ECU?

---

## Objectives

* Simulate infotainment ECU behavior (normal + attack scenarios)
* Design a structured dataset with system and network features
* Train a lightweight ML model for anomaly detection
* Optimize the model for embedded deployment (TinyML approach)
* Integrate inference into a C-based ECU simulation
* Evaluate performance under constrained conditions

---

## System Architecture

```
Simulated Signals → Feature Extraction → ML Model → Decision Engine → Alert/DTC
```

### Components:

* **Data Simulator**: Generates normal and attack scenarios
* **Feature Extractor**: Converts raw signals into model inputs
* **ML Model**: Lightweight neural network (TinyML)
* **ECU Simulation**: C-based runtime loop
* **Diagnostics Module**: Raises anomaly alerts

---

## Feature Set

Each data sample includes:

| Feature            | Description                      |
| ------------------ | -------------------------------- |
| can_msg_rate       | CAN messages per second          |
| avg_can_interval   | Average inter-message delay (ms) |
| cpu_usage          | CPU utilization (%)              |
| memory_usage       | Memory usage (%)                 |
| net_tx_rate        | Network transmit rate (KB/s)     |
| net_rx_rate        | Network receive rate (KB/s)      |
| app_switch_rate    | App switching frequency          |
| bluetooth_activity | Bluetooth activity level         |
| label              | 0 = Normal, 1 = Anomaly          |

---

## Attack Scenarios Simulated

### 1. CAN Flooding

* Sudden spike in message rate
* Reduced inter-message interval

### 2. Resource Exhaustion

* CPU usage near 100%
* Memory saturation

### 3. Network Abuse

* Abnormal TX/RX spikes

### 4. Behavioral Anomalies

* Irregular and inconsistent system activity

---

## Model Design

### Architecture

```
Input (8 features)
→ Dense (16)
→ Dense (8)
→ Output (1, Sigmoid)
```

### Training

* Framework: TensorFlow
* Loss: Binary Crossentropy
* Metrics:

  * Accuracy
  * Precision / Recall (critical for security)

---

## Model Optimization

To enable embedded deployment:

* Converted to TensorFlow Lite format
* Applied INT8 quantization

### Target Constraints:

* Model size < 50 KB
* Low RAM footprint
* Fast inference (<10 ms target)

---

## ECU Simulation (C Implementation)

### Structure

```
/ecu_simulation
  ├── main.c
  ├── scheduler.c
  ├── data_simulator.c
  ├── feature_extractor.c
  ├── ai_model.c
  ├── diagnostics.c
```

### Execution Loop

```c
while(1) {
    read_signals();
    extract_features();
    run_model();
    evaluate_result();
    log_event();
    sleep(100); // 100 ms cycle
}
```

---

## Output Behavior

* Normal operation → No action
* Anomaly detected → Raise alert

Example:

```c
if (anomaly_score > threshold) {
    raise_alert("INFOTAINMENT_ANOMALY");
}
```

---

## Performance Metrics

| Metric             | Target         |
| ------------------ | -------------- |
| Model Size         | < 50 KB        |
| RAM Usage          | < 20 KB        |
| Inference Time     | < 10 ms        |
| Detection Accuracy | > 90% (target) |

---

## Key Engineering Considerations

### Trade-offs

* Detection accuracy vs latency
* Model size vs performance
* False positives vs detection sensitivity

### Constraints (Assumed)

* ECU RAM: 128 KB
* Real-time cycle: 100 ms
* No hardware acceleration

---

## Limitations

* Uses simulated data (no OEM access)
* Simplified ECU environment
* No real CAN/Ethernet stack integration

---

## Future Improvements

* Integrate rule-based + ML hybrid detection
* Add temporal modeling (sequence-based detection)
* Secure model storage (integrity checks)
* OTA model update simulation
* Extend to Gateway ECU integration

---

## Repository Structure

```
/data
/scripts (Python - data generation & training)
/model (trained + optimized model)
/ecu_simulation (C implementation)
/results (metrics, logs)
/docs (architecture, diagrams)
```

---

## Why This Project Matters

This project demonstrates:

* Embedded AI deployment (TinyML)
* Automotive cybersecurity thinking
* System-level design under constraints
* Integration mindset (not just ML experimentation)

---

## Author

Mihir Kumar Jena

Automotive Software & Cybersecurity Engineer
Specialization: AUTOSAR, ECU Development, Secure Systems

---

