### **1️⃣ Air Intake & Compression**

* The turbine **pulls in outside air**.

  * **AT (Ambient Temperature)** and **AH (Humidity)** describe the air entering the system.
  * **AP (Ambient Pressure)** is the air’s pressure at the inlet.
  * **AFDP (Air Filter Differential Pressure)** measures how much resistance or clogging the filters cause.

    * If AFDP is **high**, filters are dirty → less air enters → incomplete combustion → higher **NOx**.

---

### **2️⃣ Compression Stage**

* The air is compressed by the compressor, increasing pressure and temperature.

  * **CDP (Compressor Discharge Pressure)** is the output of this stage — it tells how well the compressor is performing.

---

### **3️⃣ Combustion Chamber**

* Fuel mixes with hot compressed air and burns.

  * **TIT (Turbine Inlet Temperature)** = flame temperature entering the turbine.

    * Higher TIT usually increases NOx because combustion gets hotter.
  * The model helps detect whether TIT can be slightly adjusted without violating safety or CO limits.

---

### **4️⃣ Expansion & Power Output**

* Hot gases expand through turbine blades → this **spins the shaft**, generating power.

  * **TEY (Turbine Energy Yield)** or power output measures this stage.
  * Higher TEY = higher load = more heat and pressure → **NOx rises** if airflow or cooling isn’t balanced.
  * That’s why **AFDP×TEY** is a key interaction term in your model.

---

### **5️⃣ Exhaust & Emissions**

* Gases leave through the exhaust:

  * **TAT (Turbine After Temperature)** = how hot the exhaust gases are.

    * If TAT drops too much at low load, it can indicate poor mixing or incomplete combustion.
  * **GTEP (Gas Turbine Exhaust Pressure)** helps confirm whether exhaust flow is restricted.
  * **NOX (Nitrogen Oxide emissions)** = your main target.

    * It increases when combustion is too hot, air filters are dirty, or the air is too cold/dry.

---

### **🔁 Project Connection**

Your ML models used these variables to:

* Identify **which parameters or combinations (interactions)** lead to higher NOx.
* Suggest operational tuning (like reducing AFDP or adjusting TIT slightly) to **minimize NOx** without losing power.

---

✅ **Simplified Flow Summary**

| Turbine Stage | Key Sensors  | Influence on NOx                           |
| ------------- | ------------ | ------------------------------------------ |
| Air Intake    | AT, AH, AFDP | Cold or dry air ↑ NOx; dirty filters ↑ NOx |
| Compression   | CDP          | Supports air–fuel mixing, affects TIT      |
| Combustion    | TIT, TEY     | High TIT/TEY ↑ NOx                         |
| Exhaust       | TAT, GTEP    | Indicates efficiency and heat loss         |
| Output        | NOX          | Final measure of combustion quality        |

---

**In short:**
The turbine works like a controlled fire fed by air, pressure, and fuel.
Your model learned that **ambient air (AT, AH)** and **airflow (AFDP)** are the biggest levers for reducing NOx — especially when combined with **load (TEY)** and **exhaust heat (TAT)**.