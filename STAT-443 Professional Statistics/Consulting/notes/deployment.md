````markdown
# 🌀 Turbine NOx Advisor — Model Deployment Guide

## 1. Overview
This project predicts **NOx emissions** from gas turbines under varying temperature, pressure, and load conditions.  
The trained model (`nox_xgb_v1.joblib`) is deployed as an **API backend** connected to your **Lovable web interface**.

---

## 2. Files & Artifacts
| File | Description |
|------|--------------|
| `artifacts/nox_xgb_v1.joblib` | Trained XGBoost model |
| `artifacts/model_info.json` | Feature list and metadata |
| `api.py` | FastAPI server providing `/predict` endpoint |
| `requirements.txt` | Dependencies (`fastapi`, `uvicorn`, `pandas`, `xgboost`, `joblib`) |

---

## 3. Run the Backend

1. Activate your environment and install dependencies:
   ```bash
   pip install -r requirements.txt
````

2. Start the FastAPI server:

   ```bash
   uvicorn api:app --host 0.0.0.0 --port 8000
   ```

3. Test locally by opening:

   ```
   http://localhost:8000/docs
   ```

   You’ll see an interactive Swagger UI — enter turbine values and verify the predicted NOx output.

---

## 4. Connect to Lovable Website (Frontend)

Your **Lovable project (Turbine NOx Advisor)** already has a UI layout with fields for:

* Ambient conditions (`AT`, `AP`, `AH`)
* Operational parameters (`TIT`, `TAT`, `CDP`, `GTEP`, `AFDP`, `TEY`)
* Buttons for ±5 % and ±10 % parameter adjustments
* A “Predict and Optimize” button to trigger API call

---

## 5. Add Backend Endpoint to Frontend

In your Lovable editor:

1. Open the **prediction action** or **form submission logic**.

2. Configure it to send a POST request to your FastAPI endpoint:

   ```
   https://your-server-domain.com/predict
   ```

   (If running locally, use `http://127.0.0.1:8000/predict`.)

3. JSON payload structure must match exactly:

   ```json
   {
     "TIT": 1075.0,
     "TAT": 540.0,
     "CDP": 12.5,
     "GTEP": 27.5,
     "AFDP": 3.1,
     "AT": 18.0,
     "AP": 1012.0,
     "AH": 55.0,
     "TEY": 150.0
   }
   ```

4. Parse the API response:

   ```json
   {"NOX_pred": 63.4}
   ```

   and display the `NOX_pred` value in your results panel.

---

## 6. Deploying the Complete Stack

### Option A — Local Testing

* Run `uvicorn` locally and connect your Lovable preview URL to `http://localhost:8000/predict` via CORS.
  Add the following snippet to your `api.py` **before defining routes**:

  ```python
  from fastapi.middleware.cors import CORSMiddleware
  app.add_middleware(
      CORSMiddleware,
      allow_origins=["*"],
      allow_methods=["*"],
      allow_headers=["*"],
  )
  ```

### Option B — Production

* Deploy the backend to:

  * **Render**, **Railway**, **Azure App Service**, or **AWS EC2**
* Replace the endpoint in Lovable with the deployed API URL.

---

## 7. Workflow Summary

| Step                           | What happens                              |
| ------------------------------ | ----------------------------------------- |
| User enters turbine parameters | Inputs collected via Lovable UI           |
| Clicks “Predict”               | Sends JSON POST request to `/predict`     |
| FastAPI receives request       | Converts JSON → Pandas DataFrame          |
| Model computes prediction      | Returns NOx estimate                      |
| UI displays NOx value          | Updates result + triggers recommendations |

---

## 8. Example End-to-End Test

**Input:**

```json
{"TIT":1075,"TAT":540,"CDP":12.5,"GTEP":27.5,"AFDP":3.1,"AT":18,"AP":1012,"AH":55,"TEY":150}
```

**Response:**

```json
{"NOX_pred": 63.41}
```

Displayed instantly on your Lovable “Turbine NOx Advisor” interface.

---

## 9. Optional Enhancements

* Add `/explain` endpoint returning SHAP or permutation-based feature contributions.
* Save prediction logs (`timestamp`, inputs, predicted NOx) for analytics.
* Integrate email/PDF export for “Recommendation Report”.

---

✅ **Once this setup is done**, your Lovable frontend will be fully connected to the FastAPI backend.
Operators can input turbine parameters → hit **Predict** → instantly see **NOx predictions** and actionable recommendations.
