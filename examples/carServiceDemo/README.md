# 🛠️ API Simulation: Learning Mode Example for Car Service

This simulation demonstrates how to use **Learning Mode** in Tricentis API Simulation to automatically build request/response pairs for a SOAP-based Car Service API. This configuration can be used to rapidly prototype virtual services by forwarding traffic to a live backend and capturing the details needed to simulate it later.

## 🔍 Overview

This simulation defines a single learning-forwarding connection for a car service. Requests sent to the simulator on port `54345` are forwarded to a real backend (hosted locally on port `51002`) while the simulator listens and learns the request/response patterns.

## 🧩 Simulation Details

- **Schema**: `SimV1`  
- **Simulation Mode**: Learning
- **Listening Port**: `54345`
- **Learning Targets**:
  - URL path `/carservice`
  - XPath match for the `make` element in the `getCar` SOAP call

## 📡 Connections

### 1. `theCarServiceSimLearnings` (Simulator)
- **Type**: Forwarding with Learning Mode
- **Mode**: `Learning`
- **Patterns**:
  - Matches requests to `/carservice`
  - Captures SOAP request values using XPath:
    ```
    /*[local-name()='Envelope']
      /*[local-name()='Body']
        /*[local-name()='getCar']
          /*[local-name()='make']
    ```
- **Destination**: `realCarService`
- **Port**: `54345` (listens for incoming requests)

### 2. `realCarService` (Backend)
- **Endpoint**: `http://localhost:51002/carservice`
- **Listen**: false (simulator does not host this endpoint, it forwards to it)

## 📁 Usage

1. **Start the simulator** with this YAML configuration.
2. **Send real requests** to `http://localhost:54345/carservice`.
3. **Inspect the learned data** in the simulation output directory.
4. **Convert the learned simulation** into a static one for use in replay mode.

## ✅ Tips

- Learning Mode is ideal for building up simulations from scratch when you have access to a live backend.
- Use XPath to target critical elements in SOAP or XML payloads for smarter correlation.
- Once learning is complete, switch to Replay Mode to simulate without hitting the backend.

---

🔗 For more examples and documentation, visit the [Tricentis Simulation GitHub](https://github.com/Tricentis/Simulation).